---
title: Creare la prima funzione in Azure con Java e Maven | Microsoft Docs
description: Creare e pubblicare una semplice funzione attivata HTTP in Azure con Java e Maven.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Funzioni di Azure, funzioni, elaborazione eventi, calcolo, architettura senza server
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 16d6dd6a589256ad98a37465e64e847778d0cc7e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092594"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Creare la prima funzione con Java e Maven (anteprima)

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

Questa guida introduttiva fornisce istruzioni dettagliate su come creare un progetto di funzione [senza server](https://azure.microsoft.com/overview/serverless-computing/) con Maven, eseguirne il test in locale e distribuirlo in Funzioni di Azure. Al termine, si otterrà un'app per le funzioni attivate da HTTP in esecuzione in Azure.

![Accedere a una funzione di Hello World dalla riga di comando con cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisiti
Per sviluppare app per le funzioni con Java, è necessario che siano installati gli elementi seguenti:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), versione 8.
-  [Apache Maven](https://maven.apache.org), versione 3.0 o successiva.
-  [Interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Per completare questa guida introduttiva, è necessario impostare la variabile di ambiente JAVA_HOME sul percorso di installazione di JDK.

## <a name="install-the-azure-functions-core-tools"></a>Installare gli strumenti di base per Funzioni di Azure

Gli strumenti di base di Funzioni di Azure offrono un ambiente di sviluppo locale per la scrittura, l'esecuzione e il debug di Funzioni di Azure dal terminale o dal prompt dei comandi. 

Installare la [versione 2 degli strumenti principali](functions-run-local.md#v2) nel computer locale prima di continuare.

## <a name="generate-a-new-functions-project"></a>Generare un nuovo progetto di Funzioni

In una cartella vuota eseguire il comando seguente per generare il progetto di Funzioni da un [archetipo Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven richiederà l'immissione dei valori necessari per completare la generazione del progetto. Per i valori _groupId_, _artifactId_ e _version_, vedere le informazioni di riferimento relative alle [convenzioni di denominazione Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html). Il valore _appName_ deve essere univoco in Azure. Maven genera pertanto un nome di app in base al valore _artifactId_ precedentemente immesso come nome predefinito. Il valore _packageName_ determina il pacchetto Java per il codice funzione generato.

Il valore `appRegion` specifica in quale [area di Azure](https://azure.microsoft.com/global-infrastructure/regions/) si vuole eseguire l'app per le funzioni distribuita. È possibile ottenere un elenco di valori dei nomi delle aree con il comando `az account list-locations` nell'interfaccia della riga di comando di Azure. Il valore `resourceGroup` specifica in quale gruppo di risorse di Azure verrà creata l'app per le funzioni.

Gli identificatori `com.fabrikam.functions` e `fabrikam-functions` riportati più avanti vengono usati come esempio e per semplificare la lettura dei passaggi successivi di questa guida introduttiva. È consigliabile specificare valori personalizzati a Maven in questo passaggio.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus : 
Define value for property 'resourceGroup' java-functions-group: 
Confirm properties configuration: Y
```

Maven crea i file di progetto in una nuova cartella denominata _artifactId_, in questo esempio `fabrikam-functions`. Il codice pronto per l'esecuzione generato nel progetto è una funzione [attivata da HTTP](/azure/azure-functions/functions-bindings-http-webhook) semplice che restituisce il corpo della richiesta dopo una stringa "Hello, ".

```java
public class Function {
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage HttpTriggerJava(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
        } else {
            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
}
```

## <a name="run-the-function-locally"></a>Eseguire la funzione in locale

Passare alla cartella in cui si trova il nuovo progetto creato e quindi generare ed eseguire la funzione con Maven:

```
cd fabrikam-functions
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Se si verifica questa eccezione: `javax.xml.bind.JAXBException` con Java 9, vedere la soluzione alternativa in [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

Questo output viene visualizzato quando la funzione è in esecuzione in locale nel sistema ed è pronta per la risposta alle richieste HTTP:

```Output
Listening on http://0.0.0.0:7071/
Hit CTRL-C to exit...

Http Functions:

        HttpTrigger-Java: http://localhost:7071/api/HttpTrigger-Java
```

Attivare la funzione dalla riga di comando usando curl in una nuova finestra del terminale:

```
curl -w '\n' -d LocalFunctionTest http://localhost:7071/api/HttpTrigger-Java
```

```Output
Hello, LocalFunctionTest
```

Usare `Ctrl-C` nel terminal per interrompere il codice funzione.

## <a name="deploy-the-function-to-azure"></a>Distribuire la funzione in Azure

Il processo di distribuzione in Funzioni di Azure usa le credenziali dell'account dell'interfaccia della riga di comando di Azure. [Accedere tramite l'interfaccia della riga di comando di Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) prima di continuare.

```azurecli
az login
```

Distribuire il codice in una nuova app per le funzioni usando la destinazione Maven `azure-functions:deploy`.

```
mvn azure-functions:deploy
```

Quando la distribuzione è stata completata, viene visualizzato l'URL da usare per accedere all'app per le funzioni di Azure:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Eseguire il test dell'app per le funzioni in esecuzione in Azure usando `cURL`. Sarà necessario modificare l'URL dall'esempio seguente in modo che corrisponda all'URL distribuito per l'app per le funzioni specifica dal passaggio precedente.

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

```Output
Hello, AzureFunctionsTest
```

## <a name="make-changes-and-redeploy"></a>Apportare modifiche e ripetere la distribuzione

Modificare il file di origine `src/main.../Function.java` nel progetto generato per cambiare il testo restituito dall'app per le funzioni. Sostituire questa riga:

```java
return request.createResponse(200, "Hello, " + name);
```

Con la seguente:

```java
return request.createResponse(200, "Hi, " + name);
```

Salvare le modifiche e ripetere la distribuzione eseguendo `azure-functions:deploy` dal terminale, come indicato in precedenza. Verranno aggiornate l'app per le funzioni e questa richiesta:

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

L'output verrà aggiornato:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Passaggi successivi

È stata creata un'app per le funzioni Java con un trigger HTTP semplice ed è stata distribuita in Funzioni di Azure.

- Vedere il [manuale dello sviluppatore di funzioni Java](functions-reference-java.md) per altre informazioni sullo sviluppo di funzioni Java.
- Al progetto aggiungere altre funzioni con trigger diversi usando la destinazione Maven `azure-functions:add`.
- Scrivere ed eseguire il debug delle funzioni in locale con [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) ed [Eclipse](functions-create-maven-eclipse.md). 
- Eseguire il debug delle funzioni distribuite in Azure con Visual Studio Code. Per istruzioni, vedere la documentazione di Visual Studio Code relativa alle [applicazioni Java senza server](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud).
