---
title: Configurare le origini di distribuzione per Servizi app in Azure Stack | Documentazione Microsoft
description: Procedura con cui un amministratore del servizio può configurare le origini di distribuzione (Git, GitHub, BitBucket, DropBox e OneDrive) per il servizio app in Azure Stack
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: brenduns
ms.reviewer: anwestg
ms.openlocfilehash: 4945ab0b8da961fc5739388d83f7c645f40e510d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093062"
---
# <a name="configure-deployment-sources"></a>Configurare le origini di distribuzione
*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*


Servizio app in Azure Stack supporta la distribuzione su richiesta da più provider del controllo del codice sorgente. Questa funzionalità consente agli sviluppatori di applicazioni distribuzione direttamente dai propri repository di controllo di origine. Se gli utenti desiderano configurare il servizio App di connettersi ai propri repository, un operatore cloud prima di tutto necessario configurare l'integrazione tra servizio App in Azure Stack e il Provider del controllo codice sorgente.  

Oltre al Git locale, sono supportati i seguenti provider controllo codice sorgente:

* GitHub
* BitBucket
* OneDrive
* DropBox

## <a name="view-deployment-sources-in-app-service-administration"></a>Visualizzazione di origini di distribuzione nell'amministrazione del servizio App

1. Accedere al portale di amministrazione di Azure Stack (https://adminportal.local.azurestack.external) come amministratore del servizio.
2. Passare a  **Resource Providers** (Provider di risorse) e selezionare **App Service Resource Provider Admin** (Amministrazione provider risorse servizio app).  ![App Service Resource Provider Admin (Amministrazione provider risorse servizio app)][1]
3. Fare clic su **Source Control Configuration** (Configurazione controllo codice sorgente).  Ecco un elenco di tutte le origini di distribuzione configurate.
    ![Configurazione controllo codice sorgente amministrazione dei provider di risorse servizio app][2]

## <a name="configure-github"></a>Configurare GitHub

È necessario un account GitHub per completare questa attività. Si potrebbe voler usare un account per l'organizzazione anziché un account personale.

1. Accedere a GitHub, andare https://www.github.com/settings/developers e fare clic su **registrare una nuova applicazione**.
    ![GitHub - registrazione di una nuova applicazione][3]
2. Immettere un **nome dell'applicazione** ad esempio servizio App in Azure Stack.
3. Immettere l'**URL della homepage**. L'URL della Homepage deve essere l'indirizzo di portale di Azure Stack. Ad esempio: https://portal.local.azurestack.external.
4. Immettere un **descrizione dell'applicazione**.
5. Immettere il valore relativo all'**URL di callback autorizzazione**.  In una distribuzione di Azure Stack predefinita, l'Url è nel formato https://portal.local.azurestack.external/TokenAuthorize, se si esegue in un dominio diverso sostituire il dominio per local.azurestack.external
6. Fare clic su **Register application**.  Verrà ora visualizzata una pagina che elenca l'**ID client** e **il segreto client** per l'applicazione.
    ![GitHub - Registrazione applicazione completata][5]
7.  In una finestra o una nuova scheda del browser accedere al portale di amministrazione di Azure Stack (https://adminportal.local.azurestack.external) come amministratore del servizio.
8.  Passare a  **Resource Providers** (Provider di risorse) e selezionare **App Service Resource Provider Admin** (Amministrazione provider risorse servizio app).
9. Fare clic su **Source Control Configuration** (Configurazione controllo codice sorgente).
10. Copiare e incollare il **ID Client** e **privata del Client** nell'input corrispondenti caselle per GitHub.
11. Fare clic su **Save**.

## <a name="configure-bitbucket"></a>Configurare BitBucket

È necessario un account BitBucket per completare questa attività. Si potrebbe voler usare un account per l'organizzazione anziché un account personale.

1. Accedere a BitBucket e passare a **integrazioni** con il proprio account.
    ![Dashboard di BitBucket - integrazioni][7]
2. Fare clic su **OAuth** sotto la gestione degli accessi e **Aggiungi consumer**.
    ![BitBucket aggiungere Consumer OAuth][8]
3. Immettere un **nome** per il consumer, ad esempio servizio App in Azure Stack.
4. Immettere un **descrizione** per l'applicazione.
5. Copiare l'**URL di callback**.  In una distribuzione di Azure Stack predefinita, l'Url di Callback è nel formato https://portal.local.azurestack.external/TokenAuthorize, se si esegue in un dominio diverso sostituire il dominio con azurestack. Local.  L'URL deve seguire le lettere maiuscole come indicato di seguito per una corretta integrazione di BitBucket.
6. Immettere il **URL** -questo Url deve essere l'URL del portale Azure Stack, ad esempio https://portal.local.azurestack.external.
7. Selezionare il **autorizzazioni** obbligatorio:
    - **I repository**: *lettura*
    - **I Webhook**: *lettura e scrittura*
8. Fare clic su **Save**.  Verrà ora visualizzata questa nuova applicazione, insieme alla **chiave** e al **segreto** in **OAuth consumers** (Consumer OAuth).
    ![Elenco applicazioni BitBucket][9]
9.  In una finestra o una nuova scheda del browser accedere al portale di amministrazione di Azure Stack (https://adminportal.local.azurestack.external) come amministratore del servizio.
10.  Passare a  **Resource Providers** (Provider di risorse) e selezionare **App Service Resource Provider Admin** (Amministrazione provider risorse servizio app).
11. Fare clic su **Source Control Configuration** (Configurazione controllo codice sorgente).
12. Copiare e incollare il **chiave** nel **ID Client** casella di input e **Secret** nel **segreto Client** casella di input per BitBucket.
13. Fare clic su **Save**.


## <a name="configure-onedrive"></a>Configurare OneDrive

È necessario un Account Microsoft collegato a un account di OneDrive per completare questa attività.  Si potrebbe voler usare un account per l'organizzazione anziché un account personale.

> [!NOTE]
> Gli account OneDrive for Business non sono attualmente supportati.

1. Passare a https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm e accedere usando Account Microsoft.
2. Sotto **applicazioni personali**, fare clic su **Aggiungi un'app**.
![Applicazioni di OneDrive][10]
3. Immettere un **nome** per la registrazione della nuova applicazione, immettere **Servizio App di Azure Stack** e fare clic su **Crea applicazione**
4. Nella schermata successiva sono elencate le proprietà della nuova applicazione. Record di **ID applicazione**.
![Proprietà applicazione OneDrive][11]
5. In **Segreti applicazione** fare clic su **Genera nuova password**. Annotare **nuova password generata**. Questo è il segreto dell'applicazione e non è recuperabile dopo aver fatto clic **OK** in questa fase.
6. Sotto **piattaforme** fare clic su **Aggiungi piattaforma** e selezionare **Web**.
7. Immettere l'**URI di reindirizzamento**.  In una distribuzione di Azure Stack predefinita, l'URI di reindirizzamento è nel formato https://portal.local.azurestack.external/TokenAuthorize, se si esegue in un dominio diverso sostituire il dominio con azurestack. Local ![applicazione OneDrive - aggiunta piattaforma Web][12]
8. Aggiungere il **autorizzazioni di Microsoft Graph** - **autorizzazioni delegate**
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![Applicazione OneDrive - Autorizzazioni di Microsoft Graph][13]
9. Fare clic su **Save**.
10.  In una finestra o una nuova scheda del browser accedere al portale di amministrazione di Azure Stack (https://adminportal.local.azurestack.external) come amministratore del servizio.
11.  Passare a  **Resource Providers** (Provider di risorse) e selezionare **App Service Resource Provider Admin** (Amministrazione provider risorse servizio app).
12. Fare clic su **Source Control Configuration** (Configurazione controllo codice sorgente).
13. Copiare e incollare il **ID applicazione** nel **ID Client** casella di input e **Password** nel **segreto Client** casella di input per OneDrive.
14. Fare clic su **Save**.

## <a name="configure-dropbox"></a>Configurare Dropbox

> [!NOTE]
> È necessario disporre di un account DropBox per completare questa attività.  È consigliabile usare un account per l'organizzazione anziché un account personale.

1. Passare a https://www.dropbox.com/developers/apps e accedere con il DropBox Account.
2. Fare clic su **Create app**.

    ![Applicazioni di dropbox][14]

3. Selezionare **API di DropBox**.
4. Impostare l'accesso di livello **cartella App**.
5. Immettere un **nome** per l'applicazione.
![Registrazione applicazione Dropbox][15]
6. Fare clic su **Crea app**.  Verrà ora visualizzata una pagina che elenca le impostazioni per l'applicazione, tra cui la **chiave** e il **segreto app**.
7. Verificare i **nome cartella dell'App** è impostata su **servizio App in Azure Stack**.
8. Impostare l'**URI di reindirizzamento OAuth 2** e fare clic su **Aggiungi**.  In una distribuzione di Azure Stack predefinita, l'URI di reindirizzamento è nel formato https://portal.local.azurestack.external/TokenAuthorize, se si esegue in un dominio diverso sostituire il dominio con azurestack. Local.
![Configurazione applicazione dropbox][16]
9.  In una finestra o una nuova scheda del browser accedere al portale di amministrazione di Azure Stack (https://adminportal.local.azurestack.external) come amministratore del servizio.
10.  Passare a  **Resource Providers** (Provider di risorse) e selezionare **App Service Resource Provider Admin** (Amministrazione provider risorse servizio app).
11. Fare clic su **Source Control Configuration** (Configurazione controllo codice sorgente).
12. Copiare e incollare il **rappresenta il tasto** nel **ID Client** casella di input e **segreto dell'App** nel **segreto Client** casella di input per DropBox.
13. Fare clic su **Save**.


<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png

## <a name="next-steps"></a>Passaggi successivi

Gli utenti possono ora usare le origini di distribuzione per elementi quali [distribuzione continua](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment), [distribuzione Git locale](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-local-git), e [cloud la sincronizzazione delle cartelle](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-content-sync).
