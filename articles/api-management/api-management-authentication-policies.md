---
title: Criteri di autenticazione di Gestione API di Azure | Documentazione Microsoft
description: Informazioni sui criteri di autenticazione disponibili per l'uso in Gestione API di Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4d13d9dbea9da9db5bfe9a9af85fdbf9eab1ae84
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2017
ms.locfileid: "26127760"
---
# <a name="api-management-authentication-policies"></a>Criteri di autenticazione di Gestione API di Azure
Questo argomento fornisce un riferimento per i seguenti criteri di Gestione API. Per informazioni sull'aggiunta e sulla configurazione dei criteri, vedere [Criteri di Gestione API](http://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Criteri di autenticazione  
  
-   [Autenticazione con base](api-management-authentication-policies.md#Basic): consente di eseguire l'autenticazione con un servizio back-end tramite l'autenticazione di base.  
  
-   [Autenticazione con certificato](api-management-authentication-policies.md#ClientCertificate): consente di eseguire l'autenticazione con un servizio back-end tramite certificati client.  
  
##  <a name="Basic"></a> Autenticazione con base  
 Usare il criterio `authentication-basic` per eseguire l'autenticazione con un servizio di back-end tramite l'autenticazione di base. Questo criterio imposta l'intestazione di autorizzazione HTTP sul valore corrispondente alle credenziali specificate nei criteri.  
  
### <a name="policy-statement"></a>Istruzione del criterio  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Esempio  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementi  
  
|NOME|DESCRIZIONE|Obbligatoria|  
|----------|-----------------|--------------|  
|authentication-basic|Elemento radice.|Sì|  
  
### <a name="attributes"></a>Attributi  
  
|NOME|DESCRIZIONE|Obbligatoria|Predefinito|  
|----------|-----------------|--------------|-------------|  
|username|Specifica il nome utente della credenziale di base.|Sì|N/D|  
|password|Specifica la password della credenziale di base.|Sì|N/D|  
  
### <a name="usage"></a>Utilizzo  
 Questo criterio può essere usato nelle [sezioni](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e negli [ambiti](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) del criterio seguenti.  
  
-   **Sezioni del criterio:** in ingresso  
  
-   **Ambiti del criterio:** API  
  
##  <a name="ClientCertificate"></a> Autenticazione con certificato client  
 Usare il criterio `authentication-certificate` per eseguire l'autenticazione con un servizio di back-end tramite il certificato client. Il certificato deve essere prima [installato in Gestione API](http://go.microsoft.com/fwlink/?LinkID=511599) e viene identificato tramite la relativa identificazione personale.  
  
### <a name="policy-statement"></a>Istruzione del criterio  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Esempio  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementi  
  
|NOME|DESCRIZIONE|Obbligatoria|  
|----------|-----------------|--------------|  
|authentication-certificate|Elemento radice.|Sì|  
  
### <a name="attributes"></a>Attributi  
  
|NOME|DESCRIZIONE|Obbligatoria|Predefinito|  
|----------|-----------------|--------------|-------------|  
|thumbprint|Identificazione personale del certificato client.|Sì|N/D|  
  
### <a name="usage"></a>Utilizzo  
 Questo criterio può essere usato nelle [sezioni](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e negli [ambiti](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) del criterio seguenti.  
  
-   **Sezioni del criterio:** in ingresso  
  
-   **Ambiti del criterio:** API  
  

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sull'uso di questi criteri, vedere:

+ [Criteri di Gestione API](api-management-howto-policies.md)
+ [Trasformare le API](transform-api.md)
+ [Informazioni di riferimento sui criteri](api-management-policy-reference.md) per un elenco completo delle istruzioni dei criteri e delle relative impostazioni
+ [Esempi di criteri](policy-samples.md)   
