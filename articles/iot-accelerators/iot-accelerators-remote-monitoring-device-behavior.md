---
title: Simulerad enhetsbeteende i fjärrövervakningslösningen – Azure | Microsoft Docs
description: Den här artikeln beskriver hur du kan använda JavaScript för att definiera beteendet för en simulerad enhet i fjärrövervakningslösningen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: a983c7307308534140ab8999593ac4c8c6992a42
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338514"
---
# <a name="implement-the-device-model-behavior"></a>Implementera beteendet för enhetsmodellen

Artikeln [förstå schemat för enhetsmodellen](iot-accelerators-remote-monitoring-device-schema.md) beskrivs det schema som definierar en simulerad enhet-modell. Den här artikeln avses två typer av JavaScript-fil som implementerar beteendet för en simulerad enhet:

- **Tillstånd** JavaScript-filer som körs med jämna tidsintervall att uppdatera det interna tillståndet för enheten.
- **Metoden** JavaScript-filer som körs när lösningen anropar en metod på enheten.

I den här artikeln kan du se hur du:

>[!div class="checklist"]
> * Kontrollera tillståndet för en simulerad enhet
> * Definiera hur en simulerad enhet svarar på ett metodanrop från lösningen för fjärrövervakning
> * Felsök ditt skript

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Nästa steg

Den här artikeln beskrivs hur du definierar beteendet för dina egna anpassade simulerade enhetsmodell. Den här artikeln visar dig hur du:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Kontrollera tillståndet för en simulerad enhet
> * Definiera hur en simulerad enhet svarar på ett metodanrop från lösningen för fjärrövervakning
> * Felsök ditt skript

Nu när du har lärt dig hur du anger beteendet för en simulerad enhet, föreslagna nästa steg är att lära dig hur du [skapar en simulerad enhet](iot-accelerators-remote-monitoring-create-simulated-device.md).

Mer information för utvecklare om lösningen för fjärrövervakning, finns:

* [Referensguide för utvecklare](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Felsökningsguide för utvecklare](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
