---
title: Riferimento alla temperatura per le entità predefinite LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Questo articolo contiene informazioni sull'entità predefinita temperatura in Language Understanding, ovvero LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 6436a7ee8d7b796595813fa613c442824aeae8f3
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237462"
---
# <a name="temperature-entity"></a>Entità di temperatura
L'entità temperatura consente di estrarre una vasta gamma di tipi di temperatura. Poiché è già stato eseguito il training per questa entità, non è necessario aggiungere all'applicazione espressioni di esempio contenenti la temperatura. L'entità temperatura è supportata in [molte impostazioni cultura](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Tipi di temperatura
La temperatura viene gestita nel repository Github [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819)

## <a name="resolution-for-prebuilt-temperature-entity"></a>Risoluzione per l'entità temperatura predefinita
L'esempio seguente illustra la risoluzione dell'entità **builtin.temperature**.

```JSON
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passaggi successivi

Informazioni sulle entità [percentuale](luis-reference-prebuilt-percentage.md), [numero](luis-reference-prebuilt-number.md) ed [età](luis-reference-prebuilt-age.md). 