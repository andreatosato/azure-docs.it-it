---
title: Trasformare e proteggere l'API con Gestione API di Azure | Microsoft Docs
description: Informazioni su come proteggere l'API con criteri di quota e limitazione (limitazione della frequenza).
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: b94f6ad4c7c6f3b5e93cdb890e053a3d1678e161
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722950"
---
# <a name="transform-and-protect-your-api"></a>Trasformare e proteggere l'API 

L'esercitazione illustra come trasformare l'API in modo che non riveli le informazioni di un back-end privato. Potrebbe, ad esempio, essere necessario nascondere le informazioni sullo stack di tecnologie in esecuzione nel back-end. Potrebbe anche essere necessario nascondere gli URL originali visualizzati nel corpo della risposta HTTP dell'API e reindirizzarli invece al gateway di Gestione API.

Questa esercitazione illustra quanto sia semplice aggiungere la protezione all'API back-end configurando il limite di frequenza con Gestione API di Azure. Potrebbe, ad esempio, essere necessario limitare il numero di chiamate all'API in modo che non venga usata in modo eccessivo dagli sviluppatori. Per altre informazioni, vedere [Criteri in Gestione API](api-management-policies.md)

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Trasformare un'API per rimuovere le intestazioni di risposta
> * Sostituire gli URL originali nel corpo della risposta dell'API con URL del gateway di Gestione API
> * Proteggere un'API aggiungendo criteri relativi ai limiti di frequenza (limitazione delle richieste)
> * Testare le trasformazioni

![Criteri](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>prerequisiti

+ Completare la guida introduttiva seguente: [Creare un'istanza di Gestione API di Azure](get-started-create-service-instance.md).
+ Completare anche l'esercitazione seguente: [Importare e pubblicare la prima API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Trasformare un'API per rimuovere le intestazioni di risposta

Questa sezione illustra come nascondere le intestazioni HTTP che non devono essere visualizzate dagli utenti. In questo esempio le intestazioni seguenti vengono eliminate nella risposta HTTP:

* **X-Powered-By**
* **X-AspNet-Version**

### <a name="test-the-original-response"></a>Testare la risposta originale

Per visualizzare la risposta originale:

1. Nell'istanza del servizio Gestione API selezionare **API** (in **GESTIONE API**).
2. Fare clic su **Demo Conference API** (API Demo Conference) nell'elenco di API.
3. Selezionare l'operazione **GetSpeakers**.
4. Fare clic sulla scheda **Test** nella parte superiore della schermata.
5. Fare clic sul pulsante **Invia** nella parte inferiore della schermata. 

    Come si può notare, la risposta originale è simile alla seguente:

    ![Criteri](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Impostare i criteri di trasformazione

1. Selezionare **Demo Conference API**.
2. Nella parte superiore della schermata selezionare la scheda **Progettazione**.
3. Selezionare **Tutte le operazioni**.
4. Nella finestra **Elaborazione in uscita** fare clic sul triangolo (accanto alla matita) e selezionare **Editor di codice**.
     ![Modificare i criteri](./media/set-edit-policies/set-edit-policies01.png)
5. Posizionare il cursore all'interno dell'elemento **&lt;outbound&gt;**.
6. Nella finestra di destra, in **Transformation policies** (Criteri di trasformazione) fare clic su **+ Set HTTP header** (+ Imposta intestazione HTTP) due volte (per inserire due frammenti di criteri).

    ![Criteri](./media/transform-api/transform-api.png)
7. Modificare il codice **<outbound>** in modo che risulti simile al seguente:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />

    ![Criteri](./media/transform-api/set-policy.png)
8. Fare clic sul pulsante **Salva** .


## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Sostituire gli URL originali nel corpo della risposta dell'API con URL del gateway di Gestione API

Questa sezione illustra come nascondere gli URL originali visualizzati nel corpo della risposta HTTP dell'API e reindirizzarli invece al gateway di Gestione API.

### <a name="test-the-original-response"></a>Testare la risposta originale

Per visualizzare la risposta originale:

1. Selezionare **Demo Conference API**.
2. Selezionare l'operazione **GetSpeakers**.
3. Fare clic sulla scheda **Test** nella parte superiore della schermata.
4. Fare clic sul pulsante **Invia** nella parte inferiore della schermata. 

    Come si può notare, la risposta originale è simile alla seguente:

    ![Criteri](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Impostare i criteri di trasformazione

1. Selezionare **Demo Conference API**.
2. Selezionare **Tutte le operazioni**.
3. Nella parte superiore della schermata selezionare la scheda **Progettazione**.
4. Nella finestra **Elaborazione in uscita** fare clic sul triangolo (accanto alla matita) e selezionare **Editor di codice**.
5. Posizionare il cursore all'interno dell'elemento **&lt;outbound&gt;**.
6. Nella finestra a destra, in **Transformation policies** (Criteri di trasformazione) fare clic su **+ Find and replace string in body** (+ Trova e sostituisci la stringa nel corpo).
7. Modificare il codice **find-and-replace** (nell'elemento **\<in uscita\>**) per sostituire l'URL in modo che corrisponda al gateway di Gestione API. Ad esempio: 

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Proteggere un'API aggiungendo criteri relativi ai limiti di frequenza (limitazione delle richieste)

Questa sezione illustra come aggiungere la protezione per l'API back-end configurando i limiti di frequenza. Potrebbe, ad esempio, essere necessario limitare il numero di chiamate all'API in modo che non venga usata in modo eccessivo dagli sviluppatori. In questo esempio il limite è impostato su 3 chiamate ogni 15 secondi per ogni ID di sottoscrizione. Dopo 15 secondi, uno sviluppatore può ripetere la chiamata all'API.

1. Selezionare **Demo Conference API**.
2. Selezionare **Tutte le operazioni**.
3. Nella parte superiore della schermata selezionare la scheda **Progettazione**.
4. Nella finestra **Elaborazione in ingresso** fare clic sul triangolo (accanto alla matita) e selezionare **Editor di codice**.
5. Posizionare il cursore all'interno dell'elemento **&lt;inbound&gt;**.
6. Nella finestra a destra, in **Access restriction policies** (Criteri di restrizione dell'accesso) fare clic su **+ Limit call rate per key** (+ Limita frequenza delle chiamate per chiave).
7. Sostituire il codice **rate-limit-by-key** (nell'elemento **\<in ingresso\>**) con il codice seguente:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Testare le trasformazioni
        
A questo punto, se si esamina il codice nell'editor di codice, i criteri sono simili ai seguenti:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

La parte rimanente della sezione testa le trasformazioni dei criteri impostate in questo articolo.

### <a name="test-the-stripped-response-headers"></a>Testare le intestazioni della risposta eliminate

1. Selezionare **Demo Conference API**.
2. Fare clic sull'operazione **GetSpeakers**.
3. Selezionare la scheda **Test**.
4. Fare clic su **Invia**.

    Come si può notare, le intestazioni sono state eliminate:

    ![Criteri](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testare l'URL sostituito

1. Selezionare **Demo Conference API**.
2. Fare clic sull'operazione **GetSpeakers**.
3. Selezionare la scheda **Test**.
4. Fare clic su **Invia**.

    Come si può notare, l'URL è stato sostituito.

    ![Criteri](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testare il limite di frequenza (limitazione delle richieste)

1. Selezionare **Demo Conference API**.
2. Fare clic sull'operazione **GetSpeakers**.
3. Selezionare la scheda **Test**.
4. Fare clic su **Invia** tre volte in una riga.

    Dopo avere inviato la richiesta 3 volte, viene visualizzata la risposta **429 Too many requests**.
5. Attendere circa 15 secondi e fare si nuovo clic su **Invia**. Questa volta verrà visualizzata una risposta **200 OK**.

    ![Limitazione](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Passaggi successivi

Questa esercitazione illustra come:

> [!div class="checklist"]
> * Trasformare un'API per rimuovere le intestazioni di risposta
> * Sostituire gli URL originali nel corpo della risposta dell'API con URL del gateway di Gestione API
> * Proteggere un'API aggiungendo criteri relativi ai limiti di frequenza (limitazione delle richieste)
> * Testare le trasformazioni

Passare all'esercitazione successiva:

> [!div class="nextstepaction"]
> [Monitorare l'API](api-management-howto-use-azure-monitor.md)
