---
title: 'Esercitazione: Integrazione di Azure Active Directory con Direct | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Direct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e9003df88e8ed330e0344c63ee0516bc24a7eaad
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433902"
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a>Esercitazione: Integrazione di Azure Active Directory con Direct

Questa esercitazione descrive come integrare Direct con Azure Active Directory (Azure AD).

L'integrazione di Direct con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Direct
- È possibile abilitare gli utenti per l'accesso automatico Single Sign-On a Direct con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Direct sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Direct abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Direct dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-direct-from-the-gallery"></a>Aggiunta di Direct dalla raccolta
Per configurare l'integrazione di Direct in Azure AD è necessario aggiungere Direct dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Direct dalla raccolta seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Direct**.

    ![Creazione di un utente test di Azure AD](./media/direct-tutorial/tutorial_direct_search.png)

1. Nel pannello dei risultati selezionare **Direct** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Direct mediante un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve sapere quale utente di Direct corrisponde a un determinato utente di Azure AD. In altre parole è necessario stabilire una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Direct.

Per stabilire la relazione di collegamento, in Direct assegnare il valore di **nome utente** di Azure AD come valore dell'attributo **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Direct è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test di Direct](#creating-a-direct-test-user)**: per avere una controparte di Britta Simon in Direct collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel nuovo portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Direct.

**Per configurare l'accesso Single Sign-On di Azure AD con Direct, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Direct** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/direct-tutorial/tutorial_direct_samlbase.png)

1. Nella sezione **URL e dominio Direct** per configurare l'applicazione in modalità avviata da **IDP**:

    ![Configure Single Sign-On](./media/direct-tutorial/tutorial_direct_url.png)

    Nella casella di testo **Identificatore** digitare l'URL: `https://direct4b.com/`

1. Selezionare **Mostra impostazioni URL avanzate**, se si desidera configurare l'applicazione in modalità avviata da **SP**:

    ![Configure Single Sign-On](./media/direct-tutorial/tutorial_direct_url1.png)

     Nella casella di testo **URL accesso** digitare l'URL: `https://direct4b.com/sso` 
    
1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configure Single Sign-On](./media/direct-tutorial/tutorial_direct_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/direct-tutorial/tutorial_general_400.png)

1. Per configurare l'accesso Single Sign-On sul lato **Direct** è necessario inviare il file **XML metadati** scaricato al [team di supporto di Direct](https://direct4b.com/ja/support.html#inquiry). 

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/direct-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/direct-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/direct-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/direct-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-direct-test-user"></a>Creazione di un utente test di Direct

In questa sezione si crea un utente con nome Britta Simon in Direct. Collaborare con il [team di supporto di Direct](https://direct4b.com/ja/support.html#inquiry) per aggiungere gli utenti nella piattaforma Direct. Gli utenti devono essere creati e attivati prima di usare l'accesso Single Sign-On. 

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Direct.

![Assegna utente][200] 

**Per assegnare Britta Simon a Direct, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **Direct**.

    ![Configure Single Sign-On](./media/direct-tutorial/tutorial_direct_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

1. Se si vuole testare in **modalità avviata da IDP**:

    Quando si fa clic sul riquadro **Direct** nel pannello di accesso si accede automaticamente all'applicazione **Direct**.

1. Se si vuole testare in **modalità avviata da SP**:
    
    a. Fare clic sulla scheda **Direct** nel pannello di accesso. Verrà aperta la pagina per l'accesso all'applicazione.

    b. Digitare il `subdomain` nella casella di testo visualizzata e premere '次へ (Avanti) per accedere automaticamente all'Applicazione **Direct**.
    
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/direct-tutorial/tutorial_general_01.png
[2]: ./media/direct-tutorial/tutorial_general_02.png
[3]: ./media/direct-tutorial/tutorial_general_03.png
[4]: ./media/direct-tutorial/tutorial_general_04.png

[100]: ./media/direct-tutorial/tutorial_general_100.png

[200]: ./media/direct-tutorial/tutorial_general_200.png
[201]: ./media/direct-tutorial/tutorial_general_201.png
[202]: ./media/direct-tutorial/tutorial_general_202.png
[203]: ./media/direct-tutorial/tutorial_general_203.png

