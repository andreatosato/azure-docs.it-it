---
title: Configurare il provisioning di dispositivi nel portale di Azure | Microsoft Docs
description: Guida introduttiva di Azure - Configurare il servizio Device Provisioning in hub IoT di Azure nel portale di Azure
author: wesmc7777
ms.author: wesmc
ms.date: 07/12/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: ce1586e472e1d1ea5ddd9ca5a426b1bea2b5b931
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42022772"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Configurare il servizio Device Provisioning in hub IoT con il portale di Azure

Questa procedura illustra come configurare le risorse cloud di Azure nel portale per il provisioning dei dispositivi. Questo articolo include i passaggi per la creazione dell'hub IoT e di una nuova istanza del servizio Device Provisioning in hub IoT, oltre al collegamento dei due servizi. 

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.


## <a name="create-an-iot-hub"></a>Creare un hub IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>Creare una nuova istanza del servizio Device Provisioning in hub IoT

1. Fare clic sul pulsante **Crea una risorsa** visualizzato nell'angolo in alto a sinistra nel portale di Azure.

2. *Nel Marketplace* cercare il **servizio Device Provisioning**. Selezionare il servizio **Device Provisioning in hub IoT** e fare clic sul pulsante **Crea**. 

3. Specificare le informazioni seguenti per la nuova istanza del servizio Device Provisioning e quindi fare clic su **Crea**.

    * **Nome:** specificare un nome univoco per la nuova istanza del servizio Device Provisioning. Se il nome immesso è disponibile, viene visualizzato un segno di spunta verde.
    * **Sottoscrizione**: scegliere la sottoscrizione da usare per creare l'istanza del servizio Device Provisioning.
    * **Gruppo di risorse:** questo campo consente di creare un nuovo gruppo di risorse o sceglierne uno esistente per contenere la nuova istanza. Scegliere lo stesso gruppo di risorse che contiene l'hub Iot creato in precedenza, ad esempio, **TestResources**. Inserendo tutte le risorse correlate in un gruppo è possibile gestirle insieme. Ad esempio, con l'eliminazione del gruppo di risorse vengono eliminate tutte le risorse contenute in quel gruppo. Per altre informazioni, vedere [Usare i gruppi di risorse per gestire le risorse di Azure](../azure-resource-manager/resource-group-portal.md).
    * **Località:** selezionare la località più vicina ai dispositivi.
    * **Aggiungi al dashboard:** selezionare questa opzione per aggiungere l'istanza al dashboard e renderla più facile da trovare.

    ![Immettere le informazioni di base sull'istanza del servizio Device Provisioning nel pannello del portale](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Dopo aver distribuito il servizio, si aprirà automaticamente il pannello riepilogativo.


## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>Collegare l'hub IoT e il servizio Device Provisioning

In questa sezione verrà aggiunta una configurazione all'istanza del servizio Device Provisioning. Questa configurazione imposta l'hub IoT per i dispositivi che verranno sottoposti a provisioning.

1. Fare clic sul pulsante **Tutte le risorse** nel menu a sinistra del portale di Azure. Selezionare l'istanza del servizio Device Provisioning creata nella sezione precedente.  

2. Nel pannello di riepilogo del servizio Device Provisioning selezionare **Linked IoT hubs** (Hub IoT collegati). Fare clic sul pulsante **+ Aggiungi** in alto. 

3. Nella pagina **Aggiungi collegamento all'hub IoT** fornire le informazioni seguenti per collegare la nuova istanza del servizio Device Provisioning a un hub IoT. Fare quindi clic su **Salva**. 

    * **Sottoscrizione:**  selezionare la sottoscrizione contenente l'hub IoT che si vuole collegare alla nuova istanza del servizio Device Provisioning.
    * **Hub IoT:** selezionare l'hub IoT da collegare alla nuova istanza del servizio Device Provisioning.
    * **Criteri di accesso:** selezionare **iothubowner** come credenziali per stabilire il collegamento con l'hub IoT.  

    ![Specificare il nome dell'hub da collegare all'istanza del servizio Device Provisioning nel pannello del portale](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. L'hub selezionato verrà ora visualizzato nel pannello **Linked IoT hubs** (Hub IoT collegati). Potrebbe essere necessario fare clic su **Aggiorna** per visualizzare **Hub IoT collegati**.



## <a name="clean-up-resources"></a>Pulire le risorse

Altre guide introduttive della raccolta si basano su questa. Se si prevede di continuare a usare le guide introduttive o le esercitazioni successive, non eliminare le risorse create in questa guida introduttiva. Se non si prevede di continuare, seguire questa procedura per eliminare tutte le risorse create da questa guida introduttiva nel portale di Azure.

1. Nel portale di Azure fare clic su **Tutte le risorse** nel menu a sinistra e quindi selezionare il servizio Device Provisioning. Nella parte superiore del pannello **Tutte le risorse** fare clic su **Elimina**.  
2. Nel portale di Azure fare clic su **Tutte le risorse** nel menu a sinistra e quindi selezionare l'hub IoT. Nella parte superiore del pannello **Tutte le risorse** fare clic su **Elimina**.  

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva sono stati distribuiti un hub IoT e un'istanza del servizio Device Provisioning e le due risorse sono state collegate. Per informazioni su come usare questa configurazione per eseguire il provisioning di un dispositivo simulato, proseguire con la guida introduttiva per la creazione di un dispositivo simulato.

> [!div class="nextstepaction"]
> [Guida introduttiva per la creazione di un dispositivo simulato](./quick-create-simulated-device.md)
