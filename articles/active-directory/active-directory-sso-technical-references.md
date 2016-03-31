<properties
   pageTitle="Dokumentbibliothek zum Verwalten von Anwendungen | Microsoft Azure"
   description="Themen zur Azure Active Directory-Anwendungsverwaltung mit technischen Referenzen zu Vorgehensweisen, Problembehebung und häufig gestellte Fragen"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/03/2015"
   ms.author="kgremban"/>

# Dokumentbibliothek zum Verwalten von Anwendungen

Diese Seite enthält hilfreiche Links für die wichtigsten Features für einmaliges Anmelden mit Azure Active Directory. Die Links sind in die folgenden Kategorien gruppiert:

- **Übersicht:** eine Übersicht über das Feature, mit der Anwendungsfälle und Thema Einführung.
- **Gewusst wie:** detaillierte Anleitung zum Einstieg in bestimmten Szenarien.
- **Problembehandlung:** Tipps für häufig auftretende Probleme zu diagnostizieren.
- **Häufig gestellte Fragen:** für typische Fragen und Probleme beantwortet.  

## Cloud-App-Ermittlung

|   |   |
| ------ | ------ |
| Übersicht | [Wie ermittle ich nicht genehmigte Cloud-Apps, die in meiner Organisation verwendet werden?](active-directory-cloudappdiscovery-whatis.md) |
| Vorgehensweise | [Erste Schritte mit Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx) <br><br> [Cloud App Discovery Aspekte der Sicherheit und Datenschutz](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) <br><br> [Cloud App Discovery Group Policy-Bereitstellungshandbuch](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx) <br><br> [Cloud App Discovery System Center-Bereitstellungshandbuch](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx) <br><br> [Cloud App Discovery-registrierungseinstellungen für Proxydienste](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| Häufig gestellte Fragen | [Cloud App Discovery – Häufig gestellte Fragen](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |

## App-Zuweisung

|   |   |
| ------ | ------ |
| Übersicht | [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md) |
| Vorgehensweise | [Erstellen einer einfachen Regel zum Konfigurieren von dynamischen Mitgliedschaften für eine Gruppe](active-directory-accessmanagement-simplerulegroup.md) <br><br> [Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen](active-directory-accessmanagement-group-saasapps.md) <br><br> [Einrichten von Azure AD zur Self-Service-Verwaltung des Anwendungszugriffs](active-directory-accessmanagement-self-service-group-management.md) <br><br> [Verwalten von Besitzern einer Gruppe](active-directory-accessmanagement-managing-group-owners.md) <br><br> [Verwalten von Sicherheitsgruppen in Azure Active Directory](active-directory-accessmanagement-manage-groups.md) <br><br> [Dedizierte Gruppen in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md) <br><br> [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md) <br><br> [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md) <br><br> [Liste der Tutorials zur Integration von SaaS-apps mit Azure Active Directory](active-directory-saas-tutorial-list.md) |

## Einmalige Verbundanmeldung

|   |   |
| ------ | ------ |
| Übersicht |[Konfigurieren des Verbunds mit AD FS](active-directory-aadconnect-get-started-custom.md)
| Vorgehensweise | [DirSync mit einmaliges Anmelden](https://msdn.microsoft.com/library/azure/dn441213.aspx) <br><br> [Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory](https://azure.microsoft.com/blog/windows-azure-now-supports-federation-with-windows-server-active-directory/) <br><br> [Liste der Tutorials zur Integration von SaaS-apps mit Azure Active Directory](active-directory-saas-tutorial-list.md) <br><br> [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/) |
| Problembehandlung | [Problembehandlung bei Azure AD DirSync-Tool Konfigurations-Assistenten](http://social.technet.microsoft.com/wiki/contents/articles/19100.troubleshooting-azure-ad-dirsync-tool-configuration-wizard-failed-to-get-address-for-method-createidentityhandle2.aspx) <br><br> [Problembehandlung bei Azure Active Directory Sync Tool-Installationsordner und den Konfigurations-Assistenten (Fehler)](https://support.microsoft.com/kb/2684395) <br><br> [Problembehandlung bei Directory-Synchronisierung](https://msdn.microsoft.com/library/azure/jj151787.aspx) <br><br> [Problembehandlung für einmaliges Anmelden](https://msdn.microsoft.com/library/azure/jj151834.aspx) |
| Häufig gestellte Fragen | [Verwalten von Zertifikaten für Verbund einmaliges Anmelden in Azure Active Directory](active-directory-sso-certs.md) <br><br> [Häufig gestellte Fragen zu Azure Active Directory Connect](active-directory-aadconnect-faq.md) |

## Einmaliges Anmelden per Kennwort

|   |   |
| ------ | ------ |
| Vorgehensweise | [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md) <br><br> [DirSync mit Kennwortsynchronisierung](https://msdn.microsoft.com/library/azure/dn441214.aspx) |
| Problembehandlung | [Problembehandlung bei Azure AD DirSync-Tool Konfigurations-Assistenten](http://social.technet.microsoft.com/wiki/contents/articles/19100.troubleshooting-azure-ad-dirsync-tool-configuration-wizard-failed-to-get-address-for-method-createidentityhandle2.aspx) <br><br> [Problembehandlung bei Azure Active Directory Sync Tool-Installationsordner und den Konfigurations-Assistenten (Fehler)](https://support.microsoft.com/kb/2684395) <br><br> [Problembehandlung bei Directory-Synchronisierung](https://msdn.microsoft.com/library/azure/jj151787.aspx) |
| Häufig gestellte Fragen | [Häufig gestellte Fragen zu Azure Active Directory Connect](active-directory-aadconnect-faq.md) |

## Bereitstellung umfangreicher Apps

|   |   |
| ------ | ------ |
| Übersicht | [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md) |
| Vorgehensweise | [Tutorial: Azure Active Directory-Integration mit Concur](active-directory-saas-concur-tutorial.md) <br><br> [Tutorial: Azure Active Directory-Integration mit DocuSign](active-directory-saas-docussign-tutorial.md) <br><br> [Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen](active-directory-saas-dropboxforbusiness-tutorial.md) <br><br> [Tutorial: Integrieren von Google Apps in Azure Active Directory](active-directory-saas-google-apps-tutorial.md) <br><br> [Tutorial: Integrieren von Salesforce in Azure Active Directory](active-directory-saas-salesforce-tutorial.md) <br><br> [Tutorial: Azure Active Directory-Integration mit ServiceNow](active-directory-saas-servicenow-tutorial.md) <br><br> [Lernprogramm: Konfigurieren von Workday für eingehende Synchronisierung](active-directory-saas-workday-inbound-tutorial.md) |

## Azure AD-Anwendungskatalog

|   |   |
| ------ | ------ |
| Vorgehensweise | [Ihre Anwendung im Azure Active Directory-Anwendungskatalog auflisten](active-directory-app-gallery-listing.md) <br><br> [„Bring your own app“ mit Azure AD-Konfiguration für Self-Service SAML](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx) |

## Benutzerdefinierte Apps

|   |   |
| ------ | ------ |
| Übersicht | [„Bring your own app“ mit Azure AD-Konfiguration für Self-Service SAML](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx) |
| Vorgehensweise | [Tutorial: Integrieren von Salesforce in Azure Active Directory](active-directory-saas-salesforce-tutorial.md) |

## Anwendungsproxy

|   |   |
| ------ | ------ |
| Übersicht | [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md) |
| Vorgehensweise | [Aktivieren des Azure AD-Anwendungsproxys](active-directory-application-proxy-enable.md) <br><br> [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](active-directory-application-proxy-publish.md) <br><br> [Arbeiten mit bedingtem Zugriff](active-directory-application-proxy-conditional-access.md) <br><br> [Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy](active-directory-application-proxy-custom-domains.md) <br><br> [Arbeiten mit Ansprüche unterstützenden Apps im Anwendungsproxy](active-directory-application-proxy-claims-aware-apps.md) <br><br> [Einmaliges Anmelden mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md) |
| Problembehandlung | [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md) |

## Starten der App

|   |   |
| ------ | ------ |
| Übersicht | [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) |
| Vorgehensweise | [Azure Active Directory Zugriffsbereich - EMS](http://blogs.msdn.com/b/haddy_el-haggan_blog/archive/2015/04/02/azure-active-directory-access-panel-ems.aspx) <br><br> [Integration in Azure Active Directory](active-directory-how-to-integrate.md) |


