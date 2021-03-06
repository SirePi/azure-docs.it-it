---
title: Informazioni sulla messaggistica hub IoT di Azure | Microsoft Docs
description: Guida per gli sviluppatori - Messaggistica da dispositivo a cloud e da cloud a dispositivo con l'hub IoT. Include informazioni sui formati dei messaggi e sui protocolli di comunicazione supportati.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: b0667f820145f16c75a07ebe1849e20d2de36cc7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185510"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Messaggistica da dispositivo a cloud e da cloud a dispositivo con l'hub IoT

Usare la messaggistica dell'hub IoT per comunicare con i dispositivi:

* inviando messaggi [da dispositivo a cloud][lnk-d2c] dai dispositivi per il back-end della soluzione;
* inviando messaggi [da cloud a dispositivo][lnk-c2d] dal back-end della soluzione ai dispositivi.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Le proprietà di base della funzionalità di messaggistica dell'hub IoT sono l'affidabilità e la durabilità dei messaggi. Queste proprietà consentono la resilienza in caso di connettività intermittente sul lato dispositivo e picchi di carico durante l'elaborazione degli eventi sul lato cloud. L'hub IoT implementa *almeno una volta* le garanzie di recapito per la messaggistica da dispositivo a cloud e da cloud a dispositivo.

Per un'introduzione alle funzionalità dell'hub IoT, vedere[Panoramica del servizio hub IoT di Azure][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Quando usare la messaggistica dell'hub IoT

Usare i messaggi da dispositivo a cloud per inviare la telemetria relativa alle serie temporali e gli avvisi dall'app del dispositivo e i messaggi da cloud a dispositivo per notifiche unidirezionali all'app del dispositivo.

* Vedere [Indicazioni sulle comunicazioni da dispositivo a cloud][lnk-d2c-guidance] in caso di dubbi tra l'uso delle proprietà segnalate, dei messaggi da dispositivo a cloud o del caricamento di file.
* Vedere [Indicazioni sulle comunicazioni da cloud a dispositivo][lnk-c2d-guidance] in caso di dubbi tra l'uso delle proprietà desiderate, dei metodi diretti o dei messaggi da cloud a dispositivo.

## <a name="next-steps"></a>Passaggi successivi

* Informazioni sulla [messaggistica da dispositivo a cloud][lnk-d2c] dell'hub IoT.
* Informazioni sulla [messaggistica da cloud a dispositivo][lnk-c2d] dell'hub IoT.

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: about-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md