---
title: Guida introduttiva all'API Controllo ortografico Bing | Microsoft Docs
description: Illustra come iniziare a usare l'API Controllo ortografico Bing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: AF8EB1F0-386D-4555-9354-735611435F04
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: cae8353e5be6e70eca90e5995b29b6774fc7d6a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372897"
---
# <a name="your-first-spell-check-request"></a>La prima richiesta di controllo ortografico

Prima di poter effettuare la prima chiamata, è necessario ottenere una chiave di sottoscrizione di Servizi cognitivi. Per ottenere una chiave, vedere [Prova Servizi cognitivi](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api).

Per controllare gli errori di ortografia e grammatica di una stringa di testo è consigliabile inviare una richiesta GET all'endpoint seguente:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> Endpoint di anteprima V7:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
La richiesta deve usare il protocollo HTTPS.

È consigliabile che tutte le richieste abbiano origine da un server. La distribuzione della chiave come parte di un'applicazione client consente a terze parti dannose di accedervi più facilmente. L'esecuzione delle chiamate da un server fornisce anche un singolo punto di aggiornamento per le future versioni dell'API.

La richiesta deve specificare il parametro di query [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text), contenente la stringa di testo da controllare. Nonostante sia facoltativo, la richiesta dovrebbe anche specificare il parametro di query [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt), che identifica il mercato da cui devono provenire i risultati. Per un elenco di parametri di query facoltativi come `mode`, vedere [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters) (Parametri di query). Tutti i valori dei parametri di query devono essere codificati in URL.  
  
La richiesta deve specificare l'intestazione [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey). Nonostante sia facoltativo, è consigliabile specificare anche le intestazioni seguenti:  
  
-   [User-agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Per un elenco di tutte le intestazioni di richiesta e risposta, vedere [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers) (Intestazioni).

## <a name="the-request"></a>Richiesta

Di seguito è illustrata una richiesta che include tutte le intestazioni e i parametri di query suggeriti. Se è la prima volta che si chiama un'API Bing, non includere l'intestazione dell'ID client. Includere l'ID client solo se in precedenza è già stata chiamata un'API Bing e Bing ha restituito un ID client per la combinazione utente e dispositivo. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Richiesta di anteprima V7:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

Di seguito è riportata la risposta alla richiesta precedente. L'esempio mostra anche le intestazioni di risposta specifiche di Bing.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  


## <a name="next-steps"></a>Passaggi successivi

Provare l'API. Passare a [Spell Check API Testing Console](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) (Console di test dell'API Controllo ortografico). 

Per informazioni dettagliate sull'utilizzo degli oggetti della risposta, vedere [Spell check text strings](./proof-text.md) (Eseguire il controllo ortografico sulle stringhe di testo).

