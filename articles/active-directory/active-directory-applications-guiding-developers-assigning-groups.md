<properties
    pageTitle="Azure AD und Anwendungen: Zuweisen von Gruppen zu einer Anwendung | Microsoft Azure"
    description="Informationen zum Implementieren der Zuweisung von Gruppen für Azure-Anwendungen"
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="inhenk"/>

# Azure AD und Anwendungen: Zuweisen von Gruppen zu einer Anwendung
Bevor Sie einer Anwendung Benutzer und Gruppen zuweisen können, müssen Sie die Benutzerzuweisung erfordern.  Informationen zum erfordern von Benutzerrechten finden Sie unter der [Benutzerrechten erfordern](active-directory-applications-guiding-developers-requiring-user-assignment.md) Artikel.

In diesem Artikel wird davon ausgegangen, dass Sie bereits Gruppen in Active Directory für diese Anwendung erstellt haben.

## Zuweisen von Gruppen zu einer Anwendung
1. Melden Sie sich beim Azure-Portal mit dem Administratorkonto an.
2. Klicken Sie auf die **alle Elemente** Element im Hauptmenü.
3. Wählen Sie das Verzeichnis aus, das Sie für die Anwendung verwenden.
4. Klicken Sie auf die **APPLIKATIONEN** Registerkarte.
5. Wählen Sie die Anwendung aus der Liste der Programme aus, die diesem Verzeichnis zugeordnet sind.
6. Klicken Sie auf die **Benutzer und Gruppen** Registerkarte.
7. Filtern Sie die Liste der Gruppen in active Directory mithilfe der **Gruppen** Dropdown-Liste.
8. Wählen Sie die Gruppe aus.
9. Klicken Sie auf **Zuweisen**.
10. Klicken Sie auf **Ja** Aufforderung.

## Nächste Schritte
[AZURE.INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]


