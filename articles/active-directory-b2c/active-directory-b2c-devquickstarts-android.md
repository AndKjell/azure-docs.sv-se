---
title: Skaffa en token som använder en Android-App i Azure Active Directory B2C | Microsoft Docs
description: Den här artikeln visar hur du skapar en Android-app som använder AppAuth med Azure Active Directory B2C för att hantera användaridentiteter och autentiserar användare.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 716cf9e47cd71d003513066d390f9dccb5c83dcb
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344134"
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Logga in med ett Android-program

Microsofts identitetsplattform använder öppna standarder som OAuth2 och OpenID Connect. Dessa standarder kan du utnyttja alla bibliotek som du vill integrera med Azure Active Directory B2C. För att använda andra bibliotek ska använda du en genomgång som den här för att demonstrera hur du konfigurerar bibliotek med 3 part att ansluta till Microsoft identity-plattformen. De flesta bibliotek som implementerar [RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta till Microsoft Identity-plattformen.

> [!WARNING]
> Microsoft tillhandahåller inte korrigeringar för 3 part bibliotek och inte har gjort en genomgång av dessa bibliotek. Det här exemplet använder en 3 part library, även kallat AppAuth som har testats för kompatibilitet i grundläggande scenarier med Azure AD B2C. Problem och funktionsförfrågningar ska dirigeras till biblioteksprojekt öppen källkod. Se [i den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) för mer information.  
>
>

Om du inte har erfarenhet av OAuth2 eller OpenID Connect kanske du inte får ut så mycket av den här exempelkonfigurationen. Vi rekommenderar att du läser en kort [översikt över protokollet som vi har dokumenterat här](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Skaffa en Azure AD B2C-katalog

Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient. En katalog är en container för alla användare, appar, grupper och mer. Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.

## <a name="create-an-application"></a>Skapa ett program

Därefter måste du skapa en app i B2C-katalogen. Det ger Azure AD den information som tjänsten behöver för att kommunicera säkert med din app. Så här skapar du en mobil app [instruktionerna](active-directory-b2c-app-registration.md). Se till att:

* Inkludera en **Native Client** i programmet.
* Kopiera **program-ID:t** som har tilldelats din app. Du behöver det senare.
* Konfigurera en intern klient **omdirigerings-URI** (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Du behöver även detta senare.

## <a name="create-your-policies"></a>Skapa principer

I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Den här appen innehåller en identitetslösning: en kombinerad inloggning och registrering. Du behöver skapa den här principen, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Tänk på följande när du skapar principen:

* Välj den **visningsnamn** som ett registrerings attribut i din princip.
* Välj det **visningsnamn** och **objekt-ID** som programmet gör anspråk på i varje princip. Du kan också välja andra anspråk.
* Kopiera **namnet** på varje princip när du har skapat den. Det bör ha prefixet `b2c_1_`.  Du behöver principnamnet senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat dina principer är det dags att bygga appen.

## <a name="download-the-sample-code"></a>Hämta exempelkoden

Vi har lagt till ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Du kan ladda ned koden och kör den. Du kan snabbt komma igång med din egen app med din egen Azure AD B2C-konfiguration genom att följa anvisningarna i den [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

I exemplet är en ändring av exemplet som anges av [AppAuth](https://openid.github.io/AppAuth-Android/). Finns sidan för mer information om AppAuth och dess funktioner.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Ändra din app för att använda Azure AD B2C med AppAuth

> [!NOTE]
> AppAuth stöder Android API 16 (Jellybean) och senare. Vi rekommenderar att du använder API-23 och senare.
>

### <a name="configuration"></a>Konfiguration

Du kan konfigurera kommunikation med Azure AD B2C genom att ange URI för identifiering eller genom att ange både auktoriseringsslutpunkt och tokenslutpunkten URI: er. I båda fallen behöver du följande information:

* Klient-ID (t.ex. contoso.onmicrosoft.com)
* Principnamn (t.ex. B2C\_1\_SignUpIn)

Om du väljer att automatiskt identifiera auktorisering och tokenslutpunkten URI: er, behöver du för hämtning av information från identifieringen URI. Identifieringen URI kan genereras genom att ersätta klienten\_-ID och principen\_namn i följande URL:

```java
String mDiscoveryURI = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Du kan hämta auktorisering och tokenslutpunkten URI: er och skapa ett AuthorizationServiceConfiguration objekt genom att köra följande:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

Istället för att använda identifiering för att hämta auktorisering och tokenslutpunkten URI: er, du kan också ange dem uttryckligen genom att ersätta klienten\_-ID och principen\_namnet i URL: er nedan:

```java
String mAuthEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Kör följande kod för att skapa AuthorizationServiceConfiguration-objekt:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Auktoriserar

En begäran om godkännande kan skapas när du konfigurerar eller hämtar en tjänstkonfiguration för auktorisering. För att skapa begäran, behöver du följande information:

* Klient-ID (t.ex. 00000000-0000-0000-0000-000000000000)
* Omdirigerings-URI med ett anpassat schema (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Båda objekten ska har sparats när du var [registrera din app](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Finns det [AppAuth guide](https://openid.github.io/AppAuth-Android/) om hur du Slutför resten av processen. Om du behöver snabbt komma igång med en fungerande app kan du kolla [vårt exempel](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Följ stegen i den [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) att ange din egen Azure AD B2C-konfiguration.

Vi är alltid öppen för feedback och förslag! Om du har problem med den här artikeln eller har rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback längst ned på sidan. Lägga till dem i för funktionsförfrågningar om [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

