---
title: Metodo Helper di Azure Content Moderator SDK per .NET | Microsoft Docs
description: Come restituire un client di Content Moderator usando Azure Content Moderator SDK per .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 36f2124708731f78f34849d8210ed39ea8f59140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372852"
---
# <a name="helper-code-to-return-a-content-moderator-client"></a>Codice Helper per restituire un client di Content Moderator

Questo articolo contiene informazioni ed esempi di codice per iniziare a usare Content Moderator SDK per .NET per creare un client di Content Moderator per la sottoscrizione.

La libreria viene usata da altre guide introduttive in questa sezione.

Questo articolo presuppone che si abbia già familiarità con Visual Studio e C#.

> [!IMPORTANT]
> Questa libreria di classi contiene codice che può essere usato solo a scopo dimostrativo.
> Se si adatta questo codice per usarlo in produzione, usare un metodo sicuro di archiviazione e uso della chiave di sottoscrizione di Content Moderator.

## <a name="sign-up-for-content-moderator-services"></a>Eseguire la registrazione per i servizi Content Moderator

Per usare i servizi Content Moderator tramite l'API REST o l'SDK, è necessaria una chiave di sottoscrizione.
Vedere la [guida introduttiva](quick-start.md) per informazioni su come ottenere la chiave.

## <a name="create-your-visual-studio-project"></a>Creare il progetto di Visual Studio

1. Creare un nuovo progetto **Libreria di classi (.NET Framework)**.

   Nel codice di esempio il progetto è denominato **ModeratorHelper**.

1. Aggiungere un riferimento all'assembly del framework **System.Configuration**.

### <a name="install-required-packages"></a>Installare i pacchetti necessari

Installare i pacchetti NuGet seguenti:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>Creare il client di Content Moderator

Sostituire il contenuto del file ModeratorHelper.cs con il codice seguente:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Aggiornare i campi **AzureRegion** e **CMSubscriptionKey** con i valori di identificatore di area e chiave di sottoscrizione.

Questo è un modo rapido per creare un client di Content Moderator per la sottoscrizione.

## <a name="next-steps"></a>Passaggi successivi

[Scaricare la soluzione Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) per questa e altre guide introduttive di Content Moderator per .NET e iniziare a implementare l'integrazione.
