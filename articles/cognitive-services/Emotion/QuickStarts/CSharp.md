---
title: Guida introduttiva all'API Emozioni C# | Microsoft Docs
description: Ottenere informazioni e un esempio di codice per iniziare rapidamente a usare l'API Emozioni con C# in Servizi cognitivi.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 11/02/2017
ms.author: anroth
ms.openlocfilehash: 89735ae54395447e3cb421f45db3d6b99001ecd6
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016566"
---
# <a name="emotion-api-c-quick-start"></a>Guida introduttiva all'API Emozioni C#

> [!IMPORTANT]
> L'anteprima dell'API Video è terminata il 30 ottobre 2017. Per estrarre facilmente informazioni dai video, provare la nuova [Anteprima dell'API Video Indexer](https://azure.microsoft.com/services/cognitive-services/video-indexer/). È anche possibile utilizzarla per migliorare l'esperienza di individuazione dei contenuti, come i risultati di ricerca, grazie al rilevamento di testo parlato, facce, caratteri ed emozioni. Per altre informazioni, vedere la panoramica [Anteprima di Video Indexer](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Questo articolo include informazioni ed esempi di codice per iniziare rapidamente a usare il [metodo di riconoscimento dell'API Emozioni](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) con C#. È possibile usarlo per riconoscere le emozioni espresse da una o più persone in un'immagine. 

## <a name="prerequisites"></a>prerequisiti
* Ottenere l'[API Emozioni Windows SDK](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) di Servizi cognitivi.
* Ottenere la [chiave di sottoscrizione](https://azure.microsoft.com/try/cognitive-services/) gratuita.

## <a name="emotion-recognition-c-example-request"></a>Richiesta di esempio di riconoscimento emozioni C#

Creare una nuova soluzione di Console in Visual Studio, quindi sostituire Program.cs con il codice seguente. Modificare `string uri` per usare la regione in cui è stata ottenuta la chiave di sottoscrizione. Sostituire il valore **Ocp-Apim-Subscription-Key** con la chiave di sottoscrizione valida. Per individuare la chiave di sottoscrizione, passare al portale di Azure. Nel riquadro di spostamento a sinistra, nella sezione **Chiavi**, accedere alla risorsa API Emozioni. Analogamente, è possibile ottenere l'URI di connessione corretto nel pannello **Panoramica** per la risorsa elencata in **Endpoint**.

![Le chiavi della risorsa API](../../media/emotion-api/keys.png)

Per elaborare la risposta della richiesta, usare una libreria, ad esempio `Newtonsoft.Json`. In questo modo è possibile gestire una stringa JSON come una serie di oggetti gestibili denominati token. Per aggiungere questa libreria al pacchetto, fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Gestisci pacchetti NuGet**. Quindi cercare **Newtonsoft**. Il primo risultato dovrebbe essere **Newtonsoft.Json**. Selezionare **Installa**. Ora è possibile fare riferimento a questa libreria nell'applicazione.

![Installare Newtonsoft.Json](../../media/emotion-api/newtonsoft-nuget.png)

```csharp
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;
using Newtonsoft.Json.Linq;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine(); // wait for ENTER to exit program
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<your-subscription-key>"); // 

            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URI below with "westcentralus".
            string uri = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            // A peek at the raw JSON response.
            Console.WriteLine(responseContent);

            // Processing the JSON into manageable objects.
            JToken rootToken = JArray.Parse(responseContent).First;

            // First token is always the faceRectangle identified by the API.
            JToken faceRectangleToken = rootToken.First;

            // Second token is all emotion scores.
            JToken scoresToken = rootToken.Last;

            // Show all face rectangle dimensions
            JEnumerable<JToken> faceRectangleSizeList = faceRectangleToken.First.Children();
            foreach (var size in faceRectangleSizeList) {
                Console.WriteLine(size);
            }

            // Show all scores
            JEnumerable<JToken> scoreList = scoresToken.First.Children();
            foreach (var score in scoreList) {
                Console.WriteLine(score);
            }
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Risposta di esempio delle emozioni riconosciute
Una chiamata completata con successo restituisce una matrice di voci relative ai visi con associati punteggi relativi alle emozioni. Questi sono suddivisi per dimensioni del rettangolo faccia in ordine decrescente. Una risposta vuota indica che non sono stati individuati visi. Una voce contenente emozioni include i campi seguenti:

* faceRectangle: posizione del rettangolo del viso nell'immagine
* scores: punteggi delle emozioni per ogni viso nell'immagine 

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
