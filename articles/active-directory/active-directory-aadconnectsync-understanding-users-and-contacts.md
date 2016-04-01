<properties
    pageTitle="Azure AD Connect-Synchronisierung: Grundlegendes zu Benutzern und Kontakten | Microsoft Azure"
    description="Erläutert Benutzer und Kontakte in der Azure AD Connect-Synchronisierung."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/09/2015"
    ms.author="markusvi;andkjell"/>


# Azure AD Connect-Synchronisierung: Grundlegendes zu Benutzern und Kontakten

Es gibt verschiedene Gründe, weshalb Sie möglicherweise über mehrere Active Directory-Gesamtstrukturen verfügen, und es gibt eine Reihe unterschiedlicher Bereitstellungstopologien. Häufige Modelle umfassen eine Kontoressourcenbereitstellung und per GAL synchronisierte Gesamtstrukturen nach einer Unternehmensfusion oder -übernahme. Es gibt zwar reine Modelle, Hybridmodelle sind jedoch ebenfalls häufig vorhanden. Die Standardkonfiguration der Azure AD Connect-Synchronisierung geht von keinem bestimmten Modell aus. Es können jedoch auf Basis des im Installationshandbuch ausgewählten Benutzerabgleichs unterschiedliche Verhaltensweisen beobachtet werden.

In diesem Thema wird erläutert, wie sich die Standardkonfiguration in bestimmten Topologien verhält. Die Konfiguration wird durchlaufen, und der Synchronisierungsregel-Editor kann verwendet werden, um die Konfiguration anzuzeigen.

Die Konfiguration geht von einigen wenigen allgemeinen Regeln aus:

- Unabhängig davon, in welcher Reihenfolge die Active Directory-Quellen importiert werden, sollte das Endergebnis immer identisch sein.
- Ein aktives Konto trägt immer Anmeldeinformationen, einschließlich **UserPrincipalName** und **SourceAnchor**.
- Ein deaktiviertes Konto trägt "userPrincipalName" und "sourceAnchor" bei (falls kein aktives Konto gefunden wurde), es sei denn, es handelt sich um ein verknüpftes Postfach.
- Ein Konto mit einem verknüpften Postfach wird niemals für "userPrincipalName" und "sourceAnchor" verwendet. Es wird angenommen, dass später ein aktives Konto gefunden wird.
- Ein Kontaktobjekt wird Azure AD möglicherweise als Kontakt oder Benutzer bereitgestellt. Sie werden dies erst dann genau wissen, wenn alle Active Directory-Quellgesamtstrukturen verarbeitet wurden.

## Kontakte

Nach einer Unternehmensfusion oder -übernahme kommt es häufig vor, dass Kontakte einen Benutzer in einer anderen Gesamtstruktur darstellen und eine GALSynch-Lösung zwei oder mehr Exchange-Gesamtstrukturen verbindet. Das Kontaktobjekt wird immer mithilfe des Attributs "mail" vom Connectorbereich mit dem Metaverse verknüpft. Wenn bereits ein Kontakt- oder Benutzerobjekt mit derselben E-Mail-Adresse vorhanden ist, werden die Objekte miteinander verknüpft. Dies wird in der Regel konfiguriert **In from AD – Contact Join**. Es gibt auch eine Regel namens **In from AD – Contact Common** mit einem Attributfluss zum Metaverse-Attribut **Quellobjekttyp** mit der Konstanten **Kontakt**. Diese Regel hat eine sehr geringe Rangfolge, wenn ein Benutzerobjekt mit demselben Metaverse-Objekt, und klicken Sie dann auf die Regel verknüpft ist **In from AD – User Common** wird den Wert dieses Attributs Benutzer beitragen. Bei dieser Regel weist dieses Attribut den Wert "Contact" auf, wenn kein Benutzer verknüpft wurde, und den Wert "User", wenn mindestens ein Benutzer gefunden wurde.

Für die Bereitstellung eines Objekts in Azure AD, die ausgehende Regel **Out to AAD – Contact Join** erstellt ein Kontaktobjekt, wenn das Metaverse-Attribut **Quellobjekttyp** Wert **wenden Sie sich an**. Wenn dieses Attribut, um festgelegt wird **Benutzer**, klicken Sie dann auf die Regel **Out to AAD – User Join** erstellt stattdessen ein Benutzerobjekt.
Es ist möglich, dass ein Objekt von "Contact" zu "User" aufsteigt, wenn weitere Active Directory-Quellen importiert und synchronisiert werden.

In einer GALSync-Topologie finden sich beispielsweise Kontaktobjekte für alle Benutzer in der zweiten Gesamtstruktur, wenn die erste Gesamtstruktur importiert wird. Dadurch werden neue Kontaktobjekte im Azure AD-Connector bereitgestellt.. Wenn die zweite Gesamtstruktur später importiert und synchronisiert wird, werden die wirklichen Benutzer gefunden und mit den vorhandenen Metaverseobjekten verbunden. Anschließend wird das Kontaktobjekt in Azure AD gelöscht und stattdessen ein neuer Benutzer erstellt.

Wenn Sie über eine Topologie verfügen, in der Benutzer als Kontakte dargestellt werden, stellen Sie sicher, dass Sie im Installationshandbuch die Option zum Abgleich der Benutzer mit dem mail-Attribut auswählen. Wenn Sie eine andere Option auswählen, haben Sie eine Konfiguration, die von der Reihenfolge abhängig ist. Kontaktobjekte werden immer mit dem mail-Attribut verknüpft. Benutzerobjekte werden nur dann mit dem mail-Attribut verknüpft, wenn diese Option im Installationshandbuch ausgewählt wurde. Dies könnte zwei unterschiedliche Objekte im Metaverse mit demselben mail-Attribut zur Folge haben, wenn das Kontaktobjekt vor dem Benutzerobjekt importiert wurde. Während des Exports nach Azure AD wird ein Fehler ausgegeben. Dieses Verhalten ist beabsichtigt und weist auf ungültige Daten hin oder darauf, dass die Topologie während der Installation nicht richtig ermittelt wurde.

## Deaktivierte Konten

Deaktivierte Konten werden auch mit Azure AD synchronisiert. Deaktivierte Konten werden häufig zum Darstellen von Ressourcen in Exchange verwendet, beispielsweise von Konferenzräumen. Die Ausnahme bilden Benutzer mit einem verknüpften Postfach. Diese stellen, wie dies bereits zuvor erwähnt wurde, niemals ein Konto für Azure AD bereit.

Folgende Annahme gilt: Wenn ein deaktiviertes Benutzerkonto gefunden wird, wird später kein anderes aktives Konto gefunden, und das Objekt wird Azure AD mit den gefundenen Werten für "userPrincipalName" und "sourceAnchor" bereitgestellt. Sollte ein anderes aktives Konto eine Verknüpfung mit demselben Metaverseobjekt herstellen, werden "userPrincipalName" und "sourceAnchor" dieses Kontos verwendet.

## Ändern von "sourceAnchor"

Nachdem ein Objekt nach Azure AD exportiert wurde, kann der Wert für "sourceAnchor" nicht mehr geändert werden. Wenn das Objekt wurde exportiert das Metaverse-Attribut **CloudSourceAnchor** festgelegt ist, mit der **SourceAnchor** Wert von Azure AD akzeptiert. Wenn **SourceAnchor** geändert wird und nicht übereinstimmen **CloudSourceAnchor**, die Regel **Out to AAD – User Join** löst den Fehler **SourceAnchor-Attribut wurde geändert**. In diesem Fall müssen Konfiguration oder Daten korrigiert werden, sodass derselbe sourceAnchor-Wert wieder im Metaverse vorhanden ist, bevor das Objekt erneut synchronisiert werden kann.

## Zusätzliche Ressourcen

* [Azure AD Connect-Synchronisierung: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)


