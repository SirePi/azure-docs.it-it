---
title: Approvare o rifiutare le richieste per i ruoli della directory di Azure AD in PIM | Microsoft Docs
description: Informazioni su come approvare o rifiutare le richieste per i ruoli della directory di Azure in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9402824540f965cb89aa00791d093bd87712a89a
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665843"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Approvare o rifiutare le richieste per i ruoli della directory di Azure AD in PIM

Con Azure AD Privileged Identity Management (PIM) è possibile configurare ruoli per richiedere l'approvazione per l'attivazione e scegliere uno o più utenti o gruppi come responsabili dell'approvazione con delega. Seguire i passaggi descritti in questo articolo per approvare o rifiutare le richieste per i ruoli della directory di Azure AD.

## <a name="view-pending-requests"></a>Visualizzare le richieste in sospeso

In qualità di responsabile dell'approvazione con delega si riceverà una notifica di posta elettronica quando una richiesta per i ruoli della directory è in attesa di approvazione. È possibile visualizzare queste richieste in sospeso in PIM.

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Aprire **Azure AD Privileged Identity Management**.

1. Fare clic su **Ruoli della directory di Azure AD**.

1. Fare clic su **Approva richieste**.

    ![Ruoli della directory di Azure AD PIM - Ruoli](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    È possibile visualizzare un elenco delle richieste in attesa di approvazione.

## <a name="approve-requests"></a>Approvare le richieste

1. Selezionare le richieste da approvare e quindi fare clic su **Approva** per aprire il riquadro Approva le richieste selezionate.

    ![Elenco delle richieste in attesa di approvazione in PIM](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. Digitare un motivo nella casella **Motivo dell'approvazione**.

    ![Approvazione delle richieste selezionate in PIM](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Fare clic su **Approve** (Approva).

    L'icona di stato verrà aggiornata per indicare l'approvazione.

    ![Approvazione delle richieste selezionate in PIM](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Rifiutare le richieste

1. Selezionare le richieste da rifiutare e quindi fare clic su **Nega** per aprire il riquadro Rifiuta le richieste selezionate.

    ![Elenco delle richieste in attesa di approvazione in PIM](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. Digitare un motivo nella casella **Motivo del rifiuto**.

    ![Rifiutare le richieste selezionate in PIM](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Fare clic su **Nega**.

    L'icona di stato verrà aggiornata per indicare il rifiuto.

## <a name="next-steps"></a>Passaggi successivi

- [Notifiche tramite posta elettronica in PIM](pim-email-notifications.md)
- [Approvare o rifiutare le richieste per i ruoli delle risorse di Azure in PIM](pim-resource-roles-approval-workflow.md)
