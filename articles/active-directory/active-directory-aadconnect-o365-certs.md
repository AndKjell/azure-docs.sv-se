<properties
    pageTitle="Anleitungen für die Zertifikaterneuerung für Office 365- und Azure AD-Benutzer. | Microsoft Azure"
    description="In diesem Artikel wird für Office 365-Benutzer erläutert, wie Probleme mit E-Mails behoben werden, die sie zum Erneuern eines Zertifikats auffordern."
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
    ms.topic="article"
    ms.date="10/13/2015"
    ms.author="billmath"/>


# Erneuern von Verbundzertifikaten für Office 365 und Azure AD

Wenn Sie eine E-Mail oder eine Portalbenachrichtigung erhalten haben, in der Sie aufgefordert werden, Ihr Zertifikat für Office 365 zu erneuern, lesen Sie diesen Artikel, um das Problem zu beheben und zu verhindern, dass es erneut auftritt.  Dieser Artikel setzt voraus, dass Sie AD FS als Verbundserver verwenden.

## Überprüfen, ob Schritte erforderlich sind

Wenn Sie AD FS 2.0 oder höher verwenden, aktualisieren Office 365 und Azure AD das Zertifikat automatisch, bevor es abläuft.  Sie müssen keine manuellen Schritte ausführen bzw. kein Skript als geplante Aufgabe ausführen.  Damit dies funktioniert, müssen die beiden folgenden AD FS-Konfigurationseinstellungen wirksam sein:

- Die AD FS-Eigenschaft "AutoCertificateRollover" muss auf "True" festgelegt sein. Damit wird angegeben, dass AD FS automatisch neue Tokensignatur- und Tokenverschlüsselungszertifikate generiert, bevor die alten ablaufen.
    - Wenn der Wert "False" ist, verwenden Sie benutzerdefinierte Zertifikateinstellungen.  Wechseln Sie [hier](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)  finden Sie umfassende Anleitungen.
- Die Verbundmetadaten müssen für das öffentliche Internet verfügbar sein.

    So wird dies überprüft:

    - Überprüfen Sie durch Ausführen des folgenden Befehls in einem PowerShell-Befehlsfenster auf dem primären Verbundserver, ob die AD FS-Installation den automatischen Zertifikat-Rollover verwendet:

    `PS C:\> Get-ADFSProperties`

(Beachten Sie, dass Sie bei Verwendung von AD FS 2.0 zuerst "Add-Pssnapin Microsoft.Adfs.Powershell" ausführen müssen.)
else.

Überprüfen Sie, ob die Verbundmetadaten öffentlich zugänglich sind, indem Sie von einem Computer im öffentlichen Internet (außerhalb des Unternehmensnetzwerks) zur folgenden URL navigieren:


https:// (Your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

Dabei wird `(your_FS_name) ` durch den Verbunddiensthostnamen ersetzt, den Ihre Organisation verwendet, z. B. "fs.contoso.com".  Wenn Sie diese beiden Einstellungen erfolgreich überprüfen können, müssen Sie nichts weiter tun.  

Beispiel: https://fs.contos.com/federationmetadata/2007-06/federationmetadata.xml

## Wenn die AutoCertificateRollover-Eigenschaft auf "False" festgelegt ist

Wenn die AutoCertificateRollover-Eigenschaft auf "False" festgelegt ist, verwenden Sie keine standardmäßigen AD FS-Zertifikateinstellungen.  Die häufigste Ursache hierfür ist, dass Ihre Organisation AD FS-Zertifikate verwaltet, die von einer Organisationszertifizierungsstelle registriert werden.  In diesem Fall müssen Sie Ihre Zertifikate selbst erneuern und aktualisieren.  Verwenden Sie den Leitfaden [hier](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

## Wenn Ihre Metadaten nicht öffentlich zugänglich sind
Wenn die AutocertificateRollover-Einstellung "True" ist, aber die Verbundmetadaten nicht öffentlich verfügbar sind, verwenden Sie das folgende Verfahren, um sicherzustellen, dass Ihre Zertifikate sowohl lokal als auch in der Cloud aktualisiert werden:

### Stellen Sie sicher, dass Ihr AD FS-System ein neues Zertifikat generiert hat.

- Stellen Sie sicher, dass Sie am primären AD FS-Server angemeldet sind.
- Überprüfen Sie die aktuellen Signaturzertifikate in AD FS, indem Sie ein PowerShell-Befehlsfenster öffnen und den folgenden Befehl ausführen:

`PS C:\>Get-ADFSCertificate –CertificateType token-signing.`

(Beachten Sie, dass Sie bei Verwendung von AD FS 2.0 zuerst "Add-Pssnapin Microsoft.Adfs.Powershell" ausführen müssen.)


- Sehen Sie sich in der Ausgabe des Befehls die aufgeführten Zertifikate an.  Wenn AD FS ein neues Zertifikat generiert hat, sollten Sie zwei Zertifikate in der Ausgabe sehen: eine für die der IsPrimary-Wert "true" und das NotAfter-Datum ist innerhalb von 5 Tagen und eine für die IsPrimary ist "false" und nicht nach ungefähr einem Jahr in der Zukunft.

- Wenn nur ein Zertifikat angezeigt wird und das NotAfter-Datum innerhalb von 5 Tagen ist, müssen Sie ein neues Zertifikat generieren, indem Sie die folgenden Schritte ausführen.

- Um ein neues Zertifikat zu generieren, führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

- Überprüfen Sie die Aktualisierung durch den folgenden Befehl erneut ausführen: PS C:\ > Get-ADFSCertificate – CertificateType Token-signing
- Um die Eigenschaften der Office 365-Verbundvertrauensstellung manuell zu aktualisieren, gehen Sie folgendermaßen vor.

Nun sollten zwei Zertifikate aufgeführt werden, bei denen eines ein NotAfter-Datum in etwa einem Jahr in der Zukunft hat und bei dem der IsPrimary-Wert "False" ist.


### Um die Eigenschaften der Office 365-Verbundvertrauensstellung manuell zu aktualisieren, gehen Sie folgendermaßen vor.

1.  Öffnen Sie das Microsoft Azure Active Directory-Modul für Windows PowerShell.
2.  Führen Sie $cred=Get-Credential aus. Wenn Sie dieses Cmdlet zur Eingabe von Anmeldeinformationen auffordert, geben Sie die Anmeldeinformationen Ihres Clouddienstadministrators ein.
3.  Führen Sie Connect-MsolService –Credential $cred aus. Dieses Cmdlet verbindet Sie mit dem Clouddienst. Sie müssen einen Kontext erstellen, der Sie mit dem Clouddienst verbindet, bevor Sie andere Cmdlets ausführen, die vom Tool installiert werden.
4.  Wenn Sie diese Befehle auf einem Computer, der nicht der primäre AD FS-Verbundserver ist ausführen, führen Sie Set-MSOLAdfscontext-Computer <AD FS primary server>, wobei <AD FS primary server> wird von der interne FQDN des primären AD FS-Servers. Dieses Cmdlet erstellt einen Kontext, der Sie mit AD FS verbindet.
5.  Ausführen von Update-MSOLFederatedDomain – DomainName <domain>. Dieses Cmdlet aktualisiert die Einstellungen von AD FS im Clouddienst und konfiguriert die Vertrauensstellung zwischen beiden.

>[AZURE.NOTE] Wenn Sie mehrere Domänen der obersten Ebene, z. B. contoso.com und fabrikam.com, unterstützen müssen, müssen Sie den Schalter SupportMultipleDomain mit allen Cmdlets verwenden. Weitere Informationen finden Sie unter "Unterstützen mehrerer Domänen auf oberster Ebene".
Stellen Sie abschließend sicher, alle Webanwendungsproxy-Server werden mit aktualisiert [Windows Server Mai 2014](http://support.microsoft.com/kb/2955164) Rollup, andernfalls die Proxys möglicherweise nicht selbst mit dem neuen Zertifikat aktualisieren was zu einem Ausfall führt.


