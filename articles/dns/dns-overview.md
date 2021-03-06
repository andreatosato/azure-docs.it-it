---
title: Che cos'è DNS di Azure?
description: Panoramica del servizio di hosting DNS in Microsoft Azure. Ospitare il dominio in Microsoft Azure.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: e3e04bf7e35b22a56465810f476323ed217e047a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967626"
---
# <a name="what-is-azure-dns"></a>Che cos'è DNS di Azure?

DNS di Azure è un servizio di hosting per i domini DNS, che fornisce la risoluzione dei nomi usando l'infrastruttura di Microsoft Azure. Ospitando i domini in Azure, è possibile gestire i record DNS usando le stesse credenziali, API, strumenti e fatturazione come per gli altri servizi Azure.

Non è possibile usare DNS di Azure per acquistare un nome di dominio. Per una tariffa annuale, è possibile acquistare un nome di dominio usando [App Web di Azure](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) o un registrar di nomi di dominio di terze parti. I domini possono quindi essere ospitati in DNS di Azure per la gestione dei record. Per informazioni dettagliate, vedere [Delegare un dominio a DNS di Azure](dns-domain-delegation.md) .

DNS di Azure offre le funzionalità seguenti:

## <a name="reliability-and-performance"></a>Affidabilità e prestazioni

I domini DNS nel servizio DNS di Azure sono ospitati nella rete globale di Azure dei server dei nomi DNS. DNS di Azure usa le reti Anycast in modo che ogni query DNS riceva una risposta dal server DNS disponibile più vicino. Ciò consente di accelerare le prestazioni e ottenere la disponibilità elevata per il dominio.

## <a name="security"></a>Sicurezza

Il servizio DNS di Azure è basato su Azure Resource Manager. È pertanto possibile ottenere funzionalità di Resource Manager quali:

* [Controllo degli accessi in base al ruolo](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control): per controllare gli utenti autorizzati ad accedere ad azioni specifiche per l'organizzazione.

* [Log attività](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs): per monitorare il modo in cui un utente dell'organizzazione ha modificato una risorsa o per trovare un errore durante la risoluzione dei problemi.

* [Blocco delle risorse](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources): per bloccare una sottoscrizione, una risorsa o un gruppo di risorse in modo da impedire che altri utenti nell'organizzazione modifichino o eliminino accidentalmente risorse strategiche.

Per altre informazioni, vedere [Come proteggere le zone e i record DNS](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Semplicità d'uso

Il servizio DNS di Azure consente di gestire i record DNS per i servizi di Azure, nonché di garantire il servizio DNS alle risorse esterne. DNS di Azure è integrato nel portale di Azure e usa le stesse credenziali, lo stesso contratto di supporto e gli stessi metodi di fatturazione di altri servizi di Azure. 

La fatturazione di DNS di Azure è basata sul numero di zone DNS ospitate in Azure e sul numero di query DNS. Per altre informazioni sui prezzi, vedere [Prezzi di DNS di Azure](https://azure.microsoft.com/pricing/details/dns/).

I domini e i record possono essere gestiti tramite il portale di Azure, i cmdlet di Azure PowerShell e l'interfaccia della riga di comando di Azure. Le applicazioni che richiedono la gestione automatizzata di DNS possono essere integrate con il servizio tramite l'API REST e gli SDK.

## <a name="customizable-virtual-networks-with-private-domains"></a>Reti virtuali personalizzabili con domini privati

DNS di Azure supporta anche i domini DNS privati, attualmente disponibili in anteprima pubblica. Questo consente di usare nomi di dominio personalizzati nelle reti private virtuali, anziché i nomi forniti da Azure attualmente disponibili.

Per altre informazioni, vedere [Uso di DNS di Azure per i domini privati](private-dns-overview.md).

## <a name="alias-records"></a>Record alias

DNS di Azure supporta i set di record alias. È possibile usare un set di record alias per fare riferimento a una risorsa di Azure, come ad esempio un indirizzo IP pubblico di Azure o un profilo di Gestione traffico. Se l'indirizzo IP della risorsa sottostante viene modificato, il set di record alias si aggiorna automaticamente durante la risoluzione DNS. Il set di record alias fa riferimento all'istanza del servizio e l'istanza del servizio è associata a un indirizzo IP. 

Inoltre, ora è possibile indirizzare il dominio radice o di tipo naked (ad esempio, contoso.com) a un profilo di Gestione traffico usando un record alias.

Per altre informazioni, vedere la [panoramica dei record alias DNS di Azure](dns-alias.md).


## <a name="next-steps"></a>Passaggi successivi

* Per informazioni sui record e le zone DNS, vedere [Panoramica delle zone e dei record DNS](dns-zones-records.md).

* Per informazioni su come creare una zona in DNS di Azure, vedere [Creare una zona DNS](./dns-getstarted-create-dnszone-portal.md).

* Per domande frequenti sul servizio DNS di Azure, vedere [Domande frequenti su DNS di Azure](dns-faq.md).

