---
title: Rispondere a eventi con avvisi di Log Analytics di Azure | Microsoft Docs
description: Questa esercitazione consente di comprendere l'invio di avvisi con Log Analytics per identificare informazioni importanti nell'area di lavoro e per segnalare problemi all'utente in modo proattivo o richiamare le azioni per tentare di correggerli.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: c6c7b3f897e38fbd67098c9f881380bc073f13da
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432651"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Rispondere agli eventi con gli avvisi di Monitoraggio di Azure
Le regole di ricerca log vengono create da Avvisi di Azure in modo da eseguire automaticamente le query di log specificate a intervalli regolari.  Se i risultati della query del log corrispondono a criteri specifici viene creato un record di avviso. La regola può quindi eseguire automaticamente una o più azioni tramite [gruppi di azioni](../monitoring-and-diagnostics/monitoring-action-groups.md).  Questa esercitazione è la continuazione dell'esercitazione [Creare e condividere i dashboard dei dati di Log Analytics](log-analytics-tutorial-dashboards.md).   

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare una regola di avviso
> * Configurare un gruppo di azioni per l'invio di notifiche tramite posta elettronica

Per completare l'esempio contenuto in questa esercitazione, è necessario disporre di una macchina virtuale esistente [connessa all'area di lavoro di Log Analytics](log-analytics-quick-collect-azurevm.md).

## <a name="sign-in-to-azure-portal"></a>Accedere al portale di Azure
Accedere al portale di Azure all'indirizzo [https://portal.azure.com](https://portal.azure.com).

## <a name="create-alerts"></a>Creare avvisi
Gli avvisi vengono creati tramite le regole di avviso in Monitoraggio di Azure e possono eseguire automaticamente query salvate o ricerche personalizzate nei log a intervalli regolari.  È possibile creare avvisi in base a metriche di prestazioni specifiche o quando vengono creati determinati eventi, in assenza di un evento o per un numero di eventi creati all'interno di un intervallo di tempo specifico.  Ad esempio, è possibile usare gli avvisi per avvisare l'utente quando l'utilizzo medio della CPU supera una determinata soglia, quando viene rilevata l'assenza di un aggiornamento o quando viene generato un evento in caso venga rilevato che un servizio di Windows specifico o un daemon Linux non è in esecuzione.  Se i risultati della ricerca nei log corrispondono a criteri specifici, viene creato un avviso. La regola può quindi eseguire automaticamente una o più azioni, ad esempio informare l'utente dell'avviso o richiamare un altro processo.

Nell'esempio seguente viene creata una regola di avviso Unità di misura della metrica basata sulla query *Macchine virtuali di Azure - Utilizzo del processore* salvata nell'[esercitazione Visualizzare i dati](log-analytics-tutorial-dashboards.md). Viene creato un avviso per ogni macchina virtuale che supera la soglia del 90%.

1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Monitoraggio**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Monitoraggio**.
1. Nel riquadro sinistro selezionare **Avvisi** e quindi fare clic su **Nuova regola di avviso** all'inizio della pagina per creare un nuovo avviso.

    ![Creare una nuova regola di avviso](./media/log-analytics-tutorial-response/alert-rule-02.png)

1. Per il primo passaggio, nella sezione **Crea avviso** occorre selezionare l'area di lavoro di Log Analytics come risorsa, poiché di tratta di un segnale di avviso basato su log.  Filtrare i risultati scegliendo nell'elenco a discesa la **sottoscrizione** specifica (se ne esiste più di una) che contiene la macchina virtuale e l'area di lavoro di Log Analytics create in precedenza.  Filtrare **Tipo di risorsa** selezionando **Log Analytics** nell'elenco a discesa.  Infine, selezionare la **risorsa** **DefaultLAWorkspace** e fare clic su **Fine**.

    ![Passaggio 1 della creazione di un avviso](./media/log-analytics-tutorial-response/alert-rule-03.png)

1. Nella sezione **Criteri di avviso** fare clic su **Aggiungi criteri** per definire la query e quindi specificare la logica seguita dalla regola di avviso. Nel riquadro **Configura logica dei segnali** selezionare **Ricerca log personalizzata** come nome del segnale e immettere la query in **Query di ricerca**.

    Ad esempio: 
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    Il riquadro viene aggiornato in modo da presentare le impostazioni di configurazione dell'avviso.  Nella parte superiore vengono visualizzati i risultati degli ultimi 30 minuti del segnale selezionato.

1. Configurare l'avviso con le informazioni seguenti:  
   a. Nell'elenco a discesa **In base a* selezionare **Unità di misura della metrica**.  Un'unità di misura della metrica creerà un avviso per ogni oggetto nella query con un valore che supera la soglia specificata.  
   b. Per **Condizione** selezionare **Maggiore di** e immettere **90** per **Soglia**.  
   c. Nella sezione "Attiva l'avviso in base a" selezionare **Violazioni consecutive**, quindi nell'elenco a discesa selezionare **Maggiore di** e immettere il valore 3.  
   d. Nella sezione "Valutazione in base a" accettare le impostazioni predefinite. La regola verrà eseguita ogni cinque minuti e restituirà i record creati all'interno di questo intervallo dell'ora corrente.  
1. Fare clic su **Fine** per completare la regola di avviso.

    ![Configurare il segnale di avviso](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

1. Nel secondo passaggio occorre specificare un nome per l'avviso nel campo **Nome regola di avviso**, ad esempio **Percentuale CPU maggiore del 90%**.  In **Descrizione** specificare i dettagli dell'avviso e selezionare **Critico (gravità 0)** come valore di **Gravità** nelle opzioni disponibili.

    ![Configurare i dettagli dell'avviso](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

1. Per attivare immediatamente la regola di avviso alla creazione, accettare il valore predefinito di **Abilita regola alla creazione**.  
1. Per il terzo e ultimo passaggio occorre specificare un **gruppo di azioni**, che assicura che vengano eseguite le stesse azioni ogni volta che viene attivato un avviso e che può essere usato per ogni regola definita.  Configurare un nuovo gruppo di azioni con le informazioni seguenti:  
   a. Selezionare **Nuovo gruppo di azioni** per visualizzare il riquadro **Aggiungi gruppo di azioni**.  
   b. In **Nome gruppo di azioni** specificare un nome come **Operazioni IT - Notifica** e in **Nome breve** specificare un nome come **itops-n**.  
   c. Verificare che i valori predefiniti di **Sottoscrizione** e **Gruppo di risorse** siano corretti. Se non lo sono, selezionare i valori corretti negli elenchi a discesa.  
   d. Nella sezione Azioni specificare un nome per l'azione, ad esempio **Invio di posta elettronica**, e in **Tipo di azione** selezionare **Posta elettronica/SMS/Push/Voce** nell'elenco a discesa. Il riquadro delle proprietà **Posta elettronica/SMS/Push/Voce** si apre sulla destra per fornire informazioni aggiuntive.  
   e. Nel riquadro **Posta elettronica/SMS/Push/Voce** abilitare **Posta elettronica** e specificare un indirizzo SMTP valido a cui recapitare il messaggio.  
   f. Fare clic su **OK** per salvare le modifiche.  
       ![Creazione di un nuovo gruppo di azioni](./media/log-analytics-tutorial-response/action-group-properties-01.png)

1. Fare clic su **OK** per completare la creazione del gruppo di azioni.
1. Fare clic su **Crea regola di avviso** per completare la creazione della regola di avviso. L'esecuzione inizia immediatamente.

    ![Completare la creazione della nuova regola di avviso](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Visualizzare gli avvisi nel portale di Azure
Una volta creato un avviso, è possibile visualizzare gli avvisi di Azure in un unico riquadro e gestire tutte le regole di avviso nelle diverse sottoscrizioni di Azure. Include l'elenco di tutte le regole di avviso (abilitate o disabilitate), che può essere ordinato in base alle risorse di destinazione, ai gruppi di risorse, al nome della regola o allo stato. È incluso anche un riepilogo aggregato di tutti gli avvisi attivati e di tutte le regole di avviso configurate o abilitate.

![Pagina di stato degli avvisi di Azure](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Quando l'avviso viene attivato, la tabella riflette la condizione e il numero di occorrenze nell'intervallo di tempo selezionato (in base all'impostazione predefinita, le ultime sei ore).  La posta in arrivo dovrebbe contenere un messaggio di posta elettronica corrispondente simile all'esempio seguente, che mostra la macchina virtuale che causa l'errore e i primi risultati corrispondenti alla query di ricerca in questo caso.

![Esempio di avviso di azione di posta elettronica](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione si è appreso come le regole di avviso possono identificare e rispondere in modo proattivo a un problema durante l'esecuzione di ricerche nei log a intervalli pianificati e trovando corrispondenze a determinati criteri.

Seguire questo collegamento per vedere esempi di script predefiniti di Log Analytics.

> [!div class="nextstepaction"]
> [Esempi di script di Log Analytics](powershell-samples.md)
