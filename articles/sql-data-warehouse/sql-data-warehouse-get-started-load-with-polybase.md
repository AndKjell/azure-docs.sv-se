<properties
   pageTitle="Självstudier för PolyBase i SQL Data Warehouse | Microsoft Azure"
   description="Läs mer om vad PolyBase är och hur man använder det för informationslagerscenarier."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/31/2016"
   ms.author="cakarst;barbkess"/>



# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Läs in data med PolyBase i SQL Data Warehouse

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

De här självstudierna visar hur du läser in data i SQL Data Warehouse med hjälp av AzCopy och PolyBase. När du är klar, kommer du att veta hur man:

- Använder AzCopy för att kopiera data till Azure-blobblagring
- Skapar databasobjekt för att definiera data
- Kör en T-SQL-fråga för att läsa in data

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Förutsättningar

För att gå igenom de här självstudierna behöver du

- En SQL Data Warehouse-databas.
- Ett Azure-lagringskonto av typen standard lokalt redundant lagring (Standard-LRS), standard geo-redundant lagring (Standard-GRS) eller standard geo-redundant lagring med läsbehörighet (Standard-RAGRS).
- Kommandoradsverktyget AzCopy. Hämta och installera den [senaste versionen av AzCopy][] som installeras med Microsoft Azure Storage-verktyg.

    ![Azure Storage-verktyg](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Steg 1: Lägg till exempeldata i Azure blobblagret

För att kunna läsa in data, behöver vi lägga exempeldata i ett Azure-blobblager. I det här steget fyller vi en Azure Storage-blob med exempeldata. Senare kommer vi att använda PolyBase för att läsa in exempeldatan till din SQL Data Warehouse-databas.

### <a name="a-prepare-a-sample-text-file"></a>A. Förbered en exempeltextfil

Förbered en exempeltextfil:

1. Öppna anteckningar och kopiera följande datarader i en ny fil. Spara den på din lokala temp-katalog som %temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Hitta blobbtjänstens slutpunkt

Så här hittar du blobbtjänstens slutpunkt:

1. I Azure Portal väljer du **Bläddra** > **Lagringskonton**.
2. Klicka på det lagringskonto som du vill använda.
3. I bladet Storage-konton klickar du på Blobbar

    ![Klicka på Blobbar](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Spara URL:en för blobbtjänstens slutpunkt för senare.

    ![Blob-tjänstens slutpunkt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Hitta din Azure-lagringsnyckel

Hitta din Azure-lagringsnyckel:

1. I Azure Portal väljer du **Bläddra** > **Lagringskonton**.
2. Klicka på det lagringskonto som du vill använda.
3. Välj **Alla inställningar** > **Åtkomstnycklar**.
4. Klicka på kopiera-rutan för att kopiera en av dina åtkomstnycklar till urklipp.

    ![Kopiera Azure-lagringsnyckel](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Kopiera exempelfilen till Azure-blobblagring

Kopiera dina data till Azure-blobblagring:

1. Öppna en kommandotolk och ändra katalog till installationskatalogen för AzCopy. Det här kommandot ändrar till standard-installationskatalogen på en 64-bitars Windows-klient.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Kör följande kommando för att ladda upp filen. Ange URL:en för blobbtjänstens slutpunkt för <blob service endpoint URL> och nyckeln för Azure-lagringskontot för <azure_storage_account_key>.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Mer information finns i [Kom igång med kommandoradsverktyget AzCopy][].

### <a name="e-explore-your-blob-storage-container"></a>E. Utforska din blobblagringsbehållare

Om du vill se filen du laddade upp till blobblagring:

1. Gå tillbaka till bladet Blob-tjänst.
2. Under Behållare dubbelklickar du på **datacontainer**.
3. Om du vill följa sökvägen till dina data, klickar du på mappen **datedimension** för att se filen **DimDate2.txt** som du laddat upp.
4. Om du vill visa egenskaper, klickar du på **DimDate2.txt**.
5. Observera att bladet för Blob-egenskaper låter dig hämta eller ta bort filen.

    ![Visa Azure-lagringsblobb](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Steg 2: Skapa en extern tabell för exempeldata

I det här avsnittet ska vi skapa en extern tabell som definierar exempeldata.

PolyBase använder sig av externa tabeller för att komma åt data i Azure-blobblagring. Eftersom data inte lagras inom SQL Data Warehouse, hanterar PolyBase autentisering till externa data med hjälp av en databas-omfattande autentisering.

Exemplet i det här steget använder de här Transact-SQL-uttrycken för att skapa en extern tabell.

- [Skapa huvudnyckel (Transact-SQL)][] för att kryptera hemligheten för din databas-omfattande autentisering.
- [Skapa databasomfattande autentisering (Transact-SQL)][] för att ange autentiseringsinformation för ditt Azure-lagringskonto.
- [Skapa extern datakälla (Transact-SQL)][] för att ange platsen för din Azure-blobblagring.
- [Skapa externt filformat (Transact-SQL)][] för att ange formatet för dina data.
- [Skapa extern tabell (Transact-SQL)][] för att ange tabelldefinitionen och platsen för datan.

Kör den här frågan mot din SQL Data Warehouse-databas. Det skapar en extern tabell som heter DimDate2External i dbo-schemat, som pekar på exempeldatan DimDate2.txt i Azure-blobblagret.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


I SQL Server Object Explorer i Visual Studio, kan du se det externa filformatet, den externa datakällan och DimDate2External-tabellen.

![Visa extern tabell](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Steg 3: Läs in data till SQL Data Warehouse

När den externa tabellen har skapats kan du antingen läsa in dina data till en ny tabell eller infoga dem i en befintlig tabell.

- För att läsa in data till en ny tabell, kör du uttrycket [CREATE TABLE AS SELECT (Transact-SQL)][]. Den nya tabellen kommer att ha kolumnerna som namnges i frågan. Datatyperna för kolumnerna kommer att matcha datatyperna i den externa tabelldefinitionen.
- För att läsa in data i en befintlig tabell, använder du uttrycket [INSERT...SELECT (Transact-SQL)][].

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Steg 4: Skapa statistik på dina nyinlästa data

SQL Data Warehouse skapar och uppdaterar inte statistik automatiskt. För att få en hög frågeprestanda är det därför viktigt att skapa statistik för varje kolumn av varje tabell efter den första inläsningen. Det är också viktigt att uppdatera statistiken efter att det har skett betydande förändringar.

Det här exemplet skapar enkolumns-statistik för den nya DimDate2-tabellen.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

För mer information, se [Statistik][].  


## <a name="next-steps"></a>Nästa steg
Se [PolyBase-guiden][] för ytterligare information som du bör känna till när du utvecklar en PolyBase-baserad lösning.

<!--Image references-->


<!--Article references-->
[Självstudie för PolyBase i SQL Data Warehouse]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Läs in data med BCP]: ./sql-data-warehouse-load-with-bcp.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[PolyBase-guide]: ./sql-data-warehouse-load-polybase-guide.md
[Kom igång med kommandoradsverktyget AzCopy]: ../storage/storage-use-azcopy.md
[senaste versionen av AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[källa/mottagare som stöds]: https://msdn.microsoft.com/library/dn894007.aspx
[kopieringsaktivitet]: https://msdn.microsoft.com/library/dn835035.aspx
[Måladapter för SQL Server]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Skapa extern datakälla (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[Skapa externt filformat (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[Skapa extern tabell (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[Släpp extern datakälla (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[Släpp externt filformat (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[Släpp extern tabell (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Skapa tabell som val (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[Infoga ... välj(Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Skapa huvudnyckel (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[Skapa autentiseringsuppgift (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[Skapa databasomfattande autentiseringsuppgift (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[Släpp autentiseringsuppgift (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx



<!--HONumber=Oct16_HO3-->


