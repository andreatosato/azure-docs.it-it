---
title: Peering reti virtuali di Azure | Documentazione Microsoft
description: Informazioni sul peering reti virtuali in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: c20375d42786b817f677be22dee8f4e71e710bc5
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2018
ms.locfileid: "41919823"
---
# <a name="virtual-network-peering"></a>Peering di rete virtuale

Il peering di rete virtuale consente di connettere facilmente due [reti virtuali](virtual-networks-overview.md) di Azure. Dopo avere eseguito il peering, le reti virtuali vengono visualizzate come una sola per scopi di connettività. Il traffico tra macchine virtuali presenti in reti virtuali di cui è stato eseguito il peering viene instradato tramite l'infrastruttura backbone Microsoft, in modo simile all'instradamento del traffico tra macchine virtuali presenti nella stessa rete virtuale, solo tramite indirizzi IP *privati*. Azure supporta:
* Peering reti virtuali - Connessione di reti virtuali entro la stessa area di Azure
* Peering reti virtuali globali - Connessione di reti virtuali in diverse aree di Azure

Il peering reti virtuali, locale o globale, include i vantaggi seguenti:

* Il traffico di rete tra reti virtuali di cui è stato eseguito il peering è privato. Il traffico tra le reti virtuali viene mantenuto nella rete backbone Microsoft. Nella comunicazione tra le reti virtuali non sono necessari gateway, una rete Internet pubblica o la crittografia.
* Connessione a bassa latenza e larghezza di banda elevata tra le risorse in reti virtuali diverse.
* Capacità delle risorse presenti in una rete virtuale di comunicare con le risorse in una rete virtuale diversa, dopo il peering delle reti virtuali.
* Capacità di trasferire dati tra sottoscrizioni di Azure, modelli di distribuzione e aree di Azure.
* Possibilità di eseguire il peering di reti virtuali create tramite il modello di distribuzione Azure Resource Manager o di eseguire il peering di una rete virtuale creata tramite Resource Manager a una rete virtuale creata con il modello di distribuzione classica. Per altre informazioni in proposito, vedere le [informazioni sui modelli di distribuzione di Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Tempo di inattività nullo per le risorse nelle due reti virtuali durante o dopo la creazione del peering.

## <a name="connectivity"></a>Connettività

Dopo avere eseguito il peering tra reti virtuali, le risorse di una delle due reti virtuali possono connettersi direttamente alle risorse nella rete virtuale con peering.

La latenza di rete tra macchine virtuali presenti in reti virtuali di cui è stato eseguito il peering nella stessa area è identica a quella in un'unica rete virtuale. La velocità effettiva della rete si basa sulla larghezza di banda consentita per la macchina virtuale proporzionalmente alle relative dimensioni, senza altre restrizioni alla larghezza di banda consentita nel peering.

Il traffico tra macchine virtuali presenti in reti virtuali di cui è stato eseguito il peering viene direttamente instradato attraverso l'infrastruttura backbone Microsoft, non attraverso un gateway né tramite la rete Internet pubblica.

È possibile applicare gruppi di sicurezza di rete a una delle reti virtuali per bloccare l'accesso alle altre reti virtuali o subnet, se necessario.
Quando si configura il peering di rete virtuale, è possibile aprire o chiudere le regole dei gruppi di sicurezza di rete tra le reti virtuali. Se si apre la connettività completa tra reti virtuali con peering (opzione predefinita), è possibile applicare gruppi di sicurezza di rete a subnet o reti virtuali specifiche per bloccare o negare un accesso specifico. Per altre informazioni sui gruppi di sicurezza di rete, vedere [Panoramica dei gruppi di sicurezza di rete](security-overview.md).

## <a name="service-chaining"></a>Concatenamento dei servizi

È possibile configurare route definite dall'utente che puntano a macchine virtuali di reti virtuali con peering come indirizzo IP dell'*hop successivo* oppure che puntano a gateway di rete virtuali per abilitare il concatenamento dei servizi. Il concatenamento dei servizi consente di indirizzare il traffico da una rete virtuale a un'appliance virtuale o un gateway di rete virtuale in una rete virtuale con peering usando le tabelle di route definite dall'utente.

È possibile distribuire reti hub e spoke in cui la rete virtuale hub può ospitare componenti dell'infrastruttura, ad esempio un'appliance virtuale di rete o un gateway VPN. e tutte le reti virtuali spoke possono eseguire il peering con la rete virtuale dell'hub. Il traffico può fluire attraverso le appliance virtuali di rete o i gateway VPN nella rete virtuale hub. 

Il peering di rete virtuale consente di usare come hop successivo di una route definita dall'utente l'indirizzo IP di una macchina virtuale nella rete virtuale con peering oppure un gateway VPN. Non è tuttavia possibile eseguire il routing tra reti virtuali con una route definita dall'utente specificando un gateway ExpressRoute come tipo di hop successivo. Per altre informazioni sulle route definite dall'utente, vedere [Route definite dall'utente](virtual-networks-udr-overview.md#user-defined). Per informazioni su come creare una topologia di rete hub-spoke, vedere l'articolo sulla [topologia di rete hub-spoke](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="gateways-and-on-premises-connectivity"></a>Gateway e connettività locale

Ogni rete virtuale, che ne sia stato eseguito o meno il peering con un'altra rete virtuale, può comunque avere un proprio gateway e usarlo per connettersi a una rete locale. È possibile anche configurare [connessioni da rete virtuale a rete virtuale](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tramite gateway, anche se è stato eseguito il peering delle reti virtuali.

Quando vengono configurate entrambe le opzioni per la connettività tra reti virtuali, il flusso di traffico tra di esse seguirà la configurazione del peering, ovvero tramite il backbone di Azure.

Dopo aver eseguito il peering delle reti virtuali nella stessa area, gli utenti possono anche configurare il gateway nella rete virtuale con peering come punto di transito verso una rete locale. In questo caso, la rete virtuale che usa un gateway remoto non può avere un proprio gateway. Una rete virtuale può avere un solo gateway, che può essere locale o remoto (nella rete virtuale con peering), come illustrato nell'immagine seguente:

![Peering reti virtuali con transito](./media/virtual-networks-peering-overview/figure04.png)

Il transito gateway non è supportato nella relazione di peering tra reti virtuali create in regioni diverse. Entrambe le reti virtuali nella relazione di peering devono trovarsi nella stessa area per consentire il transito del gateway. Il transito del gateway tra reti virtuali create tramite modelli di distribuzione diversi (Gestione risorse e classico) è supportato solo se il gateway si trova nella rete virtuale (Resource Manager). Per altre informazioni sull'uso di un gateway per la trasmissione, vedere [Configurare un gateway VPN per il transito nel peering di rete virtuale](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Quando viene eseguito il peering di reti virtuali che condividono una singola connessione Azure ExpressRoute, il traffico tra di esse passa attraverso la relazione di peering, ovvero tramite la rete backbone di Azure. È comunque possibile continuare a usare i gateway locali in ogni rete virtuale per connettersi al circuito locale. oppure usare un gateway condiviso e configurare il transito per la connettività locale.

## <a name="troubleshoot"></a>Risolvere problemi

Per verificare un peering di rete virtuale, è possibile [verificare le route valide](diagnose-network-routing-problem.md) per un'interfaccia di rete in una qualsiasi subnet di una rete virtuale. Se esiste un peering di rete virtuale, tutte le subnet all'interno della rete virtuale hanno route con tipo di hop successivo *Peering reti virtuali* per ogni spazio degli indirizzi di ogni rete virtuale con peering.

È anche possibile risolvere i problemi di connettività a una macchina virtuale in una rete virtuale con peering usando il [controllo della connettività](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) di Network Watcher. Il controllo della connettività consente di vedere come avviene il routing del traffico dall'interfaccia di rete della VM di origine all'interfaccia di rete della VM di destinazione.

## <a name="requirements-and-constraints"></a>Requisiti e vincoli

I vincoli seguenti si applicano quando il peering viene eseguito globalmente nelle reti virtuali:
- Le reti virtuali possono trovarsi in qualsiasi area di cloud pubblico di Azure, ma non nei cloud nazionali di Azure.
- Le risorse in una rete virtuale non possono comunicare con l'indirizzo IP front-end di un servizio di bilanciamento del carico interno di Azure nella rete virtuale con peering globale. Il servizio di bilanciamento del carico e le risorse che comunicano con quest'ultimo devono essere nella stessa area.
- Non è possibile usare i gateway remoti o consentire il transito del gateway. Per usare i gateway remoti o per consentire il transito gateway,  le reti virtuali con peering devono trovarsi nella stessa area.

Per altre informazioni su requisiti e vincoli, vedere i [requisiti e i vincoli del peering di rete virtuale](virtual-network-manage-peering.md#requirements-and-constraints). Per altre informazioni sul numero massimo di peering che è possibile creare per una rete virtuale, vedere i [limiti relativi alle reti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). 

## <a name="permissions"></a>Autorizzazioni

Per altre informazioni sulle autorizzazioni necessarie per creare un peering di rete virtuale, vedere le [autorizzazioni per il peering di rete virtuale](virtual-network-manage-peering.md#permissions).

## <a name="pricing"></a>Prezzi

È previsto un addebito nominale per il traffico in ingresso e in uscita che usa una connessione con peering reti virtuali. Per altre informazioni sui prezzi per il peering reti virtuali e per il peering globale di reti virtuali, vedere la [pagina relativa ai prezzi](https://azure.microsoft.com/pricing/details/virtual-network).

Il transito nel gateway è una proprietà del peering che consente a una rete virtuale di usare un gateway VPN in una rete virtuale con peering per la connettività cross-premise o da rete virtuale a rete virtuale. Il traffico che attraversa un gateway remoto in questo scenario è soggetto ai [costi del gateway VPN](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/) e non verranno addebitati [costi per il peering reti virtuali.](https://azure.microsoft.com/pricing/details/virtual-network) Se, ad esempio, la rete virtuale A ha un gateway VPN per la connettività locale e la rete virtuale B è associata alla rete virtuale A con le proprietà appropriate configurate, il traffico dalla rete virtuale B ai dati locali viene addebitato solo in uscita in base ai prezzi per il gateway VPN. Le tariffe del peering reti virtuali non si applicano. Informazioni su come [configurare il transito nel gateway VPN per il peering di rete virtuale.](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="next-steps"></a>Passaggi successivi

* Un peering di reti virtuali viene generato tra reti virtuali create tramite modelli di distribuzione uguali o diversi esistenti in sottoscrizioni uguali o diverse. Completare un'esercitazione per uno degli scenari seguenti:

    |Modello di distribuzione di Azure             | Sottoscrizione  |
    |---------                          |---------|
    |Entrambi Resource Manager              |[Uguale](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Diversa](create-peering-different-subscriptions.md)|
    |Uno di Resource Manager, uno della versione classica  |[Uguale](create-peering-different-deployment-models.md)|
    |                                   |[Diversa](create-peering-different-deployment-models-subscriptions.md)|

* Altre informazioni su come creare una [topologia di rete hub-spoke](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Acquisire informazioni più dettagliate su tutte le [impostazioni per il peering di rete virtuale e su come modificarle](virtual-network-manage-peering.md).
* Ottenere risposte alle domande comuni sul peering reti virtuali e il peering reti virtuali globale nelle [Domande frequenti sul peering reti virtuali](virtual-networks-faq.md#vnet-peering)
