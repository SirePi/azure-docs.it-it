---
title: 'Esercitazione: Integrazione di Azure Active Directory con Promapp | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Promapp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: jeedes
ms.openlocfilehash: 6bcd1add3985112fe60aab22f1799e40ad8889b2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421863"
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>Esercitazione: Integrazione di Azure Active Directory con Promapp

Questa esercitazione descrive come integrare Promapp con Azure Active Directory (Azure AD).

L'integrazione di Promapp con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Promapp
- È possibile abilitare gli utenti per l'accesso automatico a Promapp (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Promapp, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Promapp abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Promapp dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-promapp-from-the-gallery"></a>Aggiunta di Promapp dalla raccolta
Per configurare l'integrazione di Promapp in Azure AD, è necessario aggiungere Promapp dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Promapp dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Promapp**.

    ![Creazione di un utente test di Azure AD](./media/promapp-tutorial/tutorial_promapp_search.png)

1. Nel pannello dei risultati selezionare **Promapp** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Promapp in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Promapp che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Promapp.

Per stabilire la relazione di collegamento, in Promapp assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Promapp, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test per Promapp](#creating-a-promapp-test-user)** : per avere una controparte di Britta Simon in Promapp collegata alla relativa rappresentazione in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso all'applicazione Promapp.

**Per configurare l'accesso Single Sign-On di Azure AD con Promapp, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Promapp** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_promapp_samlbase.png)

1. Nella sezione **Promapp Domain and URLs** (URL e dominio Promapp) seguire questa procedura se si vuole configurare l'applicazione in modalità avviata da **IDP**:

    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_promapp_url.png)

    a. Nella casella di testo **Identificatore** digitare un URL usando il criterio seguente:
    | |
    |--|
    | `https://go.promapp.com/TENANTNAME/`|
    | `https://au.promapp.com/TENANTNAME/`|
    | `https://us.promapp.com/TENANTNAME/`|
    | `https://eu.promapp.com/TENANTNAME/`|
    | `https://ca.promapp.com/TENANTNAME/`|
    
    > [!NOTE] 
    > Attualmente l'integrazione di Azure AD con Promapp è stata configurata solo per l'autenticazione avviata del servizio, ad esempio, passare a un URL Promapp avvia il processo di autenticazione. Tuttavia, l'URL di risposta è un campo obbligatorio.
    
    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: `https://DOMAINNAME.promapp.com/azuread/saml/authenticate.aspx`

1. Selezionare **Mostra impostazioni URL avanzate** e seguire questa procedura se si vuole configurare l'applicazione in modalità avviata da **SP**:

    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_promapp_url1.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Promapp](https://www.promapp.com/about-us/contact-us/).

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_promapp_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_general_400.png)

1. Nella sezione **Promapp Configuration** (Configurazione di Promapp) fare clic su **Configure Promapp** (Configura Promapp) per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_promapp_configure.png) 

1. Accedere al sito aziendale di Promapp come amministratore. 

1. Nel menu in alto fare clic su **Admin**. 
   
    ![Single Sign-On di Microsoft Azure AD][12]

1. Fare clic su **Configure**. 
   
    ![Single Sign-On di Microsoft Azure AD][13]

1. Nella finestra di dialogo **Security** (Sicurezza) seguire questa procedura:
   
    ![Single Sign-On di Microsoft Azure AD][14]
    
    a. Incollare l'**URL del servizio Single Sign-On SAML** copiato dal portale di Azure nella casella di testo **SSO-Login URL** (URL di accesso SSO).
    
    b. Per **SSO - Single Sign-on Mode** selezionare **Optional** e quindi fare clic su **Save**.

    > [!NOTE]
    > La modalità **Optional** (Facoltativa) serve solo a scopo di test. Quando si è soddisfatti della configurazione, selezionare la modalità **Required** (Obbligatoria) per assicurarsi che tutti gli utenti effettuino l'autenticazione con Azure AD.

    c. Aprire il certificato scaricato nel Blocco note, copiare il contenuto del certificato senza la prima riga (-----**BEGIN CERTIFICATE**-----) e l'ultima riga (-----**END CERTIFICATE**-----), incollarlo nella casella di testo **SSO-x.509 Certificate** (Certificato SSO-x.509) e quindi fare clic su **Save** (Salva).
        
> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/promapp-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/promapp-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/promapp-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/promapp-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-promapp-test-user"></a>Creazione di un utente test per Promapp

L'applicazione Promapp supporta il provisioning JIT (just-in-time). Questo significa che, se necessario, un account utente viene creato automaticamente durante il tentativo di accedere all'applicazione tramite il pannello di accesso.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Promapp.

![Assegna utente][200] 

**Per assegnare Britta Simon a Promapp, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Promapp**.

    ![Configure Single Sign-On](./media/promapp-tutorial/tutorial_promapp_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

Per testare l'applicazione nella modalità avviata da **SP** è necessario avviare l'autenticazione dal sito Promapp. Questa operazione può essere eseguita facendo clic sul pulsante 'Accesso con Single Sign-on' nella pagina di accesso mentre è abilitata la modalità **Facoltativo**.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/promapp-tutorial/tutorial_general_01.png
[2]: ./media/promapp-tutorial/tutorial_general_02.png
[3]: ./media/promapp-tutorial/tutorial_general_03.png
[4]: ./media/promapp-tutorial/tutorial_general_04.png
[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/promapp-tutorial/tutorial_general_100.png

[200]: ./media/promapp-tutorial/tutorial_general_200.png
[201]: ./media/promapp-tutorial/tutorial_general_201.png
[202]: ./media/promapp-tutorial/tutorial_general_202.png
[203]: ./media/promapp-tutorial/tutorial_general_203.png

