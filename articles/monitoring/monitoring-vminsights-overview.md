---
title: Descrizione di Monitoraggio di Azure per le macchine virtuali | Microsoft Docs
description: Monitoraggio di Azure per le macchine virtuali è una funzionalità di Monitoraggio di Azure che combina il monitoraggio dell'integrità e delle prestazioni del sistema operativo delle macchine virtuali di Azure, nonché l'individuazione automatica dei componenti e delle dipendenze delle applicazioni con altre risorse e mappa la comunicazione tra questi elementi. Questo articolo offre una panoramica.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 79f507c342f5a13c4d3784cf312f0bf8aeffa3e3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957254"
---
## <a name="what-is-azure-monitor-for-vms"></a>Descrizione di Monitoraggio di Azure per le macchine virtuali

Monitoraggio di Azure per le macchine virtuali monitora le macchine virtuali di Azure su larga scala analizzando le prestazioni e l'integrità delle macchine virtuali Windows e Linux, inclusi i relativi processi e le dipendenze interconnesse ad altre risorse e processi esterni. La soluzione include il supporto per il monitoraggio delle prestazioni e delle dipendenze dell'applicazione per le macchine virtuali ospitate in locale o in un altro provider di servizi cloud.  Include tre funzionalità principali per fornire queste informazioni dettagliate:

* I componenti logici delle macchine virtuali di Azure che eseguono un sistema operativo Windows o Linux sono misurati in base a un set di criteri di integrità preconfigurati e avvisi generati quando viene soddisfatta la condizione valutata.  
* Le metriche delle prestazioni principali di processore, memoria, disco e scheda di rete del sistema operativo della macchina virtuale guest vengono raccolte e presentate in grafici di tendenza delle prestazioni predefiniti.
* Mappa delle dipendenze che mostra i componenti interconnessi individuati con la macchina virtuale specifica da pià gruppi di risorse e sottoscrizioni.  

Queste funzionalità sono organizzate in tre prospettive:

* Integrità
* Prestazioni
* Mappa

>[!NOTE]
>Attualmente, la funzionalità Integrità è disponibile solo per le macchine virtuali di Azure.
>

L'integrazione con Log Analytics offre efficaci caratteristiche di aggregazione e filtro e la possibilità di eseguire analisi delle tendenze dei dati nel tempo. Il monitoraggio completo dei carichi di lavoro non può essere realizzato solo con Monitoraggio di Azure, Mapping dei servizi o Log Analytics.  

È possibile visualizzare questi dati nel contesto della singola macchina virtuale direttamente dalla macchina virtuale o con Monitoraggio di Azure per una visualizzazione aggregata delle macchine virtuali in base alla prospettiva seguente per ogni funzionalità:

* Integrità: macchine virtuali correlate a un gruppo di risorse
* Mappa e prestazioni: macchine virtuali configurate per inviare report a una specifica area di lavoro di Log Analytics

![Prospettiva di informazioni dettagliate della macchina virtuale dal portale](./media/monitoring-vminsights-overview/vminsights-azmon-directvm-01.png)

DevOps fornisce in modo efficace disponibilità e prestazioni prevedibili delle applicazioni vitali individuando eventi critici del sistema operativo e colli di bottiglia delle prestazioni, problemi di rete ed eventuale correlazione di un problema ad altre dipendenze.  

## <a name="data-usage"></a>Utilizzo dei dati 

Non appena si carica Monitoraggio di Azure per le macchine virtuali, i dati raccolti dalle macchine virtuali vengono inseriti e archiviati in Monitoraggio di Azure.  La fatturazione di Monitoraggio di Azure per le macchine virtuali viene effettuata per dati inseriti e conservati, numero di serie temporali di metriche di criteri di integrità monitorate, regole di avviso create,notifiche inviate, secondo il prezzo pubblicato sulla [pagina dei prezzi](https://azure.microsoft.com/pricing/details/monitor/) di Monitoraggio di Azure.

Le dimensioni del log variano in base alle lunghezze delle stringhe dei contatori e possono aumentare con il numero di dischi logici e schede di rete.  Se è già disponibile un'area di lavoro ed è in corso la raccolta di questi contatori, non verranno applicati addebiti doppi.  Se si utiizza già Mapping dei servizi, l'unico cambiamente sarano i dati di connessione aggiuntivi inviati a Monitoraggio di Azure.

## <a name="next-steps"></a>Passaggi successivi
[Caricare Monitoraggio di Azure per le macchine virtuali](monitoring-vminsights-onboard.md) per iniziare a monitorare le macchine virtuali di Azure.
