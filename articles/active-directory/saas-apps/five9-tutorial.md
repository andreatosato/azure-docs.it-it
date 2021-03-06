---
title: 'Esercitazione: integrazione di Azure Active Directory con Five9 Plus Adapter (CTI, Contact Center Agents) | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Five9 Plus Adapter (CTI, Contact Center Agents).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 8ee04008b62867c8eba68b1525cf50edec881cbc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432634"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Esercitazione: integrazione di Azure Active Directory con Five9 Plus Adapter (CTI, Contact Center Agents)

Questa esercitazione descrive come integrare Five9 Plus Adapter (CTI, Contact Center Agents) con Azure Active Directory, ovvero Azure AD.

L'integrazione di Five9 Plus Adapter (CTI, Contact Center Agents) con Azure AD comporta i vantaggi seguenti:

- È possibile controllare in Azure AD che ha accesso a Five9 Plus Adapter (CTI, Contact Center Agents)
- È possibile abilitare gli utenti per l'accesso automatico a Five9 Plus Adapter (CTI, Contact Center Agents), ovvero per l'accesso Single Sign-On, con i propri account di Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents), sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione abilitata all'accesso Single Sign-On per Five9 Plus Adapter (CTI, Contact Center Agents)

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese: [offerta prova](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Five9 Plus Adapter (CTI, Contact Center Agents) dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Aggiunta di Five9 Plus Adapter (CTI, Contact Center Agents) dalla raccolta
Per configurare l'integrazione di Five9 Plus Adapter (CTI, Contact Center Agents) in Azure AD, è necessario aggiungere Five9 Plus Adapter (CTI, Contact Center Agents) dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Five9 Plus Adapter (CTI, Contact Center Agents) dalla raccolta, eseguire la procedura seguente:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Five9 Plus Adapter (CTI, Contact Center Agents)**.

    ![Creazione di un utente test di Azure AD](./media/five9-tutorial/tutorial_five9_search.png)

1. Nel pannello dei risultati selezionare **Five9 Plus Adapter (CTI, Contact Center Agents)**, quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents) in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Five9 Plus Adapter (CTI, Contact Center Agents) che corrisponde all'utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato inFive9 Plus Adapter (CTI, Contact Center Agents).

Per stabilire la relazione di collegamento, in Five9 Plus Adapter (CTI, Contact Center Agents) assegnare il valore di **nome utente** di Azure AD come valore dell'attributo **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents), è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test di Five9 Plus Adapter (CTI, Contact Center Agents)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** : per avere una controparte di Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents) collegata alla rappresentazione dell'utente di Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Five9 Plus Adapter (CTI, Contact Center Agents).

**Per configurare il Single Sign-On di Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents) seguire la procedura seguente:**

1. Nel portale di Azure, nella pagina di integrazione dell'applicazione **Five9 Plus Adapter (CTI, Contact Center Agents)** fare clic su **Single sign-on**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. Nella sezione **Five9 Plus Adapter (CTI, Contact Center Agents) Domain and URLs** (URL e dominio di Five9 Plus Adapter (CTI, Contact Center Agents)) eseguire la procedura seguente:

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. Nella casella di testo **Identificatore** digitare un URL usando i criteri seguenti:

    |    Environment      |       URL      |
    | :-- | :-- |
    | Per "Five9 Plus Adapter for Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Per "Five9 Plus Adapter for Zendesk" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Per "Five9 Plus Adapter for Agent Desktop Toolkit" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente:

    |      Environment     |      URL      |
    | :--                  | :--           |
    | Per "Five9 Plus Adapter for Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Per "Five9 Plus Adapter for Zendesk" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Per "Five9 Plus Adapter for Agent Desktop Toolkit" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_general_400.png)

1. Nella sezione **Five9 Plus Adapter (CTI, Contact Center Agents) Configuration** (Configurazione di Five9 Plus Adapter (CTI, Contact Center Agents)) fare clic su **Configure Five9 Plus Adapter (CTI, Contact Center Agents)** (Configura Five9 Plus Adapter (CTI, Contact Center Agents)) per aprire la finestra **Configura accesso**. Copiare l'**URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido.**

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_configure.png) 

1. Per configurare l'accesso Single Sign-On sul lato **Five9 Plus Adapter (CTI, Contact Center Agents)**, è necessario inviare il file **Certificato (Base64) scaricato, l'URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** al [team di supporto di Five9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact). In aggiunta, per proseguire la configurazione SSO seguire la procedura di seguito in base all'adattatore:

    a. Guida dell'amministratore per "Five9 Plus Adapter for Agent Desktop Toolkit": [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. Guida dell'amministratore per "Five9 Plus Adapter for Microsoft Dynamics CRM": [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. Guida dell'amministratore per "Five9 Plus Adapter for Zendesk": [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/five9-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/five9-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/five9-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/five9-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Creazione dell'utente test di Five9 Plus Adapter (CTI, Contact Center Agents)

In questa sezione si crea un utente chiamato Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents). Contattare il [team di supporto di Five9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact) per aggiungere gli utenti nella piattaforma Five9 Plus Adapter (CTI, Contact Center Agents). Gli utenti devono essere creati e attivati prima di usare l'accesso Single Sign-On.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Five9 Plus Adapter (CTI, Contact Center Agents).

![Assegna utente][200] 

**Per assegnare Britta Simon a Five9 Plus Adapter (CTI, Contact Center Agents), eseguire la procedura seguente:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **Five9 Plus Adapter (CTI, Contact Center Agents)**.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Five9 Plus Adapter (CTI, Contact Center Agents) nel Pannello di accesso, l'utente dovrebbe accedere automaticamente all'applicazione Five9 Plus Adapter (CTI, Contact Center Agents).
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

