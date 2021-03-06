---
title: ta med fil
description: ta med fil
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 97e032af71947340c7e3b0af3b9d0701c972144e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843572"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testa frågor till Microsoft Graph-API från din iOS-program

Om du vill köra koden i simulatorn trycker du på **kommandot** + **R**.

![Testa ditt program i simulatorn](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

När du är redo att testa väljer **anropa Microsoft Graph API**. När du uppmanas, anger du ditt användarnamn och lösenord.

### <a name="provide-consent-for-application-access"></a>Ge medgivande för programåtkomst
Första gången du loggar in på ditt program, uppmanas du att ge ditt medgivande för att ge programmet åtkomst till din profil och logga in dig på:

![Ge ditt medgivande för programåtkomst](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Visa program resultat
När du har loggat in visas din användarprofilsinformation som returneras av Microsoft Graph API-anrop i den **loggning** avsnittet. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mer information om scope och delegerade behörigheter

Microsoft Graph API kräver den **user.read** omfattning att läsa en användares profil. Det här omfånget läggs automatiskt som standard i alla program som är registrerat på portalen för registrering. Andra API: er för Microsoft Graph, samt anpassade API: er för backend-servern, kan kräva ytterligare scope. Microsoft Graph API kräver den **Calendars.Read** omfattning att lista användarens kalendrar.

Om du vill få åtkomst till användarkalendrar i kontexten för ett program måste du lägga till den **Calendars.Read** delegerad behörighet att registreringsinformation för programmet. Lägg sedan till den **Calendars.Read** begränsa omfånget till den **acquireTokenSilent** anropa. 

>[!NOTE]
>Användaren uppmanas för ytterligare medgivanden när du ökar antalet omfång.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
