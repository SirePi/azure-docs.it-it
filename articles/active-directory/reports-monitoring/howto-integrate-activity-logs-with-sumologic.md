---
title: Come integrare i log di Azure Active Directory con SumoLogic tramite Monitoraggio di Azure (anteprima) | Microsoft Docs
description: Informazioni su come integrare i log di Azure Active Directory con SumoLogic tramite Monitoraggio di Azure (anteprima)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2a90e70a9e7951b298be408992ee5e5fc6332131
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729326"
---
# <a name="integrate-azure-ad-logs-with-sumologic-by-using-azure-monitor-preview"></a>Integrate Azure AD logs with SumoLogic by using Azure Monitor (Integrare i log di Azure AD con SumoLogic usando Monitoraggio di Azure) (anteprima)

Questo articolo illustra come integrare i log di Azure Active Directory (Azure AD) con SumoLogic tramite Monitoraggio di Azure. Innanzitutto indirizzare i log a un hub eventi di Azure e quindi integrare l'hub eventi con SumoLogic.

## <a name="prerequisites"></a>Prerequisiti

Per usare questa funzionalità, sono necessari:
* Un hub eventi di Azure contenente i log attività di Azure AD. Informazioni su come [trasmettere i log attività a un hub eventi](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Sottoscrizione di SumoLogic abilitata per l'accesso Single Sign-On.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Procedura per integrare i log di Azure AD con SumoLogic 

1. In primo luogo, [trasmettere i log di Azure AD a un hub eventi di Azure](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. Configurare l'istanza di SumoLogic per [raccogliere i log per Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Installare l'app SumoLogic di Azure AD](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) per usare i dashboard preconfigurati che forniscono un'analisi in tempo reale dell'ambiente.

 ![dashboard](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Passaggi successivi

* [Interpretare lo schema dei log di controllo in Monitoraggio di Azure](reference-azure-monitor-audit-log-schema.md)
* [Interpretare lo schema dei log di accesso in Monitoraggio di Azure](reference-azure-monitor-sign-ins-log-schema.md)
* [Domande frequenti e problemi noti](concept-activity-logs-in-azure-monitor.md#frequently-asked-questions)
