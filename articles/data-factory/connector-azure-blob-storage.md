---
title: Copiare dati in e da Archiviazione BLOB di Azure usando Data Factory | Microsoft Docs
description: Informazioni su come copiare dati da archivi dati di origine supportati in Archiviazione BLOB di Azure o da Archiviazione BLOB in archivi dati di sink supportati, usando Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: jingwang
ms.openlocfilehash: 46e12378812788d147c903046b50a93c13119f2f
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444589"
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Copiare dati da e in Archiviazione BLOB di Azure usando Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versione 1](v1/data-factory-azure-blob-connector.md)
> * [Versione corrente](connector-azure-blob-storage.md)

Questo articolo illustra come usare l'attività di copia in Azure Data Factory per copiare dati in e da Archiviazione BLOB di Azure. Si basa sull'articolo di [panoramica dell'attività di copia](copy-activity-overview.md) che presenta informazioni generali sull'attività di copia.

## <a name="supported-capabilities"></a>Funzionalità supportate

È possibile copiare dati da qualsiasi archivio di dati di origine supportato in Archiviazione BLOB. È anche possibile copiare dati da Archiviazione BLOB in qualsiasi archivio dati di sink supportato. Per un elenco degli archivi dati supportati come origini o sink dall'attività di copia, vedere la tabella relativa agli [archivi dati supportati](copy-activity-overview.md).

In particolare, il connettore di Archiviazione BLOB supporta:

- La copia di BLOB in e da account di archiviazione di Azure per utilizzo generico e servizi di Archiviazione BLOB ad accesso frequente o sporadico. 
- La copia di BLOB tramite autenticazione con chiave dell'account, firma di accesso condiviso del servizio, entità servizio o identità del servizio gestito.
- La copia di BLOB da BLOB in blocchi, di aggiunta o di pagine e la copia di dati solo in BLOB in blocchi. Archiviazione Premium di Azure non è supportata come sink poiché si basa sui BLOB di pagine.
- La copia di BLOB così come sono o l'analisi o generazione di BLOB con i [formati di file e i codec di compressione supportati](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Attività iniziali

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Le sezioni seguenti riportano informazioni dettagliate sulle proprietà usate per definire entità di Data Factory specifiche per Archiviazione BLOB.

## <a name="linked-service-properties"></a>Proprietà del servizio collegato

Il connettore BLOB di Azure supporta i seguenti tipi di autenticazione. Vedere la sezione corrispondente per le informazioni dettagliate:

- [Autenticazione basata sulla chiave dell'account](#account-key-authentication)
- [Autenticazione con firma di accesso condiviso](#shared-access-signature-authentication)
- [Autenticazione di un'entità servizio](#service-principal-authentication)
- [Autenticazione basata su identità del servizio gestita](#managed-service-identity-authentication)

>[!NOTE]
>HDInsights, Azure Machine Learning e Azure SQL Data Warehouse PolyBase supportano solo l'autenticazione basata sulla chiave dell'account di archiviazione BLOB di Azure.

### <a name="account-key-authentication"></a>Autenticazione basata sulla chiave dell'account

Per usare l'autenticazione basata sulla chiave dell'account di archiviazione, sono supportate le proprietà seguenti:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà tipo deve essere impostata su **AzureBlobStorage** (consigliato) o **AzureStorage** (vedere le note sottostanti). |Yes |
| connectionString | Specificare le informazioni necessarie per connettersi all'archiviazione per la proprietà connectionString. Contrassegnare questo campo come SecureString per archiviarlo in modo sicuro in Azure Data Factory oppure [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). |Sì |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile usare il runtime di integrazione di Azure o il runtime di integrazione self-hosted (se l'archivio dati si trova in una rete privata). Se non specificato, viene usato il runtime di integrazione di Azure predefinito. |No  |

>[!NOTE]
>L'uso di un servizio collegato di tipo "AzureStorage", è ancora supportato così com'è, tuttavia in futuro verrà consigliato di usare il nuovo tipo di servizio collegato "AzureBlobStorage".

**Esempio:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="shared-access-signature-authentication"></a>Autenticazione con firma di accesso condiviso

Una firma di accesso condiviso fornisce accesso delegato controllato alle risorse dell'account di archiviazione. È possibile usarla per concedere a un client autorizzazioni limitate per gli oggetti nell'account di archiviazione per un periodo di tempo specificato. Non è necessario condividere le chiavi di accesso degli account. La firma di accesso condiviso è un URI che racchiude nei parametri di query tutte le informazioni necessarie per l'accesso autenticato a una risorsa di archiviazione. Per accedere alle risorse di archiviazione con la firma di accesso condiviso, il client deve solo passare la firma di accesso condiviso al costruttore o al metodo appropriato. Per altre informazioni sulle firme di accesso condiviso, vedere [Uso delle firme di accesso condiviso](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Data Factory supporta attualmente sia le **firme di accesso condiviso del servizio** che le **firme di accesso condiviso dell'account**. Per altre informazioni su questi due tipi e sulla modalità di costruzione, vedere [Tipi di firme di accesso condiviso](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). 

> [!TIP]
> Per generare una firma di accesso condiviso del servizio per l'account di archiviazione, è possibile eseguire i comandi di PowerShell seguenti. Sostituire i segnaposto e concedere l'autorizzazione necessaria.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Per usare l'autenticazione basata sulla firma di accesso condiviso, sono supportate le proprietà seguenti:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà tipo deve essere impostata su **AzureBlobStorage** (consigliato) o **AzureStorage** (vedere le note sottostanti). |Yes |
| sasUri | Specificare l'URI della firma di accesso condiviso per le risorse di archiviazione come BLOB, contenitore o tabella. Contrassegnare questo campo come SecureString per archiviarlo in modo sicuro in Azure Data Factory oppure [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). |Sì |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile usare Azure Integration Runtime o il runtime di integrazione self-hosted (se l'archivio dati si trova in una rete privata). Se non specificato, viene usato il runtime di integrazione di Azure predefinito. |No  |

>[!NOTE]
>L'uso di un servizio collegato di tipo "AzureStorage", è ancora supportato così com'è, tuttavia in futuro verrà consigliato di usare il nuovo tipo di servizio collegato "AzureBlobStorage".

**Esempio:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Quando si crea un URI di firma di accesso condiviso, tenere presente quanto segue:

- Impostare le autorizzazioni appropriate di lettura o scrittura per gli oggetti in base al modo in cui il servizio collegato (lettura, scrittura, lettura/scrittura) viene usato nella data factory.
- Impostare **Ora di scadenza** in modo appropriato. Assicurarsi che l'accesso agli oggetti di archiviazione non scada nel periodo attivo della pipeline.
- L'URI deve essere creato al giusto livello di contenitore/BLOB o tabella in base alle esigenze. Un URI di firma di accesso condiviso per un BLOB consente a Data Factory di accedere a tale BLOB specifico. Un URI di firma di accesso condiviso per un contenitore di archiviazione BLOB consente a Data Factory di scorrere i BLOB nel contenitore. Per consentire l'accesso a più o meno oggetti in un secondo momento o aggiornare l'URI di firma di accesso condiviso, ricordarsi di aggiornare il servizio collegato con il nuovo URI.

### <a name="service-principal-authentication"></a>Autenticazione di un'entità servizio

Per l'autenticazione con entità servizio di archiviazione di Azure in generale, vedere [Autenticare l'accesso ad Archiviazione di Azure con Azure Active Directory](../storage/common/storage-auth-aad.md).

Per usare l'autenticazione basata su entità servizio, eseguire la procedura seguente:

1. Registrare un'entità applicazione in Azure Active Directory (Azure AD) seguendo le indicazioni fornite in [Registrare l'applicazione con un tenant di Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Prendere nota dei valori seguenti che si usano per definire il servizio collegato:

    - ID applicazione
    - Chiave applicazione
    - ID tenant

2. Concedere all'entità servizio un'autorizzazione appropriata in Archiviazione BLOB di Azure. Fare riferimento a [Gestire i diritti di accesso ai dati di Archiviazione di Azure con RBAC](../storage/common/storage-auth-aad-rbac.md) per altri dettagli sui ruoli.

    - **Come origine**, in Controllo di accesso (IAM), concedere almeno il ruolo **Lettore dei dati dei BLOB di archiviazione**.
    - **Come sink**, in Controllo di accesso (IAM), concedere almeno il ruolo **Collaboratore dei dati dei BLOB di archiviazione**.

Per un servizio collegato ad Archiviazione BLOB di Azure sono supportate queste proprietà:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà type deve essere impostata su: **AzureBlobStorage**. |Yes |
| serviceEndpoint | Specificare l'endpoint del servizio di Archiviazione BLOB di Azure con il criterio `https://<accountName>.blob.core.windows.net/`. |Yes |
| servicePrincipalId | Specificare l'ID client dell'applicazione. | Yes |
| servicePrincipalKey | Specificare la chiave dell'applicazione. Contrassegnare questo campo come **SecureString** per archiviarlo in modo sicuro in Data Factory oppure [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). | Yes |
| tenant | Specificare le informazioni sul tenant (nome di dominio o ID tenant) in cui si trova l'applicazione. Recuperarle passando il cursore del mouse sull'angolo superiore destro del portale di Azure. | Yes |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile usare il runtime di integrazione di Azure o il runtime di integrazione self-hosted (se l'archivio dati si trova in una rete privata). Se non specificato, viene usato il runtime di integrazione di Azure predefinito. |No  |

>[!NOTE]
>L'autenticazione tramite entità servizio è supportata solo dal servizio collegato di tipo "AzureBlobStorage", ma non dal servizio collegato precedente di tipo "AzureStorage".

**Esempio:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-service-identity-authentication"></a>Autenticazione basata su identità del servizio gestita

Una data factory può essere associata a un'[identità del servizio gestito](data-factory-service-identity.md), che rappresenta la data factory specificata. È possibile usare direttamente questa identità del servizio per l'autenticazione con archiviazione BLOB, analogamente all'uso dell'entità servizio. Consente alla factory designata di accedere e copiare i dati da/nella risorsa di archiviazione BLOB.

Per l'autenticazione con identità del servizio gestita di archiviazione di Azure in generale, vedere [Autenticare l'accesso ad Archiviazione di Azure con Azure Active Directory](../storage/common/storage-auth-aad.md).

Per usare l'autenticazione basata sull'identità del servizio gestito (MSI), eseguire la procedura seguente:

1. [Recuperare l'identità del servizio Data Factory](data-factory-service-identity.md#retrieve-service-identity) copiando il valore di "SERVICE IDENTITY APPLICATION ID" generato con la factory.

2. Concedere all'entità servizio un'autorizzazione appropriata in Archiviazione BLOB di Azure. Fare riferimento a [Gestire i diritti di accesso ai dati di Archiviazione di Azure con RBAC](../storage/common/storage-auth-aad-rbac.md) per altri dettagli sui ruoli.

    - **Come origine**, in Controllo di accesso (IAM), concedere almeno il ruolo **Lettore dei dati dei BLOB di archiviazione**.
    - **Come sink**, in Controllo di accesso (IAM), concedere almeno il ruolo **Collaboratore dei dati dei BLOB di archiviazione**.

Per un servizio collegato ad Archiviazione BLOB di Azure sono supportate queste proprietà:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà type deve essere impostata su: **AzureBlobStorage**. |Yes |
| serviceEndpoint | Specificare l'endpoint del servizio di Archiviazione BLOB di Azure con il criterio `https://<accountName>.blob.core.windows.net/`. |Yes |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile usare il runtime di integrazione di Azure o il runtime di integrazione self-hosted (se l'archivio dati si trova in una rete privata). Se non specificato, viene usato il runtime di integrazione di Azure predefinito. |No  |

>[!NOTE]
>Autenticazione identità del servizio gestita è supportata solo dal servizio collegato di tipo "AzureBlobStorage", ma non dal servizio collegato precedente di tipo "AzureStorage". 

**Esempio:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Proprietà dei set di dati

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione dei set di dati, vedere l'articolo [Set di dati](concepts-datasets-linked-services.md). Questa sezione presenta un elenco delle proprietà supportate dal set di dati di Archiviazione BLOB di Azure.

Per copiare dati in e da Archiviazione BLOB, impostare la proprietà type del set di dati su **AzureBlob**. Sono supportate le proprietà seguenti.

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà type del set di dati deve essere impostata su **AzureBlob**. |Yes |
| folderPath | Percorso del contenitore e della cartella nell'archivio BLOB. Il filtro con caratteri jolly non è supportato. Esempio: contenitoreblob/cartellablob/. |Yes |
| fileName | **Filtro con nome o carattere jolly** per i BLOB nell'elemento "folderPath" specificato. Se non si specifica alcun valore per questa proprietà, il set di dati punta a tutti i BLOB nella cartella. <br/><br/>Per un filtro, i caratteri jolly consentiti sono: `*` (corrispondenza di zero o più caratteri) e `?` (corrispondenza di zero caratteri o di un carattere singolo).<br/>- Esempio 1: `"fileName": "*.csv"`<br/>- Esempio 2: `"fileName": "???20180427.txt"`<br/>Usare `^` per il carattere escape se il nome effettivo del file include caratteri jolly o escape.<br/><br/>Se non si specifica fileName per un set di dati di output e non si specifica **preserveHierarchy** nel sink dell'attività, l'attività di copia genera automaticamente il nome BLOB con il modello seguente: "*Data.[GUID ID esecuzione attività].[GUID se FlattenHierarchy].[formato se configurato].[compressione se configurata]*". Un esempio è "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". |No  |
| format | Per copiare i file così come sono tra archivi basati su file (copia binaria), è possibile ignorare la sezione del formato nelle definizioni dei set di dati di input e di output.<br/><br/>Se si vuole analizzare o generare file con un formato specifico, sono supportati i tipi di formato seguenti: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** e **ParquetFormat**. Impostare la proprietà **type** in **format** su uno di questi valori. Per altre informazioni, vedere le sezioni [Formato testo](supported-file-formats-and-compression-codecs.md#text-format), [Formato JSON](supported-file-formats-and-compression-codecs.md#json-format), [Formato AVRO](supported-file-formats-and-compression-codecs.md#avro-format), [Formato OCR](supported-file-formats-and-compression-codecs.md#orc-format) e [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |No (solo per uno scenario di copia binaria) |
| compressione | Specificare il tipo e il livello di compressione dei dati. Per altre informazioni, vedere l'articolo sui [formati di file supportati e i codec di compressione](supported-file-formats-and-compression-codecs.md#compression-support).<br/>I tipi supportati sono **GZip**, **Deflate**, **BZip2** e **ZipDeflate**.<br/>I livelli supportati sono **Ottimale** e **Più veloce**. |No  |

>[!TIP]
>Per copiare tutti i BLOB in una cartella, specificare solo **folderPath**.<br>Per copiare un singolo BLOB con un determinato nome, specificare **folderPath** con il percorso della cartella e **fileName** con il nome del file.<br>Per copiare un subset di BLOB in una cartella, specificare **folderPath** con il percorso della cartella e **fileName** con il filtro con caratteri jolly. 

**Esempio:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Proprietà dell'attività di copia

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, vedere l'articolo sulle [pipeline](concepts-pipelines-activities.md). Questa sezione presenta un elenco delle proprietà supportate dall'origine e dal sink Archiviazione BLOB di Azure.

### <a name="blob-storage-as-a-source-type"></a>Archiviazione BLOB come tipo di origine

Per copiare dati da Archiviazione BLOB, impostare il tipo di origine nell'attività di copia su **BlobSource**. Nella sezione **source** dell'attività di copia sono supportate le proprietà seguenti.

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà type dell'origine dell'attività di copia deve essere impostata su **BlobSource**. |Yes |
| ricorsiva | Indica se i dati vengono letti in modo ricorsivo dalle cartelle secondarie o solo dalla cartella specificata. Si noti che quando la proprietà recursive è impostata su true e il sink è un archivio basato su file, una cartella o una sottocartella vuota non viene copiata o creata nel sink.<br/>I valori consentiti sono **true** (predefinito) e **false**. | No  |

**Esempio:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Archiviazione BLOB come tipo di sink

Per copiare dati in Archiviazione BLOB, impostare il tipo di sink nell'attività di copia su **BlobSink**. Nella sezione **sink** sono supportate le proprietà seguenti.

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà type del sink dell'attività di copia deve essere impostata su **BlobSink**. |Yes |
| copyBehavior | Definisce il comportamento di copia quando l'origine è costituita da file di un archivio dati basato su file.<br/><br/>I valori consentiti sono i seguenti:<br/><b>- PreserveHierarchy (predefinito)</b>: mantiene la gerarchia dei file nella cartella di destinazione. Il percorso relativo del file di origine nella cartella di origine è identico al percorso relativo del file di destinazione nella cartella di destinazione.<br/><b>- FlattenHierarchy</b>: tutti i file della cartella di origine si trovano nel primo livello della cartella di destinazione. I nomi dei file di destinazione vengono generati automaticamente. <br/><b>- MergeFiles</b>: unisce tutti i file della cartella di origine in un solo file. Se viene specificato il nome del file o del BLOB , il nome del file unito sarà il nome specificato. In caso contrario, verrà usato un nome di file generato automaticamente. | No  |

**Esempio:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="some-recursive-and-copybehavior-examples"></a>Alcuni esempi dei valori recursive e copyBehavior

In questa sezione viene descritto il comportamento derivante dell'operazione di copia per diverse combinazioni di valori ricorsivi e copyBehavior.

| ricorsiva | copyBehavior | Struttura della cartella di origine | Destinazione risultante |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La cartella di destinazione Cartella1 viene creata con la stessa struttura dell'origine:<br/><br/>Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 |
| true |flattenHierarchy | Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La Cartella1 di destinazione viene creata con la struttura seguente: <br/><br/>Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File5 |
| true |mergeFiles | Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La Cartella1 di destinazione viene creata con la struttura seguente: <br/><br/>Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Il contenuto di File1 + File2 + File3 + File4 + File 5 viene unito in un file con nome generato automaticamente. |
| false |preserveHierarchy | Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La Cartella1 di destinazione viene creata con la struttura seguente: <br/><br/>Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>La sottocartella1 con File3, File4 e File5 non viene considerata. |
| false |flattenHierarchy | Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La Cartella1 di destinazione viene creata con la struttura seguente: <br/><br/>Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nome generato automaticamente per File2<br/><br/>La sottocartella1 con File3, File4 e File5 non viene considerata. |
| false |mergeFiles | Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sottocartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La Cartella1 di destinazione viene creata con la struttura seguente<br/><br/>Cartella1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Il contenuto di File1 + File2 viene unito in un file con un nome di file generato automaticamente. Nome generato automaticamente per File1<br/><br/>La sottocartella1 con File3, File4 e File5 non viene considerata. |

## <a name="next-steps"></a>Passaggi successivi
Per un elenco degli archivi dati supportati come origini e sink dall'attività di copia in Data Factory, vedere gli [archivi dati supportati](copy-activity-overview.md##supported-data-stores-and-formats).
