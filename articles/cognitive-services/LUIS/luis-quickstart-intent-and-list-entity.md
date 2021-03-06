---
title: "Esercitazione: Creare un'app di Language Understanding per ottenere un elenco di dati di corrispondenza di testo esatta - Azure | Microsoft Docs"
description: Questa esercitazione illustra come creare una semplice app di Language Understanding usando finalità ed entità elenco per estrarre i dati in questa guida introduttiva.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 04411f415b7cfe07d893c43e758bd2a4a226472a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162199"
---
# <a name="tutorial-4-add-list-entity"></a>Esercitazione: 4. Aggiungere un'entità elenco
In questa esercitazione si crea un'app che dimostra come ottenere dati corrispondenti a un elenco predefinito. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Informazioni sulle entità elenco 
> * Creare una nuova app LUIS per il dominio delle risorse umane con la finalità MoveEmployee
> * Aggiungere l'entità elenco per estrarre un dipendente dall'espressione
> * Eseguire il training e pubblicare l'app
> * Eseguire query sull'endpoint dell'app per ottenere una risposta JSON di Language Understanding

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Prima di iniziare
Se non si ha l'app relativa alle risorse umane dell'esercitazione sull'[entità di espressione regolare](luis-quickstart-intents-regex-entity.md), [importare](luis-how-to-start-new-app.md#import-new-app) il codice JSON in una nuova app nel sito Web [LUIS](luis-reference-regions.md#luis-website). L'app da importare è disponibile nel repository GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-regex-HumanResources.json).

Se si vuole mantenere l'app relativa alle risorse umane originale, clonare la versione nella pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) (Impostazioni) assegnando il nome `list`. La clonazione è un ottimo modo per provare le diverse funzionalità di LUIS senza modificare la versione originale. 

## <a name="purpose-of-the-list-entity"></a>Scopo dell'entità elenco
Quest'app permette di prevedere le espressioni relative allo spostamento di un dipendente da un edificio a un altro. Quest'app usa un'entità elenco per estrarre un dipendente. Il dipendente può essere designato usando il nome, il numero di telefono, l'indirizzo di posta elettronica o il codice fiscale federale degli Stati Uniti. 

Un'entità elenco può contenere numerosi elementi con sinonimi per ogni elemento. Per le aziende di piccole e medie dimensioni, l'entità elenco viene usata per estrarre le informazioni sui dipendenti. 

Il nome canonico per ogni elemento è il numero dipendente. Ecco alcuni sinonimi di esempio per questo dominio: 

|Scopo del sinonimo|Valore del sinonimo|
|--|--|
|NOME|John W. Smith|
|Indirizzo di posta elettronica|john.w.smith@mycompany.com|
|Numero interno|x12345|
|Numero di telefono cellulare personale|425-555-1212|
|Codice fiscale federale degli Stati Uniti|123-45-6789|

Un'entità elenco è una scelta appropriata per questo tipo di dati quando:

* I valori dei dati sono un set noto.
* Il set non supera i [limiti](luis-boundaries.md) massimi di LUIS per questo tipo di entità.
* Il testo nell'espressione è una corrispondenza esatta con un sinonimo. 

LUIS estrae il dipendente in modo che sia possibile creare un ordine standard per lo spostamento del dipendente tramite l'applicazione client.
<!--
## Example utterances
Simple example utterances for a `MoveEmployee` inent:

```
move John W. Smith from B-1234 to H-4452
mv john.w.smith@mycompany from office b-1234 to office h-4452

```
-->

## <a name="add-moveemployee-intent"></a>Aggiungere la finalità MoveEmployee

1. Assicurarsi che l'app relativa alle risorse umane sia presente nella sezione **Build** (Compila) di LUIS. È possibile passare a questa sezione selezionando **Build** (Compila) nella barra dei menu in alto a destra. 

2. Selezionare **Create new intent** (Crea nuova finalità). 

3. Immettere `MoveEmployee` nella finestra di dialogo popup, quindi selezionare **Done** (Fine). 

    ![Screenshot della finestra di dialogo di creazione di una nuova finalità](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Aggiungere espressioni di esempio alla finalità.

    |Espressioni di esempio|
    |--|
    |move John W. Smith from B-1234 to H-4452|
    |mv john.w.smith@mycompany.com from office b-1234 to office h-4452|
    |shift x12345 to h-1234 tomorrow|
    |place 425-555-1212 in HH-2345|
    |move 123-45-6789 from A-4321 to J-23456|
    |mv Jill Jones from D-2345 to J-23456|
    |shift jill-jones@mycompany.com to M-12345|
    |x23456 to M-12345|
    |425-555-0000 to h-4452|
    |234-56-7891 to hh-2345|

    [ ![Schermata della pagina della finalità con le nuove espressioni evidenziate](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

## <a name="create-an-employee-list-entity"></a>Creare un'entità elenco dipendenti
Ora che la finalità **MoveEmployee** ha espressioni, LUIS deve capire cos'è un dipendente. 

1. Selezionare **Entities** (Entità) nel pannello di sinistra.

2. Selezionare **Create new entity** (Crea nuova entità).

3. Nella finestra popup relativa all'entità immettere `Employee` come nome dell'entità e **List** come tipo di entità. Selezionare **Operazione completata**.  

    [![](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Screenshot della finestra popup di creazione di una nuova entità")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. Nella pagina relativa all'entità dipendente immettere `Employee-24612` come nuovo valore.

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Screenshot di immissione di un valore")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Per i sinonimi, aggiungere i valori seguenti:

    |Scopo del sinonimo|Valore del sinonimo|
    |--|--|
    |NOME|John W. Smith|
    |Indirizzo di posta elettronica|john.w.smith@mycompany.com|
    |Numero interno|x12345|
    |Numero di telefono cellulare personale|425-555-1212|
    |Codice fiscale federale degli Stati Uniti|123-45-6789|

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Screenshot dell'immissione di sinonimi")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Immettere `Employee-45612` come nuovo valore.

7. Per i sinonimi, aggiungere i valori seguenti:

    |Scopo del sinonimo|Valore del sinonimo|
    |--|--|
    |NOME|Jill Jones|
    |Indirizzo di posta elettronica|jill-jones@mycompany.com|
    |Numero interno|x23456|
    |Numero di telefono cellulare personale|425-555-0000|
    |Codice fiscale federale degli Stati Uniti|234-56-7891|

## <a name="train-the-luis-app"></a>Eseguire il training dell'app di Language Understanding

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Pubblicare l'app per ottenere l'URL endpoint

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eseguire una query nell'endpoint con un'espressione diversa

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Andare alla fine dell'URL nell'indirizzo e immettere `shift 123-45-6789 from Z-1242 to T-54672`. L'ultimo parametro querystring è `q`, la **query** dell'espressione. Questa espressione non corrisponde ad alcuna delle espressioni etichettate, per cui rappresenta un buon test e deve restituire la finalità `MoveEmployee` con l'oggetto `Employee` estratto.

  ```JSON
  {
    "query": "shift 123-45-6789 from Z-1242 to T-54672",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9882801
      },
      {
        "intent": "FindForm",
        "score": 0.016044287
      },
      {
        "intent": "GetJobInformation",
        "score": 0.007611245
      },
      {
        "intent": "ApplyForJob",
        "score": 0.007063288
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00684710965
      },
      {
        "intent": "None",
        "score": 0.00304174074
      },
      {
        "intent": "Utilities.Help",
        "score": 0.002981
      },
      {
        "intent": "Utilities.Confirm",
        "score": 0.00212222221
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00191026414
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0007461446
      }
    ],
    "entities": [
      {
        "entity": "123 - 45 - 6789",
        "type": "Employee",
        "startIndex": 6,
        "endIndex": 16,
        "resolution": {
          "values": [
            "Employee-24612"
          ]
        }
      },
      {
        "entity": "123",
        "type": "builtin.number",
        "startIndex": 6,
        "endIndex": 8,
        "resolution": {
          "value": "123"
        }
      },
      {
        "entity": "45",
        "type": "builtin.number",
        "startIndex": 10,
        "endIndex": 11,
        "resolution": {
          "value": "45"
        }
      },
      {
        "entity": "6789",
        "type": "builtin.number",
        "startIndex": 13,
        "endIndex": 16,
        "resolution": {
          "value": "6789"
        }
      },
      {
        "entity": "-1242",
        "type": "builtin.number",
        "startIndex": 24,
        "endIndex": 28,
        "resolution": {
          "value": "-1242"
        }
      },
      {
        "entity": "-54672",
        "type": "builtin.number",
        "startIndex": 34,
        "endIndex": 39,
        "resolution": {
          "value": "-54672"
        }
      }
    ]
  }
  ```

  Il dipendente è stato trovato e restituito come tipo `Employee` con un valore di risoluzione di `Employee-24612`.

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Dov'è l'elaborazione del linguaggio naturale nell'entità elenco? 
L'entità elenco è una corrispondenza di testo esatta, quindi non si basa sull'elaborazione del linguaggio naturale o su Machine Learning. Language Understanding usa l'elaborazione del linguaggio naturale (o Machine Learning) per selezionare la finalità corretta con il punteggio più alto. Un'espressione può essere un insieme di più entità o anche di più tipi di entità. Ogni espressione viene elaborata per tutte le entità nell'app, incluse le entità di elaborazione in linguaggio naturale (o sottoposte ad apprendimento automatico).

## <a name="what-has-this-luis-app-accomplished"></a>Quali attività ha eseguito l'app di Language Understanding?
Questa app, con un'entità elenco, ha estratto il dipendente corretto. 

Il chatbot ha ora informazioni sufficienti per determinare l'azione principale, `MoveEmployee`, e il dipendente da spostare. 

## <a name="where-is-this-luis-data-used"></a>Qual è la destinazione d'uso dei dati di Language Understanding? 
LUIS ha terminato con questa richiesta. L'applicazione chiamante, ad esempio un chatbot, può acquisire il risultato topScoringIntent e i dati dell'entità per completare il passaggio successivo. LUIS non esegue questo lavoro programmatico per il bot o l'applicazione chiamante, ma si limita a determinare l'intenzione dell'utente. 

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Aggiungere un'entità gerarchica all'app](luis-quickstart-intent-and-hier-entity.md)

