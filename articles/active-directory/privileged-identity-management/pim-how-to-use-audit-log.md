---
title: Visualizzare la cronologia dei controlli per i ruoli della directory di Azure AD in PIM | Microsoft Docs
description: Informazioni su come visualizzare la cronologia dei controlli per i ruoli della directory di Azure AD in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: ab5a072d845bfdbaafabe1e0e7bdce2dfce6184d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188180"
---
# <a name="view-audit-history-for-azure-ad-directory-roles-in-pim"></a>Visualizzare la cronologia dei controlli per i ruoli della directory di Azure AD in PIM
È possibile usare la cronologia dei controlli di Privileged Identity Management (PIM) per visualizzare tutte le assegnazioni utente e le attivazioni in un determinato periodo di tempo per tutti i ruoli con privilegi. Se si desidera visualizzare la cronologia di controllo completa dell'attività nel tenant, inclusi amministratore, utente finale e attività di sincronizzazione, è possibile usare i [report di accesso e utilizzo di Azure Active Directory.](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-audit-history"></a>Passare alla cronologia dei controlli
Nel dashboard del [portale di Azure](https://portal.azure.com) selezionare l'app **Azure AD Privileged Identity Management** . Da questo punto è possibile accedere alla cronologia dei controlli facendo clic su **Gestione dei ruoli con privilegi** > **Cronologia dei controlli** nel dashboard di PIM.

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
È possibile ordinare i dati per azione e ricercare “L’attivazione è stata approvata”.


## <a name="audit-history-graph"></a>Grafico della cronologia dei controlli
È possibile usare la cronologia dei controlli per visualizzare il totale delle attivazioni, il numero massimo di attivazioni per giorno e il numero medio di attivazioni per giorno in un grafico a linee.  È anche possibile filtrare i dati per ruolo se sono presenti più ruoli nella cronologia di controlli.

Usare i pulsanti relativi a **ora**, **azione** e **ruolo** per ordinare la cronologia.

## <a name="audit-history-list"></a>Elenco della cronologia dei controlli
Le colonne nell'elenco della cronologia dei controlli sono le seguenti:

* **Richiedente** : utente che ha richiesto l'attivazione o la modifica del ruolo.  Se il valore è "Azure System", vedere la cronologia dei controlli di Azure per altre informazioni.
* **Utente** : utente che esegue l'attivazione o che è assegnato a un ruolo.
* **Ruolo** : ruolo assegnato o attivato dall'utente.
* **Azione** : azioni eseguite dal richiedente. Le azioni possono includere assegnazione, annullamento dell'assegnazione, attivazione o disattivazione.
* **Ora** : quando si è verificata l'azione.
* **Motivo** : eventuale testo immesso nel campo del motivo durante l'attivazione.
* **Scadenza** : rilevante solo per l'attivazione dei ruoli.

## <a name="filter-audit-history"></a>Filtrare la cronologia dei controlli
Per filtrare le informazioni visualizzate nella cronologia dei controlli, fare clic sul pulsante **Filtro**.  Verrà visualizzato il pannello **Aggiorna parametri grafico** .

Dopo aver impostato i filtri, fare clic su **Aggiorna** per filtrare i dati della cronologia.  Se i dati non vengono visualizzati immediatamente, aggiornare la pagina.

### <a name="change-the-date-range"></a>Modificare l'intervallo di date
Usare i pulsanti **Oggi**, **Settimana precedente**, **Mese precedente** o **Personalizzato** per modificare l'intervallo di tempo della cronologia dei controlli.

Se si sceglie il pulsante **Personalizzato**, vengono visualizzati i campi di data **Da** e **A** per specificare un intervallo di date per la cronologia.  È possibile immettere le date nel formato MM/GG/AAAA o fare clic sull'icona **calendario** e scegliere la data da un calendario.

### <a name="change-the-roles-included-in-the-history"></a>Modificare i ruoli inclusi nella cronologia
Selezionare o deselezionare la casella di controllo **Ruolo** accanto a ogni ruolo per includerlo o escluderlo dal la cronologia.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passaggi successivi

- [Visualizzare la cronologia dei controlli per i ruoli delle risorse di Azure in PIM](pim-resource-roles-use-the-audit-log.md)
