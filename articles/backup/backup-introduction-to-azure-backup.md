---
title: "Vad är Azure Backup? | Microsoft Docs"
description: "Du kan säkerhetskopiera och återställa data och program från Windows-servrar, Windows-klientdatorer, System Center DPM-servrar och virtuella datorer i Azure med Azure Backup och Recovery Services."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: tysonn
keywords: "säkerhetskopiering och återställning, återställningstjänster, lösningar för säkerhetskopiering"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2016
ms.author: jimpark; trinadhk
translationtype: Human Translation
ms.sourcegitcommit: e29891dc03f8a864ecacc893fd1cc0d3cc1436cb
ms.openlocfilehash: c827c37ae4164ebd9cd2a971e94f073de8c59b46


---
# <a name="what-is-azure-backup"></a>Vad är Azure Backup?
Azure Backup är en Azure-baserad tjänst som du använder för att säkerhetskopiera (eller skydda) och återställa data i Microsoft-molnet. Azure Backup ersätter din befintliga lokala eller externa säkerhetskopieringslösning med en tillförlitlig och säker molnbaserad lösning med ett konkurrenskraftigt pris. Azure Backup erbjuder flera komponenter som du kan ladda ned och distribuera på den aktuella datorn, servern eller i molnet. Komponenten eller agenten som du distribuerar beror på vad du vill skydda. Alla Azure Backup-komponenter (oavsett om du skyddar data lokalt eller i molnet) kan användas för att säkerhetskopiera data till ett säkerhetskopieringsvalv i Azure. I [tabellen med Azure Backup-komponenter](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (längre ned i den här artikeln) finns information om vilken komponent du ska använda för att skydda specifika data, program eller arbetsbelastningar.

[Titta på en videoöversikt över Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Varför ska jag använda Azure Backup?
Traditionella säkerhetskopieringslösningar har utvecklats för att behandla molnet som en slutpunkt eller statisk lagringsdestination som liknar diskar eller band. Samtidigt som den här metoden är enkel är den begränsad och drar inte full nytta av en underliggande molnplattform, vilket innebär en dyr, ineffektiv lösning. Andra lösningar är dyra eftersom du får betala för fel typ av lagring eller lagring som du inte behöver. Andra lösningar är ofta ineffektiva eftersom de inte tillhandahåller den typ av eller mängd lagringsutrymme du behöver, eller så tar administrativa uppgifter för mycket tid. Däremot ger Azure Backup följande viktiga fördelar:

**Automatisk lagringshantering** – hybridmiljöer kräver ofta heterogen lagring – vissa lokalt och vissa i molnet. Med Azure Backup är det kostnadsfritt att använda lokala lagringsenheter. Azure Backup allokerar och hanterar lagringen av säkerhetskopiorna automatiskt och tillämpar en modell där du betalar baserat på din användning. Att betala baserat på din användning innebär att du bara betalar för den lagring du konsumerar. Mer information finns i artikeln om [prissättning i Azure](https://azure.microsoft.com/pricing/details/backup).

**Obegränsad skalning** – Azure Backup använder Azure-molnets underliggande kraft och obegränsade skala för att tillhandahålla hög tillgänglighet – utan underhåll och omkostnader för övervakning. Du kan ställa in aviseringar för att tillhandahålla information om händelser, men du behöver inte oroa dig för hög tillgänglighet för dina data i molnet.

**Flera lagringsalternativ** – en aspekt av hög tillgänglighet är lagringsreplikering. Azure Backup erbjuder två typer av replikering: [lokalt redundant lagring](../storage/storage-redundancy.md#locally-redundant-storage) och [georeplikerad lagring](../storage/storage-redundancy.md#geo-redundant-storage). Välj lagring för säkerhetskopiering baserat på behov:

* Lokalt redundant lagring (LRS) replikerar data tre gånger (det skapas tre kopior av dina data) i ett parat datacenter i samma region. LRS är ett billigt alternativ som är idealiskt för prismedvetna kunder eftersom det skyddar data mot lokala maskinvarufel.
* Geo-replikerad lagring (GRS) replikerar dina data till en sekundär region (hundratals mil bort från den primära platsen för datakällan). GRS kostar mer än LRS, men ger högre hållbarhet för dina data även om ett regionalt avbrott sker.

**Obegränsad dataöverföring** – Azure Backup begränsar inte hur mycket inkommande eller utgående data du överför. Azure Backup debiterar inte heller för de data som överförs. Om du använder Azure Import/Export-tjänsten för att importera stora mängder data finns det dock en kostnad som är kopplad till inkommande data. Mer information om kostnaden finns i [Offline-backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Arbetsflöde för säkerhetskopiering offline i Azure Backup). Utgående data innebär data som överförs från ett säkerhetskopieringsvalv under en återställning.

**Datakryptering** – Datakryptering möjliggör säker överföring och lagring av dina data i det offentliga molnet. Krypteringslösenfrasen lagras på lokalt och överförs eller lagras aldrig i Azure. Om det är nödvändigt att återställa data kan du göra det om du har krypteringslösenfrasen eller nyckeln.

**Programkonsekvent säkerhetskopiering** – Oavsett om du säkerhetskopierar en filserver, en virtuell dator eller en SQL-databas behöver du veta att en återställningspunkt har alla nödvändiga data för att återställa säkerhetskopian. Azure Backup innehåller programkonsekventa säkerhetskopior vilket garanterar att inga ytterligare korrigeringar behövs för att återställa data. Återställning av konsekventa programdata minskar tiden för återställning, så att du snabbt kan återgå till körläge.

**Långsiktig kvarhållning** – Du kan säkerhetskopiera data till Azure i 99 år. I stället för att växla säkerhetskopior från disk till band och sedan flytta bandet till en annan plats för långsiktig lagring kan du använda Azure för kortsiktig och långsiktig kvarhållning.

## <a name="which-azure-backup-components-should-i-use"></a>Vilka Azure Backup-komponenter ska jag använda?
Om du inte är säker på vilken Azure Backup-komponent som passar dina behov kan du ta en titt i följande tabell och få information om vad du kan skydda med varje komponent. Azure Portal innehåller en guide, som är inbyggd i portalen, som hjälper dig att välja en komponent att ladda ned och distribuera. Guiden som är en del av skapandet av Recovery Services-valvet leder dig genom stegen för att välja ett mål för säkerhetskopiering och data eller program som ska skyddas.

| Komponent | Fördelar | Begränsningar | Vad skyddas? | Var lagras säkerhetskopiorna? |
| --- | --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<li>Säkerhetskopiera filer och mappar på fysisk eller virtuell dator med Windows OS (virtuella datorer kan finnas lokalt eller i Azure)<li>Ingen separat säkerhetskopieringsserver krävs. |<li>Säkerhetskopiera 3 gånger per dag <li>Inte programmedveten, endast återställning på fil-/mapp-/volymnivå, <li>  Inget stöd för Linux. |<li>Filer, <li>Mappar |Azure Backup-valv |
| System Center DPM |<li>Appmedvetna ögonblicksbilder (VSS)<li>Fullständig flexibilitet när du vill skapa säkerhetskopior<li>Återställningsprecision (allt)<li>Kan använda Azure Backup-valv<li>Linux-stöd (om den finns på Hyper-V) |Inget heterogent stöd (säkerhetskopiering av virtuella datorer med VMware, säkerhetskopiering av Oracle-arbetsbelastningar). |<li>Filer, <li>Mappar,<li> Volymer, <li>Virtuella datorer,<li> Program,<li> Arbetsbelastningar |<li>Azure Backup-valvet,<li> Lokalt ansluten disk,<li>  Band (endast lokalt) |
| Azure Backup Server |<li>Appmedvetna ögonblicksbilder (VSS)<li>Fullständig flexibilitet när du vill skapa säkerhetskopior<li>Återställningsprecision (allt)<li>Kan använda Azure Backup-valv<li>Linux-stöd (om den finns på Hyper-V)<li>Kräver inte en System Center-licens |<li>Inget heterogent stöd (säkerhetskopiering av virtuella datorer med VMware, säkerhetskopiering av Oracle-arbetsbelastningar).<li>Kräver alltid en aktiv Azure-prenumeration<li>Inget stöd för säkerhetskopiering på band |<li>Filer, <li>Mappar,<li> Volymer, <li>Virtuella datorer,<li> Program,<li> Arbetsbelastningar |<li>Azure Backup-valvet,<li> Lokalt ansluten disk |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |<li>Interna säkerhetskopieringar för Windows/Linux<li>Ingen specifik agentinstallation krävs<li>Säkerhetskopiering på infrastrukturnivå utan behov av en infrastruktur för säkerhetskopiering |<li>Återställning på säkerhetskopie-/disknivå en gång om dagen<li>Det går inte att säkerhetskopiera lokalt |<li>Virtuella datorer, <li>Alla diskar (med PowerShell) |<p>Azure Backup-valv</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Vilka är distributionsscenarierna för varje komponent?
| Komponent | Kan den distribueras i Azure? | Kan den distribuerade lokalt? | Mållagring som stöds |
| --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<p>**Ja**</p> <p>Azure Backup-agenten kan distribueras på virtuella datorer med Windows som körs i Azure.</p> |<p>**Ja**</p> <p>Backup-agenten kan distribueras på virtuella datorer med Windows Server eller en fysisk dator.</p> |<p>Azure Backup-valv</p> |
| System Center DPM |<p>**Ja**</p><p>Lär dig mer om [hur du skyddar arbetsbelastningar i Azure med hjälp av System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ja**</p> <p>Lär dig mer om [hur du skyddar arbetsbelastningar och virtuella datorer i ditt datacenter](https://technet.microsoft.com/en-us/system-center-docs/dpm/data-protection-manager).</p> |<p>Lokalt ansluten disk,</p> <p>Azure Backup-valvet,</p> <p>band (endast lokalt)</p> |
| Azure Backup Server |<p>**Ja**</p><p>Lär dig mer om [hur du skyddar arbetsbelastningar i Azure med Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ja**</p> <p>Lär dig mer om [hur du skyddar arbetsbelastningar i Azure med Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>Lokalt ansluten disk,</p> <p>Azure Backup-valv</p> |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |<p>**Ja**</p><p>En del av Azure-infrastrukturen</p><p>Specialiserad för [säkerhetskopiering av virtuella Iaas-datorer (Infrastructure as a Service) i Azure](backup-azure-vms-introduction.md).</p> |<p>**Nej**</p> <p>Använd System Center DPM för att säkerhetskopiera virtuella datorer i datacentret.</p> |<p>Azure Backup-valv</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Vilka program och arbetsbelastningar kan säkerhetskopieras?
Följande tabell innehåller en matris med data och arbetsbelastningar som kan skyddas med Azure Backup. I kolumnen med Azure Backup-lösningar finns länkar till dokumentationen för lösningen. Varje komponent i Azure Backup kan distribueras i en klassisk (Service Manager-distribuering) eller Resource Manager-modellmiljö för distribuering.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Data eller arbetsbelastning | Källmiljö | Azure Backup-lösning |
| --- | --- | --- |
| Filer och mappar |Windows Server |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Filer och mappar |Windows-dator |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuell Hyper-V-dator (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuell Hyper-V-dator (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuella IaaS-datorer i Azure (Windows) |som körs i Azure |[Azure Backup (VM-tillägg)](backup-azure-vms-introduction.md) |
| Virtuella IaaS-datorer i Azure (Linux) |som körs i Azure |[Azure Backup (VM-tillägg)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux-support
I följande tabell visas de Azure Backup-komponenter som har stöd för Linux.  

| Komponent | Linux-stöd (Azure-godkänt) |
| --- | --- |
| Azure Backup-agent (MARS) |Nej (endast Windows-baserad agent) |
| System Center DPM |Filkonsekvent säkerhetskopiering eller endast Hyper-V<br/> (inte tillgängligt för virtuella Azure-datorer) |
| Azure Backup Server |Filkonsekvent säkerhetskopiering eller endast Hyper-V<br/> (inte tillgängligt för virtuella Azure-datorer) |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |Ja |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Använd virtuella Premium Storage-datorer med Azure Backup
Azure Backup skyddar virtuella datorer i Premium Storage. Azure Premium Storage är SSD-baserad (solid-state drive) lagring som har utformats för att fungera med I/O-intensiva arbetsbelastningar. Premium Storage är attraktivt för arbetsbelastningar för virtuella datorer. Mer information om Premium-lagring finns i artikeln [Premium Storage: högpresterande lagring för virtuella Azure-datorbelastningar](../storage/storage-premium-storage.md)

### <a name="back-up-premium-storage-vms"></a>Säkerhetskopiera virtuella datorer i Premium Storage
När du säkerhetskopierar virtuella datorer i Premium Storage skapar Backup-tjänsten en tillfällig mellanlagringsplats i Premium Storage-kontot. Mellanlagringsplatsen, som har namnet ”AzureBackup-”, motsvarar den totala datastorleken på premium-diskarna som är kopplade till den virtuella datorn.

> [!NOTE]
> Ändra inte mellanlagringsplatsen.
>
>

När säkerhetskopieringen är klar tas mellanlagringsplatsen bort. Priset för lagringen som används för mellanlagringsplatsen följer [prissättningen för Premium-lagring](../storage/storage-premium-storage.md#pricing-and-billing).

### <a name="restore-premium-storage-vms"></a>Återställa virtuella datorer i Premium Storage
Virtuell dator för Premium Storage VM kan återställas till antingen Premium Storage-lagring eller normal lagring. I en typisk återställningsprocess återställs en återställningspunkt för virtuella datorer i Premium Storage till Premium Storage. Det kan dock vara kostnadseffektivt att återställa en återställningspunkt för virtuella datorer i Premium Storage till standardlagring. Den här typen av återställning kan vara praktisk om du behöver en delmängd av filerna från den virtuella datorn.

## <a name="what-are-the-features-of-each-backup-component"></a>Vilka är funktionerna i varje Backup-komponent?
Följande avsnitt innehåller tabeller som sammanfattar tillgänglighet eller stöd för olika funktioner i varje komponent i Azure Backup. Titta på informationen efter varje tabell för ytterligare support eller information.

### <a name="storage"></a>Lagring
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Azure Backup-valv |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Disklagring | |![Ja][green] |![Ja][green] | |
| Bandlagring | |![Ja][green] | | |
| Komprimering <br/>(i säkerhetskopieringsvalvet) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Inkrementell säkerhetskopiering |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Diskdeduplicering | |![Delvis][yellow] |![Delvis][yellow] | |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key.png)

Backup-valvet är det prioriterade lagringsmålet i alla komponenter. Med System Center DPM och Backup Server kan du också välja att kopiera en lokal disk. Dock kan du endast skriva data till en bandlagringsenhet med System Center DPM.

#### <a name="compression"></a>Komprimering
Säkerhetskopior komprimeras för att minska lagringsutrymmet som krävs. Den enda komponenten som inte använder komprimering är VM-tillägget. När du använder VM-tillägget kopieras alla säkerhetskopieringsdata från ditt lagringskonto till säkerhetskopieringsvalvet i samma region utan att de komprimeras. Lagringsutrymmet som används ökar något när ingen komprimering används. Men återställningen går snabbare om du lagrar data utan komprimering.

#### <a name="incremental-backup"></a>Inkrementell säkerhetskopiering
Alla komponenter stöder inkrementell säkerhetskopiering oavsett mållagring (disk, band eller säkerhetskopieringsvalv). Inkrementell säkerhetskopiering ser till att säkerhetskopieringarna är lagrings- och tidseffektiva genom att endast överföra de ändringar som gjorts sedan den senaste säkerhetskopieringen.

#### <a name="disk-deduplication"></a>Diskdeduplicering
Du kan dra nytta av datadeduplicering när du distribuerar System Center DPM eller Azure Backup Server [på en virtuell Hyper-V-dator](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). Windows Server utför datadeduplicering (på värdnivå) på virtuella hårddiskar (VHD) som är kopplade till den virtuella datorn som lagringsutrymme för säkerhetskopior.

> [!NOTE]
> Deduplicering är inte tillgängligt i Azure för någon Backup-komponenter. Om System Center DPM och Backup Server distribueras i Azure går det inte att deduplicera lagringsdiskarna som är kopplade till den virtuella datorn.
>
>

### <a name="security"></a>Säkerhet
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Nätverkssäkerhet<br/> (för Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Delvis][yellow] |
| Datasäkerhet<br/> (i Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Delvis][yellow] |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Nätverkssäkerhet
All säkerhetskopieringstrafik från dina servrar till säkerhetskopieringsvalvet krypteras med hjälp av Advanced Encryption Standard 256. Säkerhetskopierade data skickas via en säker HTTPS-anslutning. Säkerhetskopierade data lagras också i Backup-valvet i krypterad form. Endast du, Azure-kunden, har tillgång till lösenfrasen som krävs för att låsa upp dessa data. Microsoft kan aldrig dekryptera säkerhetskopierade data.

> [!WARNING]
> När du har skapat säkerhetskopieringsvalvet har bara du åtkomst till krypteringsnyckeln. Microsoft sparar aldrig en kopia av krypteringsnyckeln och har inte åtkomst till nyckeln. Om du tappar bort nyckeln kan inte Microsoft återställa dina säkerhetskopierade data.
>
>

#### <a name="data-security"></a>Datasäkerhet
Säkerhetskopieringen av virtuella datorer i Azure kräver krypteringsinställningar *på* den virtuella datorn. Använd BitLocker på virtuella Windows-datorer och **dm crypt** på virtuella Linux-datorer. Azure Backup krypterar inte automatiskt säkerhetskopieringsdata som finns på den här sökvägen.

### <a name="network"></a>Nätverk
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Nätverkskomprimering <br/>(till **backup server**) | |![Ja][green] |![Ja][green] | |
| Nätverkskomprimering <br/>(till **säkerhetskopieringsvalvet**) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Nätverksprotokoll <br/>(till **backup server**) | |TCP |TCP | |
| Nätverksprotokoll <br/>(till **säkerhetskopieringsvalvet**) |HTTPS |HTTPS |HTTPS |HTTPS |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key-2.png)

VM-tillägget (eller virtuella IaaS-datorer) läser data direkt från Azure Storage-kontot i lagringsnätverket behöver du inte komprimera den här trafiken.

Om du säkerhetskopierar data till ett System Center DPM eller Azure Backup Server ska komprimerade data gå från den primära servern till säkerhetskopieringsservern. Detta sparar bandbredd.

#### <a name="network-throttling"></a>Nätverksbegränsningar
Azure Backup-agenten tillhandahåller nätverksbegränsning som du kan använda för att styra hur nätverksbandbredden används under dataöverföringar. Begränsning kan vara användbart om du behöver säkerhetskopiera data under arbetstid, men inte vill att säkerhetskopieringsprocessen ska störa annan Internettrafik. Begränsningar av dataöverföringar gäller säkerhetskopierings- och återställningsaktiviteter.

### <a name="backup-and-retention"></a>Säkerhetskopiering och kvarhållning
|  | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Säkerhetskopieringsfrekvens<br/> (till säkerhetskopieringsvalvet) |Tre säkerhetskopieringar om dagen |Två säkerhetskopieringar om dagen |Två säkerhetskopieringar om dagen |En säkerhetskopiering om dagen |
| Säkerhetskopieringsfrekvens<br/> (till disk) |Inte tillämpligt |<li>Varje kvart för SQL Server <li>Varje timme för andra arbetsbelastningar |<li>Varje kvart för SQL Server <li>Varje timme för andra arbetsbelastningar</p> |Inte tillämpligt |
| Kvarhållningsalternativ |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |
| Kvarhållningsperiod |Upp till 99 år |Upp till 99 år |Upp till 99 år |Upp till 99 år |
| Återställningspunkter i Backup-valv |Obegränsat |Obegränsat |Obegränsat |Obegränsat |
| Återställningspunkter på lokal disk |Inte tillämpligt |<li>64 för filservrar,<li>448 för programservrar |<li>64 för filservrar,<li>448 för programservrar |Inte tillämpligt |
| Återställningspunkter på band |Inte tillämpligt |Obegränsat |Inte tillämpligt |Inte tillämpligt |

## <a name="what-is-the-vault-credential-file"></a>Vad är valvautentiseringsfilen?
Valvautentiseringsfilen är ett certifikat som genereras av portalen för varje säkerhetskopieringsvalv. Portalen överför sedan den offentliga nyckeln till Access Control Service (ACS). Den privata nyckeln får du när du laddar ned autentiseringsuppgifterna. Du kan använda den för att registrera de datorer som du ska skydda. Med den privata nyckeln kan du autentisera servrar eller datorer för att skicka säkerhetskopierade data till ett visst säkerhetskopieringsvalv.

Du använder bara valvautentiseringen för att registrera servrar eller datorer. Men var noga med autentiseringsuppgifter för valv. Om de tappas bort eller hämtas av andra kan autentiseringsuppgifterna användas för att registrera andra datorer mot samma valv. Eftersom säkerhetskopierade data krypteras med en lösenfras som endast du kan få åtkomst till kan inte befintliga säkerhetskopierade data komprometteras. Valvautentiseringsuppgifterna upphör att gälla efter 48 timmar. Du kan ladda ned autentiseringsuppgifter för säkerhetskopieringsvalvet så ofta du vill, men du kan enbart använda de senaste autentiseringsuppgifterna för registrering.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Vad är skillnaden mellan Azure Backup och Azure Site Recovery?
Azure Backup och Azure Site Recovery är relaterade eftersom båda tjänsterna säkerhetskopierar och återställer data, men deras huvudsakliga tillämpning skiljer sig åt.

Azure Backup skyddar data lokalt och i molnet. Azure Site Recovery samordnar replikeringen på virtuella datorer och fysiska servrar, redundans och återställning. Båda tjänsterna är viktiga eftersom en haveriberedskapslösning måste skydda dina data, se till att de kan återställas (säkerhetskopiering) *och* säkerställa att arbetsbelastningarna förblir tillgängliga (Site Recovery) i händelse av avbrott.

Följande begrepp hjälper dig att fatta viktiga beslut om säkerhetskopiering och haveriberedskap.

| Begrepp | Information | Säkerhetskopiering | Haveriberedskap |
| --- | --- | --- | --- |
| Mål för återställningspunkt (RPO) |Mängden godtagbar dataförlust om en återställning krävs. |Säkerhetskopieringslösningar har stor variation vad gäller deras godtagbara återställningspunktmål. Säkerhetskopieringar av virtuella datorer har vanligtvis ett återställningspunktmål på en dag, medan säkerhetskopieringar av databaser har återställningspunktmål på så lite som 15 minuter. |Lösningar för haveriberedskap har låga återställningspunktmål. Kopian för haveriberedskap kan ligga några få sekunder eller minuter efter. |
| Mål för återställningstid (RTO) |Hur lång tid det tar att slutföra en återställning. |På grund av det större återställningspunktmålet är mängden data som en säkerhetskopieringslösning behöver bearbeta normalt mycket högre, vilket leder till längre mål för återställningstid. Det kan till exempel ta dagar att återställa data från band, beroende på hur lång tid det tar att överföra bandet från den externa platsen. |Lösningar för haveriberedskap har mindre mål för återställningstid eftersom de är mer synkroniserade med källan. Färre ändringar behöver bearbetas. |
| Kvarhållning |Hur länge data ska lagras. |För scenarier som kräver driftåterställning (datafel, oavsiktlig filborttagning, operativsystemfel osv.) bevaras säkerhetskopierade data vanligtvis i 30 dagar eller mindre.<br>Ur efterlevnadssynvinkel kanske data behöver sparas i flera månader eller till och med år. Säkerhetskopierade data är idealiska för arkivering i dessa fall. |Haveriberedskap kräver endast driftåterställningsdata, som normalt tar några timmar eller upp till en dag. På grund av den detaljerade datainsamlingen i lösningar för haveriberedskap rekommenderar vi inte att haveriberedskapsdata används för långsiktig kvarhållning. |

## <a name="next-steps"></a>Nästa steg
Använd någon av följande självstudiekurser för att få detaljerade steg för steg-instruktioner för hur du skyddar data på Windows Server eller på en virtuell dator (VM) i Azure:

* [Säkerhetskopiera filer och mappar](backup-try-azure-backup-in-10-mins.md)
* [Säkerhetskopiera Azure Virtual Machines](backup-azure-vms-first-look.md)

Mer information om att skydda andra arbetsbelastningar finns i någon av följande artiklar:

* [Säkerhetskopiera Windows Server](backup-configure-vault.md)
* [Säkerhetskopiera programarbetsbelastningar](backup-azure-microsoft-azure-backup.md)
* [Säkerhetskopiera virtuella IaaS-datorer i Azure](backup-azure-vms-prepare.md)

[grön]: ./media/backup-introduction-to-azure-backup/green.png
[gul]: ./media/backup-introduction-to-azure-backup/yellow.png
[röd]: ./media/backup-introduction-to-azure-backup/red.png



<!---HONumber=Nov16_HO2-->


