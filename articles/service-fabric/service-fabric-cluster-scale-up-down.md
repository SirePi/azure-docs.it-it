---
title: Aumentare o ridurre le istanze del cluster di Service Fabric | Documentazione Microsoft
description: Aumentare o ridurre le istanze di un cluster di Service Fabric per rispondere alle esigenze impostando le regole di ridimensionamento automatico per ogni tipo di nodo o set di scalabilità di macchine virtuali. Aggiungere o rimuovere nodi in un cluster di Service Fabric
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: aljo
ms.openlocfilehash: d820898b1a0cc26d6832be9d302c74306fa4882f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42144486"
---
# <a name="read-before-you-scale"></a>Leggere prima di eseguire il ridimensionamento
Il ridimensionamento di risorse di calcolo per originare il carico di lavoro dell'applicazione richiede una pianificazione intenzionale, che quasi sempre richiede più di un'ora in un ambiente di produzione e richiede la comprensione del carico di lavoro e del contesto di business. Se non hai mai eseguito questa attività in precedenza, è consigliabile iniziare leggendo e comprendendo le [considerazioni sulla pianificazione della capacità del cluster Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-capacity), prima di continuare con il resto di questo documento. Questo suggerimento intende evitare problemi LiveSite non intenzionali. Inoltre, si consiglia di testare correttamente le operazioni che si decide di eseguire su un ambiente non di produzione. È possibile [segnalare, in qualsiasi momento, problemi di produzione o richiedere supporto a pagamento per Azure](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support#report-production-issues-or-request-paid-support-for-azure). Per i tecnici assegnati all’esecuzione di queste operazioni che conoscono il contesto di esecuzione appropriato, questo articolo descrive le operazioni di scalabilità, ma è necessario stabilire e comprendere quali operazioni siano appropriate per il caso d'uso in questione; ad esempio le risorse da ridimensionare (CPU, archiviazione, memoria), la direzione di ridimensionamento (orizzontale o verticale) e le operazioni da eseguire (distribuzione modelli delle risorse, portale, PowerShell/CLI).

# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Aumentare o ridurre le istanze del cluster di Service Fabric con le regole di ridimensionamento automatico o manualmente
I set di scalabilità di macchine virtuali sono una risorsa di calcolo di Azure che è possibile usare per distribuire e gestire una raccolta di macchine virtuali come set. Ogni tipo di nodo definito in un cluster di Service Fabric viene configurato come set di scalabilità di macchine virtuali distinto. Ogni tipo di nodo può quindi essere aumentato o ridotto in modo indipendente, avere diversi set di porte aperte e avere metriche per la capacità diverse. Per altre informazioni, vedere il documento sui [tipi di nodo di Service Fabric](service-fabric-cluster-nodetypes.md) . Poiché i tipi di nodi di Service Fabric nel cluster sono costituiti da set di scalabilità di macchine virtuali nel back-end, è necessario configurare regole di ridimensionamento automatico per ogni tipo di nodo o set di scalabilità di macchine virtuali.

> [!NOTE]
> È necessario che nella sottoscrizione sia incluso un numero di core sufficienti per aggiungere le nuove VM che costituiranno il cluster. Non esiste attualmente una funzionalità di convalida del modello quindi, se viene raggiunto uno dei limiti della quota, verrà visualizzato un errore in fase di distribuzione. Anche un tipo di nodo singolo non può superare 100 nodi per VMSS. Potrebbe essere necessario aggiungere VMSS per ottenere le il ridimensionamento di destinazione e la scalabilità automatica non può aggiungere VMSS in automatico. L'aggiunta di VMSS sul posto a un cluster in tempo reale è un'attività complessa che di norma spinge gli utenti a effettuare il provisioning di nuovi cluster con i tipi di nodo appropriati al momento della creazione; [pianifica la capacità del cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-capacity) di conseguenza. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Scegliere il tipo di nodo o il set di scalabilità di macchine virtuali da ridimensionare
Attualmente non è possibile specificare le regole di ridimensionamento automatico per i set di scalabilità di macchine virtuali tramite il portale, quindi si userà Azure PowerShell 1.0 o versione successiva per elencare i tipi di nodo e aggiungervi quindi le regole di ridimensionamento automatico.

Per ottenere l'elenco dei set di scalabilità di macchine virtuali che costituiscono il cluster, eseguire i cmdlet seguenti:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Impostare le regole di ridimensionamento automatico per il tipo di nodo o il set di scalabilità di macchine virtuali
Se nel cluster sono disponibili più tipi di nodi, è necessario ripetere questa operazione per ogni tipo di nodo o set di scalabilità di macchine virtuali per cui si vuole aumentare o ridurre le istanze. Considerare il numero di nodi che essere disponibili prima di configurare il ridimensionamento automatico. Il numero minimo di nodi necessari per il tipo di nodo primario è determinato dal livello di affidabilità scelto. Per altre informazioni, vedere la sezione relativa ai [livelli di affidabilità](service-fabric-cluster-capacity.md).

> [!NOTE]
> La riduzione delle istanze del tipo di nodo primario, impostando un valore inferiore al numero minimo, renderà il cluster instabile o lo arresterà. In questo caso, è possibile che si verifichi una perdita di dati per le applicazioni e per i servizi di sistema.
> 
> 

Attualmente la funzionalità di ridimensionamento automatico non è determinata dai carichi che le applicazioni segnalano a Service Fabric. Al momento, il ridimensionamento automatico che si ottiene dipende esclusivamente dai contatori delle prestazioni generati da ognuna delle istanze del set di scalabilità di macchine virtuali.  

Seguire le istruzioni [per configurare il ridimensionamento automatico per ogni set di scalabilità di macchine virtuali](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> In uno scenario di riduzione delle istanze, a meno che il tipo di nodo non abbia un livello di durabilità Gold o Silver, sarà necessario chiamare il cmdlet [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) con il nome del nodo appropriato. Per il livello di durabilità Bronze, è consigliabile non ridurre più nodi alla volta.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Aggiungere manualmente le VM a un tipo di nodo o set di scalabilità di macchine virtuali
Seguire l'esempio o le istruzioni nella [raccolta modelli di avvio rapido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) per modificare il numero di VM in ogni Nodetype. 

> [!NOTE]
> L'aggiunta di VM può richiedere tempo e non è un processo istantaneo. È pertanto consigliabile aggiungere capacità in anticipo, così da avere oltre 10 minuti a disposizione prima che la capacità della VM sia disponibile per collocare le repliche o istanze del servizio.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>Rimuovere manualmente le VM dal tipo di nodo primario o set di scalabilità di macchine virtuali
> [!NOTE]
> I servizi di sistema Service Fabric vengono eseguiti nel tipo di nodo primario del cluster. Non arrestare o ridurre il numero di istanze in questo tipo di nodo a un valore inferiore a quello garantito dal livello di affidabilità. Fare riferimento ai [dettagli sui livelli di affidabilità](service-fabric-cluster-capacity.md). 
> 
> 

È necessario eseguire i seguenti passaggi su un'istanza di VM alla volta. Ciò consente l'arresto normale dei servizi di sistema e dei servizi con stato nella VM che viene rimossa e la creazione di nuove repliche in altri nodi.

1. Eseguire [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) usando 'RemoveNode' per disabilitare il nodo da rimuovere, ovvero l'istanza superiore di quel tipo di nodo.
2. Eseguire [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) per verificare che il nodo sia stato effettivamente disabilitato. In caso contrario, attendere la disabilitazione del nodo. Non è possibile accelerare questo passaggio.
3. Seguire l'esempio o le istruzioni nella [raccolta di modelli di avvio rapido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) per modificare di un'unità il numero di macchine virtuali in tale oggetto Nodetype. L'istanza rimossa è l'istanza di macchina virtuale superiore. 
4. Ripetere le fasi da 1 a 3 come necessario, ma non ridurre il numero di istanze nel nodo primario a un valore inferiore a quello garantito dal livello di affidabilità. Fare riferimento ai [dettagli sui livelli di affidabilità](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>Rimuovere manualmente le VM dal tipo di nodo non primario o set di scalabilità di macchine virtuali
> [!NOTE]
> Per un servizio con stato, è necessario un determinato numero di nodi per essere sempre in grado di assicurare la disponibilità e mantenere lo stato del servizio. Come minimo, è necessario un numero di nodi uguale al conteggio dei set di repliche di destinazione della partizione o dei servizi. 
> 
> 

È necessario eseguire i seguenti passaggi su un'istanza di VM alla volta. Ciò consente l'arresto normale dei servizi di sistema e dei servizi con stato nella VM che viene rimossa e la creazione di nuove repliche altrove.

1. Eseguire [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) usando 'RemoveNode' per disabilitare il nodo da rimuovere, ovvero l'istanza superiore di quel tipo di nodo.
2. Eseguire [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) per verificare che il nodo sia stato effettivamente disabilitato. In caso contrario, attendere la disabilitazione del nodo. Non è possibile accelerare questo passaggio.
3. Seguire l'esempio o le istruzioni nella [raccolta di modelli di avvio rapido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) per modificare di un'unità il numero di macchine virtuali in tale oggetto Nodetype. Questa operazione rimuoverà l'istanza superiore di macchina virtuale. 
4. Ripetere le fasi da 1 a 3 come necessario, ma non ridurre il numero di istanze nel nodo primario a un valore inferiore a quello garantito dal livello di affidabilità. Fare riferimento ai [dettagli sui livelli di affidabilità](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Comportamenti che è possibile osservare in Service Fabric Explorer
Quando si aumentano le istanze di un cluster, Service Fabric Explorer riflette il numero di nodi, ovvero le istanze del set di scalabilità di macchine virtuali, che fanno parte del cluster.  Quando tuttavia si riducono le istanze di un cluster, il nodo o l'istanza della VM rimossa viene ancora visualizzata con uno stato non integro, a meno di chiamare il cmdlet [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) con il nome del nodo appropriato.   

Ecco la spiegazione di questo comportamento.

I nodi elencati in Service Fabric Explorer riflettono le informazioni a disposizione dei servizi di sistema di Service Fabric, in particolare FM, circa il numero di nodi presenti nel cluster attualmente o in precedenza. Quando si riducono le istanze del set di scalabilità di macchine virtuali, la macchina virtuale viene eliminata, ma il servizio di sistema FM continua a ritenere che il nodo, precedentemente mappato alla macchina virtuale eliminata, verrà ripristinato. Service Fabric Explorer continua quindi a visualizzare tale nodo, anche se lo stato di integrità è sconosciuto o di errore.

Per assicurarsi che un nodo venga rimosso quando si rimuove una VM, sono disponibili due opzioni:

1) Scegliere un livello di durabilità Gold o Silver per i tipi di nodo del cluster, che offre l'integrazione dell'infrastruttura. In questo caso i nodi saranno rimossi automaticamente dallo stato dei servizi di sistema (FM) quando si riducono le istanze.
Fare riferimento ai [i dettagli sui livelli di durabilità qui](service-fabric-cluster-capacity.md)

2) Dopo aver ridotto le istanze della VM, chiamare il [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate).

> [!NOTE]
> I cluster di Service Fabric richiedono che un certo numero di nodi sia attivo in ogni momento allo scopo di mantenere la disponibilità e lo stato, ossia per "mantenere il quorum". Di conseguenza, non è in genere sicuro arrestare tutte le macchine virtuali del cluster senza avere prima eseguito un [backup completo dello stato](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sulla pianificazione della capacità del cluster, l'aggiornamento di un cluster e il partizionamento dei servizi, vedere gli articoli seguenti:

* [Considerazioni sulla pianificazione della capacità del cluster di Service Fabric](service-fabric-cluster-capacity.md)
* [Aggiornare un cluster di Service Fabric](service-fabric-cluster-upgrade.md)
* [Partizionare Reliable Services di Service Fabric](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
