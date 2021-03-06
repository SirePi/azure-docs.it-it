---
title: Cluster HPC Pack con Azure Active Directory | Documentazione Microsoft
description: Informazioni su come integrare un cluster Microsoft HPC Pack 2016 in Azure con Azure Active Directory
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/16/2017
ms.author: danlep
ms.openlocfilehash: bb0e878c4e987d111a535603cede25c639087ca7
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2017
ms.locfileid: "25452573"
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Gestire un cluster HPC Pack in Azure con Azure Active Directory
[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) supporta l'integrazione con [Azure Active Directory](../../active-directory/index.md) (Azure AD) per gli amministratori che distribuiscono un cluster HPC Pack in Azure.



Seguire i passaggi descritti in questo articolo per le attività di alto livello seguenti: 
* Integrare manualmente il cluster HPC Pack con il proprio tenant di Azure AD
* Gestire e pianificare i processi nel cluster HPC Pack in Azure 

L'integrazione di una soluzione cluster HPC Pack con Azure AD si attiene ai passaggi standard da seguire per integrare altri servizi e applicazioni. In questo articolo si presuppone di avere dimestichezza con la gestione utente di base in Azure AD. Per altre informazioni e aspetti di fondo, consultare la [documentazione di Azure Active Directory](../../active-directory/index.md) e la sezione seguente.

## <a name="benefits-of-integration"></a>Vantaggi dell'integrazione


Azure Active Directory (Azure AD) è il servizio di gestione di identità e directory basato sul cloud e multi-tenant che offre un accesso SSO alle soluzioni cloud.

L'integrazione di un cluster HPC Pack con Azure AD consente di raggiungere gli obiettivi seguenti:

* Rimuovere il tradizionale controller di dominio di Active Directory dal cluster HPC Pack, così da contribuire a ridurre i costi di manutenzione del cluster se non è necessario per l'azienda, oltre ad accelerare il processo di distribuzione.
* Sfruttare i vantaggi seguenti che vengono offerti da Azure AD:
    *   Single sign-on 
    *   Uso di un'identità di Active Directory locale per il cluster HPC Pack in Azure 

    ![Ambiente di Azure Active Directory](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>prerequisiti
* **Cluster HPC Pack 2016 distribuito nelle macchine virtuali di Azure**: per la procedura vedere [Distribuire un cluster HPC Pack 2016 in Azure](hpcpack-2016-cluster.md). Per completare la procedura descritta in questo articolo, sono necessari il nome DNS del nodo head e le credenziali di un amministratore del cluster.

  > [!NOTE]
  > L'integrazione di Azure Active Directory non è supportata nelle versioni di HPC Pack precedenti a quella del 2016.



* **Computer client**: per eseguire le utilità client di HPC Pack è necessario un computer client Windows o Windows Server. Se si prevede di inviare i processi solo tramite il portale Web di HPC Pack o l'API REST, è possibile usare un computer client qualsiasi.

* **Utilità client di HPC Pack**: installare le utilità client di HPC Pack nel computer client usando il pacchetto di installazione gratuito disponibile nell'Area Download Microsoft.


## <a name="step-1-register-the-hpc-cluster-server-with-your-azure-ad-tenant"></a>Passaggio 1: Registrare il server del cluster HPC con il proprio tenant di Azure AD
1. Accedere al [portale di Azure](https://portal.azure.com).
2. Se l'account consente di accedere a più tenant di Azure AD, fare clic sull'account in alto a destra. Quindi impostare la sessione del portale sul tenant di Azure AD desiderato. È necessario disporre dell'autorizzazione per accedere alle risorse nella directory. 
3. Fare clic su **Azure Active Directory** nel riquadro di spostamento Servizi a sinistra, fare clic su **Utenti e gruppi** e assicurarsi che vi siano account utente già creati o configurati.
4. In **Azure Active Directory** fare clic su **Registrazioni per l'app** > **Registrazione nuova applicazione**. Immettere le seguenti informazioni:
    * **Nome**: HPCPackClusterServer
    * Per **Tipo di applicazione** selezionare **App Web/API**
    * **URL di accesso**: l'URL di base per l'esempio, che per impostazione predefinita è `https://hpcserver`
    * Fare clic su **Crea**.
5. Dopo aver aggiunto l'app, selezionarla nell'elenco **Registrazioni per l'app**. Quindi fare clic su **Impostazioni** > **proprietà**. Immettere le seguenti informazioni:
    * Selezionare **Sì** per **Multi-tenant**.
    * Cambiare **URI ID app** in `https://<Directory_name>/<application_name>`. Sostituire `<Directory_name`> con il nome completo del tenant di Azure AD, ad esempio `hpclocal.onmicrosoft.com` e sostituire `<application_name>` con il nome scelto in precedenza.
6. Fare clic su **Save**. Quando viene completato il salvataggio, nella pagina dell'app fare clic su **Manifesto**. Modificare il manifesto individuando l'impostazione `appRoles` e aggiungendo il ruolo dell'applicazione seguente, quindi fare clic su **Salva**:

  ```json
  "appRoles": [
     {
     "allowedMemberTypes": [
         "User",
         "Application"
     ],
     "displayName": "HpcAdminMirror",
     "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
     "isEnabled": true,
     "description": "HpcAdminMirror",
     "value": "HpcAdminMirror"
     },
     {
     "allowedMemberTypes": [
         "User",
         "Application"
     ],
     "description": "HpcUsers",
     "displayName": "HpcUsers",
     "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
     "isEnabled": true,
     "value": "HpcUsers"
     }
  ],
  ```
7. In **Azure Active Directory** fare clic su **Applicazioni aziendali** > **Tutte le applicazioni**. Selezionare **HPCPackClusterServer** nell'elenco.
8. Fare clic su **Proprietà** e cambiare **Assegnazione utenti obbligatoria** in **Sì**. Fare clic su **Save**.
9. Fare clic su **Utenti e gruppi** > **Aggiungi utente**. Selezionare un utente e un ruolo, quindi fare clic su **Assegna**. Assegnare uno dei ruoli disponibili (HpcUsers o HpcAdminMirror) all'utente. Ripetere questo passaggio con altri utenti nella directory. Per informazioni generali sugli utenti del cluster, vedere [Gestione di utenti cluster](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).


## <a name="step-2-register-the-hpc-cluster-client-with-your-azure-ad-tenant"></a>Passaggio 2: Registrare il client del cluster HPC con il proprio tenant di Azure AD

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Se l'account consente di accedere a più tenant di Azure AD, fare clic sull'account in alto a destra. Quindi impostare la sessione del portale sul tenant di Azure AD desiderato. È necessario disporre dell'autorizzazione per accedere alle risorse nella directory. 
3. In **Azure Active Directory** fare clic su **Registrazioni per l'app** > **Registrazione nuova applicazione**. Immettere le seguenti informazioni:

    * **Nome**: HPCPackClusterClient    
    * In **Tipo applicazione** selezionare **Nativo**
    * **URI di reindirizzamento** - `http://hpcclient`
    * Fare clic su **Crea**

4. Dopo aver aggiunto l'app, selezionarla nell'elenco **Registrazioni per l'app**. Copiare il valore **ID applicazione** e salvarlo. poiché servirà in un secondo momento durante la configurazione dell'applicazione.

5. Fare clic su **Impostazioni** > **Autorizzazioni necessarie** > **Aggiungi** > **Selezionare un'API**. Cercare e aggiungere l'applicazione HpcPackClusterServer (creata al passaggio 1).

6. Nella pagina **Abilita accesso** selezionare **Accesso HpcClusterServer**. Fare quindi clic su **Done**.


## <a name="step-3-configure-the-hpc-cluster"></a>Passaggio 3: Configurare il cluster HPC

1. Connettersi al nodo head di HPC Pack 2016 in Azure.

2. Avviare HPC PowerShell.

3. Eseguire il comando seguente:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcPackClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    dove

    * `AADTenant` specifica il nome del tenant di Azure AD, ad esempio `hpclocal.onmicrosoft.com`
    * `AADClientAppId` specifica l'ID applicazione per l'app creata al passaggio 2.

4. Fare una delle cose seguenti, a seconda della configurazione del nodo head:

    * In un cluster HPC Pack con un singolo nodo head riavviare il servizio HpcScheduler.

    * In un cluster HPC Pack con più nodi head eseguire i comandi PowerShell seguenti nel nodo head per riavviare il servizio HpcSchedulerStateful:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName "fabric:/HpcApplication/SchedulerStatefulService"

    ```

## <a name="step-4-manage-and-submit-jobs-from-the-client"></a>Passaggio 4: Gestire e inviare i processi dal client

Per installare le utilità client di HPC Pack 2016 nel computer, scaricare i file di installazione (installazione completa) dall'Area download Microsoft. All'inizio della procedura di installazione, scegliere l'opzione di installazione per le **utilità client di HPC Pack**.

Per preparare il computer client, installare il certificato usato durante la [configurazione del cluster HPC](hpcpack-2016-cluster.md) nel computer client. Seguire le procedure di gestione del certificato di Windows standard per installare il certificato pubblico nell'archivio **Certificati – utente corrente** > **Autorità di certificazione principale attendibili**. 

È ora possibile eseguire i comandi di HPC Pack o usare l'interfaccia grafica utente del gestore processi di HPC Pack per inviare e gestire i processi cluster usando l'account di Azure AD. Per le opzioni di invio di un processo vedere [Inviare i processi HPC a un cluster HPC in Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> Quando si tenta di connettersi al cluster HPC Pack in Azure per la prima volta, viene visualizzata una finestra popup. Immettere le credenziali di Azure AD per accedere. Il token viene quindi memorizzato nella cache. Per i successivi tentativi di connessione al cluster in Azure verrà usato il token memorizzato nella cache, salvo modifica dell'autenticazione o cancellazione della cache.
>
  
Ad esempio, dopo aver completato i passaggi precedenti, è possibile eseguire una query per i processi da un client locale nel modo seguente:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Cmdlet utili per l'invio di processi con l'integrazione di Azure AD 

### <a name="manage-the-local-token-cache"></a>Gestire la cache locale dei token

HPC Pack 2016 offre i seguenti cmdlet di HPC PowerShell per gestire la cache locale dei token. Questi cmdlet sono utili per inviare processi in modo non interattivo. Vedere l'esempio seguente:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-the-credentials-for-submitting-jobs-using-the-azure-ad-account"></a>Impostare le credenziali per l'invio di processi con l'account di Azure Active Directory 

A volte, è consigliabile eseguire il processo con l'utente del cluster HPC (per un cluster HPC aggiunto a un dominio, eseguito come utente di un dominio, ma per un cluster HPC non appartenente a un dominio, eseguire come un utente locale nel nodo head).

1. Per impostare le credenziali, usare i comandi seguenti:

    ```powershell
    $localUser = "<username>"

    $localUserPassword="<password>"

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Dopodiché, inviare il processo come indicato di seguito. Il processo/attività viene eseguito in $localUser sui nodi di calcolo.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Se `–Credential` non viene specificato con `Submit-HpcJob`, il processo/attività viene eseguito sotto un utente mappato in locale come account di Azure AD. Il cluster HPC crea un utente locale con lo stesso nome dell'account di Azure AD per eseguire l'attività.
    
3. Impostare i dati estesi per l'account Azure Active Directory. Ciò è utile quando si esegue un processo MPI in nodi di Linux utilizzando l'account Azure Active Directory.

   * Impostare i dati estesi per l'account Azure Active Directory

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Impostare i dati estesi ed eseguire come utente del cluster HPC
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

