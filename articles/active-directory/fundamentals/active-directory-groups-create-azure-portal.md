---
title: Creare un gruppo per utenti in Azure AD | Microsoft Docs
description: 'Procedura: Creare un gruppo in Azure Active Directory e aggiungere membri al gruppo'
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/04/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 3c71c9c49413045e3a730c10e90ea3c12648b4cb
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857724"
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Creare un gruppo e aggiungere membri in Azure Active Directory
> [!div class="op_single_selector"]
> * [portale di Azure](active-directory-groups-create-azure-portal.md)
> * [PowerShell](../users-groups-roles/groups-settings-v2-cmdlets.md)

Questo articolo descrive come creare e popolare un nuovo gruppo in Azure Active Directory. Usare un gruppo per eseguire attività di gestione come l'assegnazione di licenze o autorizzazioni per diversi utenti o dispositivi contemporaneamente.

## <a name="how-do-i-create-a-group"></a>Come si crea un gruppo?
1. Accedere al [portale di Azure](https://portal.azure.com) con un account di amministratore globale per la directory.
2. Selezionare **Tutti i servizi**, immettere **Utenti e gruppi** nella casella di testo e quindi premere **INVIO**.

   ![Apertura di Gestione utenti](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Nel pannello **Utenti e gruppi** selezionare **Tutti i gruppi**.

   ![Apertura del pannello Gruppi](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Nel pannello **Utenti e gruppi - Tutti i gruppi** selezionare il comando **Aggiungi**.

   ![Selezione del comando Aggiungi](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Nel pannello **Gruppo** aggiungere un nome e una descrizione per il gruppo.
6. Per selezionare i membri da aggiungere al gruppo, selezionare **Assegnato** nella casella **Tipo di appartenenza** e quindi selezionare **Membri**. Per altre informazioni su come gestire l'appartenenza di un gruppo in modo dinamico, vedere [Utilizzo degli attributi per creare regole avanzate per l'appartenenza al gruppo](../users-groups-roles/groups-dynamic-membership.md).

   ![Selezione dei membri da aggiungere](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Nel pannello **Membri** selezionare uno o più utenti o dispositivi da aggiungere al gruppo e fare clic sul pulsante **Seleziona** nella parte inferiore del pannello per aggiungerli al gruppo. La visualizzazione nella casella **Utente** viene filtrata in base alla corrispondenza con l'immissione di una parte di un nome utente o di dispositivo. I caratteri jolly non sono consentiti nella casella.
8. Dopo avere aggiunto i membri al gruppo, selezionare **Crea** nel pannello **Gruppo**.    

   ![Creare una conferma del gruppo](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Passaggi successivi
Questi articoli forniscono informazioni aggiuntive su Azure Active Directory.

* [Vedere i gruppi esistenti](active-directory-groups-view-azure-portal.md)
* [Gestire le impostazioni di un gruppo](active-directory-groups-settings-azure-portal.md)
* [Gestire i membri di un gruppo](active-directory-groups-members-azure-portal.md)
* [Gestire le appartenenze di un gruppo](active-directory-groups-membership-azure-portal.md)
* [Gestire le regole dinamiche per gli utenti in un gruppo](../users-groups-roles/groups-dynamic-membership.md)
