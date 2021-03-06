---
title: Origini dei dati in Monitoraggio di Azure | Microsoft Docs
description: Descrive i dati disponibili per monitorare l'integrità e le prestazioni delle risorse di Azure e le applicazioni che si basano su tali dati.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2018
ms.author: bwren
ms.openlocfilehash: b10236a1e0307c9464d58e50eb0c7b4e6a60b5e5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987779"
---
# <a name="sources-of-data-in-azure-monitor"></a>Origini dei dati in Monitoraggio di Azure
Questo articolo descrive le origini dei dati raccolti da Monitoraggio di Azure per monitorare l'integrità e le prestazioni delle risorse e le applicazioni che si basano su tali dati. Queste risorse possono trovarsi in Azure, in un altro cloud o in locale.  Vedere [Dati raccolti da Monitoraggio di Azure](monitoring-data-collection.md) per informazioni dettagliate sul modo in cui vengono archiviati questi dati e su come visualizzarli.

I dati di monitoraggio in Azure provengono da varie origini che possono essere organizzate in livelli, dove il livello più alto è la propria applicazione unitamente ai sistemi operativi e quelli più bassi sono i componenti della piattaforma di Azure. Questa organizzazione è illustrata nel diagramma seguente e ogni livello è descritto in dettaglio nelle sezioni che seguono.

![Livelli dei dati di monitoraggio](media/monitoring-data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Tenant di Azure
I dati di telemetria correlati al tenant di Azure vengono raccolti da servizi a livello di tenant, ad esempio Azure Active Directory.

![Raccolta del tenant di Azure](media/monitoring-data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Log di controllo di Azure Active Directory
I [report di Azure Active Directory](../active-directory/reports-monitoring/overview-reports.md) contengono la cronologia dell'attività di accesso e l'audit trail delle modifiche apportate all'interno di un tenant specifico. Questi log di controllo possono essere scritti in Log Analytics per analizzarli insieme ad altri dati di log.


## <a name="azure-platform"></a>Piattaforma Azure
I dati di telemetria correlati all'integrità e al funzionamento di Azure stesso includono i dati sul funzionamento e sulla gestione della sottoscrizione di Azure. Includono i dati di integrità dei servizi archiviati nel log attività di Azure e nei log di controllo di Azure Active Directory.

![Raccolta della sottoscrizione di Azure](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Integrità dei servizi di Azure
[Integrità dei servizi di Azure](../monitoring-and-diagnostics/monitoring-service-notifications.md) presenta informazioni sull'integrità dei servizi di Azure nella sottoscrizione su cui si basano l'applicazione e le risorse in uso. È possibile creare avvisi che notifichino eventuali problemi critici attuali e previsti che potrebbero influire negativamente sull'applicazione. I record dell'integrità dei servizi vengono archiviati nel [log attività di Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), pertanto è possibile visualizzarli nell'utilità di esplorazione dei log attività e copiarli in Log Analytics.

### <a name="azure-activity-log"></a>Azure Activity Log
Il [log attività di Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) include i record di integrità dei servizi insieme ai record sulle modifiche alla configurazione apportate alle risorse di Azure. Il log attività è disponibile per tutte le risorse di Azure e ne rappresenta la visualizzazione _esterna_. I tipi specifici di record nel log attività sono descritti in [Azure Activity Log event schema](../monitoring-and-diagnostics/monitoring-activity-log-schema.md) (Schema degli eventi del log attività di Azure).

È possibile visualizzare il log attività per una risorsa specifica nella pagina corrispondente nel portale di Azure o visualizzare i log di più risorse nell'[utilità di esplorazione dei log attività](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). È particolarmente utile copiare le voci di log in Log Analytics per combinarle con altri dati di monitoraggio. È possibile inoltre inviarle ad altre posizioni usando gli [hub eventi](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md).



## <a name="azure-services"></a>Servizi di Azure.
I log di diagnostica a livello di metrica e di risorsa offrono informazioni sul funzionamento _interno_ delle risorse di Azure. Questi log sono disponibili per la maggior parte dei servizi di Azure e le soluzioni di gestione offrono informazioni approfondite sui servizi specifici.

![Raccolta di risorse di Azure](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Metriche
La maggior parte dei servizi di Azure genera [metriche di piattaforma](monitoring-data-collection.md#metrics) che ne riflettono le prestazioni e il funzionamento. Le [metriche specifiche variano in base al tipo di risorsa](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Sono accessibili da Esplora metriche e possono essere copiate in Log Analytics per eseguire l'analisi delle tendenze e altre analisi.


### <a name="resource-diagnostic-logs"></a>Log di diagnostica delle risorse
Mentre il log attività include informazioni sulle operazioni eseguite nelle risorse di Azure, i [log di diagnostica](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a livello di risorsa presentano informazioni approfondite sul funzionamento della risorsa stessa.   I requisiti di configurazione e il contenuto di questi log [variano in base al tipo di risorsa](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

Non è possibile visualizzare direttamente i log di diagnostica nel portale di Azure, ma è possibile [inviarli ad Archiviazione di Azure per l'archiviazione](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) ed esportarli nell'[hub eventi](../event-hubs/event-hubs-what-is-event-hubs.md) per il reindirizzamento ad altri servizi o a [Log Analytics](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) per l'analisi. Alcune risorse possono scrivere direttamente in Log Analytics mentre altre scrivono in un account di archiviazione prima di essere [importate in Log Analytics](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="monitoring-solutions"></a>Soluzioni di monitoraggio
 Le [soluzioni di monitoraggio](../monitoring/monitoring-solutions.md) raccolgono i dati per offrire informazioni dettagliate aggiuntive sul funzionamento di un determinato servizio o applicazione. Queste soluzioni raccolgono i dati in Log Analytics, dove possono essere analizzati usando il [linguaggio delle query](../log-analytics/log-analytics-log-search.md) o le [visualizzazioni](../log-analytics/log-analytics-view-designer.md) normalmente incluse nella soluzione.

## <a name="guest-operating-system"></a>Sistema operativo guest
Le risorse di calcolo in Azure, in altri cloud e in locale dispongono di un sistema operativo guest per il monitoraggio. Con l'installazione di uno o più agenti, è possibile raccogliere i dati di telemetria dal sistema operativo guest negli stessi strumenti di monitoraggio dei servizi di Azure.

![Raccolta di risorse di calcolo di Azure](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="diagnostic-extension"></a>Estensione di diagnostica
Con l'[estensione di diagnostica di Azure](../monitoring-and-diagnostics/azure-diagnostics.md) è possibile raccogliere i log e i dati sulle prestazioni dal sistema operativo client delle risorse di calcolo di Azure. Sia le metriche che i log raccolti dai client vengono archiviati in un account di archiviazione di Azure da cui è possibile [configurare Log Analytics per l'importazione](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).  Esplora metriche sa come leggere i dati dall'account di archiviazione e include quindi le metriche del client con le altre metriche raccolte.


### <a name="log-analytics-agent"></a>Agente di Log Analytics
È possibile installare l'agente di Log Analytics in qualsiasi computer fisico o macchina virtuale [Windows](../log-analytics/log-analytics-agent-windows.md) o [Linux](). La macchina virtuale può essere eseguita in Azure, un altro cloud o in locale.  L'agente si connette a Log Analytics direttamente o tramite un [gruppo di gestione System Center Operations Manager connesso](../log-analytics/log-analytics-om-agents.md) e consente di raccogliere i dati da [origini dati](../log-analytics/log-analytics-data-sources.md) configurate o da [soluzioni di gestione](../monitoring/monitoring-solutions.md) che offrono informazioni dettagliate aggiuntive sulle applicazioni in esecuzione nella macchina virtuale.

### <a name="service-map"></a>Elenco dei servizi
La soluzione [Mapping dei servizi](../operations-management-suite/operations-management-suite-service-map.md) richiede un Dependency Agent nelle macchine virtuali Windows e Linux. Opera con l'agente di Log Analytics per la raccolta dei dati sui processi in esecuzione nella macchina virtuale e sulle dipendenze da processi esterni. Archivia questi dati in Log Analytics e include una console che visualizza i dati che raccoglie in aggiunta ad altri dati memorizzati in Log Analytics.

## <a name="applications"></a>APPLICAZIONI
Oltre ai dati di telemetria che l'applicazione può scrivere nel sistema operativo guest, il monitoraggio dettagliato delle applicazioni viene eseguito con [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights raccoglie i dati da applicazioni in esecuzione su un'ampia gamma di piattaforme. L'applicazione può essere eseguita in Azure, un altro cloud o in locale.

![Raccolta di dati dell'applicazione](media/monitoring-data-sources/application-collection.png)


### <a name="application-data"></a>Dati dell'applicazione
Quando si abilita Application Insights per un'applicazione mediante l'installazione di un pacchetto di strumentazione, questo servizio raccoglie le metriche e i log relativi alle prestazioni e al funzionamento dell'applicazione. Sono incluse le informazioni dettagliate sulle visualizzazioni di pagina, le richieste dell'applicazione e le eccezioni. Application Insights archivia i dati che raccoglie in Metriche di Azure e in Log Analytics. Include strumenti estensivi per l'analisi dei dati, ma è anche possibile analizzarli con dati provenienti da altre origini usando strumenti come Esplora metriche e le ricerche nei log.

È anche possibile usare Application Insights per [creare una metrica personalizzata](../application-insights/app-insights-api-custom-events-metrics.md).  In questo modo è possibile definire la propria logica per calcolare un valore numerico e quindi archiviare tale valore con altre metriche che possono essere accessibili da Esplora metriche e usate per la [scalabilità automatica](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) e gli avvisi delle metriche.

### <a name="dependencies"></a>Dependencies
Per monitorare operazioni logiche diverse di un'applicazione, è necessario [raccogliere i dati di telemetria tra più componenti](../application-insights/app-insights-transaction-diagnostics.md). Application Insights supporta la [correlazione di dati di telemetria distribuita](../application-insights/application-insights-correlation.md) che identifica le dipendenze tra componenti, consentendo in tal modo di analizzarli insieme.

### <a name="availability-tests"></a>Test della disponibilità
I [test di disponibilità](../application-insights/app-insights-monitor-web-app-availability.md) in Application Insights consentono di verificare la disponibilità e velocità di risposta dell'applicazione da posizioni diverse nella rete Internet pubblica. È possibile effettuare un semplice test ping per verificare che l'applicazione sia attiva o usare Visual Studio per creare un test Web che simula uno scenario utente.  I test di disponibilità non richiedono alcuna strumentazione nell'applicazione.

## <a name="custom-sources"></a>Origini personalizzate
Oltre ai livelli standard di un'applicazione, potrebbe essere necessario monitorare altre risorse che contengono dati di telemetria che non possono essere raccolti con le altre origini dati. Per queste risorse è necessario scrivere tali dati tramite un'API di Monitoraggio di Azure.

![Raccolta di dati personalizzati](media/monitoring-data-sources/custom-collection.png)

### <a name="data-collector-api"></a>API dell'Agente di raccolta dati
Monitoraggio di Azure può raccogliere dati di log da qualsiasi client REST tramite l'[API dell'agente di raccolta dati](../log-analytics/log-analytics-data-collector-api.md). In questo modo è possibile creare scenari di monitoraggio personalizzati ed estendere il monitoraggio alle risorse che non espongono dati di telemetria tramite altre origini.

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni sui [tipi di dati di monitoraggio raccolti da Monitoraggio di Azure](monitoring-data-collection.md) e su come visualizzare e analizzare i dati.
