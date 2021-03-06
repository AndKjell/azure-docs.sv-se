---
title: Tillåtna certifikatutfärdare för att aktivera anpassad HTTPS på Azure ytterdörren Service | Microsoft Docs
description: Om du använder ett eget certifikat för att aktivera HTTPS på en anpassad domän måste du använda en tillåtna certifikatutfärdare (CA) för att skapa den.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: de709133099674a0aa0386113b6459f8bc05e378
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047869"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Tillåtna certifikatutfärdare för att aktivera anpassad HTTPS på Azure ytterdörren Service

För en anpassad Azure ytterdörren Service-domän när du [aktivera HTTPS-funktionen med hjälp av ditt eget certifikat](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), måste du använda en tillåtna certifikatutfärdare (CA) för att skapa ett SSL-certifikat. Annars, om du använder en Certifikatutfärdare som inte är tillåtna eller ett självsignerat certifikat kan din begäran kommer att avvisas.

Följande CA: er tillåts när du skapar dina egna certifikat:

- AddTrust externa Certifikatutfärdare rot
- Asien och Stillahavsområdet rot-CA
- Asien och Stillahavsområdet rotcertifikatutfärdare 2013
- Asien och Stillahavsområdet rotcertifikatutfärdare 2014
- APCA DM3P
- Autopilot-Rotcertifikatutfärdare
- Baltimore CyberTrust Root
- Klass 3 primära certifikatutfärdare
- COMODO RSA-certifikatutfärdare
- COMODO RSA verifiering säker domänserver CA
- D-TRUST klass 3 Rotcertifikatutfärdare 2 2009
- DigiCert molntjänster CA-1
- DigiCert globala Rotcertifikatutfärdare
- DigiCert hög Assurance CA-3
- DigiCert hög Assurance EV rot-CA
- DigiCert SHA2 höga säkerhet Server CA
- DigiCert SHA2 säker CA-Server
- GeoTrust globala CA
- GeoTrust primära certifikatutfärdare
- GeoTrust primära certifikatutfärdaren - G2
- GlobalSign
- Utökad validering CA - SHA256 - G2 GlobalSign
- GlobalSign organisation verifiering CA - G2
- GlobalSign rot-CA
- Go Daddy rotcertifikatutfärdare - G2
- Microsoft Authenticode rotcertifikatutfärdare
- Microsoft Exchange-tjänster CA 2015
- Microsoft internt företagets rotcertifikatutfärdare
- Microsoft IT ITO SSL CA 1
- Microsoft IT SSL SHA1
- Microsoft IT SSL SHA2
- Microsoft IT TLS CA 1
- Microsoft IT TLS CA 2
- Microsoft IT TLS CA 4
- Microsoft IT TLS CA 5
- Microsoft rotcertifikatutfärdare
- Microsoft rotcertifikatutfärdare
- Microsoft rotcertifikatutfärdare 2010
- Microsoft rotcertifikatutfärdare 2011
- Microsoft säker Server CA 2011
- Microsoft Services Partner rot
- Microsoft tid stämpling Service rot
- Microsoft Windows maskinvarukompatibilitet
- MSIT CA Z2
- MSIT Enterprise CA 1
- MSIT Företagscertifikatutfärdare 3
- Rot myndighet
- Symantec-klass 3 EV SSL CA - G3
- Symantec klass 3 säker Server CA - G4
- Symantec Enterprise Mobilrot för Microsoft
- Thawte primära rot-CA
- Thawte primära rot-CA - G2
- Thawte primära rot-CA - G3
- Thawte tidsstämplar CA
- UTN-USERFirst-Object
- VeriSign Class 3 utökad validering SSL CA
- VeriSign Class 3 utökad validering SSL SGC CA
- VeriSign klass 3 primära certifikatutfärdare - G5
- VeriSign International Server CA - klass 3
- VeriSign tid stämpling Service rot
- VeriSign Universal rotcertifikatutfärdare
- WMSvc-SHA2-DALEDGE1008
