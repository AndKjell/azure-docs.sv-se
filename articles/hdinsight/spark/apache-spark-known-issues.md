---
title: Felsöka problem med Apache Spark-kluster i Azure HDInsight
description: Lär dig om problem relaterade till Apache Spark-kluster i Azure HDInsight och hur du undviker de.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: 92baa28393100abe3d7694920e5ee327966db927
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048316"
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Kända problem med Apache Spark-kluster i HDInsight

Det här dokumentet håller reda på om kända problem för den offentliga förhandsversionen för HDInsight Spark.  

## <a name="livy-leaks-interactive-session"></a>Livy läcker interaktiva sessionen
När Livy startas om (från Ambari eller på grund av omstart av huvudnod 0 VM) med en interaktiv session fortfarande lever, har en interaktiv jobbet session läckts. Nya jobb kan därför ha fastnat i tillståndet accepterad.

**Lösning:**

Använd följande procedur för att lösa problemet:

1. SSH till huvudnoden. Mer information finns i [Use SSH with HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Kör följande kommando för att hitta program-ID: N för interaktiva jobben igång via Livy. 
   
        yarn application –list
   
    Standardnamn för jobbet kommer att Livy om jobb startades med en interaktiv session Livy med ingen explicit namnen har angetts. Livy-sessionen är igång genom att Jupyter-anteckningsbok Jobbnamnet börjar med remotesparkmagics_ *. 
3. Kör följande kommando för att avsluta jobben. 
   
        yarn application –kill <Application ID>

Nya jobb börja köra. 

## <a name="spark-history-server-not-started"></a>Spark-Historikserver som inte har startats
Spark-Historikserver startas inte automatiskt när ett kluster har skapats.  

**Lösning:** 

Starta servern historik manuellt från Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Behörighetsproblem i loggkatalogen för Spark
hdiuser hämtar följande fel när du skickar ett jobb med hjälp av spark-submit:

```
java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied)
```
Och inga drivrutinen loggen att skivas. 

**Lösning:**

1. Lägga till hdiuser i Hadoop-gruppen. 
2. Ange 777 behörigheter på /var/log/spark när klustret har skapats. 
3. Uppdatera sökvägen till spark-loggen med Ambari ska vara en katalog med 777 behörigheter.  
4. Kör spark-submit som sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Spark-Phoenix anslutningen stöds inte

HDInsight Spark-kluster har inte stöd för Spark-Phoenix-anslutning.

**Lösning:**

I stället måste du använda Spark-HBase-anslutningen. Anvisningar finns i [hur du använder Spark-HBase-anslutningen](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-to-jupyter-notebooks"></a>Problem som rör Jupyter-anteckningsböcker
Följande är några kända problem som rör Jupyter-anteckningsböcker.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>-Anteckningsböcker med icke-ASCII-tecken i filnamn
Använd inte icke-ASCII-tecken i Jupyter-anteckningsbok filnamn. Om du försöker ladda upp en fil via Jupyter UI, som har ett icke-ASCII-filnamn, misslyckas utan eventuella felmeddelanden. Jupyter kan inte överföra filen, men det inget genereras ett fel som visas antingen.

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fel vid inläsning av anteckningsböcker i större storlekar
Du kan se ett fel **`Error loading notebook`** när du läser in anteckningsböcker som är större.  

**Lösning:**

Om du får det här felet kan innebär det inte dina data är skadad eller förlorade.  Dina anteckningsböcker som finns kvar på disken i `/var/lib/jupyter`, och du kan SSH till klustret för att komma åt dem. Mer information finns i [Use SSH with HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

När du har anslutit till klustret med SSH kan kopiera du dina anteckningsböcker från klustret till den lokala datorn (med SCP eller WinSCP) som en säkerhetskopia för att förhindra förlust av viktiga data i anteckningsboken. Du kan sedan SSH-tunnel till din huvudnoden på port 8001 till Jupyter utan att gå via gatewayen.  Därifrån kan du rensa utdata från din bärbara dator och spara den för att minimera storleken på den bärbara datorn.

Om du vill förhindra att det här felet igen i framtiden, måste du följa några metodtips:

* Det är viktigt att hålla notebook filstorleken mycket. Alla utdata från Spark-jobb som skickas tillbaka till Jupyter görs beständiga i anteckningsboken.  Det är bästa praxis med Jupyter i allmänhet att undvika `.collect()` på stora RDDS eller dataramar; om du vill granska en RDD innehållet i stället körs `.take()` eller `.sample()` så att dina utdata inte blir för stor.
* Dessutom när du sparar en bärbar dator, utdata Rensa alla celler att minska appens storlek.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Anteckningsboken första starten tar längre tid än förväntat
Den första koden instruktionen i Jupyter-anteckningsbok med Spark magic kan ta mer än en minut.  

**Förklaring:**

Detta beror på att när den första kodcellen körs. I bakgrunden detta startar sessionskonfigurationen och Spark, SQL och Hive-kontexterna ställs. När dessa sammanhang anges, den första instruktionen körs och det ger intryck som tog instruktionen lång tid att slutföra.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Tidsgräns för Jupyter notebook i att skapa sessionen
När Spark-kluster har slut på resurser, når Spark och PySpark kernlar i Jupyter-anteckningsboken tidsgränsen som försöker skapa sessionen. 

**Åtgärder:** 

1. Fri några resurser i ditt Spark-kluster genom att:
   
   * Stoppar andra Spark-anteckningsböcker som kommer att Stäng och stopp-menyn eller klicka på Avsluta i anteckningsboken explorer.
   * Stoppar andra Spark-program från YARN.
2. Starta om anteckningsboken som du försöker starta. Tillräckligt med resurser ska vara tillgänglig för dig att skapa en session nu.

## <a name="see-also"></a>Se också
* [Översikt: Apache Spark i Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll](apache-spark-machine-learning-mllib-ipython.md)
* [Webbplatslogganalys med Spark i HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar](apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för Apache Spark-klustret i Azure HDInsight](apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](apache-spark-job-debugging.md)

