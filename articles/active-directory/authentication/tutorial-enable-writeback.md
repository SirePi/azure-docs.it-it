---
title: Abilitare il writeback delle password in Azure AD
description: In questa esercitazione si abiliterà il writeback delle password per riportare modifiche delle password eseguite nel cloud in AD come parte di Azure AD Connect.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 9512c800a35ad4a819c657b07227d781c63c6399
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163339"
---
# <a name="tutorial-enabling-password-writeback"></a>Esercitazione: Abilitazione del writeback delle password

In questa esercitazione si abiliterà il writeback delle password per l'ambiente ibrido. Il writeback delle password viene usato per sincronizzare le modifiche delle password in Azure Active Directory (Azure AD) nell'ambiente di Active Directory Domain Services (AD DS) in locale. Il writeback delle password è abilitato come parte di Azure AD Connect per fornire un meccanismo sicuro per inviare le modifiche delle password a una directory locale esistente da Azure AD. È possibile trovare altri dettagli sul funzionamento interno del writeback delle password nell'articolo [What is password writeback](concept-sspr-writeback.md) (Informazioni sul writeback delle password).

> [!div class="checklist"]
> * Abilitare l'opzione di writeback delle password in Azure AD Connect
> * Abilitare l'opzione di writeback delle password nella reimpostazione della password self-service

## <a name="prerequisites"></a>Prerequisiti

* Accesso a un tenant di Azure AD funzionante, con almeno una licenza di valutazione assegnata.
* Un account con privilegi di amministratore globale nel tenant di Azure AD.
* Un server esistente configurato che esegue una versione aggiornata di [Azure AD Connect](../connect/active-directory-aadconnect-get-started-express.md).
* Completamento delle esercitazioni precedenti sulla reimpostazione della password self-service.

## <a name="enable-password-writeback-option-in-azure-ad-connect"></a>Abilitare l'opzione di writeback delle password in Azure AD Connect

Per abilitare il writeback delle password, è prima necessario abilitare la funzionalità dal server in cui è stato installato Azure AD Connect.

1. Per configurare e abilitare il writeback delle password accedere al server Azure AD Connect e avviare la configurazione guidata di **Azure AD Connect**.
2. Nella pagina di **benvenuto** selezionare **Configura**.
3. Nella pagina **Attività aggiuntive** selezionare **Personalizzazione delle opzioni di sincronizzazione** e fare clic su **Avanti**.
4. Nella pagina **Connessione ad Azure AD** immettere le credenziali di amministratore globale e selezionare **Avanti**.
5. Nelle pagine di filtro **Connessione delle directory** e **Dominio/unità organizzativa** selezionare **Avanti**.
6. Nella pagina **Funzionalità facoltative** selezionare la casella accanto a **Writeback password** e selezionare **Avanti**.
7. Nella pagina **Pronto per la configurazione** fare clic su **Configura** e attendere il completamento del processo.
8. Quando la configurazione termina, selezionare **Esci**.

## <a name="enable-password-writeback-option-in-sspr"></a>Abilitare l'opzione di writeback delle password nella reimpostazione della password self-service

L'abilitazione della funzionalità di writeback delle password in Azure AD Connect è soltanto parte di un processo più ampio. Per completare il ciclo, occorre consentire alla reimpostazione della password self-service di usare il writeback delle password, in modo che gli utenti che modificano o reimpostano la password possano avere la nuova password impostata anche in locale.

1. Accedere al [portale di Azure](https://portal.azure.com) con un account amministratore globale.
2. Passare ad **Azure Active Directory**, fare clic su **Reimpostazione password** e quindi scegliere **Integrazione locale**.
3. Impostare l'opzione **Eseguire il writeback delle password nella directory locale** su **Sì**.
4. Impostare l'opzione **Consentire agli utenti di sbloccare gli account senza reimpostare la password** su **Sì**.
5. Fare clic su **Save**

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato abilitato il writeback delle password per la reimpostazione della password self-service. Lasciare aperta la finestra del portale di Azure e passare all'esercitazione successiva per configurare impostazioni aggiuntive relative alla reimpostazione della password self-service prima di implementare la soluzione per un gruppo pilota.

> [!div class="nextstepaction"]
> [Reimpostazione della password self-service di Azure AD dalla schermata di accesso di Windows 10](tutorial-sspr-windows.md)
