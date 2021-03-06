---
title: 'Esercitazione: Integrazione di Azure Active Directory con Coupa | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Coupa.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: jeedes
ms.openlocfilehash: 4bf40f76f5a8f788305b4dc9f91523f53fb59acf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448085"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Esercitazione: Integrazione di Azure Active Directory con Coupa

Questa esercitazione descrive come integrare Coupa con Azure Active Directory (Azure AD).

L'integrazione di Coupa con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Coupa.
- È possibile abilitare gli utenti per l'accesso automatico a Coupa (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Coupa, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione Coupa abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Coupa dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-coupa-from-the-gallery"></a>Aggiunta di Coupa dalla raccolta
Per configurare l'integrazione di Coupa in Azure AD, è necessario aggiungere Coupa dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Coupa dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]

1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **Coupa**, selezionare **Coupa** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Coupa nell'elenco dei risultati](./media/coupa-tutorial/tutorial_coupa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Coupa usando un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Coupa che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Coupa.

Per stabilire la relazione di collegamento, in Coupa assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Coupa, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test di Coupa](#create-a-coupa-test-user)**: per avere una controparte di Britta Simon in Coupa collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Coupa.

**Per configurare l'accesso Single Sign-On di Azure AD con Coupa, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Coupa** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.

    ![Finestra di dialogo Single Sign-On](./media/coupa-tutorial/tutorial_coupa_samlbase.png)

1. Nella sezione **URL e dominio Coupa** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di Coupa](./media/coupa-tutorial/tutorial_coupa_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<companyname>.coupahost.com`

    > [!NOTE]
    > Poiché il valore dell'URL di accesso non è reale, è necessario aggiornare questo valore con l'URL di accesso Sign-On effettivo. Per ottenere questo valore, contattare [il team di supporto client Coupa](https://success.coupa.com/Support/Contact_Us?).

    b. Nella casella di testo **Identificatore** digitare l'URL:

    | Environment  | URL |
    |:-------------|----|
    | Sandbox | `devsso35.coupahost.com`|
    | Produzione | `prdsso40.coupahost.com`|
    | | |

    c. Nella casella di testo **URL di risposta** digitare l'URL:

    | Environment | URL |
    |------------- |----|
    | Sandbox | `https://devsso35.coupahost.com/sp/ACS.saml2`|
    | Produzione | `https://prdsso40.coupahost.com/sp/ACS.saml2`|
    | | |

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/coupa-tutorial/tutorial_coupa_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/coupa-tutorial/tutorial_general_400.png)

1. Accedere al sito aziendale di Coupa come amministratore.

1. Passare a **Setup (Configurazione) \> Security Control (Controllo di sicurezza)**.

   ![Security Controls](./media/coupa-tutorial/ic791900.png "Security Controls")

1. Nella sezione **Accesso mediante le credenziali di Coupa** seguire questa procedura:

    ![Metadati Coupa SP](./media/coupa-tutorial/ic791901.png "Metadati Coupa SP")

    a. Selezionare **Accedere mediante SAML**.

    b. Fare clic su **Sfoglia** per caricare i metadati scaricati dal portale di Azure.

    c. Fare clic su **Save**.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/coupa-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/coupa-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/coupa-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/coupa-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).

### <a name="create-a-coupa-test-user"></a>Creare un utente di test di Coupa

Per consentire agli utenti di Azure AD di accedere a Coupa, è necessario eseguirne il provisioning in Coupa.  

* Nel caso di Coupa, il provisioning è un'attività manuale.

**Per configurare il provisioning utenti, seguire questa procedura:**

1. Accedere al sito aziendale di **Coupa** come amministratore.

1. Nel menu in alto fare clic su **Setup** (Configura) e quindi su **Users** (Utenti).

   ![Utenti](./media/coupa-tutorial/ic791908.png "Utenti")

1. Fare clic su **Create**(Crea).

   ![Creare utenti](./media/coupa-tutorial/ic791909.png "Creare utenti")

1. Nella sezione **Crea utente** seguire questa procedura:

   ![Dettagli utente](./media/coupa-tutorial/ic791910.png "Dettagli utente")

   a. Nelle caselle **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** immettere l'account di accesso, il nome, il cognome, l'ID Single Sign-On e l'indirizzo di posta elettronica di un account Azure Active Directory valido di cui si vuole eseguire il provisioning.

   b. Fare clic su **Create**(Crea).

   >[!NOTE]
   >Il titolare dell'account Azure Active Directory riceve un messaggio di posta elettronica con un collegamento da selezionare per confermare l'account e attivarlo.
   >

>[!NOTE]
>È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Coupa per eseguire il provisioning degli account utente di AAD.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Coupa.

![Assegnare il ruolo utente][200]

**Per assegnare Britta Simon a Coupa, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201]

1. Nell'elenco delle applicazioni selezionare **Coupa**.

    ![Collegamento di Coupa nell'elenco delle applicazioni](./media/coupa-tutorial/tutorial_coupa_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Coupa nel riquadro di accesso, si dovrebbe accedere automaticamente all'applicazione Coupa.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/coupa-tutorial/tutorial_general_01.png
[2]: ./media/coupa-tutorial/tutorial_general_02.png
[3]: ./media/coupa-tutorial/tutorial_general_03.png
[4]: ./media/coupa-tutorial/tutorial_general_04.png

[100]: ./media/coupa-tutorial/tutorial_general_100.png

[200]: ./media/coupa-tutorial/tutorial_general_200.png
[201]: ./media/coupa-tutorial/tutorial_general_201.png
[202]: ./media/coupa-tutorial/tutorial_general_202.png
[203]: ./media/coupa-tutorial/tutorial_general_203.png
