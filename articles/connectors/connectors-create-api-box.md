---
title: Connettersi a Box - App per la logica di Azure | Microsoft Docs
description: Creare e gestire i file con le API REST di Box e App per la logica di Azure
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: b5c8c18c6d02710646560f29d4bc7b5784f730a2
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746878"
---
# <a name="create-and-manage-files-in-box-with-azure-logic-apps"></a>Creare e gestire file in Box con le App per la logica di Azure

Questo articolo illustra come creare e gestire i file in Box all'interno di un'app per la logica con il connettore Box. In questo modo, è possibile creare app per la logica che automatizzano le attività e i flussi di lavoro per la gestione dei file e altre azioni, ad esempio:

* Creare il flusso aziendale in base ai dati ottenuti da Box. 

* Attivare attività e flussi di lavoro automatizzati quando viene creato o aggiornato un file.

* Eseguire le azioni per copiare un file, eliminarlo e così via. 

  Quando queste azioni ottengono una risposta, rendono l'output disponibile per altre azioni. 
  Ad esempio, quando un file viene modificato in Box, è possibile usare il file nella posta elettronica tramite Office 365.

## <a name="prerequisites"></a>Prerequisiti

* Un [Account Box](https://www.box.com/home)

* Una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, <a href="https://azure.microsoft.com/free/" target="_blank">iscriversi per creare un account Azure gratuito</a>. 

* L'app per la logica in cui si intende accedere all'account Box. Per avviare l'app per la logica con un trigger Box, è necessaria un'[app per la logica vuota](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Conoscenza di base di [come creare le app per la logica](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Se non si ha familiarità con le app per la logica, consultare [Informazioni su App per la logica di Azure](../logic-apps/logic-apps-overview.md).

## <a name="connector-reference"></a>Informazioni di riferimento sui connettori

Per informazioni tecniche, ad esempio su trigger, azioni e limiti indicati nel file Swagger del connettore, vedere la [pagina di riferimento del connettore](/connectors/box/). 

## <a name="get-support"></a>Supporto

* In caso di domande, visitare il [forum di App per la logica di Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Per votare o inviare idee relative alle funzionalità, visitare il [sito dei commenti e suggerimenti degli utenti di App per la logica](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passaggi successivi

* Informazioni su altri [connettori di App per la logica](../connectors/apis-list.md)