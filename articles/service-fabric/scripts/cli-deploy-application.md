---
title: Esempio di distribuzione di script dell'interfaccia della riga di comando di Azure Service Fabric (sfctl)
description: Distribuire un'applicazione in un cluster di Azure Service Fabric usando l'interfaccia della riga di comando di Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: TylerMSFT
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/16/2018
ms.author: twhitney
ms.custom: mvc
ms.openlocfilehash: d3e619750c45ac2d8e0b942a304aadfa05301624
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069506"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Distribuire un'applicazione in un cluster di Service Fabric

Questo script di esempio copia un pacchetto dell'applicazione in un archivio immagini del cluster, registra il tipo di applicazione nel cluster e crea un'istanza di applicazione dal tipo di applicazione. Anche i servizi predefiniti vengono creati in questa fase.

Se necessario, installare l'[interfaccia della riga di comando di Service Fabric](../service-fabric-cli.md).

## <a name="sample-script"></a>Script di esempio

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Pulire la distribuzione

Al termine, è possibile usare lo script [remove](cli-remove-application.md) per rimuovere l'applicazione. Lo script remove elimina l'istanza dell'applicazione, annulla la registrazione del tipo di applicazione ed elimina il pacchetto dell'applicazione dall'archivio immagini.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere la [documentazione sull'interfaccia della riga di comando di Service Fabric](../service-fabric-cli.md).

Altri esempi dell'interfaccia della riga di comando di Service Fabric per Azure Service Fabric sono disponibili negli [esempi dell'interfaccia della riga di comando di Service Fabric](../samples-cli.md).
