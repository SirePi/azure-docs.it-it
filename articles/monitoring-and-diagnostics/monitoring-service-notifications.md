---
title: Informazioni sulle notifiche per l'integrità del servizio.
description: Le notifiche sull'integrità del servizio consentono di visualizzare i messaggi sull'integrità del servizio pubblicati da Microsoft Azure.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2017
ms.author: dukek
ms.component: activitylog
ms.openlocfilehash: d6a87b17041c4ce6cf41da863354ef5a2a37141c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264443"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Visualizzare le notifiche sull'integrità del servizio tramite il portale di Azure

Le notifiche di integrità del servizio sono pubblicate da Azure e contengono informazioni sulle risorse nella sottoscrizione. Queste notifiche sono una sottoclasse di eventi del log attività e sono disponibili anche nel log attività. Le notifiche sull'integrità del servizio possono essere informative o operative a seconda della classe.

Sono disponibili varie classi di notifiche sull'integrità del servizio:  

- **Azione richiesta:** Azure potrebbe notare un'anomalia nell'account e collaborare con l'utente per risolvere il problema. Azure invia una notifica con informazioni dettagliate sulle azioni da eseguire o sulla modalità di contatto del supporto tecnico o dell'assistenza di Azure.  
- **Recupero assistito:** si è verificato un evento e i tecnici ne confermano gli effetti ancora presenti. I tecnici di Azure hanno la necessità di collaborare con l'utente per ripristinare integralmente i servizi.  
- **Evento imprevisto:** un evento che influisce sul servizio ha effetto su una o più risorse della sottoscrizione.  
- **Manutenzione:** un'attività di manutenzione pianificata che potrebbe avere effetto su una o più risorse della sottoscrizione.  
- **Informazioni:** potenziali ottimizzazioni che potrebbero migliorare l'uso delle risorse. 
- **Sicurezza:** informazioni urgenti relative alla sicurezza delle soluzioni in esecuzione in Azure.

Ogni notifica sull'integrità del servizio contiene informazioni dettagliate sull'ambito e sull'effetto sulle risorse. I dettagli includono:

Nome proprietà | DESCRIZIONE
-------- | -----------
channels | Uno dei valori seguenti: **Admin** o **Operation**.
correlationId | In genere un GUID in formato stringa. Gli eventi che appartengono alla stessa azione in genere condividono lo stesso correlationId.
eventDataId | Identificatore univoco di un evento.
eventName | Titolo di un evento.
level | Livello di un evento.
resourceProviderName | Nome del provider di risorse della risorsa interessata.
resourceType| Tipo della risorsa interessata.
subStatus | In genere il codice di stato HTTP della chiamata REST corrispondente, ma può includere anche altre stringhe che descrivono uno stato secondario. Ad esempio: OK (codice di stato HTTP: 200), Created (codice di stato HTTP: 201), Accepted (codice di stato HTTP: 202), No Content (codice di stato HTTP: 204), Bad Request (codice di stato HTTP: 400), Not Found (codice di stato HTTP: 404), Conflict (codice di stato HTTP: 409), Internal Server Error (codice di stato HTTP: 500), Service Unavailable (codice di stato HTTP: 503), Gateway Timeout (codice di stato HTTP: 504).
eventTimestamp | Timestamp del momento in cui l'evento è stato generato dal servizio di Azure che ha elaborato la richiesta corrispondente all'evento.
submissionTimestamp | Timestamp del momento in cui l'evento è diventato disponibile per l'esecuzione di query.
subscriptionId | Sottoscrizione di Azure in cui è stato registrato l'evento.
status | Stringa che descrive lo stato dell'operazione. Alcuni dei valori comuni sono: **Started**, **In Progress**, **Succeeded**, **Failed**, **Active** e **Resolved**.
operationName | Nome dell'operazione.
category | Questa proprietà è sempre **ServiceHealth**.
ResourceId | ID risorsa della risorsa interessata.
Properties.title | Titolo della comunicazione localizzato. L'inglese è la lingua predefinita.
Properties.communication | Dettagli localizzati della comunicazione con markup HTML. L'inglese è la lingua predefinita.
Properties.incidentType | Uno dei valori seguenti: **ActionRequired**, **Information**, **Incident**, **Maintenance** o **Security**.
Properties.trackingId | L'evento imprevisto a cui è associato l'evento. Usare questa proprietà per correlare gli eventi relativi a un evento imprevisto.
Properties.impactedServices | BLOB JSON preceduto da un carattere di escape che descrive i servizi e le aree su cui ha effetto l'evento imprevisto. La proprietà include un elenco di servizi, ognuno dei quali ha un **ServiceName** e un elenco delle aree interessate, ognuna con un nome di area **RegionName**.
Properties.defaultLanguageTitle | La comunicazione in inglese.
Properties.defaultLanguageContent | La comunicazione in inglese con markup HTML o come testo normale.
Properties.stage | I valori possibili di **Incident** e **Security** sono **Active,** **Resolved** o **RCA**. Per **ActionRequired** o **Information** l'unico valore è **Active.** Per **Maintenance** i valori possibili sono: **Active**, **Planned**, **InProgress**, **Canceled**, **Rescheduled**, **Resolved** o **Complete**.
Properties.communicationId | La comunicazione a cui è associato l'evento.

### <a name="details-on-service-health-level-information"></a>Informazioni sui livelli di integrità dei servizi
  <ul>
    <li><b>Azione necessaria</b> (properties.incidentType == ActionRequired) <dl>
            <dt>Informativo</dt>
            <dd>L'azione dell'amministratore è necessaria per evitare l'impatto sui servizi esistenti</dd>
        </dl>
    </li>
    <li><b>Manutenzione</b> (properties.incidentType == Maintenance) <dl>
            <dt>Avvertenza</dt>
            <dd>manutenzione emergenze<dd>
            <dt>Informativo</dt>
            <dd>manutenzione pianificata standard</dd>
        </dl>
    </li>
    <li><b>Informazioni</b> (properties.incidentType == Information) <dl>
            <dt>Informativo</dt>
            <dd>L'amministratore potrebbe essere necessario per evitare l'impatto sui servizi esistenti</dd>
        </dl>
    </li>
    <li><b>Sicurezza</b> (properties.incidentType == Security) <dl>
            <dt>Errore</dt>
            <dd>Un grande numero di clienti ha problemi di ampia portata ad accedere a più servizi in più aree.</dd>
            <dt>Avvertenza</dt>
            <dd>Un sottoinsieme di clienti ha problemi ad accedere a servizi specifici e/o aree specifiche.</dd>
            <dt>Informativo</dt>
            <dd>Problemi che influiscono sulle operazioni di gestione e/o sulla latenza senza influire sulla disponibilità del servizio.</dd>
        </dl>
    </li>
    <li><b>Problemi relativi al servizio</b> (properties.incidentType == Incident) <dl>
            <dt>Errore</dt>
            <dd>Un grande numero di clienti ha problemi di ampia portata ad accedere a più servizi in più aree.</dd>
            <dt>Avvertenza</dt>
            <dd>Un sottoinsieme di clienti ha problemi ad accedere a servizi specifici e/o aree specifiche.</dd>
            <dt>Informativo</dt>
            <dd>Problemi che influiscono sulle operazioni di gestione e/o sulla latenza senza influire sulla disponibilità del servizio.</dd>
        </dl>
    </li>
  </ul>

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Visualizzare le notifiche sull'integrità del servizio nel portale di Azure
1.  Nel [portale di Azure](https://portal.azure.com) selezionare **Monitoraggio**.

    ![Schermata del menu del portale di Azure, con Monitoraggio selezionato](./media/monitoring-service-notifications/home-monitor.png)

    Monitoraggio di Azure riunisce tutte le impostazioni e i dati di monitoraggio in un'unica vista consolidata. Per prima cosa si apre la sezione **Log di attività**.

3.  Selezionare **Avvisi**.

    ![Schermata di Log attività in Monitoraggio, con Avvisi selezionati](./media/monitoring-service-notifications/service-health-summary.png)
4. Selezionare **+ Aggiungi avviso del log attività** e configurare un avviso per assicurarsi di ricevere le future notifiche del servizio. Per maggiori informazioni, vedere [Creare gli avvisi del log attività per le notifiche del servizio](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Passaggi successivi
Ricevere [notifiche di avviso per ogni notifica sull'integrità del servizio](monitoring-activity-log-alerts-on-service-notifications.md).  
Altre informazioni sugli [avvisi del log attività](monitoring-activity-log-alerts.md).
