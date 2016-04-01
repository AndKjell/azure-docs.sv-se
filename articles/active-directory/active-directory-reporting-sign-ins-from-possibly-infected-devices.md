<properties
    pageTitle="Anmeldungen von möglicherweise infizierten Geräten"
    description="Ein Bericht mit Anmeldeversuchen, die von Geräten ausgeführt wurden, auf denen Schadsoftware ausgeführt werden könnte."
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


# Anmeldungen von möglicherweise infizierten Geräten
Mit diesem Bericht wird versucht, die Geräte Ihrer Benutzer zu identifizieren, die infiziert wurden und jetzt Teil eines Botnetzes sind (wird auch als Zombiearmee bezeichnet). Wir korrelieren IP-Adressen der Anmeldungen von Benutzern mit IP-Adressen, von denen wir wissen, dass sie mit Botnetzservern verbunden sind.

Empfehlung: Dieser Bericht kennzeichnet IP-Adressen, nicht die Benutzergeräte. Sie sollten den Benutzer kontaktieren und alle Geräte des Benutzers überprüfen, um sicherzugehen. Es ist auch möglich, dass das persönliche Gerät eines Benutzers infiziert ist oder dass eine andere Person die gleiche IP-Adresse wie der Benutzer verwendet hat und ein infiziertes Gerät besitzt.

Weitere Informationen dazu, wie Sie Malware-Infektionen finden Sie unter der [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).

![Anmeldungen von möglicherweise infizierten Geräten](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)


