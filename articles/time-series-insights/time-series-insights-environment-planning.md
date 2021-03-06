---
title: Ridimensionare l'ambiente Time Series Insights di Azure | Microsoft Docs
description: In questo articolo viene descritto come seguire le procedure consigliate durante la pianificazione di un ambiente Azure Time Series Insights, ad esempio relativamente a capacità di archiviazione, conservazione dei dati, capacità in ingresso, monitoraggio e BCDR (business disaster recovery).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 2c06463d95467543a426079addf981aa42d53eb6
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630637"
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Pianificare l'ambiente Azure Time Series Insights

In questo articolo viene descritto come pianificare l'ambiente Azure Time Series Insights in base alla velocità in ingresso e ai requisiti di conservazione dei dati.

## <a name="best-practices"></a>Procedure consigliate

Per iniziare a usare Time Series Insights, è prima opportuno valutare il volume di dati di cui si prevede di eseguire il push al minuto e la durata del periodo di archiviazione dei dati.  

Per altre informazioni sulla capacità e sulla conservazione per entrambi gli SKU di Time Series Insights, vedere [Prezzi di Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

Prendere in considerazione gli attributi seguenti per ottimizzare la pianificazione a lungo termine dell'ambiente: 
- Capacità di archiviazione
- Periodo di conservazione dei dati
- Capacità in ingresso 
- Forme degli eventi
- Assicurarsi di avere dati di riferimento a disposizione

## <a name="understand-storage-capacity"></a>Introduzione alla capacità di archiviazione
Per impostazione predefinita, Time Series Insights conserva i dati in base alla quantità di spazio di archiviazione di cui è stato eseguito il provisioning (unità moltiplicate per la quantità di spazio di archiviazione per unità) e al traffico in ingresso.

## <a name="understand-data-retention"></a>Informazioni sulla conservazione dei dati
Per l'ambiente Time Series Insights è possibile configurare il **periodo di conservazione dei dati** fino a un massimo di 400 giorni.  Time Series Insights dispone di due modalità: la prima consente l'ottimizzazione dell'ambiente per garantire che siano inclusi i dati più aggiornati (impostazione predefinita), mentre la seconda consente l'ottimizzazione dell'ambiente per garantire la conformità con i limiti di conservazione e sospendere il traffico in ingresso non appena viene raggiunta la capacità di archiviazione complessiva definita.  È possibile adeguare il periodo di conservazione e passare alternativamente tra le due modalità nella pagina di configurazione dell'ambiente nel portale di Azure.

Nell'ambiente Time Series Insights è possibile configurare un massimo di 400 giorni di conservazione dei dati.

## <a name="configure-data-retention"></a>Configurare la conservazione dei dati

1. Nel [portale di Azure](https://portal.azure.com) selezionare l'ambiente Time Series Insights desiderato.

2. Nella **pagina dell'ambiente Time Series Insights**, sotto il titolo **Impostazioni** selezionare **Configura**. 

3. Nella casella **Periodo di conservazione dei dati (in giorni)** immettere un valore compreso tra 1 e 400.

   ![Configurare la conservazione](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Informazioni sulla capacità in ingresso

L'altro fattore da considerare durante la pianificazione è la capacità in ingresso, il cui valore deriva dall'allocazione al minuto. 

Dal punto di vista della limitazione, un pacchetto di dati in ingresso la cui dimensione è 32 KB viene interpretato come 32 eventi, ciascuno con dimensioni pari a 1 KB. La dimensione massima consentita per ogni evento è pari a 32 KB. Pertanto, i pacchetti di dati con dimensioni maggiori di 32 KB vengono troncati.

Nella tabella seguente è riportata la capacità in ingresso per ogni SKU:

|SKU  |Conteggio degli eventi al mese, per unità  |Dimensioni degli eventi al mese, per unità  |Conteggio degli eventi al minuto, per unità  | Dimensioni al minuto, per unità   |
|---------|---------|---------|---------|---------|
|S1     |   30 milioni     |  30 GB     |  700    |  700 KB   |
|S2     |   300 milioni    |   300 GB   | 7.000   | 7.000 KB  |

È possibile aumentare la capacità di uno SKU S1 o S2 a 10 unità in un unico ambiente. Non è possibile eseguire la migrazione da un ambiente S1 a un ambiente S2 o viceversa. 

Per la capacità in ingresso, è prima necessario determinare la capacità in ingresso totale richiesta al mese. Determinare quindi la capacità in ingresso richiesta al minuto, poiché questo dato è interessato dalla limitazione e dalla latenza.

Se si verifica un picco del traffico di dati in ingresso che dura meno di 24 ore, Time Series Insights può gestirlo a una velocità in ingresso doppia rispetto alle velocità sopra elencate. 

Ad esempio, se è presente un'unica SKU S1 e i dati in ingresso hanno una velocità pari a 700 eventi al minuto, in presenza di un picco che dura meno di un'ora a una velocità di 1400 eventi o meno, non si noterà alcuna latenza evidente a livello di ambiente. Tuttavia, se si superano 1400 eventi al minuto per più di un'ora, è possibile che nell'ambiente venga rilevata latenza a livello di dati visualizzati e disponibili per le query. 

Non è possibile determinare in anticipo la quantità di dati di cui si prevede di eseguire il push. In questo caso è possibile fare riferimento ai dati di telemetria relativi a [Hub IoT di Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) e [Hub eventi di Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) nel portale di Azure. Questi dati di telemetria consentono di determinare come eseguire il provisioning dell'ambiente. Nel portale di Azure usare la pagina **Metriche** relativa all'origine dell'evento desiderata per visualizzare i dati di telemetria corrispondenti. Dopo aver analizzato attentamente le metriche relative all'origine evento, è possibile eseguire in modo più efficiente la pianificazione e il provisioning dell'ambiente Time Series Insights.

### <a name="calculate-ingress-requirements"></a>Calcolare i requisiti del traffico in ingresso

- Verificare se la capacità in ingresso è superiore alla velocità media al minuto e se le dimensioni dell'ambiente sono sufficienti per la gestione del traffico in ingresso previsto a una capacità doppia per meno di un'ora.

- Se si verificano picchi del traffico in ingresso che durano più di un'ora, usare la velocità di picco come media ed eseguire il provisioning di un ambiente usando tale capacità per gestire la velocità di picco.
 
### <a name="mitigate-throttling-and-latency"></a>Ridurre la limitazione e la latenza

Per informazioni su come evitare la limitazione e la latenza, vedere [Ridurre la limitazione e la latenza](time-series-insights-environment-mitigate-latency.md). 

## <a name="shaping-your-events"></a>Forme degli eventi
È importante assicurarsi che la modalità di invio degli eventi a Time Series Insights usata supporti le dimensioni dell'ambiente di cui si esegue il provisioning. In alternativa, è possibile mappare le dimensioni dell'ambiente al numero di eventi letti da Time Series Insights e alle dimensioni di ogni evento.  Analogamente, è importante tenere conto degli attributi che potrebbe essere necessario sezionare e filtrare durante l'esecuzione di query sui dati.  Considerando questi aspetti, è consigliabile esaminare la sezione dedicata alle forme JSON supportate nella documentazione *Inviare eventi* [documentazione] (https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).  Questa sezione è verso la fine della pagina.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Assicurarsi di avere dati di riferimento a disposizione
Un set di dati di riferimento è una raccolta di elementi che aumentano gli eventi dell'origine evento. Il motore in ingresso di Time Series Insights crea un join tra ogni evento dell'origine evento e la riga di dati corrispondente nel set di dati di riferimento. Questo evento aumentato sarà quindi disponibile per le query. Questo join è basato sulla colonna o le colonne di chiavi primarie definite nel set di dati di riferimento.

Si noti che non viene creato un join dei dati di riferimento in maniera retroattiva. Questo significa che solo i dati in ingresso correnti e futuri vengono uniti in join al set di dati di riferimento, appena viene configurato e caricato.  Se si prevede di inviare molti dati cronologici a Time Series Insights e non si caricano o creano i dati di riferimento in Time Series Insights prima di tutto, potrebbe essere necessario rieseguire il lavoro.  

Per altre informazioni su come creare, caricare e gestire i dati di riferimento in Time Series Insights, vedere la documentazione dedicata ai *dati di riferimento* [documentazione](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="business-disaster-recovery"></a>BCDR (Business disaster recovery)
Come servizio di Azure, Time Series Insights offre velocità elevata usando le ridondanze a livello di area di Azure, senza richiedere attività aggiuntive alla soluzione. La piattaforma Microsoft Azure offre anche numerose funzionalità che facilitano la compilazione di soluzioni con funzionalità di ripristino di emergenza o disponibilità tra aree. Per offrire la disponibilità elevata globale tra le aree per dispositivi o utenti, sfruttare le funzionalità di ripristino di emergenza di Azure. L'articolo [Indicazioni tecniche sulla resilienza di Azure](../resiliency/resiliency-technical-guidance.md) descrive le funzionalità integrate in Azure per la continuità aziendale e il ripristino di emergenza. Il documento [Ripristino di emergenza per le applicazioni basate su Azure][Ripristino di emergenza per le applicazioni basate su Azure] fornisce informazioni sull'architettura nelle strategie per consentire alle applicazioni di Azure di ottenere disponibilità elevata e ripristino di emergenza.

Time Series Insights non dispone di BCDR incorporato.  Tuttavia, i clienti che richiedono il BCDR possono implementare una strategia di ripristino. Creare un secondo ambiente Time Series Insights in un'area di Azure di backup e inviare eventi a questo ambiente secondario dall'origine eventi primaria, sfruttando un secondo gruppo di consumer dedicato e le linee guida BCDR dell'origine dell'evento.  

1.  Creare l'ambiente nella seconda area.  Altre informazioni sulla creazione di una risorsa di Azure Time Series Insights sono reperibili [qui](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-get-started).
2.  Creare un secondo gruppo di consumer dedicato all'origine eventi e connettere tale origine evento al nuovo ambiente.  Assicurarsi di designare il secondo gruppo di consumer dedicato.  Per altre informazioni su questo argomento, vedere la [Documentazione sull'hub IoT](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) o la [Documentazione sull'hub eventi](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-data-access).
3.  Se l'area primaria si disattiva durante un evento imprevisto di emergenza, spostare le operazioni nell'ambiente di backup di Time Series Insights.  

Per altre informazioni sui criteri di BCDR dell'hub IoT, vedere [qui](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-ha-dr).  Per altre informazioni sui criteri di BCDR dell'hub eventi, vedere [qui](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-geo-dr).  

## <a name="next-steps"></a>Passaggi successivi
- [Come aggiungere un'origine evento di un hub eventi](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Come aggiungere un'origine evento di un hub IoT](time-series-insights-how-to-add-an-event-source-iothub.md)
