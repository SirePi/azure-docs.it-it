---
title: Gestire l'accesso alle risorse di Azure con Privileged Identity Management (PIM)
description: Informazioni sulla gestione dell'accesso alle risorse di Azure con Privileged Identity Management (PIM) e con il controllo degli accessi in base al ruolo.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 141cba29f5027ce092775d97c1abe9ecf11badf5
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436046"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Gestire l'accesso alle risorse di Azure con Privileged Identity Management

Per proteggere gli account con privilegi da attacchi informatici, è possibile usare Azure Active Directory Privileged Identity Management (PIM) per ridurre il tempo di esposizione dei privilegi e aumentare la visibilità nel loro uso tramite report e avvisi. Per fare ciò, PIM consente agli utenti di assumere i loro privilegi soltanto "just in time" (JIT), oppure tramite l'assegnazione dei privilegi per una durata abbreviata, dopo la quale i privilegi vengono revocati automaticamente. 

È ora possibile usare PIM con il controllo degli accessi in base al ruolo di Azure per gestire, controllare e monitorare l'accesso alle risorse di Azure. PIM può gestire l'appartenenza dei ruoli predefiniti e personalizzati per consentire di: 

- Abilitare l'accesso on demand, "just in time" alle risorse di Azure
- Terminare l'accesso alle risorse automaticamente per gli utenti e i gruppi assegnati
- Assegnare l'accesso temporaneo alle risorse di Azure per attività rapide o pianificazioni di chiamate
- Ottenere avvisi quando a nuovi utenti o gruppi viene assegnato l'accesso alle risorse e quando vengono attivate assegnazioni idonee

Per altre informazioni, vedere [Panoramica del controllo degli accessi in base al ruolo in Azure PIM](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).