---
title: File di inclusione
description: File di inclusione
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0dcb608553d4455403c073e34e83bccb81cc64db
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38750096"
---
Un'attività in esecuzione in Azure Batch può produrre dati di output quando viene eseguita. I dati di output delle attività spesso devono essere archiviati per consentirne il recupero da parte di altre attività del processo, dell'applicazione client che ha eseguito il processo o di entrambe. Le attività scrivono i dati di output nel file system di un nodo di calcolo di Batch, ma tutti i dati presenti nel nodo vanno persi quando ne viene ricreata l'immagine o quando il nodo lascia il pool. Le attività possono anche avere un periodo di memorizzazione dei file, dopo il quale i file creati dall'attività vengono eliminati. Per questi motivi, è importante salvare in modo permanente l'output delle attività che sarà necessario in seguito in un archivio dati, ad esempio [Archiviazione di Azure](https://docs.microsoft.com/azure/storage/).

Per le opzioni dell'account di archiviazione in Batch, vedere [Panoramica delle funzionalità di Batch](../articles/batch/batch-api-basics.md#azure-storage-account).
