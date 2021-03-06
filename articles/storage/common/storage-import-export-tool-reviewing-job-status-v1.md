---
title: Esame dello stato del processo di importazione/esportazione di Azure - versione 1 | Documentazione Microsoft
description: Informazioni su come usare i file di log creati quando il processo di importazione o esportazione è stato eseguito per visualizzare lo stato del processo di importazione/esportazione.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: c5b9d1993c9e90411c7b05d9874721a159275f22
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44021829"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Esame dello stato del processo di Importazione/Esportazione di Azure con i file di log di copia
Quando il servizio Importazione/Esportazione di Microsoft Azure elabora unità associate a un processo di importazione o esportazione, scrive file di log di copia per l'account di archiviazione verso cui o da cui si importano o si esportano BLOB. Il file di log contiene lo stato dettagliato di ogni file importato o esportato. L'URL ad ogni file di log di copia viene restituito quando si esegue una query sullo stato di un processo completato. Per ulteriori informazioni, vedere [Get Job](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get).  

## <a name="example-urls"></a>URL di esempio

Di seguito sono mostrati URL di esempio per i file di log di copia per un processo di importazione con due unità:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Per informazioni sul formato dei log di copia e l'elenco completo dei codici di stato, vedere [Formato dei file di log del servizio Importazione/Esportazione di Azure](../storage-import-export-file-format-log.md).  
  
## <a name="next-steps"></a>Passaggi successivi
 
 * [Configurazione dello strumento Importazione/Esportazione di Azure](storage-import-export-tool-setup-v1.md)   
 * [Preparazione dei dischi rigidi per un processo di importazione](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Repairing an import job](../storage-import-export-tool-repairing-an-import-job-v1.md) (Riparazione di un processo di importazione)   
 * [Repairing an export job](../storage-import-export-tool-repairing-an-export-job-v1.md) (Riparazione di un processo di esportazione)   
 * [Risoluzione dei problemi relativi allo strumento Importazione/Esportazione di Azure](storage-import-export-tool-troubleshooting-v1.md)
