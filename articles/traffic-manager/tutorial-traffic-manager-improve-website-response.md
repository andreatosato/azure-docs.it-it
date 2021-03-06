---
title: 'Esercitazione: Instradare il traffico per migliorare la risposta di un sito Web tramite Gestione traffico di Azure | Microsoft Docs'
description: Questa esercitazione descrive come creare un profilo di Gestione traffico per creare un sito Web altamente reattivo.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: jeconnoc
editor: ''
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: kumud
ms.openlocfilehash: 89518d30b862e18fb7c989c95144ffa7f1c294fc
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40024718"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Esercitazione: Migliorare la risposta di un sito Web tramite Gestione traffico 

Questa esercitazione descrive come usare Gestione traffico per creare un sito Web altamente reattivo indirizzando il traffico degli utenti al sito Web con una latenza minima. In genere, il data center con la latenza più bassa è quello più vicino in termini di distanza geografica.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare due macchine virtuali che eseguono un sito Web di base in IIS
> * Creare due macchine virtuali di test per visualizzare Gestione traffico in azione
> * Configurare il nome DNS per le macchine virtuali che eseguono IIS
> * Creare un profilo di Gestione traffico per ottenere migliori prestazioni del sito Web
> * Aggiungere endpoint di macchine virtuali al profilo di Gestione traffico
> * Visualizzare Gestione traffico in azione

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti
Per visualizzare Gestione traffico in azione, è necessario implementare quanto segue:
- Due istanze di siti Web di base in esecuzione in aree diverse di Azure: **Stati Uniti orientali** e **Europa occidentale**.
- Due macchine virtuali (VM) per il test di Gestione traffico: una in **Stati Uniti orientali** e la seconda in **Europa occidentale**. Le VM di test vengono usate per illustrare come Gestione traffico instrada il traffico degli utenti verso il sito Web in esecuzione nella stessa area poiché offre la latenza più bassa.

### <a name="sign-in-to-azure"></a>Accedere ad Azure 

Accedere al portale di Azure all'indirizzo https://portal.azure.com.

### <a name="create-websites"></a>Creare i siti Web

In questa sezione si creano due istanze di sito Web che forniscono due endpoint di servizio per il profilo di Gestione traffico in due aree di Azure. La procedura di creazione dei due siti Web include i passaggi seguenti:
1. Creare due VM per l'esecuzione di un sito Web di base, una in **Stati Uniti orientali** e l'altra in **Europa occidentale**.
2. Installare un server IIS in ogni VM e aggiornare la pagina predefinita del sito Web che descrive il nome della VM a cui viene connesso l'utente quando visita il sito Web.

#### <a name="create-vms-for-running-websites"></a>Creare VM per l'esecuzione di siti Web
In questa sezione si creano due VM *myIISVMEastUS* e *myIISVMWEurope* nelle aree di Azure **Stati Uniti orientali** e **Europa occidentale**.

1. Nell'angolo in alto a sinistra del portale di Azure selezionare **Crea una risorsa** > **Calcolo** > **Windows Server 2016 VM**.
2. Immettere o selezionare le informazioni seguenti in **Basics** (Generale), accettare le impostazioni predefinite rimanenti e quindi selezionare **Crea**:

    |Impostazione|Valore|
    |---|---|
    |Nome|myIISVMEastUS|
    |Nome utente| Immettere un nome utente a scelta.|
    |Password| Immettere una password a scelta. La password deve contenere almeno 12 caratteri e soddisfare i [requisiti di complessità definiti](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Gruppo di risorse| Selezionare **Nuovo** e quindi digitare *myResourceGroupTM1*.|
    |Località| Selezionare **Stati Uniti orientali**.|
    |||
4. Selezionare le dimensioni della VM in **Scegli una dimensione**.
5. Selezionare i valori seguenti in **Impostazioni**, quindi scegliere **OK**:
    
    |Impostazione|Valore|
    |---|---|
    |Rete virtuale| Selezionare **Rete virtuale** in **Crea rete virtuale**, immettere **myVNet1** per *Nome* e *mySubnet* per la subnet.|
    |Gruppo di sicurezza di rete|Selezionare **Base** e dall'elenco a discesa **Selezionare le porte in ingresso pubbliche** selezionare **HTTP** e **RDP**. |
    |Diagnostica di avvio|Selezionare **Disabilitata**.|
    |||
6. In **Crea** in **Riepilogo** selezionare **Crea** per avviare la distribuzione della VM.

7. Ripetere i passaggi da 1 a 6, con le modifiche seguenti:

    |Impostazione|Valore|
    |---|---|
    |Gruppo di risorse | Selezionare **Nuovo** e quindi digitare *myResourceGroupTM2*|
    |Località|Europa occidentale|
    |Nome macchina virtuale | myIISVMWEurope|
    |Rete virtuale | Selezionare **Rete virtuale** in **Crea rete virtuale**, immettere **myVNet2** per *Nome* e *mySubnet* per la subnet.|
    |||
8. La creazione delle VM può richiedere alcuni minuti. Non procedere con i passaggi rimanenti finché non sono state create entrambe le VM.

   ![Creare una macchina virtuale](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>Installare IIS e personalizzare la pagina Web predefinita

In questa sezione si installa il server IIS nelle due VM, *myIISVMEastUS*   &  *myIISVMWEurope*, e quindi si aggiorna la pagina predefinita del sito Web. La pagina del sito Web personalizzata mostra il nome della VM a cui viene connesso l'utente quando visita il sito Web da un Web browser.

1. Selezionare **Tutte le risorse** nel menu a sinistra e quindi nell'elenco delle risorse fare clic su *myIISVMEastUS*, che si trova nel gruppo di risorse *myResourceGroupTM1*.
2. Nella pagina **Panoramica** fare clic su **Connetti** e quindi selezionare **Scarica file RDP** in **Connetti a macchina virtuale**. 
3. Aprire il file con estensione rdp scaricato. Quando richiesto, selezionare **Connetti**. Immettere il nome utente e la password specificati al momento della creazione della VM. Potrebbe essere necessario selezionare **Altre opzioni**, quindi **Usa un altro account** per specificare le credenziali immesse al momento della creazione della VM. 
4. Selezionare **OK**.
5. Durante il processo di accesso potrebbe essere visualizzato un avviso relativo al certificato. Se viene visualizzato l'avviso, selezionare **Sì** o **Continua** per procedere con la connessione.
6. Nel desktop del server passare a **Strumenti di amministrazione Windows**>**Server Manager**.
7. Avviare Windows PowerShell su VM1 e usare i comandi seguenti per installare il server IIS e aggiornare il file con estensione htm predefinito.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Installare IIS e personalizzare la pagina Web](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. Chiudere la connessione RDP con *myIISVMEastUS*.
9. Ripetere i passaggi da 1 a 8 creando una connessione RDP alla VM *myIISVMWEurope* all'interno del gruppo di risorse *myResourceGroupTM2* per installare IIS e personalizzare la pagina Web predefinita.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configurare i nomi DNS per le VM che eseguono IIS

Gestione traffico instrada il traffico degli utenti in base al nome DNS degli endpoint di servizio. In questa sezione si configurano i nomi DNS per i server IIS, *myIISVMEastUS* e *myIISVMWEurope*.

1. Fare clic su **Tutte le risorse** nel menu a sinistra e quindi nell'elenco delle risorse selezionare *myIISVMEastUS*, che si trova nel gruppo di risorse *myResourceGroupTM1*.
2. Nella pagina **Panoramica**, in **Nome DNS**, selezionare **Configura**.
3. Nella pagina **Configurazione**, sotto l'etichetta del nome DNS, aggiungere un nome univoco e quindi selezionare **Salva**.
4. Ripetere i passaggi da 1 a 3 per la VM denominata *myIISVMWEurope* che si trova nel gruppo di risorse *myResourceGroupTM1*.

### <a name="create-test-vms"></a>Creare le VM di test

In questa sezione si crea una VM (*mVMEastUS* e *myVMWestEurope*) in ogni area di Azure (**Stati Uniti orientali** ed **Europa occidentale**). Queste VM verranno usate per verificare come Gestione traffico instrada il traffico verso il server IIS più vicino quando si visita il sito Web.

1. Nell'angolo in alto a sinistra del portale di Azure selezionare **Crea una risorsa** > **Calcolo** > **Windows Server 2016 VM**.
2. Immettere o selezionare le informazioni seguenti in **Basics** (Generale), accettare le impostazioni predefinite rimanenti e quindi selezionare **Crea**:

    |Impostazione|Valore|
    |---|---|
    |Nome|myVMEastUS|
    |Nome utente| Immettere un nome utente a scelta.|
    |Password| Immettere una password a scelta. La password deve contenere almeno 12 caratteri e soddisfare i [requisiti di complessità definiti](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Gruppo di risorse| Selezionare **Esistente** e quindi *myResourceGroupTM1*.|
    |||

4. Selezionare le dimensioni della VM in **Scegli una dimensione**.
5. Selezionare i valori seguenti in **Impostazioni**, quindi scegliere **OK**:
    |Impostazione|Valore|
    |---|---|
    |Rete virtuale| Selezionare **Rete virtuale** in **Crea rete virtuale**, immettere **myVNet3** per *Nome* e *mySubnet* per la subnet.|
    |Gruppo di sicurezza di rete|Selezionare **Base** e dall'elenco a discesa **Selezionare le porte in ingresso pubbliche** selezionare **HTTP** e **RDP**. |
    |Diagnostica di avvio|Selezionare **Disabilitata**.|
    |||

6. In **Crea** in **Riepilogo** selezionare **Crea** per avviare la distribuzione della VM.

7. Ripetere i passaggi da 1 a 5, con le modifiche seguenti:

    |Impostazione|Valore|
    |---|---|
    |Nome macchina virtuale | *myVMWEurope*|
    |Gruppo di risorse | Selezionare **Esistente** e quindi digitare *myResourceGroupTM2*.|
    |Rete virtuale | Selezionare **Rete virtuale** in **Crea rete virtuale**, immettere **myVNet4** per *Nome* e *mySubnet* per la subnet.|
    |||

8. La creazione delle VM può richiedere alcuni minuti. Non procedere con i passaggi rimanenti finché non sono state create entrambe le VM.

## <a name="create-a-traffic-manager-profile"></a>Creare un profilo di Gestione traffico
Creare un profilo di Gestione traffico che indirizza il traffico degli utenti verso l'endpoint con latenza più bassa.

1. In alto a sinistra nello schermo selezionare **Crea una risorsa** > **Rete** > **Profilo di Gestione traffico** > **Crea**.
2. In **Crea profilo di Gestione traffico** immettere o selezionare le informazioni seguenti, accettare le impostazioni predefinite per le impostazioni rimanenti e quindi selezionare **Crea**:
    | Impostazione                 | Valore                                              |
    | ---                     | ---                                                |
    | Nome                   | Questo nome deve essere univoco all'interno della zona trafficmanager.net e determina il nome DNS, trafficmanager.net, che viene usato per accedere al profilo di Gestione traffico.                                   |
    | Metodo di routing          | Selezionare il metodo di routing **Priorità**.                                       |
    | Sottoscrizione            | Selezionare la propria sottoscrizione.                          |
    | Gruppo di risorse          | Selezionare **Crea nuovo** e immettere *myResourceGroupTM1*. |
    | Località                | Selezionare **Stati Uniti orientali**.  Questa impostazione indica la località del gruppo di risorse e non ha alcun impatto sul profilo di Gestione traffico che sarà distribuito a livello globale.                              |
    |
  
    ![Creare un profilo di Gestione traffico](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Aggiungere endpoint di Gestione traffico

Aggiungere le due VM che eseguono i server IIS, *myIISVMEastUS*   &  *myIISVMWEurope*, per instradare il traffico degli utenti verso l'endpoint più vicino.

1. Nella barra di ricerca del portale cercare il nome del profilo di Gestione traffico creato nella sezione precedente e selezionarlo nei risultati visualizzati.
2. In **Profilo di Gestione traffico**, nella sezione **Impostazioni**, fare clic su **Endpoint** e quindi su **Aggiungi**.
3. Immettere o selezionare le informazioni seguenti, accettare le impostazioni predefinite rimanenti e quindi scegliere **OK**:

    | Impostazione                 | Valore                                              |
    | ---                     | ---                                                |
    | Tipo                    | Endpoint di Azure                                   |
    | Nome           | myEastUSEndpoint                                        |
    | Tipo di risorsa di destinazione           | Indirizzo IP pubblico                          |
    | Risorsa di destinazione          | **Scegliere un indirizzo IP pubblico** per visualizzare l'elenco delle risorse con gli indirizzi IP pubblici inclusi nella stessa sottoscrizione. In **Risorsa** selezionare l'indirizzo IP pubblico denominato *myIISVMEastUS-ip*. Questo è l'indirizzo IP pubblico della VM del server IIS nell'area Stati Uniti orientali.|
    |        |           |

4. Ripetere i passaggi 2 e 3 per aggiungere un altro endpoint denominato *myWestEuropeEndpoint* per l'indirizzo IP pubblico *myIISVMWEurope-ip* associato alla VM del server IIS denominata *myIISVMWEurope*.
5.  Una volta completata l'aggiunta di entrambi gli endpoint, essi vengono visualizzati in **Profilo di Gestione traffico** insieme al relativo stato di monitoraggio **Online**.

    ![Aggiungere un endpoint di Gestione traffico](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)
  

## <a name="test-traffic-manager-profile"></a>Testare il profilo di Gestione traffico
In questa sezione si verifica come Gestione traffico instrada il traffico degli utenti verso le VM più vicine che eseguono il sito Web per offrire una latenza minima. Per visualizzare Gestione traffico in azione, completare i passaggi seguenti:
1. Determinare il nome DNS del profilo di Gestione traffico.
2. Visualizzare Gestione traffico in azione nel modo seguente:
    - Dalla VM di test (*myVMEastUS*) che si trova nell'area **Stati Uniti orientali**, in un Web browser, passare al nome DNS del profilo di Gestione traffico.
    - Dalla VM di test (*myVMEastUS*) che si trova nell'area **Europa occidentale**, in un Web browser, passare al nome DNS del profilo di Gestione traffico.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Determinare il nome DNS del profilo di Gestione traffico
In questa esercitazione, per semplicità, si usa il nome DNS del profilo di Gestione traffico per visitare i siti Web. 

È possibile determinare il nome DNS del profilo di Gestione traffico nel modo seguente:

1.  Nella barra di ricerca del portale cercare il nome del **Profilo di Gestione traffico** creato nella sezione precedente. Fare clic sul profilo di Gestione traffico nei risultati visualizzati.
1. Fare clic su **Panoramica**.
2. Il **Profilo di Gestione traffico** visualizza il nome DNS del profilo di Gestione traffico appena creato. Nelle distribuzioni di produzione si configura un nome di dominio personalizzato in modo che punti al nome di dominio di Gestione traffico usando un record CNAME DNS.

   ![Nome DNS di Gestione traffico](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Visualizzare Gestione traffico in azione
In questa sezione è possibile visualizzare Gestione traffico in azione. 

1. Selezionare **Tutte le risorse** nel menu a sinistra e quindi nell'elenco delle risorse fare clic su *myVMEastUS*, che si trova nel gruppo di risorse *myResourceGroupTM1*.
2. Nella pagina **Panoramica** fare clic su **Connetti** e quindi selezionare **Scarica file RDP** in **Connetti a macchina virtuale**. 
3. Aprire il file con estensione rdp scaricato. Quando richiesto, selezionare **Connetti**. Immettere il nome utente e la password specificati al momento della creazione della VM. Potrebbe essere necessario selezionare **Altre opzioni**, quindi **Usa un altro account** per specificare le credenziali immesse al momento della creazione della VM. 
4. Selezionare **OK**.
5. Durante il processo di accesso potrebbe essere visualizzato un avviso relativo al certificato. Se viene visualizzato l'avviso, selezionare **Sì** o **Continua** per procedere con la connessione. 
1. In un Web browser, nella VM *myVMEastUS*, digitare il nome DNS del profilo di Gestione traffico per visualizzare il sito Web. Dalla VM in **Stati Uniti orientali** si viene indirizzati al sito Web più vicino ospitato nel server IIS più vicino *myIISVMEastUS* che si trova nell'area **Stati Uniti orientali**.

   ![Testare il profilo di Gestione traffico](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. A questo punto, connettersi alla VM *myVMWestEurope* che si trova in **Europa occidentale** eseguendo i passaggi da 1 a 5 e passare al nome di dominio del profilo di Gestione traffico da questa VM.  Dalla VM in **Europa occidentale** si viene ora indirizzati al sito Web ospitato nel server IIS più vicino *myIISVMWEurope* che si trova in **Europa occidentale**. 

   ![Testare il profilo di Gestione traffico](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Eliminare il profilo di Gestione traffico
Quando non sono più necessari, eliminare i gruppi di risorse (**ResourceGroupTM1** e **ResourceGroupTM2**). A tale scopo, selezionare il gruppo di risorse (**ResourceGroupTM1** o **ResourceGroupTM2**) e quindi selezionare **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Distribuire il traffico in un set di endpoint](traffic-manager-configure-weighted-routing-method.md)


