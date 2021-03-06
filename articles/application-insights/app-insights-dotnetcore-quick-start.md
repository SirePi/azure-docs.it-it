---
title: Guida introduttiva per Azure Application Insights | Microsoft Docs
description: Istruzioni per configurare rapidamente un'app Web ASP.NET Core per il monitoraggio con Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 07/11/2018
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 008e61841611f36c440bb4896ae5a85d0bf4d874
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991624"
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>Iniziare a monitorare l'applicazione Web ASP.NET Core

Con Azure Application Insights, è possibile monitorare facilmente la disponibilità, le prestazioni e l'uso dell'applicazione Web. È anche possibile identificare e diagnosticare rapidamente gli errori nell'applicazione senza attendere che vengano segnalati da un utente. 

Questa guida introduttiva illustra l'aggiunta di Application Insights SDK a un'applicazione Web ASP.Net Core esistente. 

## <a name="prerequisites"></a>Prerequisiti

Per completare questa guida introduttiva:

- Installare [Visual Studio 2017](https://www.visualstudio.com/downloads/) con i carichi di lavoro seguenti:
  - Sviluppo Web e ASP.NET
  - Sviluppo di Azure
- [Installare .NET Core 2.0 SDK](https://www.microsoft.com/net/core)
- Saranno necessarie una sottoscrizione di Azure e un'applicazione Web .NET Core esistente.

Se non si ha un'applicazione Web ASP.NET Core, è possibile usare la guida dettagliata per [creare un'app ASP.NET Core e aggiungere Application Insights](app-insights-asp-net-core.md).

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="log-in-to-the-azure-portal"></a>Accedere al Portale di Azure

Accedere al [Portale di Azure](https://portal.azure.com/).

## <a name="enable-application-insights"></a>Abilitare Application Insights

Application Insights può raccogliere dati di telemetria da un'applicazione connessa a Internet, indipendentemente dal fatto che sia in esecuzione in locale o nel cloud. Usare la procedura seguente per iniziare a visualizzare questi dati.

1. Selezionare **Crea una risorsa** > **Monitoraggio e gestione** > **Application Insights**.

   ![Aggiunta di una risorsa di Application Insights](./media/app-insights-dotnetcore-quick-start/0001-dc.png)

    Verrà visualizzata una casella di configurazione. Usare la tabella seguente per completare i campi di input.

    | Impostazioni        |  Valore           | DESCRIZIONE  |
   | ------------- |:-------------|:-----|
   | **Nome**      | Valore globalmente univoco | Nome che identifica l'app da monitorare |
   | **Tipo di applicazione** | Applicazione Web ASP.NET | Tipo di app da monitorare |
   | **Gruppo di risorse**     | myResourceGroup      | Nome del nuovo gruppo di risorse per l'hosting dei dati di Application Insights |
   | **Posizione** | Stati Uniti orientali | Scegliere una località nelle vicinanze o vicina a quella in cui è ospitata l'app |

2. Fare clic su **Crea**.

## <a name="configure-app-insights-sdk"></a>Configurare Application Insights SDK

1. Aprire il **progetto** dell'app Web ASP.NET Core in Visual Studio, quindi fare clic con il pulsante destro del mouse sul nome dell'app in **Esplora soluzioni** e scegliere **Aggiungi** > **Application Insights Telemetry**.

    ![Aggiungere Application Insights Telemetry](./media/app-insights-dotnetcore-quick-start/0001.png)

2. Fare clic sul pulsante **Inizia gratis**, selezionare la **risorsa esistente** creata nel portale di Azure e quindi fare clic su **Registra**.

3. Selezionare **Debug** > **Avvia senza eseguire debug** (CTRL+F5) per avviare l'app.

> [!NOTE]
> Prima che i dati vengano visualizzati nel portale trascorrono 3-5 minuti. Se questa app è un'app di test a basso traffico, occorre ricordare che la maggior parte delle metriche viene acquisita solo in presenza di operazioni o richieste attive.

## <a name="start-monitoring-in-the-azure-portal"></a>Avviare il monitoraggio nel portale di Azure

1. È ora possibile riaprire la pagina **Panoramica** di Application Insights nel portale di Azure, selezionando **Progetto** > **Application Insights** > **Apri portale Application Insights**, per visualizzare informazioni dettagliate sull'applicazione attualmente in esecuzione.

   ![Menu della panoramica di Application Insights](./media/app-insights-dotnetcore-quick-start/overview-001.png)

2. Fare clic su **Mappa delle applicazioni** per ottenere un layout visivo delle relazioni di dipendenza tra i componenti dell'applicazione. Ogni componente mostra indicatori KPI come carico, prestazioni, errori e avvisi.

   ![Mappa delle applicazioni](./media/app-insights-dotnetcore-quick-start/application-map.png)

3. Fare clic sull'icona di **App Analytics** ![Icona Mappa app](./media/app-insights-dotnetcore-quick-start/006.png).  Verrà aperta la finestra **Application Insights - Analisi**, che fornisce un linguaggio di query avanzato per l'analisi di tutti i dati raccolti da Application Insights. In questo caso viene generata una query che esegue il rendering del conteggio delle richieste sotto forma di grafico. È possibile scrivere query personalizzate per analizzare altri dati.

   ![Grafico di analisi delle richieste di un utente in un periodo di tempo](./media/app-insights-dotnetcore-quick-start/0007-dc.png)

4. Tornare alla pagina **Panoramica** ed esaminare i dashboard degli indicatori KPI.  Questo dashboard fornisce statistiche relative all'integrità dell'applicazione, ad esempio il numero di richieste in ingresso, la durata delle richieste ed eventuali errori che si sono verificati. 

   ![Grafici Integrità - Panoramica sequenza temporale](./media/app-insights-dotnetcore-quick-start/overview-graphs.png)

   Per abilitare la popolazione del grafico **Tempo di caricamento della visualizzazione pagina** con i dati di **telemetria lato client**, aggiungere questo script a ogni pagina da verificare:

   ```HTML
   <!-- 
   To collect user behavior analytics about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
       function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
       }({
           instrumentationKey:"<insert instrumentation key>"
       });
       
       window.appInsights=appInsights;
       appInsights.trackPageView();
   </script>
   ```

5. Fare clic su **Browser** sotto l'intestazione **Analisi**. Qui sono disponibili le metriche correlate alle prestazioni dell'app. È possibile fare clic su **Aggiungi nuovo grafico** per creare altre visualizzazioni personalizzate oppure selezionare **Modifica** per modificare tipi, altezze, tavolozza dei colori, raggruppamenti e metriche dei grafici esistenti.

   ![Grafico delle metriche del server](./media/app-insights-dotnetcore-quick-start/009-Black.png)

## <a name="clean-up-resources"></a>Pulire le risorse

Se si prevede di continuare a usare le guide introduttive o le esercitazioni successive, non eliminare le risorse create in questa guida introduttiva. Se non si prevede di continuare, seguire questa procedura per eliminare tutte le risorse create da questa guida introduttiva nel portale di Azure.

1. Scegliere **Gruppi di risorse** dal menu a sinistra del portale di Azure e quindi fare clic su **myResourceGroup**.
2. Nella pagina del gruppo di risorse fare clic su **Elimina**, digitare **myResourceGroup** nella casella di testo e quindi fare clic su **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Rilevare e diagnosticare le eccezioni di run-time ](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-runtime-exceptions)
