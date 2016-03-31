<properties
    pageTitle="Was ist mit dem MVC-Projekt passiert (verbundene Visual Studio-Dienste für Azure Active Directory)? | Microsoft Azure "
    description="Beschreibt, was mit dem MVC-Projekt geschieht, wenn Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit Azure AD herstellen "
    services="active-directory"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="tglee"/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/03/2015"
    ms.author="tarcher"/>

# Was ist mit dem MVC-Projekt passiert (verbundene Visual Studio-Dienste für Azure Active Directory)?

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-dotnet-getting-started.md)
> - [Was ist passiert?](vs-active-directory-dotnet-what-happened.md)



## Verweise wurden hinzugefügt

### NuGet-Paketverweise

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### .NET-Verweise

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## Code wurde hinzugefügt

### Ihrem Projekt wurden Codedateien hinzugefügt

Eine authentifizierungsstartklasse **App_Start/Startup.Auth.cs** wurde hinzugefügt, um das Projekt enthält Startlogik für Azure AD-Authentifizierung. Außerdem wurde eine Controllerklasse AccountController.cs enthält hinzugefügt **SignIn()"** und **SignOut()** Methoden. Schließlich eine Teilansicht **Views/Shared/_LoginPartial.cshtml** wurde hinzugefügt, die einen Aktionslink für SignIn/SignOut enthält.

### Ihrem Projekt wurde Startcode hinzugefügt

Wenn Sie bereits eine Startklasse in Ihrem Projekt die **Konfiguration** Methode wurde aktualisiert, um einen Aufruf von **verwendet**. Andernfalls wurde Ihrem Projekt eine Startklasse hinzugefügt.

### Ihre Datei "app.config" oder "web.config" weist neue Konfigurationswerte auf

Die folgenden Konfigurationseinträge wurden hinzugefügt.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### Eine Azure Active Directory-App (AD) wurde erstellt
Eine Azure AD-Anwendung wurde in dem Verzeichnis erstellt, das Sie im Assistenten ausgewählt haben.

##Wenn ich habe *Deaktivieren der Authentifizierung einzelner Benutzerkonten*, welche zusätzlichen Änderungen wurden an meinem Projekt vorgenommen?
NuGet-Paketverweise wurden entfernt, und die Dateien wurden entfernt und gesichert. Abhängig vom Status des Projekts müssen Sie möglicherweise manuell zusätzliche Verweise oder Dateien entfernen oder Code entsprechend ändern.

### NuGet-Paketverweise entfernt (die vorhanden waren)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### Codedateien gesichert und entfernt (die vorhanden waren)

Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stammverzeichnis des Projektverzeichnisses.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### Codedateien gesichert (die vorhanden waren)

Jede der folgenden Dateien wurde gesichert, bevor sie ersetzt wurde. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stammverzeichnis des Projektverzeichnisses.

- **Startup.cs**
- **App_Start\Startup.Auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## Wenn ich habe *Lesen der Verzeichnisdaten*, welche zusätzlichen Änderungen wurden an meinem Projekt vorgenommen?

Zusätzliche Verweise wurden hinzugefügt.

###Zusätzliche NuGet-Paketverweise

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###Zusätzliche .NET-Verweise

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###Ihrem Projekt wurden zusätzliche Codedateien hinzugefügt

Zur Unterstützung der tokenzwischenspeicherung wurden zwei Dateien hinzugefügt: **Models\ADALTokenCache.cs** und **Models\ApplicationDbContext.cs**.  Ein zusätzlicher Controller und eine Ansicht wurden hinzugefügt, um den Zugriff auf Benutzerprofilinformationen mithilfe von Azure Graph-APIs zu veranschaulichen.  Diese Dateien sind **Controllers\UserProfileController.cs** und **Views\UserProfile\Index.cshtml**.

###Ihrem Projekt wurde zusätzlicher Startcode hinzugefügt

In der **startup.auth.cs** eine neue Datei **OpenIdConnectAuthenticationNotifications** hinzugefügt wurde, die **Benachrichtigungen** Mitglied der **OpenIdConnectAuthenticationOptions**.  Auf diese Weise wird der Empfang des OAuth-Codes und dessen Austausch gegen ein Zugriffstoken aktiviert.

###An "app.config" oder "web.config" wurden zusätzliche Änderungen vorgenommen

Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Die folgenden Konfigurationsabschnitte und eine Verbindungszeichenfolge wurden hinzugefügt.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###Ihre Azure Active Directory-App (AD) wurde aktualisiert
Ihre Azure Active Directory-App wurde aktualisiert, um enthalten die *Lesen der Verzeichnisdaten* Berechtigung und ein zusätzlicher Schlüssel wurde erstellt, die dann als verwendet wurde die *Ida: ClientSecret* in der **web.config** Datei.

[Weitere Informationen zu Azure Active Directory](http://azure.microsoft.com/services/active-directory/)


