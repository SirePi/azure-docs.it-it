---
title: Come usare kubectl con Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Sviluppo rapido Kubernetes con contenitori e microservizi in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenitori
manager: douge
ms.openlocfilehash: 978274a6059a327c8596df6776eaa47a27ea4a34
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2018
ms.locfileid: "42145606"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Usare kubectl con uno spazio Azure Dev Spaces

È possibile accedere al cluster Kubernetes all'interno di uno spazio Azure Dev Spaces e usare gli strumenti Kubernetes esistenti, ad esempio `kubectl`.

L'esecuzione del comando `az aks use-dev-spaces` o determinerà l'aggiunta automatica di un contesto di configurazione `kubectl`. Pertanto, kubectl dovrebbe essere già connesso al cluster Kubernetes di Azure Dev Spaces. Esempi:
- Confermare il contesto corrente: `kubectl config current-context`
- Elencare tutti i contesti disponibili: `kubectl config get-contexts`. 
- Cambiare il contesto: `kubectl config use-context <context-name>`
- Visualizzare il dashboard Kubernetes: eseguire `kubectl proxy`, quindi aprire il browser all'indirizzo emesso da questo comando (aggiungere `/ui` all'URL per passare al dashboard Kubernetes).
- Elencare i servizi in esecuzione nello spazio Azure Dev Spaces predefinito denominato *default*: `kubectl get services --namespace=default`

