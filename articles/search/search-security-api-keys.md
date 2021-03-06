---
title: Creare, gestire e proteggere chiavi API amministratore e query per la ricerca di Azure | Microsoft Docs
description: Le chiavi API controllano l'accesso all'endpoint di servizio. Le chiavi amministratore concedono l'accesso in scrittura. Le chiavi di query possono essere create per l'accesso in sola lettura.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: heidist
ms.openlocfilehash: 2ec720f26cfbadb9963ff3991ad1795c9b30c136
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284982"
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>Creare e gestire chiavi API per un servizio di ricerca di Azure

Tutte le richieste indirizzate a un servizio di ricerca necessitano di una chiave API di sola lettura generata specificamente per il proprio servizio. Questa chiave API è l'unico meccanismo di autenticazione dell'accesso all'endpoint di servizio di ricerca e deve essere inclusa in ogni richiesta. In [soluzioni REST](search-get-started-nodejs.md#update-the-configjs-with-your-search-service-url-and-api-key), la chiave api viene in genere specificata in un'intestazione della richiesta. In [soluzioni .NET](search-howto-dotnet-sdk.md#core-scenarios), una chiave è spesso specificata come un'impostazione di configurazione e quindi passata come [Credenziali](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) (chiave di amministrazione) o [SearchCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.searchcredentials) (chiave di query) nel [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient).

Le chiavi vengono create con il servizio di ricerca durante il provisioning del servizio. È possibile visualizzare e ottenere valori di chiave nel [portale di Azure](https://portal.azure.com).

![Pagina del portale, sezione Impostazioni, Chiavi](media/search-manage/azure-search-view-keys.png)

## <a name="what-is-an-api-key"></a>Definizione di una chiave API

Una chiave API è una stringa composta da lettere e numeri generati casualmente. Tramite [autorizzazioni basate sui ruoli](search-security-rbac.md) è possibile eliminare o leggere le chiavi, ma non è possibile sostituire una chiave con una password definita dall'utente o usare Active Directory come metodologia di autenticazione principale per l'accesso alle operazioni di ricerca. 

Per accedere al servizio di ricerca vengono usati due tipi di chiavi: amministratore (di lettura-scrittura) e query (di sola lettura).

|Chiave|DESCRIZIONE|Limiti|  
|---------|-----------------|------------|  
|Admin|Concede diritti completi a tutte le operazioni, inclusa la possibilità di gestire il servizio, creare ed eliminare indici, indicizzatori e origini dati.<br /><br /> Due chiavi amministratore, chiamate chiave *primaria* e *secondaria* nel portale, vengono generate quando il servizio viene creato e possono essere generate di nuovo singolarmente su richiesta. Avere due chiavi consente di eseguire il rollover di una chiave mentre si usa la seconda per l'accesso continuo al servizio.<br /><br /> Le chiavi amministratore vengono specificate solo nelle intestazioni delle richieste HTTP. Non è possibile inserire un elemento api-key amministratore in un URL.|Un massimo di 2 per servizio|  
|Query|Concede l'accesso in sola lettura agli indici e ai documenti e viene in genere distribuita alle applicazioni client che inviano richieste di ricerca.<br /><br /> Le chiavi di query vengono create su richiesta. È possibile crearle manualmente nel portale o a livello di codice tramite l'[API REST di gestione](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Le chiavi di query possono essere specificate nell'intestazione di una richiesta HTTP per un'operazione di ricerca o suggerimento. In alternativa, è possibile passare una chiave di query come parametro in un URL. A seconda di come l'applicazione client formula la richiesta, può risultare più semplice passare la chiave come parametro di query:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11&api-key=[query key]`|50 per servizio|  

 Non esiste alcuna distinzione visiva tra una chiave amministratore o una chiave di query. Entrambe le chiavi sono stringhe composte da 32 caratteri alfanumerici generati in modo casuale. Se si è persa traccia del tipo di chiave specificato nell'applicazione, è possibile [controllare i valori delle chiavi nel portale](https://portal.azure.com) o usare l'[API REST](https://docs.microsoft.com/rest/api/searchmanagement/) per restituire il valore e il tipo di chiave.  

> [!NOTE]  
>  Passare dati sensibili, ad esempio un elemento `api-key`, nell'URI della richiesta è considerato una procedura di sicurezza non ottimale. Per questo motivo, Ricerca di Azure accetta solo una chiave di query come `api-key` nella stringa di query ed è consigliabile evitare di eseguire questa operazione, a meno che i contenuti dell'indice non debbano essere disponibili pubblicamente. Come regola generale, è consigliabile passare l'elemento `api-key` come intestazione della richiesta.  

## <a name="find-api-keys-for-your-service"></a>Trovare chiavi API per il servizio

È possibile ottenere le chiavi di accesso nel portale o tramite l'[API REST di gestione](https://docs.microsoft.com/rest/api/searchmanagement/). Per altre informazioni, vedere [Manage admin and query api-keys](search-security-api-keys.md) (Gestione di chiavi API query e amministratore).

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Elencare i [servizi di ricerca](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) per la sottoscrizione.
3. Selezionare il servizio nella pagina dei servizi e trovare **Impostazioni** >**Chiavi** per visualizzare le chiavi amministratore e di query.

![Pagina del portale, sezione Impostazioni, Chiavi](media/search-security-overview/settings-keys.png)

## <a name="regenerate-admin-keys"></a>Riscrivere una chiave amministratore

Per ogni servizio vengono create due chiavi amministratore in modo che sia possibile eseguire il rollover della chiave primaria, usando la chiave secondaria per l'accesso continuo.

Riscrivendo contemporaneamente sia la chiave primaria che quella secondaria, qualsiasi applicazione che usa due chiavi per l'accesso alle operazioni del servizio non sarà in grado di accedere al servizio.

1. Nella pagina **Impostazioni** >**Chiavi**, copiare la chiave secondaria.
2. Per tutte le applicazioni, aggiornare le impostazioni relative alle chiavi API per usare la chiave secondaria.
3. Riscrivere la chiave primaria.
4. Aggiornare tutte le applicazioni affinché usino la nuova chiave primaria.

## <a name="secure-api-keys"></a>Proteggere le chiavi API
La sicurezza delle chiavi viene garantita limitando l'accesso tramite il portale o le interfacce di Resource Manager (PowerShell o interfaccia della riga di comando). Come indicato, gli amministratori delle sottoscrizioni possono visualizzare e rigenerare tutte le chiavi API. Per precauzione, esaminare le assegnazioni di ruolo per sapere chi ha accesso alle chiavi amministratore.

+ Nella dashboard del servizio, fare clic su **Controllo di accesso (IAM)** per visualizzare le assegnazioni di ruolo per il servizio.

I membri dei ruoli seguenti possono visualizzare e ricreare una chiave: proprietario, collaboratore, [collaboratore Servizio di ricerca](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Per gli accessi in base al ruolo sui risultati della ricerca è possibile creare filtri di sicurezza per vagliare i risultati in base all'identità, rimuovendo eventuali documenti a cui il richiedente non dovrebbe avere accesso. Per altre informazioni, vedere [Filtri di sicurezza](search-security-trimming-for-azure-search.md) e [Secure with Active Directory](search-security-trimming-for-azure-search-with-aad.md) (Protezione mediante Active Directory).

## <a name="see-also"></a>Vedere anche 

+ [Controllo degli accessi in base al ruolo in Ricerca di Azure](search-security-rbac.md)
+ [Gestire usando PowerShell](search-manage-powershell.md) 
+ [Articolo su prestazioni e ottimizzazione](search-performance-optimization.md)