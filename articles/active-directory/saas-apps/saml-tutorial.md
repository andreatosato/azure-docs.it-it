---
title: 'Esercitazione: Integrazione di Azure Active Directory con SAML 1.1 Token enabled LOB App | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e SAML 1.1 Token enabled LOB App.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ced1d88d-0e48-40d5-9aea-ef991cd9d270
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: edabc09f820093d088ec0b8ed1222fb26c800bee
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426429"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Esercitazione: Integrazione di Azure Active Directory con SAML 1.1 Token enabled LOB App

Questa esercitazione descrive come integrare SAML 1.1 Token enabled LOB App con Azure Active Directory (Azure AD).

L'integrazione di SAML 1.1 Token enabled LOB App con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a SAML 1.1 Token enabled LOB App.
- È possibile abilitare gli utenti per l'accesso automatico a SAML 1.1 Token enabled LOB App (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con SAML 1.1 Token enabled LOB App, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Una sottoscrizione di SAML 1.1 Token enabled LOB App abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di SAML 1.1 Token enabled LOB App dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Aggiunta di SAML 1.1 Token enabled LOB App dalla raccolta
Per configurare l'integrazione di SAML 1.1 Token enabled LOB App in Azure AD, è necessario aggiungere SAML 1.1 Token enabled LOB App dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere SAML 1.1 Token enabled LOB App dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **SAML 1.1 Token enabled LOB App**, selezionare **SAML 1.1 Token enabled LOB App** dal pannello risultati, quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![SAML 1.1 Token enabled LOB App nell'elenco risultati](./media/saml-tutorial/tutorial_saml_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con SAML 1.1 Token enabled LOB App in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di SAML 1.1 Token enabled LOB App che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in SAML 1.1 Token enabled LOB App.

Per configurare e testare l'accesso Single Sign-On di Azure AD con SAML 1.1 Token enabled LOB App, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test di SAML 1.1 Token enabled LOB App](#create-a-saml-11-token-enabled-lob-app-test-user)**: per avere una controparte di Britta Simon in SAML 1.1 Token enabled LOB App collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso all'applicazione SAML 1.1 Token enabled LOB App.

**Per configurare Single Sign-On di Azure AD con SAML 1.1 Token enabled LOB App, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **SAML 1.1 Token enabled LOB App** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/saml-tutorial/tutorial_saml_samlbase.png)

1. Nella sezione **URL e dominio SAML 1.1 Token enabled LOB App** seguire questa procedura:

    ![Informazioni sull'accesso Single Sign-On per gli URL e il dominio di SAML 1.1 Token enabled LOB App](./media/saml-tutorial/tutorial_saml_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://your-app-url`

    b. Nella casella di testo **Identificatore (ID entità)** digitare un URL usando il criterio seguente: `https://your-app-url`
     
    > [!NOTE] 
    > Poiché questi non sono i valori reali, Sostituire questi valori con gli URL specifici dell'applicazione.  

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Collegamento di download del certificato](./media/saml-tutorial/tutorial_saml_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/saml-tutorial/tutorial_general_400.png)
    
1. Nella sezione **Configurazione di SAML 1.1 Token enabled LOB App** fare clic su **SAML 1.1 Token enabled LOB App** per aprire la finestra **Configura accesso**. Copiare l'**URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido.**

    ![Configurazione di SAML 1.1 Token enabled LOB App](./media/saml-tutorial/tutorial_saml_configure.png) 

1. Per configurare l'accesso Single Sign-On sul lato **SAML 1.1 Token enabled LOB App** è necessario inviare il **certificato(Base64) scaricato, l'URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** al team di supporto dell'applicazione. La configurazione viene eseguita in modo che la connessione SSO SAML sia impostata correttamente su entrambi i lati.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/saml-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/saml-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/saml-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/saml-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-a-saml-11-token-enabled-lob-app-test-user"></a>Creare un utente di test di SAML 1.1 Token enabled LOB App

In questa sezione viene creato un utente di nome Britta Simon in SAML 1.1 Token enabled LOB App. Usare il team di supporto dell'applicazione per creare l'utente sul lato applicazione. Gli utenti devono essere creati e attivati prima di usare l'accesso Single Sign-On.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a SAML 1.1 Token enabled LOB App.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a SAML 1.1 Token enabled LOB App, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **SAML 1.1 Token enabled LOB App**.

    ![Collegamento SAML 1.1 Token enabled LOB App nell'elenco delle applicazioni](./media/saml-tutorial/tutorial_saml_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro SAML 1.1 Token enabled LOB App nel pannello di accesso, verrà eseguito automaticamente l'accesso all'applicazione SAML 1.1 Token enabled LOB App.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/saml-tutorial/tutorial_general_01.png
[2]: ./media/saml-tutorial/tutorial_general_02.png
[3]: ./media/saml-tutorial/tutorial_general_03.png
[4]: ./media/saml-tutorial/tutorial_general_04.png

[100]: ./media/saml-tutorial/tutorial_general_100.png

[200]: ./media/saml-tutorial/tutorial_general_200.png
[201]: ./media/saml-tutorial/tutorial_general_201.png
[202]: ./media/saml-tutorial/tutorial_general_202.png
[203]: ./media/saml-tutorial/tutorial_general_203.png
