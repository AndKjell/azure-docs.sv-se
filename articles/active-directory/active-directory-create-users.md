<properties
    pageTitle="Erstellen oder Bearbeiten von Benutzern in Azure Active Directory | Microsoft Azure"
    description="Erklärt, wie Sie Benutzerkonten unter Azure Active Directory erstellen oder bearbeiten."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="curtand"/>

# Erstellen oder Bearbeiten von Benutzern in Azure Active Directory

Sie müssen in Azure Active Directory (Azure AD) für jeden Benutzer, der auf einen Microsoft-Clouddienst zugreifen soll, ein eigenes Konto erstellen. Sie können Benutzerkonten auch ändern oder löschen, wenn sie nicht mehr benötigt werden. Benutzer haben standardmäßig keine Administratorberechtigungen, aber Sie können ihnen diese erteilen.

## Erstellen eines Benutzers

1. Klicken Sie auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.
2. Auf der **Benutzer** auf **Benutzer hinzufügen**.
3. Auf der **Erzählen Sie uns dieses Benutzers** Seite für **Benutzertyp**, wählen Sie entweder:
    1. **Neuer Benutzer in Ihrer Organisation** – Gibt an, dass Sie ein neues Benutzerkonto erstellt und in Ihrem Verzeichnis verwalten möchten.
    2. **Benutzer mit einem vorhandenen Microsoft-Konto** – Gibt an, dass Sie ein vorhandenes Microsoft-Konto zu Ihrem Verzeichnis hinzufügen, um Azure-Ressourcen mit einem Co-Administrator zusammen arbeiten, die mit einem Microsoft-Konto auf Azure zugreift möchten.
    3. **Benutzer in einem anderen Azure AD-Verzeichnis** – Gibt an, die aus einem anderen Azure AD-Verzeichnis das Verzeichnis ein Benutzerkonto hinzugefügt werden soll. Sie müssen Mitglied des anderen Verzeichnisses sein, um darin einen Benutzer auswählen zu können.
4. Je nach ausgewählter Option geben Sie entweder einen Benutzernamen oder einen Microsoft-Kontonamen ein, mit dem sich der Benutzer anmeldet.
5. Für den Benutzer **Profil** Seite, geben Sie den vor- und Nachname, einen Anzeigenamen und eine Benutzerrolle aus dem Rollen-Dropdown-Menü. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md). Geben Sie an, ob **mehrstufige Authentifizierung aktivieren**.
6. Auf der **vorübergehendes Kennwort abrufen** auf **Erstellen**.

Achten Sie auf die folgenden Probleme, die beim Erstellen eines Benutzerkontos auftreten können, wenn Ihre Organisation mehr als eine Domäne verwendet:

- Sie können Benutzerkonten mit der gleichen Benutzerprinzipalnamen (UPN) domänenübergreifend erstellen, wenn Sie zunächst beispielsweise geoffgrisso@contoso.onmicrosoft.com gefolgt von geoffgrisso@contoso.com erstellen.
- Sie können keine geoffgrisso@contoso.com gefolgt von geoffgrisso@contoso.onmicrosoft.com erstellen.

## Bearbeiten eines Benutzers

Wenn der Benutzer, den Sie bearbeiten möchten, mit Ihrem lokalen Active Directory-Dienst synchronisiert wird, wird eine Fehlermeldung angezeigt. Sie können den Benutzer mit diesem Verfahren dann nicht bearbeiten. Verwenden Sie zum Bearbeiten des Benutzers Ihre lokalen Active Directory-Verwaltungstools.

So bearbeiten Sie einen Benutzer im klassischen Azure-Portal:

1. Klicken Sie auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.
2. Auf der **Benutzer** Seite, klicken Sie auf den Anzeigenamen des Benutzers, die Sie bearbeiten möchten.
3. Geben Sie die Änderungen, und klicken Sie dann auf **Speichern**.

## Zurücksetzen des Kennworts für einen Benutzer

1. Klicken Sie auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.
2. Auf der **Benutzer** Seite, klicken Sie auf den Anzeigenamen des Benutzers, die Sie bearbeiten möchten.
3. Klicken Sie am unteren Rand des Portals, auf **Kennwort zurücksetzen**.
4. Klicken Sie im Dialogfeld das Zurücksetzen des Kennworts auf **Zurücksetzen**.
5. Klicken Sie auf das Häkchen, um zu bestätigen, dass das Kennwort zurückgesetzt wurde.

## Erstellen eines externen Benutzers

In Azure AD können Sie einem Azure AD-Verzeichnis auch Benutzer aus einem anderen Azure AD-Verzeichnis oder einen Benutzer mit einem Microsoft-Konto hinzufügen. Ein Benutzer kann Mitglied in bis zu 20 verschiedenen Verzeichnissen sein.

Bei Benutzern, die aus einem anderen Verzeichnis hinzugefügt werden, handelt es sich um externe Benutzer. Externe Benutzer können mit Benutzern zusammenarbeiten, die einem Verzeichnis bereits vorhanden sind, z. B. in einer Testumgebung, ohne dass sie sich mit neuen Konten und Anmeldeinformationen anmelden müssen. Externe Benutzer sind über ihr Basisverzeichnis authentifiziert, wenn sie sich anmelden und die Authentifizierung für alle anderen Verzeichnisse funktioniert, denen deren Mitglied Sie sind.

Um einen externen Benutzer erstellen möchten, erstellen Sie einen Benutzer in der klassischen Azure-Portal und für **Benutzertyp**, Option **Benutzer in einem anderen Azure AD-Verzeichnis**.

## Verwaltung externer Benutzer und Einschränkungen

Wenn Sie einen Benutzer aus einem Verzeichnis einem neuen Verzeichnis hinzufügen, ist dieser Benutzer im neuen Verzeichnis ein externer Benutzer. Zuerst werden der Anzeigename und der Benutzername aus dem so genannten „Basisverzeichnis“ des Benutzers kopiert und für den externen Benutzer in dem anderen Verzeichnis verwendet. Ab diesem Zeitpunkt sind diese und andere Eigenschaften des externen Benutzerobjekts vollständig unabhängig voneinander: Wenn Sie im Basisverzeichnis eine Änderung für diesen Benutzer vornehmen, also z. B. den Benutzernamen ändern, seine Position hinzufügen usw., werden die Änderungen nicht für das externe Benutzerkonto im anderen Verzeichnis übernommen.

Die einzige Verbindung zwischen den beiden Objekten ist, dass der Benutzer immer anhand seines Basisverzeichnisses oder über sein Microsoft-Konto authentifiziert wird. Daher ist keine Option zum Zurücksetzen des Kennworts oder zum Aktivieren der Multi-Factor Authentication für ein externes Benutzerkonto vorhanden. Derzeit wird ausschließlich die Authentifizierungsrichtlinie des Basisverzeichnisses oder des Microsoft-Kontos verwendet, die beim Anmelden des Benutzers ausgewertet wird.

> [AZURE.NOTE]
> Sie können den Benutzer im externen Verzeichnis aber trotzdem deaktivieren. Dadurch wird der Zugriff auf Ihr Verzeichnis blockiert.

Wenn ein Benutzer in seinem Basisverzeichnis gelöscht wird oder sein Microsoft-Konto kündigt, ist der externe Benutzer weiterhin im Verzeichnis vorhanden. Er kann aber nicht mehr auf Ressourcen im Verzeichnis zugreifen, da die Authentifizierung über das Basisverzeichnis oder das Microsoft-Konto nicht mehr möglich ist.

Ein Benutzer, der Administrator mehrerer Verzeichnisse ist, kann diese Verzeichnisse im klassischen Azure-Portal verwalten. Andere Anwendungen wie Office 365 enthalten derzeit keine Optionen zum Zuweisen von und Zugreifen auf Dienste als externer Benutzer in einem anderen Verzeichnis. Wir stellen demnächst Anleitungen für Entwickler bereit. Darin ist beschrieben, wie Apps für Benutzer bereitgestellt werden können, die Mitglieder mehrerer Verzeichnisse sind.

Derzeit gelten bestimmte Einschränkungen. Ein Administrator kann die Zustimmung für eine mehrinstanzenfähige Anwendung nur im Basisverzeichnis gewähren, und die Bereitstellung ist nur für SaaS-Apps und SSO per Zugriffsbereich im Basisverzeichnis möglich. Für Benutzer mit Microsoft-Konto gelten die gleichen Einschränkungen. Es ist momentan nicht möglich, die Zustimmung für eine mehrinstanzenfähige Anwendung zu gewähren oder den Zugriffsbereich zu verwenden.

## Gäste

Ein **Gast** ist ein Benutzer in Ihrem Verzeichnis, Benutzertyp auf "Gast" festgelegt ist. Für normale Benutzer lautet der Benutzertyp „Mitglied“, um anzugeben, dass sie Mitglied Ihres Verzeichnisses sind. Gäste werden erstellt, wenn Sie eine Ressource für einen externen Benutzer Ihres Verzeichnisses freigeben. Beispiele hierfür sind das Hinzufügen eines Microsoft-Kontos zu Ihrem Azure-Abonnement oder das Freigeben eines Dokuments in SharePoint für einen externen Benutzer.

Gäste verfügen im Verzeichnis über eingeschränkte Berechtigungen. Diese Berechtigungen bewirken, dass Gäste nicht auf alle Informationen zu anderen Benutzern im Verzeichnis zugreifen können. Sie können mit den Benutzern und Gruppen, die den bearbeiteten Ressourcen zugeordnet sind, aber trotzdem interagieren. Ein Gast, der einem Azure-Abonnement zugewiesen ist, kann beispielsweise andere Benutzer und Gruppen sehen, die dem Azure-Abonnement zugeordnet sind. Außerdem können Gäste auch nach anderen Benutzern im Verzeichnis suchen, denen der Zugriff auf das Abonnement gewährt werden soll, sofern ihnen die vollständige E-Mail-Adresse des betreffenden Benutzers bekannt ist. Gäste können nur eingeschränkte Eigenschaften anderer Benutzer anzeigen. Diese Eigenschaften sind auf Anzeigename, E-Mail-Adresse, Benutzerprinzipalname (UPN) und Miniaturansicht (Foto) beschränkt.

## Konfigurieren von Richtlinien für den Benutzerzugriff

Die **konfigurieren** Registerkarte eines Verzeichnisses enthält Optionen zum Steuern des Zugriffs für externe Benutzer. Diese Optionen können nur über die Benutzeroberfläche (es gibt keine Windows PowerShell- oder API-Methode) im vollständigen klassischen Azure-Portal durch einen globalen Verzeichnisadministrator geändert werden.
Zum Öffnen der **konfigurieren** Registerkarte im klassischen Azure-Portal **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses.

![][1]

Anschließend können Sie die Optionen zur Zugriffssteuerung für externe Benutzer ändern.

![][2]

Standardmäßig Gäste können nicht aufgelistet werden der Inhalt des Verzeichnisses, damit sie keine Benutzer oder Gruppen in angezeigt werden der **Mitgliederliste**. Gäste können jedoch durch Eingabe der vollständigen E-Mail-Adresse nach einem Benutzer suchen und ihm dann Zugriff gewähren. Für Gäste gelten folgende allgemeine Beschränkungen:

- Benutzer und Gruppen können nicht im Verzeichnis aufgelistet werden.
- Wenn die E-Mail-Adresse des Benutzers bekannt ist, können nur eingeschränkte Informationen zu diesem Benutzer angezeigt werden.
- Wenn der Name einer Gruppe bekannt ist, können nur eingeschränkte Informationen zu dieser Gruppe angezeigt werden.

Durch die Anzeige eingeschränkter Informationen zu einem Benutzer oder einer Gruppe können Gäste andere Personen einladen und Informationen zu Personen anzeigen, mit denen sie zusammenarbeiten.  

## Nächste Schritte

- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure AD](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png


