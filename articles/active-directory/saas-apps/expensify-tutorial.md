---
title: 'Esercitazione: Integrazione di Azure Active Directory con Expensify | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory ed Expensify.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/2/2017
ms.author: jeedes
ms.openlocfilehash: d53877dbcc25edad14714633bfa11a0c3cbbf76e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433259"
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a>Esercitazione: Integrazione di Azure Active Directory con Expensify

Questa esercitazione descrive come integrare Expensify con Azure Active Directory (Azure AD).

L'integrazione di Expensify con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Expensify.
- È possibile abilitare gli utenti per l'accesso automatico a Expensify (Single Sign-On) con l'account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Expensify, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Expensify abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Expensify dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-expensify-from-the-gallery"></a>Aggiunta di Expensify dalla raccolta
Per configurare l'integrazione di Expensify in Azure AD, è necessario aggiungere Expensify dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Expensify dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **Expensify**, selezionare **Expensify** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Expensify nell'elenco risultati](./media/expensify-tutorial/tutorial_expensify_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Expensify con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Expensify che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Expensify.

Per stabilire la relazione di collegamento, in Expensify assegnare il valore di **nome utente** di Azure AD come valore dell'attributo **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Expensify, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente test di Expensify](#create-an-expensify-test-user)**: per avere una controparte di Britta Simon in Expensify collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Expensify.

**Per configurare l'accesso Single Sign-On di Azure AD con Expensify, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Expensify** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/expensify-tutorial/tutorial_expensify_samlbase.png)

1. Nella sezione **URL e dominio Expensify** seguire questa procedura:

    ![Informazioni su URL e dominio per Single Sign-On di Expensify](./media/expensify-tutorial/tutorial_expensify_url.png)

    a. Nella casella di testo **URL di accesso** digitare un URL come indicato di seguito: `https://www.expensify.com/authentication/saml/login`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://www.<companyname>.expensify.com`

    > [!NOTE] 
    > Sostituire la sezione `<companyname>` dell'identificatore URL con il dominio della società. Vedi l'esempio di `https://contoso.expensify.com` precedente. Per ottenere questo valore contattare il [team di supporto clienti di Expensify](mailto:help@expensify.com).

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/expensify-tutorial/tutorial_expensify_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/expensify-tutorial/tutorial_general_400.png)

1. Per abilitare SSO in Expensify è necessario avere abilitato **Domain Control** (Controllo di dominio) nell'applicazione. Per abilitare il controllo di dominio nell'applicazione seguire [questa procedura](http://help.expensify.com/domain-control). Per ulteriore supporto, contattare il [team di supporto clienti di Expensify](mailto:help@expensify.com). Dopo aver abilitato il controllo di dominio seguire questa procedura:
   
    ![Configure Single Sign-On](./media/expensify-tutorial/tutorial_expensify_51.png)
    
    a. Accedere all'applicazione Expensify.
    
    b. Nel barra degli strumenti in alto fare clic su **Admin**.
    
    c. Nel pannello sinistro fare clic su **Domain** (Dominio).
    
    d. Fare clic sul nome di dominio verificato.
    
    e. Nel pannello sinistro fare clic su **SAML**, quindi selezionare **Enabled** (Abilitato).
    
    f. Aprire i metadati della federazione scaricati da Azure AD nel Blocco note, copiare il contenuto e incollarlo nella casella di testo **Identity Provider Metadata** (Metadati del provider di identità).

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/expensify-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/expensify-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/expensify-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/expensify-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-an-expensify-test-user"></a>Creare un utente test di Expensify

In questa sezione viene creato un utente chiamato Britta Simon in Expensify. Collaborare con il [team di supporto clienti di Expensify](mailto:help@expensify.com) per aggiungere gli utenti nella piattaforma Expensify.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Expensify.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a Expensify, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Expensify**.

    ![Collegamento di Expensify nell'elenco delle applicazioni](./media/expensify-tutorial/tutorial_expensify_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Expensify nel pannello di accesso, verrà eseguito automaticamente l'accesso all'applicazione Expensify.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/expensify-tutorial/tutorial_general_01.png
[2]: ./media/expensify-tutorial/tutorial_general_02.png
[3]: ./media/expensify-tutorial/tutorial_general_03.png
[4]: ./media/expensify-tutorial/tutorial_general_04.png

[100]: ./media/expensify-tutorial/tutorial_general_100.png

[200]: ./media/expensify-tutorial/tutorial_general_200.png
[201]: ./media/expensify-tutorial/tutorial_general_201.png
[202]: ./media/expensify-tutorial/tutorial_general_202.png
[203]: ./media/expensify-tutorial/tutorial_general_203.png

