---
title: Come usare il controllo mappa di Mappe di Azure | Microsoft Docs
description: Informazioni su come usare la libreria JavaScript lato client del controllo mappa di Mappe di Azure.
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 619dda44b5ad74ba1dcb62cfb3318687aa6ec4d5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43340136"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Come usare il controllo mappa di Mappe di Azure
La libreria JavaScript lato client del controllo mappa consente di eseguire il rendering delle mappe e delle funzionalità incorporate di Mappe di Azure nelle applicazioni Web e per dispositivi mobili. 

## <a name="create-a-new-map-in-a-web-page"></a>Creare una nuova mappa in una pagina Web

È possibile incorporare una mappa in una pagina Web usando la libreria JavaScript lato client del controllo mappa.

1. Creare un nuovo file con il nome MapSearch.html.

2. Aggiungere i riferimenti alle origini del foglio di stile e dello script di Mappe di Azure all'elemento `<head>` del file:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Per eseguire il rendering di una nuova mappa nel browser, aggiungere un riferimento **#map** nell'elemento `<style>`.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Per inizializzare il controllo mappa, definire una nuova sezione nel corpo HTML e creare uno script. Usare la chiave dell'account Mappe di Azure nello script. Se è necessario creare un account o trovare la chiave, vedere [Come gestire l'account e le chiavi dell'account Mappe di Azure](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Aprire il file nel Web browser e visualizzare la mappa di cui è stato eseguito il rendering.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo è stato illustrato come creare una mappa di base con la chiave di Mappe di Azure. Per altri esempi di codice da aggiungere alle mappe, vedere gli articoli seguenti: 

* [Creare una mappa](map-create.md)
* [Scegliere uno stile mappa](choose-map-style.md)
