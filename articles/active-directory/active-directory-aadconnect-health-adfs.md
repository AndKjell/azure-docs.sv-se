
<properties 
    pageTitle="Verwenden von Azure AD Connect Health mit AD FS | Microsoft Azure" 
    description="Dies ist die Azure AD Connect Health-Seite, auf der Sie erfahren, wie Sie Ihre lokale AD FS-Infrastruktur überwachen." 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/14/2015" 
    ms.author="billmath"/>

# Verwenden von Azure AD Connect Health mit AD FS 
Die folgende Dokumentation bezieht sich auf die Überwachung Ihrer AD FS-Infrastruktur mit Azure AD Connect Health.

## Warnungen für AD FS
Im Abschnitt "Azure AD Connect Health-Warnungen" wird eine Liste der aktiven Warnungen angezeigt. Jede Warnung umfasst relevante Informationen, Lösungsschritte und Links zur verwandten Dokumentation. Durch Auswahl einer aktiven oder behobenen Warnung wird ein neues Blatt angezeigt. Dieses enthält zusätzliche Informationen, Lösungsschritte und Links zu weiterführender Dokumentation. Sie können außerdem Verlaufsdaten zu bereits behobenen Warnungen anzeigen.

Durch Auswahl einer Warnung werden zusätzliche Informationen, Lösungsschritte sowie Links zu weiterführender Dokumentation angezeigt.

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/alert2.png)



## Nutzungsanalyse für AD FS
In der Azure AD Connect Health-Nutzungsanalyse wird der Authentifizierungsdatenverkehr für Ihre Verbundserver analysiert. Bei Auswahl des Felds "Nutzungsanalyse" wird das Blatt zur Nutzungsanalyse geöffnet, in dem Metriken und Gruppierungen angezeigt werden.

>[AZURE.NOTE] Verwendungsanalyse mit AD FS verwenden möchten, müssen Sie sicherstellen, dass AD FS-Überwachung aktiviert ist. Weitere Informationen finden Sie unter [Aktivieren der Überwachung für AD FS](active-directory-aadconnect-health-operations.md#enable-auditing-for-ad-fs). 

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/report1.png)

Um zusätzliche Metriken auszuwählen, einen Zeitraum anzugeben oder die Gruppierung zu ändern, klicken Sie einfach mit der rechten Maustaste auf das Diagramm zur Nutzungsanalyse und wählen "Diagramm bearbeiten". Anschließend können Sie den gewünschten Zeitraum angeben, Metriken ändern oder auswählen und die Gruppierung ändern. Sie können die Verteilung des Authentifizierungsdatenverkehrs basierend auf verschiedenen Metriken anzeigen und jede Metrik über die relevanten Parameter gruppieren. Diese Parameter zur Gruppierung werden nachstehend beschrieben.

| Metrik | Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| ------ | -------- | -------------------------------------------- |
| Anforderungen insgesamt: Die Gesamtzahl der Anforderungen, die durch den Verbunddienst verarbeitet wurden. | Alle | Zeigt die Gesamtzahl von Anforderungen ohne Gruppierung. |
|  | Anwendung | Diese Option gruppiert die Gesamtzahl der Anforderungen basierend auf der Zielanwendung (vertrauende Seite). Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des gesamten Datenverkehrs auf die verschiedenen Anwendungen anzeigen möchten. |
|  | Server | Diese Option gruppiert die Gesamtzahl der Anforderungen basierend auf dem Server, der die Anforderung verarbeitet hat. Diese Gruppierung ist nützlich, um die Lastverteilung für den gesamten Datenverkehr anzuzeigen. |
|  | In den Arbeitsplatz eingebunden | Diese Option gruppiert die Gesamtzahl der Anforderungen basierend auf den Anforderungen, die von in den Arbeitsplatz eingebundenen Geräten eingehen (bekannt). Diese Gruppierung ist nützlich, wenn auf Ihre Ressourcen über Geräte zugegriffen wird, die der Identitätsinfrastruktur unbekannt sind. |
|  | Authentication Method (Authentifizierungsmethode) | Diese Option gruppiert die Gesamtzahl der Anforderungen basierend auf der Authentifizierungsmethode. Diese Gruppierung ist nützlich, wenn Sie wissen möchten, welche Authentifizierungsmethode in der Regel für die Authentifizierung verwendet wird. Folgendes sind die möglichen Authentifizierungsmethoden <ol> <li>Integrierte Windows-Authentifizierung (Windows)</li> <li>Formularbasierte Authentifizierung (Forms)</li> <li>SSO (einmaliges Anmelden)</li> <li>X509 certificate Authentication (Zertifikat)</li> <br>Beachten Sie, dass eine Anforderung als SSO (einmaliges Anmelden) gezählt werden, wenn der Verbundserver die Anforderung mit einem SSO-Cookie erhalten. In diesen Fällen wird, sofern es sich um ein gültiges Cookie handelt, der Benutzer nicht zur Eingabe von Anmeldeinformationen aufgefordert, sondern erhält nahtlos Zugriff auf die Anwendung. Dies ist üblich, wenn mehrere vertrauende Seiten von den Verbundservern geschützt werden. |
|  | Netzwerkadresse | Diese Option gruppiert die Gesamtzahl der Anforderungen basierend auf der Netzwerkadresse des Benutzers. Dies kann entweder ein Intranet oder Extranet sein. Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des eingehenden Datenverkehrs auf Intranet und Extranet anzeigen möchten. |
| Fehlerhafte Anforderungen insgesamt: Die Gesamtzahl der Anforderungen den Verbunddienst, bei denen Fehler aufgetreten sind. <br> (Diese Metrik ist nur verfügbar in AD FS für Windows Server 2012 R2)| Fehlertyp | Zeigt die Anzahl von Fehlern basierend auf den vordefinierten Fehlertypen. Diese Gruppierung ist nützlich, um die allgemeinen Arten von Fehlern zu verstehen. <ul><li>Falscher Benutzername oder falsches Kennwort: Fehler aufgrund falscher Benutzername oder falsches Kennwort.</li> <li>"Extranet Lockout": Fehler aufgrund der Anforderungen, die von einem Benutzer, die über extranet gesperrt wurde, empfangen </li><li> "Kennwort abgelaufen": Fehler aufgrund der Benutzer mit einem abgelaufenen Kennwort anmelden.</li><li>"Konto deaktiviert": Fehler aufgrund der Benutzer, die mit einem deaktivierten Konto anmelden.</li><li>"Geräteauthentifizierung": Fehler aufgrund von Benutzern, die nicht mithilfe des Geräte-Authentifizierung authentifiziert.</li><li>"Benutzerzertifikatauthentifizierung": Fehler aufgrund von Benutzern, die nicht aufgrund eines ungültigen Zertifikats authentifiziert.</li><li>"MFA": Fehler aufgrund der Benutzer nicht authentifiziert, Multi-Factor Authentication verwenden.</li><li>"Andere Credential": "Ausstellung Authorization": Fehler aufgrund von Fehler bei der Autorisierung.</li><li>"Ausstellung Delegation": Fehler aufgrund von Fehlern der Ausstellung-Delegierung.</li><li>"Annahme token": Fehler aufgrund von ADFS Ablehnen des Tokens aus a 3rd party Identitätsanbieter.</li><li>"Protokoll": Fehler aufgrund von Fehler.</li><li>"Unknown": Alle erfassen. Alle anderen Fehler, die nicht in die definierten Kategorien passen.</li> |
|  | Server | Gruppiert die Fehler basierend auf dem Server. Diese Gruppierung ist nützlich, um die Fehlerverteilung auf die Server anzuzeigen. Eine ungleichmäßige Verteilung könnte darauf hinweisen, dass sich ein Server in fehlerhaftem Zustand befindet. |
|  | Netzwerkadresse | Gruppiert die Fehler basierend auf der Netzwerkadresse der Anforderungen (Intranet oder Extranet). Dies ist nützlich, um die Art der Anforderungen herauszufinden, für die Fehler auftreten. |
|  | Anwendung | Gruppiert die Fehler basierend auf der Zielanwendung (vertrauende Seite). Mithilfe dieser Gruppierung können Sie anzeigen, für welche Zielanwendung die meisten Fehler auftreten. |
| Benutzeranzahl: Durchschnittliche Anzahl von aktiven Benutzern im System. | Alle | Stellt die durchschnittliche Anzahl von Benutzern bereit, die den Verbunddienst im ausgewählten Zeitraum verwendet haben. Die Benutzer werden nicht gruppiert. <br>Der Durchschnitt hängen von dem ausgewählten Zeitraum. |
|  | Anwendung | Gruppiert die durchschnittliche Anzahl von Benutzern basierend auf der Zielanwendung (vertrauende Seite). Über diese Gruppierung können Sie anzeigen, wie viele Benutzer welche Anwendung nutzen. |


## Leistungsüberwachung für AD FS
Die Azure AD Connect Health-Leistungsüberwachung liefert Überwachungsinformationen zu verschiedenen Metriken. Durch Auswahl des Felds "Überwachung" wird ein Blatt mit detaillierten Informationen zu den Metriken geöffnet.


![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/perf1.png)


Wenn Sie die Filteroption oben im Blatt auswählen, können Sie eine Filterung nach Server vornehmen, um Metriken zu einzelnen Servern anzuzeigen. Um Metriken zu ändern, klicken Sie einfach mit der rechten Maustaste auf das Überwachungsdiagramm unterhalb des Überwachungsblatts, und klicken Sie dann auf "Diagramm bearbeiten". Im dann geöffneten neuen Blatt können Sie zusätzliche Metriken aus der Dropdownliste auswählen und einen Zeitraum für die Anzeige von Leistungsdaten angeben.




## Verwandte Links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installieren des Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health – FAQ](active-directory-aadconnect-health-faq.md)


