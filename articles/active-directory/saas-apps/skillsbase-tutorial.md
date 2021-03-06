---
title: 'Esercitazione: Integrazione di Azure Active Directory con Skills Base | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Skills Base.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 237d90c4-8243-4f80-a305-b5ad9204159e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2018
ms.author: jeedes
ms.openlocfilehash: e11ba8ca9c4ad17b2ade909bb474ad2d1fcf4410
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205375"
---
# <a name="tutorial-azure-active-directory-integration-with-skills-base"></a>Esercitazione: Integrazione di Azure Active Directory con Skills Base

Questa esercitazione descrive come integrare Skills Base con Azure Active Directory (Azure AD).

L'integrazione di Skills Base con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Skills Base.
- È possibile abilitare gli utenti per l'accesso automatico a Skills Base (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Skills Base, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Skills Base abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Skills Base dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-skills-base-from-the-gallery"></a>Aggiunta di Skills Base dalla raccolta
Per configurare l'integrazione di Skills Base in Azure AD, è necessario aggiungere Skills Base dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Skills Base dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

2. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

4. Nella casella di ricerca digitare **Skills Base** selezionare **Skills Base** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Skills Base nell'elenco risultati](./media/skillsbase-tutorial/tutorial_skillsbase_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Skills Base con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Skills Base che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Skills Base.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Skills Base, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
2. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creare un utente di test di Skills Base](#create-a-skills-base-test-user)**: per avere una controparte di Britta Simon in Skills Base collegata alla rappresentazione dell'utente in Azure AD.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Skills Base.

**Per configurare Single Sign-On di Azure AD con Skills Base, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Skills Base** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

2. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/skillsbase-tutorial/tutorial_skillsbase_samlbase.png)

3. Nella sezione **URL e dominio Skills Base** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di Skills Base](./media/skillsbase-tutorial/tutorial_skillsbase_url.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://app.skills-base.com/o/<customer-unique-key>`

    > [!NOTE] 
    > È possibile ottenere l'URL di accesso dall'applicazione Skills Base. Eseguire l'accesso come amministratore e passare ad Admin -> Settings -> Instance details -> Shortcut link (Amministratore -> Impostazioni -> Dettagli istanza -> Collegamento tasto di scelta rapida). Copiare l'URL di accesso e incollarlo nella casella di testo indicata sopra.

4. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/skillsbase-tutorial/tutorial_skillsbase_certificate.png) 

5. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/skillsbase-tutorial/tutorial_general_400.png)

6. In un'altra finestra del Web browser accedere a Skills Base come amministratore della sicurezza.

7. Dal lato sinistro del menu, sotto **ADMIN** fare clic su **Authentication** (Autenticazione).

    ![L'amministratore](./media/skillsbase-tutorial/tutorial_skillsbase_auth.png)

8. Nella pagina **Authentication** (Autenticazione), selezionare Single Sign-On come **SAML 2**.

    ![Single](./media/skillsbase-tutorial/tutorial_skillsbase_single.png)

9. Nella pagina **Authentication** (Autenticazione) seguire questa procedura:

    ![Single](./media/skillsbase-tutorial/tutorial_skillsbase_save.png)

    a. Fare clic su **Update IdP metadata** (Aggiorna metadati IdP) accanto all'opzione **Status** (Stato) e incollare il contenuto dell'XML metadati scaricato dal portale di Azure nella casella di testo specificata.

    > [!Note]
    > È inoltre possibile convalidare i metadati idp tramite lo strumento **Metadata validator** (Validator metadati) come evidenziato nella schermata precedente.

    b. Fare clic su **Save**.
    
### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/skillsbase-tutorial/create_aaduser_01.png)

2. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/skillsbase-tutorial/create_aaduser_02.png)

3. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/skillsbase-tutorial/create_aaduser_03.png)

4. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/skillsbase-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-a-skills-base-test-user"></a>Creare un utente di test di Skills Base

L'obiettivo di questa sezione consiste nel creare un utente chiamato Britta Simon in Skills Base. Skills Base supporta il provisioning just-in-time, che è abilitato per impostazione predefinita. Non è necessario alcun intervento dell'utente in questa sezione. Durante il tentativo di accesso a Skills Base viene creato un nuovo utente, se questo non esiste già.

>[!Note]
>Se è necessario creare un utente manualmente, seguire le istruzioni riportate in [questa pagina](http://wiki.skills-base.net/index.php?title=Adding_people_and_enabling_them_to_log_in).

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Skills Base.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a Skills Base, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

2. Nell'elenco di applicazioni selezionare **Skills Base**.

    ![Collegamento di Skills Base nell'elenco delle applicazioni](./media/skillsbase-tutorial/tutorial_skillsbase_app.png)  

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

4. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

6. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

7. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Skills Base nel pannello di accesso, verrà eseguito automaticamente l'accesso all'applicazione Skills Base.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillsbase-tutorial/tutorial_general_01.png
[2]: ./media/skillsbase-tutorial/tutorial_general_02.png
[3]: ./media/skillsbase-tutorial/tutorial_general_03.png
[4]: ./media/skillsbase-tutorial/tutorial_general_04.png

[100]: ./media/skillsbase-tutorial/tutorial_general_100.png

[200]: ./media/skillsbase-tutorial/tutorial_general_200.png
[201]: ./media/skillsbase-tutorial/tutorial_general_201.png
[202]: ./media/skillsbase-tutorial/tutorial_general_202.png
[203]: ./media/skillsbase-tutorial/tutorial_general_203.png

