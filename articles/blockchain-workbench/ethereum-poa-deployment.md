---
title: Ethereum Proof-of-Authority Consortium
description: La soluzione Ethereum Proof-of-Authority Consortium consente di distribuire e configurare una rete Ethereum di consorzio multi-membro
services: azure-blockchain
keywords: ''
author: CodyBorn
ms.author: coborn
ms.date: 8/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: vamelech
ms.openlocfilehash: fcb35f23be384b3625e4e17e5dc2285b7eeb341a
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39529480"
---
# <a name="ethereum-proof-of-authority-consortium"></a>Ethereum Proof-of-Authority Consortium

[Questa soluzione](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) è stata progettata per rendere più facile la distribuzione, la configurazione e la gestione di una rete Ethereum Proof-of-Authority di consorzio multi-membro con conoscenze minime su Azure ed Ethereum.

Con un numero limitato di input utente e una distribuzione tramite un singolo clic attraverso il portale di Azure, ogni membro può effettuare il provisioning di un footprint di rete, usando i servizi di calcolo, di rete e di archiviazione di Microsoft Azure in tutto il mondo. Il footprint di rete di ogni membro è costituito da un set di nodi di convalida con carico bilanciato con cui un'applicazione o un utente può interagire per inviare transazioni Ethereum.

## <a name="concepts"></a>Concetti

### <a name="terminology"></a>Terminologia

-   **Consenso**: azione di sincronizzazione dei dati attraverso la rete distribuita tramite la convalida e la creazione di un blocco.

-   **Membro di consorzio**: entità che partecipa al consenso nella rete di blockchain.

-   **Amministratore**: account Ethereum usato per gestire la partecipazione per un determinato membro del consorzio.

-   **Validator**: computer associato a un account Ethereum che partecipa al consenso per conto di un amministratore.

### <a name="proof-of-authority"></a>Proof-of-Authority

Per gli utenti che hanno familiarità con la community blockchain, il rilascio di questa soluzione è una grande opportunità per conoscere la tecnologia in modo facile e configurabile su Azure. Proof-of-Work è un meccanismo di resistenza a Sybil che sfrutta i costi di calcolo per regolare automaticamente la rete e consentire una partecipazione equa. Questo approccio è efficace nelle reti di blockchain anonime aperte in cui la concorrenza per la criptovaluta alza di livello la sicurezza in rete. Nelle reti private/di consorzio, tuttavia, l'Ether sottostante non ha valore. Proof-of-Authority, un protocollo alternativo, è più adatto alle reti con autorizzazione in cui tutti i partecipanti al consenso sono noti e attendibili. Senza bisogno di mining, Proof-of-Authority è più efficiente pur mantenendo la tolleranza di errore bizantina.

### <a name="consortium-governance"></a>Governance del consorzio

Poiché Proof-of-Authority si basa su un elenco con autorizzazione di autorità di rete per mantenere l'integrità della rete, è importante fornire un meccanismo equo per apportare modifiche a questo elenco di autorizzazioni. Ogni distribuzione ha un set di smart contract e un portale per la governance on-chain di questo elenco con autorizzazione. Dopo che una modifica proposta raggiunge un voto di maggioranza da parte dei membri del consorzio, la modifica viene applicata. In questo modo si possono aggiungere nuovi partecipanti al consenso o rimuovere i partecipanti compromessi in un modo trasparente che favorisce una rete affidabile.

### <a name="admin-account"></a>Account amministratore

Durante la distribuzione dei nodi Proof-of-Authority, verrà chiesto un indirizzo Ethereum di amministratore. Si possono usare meccanismi diversi per generare e proteggere questo account Ethereum. Dopo che questo indirizzo viene aggiunto come autorità della rete, è possibile usare questo account per partecipare alla governance. Questo account amministratore verrà anche usato per delegare la partecipazione al consenso ai nodi di convalida creati durante questa distribuzione. Poiché viene usato solo l'indirizzo pubblico di Ethereum, ogni amministratore ha la flessibilità necessaria per proteggere le chiavi private e poter essere conforme al modello di sicurezza desiderato.

### <a name="validator-node"></a>Nodo di convalida

Nel protocollo Proof-of-Authority, i nodi di convalida prendono il posto dei tradizionali nodi di mining. Ogni validator ha un'identità Ethereum univoca che viene aggiunta a un elenco di autorizzazioni smart contract. Quando un validator è in questo elenco, può partecipare al processo di creazione del blocco. Per altre informazioni su questo processo, vedere la documentazione di Parity sul [consenso di Authority Round](https://wiki.parity.io/Aura). Ogni membro del consorzio può effettuare il provisioning di due o più nodi di convalida in cinque aree, per la ridondanza geografica. I nodi di convalida comunicano con altri nodi di convalida per convergere sullo stato del libro mastro distribuito sottostante.
Per garantire la partecipazione equa alla rete, a ogni membro del consorzio è vietato usare più validator del primo membro della rete. Se ad esempio il primo membro distribuisce tre validator, ogni membro può avere solo un massimo di tre validator.

### <a name="identity-store"></a>Archivio di identità

Poiché ogni membro avrà più nodi di convalida in esecuzione simultaneamente e ogni nodo deve avere un'identità con autorizzazione, è importante che i validator possano acquisire un'identità attiva univoca nella rete. A tale scopo, è stato creato un archivio di identità che viene distribuito nella sottoscrizione di ogni membro che contiene in modo sicuro le identità Ethereum generate. Durante la distribuzione, il contenitore dell'orchestrazione genererà una chiave privata Ethereum per ogni validator e la archivierà in Azure Key Vault. Prima che il nodo Parity venga avviato, acquisisce un lease su un'identità inutilizzata per garantire che l'identità non venga selezionata da un altro nodo. L'identità viene fornita al client che ha quindi l'autorità per iniziare a creare i blocchi. Se si verifica un'interruzione nella macchina virtuale di hosting, il lease dell'identità verrà rilasciato, consentendo a un nodo di sostituzione di riprendere l'identità in futuro.

### <a name="bootnode-registrar"></a>Registrar del nodo di avvio

Per agevolare la connettività, ogni membro ospiterà un set di informazioni di connessione nell'[endpoint API Dati](#data-api). Questi dati includono un elenco di nodi di avvio che vengono forniti come nodi peer per il membro aggiunto. Come parte di questa API Dati, l'elenco di nodi di avvio viene mantenuto aggiornato

### <a name="bring-your-own-operator"></a>Usare un operatore personalizzato

Il membro di un consorzio dovrà spesso partecipare alla governance della rete, ma senza far funzionare e gestire l'infrastruttura. A differenza dei sistemi tradizionali, un singolo operatore nella rete non è l'ideale nel modello decentralizzato dei sistemi di blockchain. Invece di assumere un intermediario centralizzato per far funzionare una rete, ogni membro del consorzio può delegare la gestione dell'infrastruttura all'operatore preferito. In questo modo si può usare un modello ibrido in cui ogni membro può scegliere di gestire la propria infrastruttura o di delegare la gestione a un altro partner. Il flusso di lavoro di gestione delegata funziona nel modo seguente:

1.  Il **membro del consorzio** genera un indirizzo Ethereum (detiene la chiave privata)

2.  Il **membro del consorzio** fornisce l'indirizzo Ethereum pubblico all'**operatore**

3.  L'**operatore** distribuisce e configura i nodi di convalida PoA usando la soluzione Azure Resource Manager

4.  L'**operatore** fornisce l'endpoint RPC e di gestione al **membro del consorzio**

5.  Il **membro del consorzio** usa la chiave privata per firmare una richiesta di accettazione dei nodi di convalida distribuiti dall'**operatore** per partecipare in loro vece

![Usare un operatore personalizzato](./media/ethereum-poa-deployment-guide/bring-you-own-operator.png)

### <a name="azure-monitor"></a>Monitoraggio di Azure

Questa soluzione include anche Monitoraggio di Azure per tenere traccia delle statistiche dei nodi e della rete. Questo offre agli sviluppatori di applicazioni visibilità sul blockchain sottostante per tenere traccia delle statistiche di generazione dei blocchi. Gli operatori di rete possono usare Monitoraggio di Azure per rilevare rapidamente ed evitare interruzioni della rete tramite le statistiche sull'infrastruttura e i log disponibili per query. Per informazioni più dettagliate, vedere [Monitoraggio dei servizi](#service-monitoring).

### <a name="deployment-architecture"></a>Architettura di distribuzione

#### <a name="description"></a>DESCRIZIONE

Questa soluzione consente di distribuire una rete di consorzio Ethereum multi-membro basata su una singola area o su più aree. Per impostazione predefinita, gli endpoint RPC e peering sono accessibili tramite IP pubblico per abilitare la connettività semplificata tra le sottoscrizioni e i cloud. È consigliabile usare i [contratti di concessione delle autorizzazioni di Parity](https://wiki.parity.io/Permissioning) per i controlli di accesso a livello di applicazione. Sono supportate anche le reti distribuite dietro VPN, che sfruttano i gateway di rete virtuale per la connettività tra sottoscrizioni. Queste distribuzioni sono più complesse, quindi è consigliabile iniziare prima con il modello di IP pubblico.

#### <a name="consortium-member-overview"></a>Panoramica dei membri di consorzio

Ogni distribuzione di un membro di un consorzio include:

-   Set di scalabilità di macchine virtuali per l'esecuzione dei validator PoA

-   Azure Load Balancer per la distribuzione di richieste di applicazioni decentralizzate per governance, peering e RPC

-   Azure Key Vault per proteggere le identità dei validator

-   Archiviazione di Azure per ospitare le informazioni di rete persistenti e coordinare il leasing

-   Monitoraggio di Azure per l'aggregazione dei log e delle statistiche sulle prestazioni

-   Gateway di rete virtuale (facoltativo) per consentire le connessioni VPN tra reti virtuali private

![Architettura di distribuzione](./media/ethereum-poa-deployment-guide/deployment-architecture.png)

Vengono sfruttati i contenitori Docker per l'affidabilità e la modularità. Viene usato il Registro contenitori di Azure per ospitare e gestire le immagini con versione durante ogni distribuzione. Le immagini del contenitore sono costituite da:

-   Orchestrator

    -   Viene eseguito una sola volta durante la distribuzione

    -   Genera le identità e i contratti di governance

    -   Archivia le identità nell'archivio identità

-   Client Parity

    -   Concede in lease le identità dall'archivio identità

    -   Individua e si connette ai peer

-   Agente EthStats

    -   Raccoglie i log e le statistiche locali tramite RPC ed esegue il push a Monitoraggio di Azure

-   App decentralizzata per la governance

    -   Interfaccia Web per l'interazione con i contratti di governance

## <a name="governance-dapp"></a>App decentralizzata per la governance

Il fulcro di Proof-of-Authority è la governance decentralizzata. L'app decentralizzata per la governance è un set di smart contract predistribuiti e un'applicazione Web usati per gestire le autorità in rete.
Le autorità sono suddivise in nodi di convalida e identità amministrative.
Gli amministratori hanno la possibilità di delegare la partecipazione al consenso a un set di nodi di convalida. Gli amministratori possono anche votare altri amministratori all'interno o all'esterno della rete.

![App decentralizzata per la governance](./media/ethereum-poa-deployment-guide/governance-dapp.png)

-   **Governance decentralizzata:** le modifiche alle autorità di rete vengono amministrate tramite la votazione on-chain da parte di amministratori selezionati.

-   **Delega dei validator:** le autorità possono gestire i nodi di convalida configurati in ogni distribuzione PoA.

-   **Cronologia delle modifiche controllabili:** ogni modifica viene registrata nel blockchain fornendo trasparenza e capacità di controllo.

## <a name="how-to-guides"></a>Guide alle procedure

### <a name="deploy-ethereum-proof-of-authority"></a>Distribuire Ethereum Proof-of-Authority

Di seguito è riportato un esempio di flusso di distribuzione tra più parti:

1.  Tre membri che generano ognuno un account Ethereum usando MetaMask

2.  Il *membro A* distribuisce Ethereum PoA, fornendo l'indirizzo pubblico di Ethereum

3.  Il *membro A* fornisce l'URL del consorzio al *membro B* e al *membro C*

4.  Il *membro B* e il *membro C* distribuiscono Ethereum PoA, fornendo l'indirizzo pubblico di Ethereum e l'URL del consorzio del *membro A*

5.  Il *membro A* vota il *membro B* come amministratore

6.  Il *membro A* e il *membro B* votano entrambi il *membro C* come amministratore

Questo processo richiede una sottoscrizione Azure in grado di supportare la distribuzione di più set di scalabilità di macchine virtuali e dischi gestiti. Se necessario, [creare un account gratuito di Azure](https://azure.microsoft.com/free/) per iniziare.

Quando una sottoscrizione è protetta, accedere al portale di Azure. Selezionare "+", Marketplace ("Visualizza tutto") e cercare Ethereum PoA Consortium.

La sezione seguente consente di configurare in modo guidato il footprint del primo membro nella rete. Il flusso di distribuzione è suddiviso in cinque fasi: informazioni di base, aree di distribuzione, dimensioni e prestazioni della rete, impostazioni di Ethereum, Monitoraggio di Azure.

#### <a name="basics"></a>Nozioni di base

In **Informazioni di base** specificare i valori dei parametri standard per qualsiasi distribuzione, ad esempio sottoscrizione, gruppo di risorse e proprietà di base della macchina virtuale.

Segue una descrizione dettagliata di ogni parametro:

Nome parametro|DESCRIZIONE|Valori consentiti|Valori predefiniti    
---|---|---|---
Creare una nuova rete o eseguire l'aggiunta a una esistente?|Creare una nuova rete o eseguire l'aggiunta a una rete di consorzio preesistente|Creazione nuova rete, aggiunta a rete esistente|Creazione di un nuovo sito
Indirizzo di posta elettronica (facoltativo)|Al termine della distribuzione si riceverà una notifica di posta elettronica con le informazioni sulla distribuzione.|Indirizzo di posta elettronica valido|ND
Nome utente macchina virtuale|Nome utente dell'amministratore di ogni macchina virtuale distribuita (solo caratteri alfanumerici)|1-64 caratteri|ND
Tipo di autenticazione|Metodo per l'autenticazione per la macchina virtuale.|Password o chiave pubblica SSH|Password
Password (tipo di autenticazione = password)|Password dell'account dell'amministratore per ognuna delle macchine virtuali distribuite.  La password deve contenere 3 dei requisiti seguenti: un carattere maiuscolo, un carattere minuscolo, un numero e un carattere speciale. Inizialmente tutte le macchine virtuali hanno la stessa password, ma è possibile modificarla dopo il provisioning.|12-72 caratteri|ND
Chiave SSH (tipo di autenticazione = chiave pubblica)|Chiave Secure Shell usata per l'accesso remoto.||ND
Sottoscrizione|La sottoscrizione sul quale eseguire la distribuzione della rete di consorzio||ND
Gruppo di risorse|Gruppo di risorse nel quale eseguire la distribuzione della rete di consorzio.||ND
Località|La regione di Azure per gruppo di risorse.||ND

Di seguito è illustrata una distribuzione di esempio: ![Pannello Informazioni di base](./media/ethereum-poa-deployment-guide/basic-blade.png)

#### <a name="deployment-regions"></a>Regioni di distribuzione

In Deployment regions (Aree di distribuzione) specificare gli input per il numero di aree in cui distribuire la rete di consorzi e la selezione delle aree di Azure in base al numero di aree specificato. La distribuzione da parte dell'utente può avvenire in massimo 5 aree. È consigliabile scegliere la prima area in modo che corrisponda alla località del gruppo di risorse dalla sezione Informazioni di base. Per le reti di sviluppo o di test, è consigliabile una singola area per ogni membro. Per gli ambienti di produzione, è consigliabile eseguire la distribuzione in due o più aree per la disponibilità elevata.

Segue una descrizione dettagliata di ogni parametro:

  Nome parametro|DESCRIZIONE|Valori consentiti|Valori predefiniti
  ---|---|---|---
  Numero di regioni|Numero di aree per la distribuzione della rete di consorzio|1, 2, 3, 4, 5|1
  Prima regione|Prima regione per la distribuzione della rete di consorzio|Tutte le aree di Azure consentite|ND
  Seconda regione|Seconda regione per la distribuzione della rete di consorzio (visibile solo quando è selezionato 2 come numero di regioni)|Tutte le aree di Azure consentite|ND
  Terza regione|Terza regione per la distribuzione della rete di consorzio (visibile solo quando è selezionato 3 come numero di regioni)|Tutte le aree di Azure consentite|ND
  Quarta regione|Quarta regione per la distribuzione della rete di consorzio (visibile solo quando è selezionato 4 come numero di regioni)|Tutte le aree di Azure consentite|ND
  Quinta regione|Quinta regione per la distribuzione della rete di consorzio (visibile solo quando è selezionato 5 come numero di regioni)|Tutte le aree di Azure consentite|ND

Di seguito è illustrata una distribuzione di esempio: ![Aree di distribuzione](./media/ethereum-poa-deployment-guide/deployment-regions.png)

#### <a name="network-size-and-performance"></a>Dimensioni e prestazioni della rete 

In "Network size and performance" (Dimensioni e prestazioni di rete) specificare gli input per la dimensione della rete del consorzio, ad esempio il numero e la dimensione dei nodi di convalida.
Le dimensioni di archiviazione dei nodi convalida determineranno le dimensioni potenziali del blockchain, che possono essere modificate dopo la distribuzione.

Segue una descrizione dettagliata di ogni parametro:

  Nome parametro|DESCRIZIONE|Valori consentiti|Valori predefiniti
  ---|---|---|---
  Number of load balanced validator nodes (Numero di nodi di convalida con carico bilanciato)|Numero di nodi di convalida di cui effettuare il provisioning come parte della rete|2-15|2
  Validator node storage performance (Prestazioni di archiviazione dei nodi di convalida)|Tipo di disco gestito che supporta ogni nodo di convalida distribuito.|Standard o Premium|Standard
  Validator node virtual machine size (Dimensioni della macchina virtuale dei nodi convalida)|Dimensioni della macchina virtuale usata per i nodi di convalida.|A Standard, D Standard, D Standard v2, serie F Standard, DS Standard e FS Standard|D1 Standard v2

Di seguito è illustrata una distribuzione di esempio: ![Dimensioni e prestazioni di rete](./media/ethereum-poa-deployment-guide/network-size-and-performance.png)

#### <a name="ethereum-settings"></a>Impostazioni di Ethereum

Successivamente, in Ethereum Settings (Impostazioni Ethereum) specificare le impostazioni di configurazione relative a Ethereum, ad esempio l'ID di rete e la password dell'account Ethereum o il blocco originale.

Segue una descrizione dettagliata di ogni parametro:

  Nome parametro|DESCRIZIONE|Valori consentiti|Valori predefiniti
  ---|---|---|---
Consortium Member ID (ID membro del consorzio)|L'ID associato a ogni membro che partecipa alla rete di consorzio che consente di configurare spazi di indirizzi IP per evitare conflitti. Nel caso di una rete privata, l'ID membro deve essere univoco per tutte le organizzazioni della stessa rete.  È necessario un ID membro univoco anche quando la stessa organizzazione esegue la distribuzione in più regioni. Prendere nota del valore di questo parametro perché è necessario condividerlo con gli altri membri aggiunti per evitare collisioni.|0-255|ND
ID rete|L'ID di rete per la rete di consorzio Ethereum in fase di realizzazione.  Ogni rete Ethereum ha il proprio ID di rete, di cui 1 è l'ID della rete pubblica.|5 - 999,999,999|10101010
Admin Ethereum Address (Indirizzo Ethereum amministratore)|Indirizzo dell'account Ethereum che viene usato per la partecipazione alla governance PoA.  È consigliabile usare MetaMask per la generazione di un indirizzo Ethereum.|42 caratteri alfanumerici che iniziano con 0x|ND
Opzioni avanzate|Opzioni avanzate per le impostazioni di Ethereum|Abilita o Disabilita|Disabilitazione
IP pubblico (Opzioni avanzate = Abilita)|Distribuisce la rete dietro un gateway di rete virtuale e rimuove l'accesso peering. Se questa opzione è selezionata, tutti i membri devono usare un gateway di rete virtuale perché la connessione sia compatibile.|IP pubblico, Private VNet (Rete virtuale privata)|IP pubblico
Transaction Permission Contract (Contratto autorizzazione transazione) (Opzioni avanzate = Abilita)|Bytecode per il contratto di concessione delle autorizzazioni per le transazioni. Limita la distribuzione e l'esecuzione di uno smart contract a un elenco autorizzato di account Ethereum.|Bytecode del contratto|ND

Di seguito è illustrata una distribuzione di esempio: ![Impostazioni di Ethereum](./media/ethereum-poa-deployment-guide/ethereum-settings.png)

#### <a name="azure-monitor"></a>Monitoraggio di Azure

Il pannello Monitoraggio di Azure consente di configurare una risorsa di Monitoraggio di Azure per la rete. Monitoraggio di Azure raccoglierà e visualizzerà metriche e log utili dalla rete, offrendo la possibilità di controllare rapidamente l'integrità della rete o i problemi di debug.

  Nome parametro|DESCRIZIONE|Valori consentiti|Valori predefiniti
  ---|---|---|---
Monitoraggio|Opzione per abilitare Monitoraggio di Azure|Abilita o Disabilita|Abilita
Connect to existing Log Analytics instance (Connetti all'istanza di Log Analytics esistente)|Creare una nuova istanza di Log Analytics come parte di Monitoraggio di Azure o aggiungere un'istanza esistente|Crea nuova o Join existing (Aggiungi esistente)|Creare un nuovo gruppo di risorse
Azure Monitor Location (Località di Monitoraggio di Azure) (Connect to Existing Azure Monitor (Connetti all'istanza di Monitoraggio di Azure esistente) = Crea nuova)|Area in cui verrà distribuita la nuova istanza di Monitoraggio di Azure|Tutte le aree di Monitoraggio di Azure|ND
Existing Log Analytics Workspace Id (ID area di lavoro di Log Analytics esistente) (Connect to Existing Azure Monitor = Join Existing) (Connetti all'istanza di Monitoraggio di Azure esistente = Aggiungi esistente)|ID area di lavoro dell'istanza di Log Analytics esistente||ND
Existing Log Analytics Primary Key (Chiave primaria di Log Analytics esistente) (Connect to Existing Azure Monitor = Join Existing) (Connetti all'istanza di Monitoraggio di Azure esistente = Aggiungi esistente)|Chiave primaria usata per connettersi all'istanza di Monitoraggio di Azure esistente||ND


Di seguito è illustrata una distribuzione di esempio: ![Monitoraggio di Azure](./media/ethereum-poa-deployment-guide/azure-monitor.png)

#### <a name="summary"></a>Summary

Fare clic su tramite il pannello di riepilogo per analizzare l'input specificato e per eseguire la convalida pre-distribuzione di base. Prima della distribuzione è possibile scaricare il modello e i parametri.

Rivedere i termini legali e sulla privacy e fare clic su "Acquista" per avviare la distribuzione. Se la distribuzione include gateway di rete virtuale, richiederà da 45 a 50 minuti.

#### <a name="post-deployment"></a>Post-distribuzione

##### <a name="deployment-output"></a>Output di distribuzione 

Dopo aver completato la distribuzione, sarà possibile accedere ai parametri necessari tramite il messaggio di posta elettronica di conferma o tramite il portale di Azure. In questi parametri sono disponibili:

-   Endpoint RPC Ethereum

-   URL del dashboard di governance

-   URL di Monitoraggio di Azure

-   URL dati

-   ID risorsa del gateway di rete virtuale (facoltativo)

##### <a name="confirmation-email"></a>Messaggio di posta elettronica di conferma 

Se si specifica un indirizzo di posta elettronica ([sezione Informazioni di base](#basics)), verrà inviato un messaggio a tale indirizzo con le informazioni dell'output della distribuzione.

![Messaggio di posta elettronica distribuzione](./media/ethereum-poa-deployment-guide/deployment-email.png)

##### <a name="portal"></a>Portale

Dopo che la distribuzione è stata completata ed è stato effettuato il provisioning di tutte le risorse, sarà possibile visualizzare i parametri di output nel gruppo di risorse.

1.  Individuare il gruppo di risorse nel portale

2.  Passare a *Distribuzioni*

3.  Selezionare la distribuzione in alto con lo stesso nome del gruppo di risorse

4.  Selezionare *Output*

### <a name="growing-the-consortium"></a>Ampliamento del consorzio

Per espandere il consorzio, è prima di tutto necessario connettere la rete fisica.
Il primo passaggio è semplice se si usa la distribuzione basata sull'IP pubblico. Se si esegue la distribuzione dietro una VPN, vedere la sezione [Connessione del gateway di rete virtuale](#connecting-vnet-gateways) per eseguire la connessione di rete durante il [passaggio 2](#step-2-new-admin-deployment).

#### <a name="step-1-add-the-new-admin"></a>Passaggio 1: Aggiungere il nuovo amministratore

1.  Raccogliere l'indirizzo Ethereum pubblico del nuovo amministratore.

2.  Passare all'app decentralizzata per la governance e creare un nuovo amministratore con l'indirizzo Ethereum e un alias appropriato

3.  Notificare la nuova richiesta agli altri membri esistenti in modo che possano votare per aggiungere questo nuovo amministratore

4.  Dopo che il voto ha raggiunto il 51%, il membro verrà aggiunto come amministratore

![Aggiunta dell'amministratore](./media/ethereum-poa-deployment-guide/adding-admin.png)

#### <a name="step-2-new-admin-deployment"></a>Passaggio 2: Distribuzione del nuovo amministratore

1.  Condividere le informazioni seguenti con il membro aggiunto. Queste informazioni sono reperibili nel messaggio di posta elettronica post-distribuzione o nell'output della distribuzione del portale.

    -  URL di dati di consorzio

    -  Numero di nodi distribuiti

    -  ID risorsa del gateway di rete virtuale (se si usa la VPN)

2.  Per la distribuzione del membro, è consigliabile usare la [stessa soluzione](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) usata per la distribuzione della presenza della rete, tenendo presente quanto segue:

    -  Selezionare *Join Existing* (Aggiungi esistente)

    -  Scegliere lo stesso numero di nodi di convalida del resto dei membri nella rete per assicurare una rappresentazione equa

    -  Usare lo stesso indirizzo Ethereum specificato nel [passaggio precedente](#step-1-add-the-new-admin)

    -  Passare l'*URL di dati di consorzio* nella scheda *Ethereum Settings* (Impostazioni Ethereum)

    -  Se il resto della rete è dietro una VPN, selezionare *Private VNet* (Rete virtuale privata) nella sezione Avanzate

#### <a name="step-3-delegate-validators"></a>Passaggio 3: Delegare i validator

Il nuovo amministratore dovrà autorizzare i validator a partecipare per conto proprio. Questa operazione non può essere eseguita automaticamente perché solo i membri controllano le proprie chiavi.\*

_\*I nodi del primo membro della rete possono essere aggiunti automaticamente perché è possibile precompilare i nodi di convalida nel blocco originale nella rete._

Il nuovo amministratore deve seguire questa procedura:

1.  Aprire l'app decentralizzata per la governance distribuita durante la *loro* distribuzione

2.  Passare alla scheda Validators (Validator)

3.  Selezionare ogni validator distribuito e fare clic su **Add** (Aggiungi)

![Aggiunta di validator](./media/ethereum-poa-deployment-guide/adding-validators.png)

#### <a name="connecting-vnet-gateways"></a>Connessione di gateway di rete virtuale

Nel caso di una rete privata, i diversi membri sono connessi tramite connessioni gateway di rete virtuale. Per potersi aggiungere alla rete e visualizzare il traffico delle transazioni, un membro esistente deve eseguire una configurazione finale sul proprio gateway VPN per accettare la connessione. Ciò significa che i nodi Ethereum del membro che si aggiunge non funzioneranno fino a quando non viene stabilita una connessione. È consigliabile creare connessioni di rete ridondanti (mesh) nel consorzio per ridurre le probabilità di un singolo punto di guasto.

Dopo aver distribuito il nuovo membro, il membro esistente deve completare la connessione bidirezionale configurando una connessione gateway di rete virtuale al nuovo membro. A questo scopo, il membro esistente dovrà avere:

1.  ID risorsa del gateway di rete virtuale del membro che si connette (vedere l'output della distribuzione)

2.  Chiave di connessione condivisa

Il membro esistente deve eseguire lo script di PowerShell seguente per completare la connessione. È consigliabile usare Azure Cloud Shell che si trova nella barra di spostamento in alto a destra nel portale.

![Cloud Shell](./media/ethereum-poa-deployment-guide/cloud-shell.png)

```bash
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzureRmSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzureRmVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

### <a name="service-monitoring"></a>Monitoraggio del servizio

È possibile individuare il portale di Monitoraggio di Azure seguendo il collegamento nel messaggio di posta elettronica di conferma o individuando il parametro nell'output di distribuzione \[OMS\_PORTAL\_URL\].

Il portale visualizza prima di tutto le statistiche di rete generali e la panoramica dei nodi.

![Categorie di monitoraggio](./media/ethereum-poa-deployment-guide/monitor-categories.png)

Selezionando **Node Overview** (Panoramica dei nodi), si accede a un portale per visualizzare le statistiche sull'infrastruttura per ogni nodo.

![Statistiche sui nodi](./media/ethereum-poa-deployment-guide/node-stats.png)

Selezionando **Network Stats** (Statistiche di rete), vengono visualizzate le statistiche di rete di Ethereum.

![Statistiche di rete](./media/ethereum-poa-deployment-guide/network-stats.png)

#### <a name="sample-log-analytics-queries"></a>Query di Log Analytics di esempio

Alla base di questi dashboard c'è un set di log non elaborati sottoponibili a query. È possibile usare questi log non elaborati per personalizzare i dashboard, esaminare gli errori o configurare gli avvisi di soglia. Di seguito è disponibile un set di query di esempio che possono essere eseguite nello strumento di ricerca log:

##### <a name="lists-blocks-that-have-been-reported-by-more-than-one-validator-useful-to-help-find-chain-forks"></a>Elenca i blocchi segnalati da più di un validator. È utile per trovare i fork della catena.

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

##### <a name="get-average-peer-count-for-a-specified-validator-node-averaged-over-5-minute-buckets"></a>Ottenere il conteggio medio dei peer per un nodo di convalida specificato calcolato su bucket di 5 minuti.

```sql
let PeerCountRegex = @"Syncing with peers: (\d+) active, (\d+) confirmed, (\d+)";
ParityLog_CL
| where Computer == "vl-devn3lgdm-reg1000001"
| project RawData, TimeGenerated
| where RawData matches regex PeerCountRegex
| extend ActivePeers = extract(PeerCountRegex, 1, RawData, typeof(int)) 
| summarize avg(ActivePeers) by bin(TimeGenerated, 5m)
```

### <a name="ssh-access"></a>Accesso SSH

Per impostazione predefinita, l'accesso alle porte SSH è negato in base a una regola del gruppo sicurezza di rete per motivi di sicurezza. Per accedere alle istanze dei set di scalabilità di macchine virtuali nella rete PoA, sarà necessario impostare questa regola su \"Consenti\"

1.  Iniziare nella sezione Panoramica del gruppo di risorse distribuite dal portale di Azure.

    ![Panoramica SSH](./media/ethereum-poa-deployment-guide/ssh-overview.png)

2.  Selezionare il gruppo di sicurezza di rete

    ![Gruppo di sicurezza di rete SSH](./media/ethereum-poa-deployment-guide/ssh-nsg.png)

3.  Selezionare la regola \"allow-ssh\"

    ![ssh-allow](./media/ethereum-poa-deployment-guide/ssh-allow.png)

4.  Impostare \"Azione\" su Consenti

    ![Abilitare Consenti per SSH](./media/ethereum-poa-deployment-guide/ssh-enable-allow.png)

5.  Fare clic su \"Salva\". L'applicazione delle modifiche può richiedere qualche minuto.

È ora possibile connettersi in remoto alle macchine virtuali per i nodi di convalida tramite SSH con il nome utente e la password/chiave SSH dell'amministratore forniti.
Il comando SSH da eseguire per accedere al primo nodo di convalida è elencato nel parametro di output di distribuzione del modello come "SSH\_TO\_FIRST\_VL\_NODE\_REGION1" (per la distribuzione di esempio: ssh - p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com). Per ottenere nodi di transazione aggiuntivi, incrementare il numero di porta di uno (ad esempio, il primo nodo di transazione è sulla porta 4000).

Se è stata eseguita la distribuzione in più aree, modificare il comando precedente con il nome DNS o l'indirizzo IP del servizio di bilanciamento del carico in tale area. Per trovare il nome DNS o l'indirizzo IP delle altre aree, trovare la risorsa con la convenzione di denominazione \*\*\*\*\*-lbpip-reg\# e visualizzare le proprietà del nome DNS e dell'indirizzo IP.

### <a name="azure-traffic-manager-load-balancing"></a>Bilanciamento del carico di Gestione traffico di Azure

Gestione traffico di Azure consente di ridurre i tempi di inattività e di migliorare la velocità di risposta della rete PoA grazie al routing del traffico in ingresso tra più distribuzioni in aree diverse. I controlli di integrità predefiniti e il reindirizzamento automatico garantiscono la disponibilità elevata degli endpoint RPC e dell'app decentralizzata per la governance. Questa funzionalità è utile se la distribuzione è stata eseguita in più aree e si è pronti per la produzione.

Usare Gestione traffico per:

-   Migliorare la disponibilità della rete PoA grazie al failover automatico.

-   Aumentare la velocità di risposta delle reti indirizzando gli utenti finali alla località di Azure con la latenza di rete più bassa.

Se si decide di creare un profilo di Gestione traffico, è possibile usare il nome DNS del profilo di accesso alla rete. Dopo che gli altri membri del consorzio sono stati aggiunti alla rete, Gestione traffico può essere usato anche per bilanciare il carico tra i validator distribuiti.

#### <a name="creating-a-traffic-manager-profile"></a>Creazione di un profilo di Gestione traffico

Cercare e selezionare \"Profilo di Gestione traffico\" dopo avere fatto clic sul pulsante \"Crea una risorsa\" nel portale di Azure.

![Cercare Gestione traffico di Azure](./media/ethereum-poa-deployment-guide/traffic-manager-search.png)

Assegnare al profilo un nome univoco e selezionare il gruppo di risorse creato durante la distribuzione PoA.

![Creare il profilo di Gestione traffico](./media/ethereum-poa-deployment-guide/traffic-manager-create.png)

Dopo la distribuzione, selezionare l'istanza nel gruppo di risorse. Il nome DNS per accedere a Gestione traffico è reperibile nella scheda Panoramica

![Individuare il nome DNS di Gestione traffico](./media/ethereum-poa-deployment-guide/traffic-manager-dns.png)

Selezionare la scheda Endpoint e fare clic sul pulsante Aggiungi. Impostare quindi il tipo di risorsa di destinazione su Indirizzo IP pubblico. Selezionare quindi l\'indirizzo IP pubblico del servizio di bilanciamento del carico della prima area.

![Routing di Gestione traffico](./media/ethereum-poa-deployment-guide/traffic-manager-routing.png)

Ripetere la procedura per ogni area nella rete distribuita. Dopo che gli endpoint sono stati \"abilitati\", il carico e l'area verranno automaticamente bilanciati nel nome DNS di Gestione traffico. È ora possibile usare questo nome DNS al posto del parametro \[CONSORTIUM\_DATA\_URL\] negli altri passaggi del documento.

## <a name="data-api"></a>API Dati

Ogni membro del consorzio ospita le informazioni necessarie per consentire agli altri utenti di connettersi alla rete. Il membro esistente fornirà [CONSORTIUM_DATA_URL] prima della distribuzione del membro. Durante la distribuzione, un membro aggiunto recupererà le informazioni dall'interfaccia JSON nell'endpoint seguente:

`<CONSORTIUM_DATA_URL>/networkinfo`

La risposta conterrà informazioni utili per l'aggiunta di membri (blocco originale, ABI del contratto del set di validator, nodi di avvio), oltre a informazioni utili per il membro esistente (indirizzi dei validator). Si consiglia l'uso di questa standardizzazione per estendere il consorzio tra provider di servizi cloud. Questa API restituirà una risposta in formato JSON con la struttura seguente:
```json
{
  "$id": "", 
  "type": "object", 
  "definitions": {}, 
  "$schema": "http://json-schema.org/draft-07/schema#", 
  "properties": {
    "majorVersion": {
      "$id": "/properties/majorVersion", 
      "type": "integer", 
      "title": "This schema’s major version", 
      "default": 0, 
      "examples": [
        0
      ]
    }, 
    "minorVersion": {
      "$id": "/properties/minorVersion", 
      "type": "integer", 
      "title": "This schema’s minor version", 
      "default": 0, 
      "examples": [
        0
      ]
    }, 
    "bootnodes": {
      "$id": "/properties/bootnodes", 
      "type": "array", 
      "items": {
        "$id": "/properties/bootnodes/items", 
        "type": "string", 
        "title": "This member’s bootnodes", 
        "default": "", 
        "examples": [
          "enode://a348586f0fb0516c19de75bf54ca930a08f1594b7202020810b72c5f8d90635189d72d8b96f306f08761d576836a6bfce112cfb6ae6a3330588260f79a3d0ecb@10.1.17.5:30300", 
          "enode://2d8474289af0bb38e3600a7a481734b2ab19d4eaf719f698fe885fb239f5d33faf217a860b170e2763b67c2f18d91c41272de37ac67386f80d1de57a3d58ddf2@10.1.17.4:30300"
        ]
      }
    }, 
    "valSetContract": {
      "$id": "/properties/valSetContract", 
      "type": "string", 
      "title": "The ValidatorSet Contract Source", 
      "default": "", 
      "examples": [
        "pragma solidity 0.4.21;\n\nimport \"./SafeMath.sol\";\nimport \"./Utils.sol\";\n\ncontract ValidatorSet …"
      ]
    }, 
    "adminContract": {
      "$id": "/properties/adminContract", 
      "type": "string", 
      "title": "The AdminSet Contract Source", 
      "default": "", 
      "examples": [
        "pragma solidity 0.4.21;\nimport \"./SafeMath.sol\";\nimport \"./SimpleValidatorSet.sol\";\nimport \"./Admin.sol\";\n\ncontract AdminValidatorSet is SimpleValidatorSet { …"
      ]
    }, 
    "adminContractABI": {
      "$id": "/properties/adminContractABI", 
      "type": "string", 
      "title": "The Admin Contract ABI", 
      "default": "", 
      "examples": [
        "[{\"constant\":false,\"inputs\":[{\"name\":\"proposedAdminAddress\",\"type\":\"address\"},…"
      ]
    }, 
    "paritySpec": {
      "$id": "/properties/paritySpec", 
      "type": "string", 
      "title": "The Parity client spec file", 
      "default": "", 
      "examples": [
        "\n{\n \"name\": \"PoA\",\n \"engine\": {\n \"authorityRound\": {\n \"params\": {\n \"stepDuration\": \"2\",\n \"validators\" : {\n \"safeContract\": \"0x0000000000000000000000000000000000000006\"\n },\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\"\n }\n }\n },\n \"params\": {\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\",\n \"wasmActivationTransition\": \"0x0\"\n },\n \"genesis\": {\n \"seal\": {\n \"authorityRound\": {\n \"step\": \"0x0\",\n \"signature\": \"0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\"\n }\n },\n \"difficulty\": \"0x20000\",\n \"gasLimit\": \"0x2FAF080\"\n },\n \"accounts\": {\n \"0x0000000000000000000000000000000000000001\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ecrecover\", \"pricing\": { \"linear\": { \"base\": 3000, \"word\": 0 } } } },\n \"0x0000000000000000000000000000000000000002\": { \"balance\": \"1\", \"builtin\": { \"name\": \"sha256\", \"pricing\": { \"linear\": { \"base\": 60, \"word\": 12 } } } },\n \"0x0000000000000000000000000000000000000003\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ripemd160\", \"pricing\": { \"linear\": { \"base\": 600, \"word\": 120 } } } },\n \"0x0000000000000000000000000000000000000004\": { \"balance\": \"1\", \"builtin\": { \"name\": \"identity\", \"pricing\": { \"linear\": { \"base\": 15, \"word\": 3 } } } },\n \"0x0000000000000000000000000000000000000006\": { \"balance\": \"0\", \"constructor\" : \"…\" }\n }\n}"
      ]
    }, 
    "errorMessage": {
      "$id": "/properties/errorMessage", 
      "type": "string", 
      "title": "Error message", 
      "default": "", 
      "examples": [
        ""
      ]
    }, 
    "addressList": {
      "$id": "/properties/addressList", 
      "type": "object", 
      "properties": {
        "addresses": {
          "$id": "/properties/addressList/properties/addresses", 
          "type": "array", 
          "items": {
            "$id": "/properties/addressList/properties/addresses/items", 
            "type": "string", 
            "title": "This member’s validator addresses", 
            "default": "", 
            "examples": [
              "0x00a3cff0dccc0ecb6ae0461045e0e467cff4805f", 
              "0x009ce13a7b2532cbd89b2d28cecd75f7cc8c0727"
            ]
          }
        }
      }
    }
  }
}

```
## <a name="development"></a>Sviluppo.

### <a name="programmatically-interacting-with-a-smart-contract"></a>Interazione a livello di codice con uno smart contract

> [!WARNING]
> Non inviare mai la chiave privata Ethereum in rete. Assicurarsi che ogni transazione venga prima firmata in locale e che la transazione firmata venga inviata in rete.

Nell'esempio seguente vengono usati *ethereumjs-wallet* per generare un indirizzo Ethereum, *ethereumjs-tx* per la firma locale e *web3* per inviare la transazione non elaborata all'endpoint RPC Ethereum.

In questo esempio verrà usato questo semplice smart contract Hello-World:

```javascript
pragma solidity ^0.4.11;
contract postBox {
    string message;
    function postMsg(string text) public {
        message = text; 
    }
    function getMsg() public view returns (string) {
        return message;
    }
}
```

In questo esempio si presuppone che il contratto sia già stato distribuito. È possibile usare *solc* e *web3* per distribuire un contratto a livello di codice. Installare prima i moduli dei nodi seguenti:
```
sudo npm install web3@0.20.2
sudo npm install ethereumjs-tx@1.3.6
sudo npm install ethereumjs-wallet@0.6.1
```
Questo script nodeJS eseguirà le operazioni seguenti:

-   Costruire una transazione non elaborata: postMsg

-   Firmare la transazione con la chiave privata generata

-   Inviare la transazione firmata alla rete Ethereum

```javascript
var ethereumjs = require('ethereumjs-tx')
var wallet = require('ethereumjs-wallet')
var Web3 = require('web3')

// TODO Replace with your contract address
var address = "0xfe53559f5f7a77125039a993e8d5d9c2901edc58";
var abi = [{"constant": false,"inputs": [{"name": "text","type": "string"}],"name": "postMsg","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "getMsg","outputs": [{"name": "","type": "string"}],"payable": false,"stateMutability": "view","type": "function"}];

// Generate a new Ethereum account
var account = wallet.generate();
var accountAddress = account.getAddressString()
var privateKey = account.getPrivateKey();

 // TODO Replace with your RPC endpoint
 var web3 = new Web3(new Web3.providers.HttpProvider(
    "http://testzvdky-dns-reg1.eastus.cloudapp.azure.com:8545"));
 
// Get the current nonce of the account
 web3.eth.getTransactionCount(accountAddress, function (err, nonce) {
   var data = web3.eth.contract(abi).at(address).postMsg.getData("Hello World");
   var rawTx = {
     nonce: nonce,
     gasPrice: '0x00', 
     gasLimit: '0x2FAF080',
     to: address, 
     value: '0x00', 
     data: data
   }
   var tx = new ethereumjs(rawTx);
   
   tx.sign(privateKey);

   var raw = '0x' + tx.serialize().toString('hex');
   web3.eth.sendRawTransaction(raw, function (txErr, transactionHash) {
     console.log("TX Hash: " + transactionHash);
     console.log("Error: " + txErr);
   });
 });
```

### <a name="debug-a-smart-contract-truffle-develop"></a>Eseguire il debug di uno smart contract (Truffle Develop)

Truffle ha una rete di sviluppo locale che è disponibile per il debug dello smart contract. Per l'esercitazione completa, vedere [qui](http://truffleframework.com/tutorials/debugging-a-smart-contract).

### <a name="webassembly-wasm-support"></a>Supporto WebAssembly (WASM)

Il supporto WebAssembly è già abilitato nelle nuove reti PoA distribuite. Consente lo sviluppo di smart contract in qualsiasi linguaggio esegua il transpile in Web-Assembly (Rust, C, C++). Per altre informazioni, vedere i collegamenti seguenti

-   Panoramica di WebAssembly in Parity: <https://wiki.parity.io/WebAssembly-Home>

-   Esercitazione di Parity Tech: <https://github.com/paritytech/pwasm-tutorial>

## <a name="faq"></a>Domande frequenti

### <a name="i-notice-a-bunch-of-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>In rete sono presenti alcune transazioni che non sono state inviate dall’\'utente. Da dove provengono?

Non è sicuro sbloccare l'[API personale](https://web3js.readthedocs.io/en/1.0/web3-eth-personal.html). Gli account bot sono in ascolto degli account Ethereum sbloccati e provano a svuotare i fondi. Il bot presuppone che questi account contengano vero ether e che provino a trasferire il saldo per primi. Non abilitare l'API personale nella rete. Firmare invece le transazioni manualmente usando un portafoglio, ad esempio MetaMask, o a livello di codice, come descritto nella sezione Interazione a livello di codice con uno smart contract.

### <a name="how-to-ssh-onto-a-vm"></a>Come si stabilisce una connessione SSH a una VM?

La porta SSH viene bloccata per motivi di sicurezza. Seguire [questa guida per abilitare la porta SSH](#_Accessing_Nodes_via).

### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>Come si configura un membro di controllo o i nodi delle transazioni?

I nodi delle transazioni sono un set di client Parity che sono associati alla rete, ma non partecipano al consenso. Questi nodi possono tuttavia essere usati per inviare transazioni Ethereum e leggere lo stato dello smart contract.
Si tratta di un valido meccanismo per fornire capacità di controllo alle autorità di controllo della rete. A questo scopo, è sufficiente seguire il passaggio 2 da Ampliamento del consorzio.

### <a name="why-are-metamask-transactions-taking-a-long-time"></a>Perché le transazioni MetaMask richiedono molto tempo?

Per garantire che le transazioni vengano ricevute nell'ordine corretto, ogni transazione Ethereum è dotata di un nonce incrementale. Se è stato usato un account in MetaMask su una rete diversa, sarà necessario reimpostare il valore del nonce. Fare clic sull'icona delle impostazioni (3 barre), Impostazioni, Ripristina Account. La cronologia delle transazioni verrà cancellata e sarà possibile inviare di nuovo la transazione.

### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>Le distribuzioni di IP pubblici sono compatibili con le distribuzioni di reti private?

No, il peering richiede la comunicazione bidirezionale e quindi l'intera rete deve essere pubblica o privata.

## <a name="next-steps"></a>Passaggi successivi

Iniziare a usare la soluzione [Ethereum Proof-of-Authority Consortium](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium).
