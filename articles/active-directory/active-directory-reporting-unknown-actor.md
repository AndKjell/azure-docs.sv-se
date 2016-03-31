<properties
   pageTitle="Azure Active Directory-Berichtsereignisse „Unbekannter Akteur“ | Microsoft Azure"
   description="Beschreibung des Ereignisses „Unbekannter Akteur“ in Azure Active Directory-Berichten"
   services="active-directory"
   documentationCenter=""
   authors="kenhoff"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/07/2015"
   ms.author="kenhoff"/>

# Azure Active Directory-Berichtsereignisse „Unbekannter Akteur“

*Diese Dokumentation ist Teil der [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

In seltenen Fällen kann es vorkommen, dass Sie in Azure AD-Berichten ungewöhnliche Werte in den Feldern für „Akteur“ oder „Benutzer“ sehen. Dieses Verhalten ist normal und wird von einem der beiden folgenden Ereignisse verursacht:

## Einen Dienstprinzipal führt ohne einen Benutzerkontext Aktionen im Verzeichnis aus.

In diesem Fall führt ein Dienstprinzipal (Anwendung) Verzeichnisaktualisierungen durch, ohne sich tatsächlich als Benutzer anzumelden. Dies bewirkt, dass als Akteur (engl. „Actor“) anstatt eines UPN die Dienstprinzipal-ID angezeigt wird. Hier sehen Sie ein Beispiel:

![](./media/active-directory-reporting-unknown-actor/spd-actor.png)

Dies ist ein bekanntes Problem, an dessen Lösung wir intensiv arbeiten.

## Ein Benutzer wurde vor der Verarbeitung des Ereignisses aus dem Verzeichnis gelöscht.

In diesem Fall wurde ein Benutzer aus dem Verzeichnis gelöscht, bevor wir das Ereignis verarbeitet und ihm einen Benutzernamen zugeordnet hatten. Hier sehen Sie ein Beispiel:

![](./media/active-directory-reporting-unknown-actor/unknown-actor.png)

Dies ist ein bekanntes Problem, an dessen Lösung wir intensiv arbeiten.

<!-- ![](./media/active-directory-reporting-unknown-actor/uid-actor.png) -->


