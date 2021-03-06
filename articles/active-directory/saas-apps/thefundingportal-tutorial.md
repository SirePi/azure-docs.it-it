---
title: 'Esercitazione: Integrazione di Azure Active Directory con The Funding Portal | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e The Funding Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: d45712a94a6a2eb18088c99d7175b73b29e62ce3
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428401"
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Esercitazione: Integrazione di Azure Active Directory con The Funding Portal

Questa esercitazione descrive come integrare The Funding Portal con Azure Active Directory (Azure AD).

L'integrazione di The Funding Portal con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a The Funding Portal
- È possibile abilitare gli utenti per l'accesso automatico a The Funding Portal (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con The Funding Portal, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di The Funding Portal abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di The Funding Portal dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-the-funding-portal-from-the-gallery"></a>Aggiunta di The Funding Portal dalla raccolta
Per configurare l'integrazione di The Funding Portal in Azure AD, è necessario aggiungere The Funding Portal dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere The Funding Portal dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **The Funding Portal**.

    ![Creazione di un utente test di Azure AD](./media/thefundingportal-tutorial/tutorial_thefundingportal_search.png)

1. Nel riquadro dei risultati selezionare **The Funding Portal** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con The Funding Portal con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di The Funding Portal che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in The Funding Portal.

Per stabilire la relazione di collegamento, in The Funding Portal assegnare il valore di **nome utente** in Azure AD come valore di **Username**.

Per configurare e testare l'accesso Single Sign-On di Azure AD con The Funding Portal, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di The Funding Portal](#creating-the-funding-portal-test-user)**: per avere una controparte di Britta Simon in The Funding Portal collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione The Funding Portal.

**Per configurare l'accesso Single Sign-On di Azure AD con The Funding Portal, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **The Funding Portal** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

1. Nella sezione **URL e dominio The Funding Portal** seguire questa procedura:

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<subdomain>.regenteducation.net/`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<subdomain>.regenteducation.net`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto client di The Funding Portal](mailto:info@regenteducation.com). 

1. L'applicazione The Funding Portal prevede che le asserzioni SAML contengano un attributo denominato "externalId1". Il valore di "externalId1" deve essere un ID studente riconosciuto. Configurare l'attestazione "externalId1" per questa applicazione. È possibile gestire i valori di questi attributi dalla scheda **Attributi utente** dell'applicazione. La schermata seguente illustra un esempio relativo a questa operazione.

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

1. Nella sezione **Attributi utente** della finestra di dialogo **Single Sign-On** configurare l'attributo del token SAML come illustrato nell'immagine e seguire questa procedura:

    | Nome attributo | Valore attributo |
    | ------------------- | ---------------- |
    | externalId1 | user.extensionattribute1 |

    a. Fare clic su **Aggiungi attributo** per aprire la finestra di dialogo **Aggiungi attributo**.

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_attribute_05.png)

    b. Nella casella di testo **Nome** digitare il nome dell'attributo indicato per la riga.

    c. Nell'elenco **Valore attributo** selezionare l'attributo utente da usare per l'implementazione. Se il valore dell'ID studente è stato archiviato nell'attributo ExtensionAttribute1, ad esempio, selezionare user.extensionattribute1.
    
    d. Fare clic su **OK**.
 
1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_general_400.png)

1. Per configurare l'accesso Single Sign-On sul lato **The Funding Portal**, è necessario inviare il file **XML metadati** scaricato al [team di supporto di The Funding Portal](mailto:info@regenteducation.com). La configurazione viene eseguita in modo che la connessione SSO SAML sia impostata correttamente su entrambi i lati.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/thefundingportal-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/thefundingportal-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/thefundingportal-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/thefundingportal-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-the-funding-portal-test-user"></a>Creazione di un utente di test di The Funding Portal

In questa sezione viene creato un utente chiamato Britta Simon in The Funding Portal. Collaborare con il [team di supporto di The Funding Portal](mailto:info@regenteducation.com) per aggiungere l'utente di test e abilitare SSO.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione si abilita Britta Simon all'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a The Funding Portal.

![Assegna utente][200] 

**Per assegnare Britta Simon a The Funding Portal, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **The Funding Portal**.

    ![Configure Single Sign-On](./media/thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro The Funding Portal nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione The Funding Portal.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/thefundingportal-tutorial/tutorial_general_203.png

