---
title: Endpoint del servizio Rete virtuale di Azure | Microsoft Docs
description: Informazioni su come abilitare l'accesso diretto alle risorse di Azure da una rete virtuale tramite gli endpoint del servizio.
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: anithaa
ms.custom: ''
ms.openlocfilehash: a4824cadfaad2115a64427df618bac5e6c6df6ee
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382298"
---
# <a name="virtual-network-service-endpoints"></a>Endpoint del servizio Rete virtuale

Gli endpoint del servizio Rete virtuale estendono lo spazio di indirizzi privato della rete virtuale e l'identità della rete virtuale ai servizi di Azure tramite una connessione diretta. Gli endpoint consentono di associare le risorse critiche dei servizi di Azure solo alle proprie reti virtuali. Il traffico che transita dalla rete virtuale al servizio di Azure rimane sempre nella rete backbone di Microsoft Azure.

Questa funzionalità è disponibile per i servizi e le aree di Azure seguenti:

- **[Archiviazione di Azure](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)**: disponibile a livello generale in tutte le aree di Azure.
- **[Database SQL di Azure](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile a livello generale in tutte le aree di Azure.
- **[Azure Cosmos DB](../cosmos-db/vnet-service-endpoint.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile a livello generale in tutte le aree di cloud pubblico di Azure.
- **[Azure Key Vault](https://blogs.technet.microsoft.com/kv/2018/06/25/announcing-virtual-network-service-endpoints-for-key-vault-preview/)**: disponibile a livello generale in tutte le aree di cloud pubblico di Azure.
- **[Azure SQL Data Warehouse](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile in anteprima in tutte le aree del cloud pubblico di Azure.
- **[Bus di servizio di Azure](../service-bus-messaging/service-bus-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile in anteprima.
- **[Hub eventi di Azure](../event-hubs/event-hubs-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile in anteprima.
- **[Server di Database di Azure per PostgreSQL](../postgresql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile a livello generale nelle aree di Azure in cui è disponibile il servizio di database.
- **[Server di Database di Azure per MySQL](../mysql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponibile a livello generale nelle aree di Azure in cui è disponibile il servizio di database.

Per le notifiche più aggiornate, vedere la pagina [Aggiornamenti della rete virtuale di Azure](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="key-benefits"></a>Vantaggi principali

Gli endpoint di servizio offrono i vantaggi seguenti:

- **Maggiore sicurezza per le risorse dei servizi di Azure**: con gli endpoint di servizio è possibile associare le risorse dei servizi di Azure alla rete virtuale. In questo modo si ottiene una maggiore sicurezza perché si rimuove completamente l'accesso Internet pubblico alle risorse, consentendo solo il traffico dalla rete virtuale.
- **Routing ottimale per il traffico del servizio di Azure dalla rete virtuale**: attualmente le route nella rete virtuale che forzano il transito del traffico Internet attraverso appliance locali e/o virtuali, eseguendo così un tunneling forzato, forzano anche il traffico del servizio di Azure nella stessa route del traffico Internet. Gli endpoint di servizio forniscono il routing ottimale per il traffico di Azure. 

  Gli endpoint instradano sempre il traffico del servizio direttamente dalla rete virtuale al servizio nella rete backbone di Microsoft Azure. Mantenendo il traffico nella rete backbone di Azure è possibile continuare a monitorare e verificare il traffico Internet in uscita dalle reti virtuali, tramite il tunneling forzato, senza conseguenze per il traffico del servizio. Vedere altre informazioni sulle [route definite dall'utente e il tunneling forzato](virtual-networks-udr-overview.md).
- **Semplice da configurare con un minor overhead di gestione**: non sono più necessari indirizzi IP pubblici riservati nelle reti virtuali per proteggere le risorse di Azure attraverso il firewall IP. Non sono necessari dispositivi NAT o gateway per configurare gli endpoint di servizio. Un endpoint di servizio può essere configurato con un semplice clic in una subnet. Non c'è alcun overhead aggiuntivo per la gestione degli endpoint.

## <a name="limitations"></a>Limitazioni

- La funzionalità è disponibile solo per le reti virtuali distribuite con il modello di distribuzione Azure Resource Manager.
- Gli endpoint vengono abilitati nelle subnet configurate nelle reti virtuali di Azure. Gli endpoint non possono essere usati per il traffico dall'ambiente locale ai servizi di Azure. Per altre informazioni, vedere [Proteggere l'accesso ai servizi di Azure dall'ambiente locale](#securing-azure-services-to-virtual-networks)
- Per SQL di Azure, un endpoint di servizio si applica solo al traffico del servizio di Azure nell'area della rete virtuale. Per il supporto del traffico RA-GRS e GRS in Archiviazione di Azure, gli endpoint includono anche le aree abbinate nelle quali è distribuita la rete virtuale. Vedere altre informazioni sulle [aree abbinate di Azure](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

## <a name="securing-azure-services-to-virtual-networks"></a>Associazione di servizi di Azure a reti virtuali

- L'endpoint di servizio della rete virtuale fornisce l'identità della rete virtuale al servizio di Azure. Quando gli endpoint di servizio sono abilitati nella rete virtuale, è possibile associare le risorse del servizio di Azure alla rete virtuale aggiungendo una regola della rete virtuale alle risorse.
- Il traffico del servizio di Azure da una rete virtuale usa attualmente indirizzi IP pubblici come indirizzi IP di origine. Con gli endpoint di servizio, il traffico del servizio passa all'uso di indirizzi privati della rete virtuale come indirizzi IP di origine per l'accesso al servizio di Azure dalla rete virtuale. Questa opzione consente di accedere ai servizi senza che siano necessari gli indirizzi IP pubblici riservati usati nei firewall IP.
- __Proteggere l'accesso ai servizi di Azure dall'ambiente locale__:

  per impostazione predefinita, le risorse del servizio associate alle reti virtuali non sono raggiungibili da reti locali. Se si vuole consentire il traffico dall'ambiente locale è necessario autorizzare anche indirizzi IP pubblici (generalmente NAT) dall'ambiente locale o da ExpressRoute. Questi indirizzi IP possono essere aggiunti tramite configurazione del firewall IP per le risorse del servizio di Azure.

  ExpressRoute: se si usa [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dall'ambiente locale, per il peering pubblico o per il peering Microsoft, sarà necessario identificare gli indirizzi IP NAT usati. Per il peering pubblico, ogni circuito ExpressRoute usa per impostazione predefinita due indirizzi IP NAT applicati al traffico del servizio di Azure quando il traffico entra nel backbone della rete di Microsoft Azure. Per il peering Microsoft, gli indirizzi IP NAT usati vengono forniti dal cliente o dal provider del servizio. Per consentire l'accesso alle risorse del servizio è necessario autorizzare questi indirizzi IP pubblici nell'impostazione del firewall IP per le risorse. Per trovare gli indirizzi IP del circuito ExpressRoute per il peering pubblico, [aprire un ticket di supporto in ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) tramite il portale di Azure. Vedere altre informazioni su [NAT per il peering pubblico e il peering Microsoft ExpressRoute.](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)

![Associazione di servizi di Azure a reti virtuali](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Configurazione

- Gli endpoint di servizio sono configurati in una subnet di una rete virtuale. Gli endpoint funzionano con qualsiasi tipo di istanza di calcolo in esecuzione in tale subnet.
- È possibile configurare più endpoint di servizio per tutti i servizi di Azure supportati in una subnet, ad esempio Archiviazione di Azure e Database SQL di Azure.
- Per Database SQL di Azure, le reti virtuali devono trovarsi nella stessa area della risorsa del servizio di Azure. Se si usano account di archiviazione di Azure GRS e RA-GRS, l'account principale deve trovarsi nella stessa area della rete virtuale. Per tutti gli altri servizi, le risorse del servizio di Azure possono essere protette per le reti virtuali in qualsiasi area. 
- La rete virtuale in cui è configurato l'endpoint può trovarsi nella stessa sottoscrizione della risorsa del servizio di Azure o in una sottoscrizione diversa. Per altre informazioni sulle autorizzazioni necessarie per configurare gli endpoint e proteggere i servizi di Azure, vedere la sezione [Provisioning](#Provisioning).
- Per i servizi supportati, è possibile associare risorse nuove o esistenti alle reti virtuali tramite gli endpoint di servizio.

### <a name="considerations"></a>Considerazioni

- Dopo aver abilitato un endpoint di servizio, gli indirizzi IP di origine delle macchine virtuali nella subnet passano dall'uso di indirizzi IPv4 pubblici all'uso dell'indirizzo IPv4 privato nella comunicazione con il servizio da tale subnet. Eventuali connessioni TCP aperte per il servizio vengono chiuse durante questo passaggio. Verificare che non siano in esecuzione attività critiche quando si abilita o disabilita un endpoint di servizio per un servizio in una subnet. Assicurarsi anche che le applicazioni possano connettersi automaticamente ai servizi di Azure dopo il cambio di indirizzo IP.

  Il cambio di indirizzo IP interessa solo il traffico del servizio dalla propria rete virtuale. Non si verifica alcun impatto sull'altro traffico in transito da o verso indirizzi IPv4 pubblici assegnati alle macchine virtuali. Per i servizi di Azure, se sono presenti regole del firewall che usano indirizzi IP pubblici di Azure, queste regole smetteranno di funzionare con il passaggio agli indirizzi privati della rete virtuale.
- Con gli endpoint di servizio, le voci DNS per i servizi di Azure rimangono invariate e continuano a risolversi in indirizzi IP pubblici assegnati al servizio di Azure.
- Gruppi di sicurezza di rete (NGS) con gli endpoint di servizio:
  - Per impostazione predefinita, i gruppi di sicurezza di rete consentono il traffico Internet in uscita e quindi anche il traffico dalla rete virtuale ai servizi di Azure. Questo comportamento rimane invariato con gli endpoint di servizio. 
  - Per negare tutto il traffico Internet in uscita e consentire solo il traffico verso servizi specifici di Azure, è possibile usare i [tag di servizio](security-overview.md#service-tags) nei gruppi di sicurezza di rete. È possibile specificare i servizi di Azure supportati come destinazione nelle regole del gruppo di sicurezza di rete. La gestione degli indirizzi IP per ogni tag verrà eseguita da Azure. Per altre informazioni, vedere [Tag del servizio di Azure per i gruppi di sicurezza di rete](security-overview.md#service-tags). 

### <a name="scenarios"></a>Scenari

- **Reti virtuali con peering, connesse o multiple**: per associare i servizi di Azure a più subnet in una o più reti virtuali, è possibile abilitare gli endpoint di servizio in modo indipendente in ognuna di queste subnet e associare le risorse dei servizi di Azure a tutte queste subnet.
- **Filtro del traffico in uscita da una rete virtuale ai servizi di Azure**: per verificare o filtrare il traffico destinato a un servizio di Azure da una rete virtuale, è possibile distribuire un'appliance virtuale di rete nella rete virtuale. Sarà quindi possibile applicare gli endpoint di servizio alla subnet in cui è distribuita l'appliance virtuale di rete e associare le risorse del servizio di Azure solo a questa subnet. Questo scenario può essere utile se si vuole limitare l'accesso al servizio di Azure dalla rete virtuale solo a risorse di Azure specifiche, usando il filtro dell'applicance virtuale di rete. Per altre informazioni, vedere il [traffico in uscita con le appliance virtuali di rete](/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- **Associazione delle risorse di Azure ai servizi distribuiti direttamente nelle reti virtuali**: diversi servizi di Azure possono essere distribuiti direttamente in subnet specifiche di una rete virtuale. È possibile associare le risorse dei servizi di Azure a subnet di [servizi gestiti](virtual-network-for-azure-services.md), configurando un endpoint di servizio nella subnet del servizio gestito.
- **Traffico del disco da una macchina virtuale di Azure**: il traffico del disco della macchina virtuale, inclusi montaggio, smontaggio e diskIO, per dischi gestiti/non gestiti non è interessato dalle modifiche al routing degli endpoint servizio per Archiviazione di Azure. È possibile limitare l'accesso REST ai BLOB di pagine per selezionare reti tramite endpoint servizio e [regole della rete di Archiviazione di Azure](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

### <a name="logging-and-troubleshooting"></a>Registrazione e risoluzione dei problemi

Dopo aver configurato gli endpoint di servizio per un servizio specifico, verificare che la route dell'endpoint di servizio sia abilitata nel modo seguente: 
 
- Convalida dell'indirizzo IP di origine di qualsiasi richiesta di servizio nella diagnostica del servizio. Tutte le nuove richieste con gli endpoint di servizio mostrano l'indirizzo IP di origine della richiesta come indirizzo IP privato della rete virtuale, assegnato al client che effettua la richiesta dalla rete virtuale. Senza l'endpoint, questo indirizzo sarà un indirizzo IP pubblico di Azure.
- Visualizzazione delle route valide in qualsiasi interfaccia di rete di una subnet. La route per il servizio:
  - Mostra una route predefinita più specifica per l'intervallo dei prefissi di indirizzo di ogni servizio
  - Ha nextHopType con valore *VirtualNetworkServiceEndpoint*
  - Indica che è applicata una connessione più diretta al servizio, rispetto alle route di tunneling forzato

>[!NOTE]
> La route dell'endpoint di servizio sostituisce qualsiasi route BGP o UDR per la corrispondenza dei prefissi di indirizzo di un servizio di Azure. Per altre informazioni, vedere [Uso di regole efficaci per risolvere i problemi](diagnose-network-routing-problem.md)

## <a name="provisioning"></a>Provisioning

Un utente con accesso in scrittura a una rete virtuale può configurare endpoint di servizio indipendenti nelle reti virtuali. Per associare le risorse dei servizi di Azure a una rete virtuale, l'utente deve avere l'autorizzazione *Microsoft.Network/JoinServicetoaSubnet* per le subnet da aggiungere. Per impostazione predefinita, questa autorizzazione è inclusa nei ruoli di amministratore del servizio predefiniti e può essere modificata creando ruoli personalizzati.

Altre informazioni sui [ruoli predefiniti](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e sull'assegnazione di autorizzazioni specifiche ai [ruoli personalizzati](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Le reti virtuali e le risorse dei servizi di Azure possono trovarsi nella stessa sottoscrizione o in sottoscrizioni diverse. Se la rete virtuale e le risorse dei servizi di Azure si trovano in sottoscrizioni diverse, le risorse devono trovarsi nello stesso tenant di Active Directory (AD). 

## <a name="pricing-and-limits"></a>Prezzi e limiti

Non sono previsti costi aggiuntivi per l'uso degli endpoint di servizio. Viene applicato il modello di determinazione dei prezzi corrente per i servizi di Azure, ovvero Archiviazione di Azure, Database SQL di Azure, ecc.

Non viene applicato alcun limite al numero totale di endpoint di servizio in una rete virtuale.

Per una risorsa del servizio di Azure, ad esempio un account di archiviazione di Azure, i servizi possono applicare limiti al numero di subnet usate per la protezione della risorsa. Per informazioni dettagliate, vedere la documentazione relativa a diversi servizi nella sezione [Passaggi successivi](#next-steps).

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come [configurare gli endpoint del servizio di rete virtuale](tutorial-restrict-network-access-to-resources.md)
- Informazioni su come [associare un account di archiviazione di Azure a una rete virtuale](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Informazioni su come [associare un database SQL di Azure a una rete virtuale](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Informazioni sull'[integrazione dei servizi di Azure nelle reti virtuali](virtual-network-for-azure-services.md)
-  Avvio rapido: [modello di Azure Resource Manager](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) per configurare un endpoint di servizio in una subnet della rete virtuale e associare l'account di Archiviazione di Azure a tale subnet.

