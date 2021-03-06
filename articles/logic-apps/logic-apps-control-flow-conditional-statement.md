---
title: Aggiungere la istruzioni condizionali ai flussi di lavoro - App per la logica di Azure | Microsoft Docs
description: Come creare condizioni che controllano le azioni nei flussi di lavoro nelle App per la logica di Azure
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d4e69d33e07f484b4ccc5343786865230368c7ca
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096377"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Creare istruzioni condizionali che controllano le azioni del flusso di lavoro nelle App per la logica di Azure

Per eseguire azioni specifiche nell'app per la logica solo dopo il superamento di una condizione specificata, aggiungere un'*istruzione condizionale*. Questa struttura confronta i dati nel flusso di lavoro con valori o campi specifici. È quindi possibile definire azioni diverse da eseguire a seconda che i dati soddisfino o meno la condizione. È possibile annidare le condizioni all'interno di altre.

Si supponga, ad esempio, di avere un'app per la logica che invia troppi messaggi di posta elettronica quando vengono visualizzati nuovi elementi sul feed RSS di un sito Web. È possibile aggiungere un'istruzione condizionale per inviare posta elettronica solo quando il nuovo elemento include una stringa specifica. 

> [!TIP]
> Per eseguire passaggi diversi in base a valori specifici, usare invece un'[*istruzione switch*](../logic-apps/logic-apps-control-flow-switch-statement.md).

## <a name="prerequisites"></a>prerequisiti

* Una sottoscrizione di Azure. Se non si ha una sottoscrizione, è possibile [iscriversi per creare un account Azure gratuito](https://azure.microsoft.com/free/).

* Conoscenza di base di [come creare le app per la logica](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Per seguire l'esempio di questo articolo, [creare questa app per la logica di esempio](../logic-apps/quickstart-create-first-logic-app-workflow.md) con un account Outlook.com o Office 365 Outlook.

## <a name="add-a-condition"></a>Aggiungere una condizione

1. Nel <a href="https://portal.azure.com" target="_blank">portale di Azure</a> aprire l'app per la logica in Progettazione app per la logica.

2. Aggiungere una condizione nella posizione desiderata. 

   Per aggiungere una condizione tra un passaggio e l'altro, spostare il puntatore sulla freccia in cui si vuole aggiungere la condizione. Scegliere il **segno più** (**+**) visualizzato, quindi **Aggiungi una condizione**. Ad esempio: 

   ![Aggiungere la condizione tra un passaggio e l'altro](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Quando si vuole aggiungere una condizione alla fine del flusso di lavoro, nella parte inferiore dell'app per la logica scegliere **+ Nuovo passaggio** > **Aggiungi una condizione**.

3. Sotto **Condizione** compilare la condizione. 

   1. Nella casella a sinistra specificare i dati o i campi che si vuole confrontare.

      Facendo clic all'interno della casella a sinistra, sarà visualizzato un elenco di contenuto dinamico. Sarà quindi possibile selezionare gli output dei passi precedenti nell'app logica. 
      Per questo esempio, selezionare il riepilogo del feed RSS.

      ![Compilare la condizione](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   2. Nell'elenco centrale selezionare l'operazione da eseguire. 
   Per questo esempio selezionare "**contiene**". 

   3. Nella casella a destra specificare un valore o un campo come criterio. 
   Per questo esempio, specificare la stringa seguente: **Microsoft**

   Questa è la condizione completa:

   ![Condizione completa](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

5. In **È true** e **Se no** aggiungere i passaggi da eseguire in base al fatto che la condizione sia soddisfatta o meno. Ad esempio: 

   ![Condizione con i percorsi "È true" e "Se no"](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > È possibile trascinare le azioni esistenti nei percorsi **È true** e **Se no**.

6. Salvare l'app per la logica.

Questa app per la logica ora invia messaggi solo quando i nuovi elementi nel feed RSS soddisfano la condizione.

## <a name="json-definition"></a>Definizione JSON

Dopo aver creato un'app per la logica usando un'istruzione condizionale, esaminare la definizione del codice generale dietro l'istruzione condizionale.

``` json
"actions": {
  "Condition": {
    "type": "If",
    "actions": {
      "Send_an_email": {
        "inputs": {},
        "runAfter": {}
    },
    "expression": {
      "and": [ 
        { 
          "contains": [ 
            "@triggerBody()?['summary']", "Microsoft"
          ]
        } 
      ]
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Supporto

* In caso di domande, visitare il [forum di App per la logica di Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Per votare o inviare suggerimenti relativi alle funzionalità, visitare il [sito dei commenti e suggerimenti degli utenti di App per la logica di Azure](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passaggi successivi

* [Eseguire i passaggi in base a valori diversi (istruzioni switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Eseguire e ripetere i passaggi (cicli)](../logic-apps/logic-apps-control-flow-loops.md)
* [Eseguire o unire passaggi paralleli (rami)](../logic-apps/logic-apps-control-flow-branches.md)
* [Eseguire i passaggi in base allo stato delle azioni raggruppate (ambiti)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
