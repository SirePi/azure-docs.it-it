---
title: Configurazione predefinita di Conversation Learner - Servizi cognitivi Microsoft | Microsoft Docs
titleSuffix: Azure
description: Informazioni sulla configurazione predefinita di Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c0ad9f71665e503fe794c68200b90a8474750823
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173626"
---
# <a name="default-values-and-boundaries"></a>Limiti e valori predefiniti

Questo documento descrive la configurazione predefinita di Conversation Learner e i principali limiti del servizio.

## <a name="default-configuration"></a>Configurazione predefinita

Parametro | Valore predefinito
--- | --- 
Timeout di sessione predefinito | 30 minuti

## <a name="boundaries"></a>Confini

Parametro | Limite
--- | --- 
API di creazione, numero massimo di chiamate HTTP al mese | 5M
API di creazione, numero massimo di chiamate HTTP al secondo | 25
API di sessione, numero massimo di chiamate HTTP al mese | 500K
API di sessione, numero massimo di chiamate HTTP al secondo | 10
Numero massimo di entità personalizzate (non a livello di codice) per modello | Vedere il documento [Limiti di LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries). Il numero effettivo nella pratica potrebbe essere leggermente inferiore.
Numero massimo di entità predefinite per modello | Vedere il documento [Limiti di LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries)
Numero massimo di entità (in totale) per modello | 100
Numero massimo di azioni per modello | 32
Numero massimo di dialoghi di training per modello | 1000
Numero massimo di turni dell'utente per dialogo di training | 100
Numero massimo di dialoghi di log per modello | Nessun limite preimpostato, ma i dialoghi di log vengono mantenuti solo per un periodo fisso di tempo prima di essere rimossi.  Nell'interfaccia utente di Conversation Learner, inoltre, verranno visualizzati 100 dialoghi di log alla volta. 
Numero massimo di modelli per utente | Nessun limite preimpostato
Numero massimo di azioni senza attesa in sequenza | 5 (*)

(*) Dopo 5 azioni senza attesa in sequenza, tutte le azioni senza attesa verranno mascherate e Conversation Learner sceglierà tra le azioni con attesa disponibili.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Introduzione a Conversation Learner](./quickstart.md)
