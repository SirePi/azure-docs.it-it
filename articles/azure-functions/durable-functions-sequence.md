---
title: Concatenamento di funzioni in Funzioni permanenti - Azure
description: Scopri come eseguire un esempio di Funzioni permanenti che effettua una sequenza di funzioni.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: a5b337e5318154e299f82b1102ca832303d752f7
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970055"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Concatenamento di funzioni in Funzioni permanenti - Esempio di sequenza di Hello

Il concatenamento di funzioni si riferisce al criterio di esecuzione di una sequenza di funzioni in un determinato ordine. Spesso l'output di una funzione deve essere applicato all'input di un'altra funzione. Questo articolo illustra un esempio che usa le [Funzioni permanenti](durable-functions-overview.md) per implementare il concatenamento di funzioni.

## <a name="prerequisites"></a>prerequisiti

* [Installare Funzioni permanenti](durable-functions-install.md).

## <a name="the-functions"></a>Funzioni

Questo articolo descrive le funzioni seguenti nell'app di esempio:

* `E1_HelloSequence`: funzione di orchestrazione che chiama `E1_SayHello` più volte in una sequenza. Archivia gli output delle chiamate `E1_SayHello` e registra i risultati.
* `E1_SayHello`: funzione di attività che antepone una stringa con "Hello".

Le sezioni seguenti illustrano la configurazione e il codice usati per gli script in C# e in JavaScript. Il codice per lo sviluppo in Visual Studio viene visualizzato alla fine dell'articolo.

> [!NOTE]
> L'estensione Funzioni permanenti è disponibile solo in JavaScript nel runtime di funzioni v2.

## <a name="e1hellosequence"></a>E1_HelloSequence
### <a name="functionjson-file"></a>File function.json

Se si usa Visual Studio Code o il portale di Azure per lo sviluppo, questo è il contenuto del file *function.json* per la funzione di orchestrazione. La maggior parte dei file *function.json* dell'agente di orchestrazione sono quasi identici a questo.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

Fondamentale è il tipo di associazione `orchestrationTrigger`. Tutte le funzioni di orchestrazione devono usare questo tipo di trigger.

> [!WARNING]
> Per rispettare la regola "no I/O" delle funzioni di orchestrazione, non usare associazioni di input o output quando si usa l'associazione di trigger `orchestrationTrigger`.  Quando sono necessarie altre associazioni di input o output, occorre invece usarle nel contesto delle funzioni `activityTrigger` già chiamate dall'agente di orchestrazione.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>Script C# (Visual Studio Code e codice di esempio del portale di Azure) 

Il codice sorgente è il seguente:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Tutte le funzioni di orchestrazione C# devono avere un parametro di tipo `DurableOrchestrationContext` disponibile nell'assembly `Microsoft.Azure.WebJobs.Extensions.DurableTask`. Se si usa lo script C#, è possibile fare riferimento all'assembly usando la notazione `#r`. Questo contesto di ambiente consente di chiamare altre funzioni di *attività* e di passare i parametri di input usando il relativo metodo [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

Il codice chiama `E1_SayHello` tre volte in sequenza con valori dei parametri diversi. Il valore restituito di ogni chiamata viene aggiunto all'elenco `outputs`, che viene restituito al termine della funzione.

### <a name="javascript"></a>JavaScript

Il codice sorgente è il seguente:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Tutte le funzioni di orchestrazione JavaScript devono includere il modulo `durable-functions`. Si tratta di una libreria JavaScript che traduce le azioni della funzione di orchestrazione nel protocollo di esecuzione di Funzioni permanenti per lingue out-of-process. Esistono tre differenze significative tra una funzione di orchestrazione e altre funzioni JavaScript:

1. La funzione è una [funzione generatore.](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript)
2. Per la funzione viene eseguito il wrapping in una chiamata al modulo `durable-functions` (qui `df`).
3. La funzione termina chiamando `return`, non `context.done`.

L'oggetto `context` contiene un oggetto `df` che consente di chiamare altre funzioni di *attività* e di passare parametri di input tramite il metodo `callActivityAsync`. Il codice chiama `E1_SayHello` tre volte in sequenza con valori di parametro diversi, usando `yield` per indicare che l'esecuzione deve attendere la restituzione delle chiamate di funzioni di attività asincrone. Il valore restituito di ogni chiamata viene aggiunto all'elenco `outputs`, che viene restituito al termine della funzione.

## <a name="e1sayhello"></a>E1_SayHello
### <a name="functionjson-file"></a>File function.json

Il file *function.json* per la funzione di attività `E1_SayHello` è simile a quello di `E1_HelloSequence` ad eccezione del fatto che usa un tipo di associazione `activityTrigger` anziché un tipo di associazione `orchestrationTrigger`.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Tutte le funzioni chiamate da una funzione di orchestrazione devono usare l'associazione `activityTrigger`.

L'implementazione di `E1_SayHello` è un'operazione di formattazione delle stringhe relativamente semplice.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Questa funzione ha un parametro di tipo [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html), usato per ottenere l'input passato dalla chiamata della funzione di orchestrazione a [`CallActivityAsync<T>`](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

### <a name="javascript"></a>JavaScript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

A differenza di una funzione di orchestrazione di JavaScript, una funzione di attività JavaScript non ha bisogno di alcuna configurazione speciale. L'input passato al metodo tramite la funzione dell'agente di orchestrazione è presente nell'oggetto `context.bindings` sotto il nome del binding `activitytrigger`, in questo caso `context.bindings.name`. Il nome del binding può essere impostato come parametro della funzione esportata e vi si può accedere direttamente, come nel codice di esempio.

## <a name="run-the-sample"></a>Eseguire l'esempio

Per eseguire l'orchestrazione `E1_HelloSequence`, inviare la richiesta POST HTTP seguente.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

Ad esempio, se si esegue l'esempio in un'app per le funzioni denominata "myfunctionapp", sostituire "{host}" con "myfunctionapp.azurewebsites.net".

Il risultato è una risposta HTTP 202, simile alla seguente (tagliata per brevità):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

A questo punto, l'orchestrazione viene messa in coda e inizia ad avviarsi immediatamente. L'URL nell'intestazione `Location` può essere usato per controllare lo stato dell'esecuzione.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Il risultato è lo stato dell'orchestrazione. Si esegue e completa rapidamente e appare nello stato *Completato* con una risposta simile alla seguente (tagliata per brevità):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Come si può notare, il `runtimeStatus` dell'istanza è *Completato* e `output` contiene il risultato JSON serializzato dell'esecuzione della funzione di orchestrazione.

> [!NOTE]
> L'endpoint HTTP POST che ha avviato la funzione di orchestrazione è implementato nell'applicazione di esempio come una funzione di trigger HTTP denominata "HttpStart". È possibile implementare una logica di avvio simile per altri tipi di trigger, ad esempio `queueTrigger`, `eventHubTrigger`, o `timerTrigger`.

Esaminare i log di esecuzione della funzione. La funzione `E1_HelloSequence` si è avviata e completata più volte a causa del comportamento di riproduzione descritto nella [panoramica](durable-functions-overview.md). In compenso, si sono verificate solo tre esecuzioni di `E1_SayHello` da quando tali esecuzioni delle funzioni non vengono riprodotte.

## <a name="visual-studio-sample-code"></a>Codice di esempio di Visual Studio

Di seguito è riportata l'orchestrazione come un unico file C# in un progetto di Visual Studio:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Passaggi successivi

In questo esempio è stata illustrata una semplice orchestrazione di concatenamento delle funzioni. Nell'esempio successivo viene illustrato come implementare il criterio di fan-out/fan-in. 

> [!div class="nextstepaction"]
> [Eseguire l'esempio di Fan-out/fan-in](durable-functions-cloud-backup.md)
