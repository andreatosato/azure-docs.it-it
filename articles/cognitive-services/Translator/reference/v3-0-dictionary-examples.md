---
title: Metodo di esempi di dizionari dell'API Traduzione testuale Microsoft | Microsoft Docs
description: Usare il metodo esempi di dizionari dell'API Traduzione testuale Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9960f3be42090edaec1df935d70e4c1a0d25b691
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377532"
---
# <a name="text-api-30-dictionary-examples"></a>API per testo 3.0: Esempi di dizionari

Fornisce esempi che illustrano come vengono usati nel contesto i termini nel dizionario. Questa operazione viene usata in parallelo con [Ricerca nel dizionario](.\v3-0-dictionary-lookup.md).

## <a name="request-url"></a>URL richiesta

Inviare una richiesta `POST` a:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>Parametri della richiesta

I parametri di richiesta passati alla stringa di query sono:

<table width="100%">
  <th width="20%">Query parameter (Parametro di query)</th>
  <th>DESCRIZIONE</th>
  <tr>
    <td>api-version</td>
    <td>*Parametro obbligatorio*.<br/>Versione dell'API richiesta dal client. Il valore deve essere `3.0`.</td>
  </tr>
  <tr>
    <td>from</td>
    <td>*Parametro obbligatorio*.<br/>Specifica la lingua del testo di input. La lingua di origine deve essere una delle [lingue supportate](.\v3-0-languages.md) incluse nell'ambito `dictionary`.</td>
  </tr>
  <tr>
    <td>to</td>
    <td>*Parametro obbligatorio*.<br/>Specifica la lingua del testo di output. La lingua di destinazione deve essere una delle [lingue supportate](.\v3-0-languages.md) incluse nell'ambito `dictionary`.</td>
  </tr>
</table>

Le intestazioni della richiesta includono:

<table width="100%">
  <th width="20%">Headers</th>
  <th>DESCRIZIONE</th>
  <tr>
    <td>_Un'intestazione_<br/>_di autorizzazione_</td>
    <td>*Intestazione di richiesta necessaria*.<br/>Vedere le [opzioni disponibili per l'autenticazione](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Intestazione di richiesta necessaria*.<br/>Specifica il tipo di contenuto del payload. I valori possibili sono:`application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Intestazione di richiesta necessaria*.<br/>Lunghezza del corpo della richiesta.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Facoltativo*.<br/>GUID generato dal client che identifica in modo univoco la richiesta. È possibile omettere questa intestazione se nella stringa della query si include l'ID traccia usando un parametro di query denominato `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Corpo della richiesta

Il corpo della richiesta è una matrice JSON. Ogni elemento della matrice è un oggetto JSON con le proprietà seguenti:

  * `Text`: una stringa che specifica il termine da ricercare. Questo deve essere il valore di un campo `normalizedText` dalle traduzioni inverse di una richiesta di [ricerca precedente nel dizionario](.\v3-0-dictionary-lookup.md). Può anche essere il valore del campo `normalizedSource`.

  * `Translation`: una stringa che specifica il testo tradotto restituito in precedenza dall'operazione di [ricerca nel dizionario](.\v3-0-dictionary-lookup.md). Deve essere il valore del campo `normalizedTarget` nell'elenco `translations` della risposta della [ricerca nel dizionario](.\v3-0-dictionary-lookup.md). Il servizio restituirà esempi relativi alla specifica coppia di parole di origine-destinazione.

Un esempio è:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Si applicano le limitazioni seguenti:

* La matrice deve essere composta al massimo da 10 elementi.
* Il valore di testo di un elemento di matrice non può superare 100 caratteri inclusi gli spazi.

## <a name="response-body"></a>Corpo della risposta

Una risposta corretta è una matrice JSON con un risultato per ogni stringa nella matrice di input. Un oggetto risultato include le proprietà seguenti:

  * `normalizedSource`: una stringa che fornisce la forma normalizzata del termine origine. In genere, dovrebbe essere identica al valore del campo `Text` nell'indice dell'elenco corrispondente nel corpo della richiesta.
    
  * `normalizedTarget`: una stringa che fornisce la forma normalizzata del termine di destinazione. In genere, dovrebbe essere identica al valore del campo `Translation` nell'indice dell'elenco corrispondente nel corpo della richiesta.
  
  * `examples`: un elenco di esempi per la coppia termine origine-termine di destinazione. Ogni elemento dell'elenco è un oggetto con le proprietà seguenti:

    * `sourcePrefix`: la stringa da concatenare _prima_ del valore `sourceTerm` per formare un esempio completo. Non aggiungere uno spazio perché è già presente. Questo valore può essere una stringa vuota.

    * `sourceTerm`: una stringa uguale al termine attuale ricercato. La stringa viene aggiunta con `sourcePrefix` e `sourceSuffix` per formare l'esempio completo. Il valore è separato e pertanto può essere contrassegnato in un'interfaccia utente, ad esempio, con il grassetto.

    * `sourceSuffix`: la stringa da concatenare _dopo_ il valore `sourceTerm` per formare un esempio completo. Non aggiungere uno spazio perché è già presente. Questo valore può essere una stringa vuota.

    * `targetPrefix`: una stringa simile a `sourcePrefix` ma per la destinazione.

    * `targetTerm`: una stringa simile a `sourceTerm` ma per la destinazione.

    * `targetSuffix`: una stringa simile a `sourceSuffix` ma per la destinazione.

    > [!NOTE]
    > Se non ci sono esempi nel dizionario, la risposta è 200 (OK) ma l'elenco `examples` è vuoto.

## <a name="examples"></a>Esempi

Questo esempio illustra come ricercare esempi per la coppia formata dal termine inglese `fly` e la sua traduzione in spagnolo `volar`.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

Il corpo della risposta (abbreviato per maggiore chiarezza) è:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
