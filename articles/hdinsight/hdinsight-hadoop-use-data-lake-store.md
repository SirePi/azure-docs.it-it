---
title: Usare Data Lake Store con Hadoop in Azure HDInsight
description: Informazioni su come eseguire query sui dati da Azure Data Lake Store e come archiviare i risultati dell'analisi.
services: hdinsight,storage
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 07/23/2018
ms.openlocfilehash: 3f72e18fbf0f3796d85b4acfb74223b6bea24c6e
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591012"
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>Usare Data Lake Store con cluster Azure HDInsight

Per analizzare i dati in un cluster HDInsight, è possibile archiviarli in [Archiviazione di Azure](../storage/common/storage-introduction.md), in [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) o in entrambe le soluzioni. Entrambe le opzioni di archiviazione consentono l'eliminazione sicura dei cluster HDInsight usati per i calcoli, senza perdita di dati utente.

Questo articolo illustra come usare Data Lake Store con i cluster HDInsight. Per sapere come usare Archiviazione di Azure con i cluster HDInsight, vedere [Usare Archiviazione di Azure con cluster Azure HDInsight](hdinsight-hadoop-use-blob-storage.md). Per altre informazioni sulla creazione di un cluster HDInsight, vedere [Creare cluster Hadoop in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]
> L'accesso a Data Lake Store avviene sempre tramite un canale protetto, pertanto non è presente un nome di schema del file system `adls`. Viene usato sempre `adl`.
> 


## <a name="availability-for-hdinsight-clusters"></a>Disponibilità per i cluster HDInsight

Hadoop supporta una nozione del file system predefinito. Il file system predefinito implica uno schema e un'autorità predefiniti e può essere usato anche per risolvere percorsi relativi. Durante il processo di creazione del cluster HDInsight è possibile specificare un contenitore BLOB in Archiviazione di Azure come file system predefinito. In alternativa, con HDInsight 3.5 e versioni successive è possibile selezionare Archiviazione di Azure o Azure Data Lake Store come file system predefinito, con alcune eccezioni. 

I cluster HDInsight possono usare Data Lake Store in due modi:

* Come risorsa di archiviazione predefinita
* Come risorsa di archiviazione aggiuntiva, con BLOB del servizio di archiviazione di Azure come risorsa predefinita.

Attualmente, solo alcuni dei tipi/versioni di cluster HDInsight supportano l'uso di Data Lake Store come account di archiviazione predefinito e di archiviazione aggiuntivo:

| Tipo di cluster HDInsight | Data Lake Store come risorsa di archiviazione predefinita | Data Lake Store come risorsa di archiviazione aggiuntiva| Note |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight versione 3.6 | Yes | Yes | |
| HDInsight versione 3.5 | Yes | Yes | Ad eccezione di HBase|
| HDInsight versione 3.4 | No  | Yes | |
| HDInsight versione 3.3 | No  | No  | |
| HDInsight versione 3.2 | No  | Yes | |
| Storm | | |È possibile usare Data Lake Store per scrivere dati da una topologia Storm. È anche possibile usare Data Lake Store per archiviare dati di riferimento che possono essere letti da una topologia Storm.|

L'uso di Data Lake Store come account di archiviazione aggiuntivo non ha impatto sulle prestazioni o sulla possibilità di leggere o scrivere nella risorsa di archiviazione di Azure dal cluster.


## <a name="use-data-lake-store-as-default-storage"></a>Usare Data Lake Store come risorsa di archiviazione predefinita

Quando si distribuisce HDInsight con Data Lake Store come risorsa di archiviazione predefinita, i file legati al cluster vengono archiviati in Data Lake Store nel percorso seguente:

    adl://mydatalakestore/<cluster_root_path>/

dove `<cluster_root_path>` è il nome di una cartella creata in Data Lake Store. Specificando un percorso radice per ogni cluster, è possibile usare lo stesso account di Data Lake Store per più di un cluster. Pertanto, è possibile disporre di una configurazione in cui:

* Cluster1 può usare il percorso `adl://mydatalakestore/cluster1storage`
* Cluster2 può usare il percorso `adl://mydatalakestore/cluster2storage`

Si noti che entrambi i cluster usano lo stesso account Data Lake Store **mydatalakestore**. Ogni cluster ha accesso al proprio file system radice in Data Lake Store. Più nello specifico, l'esperienza di distribuzione del Portale di Azure richiede di utilizzare un nome di cartella come **/clusters/\<clustername>** per il percorso radice.

Per poter usare Data Lake Store come risorsa di archiviazione predefinita, è necessario concedere all'entità servizio l'accesso ai percorsi seguenti:

- Radice dell'account Data Lake Store,  ad esempio adl://mydatalakestore/.
- Cartella per tutte le cartelle del cluster,  ad esempio adl://mydatalakestore/clusters.
- Cartella per il cluster,  ad esempio adl://mydatalakestore/clusters/cluster1storage.

Per altre informazioni su come creare un'entità servizio e concedere l'accesso, vedere [Configurare l'accesso a Data Lake Store](#configure-data-lake-store-access).


## <a name="use-data-lake-store-as-additional-storage"></a>Usare Data Lake Store come risorsa di archiviazione aggiuntiva

È anche possibile usare Data Lake Store come risorsa di archiviazione aggiuntiva per il cluster. In questi casi la risorsa di archiviazione predefinita del cluster può essere un BLOB del servizio di archiviazione di Azure o un account Data Lake Store. Se si eseguono processi di HDInsight con i dati archiviati in Data Lake Store come risorsa di archiviazione aggiuntiva, è necessario usare il percorso completo ai file. Ad esempio: 

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Si noti che al momento non esiste alcun **cluster_root_path** nell'URL. Infatti, Data Lake Store non è una risorsa di archiviazione predefinita in questo caso. È sufficiente pertanto indicare il percorso ai file.

Per poter usare Data Lake Store come risorsa di archiviazione aggiuntiva, è necessario semplicemente concedere all'entità servizio l'accesso ai percorsi in cui sono archiviati i file.  Ad esempio: 

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Per altre informazioni su come creare un'entità servizio e concedere l'accesso, vedere [Configurare l'accesso a Data Lake Store](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-store-accounts"></a>Usare più di un account di Data Lake Store

Le operazioni di aggiunta di un account di Data Lake Store come risorsa di archiviazione aggiuntiva e di aggiunta di più account di Data Lake Store vengono eseguite assegnando al cluster HDInsight l'autorizzazione per i dati in uno o più account di Data Lake Store. Vedere [Configurare l'accesso a Data Lake Store](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Configurare l'accesso a Data Lake Store

Per configurare l'accesso a Data Lake Store dal cluster HDInsight, è necessario disporre di un'entità servizio di Azure Active Directory (Azure AD). Solo un amministratore di Azure AD può creare un'entità servizio. L'entità servizio deve essere creata con un certificato. Per altre informazioni, vedi [Avvio rapido: impostazione dei cluster in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) e [Creare un'entità servizio con certificato autofirmato](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]
> Se si prevede di usare Azure Data Lake Store come risorsa di archiviazione aggiuntiva per i cluster HDInsight, è vivamente consigliabile farlo durante la creazione del cluster, come descritto in questo articolo. L'aggiunta di Azure Data Lake Store come risorsa di archiviazione aggiuntiva a un cluster HDInsight esistente non è un'opzione possibile.
>

## <a name="access-files-from-the-cluster"></a>Accedere ai file dal cluster

Esistono diversi modi per accedere ai file in Data Lake Store da un cluster HDInsight.

* **Uso di nomi completi**. Con questo approccio viene fornito il percorso completo al file a cui si desidera accedere.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Uso del formato con percorso abbreviato**. Con questo approccio si sostituisce con adl:/// il percorso fino alla radice del cluster. Nell'esempio precedente, pertanto, è possibile sostituire `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` con `adl:///`.

        adl:///<file path>

* **Uso del percorso relativo**. Con questo approccio viene fornito unicamente il percorso relativo al file a cui si desidera accedere. Ad esempio, se il percorso completo del file è:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    È possibile accedere al medesimo file sample.log usando invece il percorso relativo.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-to-data-lake-store"></a>Creare cluster HDInsight con accesso a Data Lake Store

Usare i collegamenti seguenti per informazioni dettagliate su come creare cluster HDInsight con accesso a Data Lake Store.

* [Uso del portale](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Uso di PowerShell con Data Lake Store come risorsa di archiviazione predefinita](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Uso di PowerShell con Data Lake Store come risorsa di archiviazione aggiuntiva](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Uso di modelli di Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

## <a name="refresh-the-hdinsight-certificate-for-data-lake-store-access"></a>Aggiornare il certificato di HDInsight per l'accesso a Data Lake Store

Il codice di PowerShell di esempio seguente legge un file di certificato locale e aggiorna il cluster HDInsight con il nuovo certificato per accedere a Azure Data Lake Store. Fornire il proprio nome del cluster HDInsight, nome del gruppo di risorse, ID di sottoscrizione, ID dell'app e il percorso locale al certificato. Scrivere la password quando richiesto.

```powershell-interactive
$clusterName = '<clustername>'
$resourceGroupName = '<resourcegroupname>'
$subscriptionId = '01234567-8a6c-43bc-83d3-6b318c6c7305'
$appId = '01234567-e100-4118-8ba6-c25834f4e938'
$generateSelfSignedCert = $false
$addNewCertKeyCredential = $true
$certFilePath = 'C:\localfolder\adls.pfx'
$certPassword = Read-Host "Enter Certificate Password"

if($generateSelfSignedCert)
{
    Write-Host "Generating new SelfSigned certificate"
    
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
else
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
}

Login-AzureRmAccount

if($addNewCertKeyCredential)
{
    Write-Host "Creating new KeyCredential for the app"
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    New-AzureRmADAppCredential -ApplicationId $appId -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    Write-Host "Waiting for 30 seconds for the permissions to get propagated"
    Start-Sleep -s 30
}

Select-AzureRmSubscription -SubscriptionId $subscriptionId
Write-Host "Updating the certificate on HDInsight cluster..."

Invoke-AzureRmResourceAction `
    -ResourceGroupName $resourceGroupName `
    -ResourceType 'Microsoft.HDInsight/clusters' `
    -ResourceName $clusterName `
    -ApiVersion '2015-03-01-preview' `
    -Action 'updateclusteridentitycertificate' `
    -Parameters @{ ApplicationId = $appId; Certificate = $certString; CertificatePassword = $certPassword.ToString() } `
    -Force
```

## <a name="next-steps"></a>Passaggi successivi
In questo articolo è stato descritto come usare Azure Data Lake Store compatibile con HDFS con HDInsight. In questo modo sarà possibile creare soluzioni scalabili di acquisizione e archiviazione a lungo termine dei dati e usare HDInsight per sbloccare le informazioni all'interno dei dati strutturati e non strutturati archiviati.

Per altre informazioni, vedere:

* [Introduzione ad Azure HDInsight][hdinsight-get-started]
* [Guida introduttiva: impostazione dei cluster in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Creare un cluster HDInsight per l'uso di Data Lake Store con Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Caricare dati in HDInsight][hdinsight-upload-data]
* [Usare Hive con HDInsight][hdinsight-use-hive]
* [Usare Pig con HDInsight][hdinsight-use-pig]
* [Usare le firme di accesso condiviso di Archiviazione di Azure per limitare l'accesso ai dati con HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
