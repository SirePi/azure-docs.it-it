---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Creare e inviare un processo | Microsoft Docs
description: Lo script dell'interfaccia della riga di comando di Azure in questo argomento mostra come inviare un processo a una semplice trasformazione di codifica con URL HTTPs.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/11/2018
ms.author: juliako
ms.openlocfilehash: 0b11d095e1c4a5aa75936c0437cbeb5c65550937
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094060"
---
# <a name="cli-example-create-and-submit-a-job"></a>Esempio dell'interfaccia della riga di comando: Creare e inviare un processo

Lo script dell'interfaccia della riga di comando di Azure in questo articolo mostra come creare e inviare un processo a una semplice trasformazione di codifica con URL HTTPs.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, questo articolo richiede la versione 2.0.20 o successiva dell'interfaccia della riga di comando di Azure. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script di esempio

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-jobs/Create-Jobs.sh "Create and submit jobs")]

## <a name="next-steps"></a>Passaggi successivi

Per altri esempi, vedere [Azure CLI samples](../cli-samples.md) (Esempi dell'interfaccia della riga di comando di Azure).
