---
title: Errore durante la creazione di un cluster di Esplora dati di Azure
description: Questo articolo descrive i passaggi di risoluzione dei problemi per la creazione di un cluster in Esplora dati di Azure.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6009953ece78eefd52fca9f12e3db80a6d2cc3eb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953209"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Risoluzione dei problemi: errore durante la creazione di un cluster in Esplora dati di Azure

Nel caso improbabile che si verifichi un errore di creazione del cluster in Esplora dati di Azure, seguire questa procedura.

1. Assicurarsi di disporre delle autorizzazioni appropriate. Per creare un cluster è necessario essere un membro del ruolo *Collaboratore* oppure *Proprietario* della sottoscrizione di Azure. Se necessario rivolgersi all'amministratore della sottoscrizione per essere aggiunti al ruolo appropriato.

1. Assicurarsi che non ci siano errori di validazione relativi al nome del cluster inserito in **Creare cluster** nel portale di Azure.

1. Controllare il [dashboard di integrità dei servizi di Azure](https://azure.microsoft.com/status/>). Cercare lo stato di Esplora dati di Azure nell'area in cui si sta cercando di creare il cluster.

    Se lo stato non è **Buono** (segno di spunta verde), provare a creare il cluster una volta avvenuto un miglioramento dello stato.

1. Per richiedere ulteriore assistenza per la risoluzione del problema, aprire una richiesta di supporto nel [portale di Azure](https://portal.azure.com).