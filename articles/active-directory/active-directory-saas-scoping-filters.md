<properties
    pageTitle="Attributbasierte App-Bereitstellung mit Bereichsfiltern | Microsoft Azure"
    description="Erfahren Sie, wie Sie mithilfe von Bereichsfiltern verhindern, dass Objekte in Apps mit automatisierter Benutzerbereitstellung auch dann bereitgestellt werden, wenn ein Objekt Ihre Geschäftsanforderungen nicht erfüllt."
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
    ms.date="10/20/2015"
    ms.author="markusvi"/>


# Attributbasierte App-Bereitstellung mit Bereichsfiltern

In diesem Abschnitt wird die Verwendung von Bereichsfiltern zum Definieren attributbasierter Regeln beschrieben, die festlegen, welche Benutzer der Anwendung bereitgestellt werden.





## Klauseln und Bereichsgruppen


![Bereichsfilter][1] 




Bereichsfilter werden durch eine oder mehrere definiert **Bereichsgruppen**, die jeweils eine oder mehrere **Klauseln**. Um die Klauseln für eine bestimmte Bereichsgruppe anzuzeigen, erweitern Sie sie durch Klicken auf den Pfeil links neben dem Gruppennamen.

Ein **Klausel** bestimmt, welche Benutzer die Bereichsfilter passieren, durch die Auswertung der Attribute der einzelnen Benutzer zulässig sind. Wenn beispielsweise eine Klausel erfordert, dass das State-Attribut eines Benutzers "New York" lauten muss, werden nur Ihre Benutzer in New York in der Anwendung bereitgestellt.

![Bereichsgruppenname][2] 



Jede **Bereichsgruppe** beginnt mit einer obligatorischen **Klausel**, wie im Screenshot oben gezeigt. Diese Klausel gibt einfach an, dass der Benutzer zunächst der Anwendung zugewiesen werden muss, bevor sie durch die Bereichsfilter ausgewertet wird. Diese Klausel kann nicht gelöscht oder geändert werden.

Sie können neue Klauseln oder neue Bereichsgruppen durch Wählen der entsprechenden Schaltfläche hinzufügen. Sie können jeder Bereichsgruppe einen Namen zuweisen, durch Bearbeiten der **Bereichsgruppenname** Eigenschaft.





## Auswerten von Bereichsfiltern

Während der Bereitstellung testen wir jeden zugewiesenen Benutzer mithilfe der Bereichsfilter, um festzustellen, ob dieser Benutzer auf die Anwendung zugreifen darf. Stellen Sie sich jede Klausel als Test vor, der bestanden werden muss, damit der Benutzer nicht herausgefiltert wird. 

Wenn Sie mehrere Bereichsgruppen definiert haben, muss jeder Benutzer sich für mindestens eine von ihnen qualifizieren, um auf die Anwendung zuzugreifen. Innerhalb jeder Bereichsgruppe muss der Benutzer jedoch jede einzelne Klausel bestehen, um sich für diese spezifische Bereichsgruppe zu qualifizieren. 

Mit anderen Worten, Sie können sich Bereichsgruppen als mit OR verknüpft vorstellen, wobei die darin enthaltenen Klauseln mit AND verknüpft sind. Betrachten Sie beispielsweise den folgenden Bereichsfilter:


![Bereichsgruppenname][2]  


Gemäß diesem Bereichsfilter müssen Benutzer die folgenden Kriterien erfüllen, um bereitgestellt werden zu können:

1. Sie müssen der Anwendung zugewiesen sein.

2. Sie müssen in der Engineeringabteilung arbeiten.

3. Sie müssen entweder in San Francisco oder in Kanada arbeiten.


[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png


