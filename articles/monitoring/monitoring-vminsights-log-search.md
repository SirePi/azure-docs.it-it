---
title: Come eseguire query sui log da Monitoraggio di Azure per le macchine virtuali | Microsoft Docs
description: La soluzione Monitoraggio di Azure per le macchine virtuali inoltra le metriche e i dati di log a Log Analytics e questo articolo descrive i record e include query di esempio.
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
ms.openlocfilehash: 446268f28e7c87196023636889f03be2da92ecfd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967643"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms"></a>Come eseguire query sui log da Monitoraggio di Azure per le macchine virtuali
Monitoraggio di Azure per le macchine virtuali raccoglie le metriche relative a prestazioni e connessione, i dati di inventario di computer e processi e le informazioni sullo stato di integrità e inoltra quanto raccolto all'archivio dati di Log Analytics in Monitoraggio di Azure.  Questi dati sono disponibili per la [ricerca](../log-analytics/log-analytics-log-searches.md) in Log Analytics. Questi dati possono essere applicati a diversi scenari, tra cui la pianificazione della migrazione, l'analisi della capacità, l'individuazione e la risoluzione dei problemi di prestazioni on demand.

## <a name="map-records"></a>Record della mappa
Ogni ora viene generato un record per ogni computer e processo univoco, in aggiunta ai record generati all'avvio di un processo o computer o quando ne viene eseguito l'onboarding nella funzionalità di mappa di Monitoraggio di Azure per le macchine virtuali. I record hanno le proprietà descritte nelle tabelle seguenti. I campi e i valori negli eventi ServiceMapComputer_CL eseguono il mapping ai campi della risorsa del computer nell'API ServiceMap di Azure Resource Manager. I campi e i valori negli eventi ServiceMapProcess_CL eseguono il mapping ai campi della risorsa del computer nell'API ServiceMap di Azure Resource Manager. Il campo ResourceName_s coincide con il campo del nome nella risorsa di Resource Manager corrispondente. 

Sono disponibili proprietà generate internamente che è possibile usare per identificare processi e computer univoci:

- Computer: usare *ResourceId* o *ResourceName_s* per identificare in modo univoco un computer in un'area di lavoro di Log Analytics.
- Processo: usare *ResourceId* per identificare in modo univoco un processo in un'area di lavoro di Log Analytics. Il valore di *ResourceName_s* è univoco nel contesto del computer in cui viene eseguito il processo (MachineResourceName_s) 

Poiché possono essere presenti vari record per un determinato processo o computer in un intervallo di tempo specificato, le query possono restituire più di un record per lo stesso computer o processo. Per includere solo il record più recente, aggiungere "| dedup ResourceId" alla query.

### <a name="connections"></a>connessioni
Le metriche relative alla connessione vengono scritte in una nuova tabella in Log Analytics, ovvero VMConnection. Questa tabella fornisce informazioni sulle connessioni di un computer (in ingresso e in uscita). Le metriche relative alla connessione vengono inoltre esposte con le API che forniscono i mezzi per ottenere una metrica specifica durante un intervallo di tempo.  Le connessioni TCP stabilite *accettando* l'operazione su un socket in ascolto sono connessioni in ingresso, mentre quelle create tramite *connessione* a un IP e una porta specifici sono in uscita. La direzione di una connessione è rappresentata dalla proprietà Direction, che può essere impostata su **inbound** o **outbound**. 

I record in queste tabelle vengono generati dai dati segnalati da Dependency Agent. Ogni record rappresenta un'osservazione in un intervallo di tempo di un minuto. La proprietà TimeGenerated indica l'inizio dell'intervallo di tempo. Ogni record contiene informazioni per identificare l'entità corrispondente, ovvero la connessione o la porta, oltre che le metriche associate a tale entità. Attualmente, viene segnalata solo l'attività di rete che si verifica tramite TCP su IPv4.

Per gestire i costi e la complessità, i record di connessione non rappresentano singole connessioni di rete fisiche. Più connessioni di rete fisiche vengono raggruppate in una connessione logica, che viene quindi riflessa nella rispettiva tabella.  Ciò significa che i record nella tabella *VMConnection* rappresentano un raggruppamento logico e non le singole connessioni fisiche osservate. Le connessioni di rete fisiche che condividono lo stesso valore per gli attributi seguenti durante uno specifico intervallo di un minuto vengono aggregate in un singolo record logico in *VMConnection*. 

| Proprietà | DESCRIZIONE |
|:--|:--|
|Direction |Direzione della connessione. Il valore è *inbound* o *outbound* |
|Machine |FQDN del computer |
|Process |Identità del processo o dei gruppi di processi che avviano/accettano la connessione |
|SourceIp |Indirizzo IP dell'origine |
|DestinationIp |Indirizzo IP della destinazione |
|DestinationPort |Numero di porta della destinazione |
|Protocollo |Protocollo usato per la connessione.  Il valore è *tcp*. |

Per rendere conto dell'impatto del raggruppamento, nelle proprietà del record seguenti vengono fornite informazioni sul numero di connessioni fisiche raggruppate:

| Proprietà | DESCRIZIONE |
|:--|:--|
|LinksEstablished |Numero di connessioni di rete fisiche che sono state stabilite durante l'intervallo di tempo di creazione del report |
|LinksTerminated |Numero di connessioni di rete fisiche che sono state terminate durante l'intervallo di tempo di creazione del report |
|LinksFailed |Numero di connessioni di rete fisiche che hanno avuto esito negativo durante l'intervallo di tempo di creazione del report. Queste informazioni sono attualmente disponibili solo per le connessioni in uscita. |
|LinksLive |Numero di connessioni di rete fisiche aperte alla fine dell'intervallo di tempo di creazione del report|

#### <a name="metrics"></a>Metriche

Oltre alle metriche relative al numero di connessioni, nelle proprietà del record seguenti vengono fornite anche informazioni sul volume dei dati inviati e ricevuti in una determinata connessione logica o porta di rete:

| Proprietà | DESCRIZIONE |
|:--|:--|
|BytesSent |Numero totale di byte che sono stati inviati durante l'intervallo di tempo di creazione del report |
|BytesReceived |Numero totale di byte che sono stati ricevuti durante l'intervallo di tempo di creazione del report |
|Risposte |Numero totale di risposte osservate durante l'intervallo di tempo di creazione del report. 
|ResponseTimeMax |Tempo di risposta più lungo (millisecondi) osservato durante l'intervallo di tempo di creazione del report.  In assenza di valore, la proprietà è vuota.|
|ResponseTimeMin |Tempo di risposta più breve (millisecondi) osservato durante l'intervallo di tempo di creazione del report.  In assenza di valore, la proprietà è vuota.|
|ResponseTimeSum |Somma di tutti i tempi di risposta (millisecondi) osservati durante l'intervallo di tempo di creazione del report.  In assenza di valore, la proprietà è vuota|

Il terzo tipo di dati segnalati è il tempo di risposta, ovvero il tempo di attesa, da parre di un chiamante, affinché una richiesta inviata tramite una connessione venga elaborata dall'endpoint remoto e venga fornita una risposta. Il tempo di risposta segnalato è una stima del tempo di risposta effettivo del protocollo dell'applicazione sottostante. Viene calcolato usando l'euristica in base all'osservazione del flusso di dati tra l'origine e la destinazione di una connessione di rete fisica. Concettualmente, corrisponde alla differenza tra l'ora in cui l'ultimo byte di una richiesta lascia il mittente e l'ora in cui l'ultimo byte della risposta torna al mittente. Questi due timestamp vengono usati per delineare gli eventi di richiesta e di risposta in una determinata connessione fisica. La differenza tra di essi rappresenta il tempo di risposta di una singola richiesta. 

In questa prima versione di questa funzionalità, l'algoritmo è un'approssimazione che potrebbe funzionare con un diverso livello di precisione a seconda del protocollo dell'applicazione effettivo usato per una connessione di rete specifica. L'attuale approccio, ad esempio, funziona bene per protocolli basati su richiesta-risposta, come HTTP(S), ma non con protocolli unidirezionali o protocolli basati sulla coda di messaggi.

Ecco alcuni punti importanti da considerare:

1. Se un processo accetta le connessioni sullo stesso indirizzo IP ma su più interfacce di rete, verrà segnalato un record distinto per ogni interfaccia. 
2. I record con IP con caratteri jolly non contengono attività. Sono inclusi per rappresentare il fatto che una porta nel computer è aperta per il traffico in ingresso.
3. Per ridurre il livello di dettaglio e il volume di dati, i record con IP con caratteri jolly vengono omessi quando è presente un record corrispondente (per processo, porta e protocollo uguali) con un indirizzo IP specifico. Quando un record di IP con caratteri jolly viene omesso, la proprietà IsWildcardBind del record con l'indirizzo IP specifico viene impostata su "True" per indicare che la porta è esposta in ogni interfaccia del computer che esegue la segnalazione.
4. Per le porte associate solo a un'interfaccia specifica, la proprietà IsWildcardBind è impostata su "False".

#### <a name="naming-and-classification"></a>Denominazione e classificazione
Per praticità, l'indirizzo IP dell'estremità remota di una connessione è incluso nella proprietà RemoteIp. Per le connessioni in ingresso, RemoteIp corrisponde a SourceIp, mentre per le connessioni in uscita corrisponde a DestinationIp. La proprietà RemoteDnsCanonicalNames rappresenta i nomi canonici DNS segnalati dal computer per RemoteIp. Le proprietà RemoteDnsQuestions e RemoteClassification sono riservate per l'uso futuro. 

#### <a name="geolocation"></a>Georilevazione
*VMConnection* include anche informazioni di georilevazione per l'estremità remota di ogni record di connessione nelle proprietà del record seguenti: 

| Proprietà | DESCRIZIONE |
|:--|:--|
|RemoteCountry |Nome del paese che ospita RemoteIp.  Ad esempio, *Stati Uniti* |
|RemoteLatitude |Latitudine della georilevazione.  Ad esempio, *47.68* |
|RemoteLongitude |Longitudine della georilevazione.  Ad esempio, *-122.12* |

#### <a name="malicious-ip"></a>Indirizzi IP dannosi
Ogni proprietà RemoteIp nella tabella *VMConnection* viene confrontata con un set di indirizzi IP con attività dannosa nota. Se la proprietà RemoteIp viene identificata come dannosa, le proprietà del record seguenti (vuote quando l'indirizzo IP non è considerato dannoso) vengono popolate:

| Proprietà | DESCRIZIONE |
|:--|:--|
|MaliciousIp |Indirizzo RemoteIp |
|IndicatorThreadType | |
|DESCRIZIONE | |
|TLPLevel | |
|Attendibilità | |
|Gravità | |
|FirstReportedDateTime | |
|LastReportedDateTime | |
|IsActive | |
|ReportReferenceLink | |
|AdditionalInformation | |

### <a name="servicemapcomputercl-records"></a>Record ServiceMapComputer_CL
I record di tipo *ServiceMapComputer_CL* includono dati di inventario per i server con Dependency Agent. Questi record includono le proprietà elencate nella tabella seguente:

| Proprietà | DESCRIZIONE |
|:--|:--|
| type | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Identificatore univoco per il computer nell'area di lavoro |
| ResourceName_s | Identificatore univoco per il computer nell'area di lavoro |
| ComputerName_s | FQDN del computer |
| Ipv4Addresses_s | Un elenco degli indirizzi IPv4 del server |
| Ipv6Addresses_s | Un elenco degli indirizzi IPv6 del server |
| DnsNames_s | Una matrice di nomi DNS |
| OperatingSystemFamily_s | Windows o Linux |
| OperatingSystemFullName_s | Nome completo del sistema operativo  |
| Bitness_s | Numero di bit del computer, a 32 bit o a 64 bit  |
| PhysicalMemory_d | Memoria fisica in MB |
| Cpus_d | Numero di CPU |
| CPUSpeed_d | Velocità della CPU in MHz|
| VirtualizationState_s | *sconosciuto*, *fisico*, *virtuale*, *hypervisor* |
| VirtualMachineType_s | *hyperv*, *vmware* e così via |
| VirtualMachineNativeMachineId_g | ID della macchina virtuale assegnato dal relativo hypervisor |
| VirtualMachineName_s | Nome della macchina virtuale |
| BootTime_t | Tempo di avvio |

### <a name="servicemapprocesscl-type-records"></a>Record con tipo ServiceMapProcess_CL
I record di tipo *ServiceMapProcess_CL* includono dati di inventario per i processi connessi tramite TCP nei server con Dependency Agent. Questi record includono le proprietà elencate nella tabella seguente:

| Proprietà | DESCRIZIONE |
|:--|:--|
| type | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Identificatore univoco per il processo nell'area di lavoro |
| ResourceName_s | Identificatore univoco per il processo nel computer in cui è in esecuzione|
| MachineResourceName_s | Nome della risorsa del computer |
| ExecutableName_s | Nome dell'eseguibile del processo |
| StartTime_t | Ora di inizio del pool del processo |
| FirstPid_d | Primo PID nel pool del processo |
| Description_s | Descrizione del processo |
| CompanyName_s | Nome dell'azienda |
| InternalName_s | Nome interno |
| ProductName_s | Nome del prodotto |
| ProductVersion_s | Versione del prodotto |
| FileVersion_s | Versione del file |
| CommandLine_s | Riga di comando |
| ExecutablePath _s | Percorso del file eseguibile |
| WorkingDirectory_s | La directory di lavoro |
| UserName | Account con cui è in esecuzione il processo |
| UserDomain | Dominio in cui è in esecuzione il processo |

## <a name="sample-log-searches"></a>Ricerche di log di esempio

### <a name="list-all-known-machines"></a>Visualizzare tutti i computer noti
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Visualizzare la capacità di memoria fisica di tutti i computer gestiti.
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project PhysicalMemory_d, ComputerName_s

### <a name="list-computer-name-dns-ip-and-os"></a>Visualizzare nome del computer, DNS, IP e sistema operativo.
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Trovare tutti i processi con "sql" nella riga di comando
ServiceMapProcess_CL | where CommandLine_s contains_cs "sql" | summarize arg_max(TimeGenerated, *) by ResourceId

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Trovare un computer (record più recente) in base al nome di risorsa
search in (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Trovare un computer, ovvero il record più recente, in base all’indirizzo IP
search in (ServiceMapComputer_CL) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by ResourceId

### <a name="list-all-known-processes-on-a-specified-machine"></a>Elencare tutti i processi noti su un computer specifico
ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId

### <a name="list-all-computers-running-sql"></a>Visualizzare tutti i computer che eseguono SQL
ServiceMapComputer_CL | where ResourceName_s in ((search in (ServiceMapProcess_CL) "\*sql\*" | distinct MachineResourceName_s)) | distinct ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Elencare tutte le versioni di prodotto univoche di curl nel data center
ServiceMapProcess_CL | where ExecutableName_s == "curl" | distinct ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Creare un gruppo di tutti i computer che eseguono CentOS
ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Riepilogare le connessioni in uscita da un gruppo di computer
```
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="next-steps"></a>Passaggi successivi
* Se non si ha familiarità con la scrittura di query in Log Analytics,vedere le informazioni su [come usare la pagina di Log Analytics](../log-analytics/query-language/get-started-analytics-portal.md) nel portale di Azure per scrivere query di Log Analytics.
* Leggere le informazioni su come [scrivere query di ricerca](../log-analytics/query-language/search-queries.md).