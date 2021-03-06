---
title: 'Esercitazione: Integrazione di Azure Active Directory con Workplace by Facebook | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Workplace by Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: jeedes
ms.openlocfilehash: 1f83dd64c7f6773ddb8956e6ebbc37b8c55aacec
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423872"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Esercitazione: Integrazione di Azure Active Directory con Workplace by Facebook

Questa esercitazione descrive come integrare Workplace by Facebook con Azure Active Directory, ovvero Azure AD.

L'integrazione di Workplace by Facebook con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Workplace by Facebook
- È possibile abilitare gli utenti per l'accesso automatico a Workplace by Facebook (Single Sign-On) con i propri account di Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Workplace by Facebook, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Una sottoscrizione di Workplace by Facebook abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Facebook offre due prodotti, Workplace Standard (gratuito) e Workplace Premium (a pagamento). Qualsiasi tenant di Workplace Premium può configurare l'integrazione SCIM e SSO senza alcun costo né alcuna necessità di una licenza. SSO e SCIM non sono disponibili nelle istanze di Workplace Standard.

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Workplace by Facebook dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Aggiunta di Workplace by Facebook dalla raccolta
Per configurare l'integrazione di Workplace by Facebook in Azure AD, è necessario aggiungere Workplace by Facebook dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Workplace by Facebook dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Workplace by Facebook**.

    ![Creazione di un utente test di Azure AD](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

1. Nel pannello dei risultati selezionare **Workplace by Facebook** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Workplace by Facebook usando un utente di test di nome "Britta Simon".

Affinché l'accesso Single Sign-On funzioni correttamente, Azure AD deve conoscere l'utente controparte di Workplace by Facebook corrispondente a un utente di Azure AD. In altre parole, si deve stabilire una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Workplace by Facebook.

La relazione di collegamento viene stabilita assegnando al valore di **nome utente** in Azure AD lo stesso valore di **Username** (Nome utente) in Workplace by Facebook.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Workplace by Facebook, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Configurazione della frequenza di riautenticazione](#configuring-reauthentication-frequency)**: per configurare Workplace per la richiesta di una verifica SAML.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di Workplace by Facebook](#creating-a-workplace-by-facebook-test-user)**: per avere una controparte di Britta Simon in Workplace by Facebook collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Workplace by Facebook.

**Per configurare l'accesso Single Sign-On di Azure AD con Workplace by Facebook, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Workplace by Facebook** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

1. Nella sezione **Workplace by Facebook Domain and URLs** (URL e dominio Workplace by Facebook) seguire questa procedura:

    ![Configure Single Sign-On](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<instancename>.facebook.com`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per i valori corretti per la community Workplace, vedere la pagina di autenticazione del dashboard aziendale di Workplace. 

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/workplacebyfacebook-tutorial/tutorial_general_400.png)

1. Nella sezione **Workplace by Facebook Configuration** (Configurazione di Workplace by Facebook) fare clic su **Configure Workplace by Facebook** (Configurare Workplace by Facebook) per aprire la finestra **Configurare accesso**. Copiare l'**URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido.**

    ![Configure Single Sign-On](./media/workplacebyfacebook-tutorial/config.png) 

1. In un'altra finestra del Web browser accedere al sito aziendale di Workplace by Facebook come amministratore.
  
   > [!NOTE] 
   > Come parte del processo di autenticazione SAML, Workplace può usare stringhe di query di dimensioni massime di 2,5 kilobyte per passare i parametri ad Azure AD.

1. In **Company Dashboard** (Company Dashboard) passare alla scheda **Authentication** (Autenticazione).

1. In **SAML Authentication** (Autenticazione SAML) selezionare **SSO Only** (Solo SSO) dall'elenco a discesa.

1. Immettere i valori copiati dalla sezione **Workplace by Facebook Configuration** (Configurazione di Workplace by Facebook) del portale di Azure nei campi corrispondenti:

    *   Nella casella di testo **SAML URL** (URL SAML) incollare il valore di **URL servizio Single Sign-On** copiato dal portale di Azure.
    *   Nella **casella di testo SAML Issuer URL** (URL dell'autorità di certificazione SAML) incollare il valore dell'**ID di entità SAML** copiato dal portale di Azure.
    *   In **SAML Logout Redirect (Optional)** (Reindirizzamento disconnessione SAML (facoltativo)) incollare il valore dell'**URL di disconnessione** copiato dal portale di Azure.
    *   Aprire nel Blocco note il **certificato con codifica Base 64** scaricato dal portale di Azure, copiarne il contenuto negli Appunti e incollarlo nella casella di testo **SAML Certificate** (Certificato SAML).

1. Potrebbe essere necessario inserire l'URL del pubblico, l'URL del destinatario e l'URL ACS (servizio consumer di asserzione) elencati nella sezione **SAML Configuration** (Configurazione SAML).

1. Scorrere fino alla fine della sezione e fare clic sul pulsante **Test SSO** (Testa SSO). Si ottiene una finestra popup visualizzata con la pagina di accesso di Azure AD. Immettere le credenziali come di consueto per l'autenticazione. 

    **Risoluzione dei problemi:** assicurarsi che l'indirizzo e-mail restituito da Azure AD sia lo stesso dell'account aziendale con cui si è connessi.

1. Dopo il completamento del test, scorrere fino alla fine della pagina e fare clic sul pulsante **Save** (Salva).

1. Tutti gli utenti che usano Workplace visualizzeranno ora una pagina di accesso di Azure AD per l'autenticazione.

1. **Reindirizzamento disconnessione SAML (facoltativo)** - 

    È possibile scegliere di configurare facoltativamente un URL di disconnessione SAML, che può essere usato per puntare alla pagina di disconnessione di Azure AD. Quando questa impostazione è abilitata e configurata, l'utente non sarà più indirizzato alla pagina di disconnessione di Workplace. Al contrario, l'utente verrà reindirizzato all'URL aggiunto nelle impostazioni di reindirizzamento di disconnessione SAML.

### <a name="configuring-reauthentication-frequency"></a>Configurazione della frequenza di riautenticazione

È possibile configurare Workplace per la richiesta di una verifica SAML ogni giorno, ogni 3 giorni, ogni settimana, ogni 2 settimane, ogni mese o mai.

> [!NOTE] 
>Il valore minimo per la verifica SAML nelle applicazioni per dispositivi mobili è impostato su una settimana.

È anche possibile forzare una reimpostazione SAML per tutti gli utenti che usano il pulsante: Require SAML authentication for all users now (Richiedi ora l'autenticazione SAML a tutti gli utenti).


### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Creazione di un utente test di Workplace by Facebook

In questa sezione si crea un utente di nome Britta Simon in Workplace by Facebook. Workplace by Facebook supporta il provisioning JIT, abilitato per impostazione predefinita.

Non è necessaria alcuna azione dell'utente in questa sezione. Se un utente non esiste in Workplace by Facebook, si crea una nuova istanza quando si tenta di accedere a Workplace by Facebook.

>[!Note]
>Se è necessario creare un utente manualmente, contattare il [team di supporto al cliente di Workplace by Facebook](https://workplace.fb.com/faq/)

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a Workplace by Facebook.

![Assegna utente][200] 

**Per assegnare Britta Simon a Workplace by Facebook, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Workplace by Facebook**.

    ![Configure Single Sign-On](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

Se si desidera testare le impostazioni di Single Sign-On, aprire il pannello di accesso.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)
* [Configura provisioning utenti](workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/workplacebyfacebook-tutorial/tutorial_general_203.png
