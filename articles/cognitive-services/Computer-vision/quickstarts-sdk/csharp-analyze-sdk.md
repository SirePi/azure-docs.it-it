---
title: API Visione artificiale - Guida introduttiva all'analisi di un'immagine con C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In questa guida introduttiva si analizza un'immagine usando la libreria client Windows C# di Visione artificiale in Servizi cognitivi.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 3ff3a4702ab0b1fb663ee896f268065caf043809
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772324"
---
# <a name="quickstart-analyze-an-image---sdk-c35"></a>Guida introduttiva: Analizzare un'immagine - SDK, C&#35;

In questa guida introduttiva si analizzano un'immagine locale e una remota per estrarre le caratteristiche visive usando la libreria client Windows di Visione artificiale.

## <a name="prerequisites"></a>Prerequisiti

* Per usare Visione artificiale, è necessario avere una chiave di sottoscrizione. A tale scopo, vedere [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Come ottenere chiavi di sottoscrizione).
* Qualsiasi edizione di [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).
* Il pacchetto NuGet della libreria client [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). Non è necessario scaricare il pacchetto. Le istruzioni di installazione sono disponibili più avanti.

## <a name="analyzeimageasync-method"></a>Metodo AnalyzeImageAsync

I metodi `AnalyzeImageAsync` e `AnalyzeImageInStreamAsync` eseguono il wrapping dell'[API Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) rispettivamente per le immagini locali e remote. È possibile usare questi metodi per estrarre caratteristiche visive in base al contenuto dell'immagine e scegliere le caratteristiche da restituire, tra cui:

* Un elenco dettagliato di tag correlati al contenuto dell'immagine.
* Una frase completa di descrizione del contenuto dell'immagine.
* Le coordinate, il sesso e l'età di tutti i visi presenti nell'immagine.
* Il tipo di immagine (ClipArt o disegno di linee).
* Il colore principale, il colore dominante o l'indicazione se un'immagine è in bianco e nero.
* La categoria definita in questa [tassonomia](../Category-Taxonomy.md).
* L'indicazione se l'immagine presenta contenuto per adulti o con riferimenti sessuali.

Per eseguire l'esempio, seguire questa procedura:

1. Creare una nuova app console Visual C# in Visual Studio.
1. Installare il pacchetto NuGet della libreria client di Visione artificiale.
    1. Nel menu fare clic su **Strumenti**, selezionare **Gestione pacchetti NuGet** e quindi **Gestisci pacchetti NuGet per la soluzione**.
    1. Fare clic sulla scheda **Sfoglia** e nella casella di **ricerca** digitare "Microsoft.Azure.CognitiveServices.Vision.ComputerVision".
    1. Selezionare la voce **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** quando viene visualizzata, quindi fare clic sulla casella di controllo accanto al nome del progetto e infine su **Installa**.
1. Sostituire `Program.cs` con il codice seguente.
1. Sostituire `<Subscription Key>` con la propria chiave di sottoscrizione valida.
1. Modificare `computerVision.AzureRegion = AzureRegions.Westcentralus` impostando l'indirizzo in cui si sono ottenute le chiavi di sottoscrizione, se necessario.
1. Sostituire `<LocalImage>` con il percorso e il nome del file di un'immagine locale.
1. Facoltativamente, impostare un'immagine diversa per `remoteImageUrl`.
1. Eseguire il programma.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace ImageAnalyze
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

        // Specify the features to return
        private static readonly List<VisualFeatureTypes> features =
            new List<VisualFeatureTypes>()
        {
            VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
            VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
            VisualFeatureTypes.Tags
        };

        static void Main(string[] args)
        {
            ComputerVisionAPI computerVision = new ComputerVisionAPI(
                new ApiKeyServiceClientCredentials(subscriptionKey), 
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "Westcentralus" with "Westus".
            //
            // Free trial subscription keys are generated in the westcentralus
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.AzureRegion = AzureRegions.Westcentralus;

            Console.WriteLine("Images being analyzed ...");
            var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
            var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Analyze a remote image
        private static async Task AnalyzeRemoteAsync(
            ComputerVisionAPI computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            ImageAnalysis analysis =
                await computerVision.AnalyzeImageAsync(imageUrl, features);
            DisplayResults(analysis, imageUrl);
        }

        // Analyze a local image
        private static async Task AnalyzeLocalAsync(
            ComputerVisionAPI computerVision, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            using (Stream imageStream = File.OpenRead(imagePath))
            {
                ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                    imageStream, features);
                DisplayResults(analysis, imagePath);
            }
        }

        // Display the most relevant caption for the image
        private static void DisplayResults(ImageAnalysis analysis, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
        }
    }
}
```

## <a name="analyzeimageasync-response"></a>Risposta di AnalyzeImageAsync

Una risposta con esito positivo visualizza la didascalia più pertinente per ogni immagine.

Per un esempio di output JSON non elaborato, vedere [Guide introduttive all'API: Analizzare un'immagine locale con C#](../QuickStarts/CSharp-analyze.md#analyze-image-response).

```cmd
http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Passaggi successivi

Esaminare le API Visione artificiale usate per analizzare un'immagine, rilevare celebrità e luoghi di interesse, creare un'anteprima ed estrarre testo scritto a mano e stampato.

> [!div class="nextstepaction"]
> [Esaminare le API Visione artificiale](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
