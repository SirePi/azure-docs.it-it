---
title: Creare il database MySQL a disponibilità elevata in Azure Stack | Microsoft Docs
description: Informazioni su come creare un provider di risorse MySQL Server computer host e i database MySQL a disponibilità elevata con Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 17314f921a3d7e8187523b02d3ba47364e7dfb25
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369163"
---
# <a name="tutorial-create-highly-available-mysql-databases"></a>Esercitazione: Creare il database MySQL a disponibilità elevata

Come un utente di tenant di Azure Stack, è possibile configurare le macchine virtuali server per ospitare i database di MySQL Server. Dopo un MySQL cluster è stata creata e gestita da Azure Stack, gli utenti che hanno sottoscritto servizi MySQL possono creare facilmente database MySQL a disponibilità elevata.

Questa esercitazione illustra come usare elementi del marketplace Azure Stack per creare un [MySQL con il cluster di replica](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Questa soluzione Usa più macchine virtuali da replicare i database dal nodo master per un numero configurabile di repliche. Una volta creato, il cluster può quindi essere aggiunto come un Server di Hosting MySQL di Azure Stack, e quindi gli utenti possono creare un database MySQL a disponibilità elevata.

> [!IMPORTANT]
> Il **MySQL con replica** elemento del marketplace Azure Stack potrebbe non essere disponibile per tutti gli ambienti di sottoscrizione cloud di Azure. Verificare che l'elemento del marketplace è disponibile nella sottoscrizione prima di provare a seguire il resto di questo tutoral.

Che cosa si apprenderà:

> [!div class="checklist"]
> * Creare un cluster di MySQL Server da elementi di marketplace
> * Creare un Server di Hosting MySQL in Azure Stack
> * Creare un database MySQL a disponibilità elevata

In questa esercitazione, un tre cluster di MySQL Server di macchina virtuale verrà creato e configurato utilizzando elementi disponibili del marketplace Azure Stack. 

Prima di iniziare i passaggi descritti in questa esercitazione, assicurarsi che l'operatore di Stack di Azure ha reso i seguenti elementi disponibile nel marketplace di Azure Stack:

> [!IMPORTANT]
> Tutti gli elementi seguenti sono necessari per creare il cluster MySQL.

- [MySQL con replica](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Questo è il modello di soluzione Bitnami che verrà usato per la distribuzione di cluster MySQL.
- [Debian 8 "Jessie"](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/credativ.Debian8backports?tab=Overview). Debian 8 "Jessie" con il kernel backports per Microsoft Azure forniti da credativ. Debian GNU/Linux è una delle distribuzioni Linux più popolari.
- [Script personalizzato per linux 2.0](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft.custom-script-linux?tab=Overview). L'estensione Script personalizzato è uno strumento per eseguire attività personalizzazione il macchina virtuale post effettuare il provisioning della macchina virtuale. Quando questa estensione viene aggiunta a una macchina virtuale, è possibile scaricare gli script da archiviazione di Azure ed eseguirli nella macchina virtuale. Attività di estensione Script personalizzata può essere automatizzata tramite i cmdlet di Azure PowerShell e interfaccia della riga di comando multipiattaforma di Azure (xPlat CLI).
- Accesso alla macchina virtuale per l'estensione Linux 1.4.7. L'estensione macchina virtuale l'accesso consente di reimpostare la password, chiave SSH o le configurazioni SSH, pertanto è possibile ottenere nuovamente l'accesso alla macchina virtuale. È anche possibile aggiungere un nuovo utente con password o chiave SSH o eliminare un utente tramite questa estensione. Questa estensione è destinato a macchine virtuali Linux.

  > [!TIP]
  > Sarà in grado di vedere Debian 8 "Jessie", uno script personalizzato per linux o macchina virtuale l'accesso per gli elementi del marketplace estensione Linux durante la creazione di una macchina virtuale dal portale per gli utenti. Contattare l'operatore di Azure Stack per assicurarsi che questi elementi sono stati scaricati da Azure prima di iniziare i passaggi descritti in questa esercitazione.

Per altre informazioni sull'aggiunta di elementi nel Marketplace di Azure Stack, vedere la [Panoramica di Azure Stack Marketplace](.\.\azure-stack-marketplace.md).

Inoltre, è necessario un client SSH, ad esempio [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) per accedere alle macchine virtuali Linux, dopo la distribuzione.

## <a name="create-a-mysql-server-cluster"></a>Creare un cluster di MySQL Server 
Usare la procedura descritta in questa sezione per distribuire il MySQL Server cluster usando il [MySQL con replica](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster) elemento del marketplace. Questo modello distribuisce tre istanze di MySQL Server configurate in un cluster MySQL a disponibilità elevata. Per impostazione predefinita, crea le risorse seguenti:

- Una rete virtuale
- Un gruppo di sicurezza di rete
- Un account di archiviazione
- Un set di disponibilità
- Tre interfacce, una per ogni VM predefinito, di rete
- Un indirizzo IP pubblico (per il cluster MySQL primario della macchina virtuale)
- Tre macchine virtuali di Linux per ospitare il cluster MySQL

> [!NOTE]
> Eseguire questi passaggi dal portale per gli utenti Azure Stack come un utente del tenant con una sottoscrizione che fornisce funzionalità IaaS (calcolo, rete, servizi di archiviazione).

1. Accedere al portale per gli utenti:
    - Per una distribuzione del sistema integrato, l'indirizzo di portale varia in base della soluzione area e nome di dominio esterno. Sarà nel formato https://portal.&lt; *area geografica*&gt;.&lt; *FQDN*&gt;.
    - Se si usa Azure Stack Development Kit (ASDK), l'indirizzo del portale utenti è [ https://portal.local.azurestack.external ](https://portal.local.azurestack.external).

2. Selezionare **\+** **crea una risorsa** > **calcolo**, quindi **MySQL con replica**.

   ![Distribuzione del modello personalizzato](media/azure-stack-tutorial-mysqlrp/createcluster1.png)

3. Fornire informazioni sulla distribuzione di base sui **nozioni di base** pagina. Esaminare i valori predefiniti e modificare in base alle necessità e fare clic su **OK**.<br><br>Come minimo, fornire gli elementi seguenti:
   - Nome della distribuzione (valore predefinito è mysql)
   - Password radice dell'applicazione. Fornire una password alfanumerica di 12 caratteri con **senza caratteri speciali**
   - Nome database dell'applicazione (valore predefinito è bitnami)
   - Numero di repliche di database MySQL macchine virtuali da creare (valore predefinito è 2)
   - Selezionare la sottoscrizione da usare
   - Selezionare il gruppo di risorse da usare o crearne uno nuovo
   - Selezionare il percorso (valore predefinito è locale per ASDK)

   ![Nozioni fondamentali sulla distribuzione](media/azure-stack-tutorial-mysqlrp/createcluster2.png)

4. Nel **configurazione dell'ambiente** pagina, fornire le informazioni seguenti e quindi fare clic su **OK**: 
   - Password o SSH chiave pubblica da usare per l'autenticazione SSH (secure shell). Se si usa una password, deve contenere lettere, numeri e **possibile** può contenere caratteri speciali
   - Dimensioni della macchina virtuale (valore predefinito è Standard D1 v2 VM)
   - Dimensioni in fare clic su GB del disco dati **OK**

   ![Configurazione dell'ambiente](media/azure-stack-tutorial-mysqlrp/createcluster3.png)

5. Esaminare la distribuzione **riepilogo**. Facoltativamente, è possibile scaricare il modello personalizzato e i parametri e quindi fare clic su **OK**.

   ![Summary](media/azure-stack-tutorial-mysqlrp/createcluster4.png)

6. Fare clic su **Create** nel **acquistare** pagina per avviare la distribuzione.

   ![Acquistare](media/azure-stack-tutorial-mysqlrp/createcluster4.png)

    > [!NOTE]
    > La distribuzione richiederà circa un'ora. Assicurarsi che la distribuzione è stata completata e il cluster MySQL è stato configurato completamente prima di continuare. 

7. Dopo che tutte le distribuzioni sono stati completati correttamente, esaminare gli elementi del gruppo di risorse e selezionare il **mysqlip** elemento dell'indirizzo IP pubblico. Registrare l'indirizzo IP pubblico e dominio completo dell'indirizzo IP pubblico per il cluster.<br><br>È necessario fornirlo a un operatore di Azure Stack, che consente di creare un server di hosting MySQL sfruttando questo cluster MySQL.


### <a name="create-a-network-security-group-rule"></a>Creare una regola del gruppo di sicurezza di rete
Per impostazione predefinita, nessun accesso pubblico è configurato per MySQL nella macchina virtuale tenant. Per il provider di risorse MySQL di Azure Stack per connettersi e gestire il cluster MySQL, è necessario creare una regola di gruppo (NSG) di sicurezza di rete in ingresso.

1. Nel portale per gli utenti, passare al gruppo di risorse creato quando si distribuisce il cluster MySQL e selezionare il gruppo di sicurezza di rete (**predefinito-subnet-sg**):

   ![apre](media/azure-stack-tutorial-mysqlrp/nsg1.png)

2. Selezionare **regole di sicurezza in ingresso** e quindi fare clic su **Add**.<br><br>Immettere **3306** nel **intervallo di porte di destinazione** e, facoltativamente, fornire una descrizione nel **Name** e **descrizione** campi. Fare clic su Aggiungi per chiudere la finestra di dialogo regola di sicurezza in ingresso.

   ![apre](media/azure-stack-tutorial-mysqlrp/nsg2.png)

### <a name="configure-external-access-to-the-mysql-cluster"></a>Configurare l'accesso esterno al cluster MySQL
Prima che il cluster MySQL può essere aggiunto come un host del MySQL Server di Azure Stack, è necessario abilitare l'accesso esterno.

1. Usando un client SSH, questo esempio viene usato [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), accedere alla macchina primaria MySQL da un computer che può accedere all'indirizzo IP pubblico. Nome della VM MySQL primario è in genere termina con **0** e ha un indirizzo IP pubblico assegnato a esso.<br><br>Usare l'indirizzo IP pubblico e log alla macchina virtuale con il nome utente del **bitnami** e la password dell'applicazione creata in precedenza senza caratteri speciali.

   ![LinuxLogin](media/azure-stack-tutorial-mysqlrp/bitnami1.png)


2. Nella finestra del client SSH, usare il comando seguente per verificare che il servizio bitnami è attiva e funzionante. Fornire di nuovo la password bitnami quando richiesto:

   `sudo service bitnami status`

   ![Servizio di controllo](media/azure-stack-tutorial-mysqlrp/bitnami2.png)

3. Creare un account utente di accesso remoto da utilizzare per il Server di Hosting MySQL di Azure Stack per connettersi a MySQL e quindi chiudere il client SSH.<br><br>Eseguire i comandi seguenti per accedere a MySQL come utente root, usando la password radice creata in precedenza e creare un nuovo utente amministratore, sostituire *\<username\>* e *\<password\>* come richiesto per l'ambiente. In questo esempio, l'utente deve essere creato è denominato **sqlsa** e viene usata una password complessa:

   ```mysql
   mysql -u root -p
   create user <username>@'%' identified by '<password>';
   grant all privileges on *.* to <username>@'%' with grant option;
   flush privileges;
   ```
   ![Creare l'utente amministratore](media/azure-stack-tutorial-mysqlrp/bitnami3.png)


4. Registrare le nuove informazioni utente di MySQL.<br><br>È necessario fornire questo nome utente e password, con l'indirizzo IP pubblico o di dominio completo dell'indirizzo IP pubblico per il cluster, a un operatore di Stack di Azure, che consente di creare un server di hosting MySQL usando il cluster MySQL.


## <a name="create-an-azure-stack-mysql-hosting-server"></a>Creare un Server di Hosting MySQL in Azure Stack
Dopo che il cluster di MySQL Server è stato creato e configurato correttamente, un operatore di Azure Stack deve creare un Azure Stack MySQL Server di Hosting per rendere disponibili agli utenti di creare i database alla capacità aggiuntiva. 

Assicurarsi che per fornire l'operatore di Azure Stack, l'indirizzo IP pubblico o dominio completo per l'indirizzo IP pubblico del cluster MySQL VM primaria che è stato registrato in precedenza quando è stato creato il gruppo di risorse del cluster MySQL (**mysqlip**). Inoltre, l'operatore dovrà conoscere le credenziali di autenticazione creato per accedere in remoto il database di MySQL cluster il MySQL Server.

> [!NOTE]
> Questo passaggio deve essere eseguito dal portale di amministrazione di Azure Stack da un operatore di Azure Stack.

Con indirizzo IP pubblico e MySQL autenticazione account di accesso le informazioni del cluster MySQL fornite dall'utente tenant, un operatore di Stack di Azure possono ora [creare un Server di Hosting MySQL con il nuovo cluster MySQL](.\.\azure-stack-mysql-resource-provider-hosting-servers.md#connect-to-a-mysql-hosting-server). 

Assicurarsi anche che l'operatore di Stack di Azure ha creato i piani e offerte da rendere disponibile la creazione del database MySQL per gli utenti. L'operatore dovrà aggiungere il **Microsoft.MySqlAdapter** a un piano di servizio e creare una nuova quota in modo specifico per i database a disponibilità elevata. Per altre informazioni sulla creazione dei piani, vedere [Panoramica di piano, offerta, quote e sottoscrizione](.\.\azure-stack-plan-offer-quota-overview.md).

> [!TIP]
> Il **Microsoft.MySqlAdapter** servizio non sarà disponibile per l'aggiunta ai piani finché il [provider di risorse di MySQL Server è stato distribuito](.\.\azure-stack-mysql-resource-provider-deploy.md).



## <a name="create-a-highly-available-mysql-database"></a>Creare un database MySQL a disponibilità elevata
Dopo che il cluster MySQL è stato creato, configurato e aggiunto come un Server di Hosting MySQL di Azure Stack da un operatore di Azure Stack, un utente di tenant con una sottoscrizione, incluse le funzionalità di MySQL Server del database può creare database MySQL a disponibilità elevata da la procedura descritta in questa sezione. 

> [!NOTE]
> Eseguire questi passaggi dal portale per gli utenti Azure Stack come un utente del tenant con una sottoscrizione che fornisce funzionalità di MySQL Server (Microsoft.MySQLAdapter servizio).

1. Accedere al portale per gli utenti:
    - Per una distribuzione del sistema integrato, l'indirizzo di portale varia in base della soluzione area e nome di dominio esterno. Sarà nel formato https://portal.&lt; *area geografica*&gt;.&lt; *FQDN*&gt;.
    - Se si usa Azure Stack Development Kit (ASDK), l'indirizzo del portale utenti è [ https://portal.local.azurestack.external ](https://portal.local.azurestack.external).

2. Selezionare **\+** **crea una risorsa** > **dati \+ archiviazione**e quindi **MySQL Database** .<br><br>Fornire le informazioni sulle proprietà di database necessari inclusi nome, le regole di confronto, la sottoscrizione da usare e percorso da utilizzare per la distribuzione. 

   ![Creare il database MySQL](./media/azure-stack-tutorial-mysqlrp/createdb1.png)

3. Selezionare **SKU** e quindi scegliere il MySQL di Hosting Server SKU appropriato da usare. In questo esempio, l'operatore di Stack di Azure ha creato il **MySQL a disponibilità elevata** SKU per supportare la disponibilità elevata per i database cluster MySQL.

   ![Selezionare lo SKU](./media/azure-stack-tutorial-mysqlrp/createdb2.png)

4. Selezionare **account di accesso** > **creare un nuovo account di accesso** e quindi fornire le credenziali di autenticazione da utilizzare per il nuovo database MySQL. Al termine, fare clic su **OK** e quindi **crea** per avviare il processo di distribuzione di database.

   ![Aggiungere account di accesso](./media/azure-stack-tutorial-mysqlrp/createdb3.png)

5. Quando la distribuzione del database MySQL viene completato correttamente, esaminare le proprietà del database per individuare la stringa di connessione da utilizzare per la connessione al nuovo database a disponibilità elevata. 

   ![Visualizza stringa di connessione](./media/azure-stack-tutorial-mysqlrp/createdb4.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un cluster di MySQL Server da elementi di marketplace
> * Creare un Server di Hosting MySQL in Azure Stack
> * Creare un database MySQL a disponibilità elevata

Passare all'esercitazione successiva per apprendere come:
> [!div class="nextstepaction"]
> [Distribuire le App in Azure e Azure Stack](azure-stack-solution-pipeline.md)