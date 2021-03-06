---
title: Uppdateringsinfrastruktur för Red Hat | Microsoft Docs
description: Läs mer om Red Hat-Uppdateringsinfrastruktur för Red Hat Enterprise Linux på begäran-instanser i Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: borisb
ms.openlocfilehash: 4a8bc45b253def1130e5a02dfcd6d359f0e74506
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/21/2018
ms.locfileid: "42060357"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Uppdateringsinfrastruktur för Red Hat för på begäran Red Hat Enterprise Linux-datorer i Azure
 [Uppdateringsinfrastruktur för Red Hat](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) gör att cloud-leverantörer, till exempel Azure för spegling av Red Hat-värdbaserade databasinnehåll, skapa anpassade databaser med Azure-specifika innehåll och gör den tillgänglig för slutanvändaren virtuella datorer.

Red Hat Enterprise Linux (RHEL) betala per användning (PAYG) avbildningar kommer förkonfigurerade för att komma åt Azure RHUI. Ingen ytterligare konfiguration krävs. För att få de senaste uppdateringarna, köra `sudo yum update` när din RHEL-instans är klar. Den här tjänsten ingår som en del av RHEL PAYG-programvaruavgifter.

## <a name="important-information-about-azure-rhui"></a>Viktig information om Azure RHUI
* Azure RHUI stöder för närvarande endast den senaste mindre versionen i varje RHEL-serien (RHEL6 eller RHEL7). Om du vill uppgradera en RHEL VM-instans som är anslutna till RHUI till den senaste minor-versionen, kör `sudo yum update`.

    Exempel: Om du vill etablera en virtuell dator från en RHEL 7.2 PAYG-avbildning och kör `sudo yum update`, att avslutas med en RHEL 7.5 virtuell dator (de senaste mindre versionen i RHEL7-serien).

    För att undvika det här beteendet kan du behöva skapa en egen avbildning enligt beskrivningen i den [skapa och ladda upp en Red Hat-baserad virtuell dator för Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikeln. Måste du ansluta den till en annan uppdatering-infrastruktur ([direkt till Red Hat innehåll servrar](https://access.redhat.com/solutions/253273) eller en [Red Hat satellit server](https://access.redhat.com/products/red-hat-satellite)).

* Åtkomst till Azure som värd-RHUI ingår i priset för RHEL PAYG-avbildningen. Om du avregistrerar en PAYG RHEL virtuell dator från Azure som värd-RHUI konverterar som den virtuella datorn inte till en bring-your-own-license (BYOL) typ av virtuell dator. Om du har registrerat samma virtuella dator med en annan källa för uppdateringar kan roamingavgifter _indirekt_ dubbelklicka avgifter. Du debiteras för första gången för avgiften för Azure RHEL-programvara. Du debiteras den andra gången för Red Hat-prenumerationer som köpts tidigare. Om du behöver konsekvent att använda en uppdateringsinfrastruktur än Azure-värdbaserade RHUI kan du skapa och distribuera dina egna avbildningar (BYOL-typ). Den här processen beskrivs i [skapa och ladda upp en Red Hat-baserad virtuell dator för Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Två klasser av RHEL PAYG-avbildningar i Azure (RHEL for SAP HANA) och RHEL for SAP Business Applications är anslutna till RHUI kanaler som finns kvar på den specifika RHEL lägre versionen som krävs för SAP-certifiering. 

* Åtkomst till Azure-värdbaserade RHUI är begränsad till de virtuella datorerna inom de [Azure datacenter IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653). Om du är proxy är alla VM-trafik via en lokal nätverksinfrastruktur, du kan behöva konfigurera användardefinierade vägar för RHEL PAYG virtuella datorer till Azure-RHUI.

### <a name="the-ips-for-the-rhui-content-delivery-servers"></a>IP-adresser för servrarna som RHUI

RHUI är tillgänglig i alla regioner där RHEL-avbildningar på begäran är tillgängliga. Omfattar för närvarande alla offentliga regioner som visas på den [Azure statusinstrumentpanelen](https://azure.microsoft.com/status/) sida, Azure US Government och Microsoft Azure Tyskland-regioner. 

Om du använder en nätverkskonfiguration för att ytterligare begränsa åtkomst från RHEL PAYG virtuella datorer, se till att följande IP-adresser tillåts för `yum update` ska fungera beroende på den miljö som du använder: 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="rhui-azure-infrastructure-update"></a>Uppdatering av RHUI-Azure-infrastrukturen

I September 2016 kommer distribuerade vi en uppdaterade Azure RHUI. I April 2017 stänga vi av den gamla Azure-RHUI. Om du har använt RHEL PAYG-bilder (eller deras ögonblicksbilder) från September 2016 eller senare måste ansluter du automatiskt till den nya Azure-RHUI. Om du har dock äldre ögonblicksbilder på dina virtuella datorer kan behöva du manuellt uppdatera konfigurationen för att komma åt Azure-RHUI enligt beskrivningen i följande avsnitt.

De nya Azure RHUI-servrarna distribueras med [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). I Traffic Manager, en enda slutpunkt (rhui-1.microsoft.com) kan användas av alla virtuella datorer, oavsett region. 

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Felsök anslutningsproblem till Azure RHUI
Om du får problem med att ansluta till Azure RHUI från Azure RHEL PAYG-VM, gör du följande:

1. Kontrollera konfigurationen för virtuell dator för Azure RHUI-slutpunkten:

    a. Kontrollera om den `/etc/yum.repos.d/rh-cloud.repo` filen innehåller en referens till `rhui-[1-3].microsoft.com` i den `baseurl` av den `[rhui-microsoft-azure-rhel*]` i filen. I annat fall använder du den nya Azure-RHUI.

    b. Om den pekar på en plats med följande mönster `mirrorlist.*cds[1-4].cloudapp.net`, krävs en konfigurationsuppdatering. Du använder den gamla VM-ögonblicksbilden och du måste du uppdatera den så att den pekar till den nya Azure-RHUI.

1. Åtkomst till Azure-värdbaserade RHUI begränsas till virtuella datorer [Azure-datacenter IP-adressintervall] (https://www.microsoft.com/download/details.aspx?id=41653).
 
1. Om du använder den nya konfigurationen har verifierat att den virtuella datorn ansluter från Azure-IP-adressintervall och fortfarande inte kan ansluta till Azure RHUI, filen ett supportärende med Microsoft eller Red Hat.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Manuell uppdateringsproceduren för att använda Azure RHUI-servrar
Den här proceduren tillhandahålls endast för referens. RHEL PAYG-avbildningar har redan rätt konfiguration för att ansluta till Azure RHUI. För att manuellt uppdatera konfigurationen för att använda Azure RHUI-servrarna, gör du följande:

1. Hämta den offentliga nyckel signaturen via curl.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
   ```

1. Verifiera att den nedladdade nyckeln.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

1. Kontrollera utdata och kontrollera den `keyid` och `user ID packet`.

   ```bash
   Version: GnuPG v1.4.7 (GNU/Linux)
   :public key packet:
           version 4, algo 1, created 1446074508, expires 0
           pkey[0]: [2048 bits]
           pkey[1]: [17 bits]
           keyid: EB3E94ADBE1229CF
   :user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
   :signature packet: algo 1, keyid EB3E94ADBE1229CF
           version 4, created 1446074508, md5len 0, sigclass 0x13
           digest algo 2, begin of digest 1a 9b
           hashed subpkt 2 len 4 (sig created 2015-10-28)
           hashed subpkt 27 len 1 (key flags: 03)
           hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
           hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
           hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
           hashed subpkt 30 len 1 (features: 01)
           hashed subpkt 23 len 1 (key server preferences: 80)
           subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
           data: [2047 bits]
   ```

1. Installera den offentliga nyckeln.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

1. Hämta, kontrollera och installera en klient RPM Package Manager (RPM).
    
    >[!NOTE]
    >Paketversioner ändra. Om du ansluter manuellt till Azure RHUI kan hitta du den senaste versionen av klientpaketet för respektive RHEL-familj genom att etablera den senaste avbildningen från galleriet.
  
   a. Ladda ned. 
   
    - För RHEL 6:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.1-32.noarch.rpm 
        ```
    
    - För RHEL 7:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.1-19.noarch.rpm  
        ```

   b. Kontrollera.

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Kontrollera utdata för att säkerställa att signaturen för paketet är OK.

   ```bash
   azureclient.rpm:
       Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
       Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
       V3 RSA/SHA256 Signature, key ID be1229cf: OK
       MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
   ```

   d. Installera RPM.

    ```bash
    sudo rpm -U azureclient.rpm
    ```

1. När du är klar kan du kontrollera att du kan komma åt Azure RHUI från den virtuella datorn.

## <a name="next-steps"></a>Nästa steg
Skapar en Red Hat Enterprise Linux virtuell dator från en Azure Marketplace PAYG-avbildning och kan använda Azure-värdbaserade RHUI går du till den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). 
