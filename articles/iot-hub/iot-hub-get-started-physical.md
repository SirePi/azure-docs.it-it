---
title: Introduzione alla connessione dei dispositivi fisici all'hub IoT Azure | Microsoft Docs
description: Informazioni su come connettere dispositivi fisici e schede all'hub IoT. I dispositivi possono inviare dati di telemetria all'hub IoT e l'hub Iot può monitorare e gestire i dispositivi.
author: dominicbetts
manager: timlt
keywords: esercitazione sull'hub iot azure
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: e7911c190ded59f758eff868add6440f5add6579
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633996"
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Hub IoT di Azure - Esercitazioni introduttive per i dispositivi fisici

Queste esercitazioni presentano l'hub IoT Azure e gli SDK per dispositivi. Le esercitazioni concernono scenari IoT comuni per illustrare le funzionalità dell'hub IoT. Le esercitazioni spiegano anche come combinare l'hub IoT con altri servizi di Azure e strumenti per creare soluzioni IoT più potenti. Le esercitazioni elencate nella tabella seguente illustrano come creare dispositivi IoT fisici.

| Dispositivo IoT                       | Linguaggio di programmazione |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| DevKit di IoT                      | [Arduino in VSCode][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Adafruit Feather HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev      | [Arduino][Th_Ard]              |
| Adafruit Feather M0             | [Arduino][M0_Ard]              |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]


[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
