---
title: 'Esercitazione: integrazione di Azure Active Directory con LCVista | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e LCVista.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 1ec1783e6c9caabfbc5e03849b6d4c04b1f33d23
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448156"
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a>Esercitazione: integrazione di Azure Active Directory con LCVista

Questa esercitazione descrive come integrare LCVista con Azure Active Directory (Azure AD).

L'integrazione di LCVista con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a LCVista
- È possibile abilitare gli utenti per l'accesso automatico ad LCVista (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con LCVista, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di LCVista abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di LCVista dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-lcvista-from-the-gallery"></a>Aggiunta di LCVista dalla raccolta
Per configurare l'integrazione di LCVista in Azure AD, è necessario aggiungere LCVista dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere LCVista dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **LCVista**.

    ![Creazione di un utente test di Azure AD](./media/lcvista-tutorial/tutorial_lcvista_search.png)

1. Nel pannello dei risultati selezionare **LCVista** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con LCVista in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di LCVista che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in LCVista.

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Nome utente** in LCVista.

Per configurare e testare l'accesso Single Sign-On di Azure AD con LCVista, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test di LCVista](#creating-a-lcvista-test-user)**: per avere una controparte di Britta Simon in LCVista collegata alla relativa rappresentazione in Azure AD dell'utente.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione LCVista.

**Per configurare Single Sign-On di Azure AD con LCVista, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **LCVista** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_lcvista_samlbase.png)

1. Nella sezione **URL e dominio LCVista** seguire questa procedura:

    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_lcvista_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<subdomain>.lcvista.com/rainier/login`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<subdomain>.lcvista.com` 
     
    > [!NOTE] 
    > Questi non sono i valori reali. Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di LCVista](https://lcvista.com/contact). 

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_lcvista_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_general_400.png)
    
1. Nella sezione **Configurazione di LCVista** fare clic su **Configura LCVista** per aprire la finestra **Configura accesso**. Copiare l'**ID di entità SAML** e l'**URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido**.

    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_lcvista_configure.png) 

1.  Accedere all'applicazione LCVista come amministratore.

1. Nella sezione **Configurazione SAML** selezionare **Enable SAML login** (Abilita accesso SAML) e immettere i dettagli, come indicato nell'immagine seguente. 

    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_lcvista_config.png)

    a. Incollare l'**URL autorità di certificazione** che è stata copiata da Azure AD nella sezione **ID entità**. 

    b. Incollare l'**URL servizio Single Sign-On** che è stata copiata da Azure AD nella sezione **URL**.

    c. Da Metadati (XML) che è stato scaricato dal portale di Azure, copiare il valore **X509Certificate** e incollarlo nella sezione **Certificato x509**.

    d. Nella casella di testo **Attributo nome** incollare il valore `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. Nella casella di testo **Attributo cognome** incollare il valore `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    f. Nella casella di testo **Attributo posta elettronica** incollare il valore `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    g. Nella casella di testo **Attributo nome utente** incollare il valore `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. Fare clic su **Salva** per salvare le impostazioni.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/lcvista-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/lcvista-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/lcvista-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/lcvista-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-lcvista-test-user"></a>Creazione di un utente test LCVista

In questa sezione viene creato un utente chiamato Britta Simon in LCVista. È necessario contattare il [team di supporto LCVista Client](https://lcvista.com/contact) per aggiungere gli utenti nell'applicazione LCVista. 

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso ad LCVista.

![Assegna utente][200] 

**Per assegnare Britta Simon a LCVista, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **LCVista**.

    ![Configure Single Sign-On](./media/lcvista-tutorial/tutorial_lcvista_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso. Fare clic sul riquadro LCVista nel Pannello di accesso e si verrà reindirizzati alla pagina di accesso dell'organizzazione. Dopo aver eseguito l'accesso, si verrà registrati nell'applicazione LCVista. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lcvista-tutorial/tutorial_general_01.png
[2]: ./media/lcvista-tutorial/tutorial_general_02.png
[3]: ./media/lcvista-tutorial/tutorial_general_03.png
[4]: ./media/lcvista-tutorial/tutorial_general_04.png

[100]: ./media/lcvista-tutorial/tutorial_general_100.png

[200]: ./media/lcvista-tutorial/tutorial_general_200.png
[201]: ./media/lcvista-tutorial/tutorial_general_201.png
[202]: ./media/lcvista-tutorial/tutorial_general_202.png
[203]: ./media/lcvista-tutorial/tutorial_general_203.png

