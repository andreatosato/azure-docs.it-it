---
title: Acquisizione di immagini dal Web con l'API Ricerca immagini Bing | Microsoft Docs
description: Usare l'API Ricerca immagini Bing per cercare e trovare immagini pertinenti dal Web.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: c379cc2dfbe53f83e4d79500cd947879407c8d55
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082582"
---
# <a name="getting-images-from-the-web-with-the-bing-image-search-api"></a>Acquisizione di immagini dal Web con l'API Ricerca immagini Bing

Usando l'API REST Ricerca immagini Bing, è possibile ottenere immagini dal Web correlate al termine di ricerca dell'utente inviando la richiesta GET seguente:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

> [!IMPORTANT]
> * Tutte le richieste devono essere eseguite da un server, non da un client.
> * Se è la prima volta che si chiama un'API di ricerca Bing, non includere l'intestazione dell'ID client. Includere l'ID client solo se in precedenza è già stata chiamata un'API Bing che ha restituito un ID client per la combinazione utente e dispositivo.
> * È necessario visualizzare le immagini nell'ordine indicato nella risposta.

## <a name="getting-images-from-a-specific-web-domain"></a>Acquisizione di immagini da un dominio Web specifico

Per ottenere immagini da un dominio specifico, usare l'operatore query [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Le risposte alle query tramite l'operatore `site:` possono includere contenuti per adulti indipendentemente dall'impostazione [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch). Usare `site:` solo se si è a conoscenza del contenuto nel dominio.

L'esempio seguente illustra come ottenere immagini di piccole dimensioni da ContosoSailing.com individuate da Bing la scorsa settimana.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filtering-images"></a>Applicazione di filtri alle immagini

 Per impostazione predefinita, l'API Ricerca immagini Bing restituisce tutte le immagini pertinenti alla query. Tuttavia, per filtrare le immagini restituite da Bing, ad esempio se sono necessarie solo immagini con uno sfondo trasparente o di una dimensione specifica, è consigliabile usare i parametri di query seguenti:

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): filtra le immagini in base alle proporzioni (ad esempio, immagini standard o widescreen)
* [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): filtra le immagini in base al colore dominante o al bianco e nero
* [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): filtra le immagini in base alla durata (ad esempio, immagini individuate da Bing la scorsa settimana)
* [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width): filtrano le immagini in base alla larghezza e all'altezza
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): filtra le immagini in base al contenuto (ad esempio, immagini che mostrano solo il viso di una persona)
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): filtra le immagini in base al tipo (ad esempio, ClipArt, GIF animate o sfondi trasparenti)
* [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): filtra le immagini in base al tipo di licenza associata al sito
* [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): filtra le immagini in base alla dimensione, ad esempio immagini piccole fino a 200x200 pixel

Per ottenere immagini da un dominio specifico, usare l'operatore query [site:](http://msdn.microsoft.com/library/ff795613.aspx).

    > [!NOTE]
    > Responses to queries using the `site:` operator may include adult content regardless of the [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) setting. Only use `site:` if you are aware of the content on the domain.

L'esempio seguente illustra come ottenere immagini di piccole dimensioni da ContosoSailing.com individuate da Bing la scorsa settimana.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Formato della risposta di Ricerca immagini Bing

Il messaggio di risposta da Bing contiene una risposta [Immagini](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) contenente un elenco di immagini che i servizi cognitivi di Azure hanno determinato essere pertinenti alla query. Ogni oggetto [Immagine](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) contenuto nell'elenco include le informazioni seguenti sull'immagine: l'URL, le dimensioni, le proporzioni, il formato di codifica, un URL di un'anteprima dell'immagine e le dimensioni dell'anteprima.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Quando si chiama l'API Ricerca immagini Bing, Bing restituisce un elenco di risultati. L'elenco è un subset del numero totale di risultati pertinenti alla query. Il campo `totalEstimatedMatches` della risposta contiene una stima del numero di immagini disponibili da visualizzare. Per informazioni dettagliate su come Sfogliare le immagini rimanenti, vedere [Paging Images (Paging di immagini)](../paging-images.md).

## <a name="next-steps"></a>Passaggi successivi

Se non si è ancora provato l'API Ricerca immagini Bing, consultare una [Guida introduttiva](../quickstarts/csharp.md). Se si sta cercando un elemento più complesso, provare l'esercitazione per la creazione di un'[app web a pagina singola](../tutorial-bing-image-search-single-page-app.md).
