---
title: Monitorare i dispositivi in Azure IoT Central | Microsoft Docs
description: Un operatore può usare l'applicazione Azure IoT Central per monitorare i dispositivi.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 9a3b7383651d679b079818fb32bd8f98160d0a4c
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235482"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Esercitazione: Usare Azure IoT Central per monitorare i dispositivi

Questa esercitazione illustra come un operatore può usare l'applicazione Microsoft Azure IoT Central per monitorare i dispositivi e modificare le impostazioni.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Ricevere una notifica
> * Ricercare la causa di un problema
> * Correggere un problema

## <a name="prerequisites"></a>prerequisiti

Prima di iniziare, il generatore deve completare le esercitazioni per creare l'applicazione Azure IoT Central:

* [Definire un nuovo tipo di dispositivo](tutorial-define-device-type.md)
* [Configurare le regole e le azioni per i dispositivi](tutorial-configure-rules.md)
* [Personalizzare la visualizzazione dell'operatore](tutorial-customize-operator.md)

## <a name="receive-a-notification"></a>Ricevere una notifica

Azure IoT Central invia le notifiche sui dispositivi come messaggi di posta elettronica. Il generatore ha aggiunto una regola che prevede l'invio di una notifica quando la temperatura in un dispositivo condizionatore d'aria connesso supera una certa soglia. Controllare i messaggi di posta elettronica inviati all'account scelto dal generatore per la ricezione delle notifiche.

Aprire il messaggio di posta elettronica ricevuto al termine dell'esercitazione [Configurare le regole e le azioni per il dispositivo](tutorial-configure-rules.md). Nel messaggio di posta elettronica scegliere **Click here to open your device** (Fare clic qui per accedere al dispositivo):

![Regole di Application Builder (Generatore applicazioni)](media/tutorial-monitor-devices/email.png)

Nel browser viene visualizzata la pagina **Dispositivo** relativa al dispositivo simulato **Connected Air Conditioner-1** creato nelle esercitazioni precedenti:

![Dispositivo che ha attivato il messaggio di posta elettronica di notifica](media/tutorial-monitor-devices/sourcedevice.png)

## <a name="investigate-an-issue"></a>Ricercare la causa di un problema

Un operatore può visualizzare le informazioni sul dispositivo nelle pagine **Misurazioni**, **Impostazioni**, **Proprietà**, **Regole** e **Dashboard**. Il generatore ha personalizzato il **Dashboard** in modo da informazioni importanti relative a un dispositivo condizionatore d'aria connesso.

Scegliere la visualizzazione **Dashboard** per vedere le informazioni sul dispositivo.

![Pagina dashboard](media/tutorial-monitor-devices/initial_screen.png)

Il grafico del dashboard mostra il tracciato della temperatura del dispositivo. È inoltre possibile visualizzare la temperatura target corrente impostata per il dispositivo nel riquadro **Set target temperature** (Temperatura target impostata). Si decide che la temperatura target è troppo alta.

## <a name="remediate-an-issue"></a>Correggere un problema

Per modificare la temperatura target del dispositivo, usare la pagina **Impostazioni**:

1. Scegliere **Impostazioni**. Modificare il valore **Set Temperature** (Temperatura impostata) impostandolo su 100. Scegliere **Aggiorna** per inviare la nuova temperatura target al dispositivo. Quando il dispositivo riconosce la modifica delle impostazioni, lo stato del valore dell'impostazione passa a **sincronizzato**:

    ![Aggiornamento delle impostazioni](media/tutorial-monitor-devices/change_settings.png)

2. Scegliere **Dashboard** e verificare il nuovo valore dell'impostazione:

    ![Dashboard del dispositivo aggiornato](media/tutorial-monitor-devices/new_settings.png)

## <a name="next-steps"></a>Passaggi successivi

Questa esercitazione illustra come:

> [!div class="nextstepaction"]
> * Ricevere una notifica
> * Ricercare la causa di un problema
> * Correggere un problema

Ora che si sa come monitorare il dispositivo, come passaggio successivo si consiglia di [aggiungere un dispositivo](tutorial-add-device.md).