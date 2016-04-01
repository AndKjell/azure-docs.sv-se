<properties
    pageTitle="Beziehung zwischen Azure-Abonnements und Azure Active Directory | Microsoft Azure"
    description="Anmelden bei Microsoft Azure und verwandte Aspekte, z. B. die Beziehung zwischen einem Azure-Abonnement und Azure Active Directory."
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
    ms.topic="get-started-article"
    ms.date="12/01/2015"
    ms.author="curtand"/>

# Beziehung zwischen Azure-Abonnements und Azure Active Directory

In diesem Thema werden Informationen zum Anmelden bei Microsoft Azure und verwandte Themen behandelt, z. B. die Beziehung zwischen einem Azure-Abonnement und Azure Active Directory (Azure AD).

## Konten, die für die Anmeldung verwendet werden können
Beginnen wir mit den Konten, die Sie zum Anmelden verwenden können. Zwei Typen sind verfügbar: ein Microsoft-Konto (früher als Microsoft Live ID bezeichnet) und ein Azure AD-Geschäfts- oder -Schulkonto (das in Azure AD gespeichert ist).

 Microsoft-Konto  | Azure AD-Konto
    ------------- | -------------
Das Privatnutzeridentitätssystem, das von Microsoft betrieben wird. | Das Unternehmensidentitätssystem, das von Microsoft betrieben wird.
Authentifizierung bei Diensten, die privatnutzerorientiert sind, z. B. Hotmail und MSN. | Authentifizierung bei Diensten, die unternehmensorientiert sind, z. B. Office 365
Privatnutzer erstellen ihre eigenen Microsoft-Konten, z. B. beim Registrieren für E-Mail. | Unternehmen und Organisationen erstellen und verwalten ihre eigenen Geschäfts- oder Schulkonten.
Identitäten werden im Microsoft-Kontosystem erstellt und gespeichert. | Identitäten werden mithilfe von Azure oder anderen Diensten wie Office 365 erstellt und in einer Azure AD-Instanz gespeichert, die der Organisation zugeordnet ist.

Obwohl Azure ursprünglich nur von Microsoft-Konto zulässig, inzwischen Zugriff durch Benutzer aus *beide* Systeme. Dies wurde erreicht, indem alle Azure-Eigenschaften Azure AD für die Authentifizierung vertrauen, Azure AD Organisationsbenutzer authentifiziert und eine Verbundbeziehung erstellt wurde, in der Azure AD dem Privatnutzeridentitätssystem für Microsoft-Konten vertraut, um Privatnutzer zu authentifizieren. Daher kann Azure AD Microsoft-"Gastkonten" ebenso wie "systemeigene" Azure AD-Konten authentifizieren.

Hier meldet sich z. B. ein Benutzer mit einem Microsoft-Konto beim klassischen Azure-Portal an.

> [AZURE.NOTE]
> Zum klassischen Azure-Portal anmelden, müssen msmith@hotmail.com eine Azure-Abonnement verfügen. Das Konto muss einem Dienstadministrator oder Co-Administrator des Abonnements gehören.

![][1]

Da diese Hotmail-Adresse zu einem Privatnutzerkonto gehört, wird die Anmeldung vom Privatnutzeridentitätssystem für Microsoft-Konten authentifiziert. Das Azure AD-Identitätssystem vertraut der vom Microsoft-Kontosystem vorgenommenen Authentifizierung und stellt ein Token für den Zugriff auf Azure-Dienste aus.

## Die Beziehung zwischen einem Azure-Abonnement und Azure AD

Jedes Azure-Abonnement weist eine Vertrauensstellung mit einer Azure AD-Instanz auf. Dies bedeutet, dass es diesem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten vertraut. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, ein Abonnement vertraut jedoch nur einem Verzeichnis. Sie können auf der Registerkarte "Einstellungen" ermitteln, welchem Verzeichnis Ihr Abonnement vertraut. Sie können [abonnementeinstellungen bearbeiten](active-directory-understanding-resource-access.md) So ändern Sie das Verzeichnis es vertraut.

Diese Vertrauensstellung, die ein Abonnement mit einem Verzeichnis aufweist, unterscheidet sich von der Beziehung, die ein Abonnement mit allen anderen Ressourcen in Azure (Websites, Datenbanken usw.) hat. Diese ähneln eher den untergeordneten Ressourcen eines Abonnements. Wenn ein Abonnement abläuft, wird der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind, ebenfalls beendet. Das Verzeichnis verbleibt jedoch in Azure, und Sie können ihm ein anderes Abonnement zuordnen und weiterhin die Benutzer des Verzeichnisses verwalten.

Die Azure AD-Erweiterung, die in Ihrem Abonnement angezeigt wird, funktioniert ebenfalls nicht wie die anderen Erweiterungen im klassischen Azure-Portal. Andere Erweiterungen im klassischen Azure-Portal sind auf den Umfang des Azure-Abonnements ausgerichtet. Die Anzeige in der Azure AD-Erweiterung ändert sich nicht basierend auf dem Abonnement – es werden nur Verzeichnisse basierend auf dem angemeldeten Benutzer angezeigt.

Alle Benutzer verfügen über ein Basisverzeichnis, das ihre Authentifizierung ausführt. Sie können jedoch auch Gäste in anderen Verzeichnissen sein. In der Azure AD-Erweiterung wird jedes Verzeichnis angezeigt, in dem Ihr Benutzerkonto Mitglied ist. Alle Verzeichnisse, in denen Ihr Konto kein Mitglied ist, werden nicht angezeigt. Ein Verzeichnis kann Token für die Benutzer von Geschäfts- oder Schulkonten oder von Microsoft-Konten ausstellen (da Azure AD einen Verbund mit dem Microsoft-Kontosystem bildet).

Die folgende Abbildung zeigt ein Abonnement für Michael Smith, nachdem er sich mithilfe eines Geschäftskontos für Contoso registriert hat.

![][2]

## Verwalten eines Abonnements und eines Verzeichnisses
Die Administratorrollen für ein Azure-Abonnement verwalten Ressourcen, die an das Azure-Abonnement gebunden sind. Diese Rollen und die bewährten Methoden zum Verwalten Ihres Abonnements fallen am [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

Standardmäßig wird Ihnen die Dienstadministratorrolle zugewiesen, wenn Sie sich registrieren. Wenn sich andere Benutzer anmelden und mithilfe des gleichen Abonnements auf die Dienste zugreifen müssen, können Sie diese als Co-Administratoren hinzufügen. Der Dienstadministrator und die Co-Administratoren können Microsoft-Konten oder Geschäfts- oder Schulkonten im Verzeichnis sein, dem das Azure-Abonnement zugeordnet ist.

Azure AD weist eine andere Gruppe von Administratorrollen zum Verwalten der Features für Verzeichnisse und Identitäten. Der globale Administrator eines Verzeichnisses kann z. B. dem Verzeichnis Benutzer hinzufügen oder für Benutzer die Multi-Factor Authentication verlangen. Ein Benutzer, der ein Verzeichnis erstellt, wird der globalen Administratorrolle zugewiesen und kann anderen Benutzern Administratorrollen zuweisen.

Wie bei den Abonnementadministratoren kann es sich bei den Azure AD-Administratorrollen um Microsoft-Konten oder Geschäfts- oder Schulkonten handeln. Die Azure AD-Administratorrollen werden auch von anderen Diensten (z. B. Office 365 und Microsoft Intune) genutzt. Weitere Informationen finden Sie unter [Zuweisen von Administratorrollen](active-directory-assign-admin-roles.md).

Aber der wichtigste Punkt hierbei ist, dass Azure-Abonnementadministratoren und Azure AD-Verzeichnisadministratoren zwei getrennte Konzepte darstellen. Azure-Abonnementadministratoren können Ressourcen in Azure verwalten und die Active Directory-Erweiterung im klassischen Azure-Portal anzeigen (weil das klassische Azure-Portal eine Azure-Ressource ist). Verzeichnisadministratoren können Eigenschaften im Verzeichnis verwalten.

Eine Person kann Mitglied beider Rollen sein, was allerdings nicht erforderlich ist. Ein Benutzer kann der globalen Verzeichnisadministratorrolle, nicht jedoch als Dienstadministrator oder Co-Administrator eines Azure-Abonnements zugewiesen sein. Da dieser Benutzer kein Administrator des Abonnements ist, kann er sich nicht beim klassischen Azure-Portal anmelden. Der Benutzer kann jedoch Verzeichnisverwaltungsaufgaben mithilfe anderer Tools (z. B. Azure AD PowerShell oder Office 365 Admin Center) ausführen.

## Warum kann ich das Verzeichnis nicht mit meinem aktuellen Benutzerkonto verwalten?

Mitunter versucht ein Benutzer, sich mithilfe eines Geschäfts- oder Schulkontos beim klassischen Azure-Portal anzumelden, bevor er sich für ein Azure-Abonnement registriert. In diesem Fall erhält der Benutzer die Meldung, dass kein Abonnement für dieses Konto vorhanden ist. Die Meldung enthält einen Link zum Ausprobieren eines kostenlosen Testabonnements.

Nachdem sich der Benutzer für die kostenlose Testversion registriert hat, kann er das Verzeichnis für die Organisation im klassischen Azure-Portal anzeigen, jedoch nicht verwalten (also keine Benutzer hinzufügen oder vorhandene Benutzereigenschaften bearbeiten), weil der Benutzer kein globaler Verzeichnisadministrator ist. Das Abonnement ermöglicht dem Benutzer zwar die Verwendung des klassischen Azure-Portals und das Anzeigen der Azure Active Directory-Erweiterung, aber zum Verwalten des Verzeichnisses sind die zusätzlichen Berechtigungen eines globalen Administrators erforderlich.

## Verwalten eines Azure-Abonnements, das mithilfe eines Microsoft-Kontos erstellt wurde, mit Ihrem Geschäfts- oder Schulkonto

Als bewährte Methode sollten Sie [für Azure als Organisation registrieren](sign-up-organization.md) und Geschäfts-oder schulkonten zum Verwalten von Ressourcen in Azure verwenden. Geschäfts- oder Schulkonten werden bevorzugt, weil sie zentral von der Organisation verwaltet werden können, die sie erstellt hat, eine größere Anzahl von Features als Microsoft-Konten bieten und direkt von Azure AD authentifiziert werden. Das gleiche Konto bietet Zugriff auf andere Microsoft Online Services, die für Unternehmen und Organisationen angeboten werden, z. B. Office 365 oder Microsoft Intune. Wenn Sie bereits über ein Konto verfügen, das Sie mit diesen anderen Eigenschaften verwenden, möchten Sie dasselbe Konto wahrscheinlich auch mit Azure verwenden. Sie verfügen außerdem bereits über eine Active Directory-Instanz, die diese Eigenschaften unterstützt, denen Ihr Azure-Abonnement vertrauen soll.

Geschäfts- oder Schulkonten können auch auf vielfältigere Weise als ein Microsoft-Konto verwaltet werden. Ein Administrator kann z. B. das Kennwort eines Geschäfts- oder Schulkontos zurücksetzen oder für dieses Konto die Multi-Factor Authentication verlangen.

In einigen Fällen wünschen Sie ggf., dass ein Benutzer aus Ihrer Organisation Ressourcen verwalten können soll, die einem Azure-Abonnement für ein Microsoft-Privatnutzerkonto zugeordnet sind. Weitere Informationen zum Übergang auf die Verwaltung von Abonnements oder Verzeichnissen durch verschiedene Konten haben, finden Sie unter [Verwalten des Verzeichnisses für Ihr Office 365-Abonnement in Azure](#manage-the-directory-for-your-office-365-subscription-in-azure).


## Anmeldung, wenn Sie Ihre geschäftliche E-Mail-Adresse für Ihr Microsoft-Konto verwendet haben

Wenn Sie in der Vergangenheit ein Microsoft-Privatnutzerkonto mithilfe Ihrer geschäftlichen E-Mail-Adresse als Benutzerbezeichner erstellt haben, wird ggf. eine Seite angezeigt, auf der Sie aufgefordert werden, aus dem Microsoft Azure-Kontosystem oder Microsoft-Kontosystem auszuwählen.

![][3]

Sie haben Benutzerkonten mit dem gleichen Namen – ein Konto in Azure AD und ein anderes im Privatnutzerkontosystem von Microsoft. Sie müssen das Konto wählen, das dem Azure-Abonnement zugeordnet ist, das Sie verwenden möchten. Wenn ein Fehler ausgegeben wird, der besagt, dass für diesen Benutzer kein Abonnement vorhanden ist, haben Sie wahrscheinlich die falsche Option gewählt. Melden Sie sich ab, und versuchen Sie es erneut. Weitere Informationen zu Fehlern, die Anmeldung verhindern können, finden Sie unter ["Es wurden keine mit Ihrem Konto verknüpften Abonnements gefunden," Problembehandlung](https://social.msdn.microsoft.com/Forums/en-US/f952f398-f700-41a1-8729-be49599dd7e2/troubleshooting-we-were-unable-to-find-any-subscriptions-associated-with-your-account-errors-in?forum=windowsazuremanagement).

## Verwalten des Verzeichnisses für Ihr Office 365-Abonnement in Azure

Angenommen, Sie haben sich zuerst für Office 365 und dann für Azure registriert. Nun möchten Sie das Verzeichnis für das Office 365-Abonnement im klassischen Azure-Portal verwalten. Je nachdem, ob Sie sich für Azure registriert haben oder nicht, gibt es hierzu zwei Möglichkeiten

### Ich habe kein Abonnement für Azure

In diesem Fall [für Azure registrieren](sign-up-organization.md) mit dem gleichen Geschäfts- oder schulkonto, mit denen Sie sich bei Office 365 anmelden. Relevante Informationen aus dem Office 365-Konto werden dann in das Azure-Registrierungsformular übertragen. Ihr Konto wird der Rolle des Dienstadministrators des Abonnements zugewiesen.  

### Ich habe mit meinem Microsoft-Konto ein Abonnement für Azure

Wenn Sie sich für Office 365 mit einem Geschäfts- oder Schulkonto registriert und dann für Azure mit einem Microsoft-Konto registriert haben, stehen Ihnen zwei Verzeichnisse zur Verfügung: eines für Arbeit oder Schule und ein Standardverzeichnis, das erstellt wurde, als Sie sich bei Azure registriert haben.

Führen Sie diese Schritte aus, um beide Verzeichnisse im klassischen Azure-Portal zu verwalten.

> [AZURE.NOTE]
> Diese Schritte können nur ausgeführt werden, während ein Benutzer mit einem Microsoft-Konto angemeldet ist. Wenn der Benutzer, sich mit einer Arbeitsaufgabe oder Schul-Kontos, wird die Option angemeldet ist **vorhandenes Verzeichnis** ist nicht verfügbar, da ein Geschäfts- oder schulkonto nur durch sein Basisverzeichnis authentifiziert werden können (also das Verzeichnis, in dem Arbeits-oder schulkonto gespeichert ist, und das im Besitz der Organisation oder Schule).

1. Melden Sie sich mit Ihrem Microsoft-Konto beim klassischen Azure-Portal an.
2. Klicken Sie auf **Neu** > **Anwendungsdienste** > **Active Directory** > **Verzeichnis** > **Erstellen Sie benutzerdefinierte**.
3. Klicken Sie auf **vorhandenes Verzeichnis** und überprüfen Sie **bin ich nun die Abmeldung bereit** und klicken Sie auf das Häkchen, um die Aktion abzuschließen.
4. Melden Sie sich beim klassischen Azure-Portal mit einem Konto an, das über globale Administratorrechte für das Geschäfts- oder Schulverzeichnis verfügt.
5. Bei der Aufforderung zum **Verzeichnis "Contoso" mit Azure verwenden?**, und klicken Sie auf **weiterhin**.
6. Klicken Sie auf **jetzt abmelden**.
7. Melden Sie sich mit Ihrem Microsoft-Konto wieder beim klassischen Azure-Portal an. Beide Verzeichnisse werden in der Active Directory-Erweiterung angezeigt.


## Nächste Schritte
[Als Unternehmen für Azure registrieren](sign-up-organization.md)


<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG


