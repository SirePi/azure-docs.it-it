---
title: Configurare l'analisi di app Web per ASP.NET con Azure Application Insights | Microsoft Docs
description: Configurare gli strumenti di analisi delle prestazioni, della disponibilità e del comportamento degli utenti per un sito Web ASP.NET, ospitato in locale o in Azure.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: mbullwin
ms.openlocfilehash: 73bb1e3d06066c422614bc9d6d3431b49be9c6de
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992368"
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Installare Application Insights per un sito Web ASP.NET

Questa procedura consente di configurare un'app Web ASP.NET per l'invio di dati di telemetria al servizio [Azure Application Insights](app-insights-overview.md). È valida per le app ASP.NET ospitate nel server IIS locale o nel cloud. Offre grafici e un linguaggio di query avanzato che permettono di comprendere le prestazioni dell'app e il suo utilizzo da parte degli utenti, oltre ad avvisi automatici in caso di errori o problemi di prestazioni. Molti sviluppatori trovano utili queste funzionalità così come sono, ma è anche possibile estendere e personalizzare i dati di telemetria, se necessario.

Il programma di installazione richiede pochi clic in Visual Studio. Per evitare addebiti è possibile limitare il volume dei dati di telemetria. In questo modo è possibile provare le funzionalità ed eseguire il debug o monitorare un sito con un numero di utenti limitato. Se si decide di monitorare l'intero sito di produzione, è facile aumentare il limite in un secondo momento.

## <a name="prerequisites"></a>Prerequisiti
Per aggiungere Application Insights al sito Web ASP.NET è necessario:

- Installare [Visual Studio 2017 per Windows](https://www.visualstudio.com/downloads/) con i carichi di lavoro seguenti:
    - Sviluppo Web e ASP.NET
    - Sviluppo di Azure

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="ide"></a>Passaggio 1: Aggiungere Application Insights SDK

> [!IMPORTANT]
> Il processo di aggiunta di Application Insights varia in base al tipo di modello ASP.NET. Se si usa il modello **Vuoto** o **App per dispositivi mobili di Azure**, selezionare **Progetto** > **Aggiungi Application Insights Telemetry**. Per tutti gli altri modelli ASP.NET, vedere le istruzioni seguenti. 

In Esplora soluzioni fare clic con il pulsante destro del mouse sull'app Web e scegliere **Configura Application Insights**

![Screenshot di Esplora soluzioni con l'opzione Configura Application Insights evidenziata](./media/app-insights-asp-net/0001-configure-application-insights.png)

A seconda della versione di Application Insights SDK potrebbe essere richiesto di eseguire l'aggiornamento alla versione più recente dell'SDK. Quando richiesto, selezionare **Aggiorna SDK**.

![Screenshot: È disponibile una nuova versione di Microsoft Application Insights SDK. Opzione Aggiorna SDK evidenziata](./media/app-insights-asp-net/0002-update-sdk.png)

Schermata di configurazione di Application Insights:

Selezionare **Inizia gratis**.

![Screenshot della pagina Registra l'app con Application Insights](./media/app-insights-asp-net/0004-start-free.png)

Se si vuole impostare il gruppo di risorse o la località in cui verranno archiviati i dati, fare clic su **Configura impostazioni**. I gruppi di risorse vengono usati per controllare l'accesso ai dati. Se si hanno diverse app che fanno parte dello stesso sistema, ad esempio, è possibile inserire i relativi dati di Application Insights nello stesso gruppo di risorse.

 Selezionare **Registra**. 

![Screenshot della pagina Registra l'app con Application Insights](./media/app-insights-asp-net/0005-register-ed.png)

 I dati di telemetria verranno inviati al [portale di Azure](https://portal.azure.com), sia durante il debug che dopo la pubblicazione dell'app.
> [!NOTE]
> Per non inviare i dati di telemetria al portale durante il debug, è possibile aggiungere Application Insights SDK all'app senza configurare una risorsa nel portale. È possibile visualizzare i dati di telemetria in Visual Studio durante il debug. Successivamente, è possibile tornare a questa pagina di configurazione oppure attendere di aver distribuito l'app e quindi [attivare la telemetria in fase di esecuzione](app-insights-monitor-performance-live-website-now.md).

## <a name="run"></a>Passaggio 2: Eseguire l'app
Eseguire l'app con F5. Aprire pagine diverse per generare alcuni dati di telemetria.

In Visual Studio verrà visualizzato il conteggio degli eventi che sono stati registrati.

![Screenshot di Visual Studio. Il pulsante Application Insights viene visualizzato durante il debug.](./media/app-insights-asp-net/0006-Events.png)

## <a name="step-3-see-your-telemetry"></a>Passaggio 3: Visualizzare i dati di telemetria
È possibile visualizzare i dati di telemetria in Visual Studio o nel portale Web di Application Insights. Cercare i dati di telemetria in Visual Studio per eseguire il debug dell'app. Monitorare le prestazioni e l'utilizzo nel portale Web quando il sistema è attivo. 

### <a name="see-your-telemetry-in-visual-studio"></a>Visualizzare i dati di telemetria in Visual Studio

Per visualizzare i dati di Application Insights, in Visual Studio  selezionare **Esplora soluzioni** > **Servizi connessi** > fare clic con il pulsante destro del mouse su **Application Insights** e quindi scegliere **Cerca nei dati di telemetria attivi**.

Nella finestra Ricerca di Application Insights di Visual Studio verranno visualizzati i dati di telemetria dell'applicazione generati sul lato server dell'app. Sperimentare i filtri e fare clic su qualsiasi evento per visualizzare altri dettagli.

![Screenshot della visualizzazione Dati di Dati di telemetria della sessione di debug nella finestra di Application Insights.](./media/app-insights-asp-net/55.png)

> [!Tip]
> Se non vengono visualizzati dati, verificare che l'intervallo di tempo sia corretto e fare clic sull'icona di ricerca.

[Uso di Application Insights in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Visualizzare i dati di telemetria nel portale Web

Se non si è scelto di installare solo l'SDK, è possibile visualizzare i dati di telemetria anche nel portale Web di Application Insights. Il portale offre un maggior numero di grafici, strumenti di analisi e viste di più componenti rispetto a Visual Studio. Nel portale sono anche disponibili avvisi.

Aprire la risorsa Application Insights. Accedere al [portale di Azure](https://portal.azure.com/) per cercarla oppure selezionare **Esplora soluzioni** > **Servizi connessi** > fare clic con il pulsante destro del mouse su **Application Insights** > **Apri portale Application Insights**.

Nel portale verrà visualizzata la telemetria dell'app.

![Screenshot della pagina Panoramica di Application Insights](./media/app-insights-asp-net/66.png)

Per visualizzare altri dettagli nel portale, fare clic su qualsiasi riquadro o grafico.

[Altre informazioni sull'uso di Application Insights nel portale di Azure](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Passaggio 4: Pubblicare l'app
Pubblicare l'app nel server IIS o in Azure. Verificare in [Flusso metriche attive](app-insights-metrics-explorer.md#live-metrics-stream) che tutto funzioni correttamente.

La telemetria viene creata nel portale di Application Insights, in cui è possibile monitorare le metriche, eseguire ricerche sui dati di telemetria e configurare i [dashboard](app-insights-dashboards.md), nonché usare l'avanzato [linguaggio di query di Log Analytics](https://docs.loganalytics.io/) per analizzare l'utilizzo e le prestazioni o trovare eventi specifici.

È anche possibile continuare ad analizzare i dati di telemetria in [Visual Studio](app-insights-visual-studio.md) con strumenti come la ricerca diagnostica e le [tendenze](app-insights-visual-studio-trends.md).

> [!NOTE]
> Se la quantità di dati di telemetria inviata dall'app sta per raggiungere le [limitazioni](app-insights-pricing.md#limits-summary), viene attivato il [campionamento](app-insights-sampling.md) automatico. Il campionamento riduce la quantità di dati di telemetria inviata dall'app mantenendo i dati correlati per scopi diagnostici.
>
>

## <a name="land"></a> Le impostazioni sono state completate.

Congratulazioni! Il pacchetto Application Insights è stato installato nell'app e configurato per l'invio di dati di telemetria al servizio Application Insights in Azure.

![Diagramma dello spostamento dei dati di telemetria](./media/app-insights-asp-net/01-scheme.png)

La risorsa di Azure che riceve i dati di telemetria dell'app è identificata da una *chiave di strumentazione*, disponibile nel file ApplicationInsights.config.


## <a name="upgrade-to-future-sdk-versions"></a>Eseguire l'aggiornamento alle versioni future dell'SDK
Per eseguire l'aggiornamento a una [nuova versione dell'SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), aprire **Gestione pacchetti NuGet** e filtrare i pacchetti installati. Selezionare **Microsoft.ApplicationInsights.Web** e scegliere **Aggiorna**.

Se sono state apportate personalizzazioni a ApplicationInsights.config, salvarne una copia prima di eseguire l'aggiornamento. Successivamente, unire le modifiche nella nuova versione.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Passaggi successivi

Se si è interessati, vedere gli argomenti alternativi seguenti:

* [Strumentazione di un'app Web in fase di esecuzione](app-insights-monitor-performance-live-website-now.md)
* [Servizi cloud di Azure](app-insights-cloudservices.md)

### <a name="more-telemetry"></a>Altri dati di telemetria

* **[Dati sul browser e sul caricamento di pagine](app-insights-javascript.md)**: inserire un frammento di codice nelle pagine Web.
* **[Ottenere un monitoraggio più dettagliato di dipendenze ed eccezioni](app-insights-monitor-performance-live-website-now.md)**: installare Status Monitor nel server.
* **[Scrivere codice per gli eventi personalizzati](app-insights-api-custom-events-metrics.md)**: ottenere conteggi, orari o misurazioni delle azioni utente.
* **[Ottenere dati di log](app-insights-asp-net-trace-logs.md)**: correlare i dati di log con i dati di telemetria.

### <a name="analysis"></a>Analisi

* **[Uso di Application Insights in Visual Studio](app-insights-visual-studio.md)**<br/>Include informazioni su debug con telemetria, ricerca diagnostica e drill-through nel codice.
* **[Uso del portale Application Insights](app-insights-dashboards.md)**<br/> Include informazioni su dashboard, strumenti avanzati di diagnostica e di analisi, avvisi, mappa attiva delle dipendenze dell'applicazione ed esportazione dei dati di telemetria.
* **[Analytics](app-insights-analytics-tour.md)**: linguaggio di query avanzato.

### <a name="alerts"></a>Avvisi

* [Test di disponibilità](app-insights-monitor-web-app-availability.md): creare test per verificare che il sito sia visibile sul Web.
* [Diagnostica intelligente](app-insights-proactive-diagnostics.md): questi test vengono eseguiti automaticamente e non è quindi necessario effettuare alcuna operazione per configurarli. Se l'app ha una frequenza insolita di richieste non riuscite, verrà comunicato automaticamente.
* [Avvisi per le metriche](app-insights-alerts.md): impostare questi avvisi per essere avvertiti se una metrica supera una soglia. È possibile impostarli nelle metriche personalizzate di cui si scrive il codice nell'app.

### <a name="automation"></a>Automazione

* [Automatizzare la creazione di risorse di Application Insights](app-insights-powershell.md)
