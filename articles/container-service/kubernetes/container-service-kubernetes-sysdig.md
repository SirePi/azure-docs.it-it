---
title: Monitorare un cluster Azure Kubernetes - Sysdig
description: Monitoraggio di cluster Kubernetes nel servizio contenitore di Azure con Sysdig
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 275e71ce054b83c16b9f9cbfe621c6a7e31f79c6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32162229"
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Monitorare un cluster Kubernetes del servizio contenitore di Azure con Sysdig

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>prerequisiti
Si presume che questa procedura dettagliata abbia [creato un cluster Kubernetes mediante il servizio contenitore di Azure](container-service-kubernetes-walkthrough.md).

Si presume inoltre che gli strumenti azure cli e kubectl siano installati.

È possibile verificare se lo strumento `az` è installato eseguendo:

```console
$ az --version
```

Se lo strumento `az` non è installato, le istruzioni sono disponibili [qui](https://github.com/azure/azure-cli#installation).

È possibile verificare se lo strumento `kubectl` è installato eseguendo:

```console
$ kubectl version
```

Se `kubectl` non è installato, è possibile eseguire:

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a>Sysdig
Sysdig è una società che offre uno strumento di monitoraggio esterno come servizio, che consente di monitorare i contenuti nel cluster Kubernetes in esecuzione in Azure. L'uso di Sysdig richiede un account Sysdig attivo.
È possibile iscriversi per creare un account nel rispettivo [sito](https://app.sysdigcloud.com).

Una volta connessi al sito Web del cloud di Sysdig, fare clic sul nome utente. Nella pagina verrà visualizzata la chiave di accesso. 

![Chiave API di Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Installazione degli agenti Sysdig in Kubernetes
Per monitorare i contenitori, Sysdig esegue un processo in ogni computer usando un oggetto `DaemonSet` di Kubernetes.
Gli oggetti DaemonSet sono oggetti API di Kubernetes che eseguono una singola istanza del contenitore per ogni macchina virtuale.
Sono ottimali per l'installazione di strumenti quale l'agente di monitoraggio di Sysdig.

Per installare gli oggetti DaemonSet di Sysdig, è necessario scaricare il [modello](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) da sysdig. Salvare il file come `sysdig-daemonset.yaml`.

In Linux e OS X è possibile eseguire:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

In PowerShell:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Modificare quindi il file per inserire la chiave di accesso ottenuta dall'account Sysdig.

Al termine, creare l'oggetto DaemonSet:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>Visualizzare il monitoraggio
Dopo l'installazione e l'esecuzione, gli agenti dovrebbero restituire dati a Sysdig.  Tornare al [dashboard sysdig](https://app.sysdigcloud.com) per visualizzare le informazioni sui contenitori.

È anche possibile installare dashboard specifici di Kubernetes tramite la [creazione guidata di un nuovo dashboard](https://app.sysdigcloud.com/#/dashboards/new).
