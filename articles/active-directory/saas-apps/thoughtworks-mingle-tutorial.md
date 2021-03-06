---
title: 'Esercitazione: Integrazione di Azure Active Directory con Thoughtworks Mingle | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Thoughtworks Mingle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a685b5702aa9f74f3e0abf2a06774a30ac0d996f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436992"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Esercitazione: Integrazione di Azure Active Directory con Thoughtworks Mingle

Questa esercitazione descrive come integrare Thoughtworks Mingle con Azure Active Directory (Azure AD).

L'integrazione di Thoughtworks Mingle con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Thoughtworks Mingle
- È possibile abilitare gli utenti per l'accesso automatico a Thoughtworks Mingle (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Thoughtworks Mingle, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Thoughtworks Mingle abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Thoughtworks Mingle dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Aggiunta di Thoughtworks Mingle dalla raccolta
Per configurare l'integrazione di Thoughtworks Mingle in Azure AD, è necessario aggiungere Thoughtworks Mingle dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Thoughtworks Mingle dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **Thoughtworks Mingle**, selezionare **Thoughtworks Mingle** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Thoughtworks Mingle nell'elenco dei risultati](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Thoughtworks Mingle usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Thoughtworks Mingle che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Thoughtworks Mingle.

Per stabilire la relazione di collegamento, in Thoughtworks Mingle assegnare il valore di **nome utente** di Azure AD come valore dell'attributo **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Thoughtworks Mingle, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test per Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**: per avere una controparte di Britta Simon in Thoughtworks Mingle collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)**: per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Thoughtworks Mingle.

**Per configurare l'accesso Single Sign-On di Azure AD con Thoughtworks Mingle, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Thoughtworks Mingle** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

1. Nella sezione **URL e dominio Thoughtworks Mingle** seguire questa procedura:

    ![Informazioni sull'accesso Single Sign-On per URL e dominio di Thoughtworks Mingle](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > Poiché non è reale, è necessario aggiornare questo valore con l'URL di accesso effettivo. Per ottenere il valore contattare il [team di supporto clienti di Thoughtworks Mingle](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support). 
 
1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

1. Accedere al sito aziendale di **Thoughtworks Mingle** come amministratore.

1. Fare clic sulla scheda **Admin**, quindi fare clic su **Config SSO**.
   
    ![Scheda Amministrazione](./media/thoughtworks-mingle-tutorial/ic785157.png "Config SSO")

1. Nella sezione **Config SSO** , eseguire la procedura seguente:
   
    ![Configurazione dell'accesso SSO](./media/thoughtworks-mingle-tutorial/ic785158.png "Configurazione dell'accesso SSO")
    
    a. Per caricare il file di metadati, fare clic su **Scegli file**. 

    b. Fare clic su **Salva modifiche**.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Pulsante Aggiungi](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Finestra di dialogo Utente](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Creare un utente di test di Thoughtworks Mingle

Per consentire agli utenti di Azure AD di accedere, è necessario eseguirne il privisioning nell'applicazione Thoughtworks Mingle utilizzando i relativi nomi utente di Azure Active Directory. Nel caso di Thoughtworks Mingle, il provisioning è un'attività manuale.

**Per configurare il provisioning utenti, seguire questa procedura:**

1. Accedere al sito aziendale di Thoughtworks Mingle come amministratore.

1. Fare clic su **Profilo**.
   
    ![Primo progetto](./media/thoughtworks-mingle-tutorial/ic785160.png "Primo progetto")

1. Fare clic sulla scheda **Admin**, quindi fare clic su **Utenti**.
   
    ![Utenti](./media/thoughtworks-mingle-tutorial/ic785161.png "Utenti")

1. Fare clic su **Nuovo utente**.
   
    ![Nuovo utente](./media/thoughtworks-mingle-tutorial/ic785162.png "Nuovo utente")

1. Nella pagina **Nuovo utente** eseguire la procedura seguente:
   
    ![Finestra di dialogo Nuovo utente](./media/thoughtworks-mingle-tutorial/ic785163.png "Nuovo utente")  
 
    a. Digitare nelle caselle di testo correlate **Nome accesso**, **Nome visualizzato**, **Scegli password**, **Conferma password** le informazioni relative a un account Azure AD valido di cui si desidera eseguire il provisioning. 

    b. In **Tipo di utente** selezionare **Base**.

    c. Fare clic su **Crea questo profilo**.

>[!NOTE]
>È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Thoughtworks Mingle per eseguire il provisioning degli account utente di AAD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a Thoughtworks Mingle.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a Thoughtworks Mingle, eseguire la procedura seguente:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **Thoughtworks Mingle**.

    ![Il collegamento Thoughtworks Mingle nell'elenco delle applicazioni](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Thoughtworks Mingle nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Thoughtworks Mingle.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

