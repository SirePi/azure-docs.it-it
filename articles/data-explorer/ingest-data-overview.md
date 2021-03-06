---
title: Inserimento dati in Esplora dati di Azure
description: Informazioni sui diversi modi di inserire (caricare) i dati in Esplora dati di Azure
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 94f96d949f2a05f71e9565fdcbc7b48ed2c2a5c5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972660"
---
# <a name="azure-data-explorer-data-ingestion"></a>Inserimento dati in Esplora dati di Azure

L'inserimento dati è il processo usato per caricare i record di dati da una o più origini per creare o aggiornare una tabella in Esplora dati di Azure. Una volta inseriti, i dati diventano disponibili per le query. Il diagramma seguente illustra il flusso end-to-end per usare Esplora dati di Azure, incluso l'inserimento dati **(2)**.

![Flusso di dati complessivo](media/ingest-data-overview/overall-data-flow.png)

Il servizio di gestione dei dati Esplora dati di Azure, responsabile dell'inserimento dati, offre le funzionalità seguenti:

1. **Pull dei dati**: esegue il pull dei dati dalle origini esterne (hub eventi) o legge le richieste di inserimento da una coda di Azure.

1. **Invio in batch**: invia in batch i flussi di dati allo stesso database e alla stessa tabella per ottimizzare la velocità effettiva di inserimento.

1. **Convalida**: convalida preliminare e conversione del formato, se necessario.

1. **Manipolazione dei dati**: schema corrispondente, organizzazione, indicizzazione, codifica e compressione dei dati.

1. **Punto di persistenza nel flusso di inserimento**: gestisce il carico dell'inserimento sul motore e i nuovi tentativi in caso di errori temporanei.

1. **Commit dell'inserimento dati**: rende i dati disponibili per le query.

> [!NOTE]
> I criteri di conservazione effettivi dei dati inseriti sono derivati dai criteri di conservazione del database. Per informazioni dettagliate, vedere [Criteri di conservazione](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy). Per inserire i dati, sono necessarie le autorizzazioni per l'**inserimento nella tabella** o per l'**inserimento nel database**.

## <a name="ingestion-methods"></a>Metodi di inserimento

Esplora dati di Azure supporta diversi metodi di inserimento, ognuno dei quali presenta scenari di destinazione, vantaggi e svantaggi. Esplora dati di Azure offre connettori ai servizi comuni, l'inserimento a livello di codice tramite gli SDK e l'accesso diretto al motore per scopi di esplorazione.

### <a name="ingestion-using-connectors"></a>Inserimento tramite connettori

Esplora dati di Azure supporta attualmente il connettore dell'hub eventi, che può essere gestito usando la gestione guidata nel portale di Azure. Per altre informazioni, vedere [Quickstart: Ingest data from Event Hub into Azure Data Explorer](ingest-data-event-hub.md) (Guida introduttiva: Inserire dati dall'hub eventi a Esplora dati di Azure).

### <a name="programmatic-ingestion"></a>Inserimento a livello di codice

Esplora dati Azure fornisce SDK che possono essere usati per l'inserimento dati e le query. L'inserimento a livello di codice è ottimizzato per diminuire i costi di inserimento, riducendo al minimo le transazioni di archiviazione durante e dopo il processo di inserimento.

**SDK disponibili e progetti open source**:

Kusto offre un SDK client che può essere usato per inserire ed eseguire query sui dati con:

* [Python SDK](https://docs.microsoft.com/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](https://docs.microsoft.com/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](https://docs.microsoft.com/azure/kusto/api/java/kusto-java-client-library)

* [API REST](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Tecniche di inserimento a livello di codice**:

* Inserimento dei dati direttamente nel motore Esplora dati di Azure (il più appropriato per l'esplorazione e la creazione di prototipi):

  * **Inserimento inline**: il comando di controllo (.ingest inline) contenente i dati in banda è destinato ai test ad hoc.

  * **Inserimento da query**: il comando di controllo (.set, .set-or-append, .set-or-replace) che fa riferimento ai risultati della query viene usato per la generazione di report o di tabelle temporanee di piccole dimensioni.

  * **Inserimento da risorsa di archiviazione**: il comando di controllo (.ingest into) con i dati archiviati esternamente (ad esempio in Archiviazione BLOB di Azure) consente un inserimento in blocco efficiente dei dati.

* Inserimento di dati tramite il servizio di gestione dati Esplora dati di Azure (velocità effettiva elevata e inserimento affidabile)

  * [**Inserimento in batch**](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (fornito dall'SDK): il client carica i dati in Archiviazione BLOB di Azure (designato dal servizio di gestione dati Esplora dati di Azure) e invia una notifica a una coda di Azure. Si tratta della tecnica consigliata per inserire grandi volumi di dati in modo affidabile ed economico.

**Latenza dei diversi metodi**:

| Metodo | Latenza |
| --- | --- |
| **Inserimento inline** | Immediato |
| **Inserimento da query** | Tempo di query + tempo di elaborazione |
| **Inserimento da risorsa di archiviazione** | Tempo di download + tempo di elaborazione |
| **Inserimento in coda** | Tempo di invio in batch + tempo di elaborazione |
| |

Il tempo di elaborazione dipende dalle dimensioni dei dati. In genere richiede pochi secondi. Il tempo di invio in batch predefinito è pari a 5 minuti.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>Scelta del metodo di inserimento più appropriato

Prima di iniziare a inserire i dati, è consigliabile porsi le domande seguenti.

* Dove si trovano i dati? 
* Qual è il formato dei dati? Può essere modificato? 
* Su quali deve essere eseguita la query? 
* Quali sono la velocità e il volume dei dati previsti? 
* Quanti tipi di eventi sono previsti (indicati come numero di tabelle)? 
* Con quale frequenza si prevede che lo schema degli eventi venga modificato? 
* Quanti nodi genereranno i dati? 
* Qual è il sistema operativo di origine? 
* Quali sono i requisiti relativi alla latenza? 
* È possibile usare una delle pipeline di inserimento gestite esistenti? 

Per le organizzazioni con un'infrastruttura esistente basata su un servizio di messaggistica, ad esempio l'hub eventi, l'uso di un connettore è probabilmente la soluzione più appropriata. L'inserimento in coda è adatto per grandi volumi di dati.

## <a name="supported-data-formats"></a>Formati di dati supportati

Per tutti i metodi di inserimento diversi dall'inserimento da query, i dati devono essere formattati in uno dei formati supportati in modo che Esplora dati di Azure possa analizzarli.

* CSV, TSV, PSV, SCSV, SOH
* JSON (separato da righe, multiriga), Avro
* ZIP e GZIP 

> [!NOTE]
> Quando i dati vengono inseriti, i tipi di dati vengono dedotti in base alle colonne della tabella di destinazione. Se un record è incompleto o un campo non può essere analizzato come tipo di dati necessario, le colonne della tabella corrispondenti verranno popolate con valori Null.

## <a name="schema-mapping"></a>Mapping dello schema

Il mapping dello schema consente di associare in modo deterministico i campi dati di origine alle colonne della tabella di destinazione.

* Il [mapping CSV](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#csv-mapping) (facoltativo) usa tutti i formati basati sull'ordinale ed è possibile passarlo come parametro del comando di inserimento o [crearlo anticipatamente nella tabella](https://docs.microsoft.com/azure/kusto/management/tables?branch=master#create-ingestion-mapping) e farvi riferimento dal parametro del comando di inserimento.
* È possibile passare il [mapping JSON](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#json-mapping) (obbligatorio) e il [mapping Avro](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#avro-mapping) (obbligatorio) come parametro del comando di inserimento o [crearli anticipatamente nella tabella](https://docs.microsoft.com/azure/kusto/management/tables#create-ingestion-mapping) e farvi riferimento dal parametro del comando di inserimento.

## <a name="next-steps"></a>Passaggi successivi

[Quickstart: Ingest data from Event Hub into Azure Data Explorer (Guida introduttiva: Inserire dati dall'hub eventi a Esplora dati di Azure)](ingest-data-event-hub.md)

[Guida introduttiva: Inserire dati usando la libreria di Esplora dati di Azure per Python](python-ingest-data.md)

