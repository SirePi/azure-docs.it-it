---
title: Bing Search SDK | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bing Search SDK per le applicazioni che eseguono ricerche nel Web.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.assetid: ''
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: 4a40ea665e153536d2322706b455598902ce41eb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377921"
---
# <a name="bing-search-sdk"></a>Bing Search SDK
Gli esempi dell'API Ricerca notizie Bing includono scenari di:
1. Ricerca di notizie in base a termini di ricerca con i parametri `market` e `count`, verifica del numero di risultati e stampa di `totalEstimatedMatches`, nome, URL, descrizione, ora di pubblicazione e nome del provider del primo risultato.
2. Ricerca delle notizie più recenti in base a termini di ricerca con i parametri `freshness` e `sortBy`, verifica del numero di risultati e stampa di `totalEstimatedMatches`, URL, descrizione, ora di pubblicazione e nome del provider del primo risultato.
3. Ricerca di notizie di categoria in base a `movie` e `TV entertainment` con ricerca sicura, verifica del numero di risultati e stampa di categoria, nome, URL, descrizione, ora di pubblicazione e nome del provider del primo risultato.
4. Ricerca di argomenti di tendenza delle notizie in Bing, verifica del numero di risultati e stampa di nome, testo della query, `webSearchUrl`, `newsSearchUrl` e URL dell'immagine della notizia comparsa come primo risultato.

I Bing Search SDK rendono facilmente accessibile la funzionalità di ricerca Web nei linguaggi di programmazione seguenti:
* Introduzione agli [esempi .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [Pacchetto NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * Vedere anche [Librerie .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) per le definizioni e le dipendenze.
* Introduzione agli [esempi Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Vedere anche [Librerie Node.js](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) per le definizioni e le dipendenze.
* Introduzione agli [esempi Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Vedere anche [Librerie Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) per le definizioni e le dipendenze.
* Introduzione agli [esempi Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Vedere anche [Librerie Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) per le definizioni e le dipendenze.

Gli esempi dell'SDK per ogni linguaggio includono un file Leggimi con i dettagli sui prerequisiti e sull'installazione e l'esecuzione degli esempi.