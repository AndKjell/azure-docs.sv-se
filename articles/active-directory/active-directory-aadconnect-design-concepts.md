<properties
   pageTitle="Entwurfskonzepte für Azure AD Connect | Microsoft Azure"
   description="In diesem Thema werden bestimmte Aspekte des Implementierungsentwurfs beschrieben."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="12/02/2015"
   ms.author="andkjell"/>

# Entwurfskonzepte für Azure AD Connect
Dieses Themas beschreibt, welche Aspekte bei der Planung der Implementierung von Azure AD Connect berücksichtigt werden müssen. Bestimmte Aspekte werden hier sehr gründlich behandelt, und diese Konzepte werden in anderen Themen auch kurz beschrieben.

## sourceAnchor
Das Attribut "sourceanchor" ist definiert als *ein Attribut während der Lebensdauer eines Objekts unveränderliche*. Es identifiziert ein Objekt eindeutig als in Azure AD und lokal identisch. Das Attribut nennt man auch **ImmutableId** und die beiden Namen austauschbar verwendet werden.

Das Wort "unveränderlich" ist in diesem Thema wichtig. Da der Wert dieses Attributs nicht geändert werden kann, nachdem es festgelegt wurde, ist es wichtig, einen Entwurf auszuwählen, der Ihr Szenario unterstützt.

Das Attribut wird für die folgenden Szenarien verwendet:

- Wenn Sie einen neuen Synchronisierungsmodulserver erstellen oder nach einem Notfallwiederherstellungsszenario neu erstellen, verknüpft dieses Attribut vorhandene Objekte in Azure AD mit lokalen Objekten.
- Wenn Sie von einer ausschließlichen Cloudidentität zu einem synchronisierten Identitätsmodell wechseln, ermöglicht dieses Attribut die genaue Übereinstimmung vorhandener Objekte in Azure AD mit lokalen Objekten.
- Wenn Sie Verbund verwenden, dieses Attribut zusammen mit den **UserPrincipalName** wird im Anspruch zur eindeutigen Identifizierung ein Benutzers verwendet.

In diesem Thema wird nur sourceAnchor behandelt, da es sich auf Benutzer bezieht. Die gleichen Regeln gelten für alle Objekttypen, doch üblicherweise ist dies nur für Benutzer von besonderem Belang.

### Auswählen eines guten Attributs sourceAnchor
Der Attributwert muss den folgenden Regeln entsprechen:

- Weniger als 60 Zeichen lang
- Ein Sonderzeichen nicht enthalten: & #92;! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
- Global eindeutig
- Zeichenfolge, Ganzzahl oder Binärzahl
- Sollte nicht auf einem Benutzernamen beruhen, da dieser geändert werden kann
- Groß-/Kleinschreibung sollte nicht relevant sein und Werte, die sich nach Groß-/Kleinschreibung unterscheiden, sollten vermieden werden
- Sollte bei Erstellung des Objekts zugewiesen werden


Ist das ausgewählte Attribut sourceAnchor nicht vom Typ Zeichenfolge, unterzieht Azure AD Connect den Wert des Attributs einem Base64Encode-Prozess, um sicherzustellen, dass keine Sonderzeichen angezeigt werden. Wenn Sie einen andere Verbundserver als ADFS verwenden, stellen Sie sicher, dass Ihr Server auch in der Lage ist, das Attribut einem Base64Encode-Prozess zu unterziehen.

Beim Attribut sourceAnchor wird die Groß-/Kleinschreibung berücksichtigt. Der Wert "JohnDoe" ist nicht identisch mit "johndoe".

Ist eine einzige Gesamtstruktur lokale dann ist das Attribut, das Sie verwenden sollten **ObjectGUID**. Dieses Attribut wird auch bei der Verwendung von Expresseinstellungen in Azure AD Connect und von DirSync verwendet.

Wenn Sie über mehrere Gesamtstrukturen verfügen, und Sie Benutzer zwischen Gesamtstrukturen und Domänen in derselben Gesamtstruktur, dann nicht verschieben **ObjectGUID** ist ein gutes Attribut selbst in diesem Fall verwendet.

Wenn Sie Benutzer zwischen Gesamtstrukturen und Domänen verschieben, müssen Sie ein Attribut finden, das nicht geändert wird oder während des Verschiebevorgangs zusammen mit den Benutzern verschoben werden kann. Ein empfohlenes Verfahren ist die Einführung eines synthetischen Attributs. Ein Attribut, das etwas Ähnliches wie eine GUID enthält, wäre geeignet. Beim Erstellen des Objekts wird eine neue GUID erstellt und dem Benutzer zugewiesen. Eine benutzerdefinierte Regel kann erstellt werden, in dem Modul Synchronisierungsserver erstellen Sie diesen Wert auf Basis der **ObjectGUID** und das ausgewählte Attribut in ADDS aktualisieren. Wenn Sie das Objekt verschieben, stellen Sie sicher, dass Sie auch den Inhalt dieses Werts kopieren.

Eine andere Lösung ist, ein vorhandenes Attribut zu wählen, von dem Sie wissen, dass es nicht geändert wird. Häufig verwendete Attribute enthalten **EmployeeID**. Wenn Sie ein Attribut in Betracht ziehen, das Buchstaben enthält, stellen Sie sicher, dass keine Möglichkeit besteht, dass sich die Groß-/Kleinschreibung für den Wert des Attributs ändern kann. Zu schlechten Attributen, die nicht verwendet werden sollten, gehören solche mit dem Namen des Benutzers. Durch Hochzeit oder Scheidung könnte sich der Name ändern, und dies ist für dieses Attribut nicht zulässig. Dies ist auch ein Grund, warum Attribute wie **UserPrincipalName**, **Mail**, und **TargetAddress** ist auch möglich, wählen Sie im Azure AD Connect-Installations-Assistenten. Diese Attribute enthalten außerdem das @-Zeichen, das in sourceAnchor nicht zulässig ist.

### Ändern des Attributs sourceAnchor
Der Wert des Attributs sourceAnchor kann nicht geändert werden, nachdem das Objekt in Azure AD erstellt und die Identität synchronisiert wurde.

Aus diesem Grund gelten die folgenden Einschränkungen für Azure AD Connect:

- Das Attribut sourceAnchor kann nur bei der Erstinstallation festgelegt werden. Diese Option ist schreibgeschützt, wenn Sie den Installations-Assistenten erneut ausführen. Wenn Sie diese Einstellung ändern müssen, müssen Sie deinstallieren und neu installieren.
- Wenn Sie einen anderen Azure AD Connect-Server installieren, müssen Sie das gleiche Attribut sourceAnchor wie zuvor auswählen. Wenn Sie haben zuvor DirSync und in Azure AD Connect verschieben, verwenden Sie **"objectGUID"** da, die das Attribut, das von DirSync verwendet wird.
- Wenn der Wert für sourceAnchor geändert wird, nachdem das Objekt nach Azure AD exportiert wurde, meldet die Azure AD Connect-Synchronisierung einen Fehler, und lässt keine weiteren Änderungen am Objekt zu, bevor das Problem behoben und die Änderung von sourceAnchor im Quellverzeichnis wieder rückgängig gemacht worden ist.

## Nächste Schritte
Erfahren Sie mehr über [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).


