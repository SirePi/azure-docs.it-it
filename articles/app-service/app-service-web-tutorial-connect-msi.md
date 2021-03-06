---
title: Proteggere la connessione al database SQL di Azure dal servizio app con un'identità del servizio gestito | Microsoft Docs
description: Informazioni su come rendere più sicura la connettività del database usando un'identità del servizio gestito, nonché su come applicare questa funzionalità ad altri servizi di Azure.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 173588c0200666c52f3ac0a5d2e70d667cfe3294
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445562"
---
# <a name="tutorial-secure-sql-database-connection-with-managed-service-identity"></a>Esercitazione: Proteggere la connessione al database SQL con un'identità del servizio gestito

Il [Servizio app](app-service-web-overview.md) fornisce un servizio di hosting Web ad alta scalabilità e con funzioni di auto-correzione in Azure. Offre anche un'[identità del servizio gestito](app-service-managed-service-identity.md) per l'app, una soluzione chiavi in mano per proteggere l'accesso al [database SQL di Azure](/azure/sql-database/) e ad altri servizi di Azure. Le identità del servizio gestito nel servizio app rendono l'app più sicura eliminando i segreti dall'app, come ad esempio le credenziali nelle stringhe di connessione. In questa esercitazione si aggiungerà un'identità del servizio gestito all'app Web ASP.NET di esempio creata in [Esercitazione: Creare un'app ASP.NET in Azure con un database SQL](app-service-web-tutorial-dotnet-sqldatabase.md). Al termine, l'app di esempio si connetterà al database SQL in modo sicuro senza che siano necessari nome utente e password.

Si apprenderà come:

> [!div class="checklist"]
> * Abilitare l'identità del servizio gestito
> * Concedere all'identità del servizio l'accesso al database SQL
> * Configurare il codice dell'applicazione per eseguire l'autenticazione al database SQL con l'autenticazione di Azure Active Directory
> * Concedere privilegi minimi all'identità del servizio nel database SQL

> [!NOTE]
> L'autenticazione di Azure Active Directory è _diversa_ dall'[autenticazione integrata di Windows](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) in Active Directory locale (AD DS). AD DS e Azure Active Directory usano protocolli di autenticazione completamente diversi. Per altre informazioni, vedere [Differenza tra Windows Server AD DS e Azure AD](../active-directory/fundamentals/understand-azure-identity-solutions.md#the-difference-between-windows-server-ad-ds-and-azure-ad).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisiti

Questo articolo riprende come punto di partenza le procedure completate in [Esercitazione: Creare un'app ASP.NET in Azure con un database SQL](app-service-web-tutorial-dotnet-sqldatabase.md). Se non si è ancora provveduto, seguire prima tale esercitazione. In alternativa, è possibile adattare le procedure alla propria app ASP.NET con un database SQL.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-service-identity"></a>Abilitare l'identità del servizio gestito

Per abilitare un'identità del servizio per l'app Azure, usare il comando [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) in Cloud Shell. Nel comando seguente sostituire *\<app name>*.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Di seguito è riportato un esempio dell'output dopo la creazione dell'identità in Azure Active Directory:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

Il valore di `principalId` verrà usato nel passaggio successivo. Per visualizzare i dettagli della nuova identità in Azure Active Directory, eseguire questo comando facoltativo con il valore di `principalId`:

```azurecli-interactive
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>Concedere all'identità l'accesso al database

A questo punto si concede all'identità del servizio dell'app l'accesso al database usando il comando [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin_create) in Cloud Shell. Nel comando seguente sostituire *\<server_name>* e <principalid_from_last_step>. Digitare il nome di un amministratore al posto di *\<admin_user>*.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

L'identità del servizio gestito ha ora accesso al server di database SQL di Azure.

## <a name="modify-connection-string"></a>Modificare la stringa di connessione

Modificare la connessione impostata in precedenza per l'app usando il comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell. Nel comando seguente sostituire *\<app name>* con il nome dell'app e *\<server_name>* e *\<db_name>* con i nomi per il database SQL.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>Modificare il codice ASP.NET

Nel progetto **DotNetAppSqlDb** in Visual Studio aprire _packages.config_ e aggiungere la riga seguente nell'elenco dei pacchetti.

```xml
<package id="Microsoft.Azure.Services.AppAuthentication" version="1.1.0-preview" targetFramework="net461" />
```

Aprire _Models\MyDatabaseContext.cs_ e aggiungere le istruzioni `using` seguenti all'inizio del file:

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

Nella classe `MyDatabaseContext` aggiungere il costruttore seguente:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Questo costruttore configura un oggetto SqlConnection per l'uso di un token di accesso per il database SQL di Azure dal servizio app. Con il token di accesso, l'app del servizio app esegue l'autenticazione al database SQL di Azure con l'identità del servizio gestito. Per altre informazioni, vedere [Ottenimento di token per le risorse di Azure](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources). L'istruzione `if` consente di continuare a testare l'app in locale con Local DB.

> [!NOTE]
> `SqlConnection.AccessToken` è attualmente supportato solo in .NET Framework 4.6 e versioni successive, non in [.NET Core](https://www.microsoft.com/net/learn/get-started/windows).
>

Per usare questo nuovo costruttore, aprire `Controllers\TodosController.cs` e trovare la riga `private MyDatabaseContext db = new MyDatabaseContext();`. Il codice esistente usa il controller `MyDatabaseContext` predefinito per creare un database con la stringa di connessione standard, che prima della [modifica apportata](#modify-connection-string) conteneva nome utente e password in testo non crittografato.

Sostituire l'intera riga con il codice seguente:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new SqlConnection());
```

### <a name="publish-your-changes"></a>Pubblicare le modifiche

A questo punto è sufficiente pubblicare le modifiche in Azure.

In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto **DotNetAppSqlDb** e selezionare **Pubblica**.

![Pubblicare da Esplora soluzioni](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Nella pagina di pubblicazione fare clic su **Pubblica**. Quando nella nuova pagina Web viene visualizzato l'elenco attività, l'app si connette al database con l'identità del servizio gestito.

![App Web di Azure dopo la migrazione Code First](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Dovrebbe ora essere possibile modificare l'elenco attività come prima.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Concedere privilegi minimi all'identità

Durante i passaggi precedenti si sarà probabilmente notato che l'identità del servizio gestito è connessa a SQL Server come amministratore di Azure AD. Per concedere privilegi minimi all'identità del servizio gestito, è necessario accedere al server di database SQL di Azure come amministratore di Azure AD e quindi aggiungere un gruppo di Azure Active Directory contenente l'identità del servizio. 

### <a name="add-managed-service-identity-to-an-azure-active-directory-group"></a>Aggiungere l'identità del servizio gestito a un gruppo di Azure Active Directory

In Cloud Shell aggiungere l'identità del servizio gestito per l'app a un nuovo gruppo di Azure Active Directory denominato _myAzureSQLDBAccessGroup_, come illustrato nello script seguente:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Per visualizzare l'output JSON completo per ogni comando, rimuovere i parametri `--query objectId --output tsv`.

### <a name="reconfigure-azure-ad-administrator"></a>Riconfigurare l'amministratore di Azure AD

In precedenza, l'identità del servizio gestito è stata assegnata come amministratore di Azure AD per il database SQL. Questa identità non può essere usata per l'accesso interattivo (per aggiungere utenti del database), quindi è necessario usare l'utente di Azure AD reale. Per aggiungere l'utente di Azure AD, seguire la procedura descritta in [Effettuare il provisioning di un amministratore di Azure Active Directory per il server di database SQL di Azure](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server). 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Concedere autorizzazioni al gruppo di Azure Active Directory

In Cloud Shell accedere al database SQL con il comando SQLCMD. Sostituire _\<servername>_ con il nome del server di database SQL e _\<AADusername>_ e _\<AADpassword>_ con le credenziali dell'utente di Azure AD.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Al prompt di SQL eseguire questi comandi, che aggiungono il gruppo di Azure Active Directory creato in precedenza come utente.

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
GO
```

Digitare `EXIT` per tornare al prompt di Cloud Shell. Eseguire quindi di nuovo SQLCMD, specificando però il nome del database in _\<dbname>_.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Al prompt di SQL per il database desiderato eseguire questi comandi per concedere le autorizzazioni di lettura e scrittura al gruppo di Azure Active Directory.

```sql
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

## <a name="next-steps"></a>Passaggi successivi

Contenuto dell'esercitazione:

> [!div class="checklist"]
> * Abilitare l'identità del servizio gestito
> * Concedere all'identità del servizio l'accesso al database SQL
> * Configurare il codice dell'applicazione per eseguire l'autenticazione al database SQL con l'autenticazione di Azure Active Directory
> * Concedere privilegi minimi all'identità del servizio nel database SQL

Passare all'esercitazione successiva per apprendere come eseguire il mapping di un nome DNS personalizzato all'app Web.

> [!div class="nextstepaction"]
> [Eseguire il mapping di un nome DNS personalizzato esistente ad app Web di Azure](app-service-web-tutorial-custom-domain.md)
