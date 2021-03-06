---
title: Personalizzare le impostazioni di valutazione di Azure Migrate | Microsoft Docs
description: Descrive come configurare ed eseguire una valutazione per la migrazione di macchine virtuali VMware in Azure usando Azure Migration Planner
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 9ddd6c32388b2e05fd97138414958b67c009f9ee
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284914"
---
# <a name="customize-an-assessment"></a>Personalizzare una valutazione

[Azure Migrate](migrate-overview.md) crea valutazioni con proprietà predefinite. Dopo aver creato una valutazione, è possibile modificare le proprietà predefinite seguendo le istruzioni contenute in questo articolo.


## <a name="edit-assessment-properties"></a>Modificare le proprietà di valutazione

1. Nella pagina **Valutazioni** del progetto di migrazione selezionare la valutazione e fare clic su **Modifica proprietà**.
2. Modificare le proprietà in base alla tabella seguente:

    **Impostazione** | **Dettagli** | **Default**
    --- | --- | ---
    **Posizione di destinazione** | Area di Azure in cui si vuole eseguire la migrazione.<br/><br/> Azure Migrate supporta attualmente 30 aree, tra cui Asia orientale, Asia sud-orientale, Australia orientale, Australia sud-orientale, Brasile meridionale, Canada centrale, Canada orientale, Cina orientale, Cina settentrionale, Corea del Sud centrale, Corea del Sud meridionale, Europa occidentale, Europa settentrionale, Germania centrale, Germania nordorientale, Giappone occidentale, Giappone orientale, India centrale, India meridionale, India occidentale, Regno Unito meridionale, Regno Unito occidentale, Governo degli Stati Uniti Arizona, Governo degli Stati Uniti Texas, Governo degli Stati Uniti Virginia, Stati Uniti centrali, Stati Uniti centro-meridionali, Stati Uniti centro-occidentali, Stati Uniti centro-settentrionali, Stati Uniti orientali, Stati Uniti orientali 2, Stati Uniti occidentali e Stati Uniti occidentali 2. |  L'area predefinita è Stati Uniti occidentali 2.
    **Piano tariffario** | È possibile specificare il [piano tariffario (Basic o Standard)](../virtual-machines/windows/sizes-general.md) per le macchine virtuali di Azure di destinazione. Se, ad esempio, si prevede di eseguire la migrazione di un ambiente di produzione, considerare il livello Standard, che offre macchine virtuali con bassa latenza, anche se a un costo superiore. D'altra parte, se si ha un ambiente di sviluppo e test, può essere preferibile il livello Basic, che ha macchine virtuali con latenza maggiore e costi minori. | Per impostazione predefinita, viene usato il livello [Standard](../virtual-machines/windows/sizes-general.md).
    **Tipo di archiviazione** | È possibile specificare il tipo di dischi da allocare in Azure. Questa proprietà è applicabile quando il criterio di ridimensionamento è Determinazione della dimensione come in locale. È possibile specificare il tipo di disco di destinazione come dischi gestiti Premium o dischi gestiti Standard. Per il ridimensionamento in base alle prestazioni, la raccomandazione per i dischi avviene automaticamente in base ai dati delle prestazioni delle macchine virtuali. Si noti che Azure Migrate supporta solo dischi gestiti per la valutazione della migrazione. | Il valore predefinito è Managed Disks Premium, con il criterio di dimensionamento *as on-premises sizing* (Determinazione della dimensione come in locale).
    **Istanze riservate** |  È anche possibile specificare se sono presenti [istanze riservate](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure. Azure Migrate stimerà il costo di conseguenza. Le istanze riservate non sono applicabili alle aree sovrane (Azure per enti pubblici, Germania e Cina), ma solo all'offerta con pagamento in base al consumo in Azure Migrate. | Il valore predefinito per questa proprietà corrisponde a istanze riservate per 3 anni.
    **Criterio di dimensionamento** | Criterio che Azure Migrate deve usare per definire correttamente le dimensioni delle macchine virtuali per Azure. È possibile eseguire il dimensionamento *basato sulle prestazioni* o definire le dimensioni delle macchine virtuali *come in locale*, senza considerare la cronologia delle prestazioni. | L'opzione predefinita è il dimensionamento basato sulle prestazioni.
    **Cronologia delle prestazioni** | Durata da considerare per la valutazione delle prestazioni delle macchine virtuali. Questa proprietà è applicabile solo quando il criterio di dimensionamento è *basato sulle prestazioni*. | Il valore predefinito è un giorno.
    **Utilizzo percentile** | Valore percentile dell'esempio di prestazioni da tenere in considerazione per il dimensionamento corretto. Questa proprietà è applicabile solo quando il criterio di dimensionamento è *basato sulle prestazioni*.  | Il valore predefinito è 95° percentile.
    **Serie VM** | È possibile specificare la serie di macchine virtuali che si intende tenere in considerazione per il dimensionamento corretto. Ad esempio, se si dispone di un ambiente di produzione di cui non si intende eseguire la migrazione a macchine virtuali serie A in Azure, è possibile escludere la serie A dall'elenco o dalla serie e il dimensionamento verrà eseguito solo nella serie selezionata. | Per impostazione predefinita, sono selezionate tutte le serie di macchine virtuali.
    **Fattore di comfort** | Durante la valutazione, Azure Migrate considera un buffer (fattore di comfort), che viene applicato ai dati sull'utilizzo delle VM (CPU, memoria, disco e rete). Il fattore di comfort tiene conto di aspetti come utilizzo stagionale, breve cronologia delle prestazioni e probabile aumento dell'utilizzo futuro.<br/><br/> Da una VM con 10 core e un utilizzo del 20%, ad esempio, si ottiene normalmente una VM con 2 core. Con un fattore di comfort pari a 2.0x, invece, il risultato è una VM con 4 core. | L'impostazione predefinita è 1.3x.
    **Offerta** | [Offerta Azure](https://azure.microsoft.com/support/legal/offer-details/) sottoscritta. | L'opzione predefinita è [Pagamento in base al consumo](https://azure.microsoft.com/offers/ms-azr-0003p/).
    **Valuta** | Valuta di fatturazione. | La valuta predefinita è il dollaro statunitense.
    **Sconto (%)** | Qualsiasi sconto specifico della sottoscrizione ricevuto oltre all'offerta Azure. | L'impostazione predefinita è 0%.
    **Tempo di attività macchina virtuale** | Se non si prevede di eseguire ininterrottamente le macchine virtuali in Azure, è possibile specificare la durata (numero di giorni al mese e numero di ore al giorno) dell'esecuzione. Il costo verrà stimato di conseguenza. | Il valore predefinito è di 31 giorni al mese e di 24 ore al giorno.
    **Vantaggio Azure Hybrid** | Specificare se si dispone di licenze Software Assurance e se si è idonei per l'opzione [Vantaggio Azure Hybrid](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se il valore è impostato su Sì, alle macchine virtuali Windows si applicano i prezzi di Azure per sistemi non Windows. | Il valore predefinito è Yes.

3. Fare clic su **Salva** per aggiornare la valutazione.


## <a name="next-steps"></a>Passaggi successivi

[Altre informazioni](concepts-assessment-calculation.md) sul modo in cui vengono calcolate le valutazioni.
