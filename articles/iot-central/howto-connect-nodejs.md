---
title: Ansluta en allmän Node.js-klientprogram till Azure IoT Central | Microsoft Docs
description: Som utvecklare av enheten, hur du ansluter en allmän Node.js-enhet till Azure IoT Central programmet.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 8a5d880d0238e38fbbaa9de22fc1baf604f0fc07
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733472"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Ansluta ett allmänt klientprogram till ditt Azure IoT Central program (Node.js)

Den här artikeln beskrivs hur du som utvecklare enheten att ansluta ett allmänt Node.js-program som representerar en fysisk enhet för ditt Microsoft Azure IoT Central program.

## <a name="before-you-begin"></a>Innan du börjar

Du behöver följande för att slutföra stegen i den här artikeln:

1. Ett Azure IoT Central program. Mer information finns i [skapa Azure IoT Central programmet](howto-create-application.md).
1. En utvecklingsdator med [Node.js](https://nodejs.org/) version 4.0.0 eller senare installerat. Du kan köra `node --version` i kommandoraden för att kontrollera vilken version. Node.js är tillgängligt för många olika operativsystem.

## <a name="create-a-device-template"></a>Skapa en mall för enhet

I Azure IoT Central programmet behöver du en mall för enheten med följande mätningar och enhetsegenskaper som definierats:

### <a name="telemetry-measurements"></a>Telemetri mätning av faktisk användning

Lägg till följande telemetri i den **mätningar** sidan:

| Visningsnamn | Fältnamn  | Enheter | Min | Max | Decimaler |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Temperatur  | temperatur | F     | 60  | 110 | 0              |
| Fuktighet     | luftfuktighet    | %     | 0   | 100 | 0              |
| Tryck     | tryck    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  Datatypen för måttet telemetri är dubbla.

Ange fältnamn exakt som de visas i tabellen i mallar för enheten. Om fältnamnen inte matchar kan inte visas telemetri i programmet.

### <a name="state-measurements"></a>Statliga mätning av faktisk användning

Lägg till följande tillstånd i den **mätningar** sidan:

| Visningsnamn | Fältnamn  | Värde 1 | Visningsnamn | Värde 2 | Visningsnamn |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Fläktläge     | fläktläge     | 1       | Körs      | 0       | Stoppad      |

> [!NOTE]
  Datatypen för måttet tillstånd är sträng.

Ange fältnamn exakt som de visas i tabellen i mallar för enheten. Om fältnamnen inte matchar kan tillståndet inte visas i programmet.

### <a name="event-measurements"></a>Händelsen mätning av faktisk användning

Lägg till följande händelse i den **mätningar** sidan:

| Visningsnamn | Fältnamn  | Severity |
| ------------ | ----------- | -------- |
| Överhettning  | överhettas    | Fel    |

> [!NOTE]
  Datatypen för måttet händelse är sträng.

### <a name="device-properties"></a>Enhetsegenskaper

Lägg till följande enhetsegenskaper i den **egenskapssidan**:

| Visningsnamn        | Fältnamn        | Datatyp |
| ------------------- | ----------------- | --------- |
| Serienummer       | serieNummer      | text      |
| Enhetstillverkare | tillverkare      | text      |

Ange namnen på exakt som de visas i tabellen i mallar för enheten. Om fältnamnen inte matchar kan inte programmet visa egenskapsvärdet.

### <a name="settings"></a>Inställningar

Lägg till följande **nummer** inställningar i den **inställningssidan**:

| Visningsnamn    | Fältnamn     | Enheter | Decimaler | Min | Max  | Inledande |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Fläkthastighet       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Ange temperatur | angeTemperatur | F     | 0        | 20  | 200  | 80      |

Ange fältnamn exakt som de visas i tabellen i mallar för enheten. Om fältnamnen inte matchar inte enheten ta emot inställningens värde.

## <a name="add-a-real-device"></a>Lägga till en riktig enhet

Lägg till en riktig enhet från enheten mallen du skapar och anteckna enhetens anslutningssträng i programmet Azure IoT Central. Mer information finns i [ge en riktig enhet till Azure IoT Central programmet](tutorial-add-device.md)

### <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program

Följande steg visar hur du skapar ett klientprogram som implementerar en riktig enhet som du lade till programmet.

1. Skapa en mapp med namnet `connected-air-conditioner-adv` på datorn. Navigera till mappen i kommandoradsverktyget-miljön.

1. För att initiera Node.js-projekt, kör du följande kommandon:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Skapa en fil med namnet **connectedAirConditionerAdv.js** i den `connected-air-conditioner-adv` mapp.

1. Lägg till följande `require` instruktioner i början av den **connectedAirConditionerAdv.js** fil:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Lägg till följande variabeldeklarationer i filen:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

  > [!NOTE]
   > Azure IoT Central har övergått till med hjälp av Azure IoT Hub Device Provisioning-tjänsten (DPS) för alla anslutningar på enheten, följer du dessa instrustions till [hämta enhetens anslutningssträng](concepts-connectivity.md#getting-device-connection-string) och fortsätta med resten av kursen.


    Uppdatera platshållaren `{your device connection string}` med enhetens anslutningssträng. I det här exemplet vi initiera `targetTemperature` till noll och, du kan eventuellt ta medan den aktuella läsningen från enheten eller värdet från enhetstvillingen. 

1. Om du vill skicka telemetri, tillstånd och mätning av faktisk användning till programmet Azure IoT Central, lägger du till följande funktion i filen:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({ 
        temperature: temperature, 
        humidity: humidity, 
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    1. Om du vill skicka enhetsegenskaper till programmet Azure IoT Central, lägger du till följande funktion i filen:

    ```javascript
    // Send device properties.
    function sendDeviceProperties(twin) {
      var properties = {
        serialNumber: '123-ABC',
        manufacturer: 'Contoso'
      };
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Lägg till följande definition för att definiera de inställningar som enheten svarar på:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Lägg till följande i filen för att hantera uppdaterade inställningarna från din Azure IoT Central-program:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Lägg till följande för att slutföra anslutningen till Azure IoT Central och kopplar in funktioner i klientkoden:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Kör Node.js-programmet

Kör följande kommando i miljön kommandoraden:

```cmd/sh
node connectedAirConditionerAdv.js
```

Som operatör i Azure IoT Central programmet för verkliga enheten kan du:

* Visa telemetri på den **mätningar** sidan:

    ![Visa telemetrin](media/howto-connect-nodejs/viewtelemetry.png)

* Visa enhet egenskapsvärden som skickas från din enhet den **egenskaper** sidan.

    ![Visa egenskaper för enhet](media/howto-connect-nodejs/viewproperties.png)

* Ange fläkt hastighet och mål temperaturen från den **inställningar** sidan.

    ![Hastighet för set-fläkt](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du ansluter en allmän Node.js-klient till Azure IoT Central programmet, är här nästa föreslagna steg:
* [Förbereda och ansluta en Raspberry Pi](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->
