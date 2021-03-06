---
title: Konfigurera säkerhetsaviseringar för Azure AD-katalogroller i PIM | Microsoft Docs
description: Lär dig hur du konfigurerar säkerhetsaviseringar för Azure AD-katalogroller i Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4e1cb47989011f179c54061bd29ae55b4ff86d80
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669419"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Konfigurera säkerhetsaviseringar för Azure AD-katalogroller i PIM
## <a name="security-alerts"></a>Säkerhetsaviseringar
Azure Privileged Identity Management (PIM) genererar aviseringar när det finns misstänkt eller osäkra aktivitet i din miljö. När en avisering utlöses, visas det i PIM-instrumentpanelen. Välj vilken avisering du vill se en rapport som visar den användare eller de roller som utlöste aviseringen.

![PIM instrumentpanelen säkerhetsaviseringar – skärmbild](./media/pim-how-to-configure-security-alerts/PIM_security_dash.png)

| Varning | Severity | Utlösare | Rekommendation |
| --- | --- | --- | --- |
| **Roller tilldelas utanför PIM** |Hög |En användare har permanent tilldelade en privilegierad roll utanför PIM-gränssnittet. |Granska användare i listan och tilldela dem från Privilegierade roller som tilldelats utanför PIM. |
| **Roller som aktiveras för ofta** |Medel |Det fanns för många omaktiveringar av samma roll inom den tiden som tillåts i inställningarna. |Kontakta användaren för att se varför de har aktiverat rollen så många gånger. Tidsgränsen är kanske är för kort för dem att slutföra sina uppgifter eller kanske de använder skript att automatiskt aktivera en roll. Se till att aktiveringsvaraktighet för sin roll anges tillräckligt länge för att utföra sina uppgifter. |
| **Roller som inte kräver autentisering på flera plan för aktivering** |Medel |Det finns roller utan MFA är aktiverat i inställningarna. |Vi kräver MFA för de mest mycket Privilegierade rollerna, men att starkt du aktiverar MFA för aktivering av alla roller. |
| **Användarna använder inte sina Privilegierade roller** |Låg |Det finns berättigade administratörer som inte har aktiverat deras roller nyligen. |Starta en åtkomstgranskning för att fastställa de användare som inte längre behöver åtkomst. |
| **Det finns för många globala administratörer** |Låg |Det finns fler globala administratörer än vad som rekommenderas. |Om du har ett stort antal globala administratörer är det troligt att användarna får fler behörigheter än vad de behöver. Flytta användare till mindre privilegierade roller eller se några av dem berättigad till rollen i stället för permanent tilldelade. |

### <a name="severity"></a>Severity
* **Hög**: kräver omedelbara åtgärder på grund av en Policyöverträdelse. 
* **Medel**: inte kräver omedelbar åtgärd men signalerar potentiella Policyöverträdelse.
* **Låg**: inte kräver omedelbar åtgärd men en ändring av en preferrable.

## <a name="configure-security-alert-settings"></a>Konfigurera säkerhetsaviseringsinställningar
Du kan anpassa vissa av säkerhetsvarningar i PIM att arbeta med din miljö och säkerhetsmål. Följ dessa steg för att nå inställningsbladet:

1. Logga in på den [Azure-portalen](https://portal.azure.com/) och välj den **Azure AD Privileged Identity Management** panelen på instrumentpanelen.
2. Välj **hanteras Privilegierade roller** > **inställningar** > **aviseringsinställningar**.
   
    ![Navigera till säkerhetsinställningar för aviseringar](./media/pim-how-to-configure-security-alerts/PIM_security_settings.png)

### <a name="roles-are-being-activated-too-frequently-alert"></a>”Roller som aktiveras för ofta” avisering
Den här aviseringen utlöses om en användare aktiverar rollen för samma Privilegierade flera gånger inom en angiven tidsperiod. Du kan konfigurera både hur lång tid och antalet aktiveringar.

* **Aktiveringsförnyelse**: Ange i dagar, timmar, minuter och andra hur lång tid som du vill använda för att spåra misstänkta förnyelser.
* **Antal förnyelser aktivering**: Ange antalet aktiveringar från 2 till 100, som du anser vara värt aviseringens under den tidsperiod som du har valt. Du kan ändra den här inställningen genom att flytta skjutreglaget eller ange ett värde i textrutan.

### <a name="there-are-too-many-global-administrators-alert"></a>”Det finns för många globala administratörer” avisering
PIM utlöser aviseringen om två olika villkor är uppfyllda och du kan konfigurera båda. Du måste först når ett visst tröskelvärde för globala administratörer. En viss procentandel av din totala rolltilldelningar måste dessutom vara globala administratörer. Om du bara uppfyller något av de här mätningarna visas aviseringen inte.  

* **Minsta antal globala administratörer**: Ange antalet globala administratörer från 2 till 100, att du överväger belopp som osäkra.
* **Procentandel globala administratörer**: Ange vilken procentandel av administratörer som är globala administratörer, från 0% till 100%, som är inte säkert i din miljö.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>”Administratörer inte använder sina Privilegierade roller” avisering
Den här aviseringen utlöses om en användare går en viss tidsperiod utan att aktivera en roll.

* **Antal dagar**: Ange antalet dagar från 0 till 100, som en användare kan gå utan att aktivera en roll.

## <a name="next-steps"></a>Nästa steg

- [Konfigurera Azure AD directory rollinställningar i PIM](pim-how-to-change-default-settings.md)
