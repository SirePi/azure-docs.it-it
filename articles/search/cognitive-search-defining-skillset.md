---
title: Creare un set di competenze in una pipeline di ricerca cognitiva (Ricerca di Azure) | Microsoft Docs
description: Definire le procedure di estrazione dei dati, di elaborazione del linguaggio naturale o di analisi delle immagini per completare ed estrarre informazioni strutturate dai dati a disposizione da usare in Ricerca di Azure.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: luisca
ms.openlocfilehash: 816951ac128fb76d748262cfbc5f064a44e6376c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640927"
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Come creare un set di competenze in una pipeline di arricchimento

La ricerca cognitiva estrae e arricchisce i dati per consentirne la ricerca in Ricerca di Azure. I passaggi di estrazione e arricchimento vengono definiti *competenze cognitive*, combinate in un *set di competenze* a cui viene fatto riferimento durante l'indicizzazione. Un set di competenze può usare [competenze predefinite](cognitive-search-predefined-skills.md) o competenze personalizzate. Per altre informazioni, vedere [Example: create a custom skill](cognitive-search-create-custom-skill-example.md) (Esempio: creare una competenza personalizzata).

Questo articolo descrive come creare una pipeline di arricchimento per le competenze che si vuole usare. Un set di competenze è collegato all'[indicizzatore](search-indexer-overview.md) di Ricerca di Azure. Una parte della progettazione della pipeline, trattata in questo articolo, consiste nella costruzione del set di competenze stesso. 

> [!NOTE]
> L'altra parte della progettazione della pipeline prevede la specifica di un indicizzatore e viene descritta nel [prossimo passaggio](#next-step). Una definizione di indicizzatore include un riferimento al set di competenze, nonché i mapping dei campi usati per connettere gli input agli output nell'indice di destinazione.

Punti principali da tenere presente:

+ È possibile avere solo un set di competenze per indicizzatore.
+ Un set di competenze deve includere almeno una competenza.
+ È possibile creare più competenze dello stesso tipo (ad esempio, varianti di una competenza di analisi di immagini), ma ogni competenza può essere usata una sola volta all'interno dello stesso set di competenze.

## <a name="begin-with-the-end-in-mind"></a>Iniziare definendo l'obiettivo da raggiungere

Il primo passaggio consigliato consiste nel decidere quali dati estrarre dai dati non elaborati e come usarli in una soluzione di ricerca. Per identificare meglio i passaggi necessari, si può creare un'illustrazione dell'intera pipeline di arricchimento.

Si supponga di essere interessati all'elaborazione di un set di commenti di analisti finanziari. Per ogni file si desidera estrarre i nomi delle società e la valutazione generale dei commenti. Si desidera inoltre scrivere un arricchitore personalizzato che usi il servizio Ricerca entità di Bing per ottenere informazioni aggiuntive sulla società, ad esempio il tipo di business svolto della società. Si desidera, in pratica, estrarre informazioni simili alle seguenti, indicizzate per ogni documento:

| Testo del record | Società | Valutazione | Descrizione della società |
|--------|-----|-----|-----|
|record di esempio| ["Microsoft", "LinkedIn"] | 0,99 | ["Microsoft Corporation è una società di tecnologia multinazionale american...", "LinkedIn è un social network orientato al business e all'occupazione..."]

Il diagramma seguente illustra una pipeline di arricchimento ipotetica:

![Pipeline di arricchimento ipotetica](media/cognitive-search-defining-skillset/sample-skillset.png "Pipeline di arricchimento ipotetica")


Quando si ha un'idea chiara del contenuto che si desidera includere nella pipeline, si può specificare il set di competenze per eseguire questi passaggi. A livello funzionale, il set di competenze viene specificato quando si carica la definizione dell'indicizzatore in Ricerca di Azure. Per altre informazioni su come caricare l'indicizzatore, vedere la [documentazione relativa all'indicizzatore](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


Nel diagramma il passaggio di *individuazione del documento* avviene automaticamente. Ricerca di Azure sa fondamentalmente come aprire i file noti e crea un campo di *contenuto* in cui inserisce il testo estratto da ogni documento. Le caselle bianche sono arricchitori integrati e la casella punteggiata "Ricerca entità di Bing" rappresenta un arricchitore personalizzato che si sta creando. Come illustrato, il set di competenze contiene tre competenze.

## <a name="skillset-definition-in-rest"></a>Definizione del set di competenze in REST

Un set di competenze è definito come una matrice di competenze. Ogni competenza definisce l'origine dei relativi input e il nome degli output generati. Tramite l'[API REST di creazione del set di competenze](https://docs.microsoft.com/rest/api/searchservice/create-skillset) è possibile definire un set di competenze corrispondente al diagramma precedente: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

```json
{
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "Calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      },
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
  ]
}
```

## <a name="create-a-skillset"></a>Creare un set di competenze

Durante la creazione di un set di competenze è possibile specificare una descrizione per autodocumentare il set. Una descrizione è facoltativa, ma è utile per tenere traccia dello scopo di un set di competenze. Poiché il set di competenze è un documento JSON, che non consente di includere commenti, è necessario usare un elemento `description` per la descrizione.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

La parte successiva nel set di competenze è una matrice di competenze. Si può pensare a ogni competenza come una primitiva di arricchimento. Ogni competenza esegue una breve attività all'interno di questa pipeline di arricchimento. Ognuna prende un input (o un set di input) e restituisce output. Le prossime sezioni descrivono come specificare le competenze predefinite e personalizzate, concatenando le competenze tra loro tramite riferimenti di input e di output. Gli input possono provenire da dati di origine o da un'altra competenza. Gli output possono essere mappati a un campo in un indice di ricerca o essere usati come input per una competenza a valle.

## <a name="add-predefined-skills"></a>Aggiungere competenze predefinite

Si osservi la prima competenza, che è la [competenza di riconoscimento delle entità denominate](cognitive-search-skill-named-entity-recognition.md) predefinita:

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    }
```

* Ogni competenza predefinita include le proprietà `odata.type`, `input` e `output`. Le proprietà specifiche della competenza includono informazioni aggiuntive applicabili alla competenza stessa. Per il riconoscimento delle entità, `categories` è un'entità tra un set fisso di tipi di entità che il modello precedentemente preparato è in grado di riconoscere.

* Ogni competenza deve avere un ```"context"```. Il contesto rappresenta il livello in cui vengono eseguite le operazioni. Nella competenza precedente il contesto è l'intero documento, vale a dire che la competenza di riconoscimento delle entità denominate viene chiamata una volta per ogni documento. Anche gli output vengono generati in tale livello. L'elemento ```"organizations"```, più specificatamente, viene generato come membro di ```"/document"```. Nelle competenze a valle è possibile fare riferimento a queste informazioni appena create come ```"/document/organizations"```.  Se il campo ```"context"``` non viene impostato in modo esplicito, il contesto predefinito è il documento.

* La competenza include un input denominato "testo", con un input di origine impostato su ```"/document/content"```. La competenza (riconoscimento delle entità denominate) opera sul campo *contenuto* di ogni documento, che è un campo standard creato dall'indicizzatore di BLOB di Azure. 

* La competenza genera un output denominato ```"organizations"```. Gli output esistono solo durante l'elaborazione. Per concatenare questo output all'input della competenza a valle, fare riferimento all'output come ```"/document/organizations"```.

* Per un particolare documento, il valore di ```"/document/organizations"``` è una matrice di organizzazioni estratte dal testo. Ad esempio: 

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Alcune situazioni richiedono che si faccia riferimento a ogni elemento della matrice separatamente. Si supponga ad esempio di voler passare ogni elemento di ```"/document/organizations"``` separatamente a un'altra competenza (ad esempio l'arricchitore di Ricerca entità di Bing personalizzato). È possibile fare riferimento a ogni elemento della matrice aggiungendo un asterisco al percorso: ```"/document/organizations/*"``` 

La seconda competenza che riguarda l'estrazione delle valutazioni segue lo stesso modello del primo arricchitore. Prende ```"/document/content"``` come input e restituisce un punteggio di valutazione per ogni istanza del contenuto. Poiché il campo ```"context"``` non è stato impostato in modo esplicito, l'output (mySentiment) è un figlio di ```"/document"```.

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
```

## <a name="add-a-custom-skill"></a>Aggiungere una competenza personalizzata

Richiamare la struttura dell'arricchitore di Ricerca entità di Bing personalizzato:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      }
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
```

Questa definizione è una competenza personalizzata che chiama un'API Web come parte del processo di arricchimento. Per ogni organizzazione identificata dal riconoscimento delle entità denominate, questa competenza chiama un'API Web per individuare la descrizione dell'organizzazione. L'orchestrazione di quando chiamare l'API Web e come propagare le informazioni ricevute viene gestita internamente dal motore di arricchimento. L'inizializzazione necessaria per chiamare l'API personalizzata deve tuttavia essere specificata nel file JSON (ad esempio URI, intestazioni http e input previsti). Per informazioni sulla creazione di un'API Web personalizzata per la pipeline di arricchimento, vedere [How to define a custom interface](cognitive-search-custom-skill-interface.md) (Come definire un'interfaccia personalizzata).

Si noti che il campo "contesto" è impostato su ```"/document/content/organizations/*"``` con un asterisco. Questo significa che il passaggio di arricchimento viene chiamato *per ogni* organizzazione presente in ```"/document/content/organizations"```. 

L'output, in questo caso la descrizione di una società, viene generato per ogni organizzazione identificata. Quando si fa riferimento alla descrizione in un passaggio a valle (ad esempio, nell'estrazione di frasi chiave), si usa il percorso ```"/document/content/organizations/*/description"``` a tale scopo. 

## <a name="enrichments-create-structure-out-of-unstructured-information"></a>Creazione di strutture da informazioni non strutturate tramite gli arricchimenti

Il set di competenze genera informazioni strutturate da dati non strutturati. Si consideri l'esempio seguente:

*"Nel quarto trimestre, Microsoft ha registrato un fatturato di 1,1 miliardi di dollari derivante da LinkedIn, il social network che ha acquisito lo scorso anno. L'acquisizione consente a Microsoft di combinare le funzionalità di LinkedIn con le funzionalità CRM e di Office. Al momento le parti interessate sono molto soddisfatte dei progressi raggiunti."*

Un probabile risultato può essere una struttura generata simile a quella nella figura riportata di seguito:

![Struttura di output di esempio](media/cognitive-search-defining-skillset/enriched-doc.png "Struttura di output di esempio")

Non va dimenticato che questa struttura è interna. Non è possibile ottenere di fatto questo grafico nel codice.

<a name="next-step"></a>
## <a name="next-steps"></a>Passaggi successivi

Dopo avere acquisito familiarità con la pipeline di arricchimento e i set di competenze, passare a [Come fare riferimento alle annotazioni in un set di competenze](cognitive-search-concept-annotations-syntax.md) o [How to map outputs to fields in an index](cognitive-search-output-field-mapping.md) (Come mappare gli output ai campi di un indice). 
