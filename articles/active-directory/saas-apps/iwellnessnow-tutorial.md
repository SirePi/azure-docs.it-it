---
title: 'Esercitazione: Integrazione di Azure Active Directory con iWellnessNow | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e iWellnessNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24ffc841-7a77-481c-9cc4-6f8bda58fe66
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: c260b32dc6f659ca4cc1b4c3f59859f75ba999d0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447374"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Esercitazione: Integrazione di Azure Active Directory con iWellnessNow

Questa esercitazione descrive come integrare iWellnessNow con Azure Active Directory (Azure AD).

L'integrazione di iWellnessNow con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a iWellnessNow.
- È possibile abilitare gli utenti per l'accesso automatico a iWellnessNow (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con iWellnessNow sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di iWellnessNow abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di iWellnessNow dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-iwellnessnow-from-the-gallery"></a>Aggiunta di iWellnessNow dalla raccolta
Per configurare l'integrazione di iWellnessNow in Azure AD, è necessario aggiungere iWellnessNow dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere iWellnessNow dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **iWellnessNow**, selezionare **iWellnessNow** dal pannello dei risultati quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![iWellnessNow nell'elenco risultati](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con iWellnessNow in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di iWellnessNow che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in iWellnessNow.

Per configurare e testare l'accesso Single Sign-On di Azure AD con iWellnessNow, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test di iWellnessNow](#create-an-iwellnessnow-test-user)**: per avere una controparte di Britta Simon in iWellnessNow collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione iWellnessNow.

**Per configurare l'accesso Single Sign-On di Azure AD con iWellnessNow, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **iWellnessNow** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

1. Per configurare l'applicazione in modalità avviata da **IDP** se si dispone del **file di metadati del provider di servizi**, nella sezione **URL e dominio iWellnessNow** seguire questa procedura:

    ![Upload di URL e dominio per l'accesso Single Sign-On di iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

    a. Fare clic su **Carica il file di metadati**.

    ![Uploadconfig di URL e dominio per l'accesso Single Sign-On di iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    b. Fare clic su **logo cartella** per selezionare il file di metadati e fare quindi clic su **Upload**.
    
    c. Dopo aver completato il caricamento dei **file di metadati del provider di servizi**, i valori dell'**identificatore** e dell'**URL di risposta** popolano automaticamente la casella di testo della sezione **gruppo di dominio e gli URL di iWellnessNow**, come illustrato di seguito:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

1. Per configurare l'applicazione in modalità avviata da **IDP** se non si dispone del **file di metadati del provider di servizi**, seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

    a. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `http://<CustomerName>.iwellnessnow.com`

    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: `https://<CustomerName>.iwellnessnow.com/ssologin`

1. Selezionare **Mostra impostazioni URL avanzate** e seguire questa procedura se si vuole configurare l'applicazione in modalità avviata da **SP**:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<CustomerName>.iwellnessnow.com/`
     
    > [!NOTE] 
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore, l'URL di risposta e l'URL di accesso effettivi. Per ottenere questi valori contattare il [team di supporto clienti di iWellnessNow](mailto:info@iwellnessnow.com).

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/iwellnessnow-tutorial/tutorial_general_400.png)
    
1. Per configurare l'accesso Single Sign-On sul lato **iWellnessNow**, è necessario inviare il file **XML metadati** scaricato al [team di supporto di iWellnessNow](mailto:info@iwellnessnow.com). La configurazione viene eseguita in modo che la connessione SSO SAML sia impostata correttamente su entrambi i lati.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/iwellnessnow-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/iwellnessnow-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/iwellnessnow-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/iwellnessnow-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-an-iwellnessnow-test-user"></a>Creare un utente di test di iWellnessNow

In questa sezione viene creato un utente di nome Britta Simon in iWellnessNow. Lavorare con il [team di supporto iWellnessNow](mailto:info@iwellnessnow.com) per aggiungere gli utenti nella piattaforma iWellnessNow. Gli utenti devono essere creati e attivati prima di usare l'accesso Single Sign-On

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a iWellnessNow.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a iWellnessNow, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **iWellnessNow**.

    ![Collegamento di iWellnessNow nell'elenco delle applicazioni](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro iWellnessNow nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione iWellnessNow.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/iwellnessnow-tutorial/tutorial_general_203.png

