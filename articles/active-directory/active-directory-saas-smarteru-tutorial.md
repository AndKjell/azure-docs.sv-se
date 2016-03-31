<properties 
    pageTitle="Tutorial: Azure Active Directory-Integration mit SmarterU | Microsoft Azure" 
    description="Hier erfahren Sie, wie Sie SmarterU mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen." 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#Tutorial: Azure Active Directory-Integration mit SmarterU
  
In diesem Tutorial wird die Integration von Azure und SmarterU erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

-   Ein gültiges Azure-Abonnement
-   Ein SmarterU-Mandant
  
Nach Abschluss dieses Lernprogramms SmarterU zugewiesene Azure AD-Benutzer werden mit einmaligem Anmelden bei der Anwendung auf Ihrer SmarterU-Unternehmenssite (vom Dienstanbieter initiierte Anmeldung) oder über die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).
  
Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1.  Aktivieren der Anwendungsintegration für SmarterU
2.  Konfigurieren der einmaligen Anmeldung
3.  Konfigurieren der Benutzerbereitstellung
4.  Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenario")

##Aktivieren der Anwendungsintegration für SmarterU
  
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SmarterU aktivieren.

###So aktivieren Sie die Anwendungsintegration für SmarterU:

1.  Klicken Sie in der Azure-Verwaltungsportal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory](./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Aus der **Verzeichnis** wählen Sie das Verzeichnis für die Verzeichnisintegration aktiviert werden sollen.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht **Applikationen** im oberen Menü.

    ![Anwendungen](./media/active-directory-saas-smarteru-tutorial/IC700994.png "Applications")

4.  Klicken Sie auf **Hinzufügen** am unteren Rand der Seite.

    ![Anwendung hinzufügen](./media/active-directory-saas-smarteru-tutorial/IC749321.png "Add application")

5.  Auf der **Was möchten Sie tun** Dialogfeld klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-smarteru-tutorial/IC749322.png "Add an application from gallerry")

6.  In der **Suchfeld**, Typ **SmarterU**.

    ![Anwendungsfehler](./media/active-directory-saas-smarteru-tutorial/IC777321.png "Application fallery")

7.  Wählen Sie im Ergebnisbereich **SmarterU**, und klicken Sie dann auf **abgeschlossen** um die Anwendung hinzuzufügen.

    ![SmarterU](./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##Konfigurieren der einmaligen Anmeldung
  
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SmarterU zu authentifizieren.

###So konfigurieren Sie einmaliges Anmelden

1.  In Azure AD-Portal auf der **SmarterU** anwendungsintegrationsseite klicken Sie auf **einmaliges Anmelden konfigurieren** Öffnen der ** einmaliges Anmelden konfigurieren ** Dialogfeld.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777323.png "Configure Single Sign-On")

2.  Auf der **wie sollen sich Benutzer bei SmarterU anmelden** Seite **Microsoft Azure AD Single Sign-On**, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777324.png "Configure Single Sign-On")

3.  Auf der **einmaliges Anmelden für smarteru konfigurieren** Seite, um Ihre Metadaten herunterzuladen, klicken Sie **Metadaten**, und klicken Sie dann die Datendatei lokal als **c:\\SmarterUMetaData.cer**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777325.png "Configure Single Sign-On")

4.  Melden Sie sich in einem anderen Webbrowserfenster bei der SmarterU-Unternehmenswebsite als Administrator an.

5.  Klicken Sie in der Symbolleiste am oberen Rand auf **Konteneinstellungen**.

    ![Kontoeinstellungen](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")

6.  Führen Sie auf der Kontenkonfigurationsseite die folgenden Schritte aus:

    ![Externe Autorisierung](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")

    1.  Wählen Sie **externe Autorisierung aktivieren**.
    2.  In der **Masteranmeldesteuerung** Abschnitt der **SmarterU** Registerkarte.
    3.  In der **Benutzerstandardanmeldung** Abschnitt der **SmarterU** Registerkarte.
    4.  Wählen Sie **Okta aktivieren**.
    5.  Kopieren Sie den Inhalt der heruntergeladenen Metadatendatei, und fügen Sie ihn in die **Okta Metadata** Textfeld.
    6.  Klicken Sie auf **Speichern**.

7.  Auf Azure AD-Portal, wählen Sie die Konfiguration von SSO Bestätigung, und klicken Sie dann auf **abgeschlossen** Schließen die **einmaliges Anmelden konfigurieren für** Dialogfeld.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777328.png "Configure Single Sign-On")

##Konfigurieren der Benutzerbereitstellung
  
Damit sich Azure AD-Benutzer bei SmarterU anmelden können, müssen sie in SmarterU bereitgestellt werden.  
Im Fall von SmarterU ist die Bereitstellung eine manuelle Aufgabe.

###So stellen Sie Benutzerkonten bereit

1.  Melden Sie sich bei Ihrem **SmarterU** Mandanten.

2.  Wechseln Sie zu **Benutzer**.

3.  Führen Sie im Abschnitt "Benutzer“ die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")

    1.  Klicken Sie auf **+ Benutzer**.
    2.  Geben Sie die zugehörigen Attributwerte des Azure AD-Benutzerkontos in die folgenden Textfelder ein: **primäre e-Mail-Adresse**, **Mitarbeiter-ID**, **Kennwort**, **Kennwort bestätigen**, **angegebenen Namen**, **Surname**.
    3.  Klicken Sie auf **Active**.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen SmarterU Benutzerkonten oder APIs von SmarterU Bereitstellung AAD-Benutzerkonten.

##Zuweisen von Benutzern
  
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

###So weisen Sie SmarterU Benutzer zu:

1.  Erstellen Sie im Azure AD-Portal ein Testkonto.

2.  Auf der ** SmarterU ** anwendungsintegrationsseite, klicken Sie auf **Zuweisen von Benutzern**.

    ![Benutzer zuweisen](./media/active-directory-saas-smarteru-tutorial/IC777330.png "Assign Users")

3.  Wählen Sie Ihren Testbenutzer aus, klicken Sie auf **Zuweisen**, und klicken Sie dann auf **Ja** auf die Zuweisung zu bestätigen.

    ![Ja](./media/active-directory-saas-smarteru-tutorial/IC767830.png "Yes")
  
Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

