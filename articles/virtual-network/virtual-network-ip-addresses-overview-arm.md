---
title: Tipi di indirizzo IP in Azure | Documentazione Microsoft
description: Informazioni sugli indirizzi IP pubblici e privati in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: jdial
ms.openlocfilehash: 65dc420b6832b7b0a4cf14d63203d4c66e2a4254
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993596"
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>Tipi di indirizzi IP e metodi di allocazione in Azure

È possibile assegnare gli indirizzi IP alle risorse di Azure per comunicare con altre risorse di Azure, con la rete locale e con Internet. In Azure è possibile usare due tipi di indirizzi IP:

* **Indirizzi IP pubblici**: usati per la comunicazione con Internet, compresi i servizi pubblici di Azure.
* **Indirizzi IP privati**: usati per la comunicazione all'interno di una rete virtuale di Azure e della rete locale quando si usa un gateway VPN o un circuito ExpressRoute per estendere la rete ad Azure.

È anche possibile creare un intervallo contiguo di indirizzi IP pubblici statici tramite un prefisso IP pubblico. [Informazioni sui prefissi IP pubblici.](public-ip-address-prefix.md)

> [!NOTE]
> Azure offre due modelli di distribuzione per creare e usare le risorse: [Gestione risorse e la distribuzione classica](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  Questo articolo illustra l'uso del modello di distribuzione Resource Manager che Microsoft consiglia di usare invece del [modello di distribuzione classica](virtual-network-ip-addresses-overview-classic.md) per le distribuzioni più recenti.
> 

Se si ha familiarità con il modello di distribuzione classica, verificare le [differenze tra gli indirizzi IP nella versione classica e in Resource Manager](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Indirizzi IP pubblici

Gli indirizzi IP pubblici consentono la comunicazione in ingresso dalle risorse Internet alle risorse di Azure, nonché la comunicazione in uscita dalle risorse di Azure a Internet e servizi pubblici di Azure con un indirizzo IP assegnato alla risorsa. L'indirizzo è dedicato alla risorsa finché non si annulla l'assegnazione. Se a una risorsa non è assegnato un indirizzo IP pubblico, la comunicazione in uscita dalla risorsa a Internet è comunque consentita, ma Azure assegna dinamicamente un indirizzo IP disponibile non dedicato alla risorsa. Per altre informazioni sulle connessioni in uscita, vedere [Informazioni sulle connessioni in uscita in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

In Gestione risorse di Azure un [indirizzo IP pubblico](virtual-network-public-ip-address.md) è una risorsa che ha proprietà specifiche. Alcune delle risorse a cui è possibile associare una risorsa indirizzo IP pubblico sono:

* Interfacce di rete di macchina virtuale
* Servizi di bilanciamento del carico con connessione Internet
* Gateway VPN
* Gateway di applicazione

### <a name="ip-address-version"></a>Versione dell'indirizzo IP

Gli indirizzi IP pubblici vengono creati con un indirizzo IPv4 o IPv6. Gli indirizzi IPv6 pubblici possono essere assegnati solo a servizi di bilanciamento del carico con connessione Internet.

### <a name="sku"></a>SKU

Gli indirizzi IP pubblici vengono creati con uno degli SKU seguenti:

>[!IMPORTANT]
> Per le risorse di bilanciamento del carico e IP pubblico è necessario usare SKU corrispondenti. Non è possibile avere una combinazione di risorse SKU Basic e Standard. Non è possibile collegare le macchine virtuali autonome, le macchine virtuali in una risorsa del set di disponibilità, o una risorsa di un set di scalabilità di macchina virtuale per entrambi gli SKU contemporaneamente.  Per i nuovi progetti è consigliabile prendere in considerazione l'uso di risorse SKU Standard.  Per informazioni dettagliate, consultare [Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="basic"></a>Basic

Tutti gli indirizzi IP pubblici creati prima dell'introduzione degli SKU sono indirizzi IP pubblici con SKU Basic. Con l'introduzione degli SKU, è possibile specificare lo SKU per l'indirizzo IP pubblico. Gli indirizzi con SKU Basic sono:

- Assegnati con il metodo di allocazione statica o dinamica.
- Sono aperti per impostazione predefinita.  I gruppi di sicurezza di rete sono consigliati ma facoltativi per limitare il traffico in ingresso o in uscita.
- Assegnati a qualsiasi risorsa di Azure a cui può essere assegnato un indirizzo IP pubblico, ad esempio interfacce di rete, gateway VPN e servizi di bilanciamento del carico con connessione Internet.
- Assegnabili a una zona specifica.
- Senza ridondanza della zona. Per altre informazioni sulle zone di disponibilità, vedere [Panoramica delle zone di disponibilità](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="standard"></a>Standard

Gli indirizzi IP pubblici con SKU Standard sono:

- Assegnati solo con il metodo di allocazione statica.
- Sono protetti per impostazione predefinita e chiusi al traffico in ingresso. È necessario inserire esplicitamente in un elenco di elementi consentiti il traffico di rete in ingresso con un [gruppo di sicurezza di rete](security-overview.md#network-security-groups).
- Sono assegnati a interfacce di rete o servizi di bilanciamento del carico Standard. Per altre informazioni sui servizi di bilanciamento del carico Standard di Azure, vedere [Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Con ridondanza della zona per impostazione predefinita. Può essere creato a livello di zona ed è garantito in una zona di disponibilità specifica. Per altre informazioni sulle zone di disponibilità, vedere [Panoramica delle zone di disponibilità](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e [Load Balancer Standard e zone di disponibilità](../load-balancer/load-balancer-standard-availability-zones.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
 
> [!NOTE]
> La comunicazione con una risorsa SKU Standard non riesce finché non si crea e si associa un [gruppo di sicurezza di rete](security-overview.md#network-security-groups) e si consente in modo esplicito il traffico in ingresso.

### <a name="allocation-method"></a>Metodo di allocazione

Gli indirizzi IP di SKU sia Standard che Basic supportano il metodo di allocazione *statica*.  Alla risorsa viene assegnato un indirizzo IP al momento della creazione e l'indirizzo IP viene rilasciato quando la risorsa viene eliminata.

Gli indirizzi IP pubblici di SKU Basic supportano anche il metodo di allocazione *dinamica*, ovvero l'impostazione predefinita se non viene specificato il metodo di allocazione.  La selezione del metodo di allocazione *dinamica* per una risorsa indirizzo IP pubblica Basic significa che l'indirizzo IP **non** è allocato al momento della creazione della risorsa.  L'indirizzo IP pubblico viene allocato quando si associa l'indirizzo IP pubblico a una macchina virtuale o quando si posiziona la prima istanza di macchina virtuale nel pool back-end di un servizio di bilanciamento del carico Basic.   L'indirizzo IP viene rilasciato quando si arresta o si elimina la risorsa.  Dopo essere stato rilasciato dalla risorsa A, ad esempio, l'indirizzo IP può essere assegnato a un'altra risorsa. Se l'indirizzo IP viene assegnato a un'altra risorsa mentre la risorsa A è arrestata, quando si riavvia la risorsa A, viene assegnato un indirizzo IP diverso. Se si modifica il metodo di allocazione di una risorsa indirizzo IP pubblico Basic da *statica* a *dinamica*, l'indirizzo viene rilasciato. Per assicurare che l'indirizzo IP per la risorsa associata rimanga invariato, è possibile impostare in modo esplicito il metodo di allocazione su *statico*. Un indirizzo IP statico viene assegnato immediatamente.

> [!NOTE]
> Anche quando si imposta il metodo di allocazione su *statico*, non è possibile specificare l'indirizzo IP effettivo assegnato alla risorsa indirizzo IP pubblico. Azure assegna l'indirizzo IP da un pool di indirizzi IP disponibili nella località di Azure in cui viene creata la risorsa.
>

Gli indirizzi IP pubblici statici sono comunemente usati negli scenari seguenti:

* Quando è necessario aggiornare le regole del firewall per comunicare con le risorse di Azure.
* La risoluzione del nome DNS, in cui una modifica dell'indirizzo IP richiederebbe l'aggiornamento dei record A.
* Le risorse di Azure comunicano con altri servizi o altre app che usano il modello di sicurezza basato su indirizzi IP.
* Si usano certificati SSL collegati a un indirizzo IP.

> [!NOTE]
> Azure assegna gli indirizzi IP pubblici da un intervallo univoco per ogni area del cloud di Azure. È possibile scaricare l'elenco degli intervalli (prefissi) per i cloud [Pubblico](https://www.microsoft.com/download/details.aspx?id=56519), [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [Cina](https://www.microsoft.com/download/details.aspx?id=57062) e [Germania](https://www.microsoft.com/download/details.aspx?id=57064) di Azure.
>

### <a name="dns-hostname-resolution"></a>Risoluzione del nome host DNS
È possibile specificare un'etichetta del nome di dominio DNS per una risorsa IP pubblica, che crea un mapping per *etichettanomedominio*.*località*.cloudapp.azure.com per l'indirizzo IP pubblico nei server DNS gestiti di Azure. Se si crea una risorsa IP pubblica con **contoso** come *etichettanomedominio* nella *località* di Azure **Stati Uniti occidentali**, ad esempio, il nome di dominio completo (FQDN) **contoso.westus.cloudapp.azure.com** viene risolto nell'indirizzo IP pubblico della risorsa. È possibile usare il nome di dominio completo per creare un record CNAME di dominio personalizzato che punta all'indirizzo IP pubblico in Azure. Anziché (oppure oltre a) usare l'etichetta del nome DNS con il suffisso predefinito, è possibile usare il servizio DNS di Azure per configurare un nome DNS con un suffisso personalizzato che viene risolto nell'indirizzo IP pubblico. Per altre informazioni, vedere [Usare DNS di Azure con un indirizzo IP pubblico di Azure](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).

> [!IMPORTANT]
> Ogni etichetta di nome di dominio creata deve essere univoca nella relativa posizione di Azure.  
>

### <a name="virtual-machines"></a>Macchine virtuali

È possibile associare un indirizzo IP pubblico a una macchina virtuale [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) assegnandola alla relativa **interfaccia di rete**. A una macchina virtuale è possibile assegnare un indirizzo IP pubblico statico o dinamico. Per altre informazioni sull'assegnazione di indirizzi IP all'interfaccia di rete, vedere [Aggiungere, modificare o rimuovere indirizzi IP per un'interfaccia di rete di Azure](virtual-network-network-interface-addresses.md).

### <a name="internet-facing-load-balancers"></a>Servizi di bilanciamento del carico con connessione Internet

È possibile associare un indirizzo IP pubblico creato con uno dei due [SKU](#SKU) a un'istanza di [Azure Load Balancer](../load-balancer/load-balancer-overview.md) assegnandolo alla configurazione **front-end** del servizio di bilanciamento del carico. L'indirizzo IP pubblico viene usato come indirizzo IP virtuale (indirizzo VIP) di bilanciamento del carico. A un servizio di bilanciamento del carico front-end è possibile assegnare un indirizzo IP pubblico statico o dinamico. È anche possibile assegnare più indirizzi IP pubblici a un front-end del servizio di bilanciamento del carico, consentendo così scenari con [più indirizzi VIP](../load-balancer/load-balancer-multivip-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) , ad esempio un ambiente multi-tenant con siti Web basati su SSL. Per altre informazioni sugli SKU di Azure Load Balancer, vedere [Azure load balancer standard SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (SKU Standard di Azure Load Balancer).

### <a name="vpn-gateways"></a>Gateway VPN

Un [gateway VPN di Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) connette una rete virtuale di Azure ad altre reti virtuali o locali di Azure. Un indirizzo IP pubblico viene assegnato al gateway VPN per abilitarne la comunicazione con la rete remota. A un gateway VPN è possibile assegnare solo un indirizzo IP pubblico *dinamico* Basic.

### <a name="application-gateways"></a>Gateway di applicazione

È possibile associare un indirizzo IP pubblico a un [gateway applicazione](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)di Azure, assegnandolo alla configurazione **front-end** del gateway. Questo indirizzo IP pubblico viene usato come indirizzo VIP con carico bilanciato. Alla configurazione front-end di un gateway applicazione è possibile assegnare solo un indirizzo IP pubblico *dinamico* Basic.

### <a name="at-a-glance"></a>Riepilogo
La tabella seguente illustra la proprietà specifica tramite la quale un indirizzo IP pubblico può essere associato a una risorsa di livello superiore e i metodi di allocazione possibili (dinamici o statici) utilizzabili.

| Risorse di livello superiore | Associazione di indirizzi IP | Dinamico | statico |
| --- | --- | --- | --- |
| Macchina virtuale |interfaccia di rete |Yes |Yes |
| Servizio di bilanciamento del carico con connessione Internet |Configurazione front-end |Yes |Yes |
| gateway VPN |Configurazione IP del gateway |Yes |No  |
| gateway applicazione |Configurazione front-end |Yes |No  |

## <a name="private-ip-addresses"></a>Indirizzi IP privati
Gli indirizzi IP privati consentono alle risorse di Azure di comunicare con altre risorse in una [rete virtuale](virtual-networks-overview.md) o in una rete locale tramite un gateway VPN o un circuito ExpressRoute, senza usare un indirizzo IP raggiungibile tramite Internet.

Nel modello di distribuzione Azure Resource Manager un indirizzo IP privato è associato ai tipi di risorse di Azure seguenti:

* Interfacce di rete di macchina virtuale
* Servizi di bilanciamento del carico interno
* Gateway di applicazione

### <a name="ip-address-version"></a>Versione dell'indirizzo IP

Gli indirizzi IP privati vengono creati con un indirizzo IPv4 o IPv6. Gli indirizzi IPv6 privati possono essere assegnati solo con il metodo di allocazione dinamica. Non è possibile comunicare tra gli indirizzi IPv6 privati in una rete virtuale. Per la comunicazione in ingresso verso un indirizzo IPv6 privato da Internet, è possibile usare un servizio di bilanciamento del carico con connessione Internet. Per informazioni dettagliate, vedere [Creare un servizio di bilanciamento del carico Internet con IPv6](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="allocation-method"></a>Metodo di allocazione

Un indirizzo IP privato viene allocato dall'intervallo di indirizzi della subnet della rete virtuale in cui è distribuita una risorsa. Azure riserva i primi quattro indirizzi dell'intervallo di indirizzi di ogni subnet, che non possono quindi essere assegnati alle risorse. Se l'intervallo di indirizzi della subnet è 10.0.0.0/16, ad esempio, gli indirizzi 10.0.0.0-10.0.0.3 non possono essere assegnati alle risorse. Gli indirizzi IP dell'intervallo di indirizzi della subnet possono essere assegnati a una sola risorsa alla volta. 

Un indirizzo IP privato può essere allocato con due metodi.

- **Dinamico**: Azure assegna l'indirizzo IP disponibile successivo non assegnato o non riservato nell'intervallo di indirizzi della subnet. Ad esempio, Azure assegna 10.0.0.10 a una nuova risorsa se gli indirizzi 10.0.0.4-10.0.0.9 sono già assegnati ad altre. Il metodo di allocazione predefinito è quello dinamico. Dopo che sono stati assegnati, gli indirizzi IP dinamici vengono rilasciati solo se un'interfaccia di rete viene eliminata o assegnata a un'altra subnet della stessa rete virtuale oppure se il metodo di allocazione viene modificato in statico e viene specificato un diverso indirizzo IP. Quando si modifica il metodo di allocazione da dinamico a statico, per impostazione predefinita Azure assegna l'indirizzo assegnato dinamicamente precedente come indirizzo statico.
- **Statico**: si seleziona e si assegna l'indirizzo IP disponibile successivo non assegnato o non riservato nell'intervallo di indirizzi della subnet. Ad esempio, se l'intervallo di indirizzi di una subnet è 10.0.0.0/16 e gli indirizzi 10.0.0.4-10.0.0.9 sono già assegnati ad altre risorse, è possibile assegnare qualsiasi indirizzo da 10.0.0.10 a 10.0.255.254. Gli indirizzi statici vengono rilasciati solo in caso di eliminazione di un'interfaccia di rete. Se si modifica il metodo di allocazione in dinamico, Azure assegna dinamicamente l'indirizzo IP statico assegnato in precedenza come indirizzo dinamico, anche se non è l'indirizzo disponibile successivo nell'intervallo di indirizzi della subnet. L'indirizzo viene modificato anche in caso di assegnazione dell'interfaccia di rete a un'altra subnet nella stessa rete virtuale, ma per assegnare l'interfaccia di rete a un'altra subnet è prima necessario modificare il metodo di allocazione da statico a dinamico. Dopo aver assegnato l'interfaccia di rete a un'altra subnet, è possibile modificare il metodo di allocazione di nuovo in statico e assegnare un indirizzo IP dell'intervallo di indirizzi della nuova subnet.

### <a name="virtual-machines"></a>Macchine virtuali

Uno o più indirizzi IP privati vengono assegnati a una o più **interfacce di rete** di una macchina virtuale [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Per ogni indirizzo IP privato è possibile specificare il metodo di allocazione statico o dinamico.

#### <a name="internal-dns-hostname-resolution-for-virtual-machines"></a>Risoluzione del nome host DNS interno per le macchine virtuali

Tutte le macchine virtuali di Azure sono configurate con [server DNS gestiti da Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) per impostazione predefinita, a meno che non si configurino in modo esplicito server DNS personalizzati. Questi server DNS forniscono la risoluzione dei nomi interna per le macchine virtuali che risiedono nella stessa rete virtuale.

Quando si crea una macchina virtuale, ai server DNS gestiti da Azure viene aggiunto un mapping del nome host al relativo indirizzo IP privato. Se una macchina virtuale ha più interfacce di rete o più configurazioni IP per un'interfaccia di rete, il nome host viene mappato all'indirizzo IP privato della configurazione IP primaria dell'interfaccia di rete primaria.

Le macchine virtuali configurate con server DNS gestiti di Azure possono risolvere i nomi host di tutte le macchine virtuali nella stessa rete virtuale nei relativi indirizzi IP privati. Per risolvere i nomi host delle macchine virtuali nelle reti virtuali connesse è necessario usare un server DNS personalizzato.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Servizi di bilanciamento del carico interno e gateway applicazione

È possibile assegnare un indirizzo IP privato alla configurazione **front-end** di un [servizio di bilanciamento del carico interno di Azure](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o di un [gateway applicazione di Azure](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Questo indirizzo IP privato funge da endpoint interno, accessibile solo alle risorse all'interno della rete virtuale e alle reti remote connesse alla rete virtuale. È possibile assegnare un indirizzo IP privato dinamico o statico alla configurazione front-end.

### <a name="at-a-glance"></a>Riepilogo
La tabella seguente illustra la proprietà specifica tramite la quale un indirizzo IP privato può essere associato a una risorsa di livello superiore e i metodi di allocazione possibili (dinamici o statici) utilizzabili.

| Risorse di livello superiore | Associazione di indirizzi IP | dinamico | statico |
| --- | --- | --- | --- |
| Macchina virtuale |interfaccia di rete |Yes |Yes |
| Bilanciamento del carico |Configurazione front-end |Yes |Yes |
| gateway applicazione |Configurazione front-end |Yes |Yes |

## <a name="limits"></a>Limiti
I limiti imposti agli indirizzi IP sono indicati nel set completo di [limiti della rete](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) in Azure. I limiti sono classificati per area e per sottoscrizione. È possibile [contattare il supporto tecnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) per aumentare i limiti predefiniti fino ai massimi consentiti in base alle esigenze aziendali.

## <a name="pricing"></a>Prezzi
Per gli indirizzi IP pubblici può essere previsto un addebito nominale. Per altre informazioni sui prezzi degli indirizzi IP in Azure, vedere la pagina [Prezzi per gli indirizzi IP](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Passaggi successivi
* [Distribuire una VM con un IP pubblico statico tramite il portale di Azure](virtual-network-deploy-static-pip-arm-portal.md)
* [Come impostare un indirizzo IP statico privato nel portale di Azure](virtual-networks-static-private-ip-arm-pportal.md)
