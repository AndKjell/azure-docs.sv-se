---
title: Skapa en CI/CD-pipeline för PHP med Azure DevOps-projekt | Snabbstart
description: DevOps-projekt gör det enkelt att komma igång med Azure. Det hjälper dig att starta en app på en Azure-tjänst med några enkla få steg.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: eba23f9e30dff74d19252e93bdedd82bb55599b2
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967024"
---
# <a name="create-a-cicd-pipeline-for-php-with-the-azure-devops-project"></a>Skapa en CI/CD-pipeline för PHP med Azure DevOps-projekt

Azure DevOps-projektet ger ett förenklat sätt att skapa Azure-resurser och konfigurerar en pipeline för kontinuerlig integrering (CI) och kontinuerlig leverans (CD) för din PHP-app i Visual Studio Team Services (VSTS), som är Microsofts DevOps-lösning för Azure.  

Om du inte har en Azure-prenumeration kan du skaffa en kostnadsfritt via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure Portal

Azure DevOps-projektet skapar en CI/CD-pipeline i VSTS.  Du kan skapa ett kostnadsfritt **nytt VSTS**-konto eller använda ett **befintligt konto**.  DevOps-projektet skapar även **Azure-resurser** i den **Azure-prenumeration** som du väljer.

1. Logga in i [Microsoft Azure-portalen](https://portal.azure.com).

1. Välj ikonen **Skapa en resurs** i det vänstra navigeringsfältet och sök efter **DevOps-projekt**.  Välj **Skapa**.

    ![Börja konfiguration av kontinuerlig leverans](_img/azure-devops-project-php/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Välj ett exempelprogram och en Azure-tjänst

1. Välj **PHP**-exempelprogrammet.  PHP-exemplet innehåller ett val av flera programramverk.

1. Exempelramverket som är standard är **Laravel**. Låt standardinställningen vara och välj **Nästa**.  

1. **Web App for Containers** är distributionsmålet som är standard.  Programramverket som du valde i föregående steg avgör vilka typer av distributionsmål som finns tillgängliga för Azure-tjänsten här.  Låt standardinställningen för tjänsten vara och välj **Nästa**.
 
## <a name="configure-vsts-and-an-azure-subscription"></a>Konfigurera VSTS och en Azure-prenumeration 

1. Skapa ett **nytt** VSTS-konto eller välj ett **befintligt** konto.  Välj ett **namn** för ditt VSTS-projekt.  Välj din **Azure-prenumeration**, **plats** och välj ett **namn** för ditt program.  När du är klar väljer du **Klar**.

    ![Ange VSTS-information](_img/azure-devops-project-php/vstsazureinfo.png)

1. **Projektinstrumentpanelen** läses in i Azure-portalen på några minuter.  Ett exempelprogram konfigureras i en lagringsplats i VSTS-kontot, en version körs och programmet distribueras till Azure.  Den här instrumentpanelen ger insyn i **kodlagringsplatsen**, **VSTS CI/CD-pipeline** och i ditt **program i Azure**.  På höger sida av instrumentpanelen väljer du **Bläddra** för att visa dt program som körs.

    ![Instrumentpanelsvy](_img/azure-devops-project-php/dashboardnopreview.png) 
    
Azure DevOps-projektet konfigurerar automatiskt en CI-version och släpper utlösaren.  Nu är du redo att samarbeta med ett team på en PHP-app med en CI/CD-process som automatiskt distribuerar ditt senaste arbete till din webbplats.

## <a name="commit-code-changes-and-execute-cicd"></a>Genomför ändringar i koden och kör CI/CD

Azure DevOps-projektet skapade en Git-lagringsplats i din VSTS eller ditt GitHub-konto.  Följ stegen nedan för att visa lagringsplatsen och gör ändringar i koden i programmet.

1. På vänster sida av instrumentpanelen för DevOps-projektet väljer du länken för din **huvudgren**.  Den här länken öppnar en vy till den nyligen skapade Git-lagringsplatsen.

1. Om du vill visa webbadressen för den klonade lagringsplatsen väljer du **Klona** längst upp till höger i webbläsaren. Du kan klona Git-lagringsplatsen till din favorit-IDE.  I kommande steg kan du använda webbläsaren för att göra och checka in ändringar i koden direkt till huvudgrenen.

1. På vänster sida i webbläsaren navigerar du till filen **resources/views/welcome.blade.php**.

1. Välj **Redigera** och gör en ändring i en del av texten.  Du kan till exempel ändra en del av texten för en av div-taggarna.

1. Välj **Checka in** och spara sedan ändringarna.

1. I webbläsaren navigerar du till **instrumentpanelen för Azure DevOps-projektet**.  Du bör nu se att en version håller på att skapas.  De ändringar du just utfört skapas och distribueras automatiskt via en VSTS-CI/CD-pipeline.

## <a name="examine-the-vsts-cicd-pipeline"></a>Granska VSTS-CI/CD-pipelinen

Azure DevOps-projektet konfigurerade automatiskt en fullständig VSTS-CI/CD-pipeline i VSTS-kontot.  Utforska och anpassa pipelinen efter behov.  Följ stegen nedan för att bekanta dig med VSTS-versionen och versionsdefinitioner.

1. Välj **Skapa pipelines** **längst upp** på instrumentpanelen för Azure DevOps-projektet.  Den här länken öppnar en flik i webbläsaren och öppnar VSTS-versionsdefinitionen för det nya projektet.

1. Flytta markören till höger om versionsdefinitionen bredvid fältet **Status**. Välj den **ellips** som visas.  Den här åtgärden öppnar en meny där du kan starta flera aktiviteter, till exempel att lägga till en ny version i en kö, pausa en version och redigera versionsdefinitionen.

1. Välj **Redigera**.

1. Från den här vyn **granskar du de olika uppgifterna** för versionsdefinitionen.  Versionen kör olika uppgifter som att hämta källor från Git-lagringsplatsen, återställa beroenden och publicera utdata för distributioner.

1. Längst upp i versionsdefinitionen väljer du **namn på versionsdefinitionen**.

1. Ändra **namnet** på din versionsdefinition till något mer beskrivande.  Välj **Spara och lägg till i kö** och sedan **Spara**.

1. Under versionsdefinitionsnamnet väljer du **Historik**.  Du kan se en spårningslogg över de senaste ändringarna för versionen.  VSTS spårar alla ändringar som görs på versionsdefinition, vilket gör att du kan jämföra versioner.

1. Välj **Utlösare**.  Azure DevOps-projektet skapade automatiskt en CI-utlösare, och varje incheckning till lagringsplatsen startar en ny version.  Du kan välja att inkludera eller exkludera grenar från CI-processen.

1. Välj **Kvarhållning**.  Baserat på ditt scenario kan du ange principer för att behålla eller ta bort ett visst antal versioner.

1. Välj **Build and Release** (Build-versioner och versioner) och sedan **Versioner**.  Azure DevOps-projektet skapade en VSTS-versionsdefinition för att hantera distributioner till Azure.

1. På vänster sida i webbläsaren väljer du **ellipsen** bredvid din versionsdefinition och därefter **Redigera**.

1. Versionsdefinitionen innehåller en **pipeline** som definierar lanseringsprocessen.  Under **Artefakter** väljer du **Släpp**.  Den versionsdefinition du undersökte i de föregående stegen skapar de utdata som används för artefakten. 

1. På höger sida av ikonen **Släpp** väljer du **Utlösare av kontinuerlig distribution**.  Den här versionsdefinitionen har en aktiverad CD-utlösare som kör en distribution varje gång en ny versionsartefakt är tillgänglig.  Du kan även inaktivera utlösaren så att dina distributioner kräver manuell körning. 

1. På vänster sida i webbläsaren väljer du **Uppgifter**.  Uppgifter är de aktiviteter som distributionsprocessen utför.  I det här exemplet skapades en uppgift för att distribuera till **Azure App-tjänsten**.

1. Till höger i webbläsaren väljer du **Visa versioner**.  Den här vyn visar en historik över versioner.

1. Välj **ellipsen** bredvid en versionerna och välj **Öppna**.  Det finns flera menyer att utforska från den här vyn, till exempel en versionssammanfattning, tillhörande arbetsobjekt och tester.

1. Välj **Incheckningar**.  Den här vyn visar kodincheckningar som är associerade med den specifik distributionen. 

1. Välj **Loggar**.  Loggarna innehåller användbar information om distributionsprocessen.  De kan visas både under och efter distributionerna.

## <a name="clean-up-resources"></a>Rensa resurser

När de inte längre behövs kan du ta bort Azure App-tjänsten och relaterade resurser som skapats i den här snabbstarten med hjälp av funktionen **Ta bort** på instrumentpanelen för Azure DevOps-projektet.

## <a name="next-steps"></a>Nästa steg

När du konfigurerade CI/CD-processen i den här snabbstarten skapades automatiskt en build-versions- och en versionsdefinition i VSTS-projektet. Du kan ändra dessa build-versions- och versionsdefinitioner för att uppfylla behoven i ditt team. Mer information finns i den här självstudien:

> [!div class="nextstepaction"]
> [Anpassa CD-process](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
