---
title: Ändra signaturens hashalgoritm för förtroende för förlitande part i Office 365 | Microsoft Docs
description: Den här sidan innehåller riktlinjer för att ändra SHA-algoritmen för federationsförtroende med Office 365
keywords: SHA1, SHA256, O365, federation, aadconnect, adfs, ADFS, ändra sha federationsförtroende förtroende för förlitande part
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 5352c76d2f41adef8fae29d3f60204fb05546ddc
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46298055"
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Ändra signaturens hashalgoritm för Office 365 förtroende för förlitande part
## <a name="overview"></a>Översikt
Active Directory Federation Services (AD FS) loggar dess token till Microsoft Azure Active Directory så att de inte kan brytas. Den här signaturen kan baseras på SHA1 eller SHA256. Azure Active Directory stöder nu token som signerats med ett SHA256-algoritmen och vi rekommenderar att ställa in algoritmen för tokensignering SHA256 för den högsta säkerhetsnivån. Den här artikeln beskriver de steg som behövs för att ange algoritmen för tokensignering att säkrare SHA256-nivå.

>[!NOTE]
>Microsoft rekommenderar användningen av SHA256 som algoritmen för att signera token som det är säkrare än SHA1 men SHA1 är fortfarande ett alternativ som stöds.

## <a name="change-the-token-signing-algorithm"></a>Ändra algoritmen för tokensignering
När du har angett signaturalgoritmen med någon av de två processerna nedan, registrerar AD FS tokens för Office 365 förtroende för förlitande part med SHA256. Du behöver inte göra några extra konfigurationsändringar och den här ändringen påverkar inte din möjlighet att komma åt Office 365 eller andra Azure AD-program.

### <a name="ad-fs-management-console"></a>AD FS-hanteringskonsolen
1. Öppna hanteringskonsolen för AD FS på den primära AD FS-servern.
2. Expandera noden AD FS och klicka på **förlitande Partsförtroenden**.
3. Högerklicka på ditt Office 365-/ Azure förlitande part och välj **egenskaper**.
4. Välj den **Avancerat** fliken och markera den säkra hash-algoritmen SHA256.
5. Klicka på **OK**.

![SHA256 Signeringsalgoritm--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS-PowerShell-cmdletar
1. Öppna PowerShell under administratörsbehörighet på alla AD FS-servern.
2. Ange den säkra hash-algoritmen med hjälp av den **ange AdfsRelyingPartyTrust** cmdlet.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Läs även
* [Reparera förtroende för Office 365 med Azure AD Connect](hybrid/how-to-connect-fed-management.md#repairthetrust)

