<properties
    pageTitle="Irreguläre Anmeldeaktivitäten"
    description="Ein Bericht mit Anmeldungen, die von unseren Algorithmen für maschinelles Lernen als anomal identifiziert wurden."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2015"
    ms.author="saah;kenhoff"/>

# Irreguläre Anmeldeaktivitäten

Irreguläre Anmeldungen sind Anmeldungen, die von Machine Learning-Algorithmen auf der Grundlage einer Bedingung für eine "unmögliche Strecke" in Kombination mit einem anomalen Standort und Gerät für die Anmeldung identifiziert wurden. Dies kann ein Hinweis darauf sein, dass sich ein Hacker erfolgreich mit diesem Konto angemeldet hat.
Wir senden eine E-Mail-Benachrichtigung an die globalen Administratoren, wenn wir 10 oder mehr anomale Anmeldeereignisse innerhalb eines Zeitraums von 30 Tagen oder weniger feststellen. Seien Sie sicher, dass aad-alerts-noreply@mail.windowsazure.com in Ihrer Liste sicherer Absender hinzufügen.


