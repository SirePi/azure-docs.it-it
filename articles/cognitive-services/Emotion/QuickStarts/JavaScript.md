---
title: Guida introduttiva all'API Emozioni con JavaScript | Microsoft Docs
description: Informazioni ed esempi di codice per iniziare rapidamente a usare l'API Emozioni con JavaScript in Servizi cognitivi.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: fb9cc2335582c4ec75ec45635e519346d65d7e08
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072093"
---
# <a name="emotion-api-javascript-quick-start"></a>Guida introduttiva all'API Emozioni con JavaScript

> [!IMPORTANT]
> L'anteprima dell'API Video è terminata il 30 ottobre 2017. La nuova [anteprima dell'API Video Indexer](https://azure.microsoft.com/services/cognitive-services/video-indexer/) consente di estrarre facilmente informazioni dettagliate dai video e di ottimizzare le esperienze di individuazione del contenuto, ad esempio i risultati della ricerca, tramite il rilevamento di testo parlato, visi, caratteri ed emozioni. [Altre informazioni](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Questo articolo contiene informazioni ed esempi di codice per iniziare rapidamente a usare il [metodo di riconoscimento dell'API Emozioni](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) con JavaScript per riconoscere le emozioni espresse da una o più persone in un'immagine.

## <a name="prerequisite"></a>Prerequisito
* È possibile ottenere la chiave di sottoscrizione gratuita [qui](https://azure.microsoft.com/try/cognitive-services/) oppure, se si ha una sottoscrizione di Azure, creare una risorsa API Emozioni e ottenere la chiave di sottoscrizione e l'endpoint.

![Creare la risorsa API Emozioni](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Richiesta di esempio di riconoscimento delle emozioni con JavaScript

Copiare il codice seguente e salvarlo in un file, ad esempio `test.html`. Modificare il valore di `url` della richiesta per usare il percorso da cui sono state ottenute le chiavi di sottoscrizione e sostituire il valore di "Ocp-Apim-Subscription-Key" con la propria chiave di sottoscrizione valida. È possibile trovare queste informazioni nel portale di Azure, rispettivamente nelle sezioni Panoramica e Chiavi della risorsa API Emozioni. 

![Endpoint API](../Images/api-url.png)

![Chiave di sottoscrizione API](../Images/keys.png)

Modificare quindi il corpo della richiesta specificando il percorso di un'immagine da usare. Per eseguire l'esempio, trascinare il file nel browser.

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<h2>Face Rectangle</h2>
<ul id="faceRectangle">
<!-- Will populate list with response content -->
</ul>

<h2>Emotions</h2>
<ul id="scores">
<!-- Will populate list with response content -->
</ul>

<body>

<script type="text/javascript">
    $(function() {
        // No query string parameters for this API call.
        var params = { };
      
        $.ajax({
            // NOTE: You must use the same location in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URL below with "westcentralus".
            url: "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers, also supports "application/octet-stream"
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","<your subscription key>");
            },
            type: "POST",
            // Request body
            data: '{"url": "<your image url>"}',
        }).done(function(data) {
            // Get face rectangle dimensions
            var faceRectangle = data[0].faceRectangle;
            var faceRectangleList = $('#faceRectangle');

            // Append to DOM
            for (var prop in faceRectangle) {
                faceRectangleList.append("<li> " + prop + ": " + faceRectangle[prop] + "</li>");
            }
            
            // Get emotion confidence scores
            var scores = data[0].scores;
            var scoresList = $('#scores');

            // Append to DOM
            for(var prop in scores) {
                scoresList.append("<li> " + prop + ": " + scores[prop] + "</li>")
            }
        }).fail(function(err) {
            alert("Error: " + JSON.stringify(err));
        });
    });
</script>
</body>
</html>
```

## <a name="recognize-emotions-sample-response"></a>Risposta di esempio di riconoscimento delle emozioni
Una chiamata completata con successo restituisce una matrice di voci relative ai visi con associati punteggi relativi alle emozioni, ordinate in base alle dimensioni del rettangolo del viso in ordine decrescente. Una risposta vuota indica che non sono stati rilevati visi. Una voce relativa alle emozioni include i campi seguenti:
* faceRectangle: posizione del rettangolo del viso nell'immagine.
* scores: punteggi delle emozioni per ogni viso nell'immagine. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
