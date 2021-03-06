---
title: Åtgärda problem med certifikat för Azure Stack | Microsoft Docs
description: Använda Azure Stack-beredskap layout för att granska och åtgärda problem med certifikat.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 5e96c731496d79ca081091e2059a35545f963bd6
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078650"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Åtgärda vanliga problem med Azure Stack PKI-certifikat
Informationen i den här artikeln kan hjälpa dig att förstå och lösa vanliga problem med Azure Stack PKI-certifikat. Du kan identifiera problem när du använder Azure Stack-beredskap för installation för att [Validera Azure Stack PKI-certifikat](azure-stack-validate-pki-certs.md). Verktyget kontrollerar för att säkerställa att certifikat uppfyller kraven för PKI för ett Azure Stack-distributioner och Azure Stack hemlighet Rotation och loggas resultaten i en [report.json filen](azure-stack-validation-report.md).  

## <a name="pfx-encryption"></a>PFX-kryptering
**Fel** -PFX-kryptering är inte TripleDES SHA1.   
**Reparation** -exportera PFX-filer med **TripleDES SHA1** kryptering. Det här är standard för alla Windows 10-klienter när du exporterar från certifikatet Fäst i eller med hjälp av Export-PFXCertificate. 

## <a name="read-pfx"></a>Läsa PFX
**Varning** -lösenord skyddar bara privat information i certifikatet.  
**Reparation** -vi rekommenderar att du exportera PFX-filer med den valfria inställningen för **aktivera certifikatet sekretess**.  

**Fel** -ogiltig för PFX-filen.  
**Reparation** – exportera certifikatet med hjälp av stegen i [förbereda Azure Stack-PKI-certifikat för distribution av](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>Signaturalgoritm
**Fel** -signaturalgoritmen är SHA1.    
**Reparation** – Följ stegen i Azure Stack certifikat för signering begäran genererades för att återskapa den certifikatsignering begäran (CSR) med signatur algoritmen för SHA256. Skicka sedan CSR till certifikatutfärdaren att utfärda det på nytt.

## <a name="private-key"></a>Privat nyckel
**Fel** -den privata nyckeln saknas eller innehåller inte den lokala datorn-attribut.  
**Reparation** – från den dator som genererats CSR, exportera certifikatet med hjälp av stegen i förbereda Azure Stack-PKI-certifikat för distribution. De här stegen omfattar export från lokala datorns certifikatarkiv.

## <a name="certificate-chain"></a>Certifikatkedjan
**Fel** -certifikatkedjan har inte slutförts.  
**Reparation** -certifikat ska innehålla en fullständig kedja.  Exportera certifikatet med hjälp av stegen i [förbereda Azure Stack-PKI-certifikat för distribution av](azure-stack-prepare-pki-certs.md) och välja alternativet **inkludera om möjligt alla certifikat i certifieringssökvägen.**

## <a name="dns-names"></a>DNS-namn
**Fel** -DNSNameList på certifikatet inte innehåller namnet på Azure Stack slutpunkten eller en giltig jokertecken-matchning.  Matchar jokertecken är endast giltiga för vänster namnområdet DNS-namn. Till exempel _*. region.domain.com_ är bara giltig för *portal.region.domain.com*, inte _*. table.region.domain.com_.  
**Reparation** -använda stegen i Azure Stack certifikat för signering begäran genererades för att återskapa CSR med rätt DNS-namn för att stödja Azure Stack-slutpunkter. Skicka CSR till en certifikatutfärdare och följ sedan stegen i [förbereda Azure Stack-PKI-certifikat för distribution](azure-stack-prepare-pki-certs.md) att exportera certifikatet från den dator som genereras i CSR.  

## <a name="key-usage"></a>Nyckelanvändning
**Fel** – nyckelanvändning saknar Digital signatur eller nyckelchiffrering, orEnhanced nyckelanvändning saknar Server-autentisering eller klientautentisering.  
**Reparation** – Följ stegen i [Azure Stack certifikat signering begäran generation](azure-stack-get-pki-certs.md) att återskapa CSR med rätt nyckelanvändning attribut.  Skicka CSR till certifikatutfärdaren och bekräfta att en certifikatmall inte skriver över nyckelanvändning i begäran.

## <a name="key-size"></a>Nyckelstorlek
**Fel** -nyckel är mindre än 2048    
**Reparation** – Följ stegen i [Azure Stack certifikat signering begäran generation](azure-stack-get-pki-certs.md) att återskapa CSR med rätt nyckellängd (2048) och sedan skicka CSR till certifikatutfärdaren.

## <a name="chain-order"></a>Kedjans ordning
**Fel** -ordningen i certifikatkedjan är felaktig.  
**Reparation** – exportera certifikatet med hjälp av stegen i [förbereda Azure Stack-PKI-certifikat för distribution av](azure-stack-prepare-pki-certs.md) och välja alternativet **inkludera om möjligt alla certifikat i certifieringssökvägen.** Se till att endast lövcertifikatet har valts för export. 

## <a name="other-certificates"></a>Andra certifikat
**Fel** -den PFX-paketet innehåller certifikat som inte är lövcertifikatet eller en del av certifikatkedjan.  
**Reparation** – exportera certifikatet med hjälp av stegen i [förbereda Azure Stack-PKI-certifikat för distribution av](azure-stack-prepare-pki-certs.md), och väljer alternativet **inkludera om möjligt alla certifikat i certifieringssökvägen.** Se till att endast lövcertifikatet har valts för export.

## <a name="fix-common-packaging-issues"></a>Åtgärda vanliga problem med paketering
AzsReadinessChecker kan importera och exportera en PFX-fil för att åtgärda vanliga problem i paketering, inklusive: 
 - *PFX-kryptering* är inte TripleDES-SHA1
 - *Privat nyckel* saknar lokal dator-attribut.
 - *Certifikatkedjan* är ofullständig eller fel. (Den lokala datorn måste innehålla certifikatkedjan om PFX-paketet inte.) 
 - *Andra certifikat*.
Men AzsReadinessChecker inte hjälpa dig att om du vill skapa en ny begäran om Certifikatsignering och ange ett certifikat. 

### <a name="prerequisites"></a>Förutsättningar
Följande krav måste vara uppfyllda på datorn där verktyget körs: 
 - Windows 10 eller Windows Server 2016, internet-anslutning.
 - PowerShell 5.1 eller senare. Om du vill kontrollera vilken version du kör följande PowerShell-cmd och granska de *större* version och *mindre* versioner.

   > `$PSVersionTable.PSVersion`
 - Konfigurera [PowerShell för Azure Stack](azure-stack-powershell-install.md). 
 - Ladda ned den senaste versionen av [Microsoft Azure Stack-beredskap Checker](https://aka.ms/AzsReadinessChecker) verktyget.

### <a name="import-and-export-an-existing-pfx-file"></a>Importera och exportera en befintlig PFX-fil
1. På en dator som uppfyller kraven och öppna en administrativ PowerShell-kommandotolk och kör sedan följande kommando för att installera AzsReadinessChecker  
   > `Install-Module Microsoft.AzureStack.ReadinessChecker- Force`

2. Från PowerShell-Kommandotolken kör du följande för att ange PFX-lösenordet. Ersätt *PFXpassword* med det faktiska lösenordet. 
   > `$password = Read-Host -Prompt PFXpassword -AsSecureString`

3. Från PowerShell-Kommandotolken kör du följande för att exportera en ny PFX-fil.
   - För *- PfxPath*, ange sökvägen till PFX-filen som du arbetar med.  I följande exempel är sökvägen *.\certificates\ssl.pfx*.
   - För *- ExportPFXPath*, ange platsen och namnet på PFX-filen för export.  I följande exempel är sökvägen *.\certificates\ssl_new.pfx*

   > `Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx`  

4. När verktyget har slutförts kan du granska utdata för en lyckad: ![resultat](./media/azure-stack-remediate-certs/remediate-results.png)

## <a name="next-steps"></a>Nästa steg
[Läs mer om Azure Stack-säkerhet](azure-stack-rotate-secrets.md)
