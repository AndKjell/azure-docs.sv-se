<properties 
    pageTitle="Weitere Informationen: Azure AD-Kennwortverwaltung | Microsoft Azure" 
    description="Weiterführende Themen zur Azure AD-Kennwortverwaltung, einschließlich Kennwortrückschreibung, Sicherheit bei der Kennwortrückschreibung, Funktionsweise des Portals für die Kennwortzurücksetzung und der zur Kennwortzurücksetzung verwendeten Daten." 
    services="active-directory" 
    documentationCenter="" 
    authors="asteen" 
    manager="kbrint" 
    editor="billmath"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="asteen"/>

# Weitere Informationen zur Kennwortverwaltung
Wenn Sie die Kennwortverwaltung bereits bereitgestellt haben oder vor der Bereitstellung einfach nur mehr über die technischen Grundlagen erfahren möchten, liefert dieser Abschnitt einen guten Überblick über die technischen Konzepte, die hinter dem Dienst stehen. In diesem Artikel werden die folgenden Themen abgedeckt:

* [**Übersicht über die Kennwortrückschreibung**](#password-writeback-overview)
  - [Funktionsweise der Kennwortrückschreibung](#how-password-writeback-works)
  - [Unterstützte Szenarien für die Kennwortrückschreibung](#scenarios-supported-for-password-writeback)
  - [Sicherheitsmodell für die Kennwortrückschreibung](#password-writeback-security-model)
* [**Wie funktioniert das Portal für die Kennwortzurücksetzung?**](#how-does-the-password-reset-portal-work)
  - [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](#what-data-is-used-by-password-reset)
  - [Zugriff auf Daten zur Kennwortzurücksetzung für Ihre Benutzer](#how-to-access-password-reset-data-for-your-users)

## Übersicht über die Kennwortrückschreibung
Das Rückschreiben von Kennwörtern ist ein [Azure Active Directory Connect](active-directory-aadconnect) Komponente, die aktiviert und von den aktuellen Abonnenten von Azure Active Directory Premium verwendet werden kann. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md). 

Mit der Kennwortrückschreibung können Sie Ihren Cloudmandanten so konfigurieren, dass Kennwörter in das lokale Active Directory zurückgeschrieben werden.  Auf diese Weise ist es nicht erforderlich, eine lokale Self-Service-Lösung für das Zurücksetzen von Kennwörtern einzurichten und zu verwalten. Gleichzeitig bietet diese Funktion Ihren Benutzern eine bequeme, cloudbasierte Möglichkeit zum Zurücksetzen ihrer lokalen Kennwörter – unabhängig davon, wo sie sich gerade befinden.  Nachfolgend werden einige der wichtigsten Merkmale der Kennwortrückschreibung aufgeführt:

- **Sofortiges Feedback.**  Die Kennwortrückschreibung ist ein synchroner Vorgang.  Ihre Benutzer werden sofort benachrichtigt, wenn ihre Kennwörter nicht der Richtlinie entsprechen oder das Zurücksetzen oder Ändern des Kennworts aus beliebigen Gründen nicht möglich war.
- **Unterstützung für das Zurücksetzen von Kennwörtern für Benutzer, die AD FS oder andere Verbundtechnologien nutzen.**  Solange die Verbundbenutzerkonten mit Ihrem Azure AD-Mandanten synchronisiert sind, können die Benutzer bei Verwendung der Kennwortrückschreibung ihre lokalen AD-Kennwörter über die Cloud verwalten.
- **Unterstützung für das Zurücksetzen von Kennwörtern für Benutzer, die eine Kennworthashsynchronisierung verwenden.** Wenn der Dienst für die Kennwortzurücksetzung ermittelt, dass ein synchronisiertes Benutzerkonto für die Kennworthashsynchronisierung aktiviert ist, werden das lokale Kennwort und das Cloudkennwort für dieses Konto simultan zurückgesetzt.
- **Unterstützung für das Ändern von Kennwörtern über den Zugriffsbereich und Office 365.**  Wenn Verbundbenutzer oder Benutzer mit Kennwortsynchronisierung ihre abgelaufenen oder noch nicht abgelaufenen Kennwörter ändern möchten, werden diese Kennwörter in die lokale AD-Umgebung zurückgeschrieben.
- **Unterstützt das Zurückschreiben von Kennwörtern bei einem Administrator zurückgesetzt werden aus den** [**Azure-Verwaltungsportal**](https://manage.windowsazure.com).  Jedes Mal, wenn ein Administrator das Kennwort eines Benutzers in setzt die [Azure-Verwaltungsportal](https://manage.windowsazure.com), wenn der Benutzer einen Verbund oder Kennwort synchronisiert wird, setzen wir das Kennwort, die der Administrator im lokalen AD auch wählt.  Diese Funktion wird im Office-Verwaltungsportal derzeit nicht unterstützt.
- **Erzwingung von lokalen AD-Kennwortrichtlinien.**  Wenn ein Benutzer sein Kennwort zurücksetzt, wird sichergestellt, dass es die lokalen AD-Richtlinien erfüllt, bevor es in das Verzeichnis geschrieben wird.  Dies schließt Verlauf, Komplexität, Alter, Kennwortfilter und alle weiteren Kennwortbeschränkungen ein, die Sie im lokalen AD definiert haben.
- **Nicht Firewallregeln alle eingehenden.**  Bei der Kennwortrückschreibung wird als zugrunde liegender Kommunikationskanal ein Azure Service Bus Relay verwendet. Dies bedeutet, dass Sie keine eingehenden Ports in Ihrer Firewall öffnen müssen, damit diese Funktion ordnungsgemäß arbeitet.
- **Keine Unterstützung für Benutzerkonten in geschützten Gruppen, die im lokalen Active Directory vorliegen.** Weitere Informationen zu geschützten Gruppen finden Sie unter [geschützte Konten und Gruppen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### Funktionsweise der Kennwortrückschreibung
Die Kennwortrückschreibung umfasst drei Hauptkomponenten:

- Einen Clouddienst für das Zurücksetzen von Kennwörtern (auch integriert in die Azure AD-Seiten für die Kennwortzurücksetzung)
- Ein mandantenspezifisches Azure Service Bus Relay
- Einen lokalen Endpunkt für die Kennwortzurücksetzung

Diese Komponenten arbeiten folgendermaßen zusammen:

  ![][001]

Wenn ein  Verbundbenutzer oder ein Benutzer mit Kennworthashsynchronisierung ihr Kennwort in der Cloud zurücksetzen oder ändern, geschieht Folgendes:

1.  Es wird überprüft, über welche Art von Kennwort der Benutzer verfügt.  Falls das Kennwort lokal verwaltet wird, wird zunächst sichergestellt, dass der Dienst für das Zurückschreiben aktiv ist und ausgeführt wird.  Ist der Dienst aktiv, kann der Benutzer fortfahren. Ist der Dienst nicht aktiv, wird der Benutzer darüber informiert, dass das Kennwort an dieser Stelle nicht zurückgesetzt werden kann.
2.  Im nächsten Schritt muss der Benutzer sich erfolgreich authentifizieren und gelangt dann zum Bildschirm für die Kennwortzurücksetzung.
3.  Der Benutzer wählt ein neues Kennwort aus und bestätigt es.
4.  Beim Klicken auf "Senden" wird das Klartextkennwort mit einem symmetrischen Schlüssel verschlüsselt, der beim Einrichten der Kennwortrückschreibung erstellt wurde.
5.  Nachdem das Kennwort verschlüsselt wurde, wird es in eine Nutzlast eingeschlossen, die über einen HTTPS-Kanal an das mandantenspezifische Service Bus Relay gesendet wird (dieses wurde ebenfalls beim Einrichten der Kennwortrückschreibung festgelegt).  Dieses Relay wird durch ein zufällig generiertes Kennwort geschützt, das nur der lokalen Installation bekannt ist.
6.  Sobald die Nachricht vom Service Bus empfangen wird, wird der Endpunkt für die Kennwortzurücksetzung automatisch aktiviert und erkennt, dass eine Anforderung zur Kennwortzurücksetzung aussteht. 
7.  Der Dienst sucht anschließend unter Verwendung des Cloudankerattributs nach dem betreffenden Benutzer.  Wenn diese Suche erfolgreich war, muss das Benutzerobjekt im AD-Connectorbereich vorliegen, dem entsprechenden MV-Objekt und dem entsprechenden AAD-Connectorobjekt zugeordnet sein. Und schließlich muss – damit dieses Benutzerkonto über das Synchronisierungsmodul gefunden wird – für die Verbindung zwischen AD-Connectorobjekt und MV die Synchronisierungsregel `Microsoft.InfromADUserAccountEnabled.xxx` gelten.    Dies ist erforderlich, weil der Aufruf von der Cloud eingeht, das Synchronisierungsmodul das cloudAnchor-Attribut verwendet, um das AAD-Connectorbereichsobjekt zu ermitteln, der Verbindung zum MV-Objekt und anschließend der Verbindung zurück zum AD-Objekt folgt.  Da mehrere AD-Objekte (mehrere Gesamtstrukturen) für denselben Benutzer vorliegen können, wählt das Synchronisierungsmodul das richtige Objekt basierend auf der `Microsoft.InfromADUserAccountEnabled.xxx`-Verbindung aus.
8.  Sobald das Benutzerkonto ermittelt wurde, wird versucht, das Kennwort direkt in der geeigneten AD-Gesamtstruktur zurückzusetzen.
9.  Wenn die Kennwortzurücksetzung erfolgreich war, wird der Benutzer darüber informiert, dass das Kennwort geändert wurde, und der Vorgang ist damit abgeschlossen.
10. Wenn beim Zurücksetzen des Kennworts ein Fehler auftritt, wird dem Benutzer eine Fehlermeldung angezeigt, und der Benutzer kann es erneut versuchen.  Es kann zu Fehlern kommen, weil der Dienst nicht verfügbar ist, weil das ausgewählte Kennwort nicht den Organisationsrichtlinien entspricht, weil der Benutzer nicht im lokalen AD gefunden wurde, oder weil andere Gründe vorliegen.  Es gibt für jeden dieser Fälle eine spezifische Meldung, die den Benutzer darüber informiert, was zur Problemlösung unternommen werden kann.

### Unterstützte Szenarien für die Kennwortrückschreibung
In der nachstehenden Tabelle wird beschrieben, welche Szenarien für welche Versionen der Synchronisierungsfunktionen unterstützt werden.  Im Allgemeinen wird empfohlen, dass Sie die neueste Version von installieren [Azure AD Connect](active-directory-aadconnect.md#download-azure-ad-connect) wenn Sie das Rückschreiben von Kennwörtern verwenden möchten.

  ![][002]

### Sicherheitsmodell für die Kennwortrückschreibung
Die Kennwortrückschreibung ist ein äußerst sicherer und zuverlässiger Dienst.  Zum Schutz Ihrer Informationen wird ein vierstufiges Sicherheitsmodell angewendet, das nachfolgend beschrieben wird.

- **Mandantenspezifisches Service Bus Relay** – Wenn Sie den Dienst einrichten, wird ein mandantenspezifisches Servicebus-Relay, die durch ein zufällig generiertes sicheres Kennwort geschützt ist, das Microsoft nie zugreifen kann.
- **Gesperrter, kryptografisch sicherer Kennwortverschlüsselungsschlüssel** – nach dem Servicebus Relay erstellt wurde, erstellen wir einen starken symmetrischen Schlüssels, der verwendet wird, das Kennwort unverschlüsselt Netzwork zu verschlüsseln.  Dieser Schlüssel liegt ausschließlich im Speicher für geheime Schlüssel Ihres Unternehmens in der Cloud vor, der streng geschützt und überwacht wird, wie jedes Kennwort im Verzeichnis.
- **TLS nach Industriestandard** – beim Zurücksetzen eines Kennworts oder Vorgang auftritt, in der Cloud, wir nehmen das Klartextkennwort und mit Ihrem öffentlichen Schlüssel zu verschlüsseln.  Anschließend wird es in eine HTTPS-Nachricht eingefügt und über einen mithilfe der SSL-Zertifikate von Microsoft verschlüsselten Kanal an Ihre Service Bus Relay gesendet.  Nachdem die Nachricht vom Service Bus empfangen wurde, wird der lokale Agent aktiviert, authentifiziert sich beim Service Bus mit dem zuvor generierten sicheren Kennwort, nimmt die verschlüsselte Nachricht entgegen, entschlüsselt diese mit dem generierten privaten Schlüssel und versucht anschließend, das Kennwort über die AD DS-SetPassword-API festzulegen.  In diesem Schritt kann Ihre lokale AD-Kennwortrichtlinie (Komplexität, Alter, Verlauf, Filter usw.) in der Cloud erzwungen werden.
- **Richtlinien** – schließlich, wenn aus irgendeinem Grund die Nachricht in Service Bus verbleibt, weil Ihr lokaler Dienst ausgefallen ist, wird das Zeitlimit und nach einigen Minuten entfernt, um die Sicherheit noch weiter erhöhen.

## Wie funktioniert das Portal für die Kennwortzurücksetzung?
Wenn ein Benutzer zum Portal für die Kennwortzurücksetzung navigiert, wird ein Workflow gestartet. Über diesen Workflow wird geprüft, ob das Benutzerkonto gültig ist, welcher Organisation der Benutzer angehört, wo das Benutzerkennwort verwaltet wird und ob der Benutzer zur Verwendung der Funktion lizenziert ist.  Nachfolgend wird beschrieben, welche Logik hinter der Seite zur Kennwortzurücksetzung steckt.

1.  Benutzer klickt auf die kein Zugriff auf Ihre Link Konto oder wechselt direkt zu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Der Benutzer gibt eine Benutzer-ID ein und durchläuft erfolgreich die Captchaprüfung.
3.  Azure AD prüft folgendermaßen, ob der Benutzer diese Funktion verwenden darf:
    - Es wird geprüft, ob die Funktion für diesen Benutzer aktiviert ist und ob eine Azure AD-Lizenz zugewiesen ist.
        - Wenn diese Funktion für den Benutzer nicht aktiviert ist oder keine Lizenz vorliegt, wird der Benutzer aufgefordert, sich zum Zurücksetzen des Kennworts an den Administrator zu wenden.
    - Es wird überprüft, ob der Benutzer in seinem Konto Daten für die Überprüfung in mehreren Schritten definiert hat, die der Administratorrichtlinie entsprechen.
        - Wenn die Richtlinie nur eine Überprüfung in einem Schritt vorsieht, wird sichergestellt, dass der Benutzer für mindestens eine der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
          - Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
        - Wenn die Richtlinie eine Überprüfung in zwei Schritten vorsieht, wird sichergestellt, dass der Benutzer für mindestens zwei der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
          - Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
    - Es wird überprüft, ob das Benutzerkennwort lokal verwaltet wird oder nicht (im Verbund oder mit Kennworthashsynchronisierung).
       - Wenn das Rückschreiben von Kennwörtern konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, kann der Benutzer mit der Authentifizierung fortfahren und sein Kennwort zurücksetzen.
       - Wenn das Rückschreiben von Kennwörtern nicht konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
4.  Wenn festgestellt wird, dass der Benutzer sein Kennwort zurücksetzen darf, wird er durch den Vorgang für die Kennwortzurücksetzung geleitet.

Weitere Informationen zum Bereitstellen des Rückschreibens am [Erste Schritte: Azure AD-Kennwortverwaltung](active-directory-passwords-getting-started.md).

### Welche Daten werden bei der Kennwortzurücksetzung verwendet?
In der folgenden Tabelle wird beschrieben, wo und wie diese Daten während des Vorgangs der Kennwortzurücksetzung verwendet werden. Diese Aufstellung soll Sie dabei unterstützen, die für Ihre Organisation geeigneten Authentifizierungsoptionen auszuwählen. Diese Tabelle zeigt auch Formatierungsanforderungen für Fälle, in denen Sie Daten im Namen des Benutzers aus Eingabepfaden bereitstellen, die diese Daten nicht validieren.

> [AZURE.NOTE] Telefon (Büro) wird nicht im registrierungsportal angezeigt, da der Benutzer derzeit nicht zum Bearbeiten dieser Eigenschaft im Verzeichnis können.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Name der Kontaktmethode</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory-Datenelement</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Verwendet / festlegbar, wo?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Formatanforderungen</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Bürotelefon</p>
            </td>
            <td>
              <p>PhoneNumber</p>
              <p>z. B. Set-MsolUser -UserPrincipalName JWarner@contoso.com -PhoneNumber "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Festlegbar wo:</p>
              <p>"PhoneNumber" kann in PowerShell, DirSync, im Azure-Verwaltungsportal und im Office-Verwaltungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (z. B. +1 1234567890)</p>
              <ul>
                <li class="unordered">
                                        Must provide a country code<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must provide an area code (where applicable)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have provide a + in front of the country code<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have a space between country code and the rest of the number<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Extensions are not supported, if you have any extensions specified, we will strip it from the number before dispatching the phone call.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiltelefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>ODER</p>
              <p>MobilePhone</p>
              <p>("AuthenticationPhone" wird verwendet, wenn Daten vorhanden sind, andernfalls wird das Feld "Mobiltelefon" verwendet.)</p>
              <p>z. B. Set-MsolUser -UserPrincipalName JWarner@contoso.com -MobilePhone "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Registrierungsportal</p>
              <p>Festlegbar wo: </p>
              <p>"AuthenticationPhone" kann im Kennwortzurücksetzungsportal oder im MFA-Registrierungsportal festgelegt werden.</p>
              <p>"MobilePhone" kann in PowerShell, DirSync, im Azure-Verwaltungsportal und im Office-Verwaltungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (z. B. +1 1234567890)</p>
              <ul>
                <li class="unordered">
                                        Must provide a country code.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must provide an area code (where applicable).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have provide a + in front of the country code.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have a space between country code and the rest of the number.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Extensions are not supported, if you have any extensions specified, we ignore it when dispatching the phone call.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternative E-Mail-Adresse</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>ODER</p>
              <p>AlternateEmailAddresses[0] </p>
              <p>("AuthenticationEmail" wird verwendet, wenn Daten vorhanden sind, andernfalls wird das Feld "Alternative E-Mail-Adresse" verwendet.)</p>
              <p>Hinweis: Das Feld "Alternative E-Mail-Adresse" wird als Array aus Zeichenfolgen im Verzeichnis angegeben.  Es wird der erste Eintrag im Array verwendet.</p>
              <p>z. B. Set-MsolUser -UserPrincipalName JWarner@contoso.com -AlternateEmailAddresses "email@live.com"</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Registrierungsportal</p>
              <p>Festlegbar wo: </p>
              <p>"AuthenticationEmail" kann im Kennwortzurücksetzungsportal oder im MFA-Registrierungsportal festgelegt werden.</p>
              <p>"AlternateEmail" kann in PowerShell, im Azure-Verwaltungsportal und im Office-Verwaltungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">User@Domain.com</a> oder 甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
                                        Emails should follow standard formatting as per .<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Unicode emails are supported.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Sicherheitsfragen und -antworten</p>
            </td>
            <td>
              <p>Stehen nicht zur direkten Änderung im Verzeichnis zur Verfügung.</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Registrierungsportal </p>
              <p>Festlegbar wo: </p>
              <p>Sicherheitsfragen können ausschließlich im Azure-Verwaltungsportal festgelegt werden.</p>
              <p>Die Antworten auf die für einen Benutzer festgelegten Sicherheitsfragen können nur über das Registrierungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>Sicherheitsfragen können maximal 200 Zeichen und müssen mindestens 3 Zeichen umfassen.</p>
              <p>Antworten können maximal 40 Zeichen und müssen mindestens 3 Zeichen umfassen.</p>
            </td>
          </tr>
        </tbody></table>

###Zugriff auf Daten zur Kennwortzurücksetzung für Ihre Benutzer
####Per Synchronisierung festlegbare Daten
Die folgenden Felder können lokal synchronisiert werden:

* Mobiltelefon
* Bürotelefon

####Mit Azure AD PowerShell festlegbare Daten
Auf die folgenden Felder können Sie mit Azure AD PowerShell und der Graph-API zugreifen:

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon
* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung

####Nur über die Benutzeroberfläche für die Registrierung festlegbare Daten
Die folgenden Felder sind nur über die SSP-Registrierung UI (https://aka.ms/ssprsetup):

* Sicherheitsfragen und -antworten

####Was geschieht bei Registrierung eines Benutzers?
Wenn ein Benutzer registriert, wird die Registrierungsseite **immer** Legen Sie die folgenden Felder:

* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung
* Sicherheitsfragen und -antworten

Wenn Sie einen Wert für angegeben haben **Mobiltelefon** oder **Alternative e-Mail-Adresse**, sofort können Sie die ihre Kennwörter zurücksetzen, selbst wenn sie für den Dienst registriert haben.  Benutzer sehen diese Werte zudem bei der ersten Registrierung und können sie bei Bedarf ändern.  Jedoch, nachdem sie erfolgreich registriert haben, diese Werte beibehalten werden der **Authentifizierungstelefon** und **Authentifizierung E-Mail** Feldern.

Dies kann eine gute Möglichkeit darstellen, eine größere Anzahl von Benutzern zum Verwenden von SSPR zu entsperren und Benutzer dennoch zu ermöglichen, diese Informationen im Registrierungsprozess zu prüfen.

####Festlegen der Daten zur Kennwortzurücksetzung mit PowerShell
Sie können Werte für die folgenden Felder mit Azure AD PowerShell festlegen.

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon

Informationen zum Einstieg, Sie müssen zunächst [herunterladen und Installieren von Azure AD PowerShell-Modul](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Sobald Sie es installiert haben, können Sie die folgenden Schritte befolgen, um jedes Feld zu konfigurieren.

#####Alternative E-Mail-Adresse
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####Mobiltelefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####Bürotelefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####Lesen der Daten zur Kennwortzurücksetzung mit PowerShell
Sie können Werte für die folgenden Felder mit Azure AD PowerShell lesen.

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon
* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung

Informationen zum Einstieg, Sie müssen zunächst [herunterladen und Installieren von Azure AD PowerShell-Modul](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Sobald Sie es installiert haben, können Sie die folgenden Schritte befolgen, um jedes Feld zu konfigurieren.

#####Alternative E-Mail-Adresse
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####Mobiltelefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####Bürotelefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####Telefonnummer für Authentifizierung
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####E-Mail-Adresse für Authentifizierung
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## Links zu Informationen zur Kennwortzurücksetzung
Im Folgenden finden Sie Links zu allen Webseiten mit Informationen zur Kennwortzurücksetzung für Azure AD: 

* [**Ihr eigenes Kennwort zurücksetzen**](active-directory-passwords-update-your-own-password.md) -erfahren Sie mehr über zurücksetzen oder ändern Ihr eigenes Kennwort als Benutzer des Systems
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) -erfahren Sie mehr über die sechs verschiedenen Komponenten der Dienst und die jeweilige
* [**Erste Schritte**](active-directory-passwords-getting-started.md) -erfahren Sie, wie Sie Benutzern das Zurücksetzen und Ändern ihrer Cloud- oder lokalen Kennwörter erlauben
* [**Anpassen von**](active-directory-passwords-customize.md) -erfahren Sie, wie das Aussehen und Verhalten des Diensts, um den Bedürfnissen Ihres Unternehmens anpassen.
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) -erfahren Sie, wie schnell bereitstellen und effektiv verwalten von Kennwörtern in Ihrer Organisation
* [**Einblicke**](active-directory-passwords-get-insights.md) -erfahren Sie mehr über unsere integrierten Berichtsfunktionen
* [**Häufig gestellte Fragen zu**](active-directory-passwords-faq.md) – Hier erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung bei**](active-directory-passwords-troubleshoot.md) -erfahren Sie, wie Probleme mit dem Dienst schnell beheben



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"


