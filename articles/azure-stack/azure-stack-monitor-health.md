---
title: Monitorare l'integrità e gli avvisi in Azure Stack | Microsoft Docs
description: Informazioni su come monitorare l'integrità e gli avvisi in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/10/2018
ms.author: mabrigg
ms.openlocfilehash: 69ed08e8f6c820790c432bfa25988e038fd0efbd
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379690"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Monitorare l'integrità e gli avvisi in Azure Stack

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

Lo Stack di Azure include funzionalità che consentono di visualizzare l'integrità e gli avvisi per un'area di Azure Stack di monitoraggio dell'infrastruttura. Il **gestione delle aree** riquadro, aggiunto per impostazione predefinita nel portale di amministrazione per la sottoscrizione di Provider predefinito, vengono elencate tutte le aree distribuite di Azure Stack. Il riquadro mostra il numero di avvisi critici e di avviso attivi per ogni area ed è il punto di ingresso dell'integrità e la funzionalità degli avvisi di Azure Stack.

 ![Il riquadro di gestione delle aree](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>Comprendere l'integrità in Azure Stack

 Integrità e gli avvisi vengono gestiti dal provider di risorse dell'integrità. I componenti dell'infrastruttura di Azure Stack registrano con il provider di risorse di integrità durante la configurazione e distribuzione di Azure Stack. Questa registrazione consente la visualizzazione dell'integrità e gli avvisi per ogni componente. Integrità in Azure Stack è un concetto semplice. In presenza di avvisi per un'istanza di un componente registrato, lo stato di integrità di tale componente riflette la gravità dell'avviso attiva peggiore; avviso o critico.

## <a name="alert-severity-definition"></a>Definizione di gravità dell'avviso

In Azure Stack vengono generati avvisi con solo due livelli di gravità: **avviso** e **critici**.

- **Warning**  
  Un operatore può indirizzare il messaggio di avviso in modo pianificato. L'avviso in genere non riguarda i carichi di lavoro utente.

- **Critical**  
  Un operatore deve prendere in considerazione l'avviso critico con urgenza. Questi sono problemi che attualmente impatto o verranno presto influiscono sugli utenti di Azure Stack. 

 
 ## <a name="view-and-manage-component-health-state"></a>Visualizzare e gestire lo stato di integrità di componente
 
 Un operatore di Azure Stack, è possibile visualizzare lo stato di integrità dei componenti nel portale di amministrazione e tramite l'API REST e PowerShell.
 
Per visualizzare lo stato di integrità nel portale, fare clic sull'area che si desidera visualizzare le **gestione delle aree** riquadro. È possibile visualizzare lo stato di integrità dei ruoli di infrastruttura e dei provider di risorse.

![Elenco di ruoli di infrastruttura](media/azure-stack-monitor-health/image2.png)

È possibile fare clic su un provider di risorse o un ruolo di infrastruttura per visualizzare informazioni più dettagliate.

> [!WARNING]  
> Se si scegliere un ruolo di infrastruttura, quindi l'istanza del ruolo, sono presenti opzioni per avviare, riavviare o arrestare. Non usare queste azioni quando si applicano gli aggiornamenti a un sistema integrato. Inoltre, eseguire **non** usare queste opzioni in un ambiente Azure Stack Development Kit. Queste opzioni sono progettate soltanto per un ambiente di sistemi integrati, in cui è presente più di un'istanza del ruolo in base al ruolo di infrastruttura. Il riavvio di un'istanza del ruolo (in particolare AzS-Xrp01) nel kit di sviluppo causa l'instabilità del sistema. Per la risoluzione dei problemi di assistenza, pubblicare il problema per il [forum di Azure Stack](https://aka.ms/azurestackforum).
>
 
## <a name="view-alerts"></a>Visualizzare gli avvisi

L'elenco di avvisi attivi per ogni area di Azure Stack è disponibile direttamente la **gestione delle aree** pannello. Il primo riquadro nella configurazione predefinita è il **avvisi** riquadro, che consente di visualizzare un riepilogo delle critici e avvertenze per l'area. È possibile aggiungere il riquadro avvisi, come qualsiasi altro riquadro in questo pannello al dashboard per l'accesso rapido.   

![Avvisa che presenta un avviso](media/azure-stack-monitor-health/image3.png)

Selezionando la parte superiore del **avvisi** riquadro, passare all'elenco di tutti gli avvisi attivi per l'area. Se si seleziona il **critici** oppure **avviso** voce presente nel compartimento, si passa a un elenco filtrato degli avvisi (critico o avviso). 

![Avvisi di avvertimento filtrato](media/azure-stack-monitor-health/image4.png)
  
Il **avvisi** pannello supporta la possibilità di filtrare sia a livello di gravità (critico o avviso) e lo stato (attivo o chiuso). La visualizzazione predefinita Mostra tutti gli avvisi attivi. Tutti gli avvisi chiusi vengono rimossi dal sistema dopo sette giorni.

![Riquadro filtro per filtrare per stato critico o stato di avviso](media/azure-stack-monitor-health/image5.png)

Il **vista API** azione consente di visualizzare l'API REST che è stato usato per generare la visualizzazione elenco. Questa azione fornisce un modo rapido per acquisire familiarità con la sintassi dell'API REST che è possibile usare per gli avvisi di query. È possibile utilizzare questa API di automazione o per l'integrazione con Data Center esistenti di monitoraggio, reporting e la creazione di ticket solutions. 

![L'opzione di API di visualizzazione che mostra l'API REST](media/azure-stack-monitor-health/image6.png)

È possibile fare clic su uno specifico avviso per visualizzare i dettagli dell'avviso. I dettagli dell'avviso mostrano tutti i campi che sono associati all'avviso e abilitare la navigazione rapida per il componente interessato e l'origine dell'avviso. Ad esempio, viene generato l'avviso seguente, se una delle istanze del ruolo di infrastruttura passa alla modalità offline o non è accessibile.  

![Nel pannello dei dettagli degli avvisi](media/azure-stack-monitor-health/image7.png)

Dopo che l'istanza del ruolo di infrastruttura è di nuovo online, l'avviso deve essere chiuso automaticamente. Molte, ma non a ogni avviso deve essere chiuso automaticamente quando viene risolto il problema sottostante. Si consiglia di selezionare **Chiudi avviso** dopo aver eseguito i passaggi correttivi. Se il problema persiste, Azure Stack genera un nuovo avviso. Se si risolvono il problema, l'avviso rimane chiuso e non richiede operazioni aggiuntive.

## <a name="next-steps"></a>Passaggi successivi

[Gestire gli aggiornamenti in Azure Stack](azure-stack-updates.md)

[Gestione area nello Stack di Azure](azure-stack-region-management.md)
