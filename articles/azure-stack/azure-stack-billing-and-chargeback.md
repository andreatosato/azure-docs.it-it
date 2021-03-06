---
title: La fatturazione dei clienti e il Chargeback In Azure Stack | Microsoft Docs
description: Informazioni su come recuperare le informazioni sull'utilizzo di risorse di Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: fcfd5d4e9e2f30d769818df29cf8a76bd9113d4f
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632411"
---
# <a name="usage-and-billing-in-azure-stack"></a>Informazioni sull'utilizzo e fatturazione in Azure Stack

Questo articolo descrive come gli utenti di Azure Stack vengono fatturati per l'utilizzo delle risorse. Per altre modalità di accesso per l'analitica e carica torna le informazioni di fatturazione.

Azure Stack raccoglie e raggruppa i dati di utilizzo per le risorse usate. Azure Stack inoltra quindi i dati di Azure Commerce. Commerce di Azure prevede la fatturazione per l'utilizzo di Azure Stack esattamente come lo sarebbe addebito per l'utilizzo di Azure.

È anche possibile ottenere i dati di utilizzo e l'esportazione per il proprio fatturazione o chargeback eseguire il sistema utilizzando un adattatore di fatturazione, o esportarli in un strumento di business intelligence come Microsoft Power BI.


## <a name="usage-pipeline"></a>Pipeline di utilizzo

Ogni provider di risorse in Azure Stack invia i dati di utilizzo per ogni utilizzo delle risorse. Periodicamente il servizio di utilizzo (su base oraria e giornaliera) aggrega i dati di utilizzo e lo archivia nel database di utilizzo. Gli operatori di Stack e gli utenti di Azure possono accedere ai dati sull'utilizzo archiviati tramite le API di utilizzo risorse di Azure Stack. 

Se hai [registrata l'istanza di Azure Stack con Azure](azure-stack-register.md), Azure Stack è configurato per inviare i dati di utilizzo per e-Commerce di Azure. Dopo aver caricati i dati in Azure, è possibile accedervi tramite il portale di fatturazione o API di utilizzo di risorse di Azure. Vedere la [segnalazione errori dati di utilizzo](azure-stack-usage-reporting.md) articolo per informazioni più su quale tipo di utilizzo dei dati viene segnalati ad Azure.  

L'immagine seguente mostra i componenti chiave della pipeline di utilizzo: 

![Pipeline di utilizzo](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Le informazioni sull'utilizzo è possibile reperire e in che modo?

Provider di Azure Stack Resource (ad esempio calcolo, archiviazione e rete) generano dati sull'utilizzo a intervalli di ore per ogni sottoscrizione. I dati di utilizzo contengono informazioni relative alla risorsa usata come nome della risorsa, sottoscrizione usata e quantità utilizzata. Per altre informazioni sulle risorse ID misuratori, vedere la [domande frequenti sull'API di utilizzo](azure-stack-usage-related-faq.md) articolo.

Dopo aver raccolto i dati di utilizzo, si tratta [segnalato ad Azure](azure-stack-usage-reporting.md) per generare una distinta, che può essere visualizzata tramite il portale di fatturazione di Azure. 

> [!NOTE]  
> Segnalazione errori dati di utilizzo non è necessaria per Azure Stack Development Kit e per gli utenti del sistema integrato Azure Stack che acquistano una licenza in base al modello di capacità. Per altre informazioni sulla gestione delle licenze in Azure Stack, vedere la [creazione di pacchetti e prezzi](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) foglio dati.

Il portale di fatturazione di Azure Mostra i dati di utilizzo per le risorse addebitabili. Oltre alle risorse addebitabili, Azure Stack consente di acquisire dati di utilizzo per un set più ampio di risorse, che è possibile accedere nel proprio ambiente di Azure Stack tramite le API REST o PowerShell. Operatori di Azure Stack possono ottenere i dati di utilizzo per tutte le sottoscrizioni utente. Singoli utenti possono ottenere solo i dettagli sull'uso. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Utilizzo di creazione di report per multi-tenant i provider di servizi Cloud

Un tenant Cloud Service Provider (CSP) che dispone di molti clienti che usano Azure Stack potrebbe voler segnalare ogni utilizzo del cliente separatamente, in modo che il provider, è possibile ricaricare l'utilizzo di diverse sottoscrizioni di Azure. 

Ogni cliente dovrà avere la propria identità rappresentata da un altro tenant di Azure Active Directory (Azure AD). Azure Stack supporta assegnando una sottoscrizione di CSP per ogni tenant di Azure AD. È possibile aggiungere i tenant e le sottoscrizioni per la registrazione di Azure Stack base. Viene eseguita la registrazione di base per tutti gli stack di Azure. Se una sottoscrizione non è registrata per un tenant, l'utente può comunque usare Azure Stack e il loro utilizzo verrà inviato per la sottoscrizione usata per la registrazione di base. 


## <a name="next-steps"></a>Passaggi successivi

[Registrare con Azure Stack](azure-stack-registration.md)

[Segnalare i dati di utilizzo di Azure Stack in Azure](azure-stack-usage-reporting.md)

[Utilizzo del provider di risorse API](azure-stack-provider-resource-api.md)

[Utilizzo delle risorse API tenant](azure-stack-tenant-resource-usage-api.md)

[Domande frequenti relative all'uso](azure-stack-usage-related-faq.md)