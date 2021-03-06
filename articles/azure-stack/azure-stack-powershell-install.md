---
title: Installera PowerShell för Azure Stack | Microsoft Docs
description: Lär dig mer om att installera PowerShell för Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: f6644d8a2e01242937943f8139059abbd65d1913
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48017407"
---
# <a name="install-powershell-for-azure-stack"></a>Installera PowerShell för Azure Stack

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

För att fungera med ditt moln, måste du installera Azure Stack-kompatibla PowerShell-moduler. Kompatibilitet aktiveras via en funktion som kallas *API profiler*.

API-profilerna ger ett sätt att hantera skillnaderna mellan Azure och Azure Stack. En profil för API-versionen är en uppsättning Azure Resource Managers PowerShell-moduler med specifika API-versioner. Varje molnplattform har en uppsättning API-versionsprofiler stöds. Till exempel Azure Stack stöd för en specifik daterat profil-version som **2018-03-01-hybrid**, och Azure har stöd för den **senaste** API version profil. När du installerar en profil kan installeras på Azure Resource Managers PowerShell-moduler som motsvarar den angivna profilen.

Du kan installera Azure Stack kompatibla PowerShell-moduler i Internet ansluten, delvis ansluten eller frånkopplad scenarier. Den här artikeln beskriver de detaljerade anvisningarna för att installera PowerShell för Azure Stack för dessa scenarier.

## <a name="1-verify-your-prerequisites"></a>1. Kontrollera ditt krav är uppfyllda

Innan du sätter igång med Azure Stack och PowerShell, måste du ha följande krav:

- **PowerShell Version 5.0** för att kontrollera vilken version du kör **$PSVersionTable.PSVersion** och jämföra den **större** version. Om du inte har PowerShell 5.0, följer du de [installera Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > PowerShell 5.0 kräver en Windows-dator.

- **Kör Powershell i en upphöjd kommandotolk** måste du köra PowerShell med administratörsbehörighet.

- **PowerShell-galleriet åtkomst** du behöver åtkomst till den [PowerShell-galleriet](https://www.powershellgallery.com). Galleriet är en central lagringsplats för PowerShell-innehåll. Den **PowerShellGet** modulen innehåller cmdlets för identifiering, installera, uppdatera och publicera PowerShell artefakter som moduler, DSC-resurser, rollfunktioner och skript från PowerShell-galleriet och andra privata databaser. Om du använder PowerShell i ett frånkopplat scenario, måste du hämta resurser från en dator med en anslutning till Internet och lagra dem på en plats som är tillgängliga för frånkopplade datorn.


## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. Kontrollera tillgängligheten för PowerShell-galleriet

Kontrollera om PSGallery är registrerad som en databas.

> [!Note]
> Det här steget kräver Internetåtkomst.

Öppna en upphöjd PowerShell-kommandotolk och kör följande cmdlets:

````PowerShell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
````

Om databasen inte är registrerad, öppna en upphöjd PowerShell-session och kör följande kommando:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Avinstallera befintliga versioner av Azure Stack PowerShell-moduler

Innan du installerar versionen som krävs, se till att avinstallera eventuella tidigare installerade Azure Stack AzureRM PowerShell-moduler. Du kan avinstallera dem på något av följande två metoder:

1. Om du vill avinstallera de befintliga AzureRM PowerShell-modulerna, Stäng alla aktiva PowerShell-sessioner och kör följande cmdlets:

  ````PowerShell
    Uninstall-Module -Name AzureRM.AzureStackAdmin -Force
    Uninstall-Module -Name AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force
  ````

2. Ta bort alla mappar som börjar med `Azure` från den `C:\Program Files\WindowsPowerShell\Modules` och `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` mappar. Tar bort dessa mappar tar bort alla befintliga PowerShell-moduler.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Ansluten: Installera PowerShell för Azure Stack med Internetanslutning

Azure Stack kräver den **2018-03-01-hybrid** API version profil för Azure Stack-version 1808. Profilen är tillgänglig genom att installera den **AzureRM.Bootstrapper** modulen. Dessutom bör till AzureRM-moduler, du även installera Azure Stack-specifika PowerShell-moduler. Profil för API-version och Azure Stack PowerShell-moduler som du behöver beror på vilken version av Azure Stack du kör.

Kör följande PowerShell-skript för att installera dessa moduler på utvecklingsdatorn:

  - Azure Stack 1808 eller senare.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

> [!Note]
> Så här uppgraderar du Azure PowerShell från den **2017-03-09-profile** till **2018-03-01-hybrid**, finns det [Migreringsguide](https://github.com/bganapa/azure-powershell/blob/migration-guide/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

  - Azure Stack 1807 eller tidigare.

    ```PowerShell
    Install-Module -Name AzureRm.BootStrapper
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
    ```

  - Azure Stack 1804 eller tidigare.

    ```PowerShell
    Install-Module -Name AzureRm.BootStrapper
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.2.11
    ```

Bekräfta installationen genom att köra följande kommando:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Om installationen lyckas visas AzureRM- och AzureStack-moduler i utdata.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Frånkopplad: Installera PowerShell utan Internetanslutning

I ett scenario med frånkopplade måste du först hämta PowerShell-moduler till en dator som är ansluten till Internet och överför dem till Azure Stack Development Kit för installation.

Logga in på en dator med Internetanslutning och Använd följande skript för att hämta Azure Resource Manager och AzureStack paket, beroende på din version av Azure Stack:

  - Azure Stack 1808 eller senare.

    ````PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ````

  - Azure Stack 1807 eller tidigare.

    > [!Note]
    Uppgradera från 1.2.11 version, finns i den [Migreringsguide](https://aka.ms/azspowershellmigration).

    ````PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - Azure Stack 1804 eller tidigare.

    ````PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.3.0
    ````

2. Kopiera de hämta paketen till en USB-enhet.

3. Logga in på arbetsstationen och kopiera paket från USB-enheten till en plats på arbetsstationen.

4. Nu registrera den här platsen som standard-databasen och installerar AzureRM- och AzureStack-moduler från den här lagringsplatsen:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. Konfigurera PowerShell för att använda en proxyserver

I scenarier som kräver en proxyserver för att få åtkomst till Internet, måste du först konfigurera PowerShell för att använda en befintlig proxyserver:

1. Öppna en upphöjd PowerShell-prompt.
2. Kör följande kommandon:

   ```PowerShell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Nästa steg

 - [Ladda ned Azure Stack-verktyg från GitHub](azure-stack-powershell-download.md)
 - [Konfigurera PowerShell-miljö för Azure Stack-användare](user/azure-stack-powershell-configure-user.md)
 - [Konfigurera Azure Stack-operatör PowerShell-miljö](azure-stack-powershell-configure-admin.md)
 - [Hantera API-versionsprofiler i Azure Stack](user/azure-stack-version-profiles.md)
