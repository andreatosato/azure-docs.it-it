---
title: Metodo Evaluate nell'API Academic Knowledge | Microsoft Docs
description: Usare il metodo Evaluate per restituire un set di entità accademiche basate su un'espressione di query in Servizi cognitivi Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 3005ae1f6df042a49db086de4982d8206f6938a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372740"
---
# <a name="evaluate-method"></a>Metodo Evaluate

L'API REST **evaluate** viene usata per restituire un set di entità accademiche basate su un'espressione di query.
<br>

**Endpoint REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>
## <a name="request-parameters"></a>Parametri della richiesta  
NOME     | Valore | Obbligatorio?  | DESCRIZIONE
-----------|-----------|---------|--------
**expr**       | Stringa di testo | Sì | Espressione di query che specifica le entità che devono essere restituite.
**model**      | Stringa di testo | No   | Nome del modello su cui si vuole eseguire la query.  Attualmente il valore predefinito è *latest*.        
**attributes** | Stringa di testo | No <br>Valore predefinito: ID | Elenco delimitato da virgole che specifica i valori di attributo inclusi nella risposta. Per i nomi degli attributi viene fatta distinzione tra maiuscole e minuscole.
**count**        | Number | No <br>Valore predefinito: 10 | Numero di risultati da restituire.
**offset**     | Number |   No <br>Predefinito: 0    | Indice del primo risultato da restituire.
**orderby** |   Stringa di testo | No <br>Valore predefinito: per probabilità decrescente | Nome di un attributo che viene usato per ordinare le entità. Facoltativamente è possibile specificare crescente/decrescente. Il formato è: *name:asc* o *name:desc*.
  
 <br>
## <a name="response-json"></a>Risposta (JSON)
NOME | DESCRIZIONE
-------|-----   
**expr** |  Parametro *expr* della richiesta.
**entities** |  Matrice di 0 o più entità che corrispondono all'espressione di query. Ogni entità contiene il valore della probabilità logaritmica naturale e i valori degli altri attributi richiesti.
**aborted** | True se si è verificato il timeout della richiesta.

<br>
#### <a name="example"></a>Esempio:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>In genere, si otterrà un'espressione da una risposta al metodo **interpret**.  Ma è possibile anche comporre manualmente le espressioni di query (vedere [Sintassi delle espressioni di query](QueryExpressionSyntax.md)).  
  
Usando i parametri *count* e *offset*, un numero elevato di risultati può essere ottenuto senza inviare una singola richiesta che produce una risposta di grandi dimensioni (e potenzialmente lenta).  In questo esempio la richiesta usa l'espressione per la prima interpretazione della risposta dell'API **interpret** come valore *expr*. Il parametro *count=2* specifica che sono stati richiesti 2 risultati delle entità. Il parametro *attributes=Ti,Y,CC,AA.AuN,AA.AuId* indica che il titolo, l'anno, il numero delle citazioni, il nome dell'autore e l'ID dell'autore sono stati richiesti per ogni risultato.  Per un elenco degli attributi, vedere [Attributi di entità](EntityAttributes.md).
  
```JSON
{
  "expr": "Composite(AA.AuN=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -15.08,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "CC": 372,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1968481722
        },
        {
          "AuN": "susan t dumais",
          "AuId": 676500258
        },
        {
          "AuN": "eric horvitz",
          "AuId": 1470530979
        }
      ]
    },
    {
      "logprob": -15.389,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "CC": 237,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1982462162
        },
        {
          "AuN": "christine alvarado",
          "AuId": 2163512453
        },
        {
          "AuN": "mark s ackerman",
          "AuId": 2055132526
        },
        {
          "AuN": "david r karger",
          "AuId": 2012534293
        }
      ]
    }
  ]
}
 ```
 
