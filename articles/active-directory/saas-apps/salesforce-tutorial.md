---
title: 'Esercitazione: Integrazione di Azure Active Directory con Salesforce | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.author: jeedes
ms.openlocfilehash: 2f87c4a15ac21241b3304d1fdf0a5bd0ae715615
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430206"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Esercitazione: Integrazione di Azure Active Directory con Salesforce

Questa esercitazione descrive come integrare Salesforce con Azure Active Directory (Azure AD).

L'integrazione di Salesforce con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD quali utenti hanno accesso a Salesforce.
- È possibile abilitare gli utenti per l'accesso automatico a Salesforce (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Salesforce, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Salesforce abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Salesforce dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-salesforce-from-the-gallery"></a>Aggiunta di Salesforce dalla raccolta
Per configurare l'integrazione di Salesforce in Azure AD, è necessario aggiungere Salesforce dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Salesforce dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]

1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **Salesforce**, selezionare **Salesforce** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Salesforce nell'elenco risultati](./media/salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Salesforce in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Salesforce che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Salesforce.

Per stabilire la relazione di collegamento, in Salesforce assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Salesforce, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente test di Salesforce](#create-a-salesforce-test-user)**: per avere una controparte di Britta Simon in Salesforce collegata alla rappresentazione in Azure AD dell'utente.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Salesforce.

**Per configurare Single Sign-On di Azure AD con Salesforce, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Salesforce** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.

    ![Finestra di dialogo Single Sign-On](./media/salesforce-tutorial/tutorial_salesforce_samlbase.png)

1. Nella sezione **URL e dominio Salesforce** seguire questa procedura:

    ![Informazioni sull'accesso Single Sign-On per URL e dominio di Salesforce](./media/salesforce-tutorial/tutorial_salesforce_url.png)

    a. Nella casella di testo **URL di accesso** digitare il valore usando il modello seguente:

    Account aziendale: `https://<subdomain>.my.salesforce.com`

    Account sviluppatore: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. Nella casella di testo **Identificatore** digitare il valore adottando il modello seguente:

    Account aziendale: `https://<subdomain>.my.salesforce.com`

    Account sviluppatore: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Salesforce](https://help.salesforce.com/support).

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato** e quindi salvare il file del certificato nel computer.

    ![Collegamento di download del certificato](./media/salesforce-tutorial/tutorial_salesforce_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/salesforce-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di Salesforce** fare clic su **Configura Salesforce** per aprire la finestra **Configura accesso**. Copiare l'**ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido.**

    ![Configurazione di Salesforce](./media/salesforce-tutorial/tutorial_salesforce_configure.png) 

1. Aprire una nuova scheda del browser e accedere all'account di amministratore di Salesforce.

1. Fare clic su **Setup** (Configura) sotto l'**icona Settings** (Impostazioni) nell'angolo superiore destro della pagina.

    ![Configure Single Sign-On](./media/salesforce-tutorial/configure1.png)

1. Scorrere verso il basso fino alla voce **SETTINGS** (Impostazioni) nel riquadro di spostamento e fare clic su **Identity** (Identità) per espandere la sezione corrispondente. Fare quindi clic su **Single Sign-On Settings**.

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-admin-sso.png)

1. Nella pagina **Single Sign-On Settings** fare clic su **Edit**.

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-admin-sso-edit.png)
    
    > [!NOTE]
    > Se non si è in grado di abilitare le impostazioni dell'accesso Single Sign-On per l'account Salesforce, potrebbe essere necessario contattare il [team di supporto clienti di Salesforce](https://help.salesforce.com/support). 

1. Selezionare **SAML Enabled**, quindi fare clic su **Save**.

      ![Configure Single Sign-On](./media/salesforce-tutorial/sf-enable-saml.png)
1. Per configurare le impostazioni dell'accesso Single Sign-On SAML, fare clic su **New**.

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-admin-sso-new.png)

1. Nella pagina **SAML Single Sign-On Setting Edit** effettuare le configurazioni seguenti:

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-saml-config.png)

    a. Nel campo **Name** specificare un nome descrittivo per la configurazione. Se si specifica un valore per **Name**, verrà popolata automaticamente la casella di testo **API Name**.

    b. Nel campo **Issuer** (Autorità emittente) incollare il valore dell'**ID di entità SAML** copiato dal portale di Azure.

    c. Nella casella di testo **Entity Id**immettere il nome di dominio di Salesforce nel formato seguente:

      * Account aziendale: `https://<subdomain>.my.salesforce.com`
      * Account sviluppatore: `https://<subdomain>-dev-ed.my.salesforce.com`

    d. Per caricare **Identity Provider Certificate** (Certificato provider di identità) fare clic su **Choose file** (Scegli file) per esplorare e selezionare il file di certificato scaricato dal portale di Azure.

    e. Nel campo **SAML Identity Type** (Tipo di identità SAML) scegliere una delle opzioni seguenti:

      * Selezionare **Assertion contains the User's Salesforce username** (L'asserzione contiene il nome utente Salesforce dell'utente) se il nome utente Salesforce dell'utente viene passato nell'asserzione SAML

      * Selezionare **Assertion contains the Federation ID from the User object** (L'asserzione contiene l'ID federazione dell'oggetto User) se l'ID federazione dell'oggetto User viene passato nell'asserzione SAML

      * Selezionare **Assertion contains the Use ID from the User object** (L'asserzione contiene l'ID uso dell'oggetto User) se l'ID uso dell'oggetto User viene passato nell'asserzione SAML

    f. In **SAML Identity Location**(Percorso identità SAML) selezionare **Identity is in the NameIdentifier element of the Subject statement** (L'identità è nell'elemento NameIdentifier dell'istruzione Subject).

    g. Per **Service Provider Initiated Request Binding** selezionare **HTTP Redirect**.

    h. Nella casella di testo **Identity Provider Login URL** (URL di accesso provider di identità) incollare il valore dell'**URL del servizio Single Sign-On**  copiato dal portale di Azure.

    i. Fare infine clic su **Save** per applicare le impostazioni di SAML Single Sign-On.

1. Nel pannello di navigazione sinistro in Salesforce fare clic su **Company Settings** (Impostazioni aziendali) per espandere la sezione correlata e quindi fare clic su **My Domain** (Dominio personale).

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-my-domain.png)

1. Scorrere verso il basso fino alla sezione **Authentication Configuration**, quindi fare clic su **Edit**.

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-edit-auth-config.png)

1. Nella sezione **Authentication Configuration** (Autenticazione configurazione) selezionare **AzureSSO** come **Authentication Service** (Servizio di autenticazione) della configurazione SAML SSO e quindi fare clic su  **Save** (Salva).

    ![Configure Single Sign-On](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Se vengono selezionati più servizi di autenticazione, agli utenti viene richiesta la selezione del servizio di autenticazione da usare per l'accesso quando si tenta di avviare l'accesso Single Sign-On all'ambiente di Salesforce. Se non si vuole visualizzare la richiesta, è consigliabile **lasciare deselezionati tutti gli altri servizi di autenticazione**.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/salesforce-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/salesforce-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/salesforce-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/salesforce-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).

### <a name="create-a-salesforce-test-user"></a>Creare un utente test di Salesforce

In questa sezione si crea un utente di nome Britta Simon in Salesforce. Salesforce supporta il provisioning JIT (Just-In-Time) che è abilitato per impostazione predefinita. Non è necessario alcun intervento dell'utente in questa sezione. Se un utente non esiste in Salesforce, ne viene creato uno nuovo quando si tenta di accedere a Salesforce. Salesforce supporta anche il provisioning utenti automatico; [qui](salesforce-provisioning-tutorial.md) è possibile trovare altre informazioni su come configurare il provisioning utenti automatico.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a Salesforce.

![Assegnare il ruolo utente][200]

**Per assegnare Britta Simon a Salesforce, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201]

1. Nell'elenco di applicazioni selezionare **Salesforce**.

    ![Collegamento Salesforce nell'elenco delle applicazioni](./media/salesforce-tutorial/tutorial_salesforce_app.png)

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Salesforce nel pannello di accesso, verrà eseguito automaticamente l'accesso all'applicazione Salesforce.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)
* [Configura provisioning utenti](salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-tutorial/tutorial_general_203.png
