---
title: Dettagli della raccolta dati per le soluzioni di gestione in Azure | Microsoft Docs
description: Le soluzioni di gestione in Azure sono una raccolta di regole logiche, di visualizzazione e di acquisizione dei dati che forniscono metriche relative a un'area problematica specifica.  Questo articolo presenta un elenco delle soluzioni di gestione disponibili da Microsoft e informazioni dettagliate sul metodo e la frequenza della raccolta dati.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: 3154a2f8b283f68ec3e10ba621ccba3ee6d77de2
ms.sourcegitcommit: 465ae78cc22eeafb5dfafe4da4b8b2138daf5082
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44324751"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Informazioni dettagliate sulla raccolta dati per le soluzioni di gestione in Azure
Questo articolo include un elenco delle [soluzioni di gestione](monitoring-solutions.md) disponibili da Microsoft con collegamenti alla relativa documentazione dettagliata.  Sono inoltre disponibili informazioni sul metodo e la frequenza della raccolta dati in Log Analytics.  È possibile usare le informazioni in questo articolo per identificare le diverse soluzioni disponibili e per comprendere i requisiti di flusso di dati e connessione per le diverse soluzioni di gestione. 

## <a name="list-of-management-solutions"></a>Elenco delle soluzioni di gestione

La tabella seguente elenca le [soluzioni di gestione](monitoring-solutions.md) in Azure fornite da Microsoft. Un punto nella colonna indica che la soluzione raccoglie i dati in Log Analytics usando il metodo corrispondente.  Se per una soluzione non è selezionata alcuna colonna, questo indica che scrive direttamente in Log Analytics da un altro servizio di Azure. Fare clic sul collegamento per ognuna per visualizzare altre informazioni nella relativa documentazione dettagliata.

L'elenco seguente include le spiegazioni per le colonne:

- **Microsoft Monitoring Agent** - Agente usato in Windows e Linux per eseguire i Management Pack da SCOM e le soluzioni di gestione da Azure. In questa configurazione, l'agente viene connesso direttamente a Log Analytics senza connettersi a un gruppo di gestione di Operations Manager. 
- **Operations Manager** - Stesso agente di Microsoft Monitoring Agent. In questa configurazione, viene [connesso a un gruppo di gestione di Operations Manager](../log-analytics/log-analytics-om-agents.md) connesso a Log Analytics. 
-  **Archiviazione di Azure** - La soluzione raccoglie i dati da un account di Archiviazione di Azure. 
- **È necessario Operations Manager?** - Un gruppo di gestione di Operations Manager connesso è obbligatorio per la raccolta dati dalla soluzione di gestione. 
- **Dati dell'agente di Operations Manager inviati tramite il gruppo di gestione** - Se l'agente è [connesso a un gruppo di gestione SCOM](../log-analytics/log-analytics-om-agents.md), i dati vengono inviati a Log Analytics dal server di gestione. In questo caso, l'agente non deve connettersi direttamente a Log Analytics. Se questa casella non è selezionata, i dati vengono inviati dall'agente direttamente a Log Analytics anche se l'agente è connesso a un gruppo di gestione di SCOM. Dovrà essere in grado di comunicare con Log Analytics tramite un [gateway OMS](../log-analytics/log-analytics-oms-gateway.md).
- **Frequenza di raccolta** - Specifica la frequenza con cui la soluzione di gestione raccoglie i dati. 



| **Soluzione di gestione** | **Piattaforma** | **Microsoft Monitoring Agent** | **Agente di Operations Manager** | **Archiviazione di Azure** | **È necessario Operations Manager?** | **Dati dell'agente di Operations Manager inviati tramite il gruppo di gestione** | **Frequenza di raccolta** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Activity Log Analytics](../log-analytics/log-analytics-activity.md) | Azure | | | | | | su notifica |
| [Valutazione di AD](../log-analytics/log-analytics-ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 giorni |
| [Stato della replica di AD](../log-analytics/log-analytics-ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 giorni |
| [Integrità agente](../operations-management-suite/oms-solution-agenthealth.md) | Windows e Linux | &#8226; | &#8226; | | | &#8226; | 1 minuto |
| [Alert Management](../log-analytics/log-analytics-solution-alert-management.md) (Nagios) |Linux |&#8226; | | | | |all'arrivo |
| [Alert Management](../log-analytics/log-analytics-solution-alert-management.md) (Zabbix) |Linux |&#8226; | | | | |1 minuto |
| [Gestione avvisi](../log-analytics/log-analytics-solution-alert-management.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 minuti |
| [Azure Site Recovery](../site-recovery/site-recovery-overview.md) | Azure | | | | | | n/d |
| [Connettore di Application Insights (anteprima)](../log-analytics/log-analytics-app-insights-connector.md) | Azure | | | |  |  | su notifica |
| [Ruolo di lavoro ibrido per runbook di Automazione](../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | n/d |
| [Analisi dei gateway applicazione di Azure](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | su notifica |
| **Soluzione di gestione** | **Piattaforma** | **Microsoft Monitoring Agent** | **Agente di Operations Manager** | **Archiviazione di Azure** | **È necessario Operations Manager?** | **Dati dell'agente di Operations Manager inviati tramite il gruppo di gestione** | **Frequenza di raccolta** |
| [Analisi gruppo di sicurezza di rete di Azure](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | su notifica |
| [Azure SQL Analytics (Anteprima)](../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 minuto |
| [Backup](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | su notifica |
| [Capacità e prestazioni (anteprima)](../log-analytics/log-analytics-capacity.md) |Windows |&#8226; |&#8226; | | |&#8226; |all'arrivo |
| [Rilevamento delle modifiche](../log-analytics/log-analytics-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |Ogni ora |
| [Rilevamento delle modifiche](../log-analytics/log-analytics-change-tracking.md) |Linux |&#8226; | | | | |Ogni ora |
| [Contenitori](../log-analytics/log-analytics-containers.md) | Windows e Linux | &#8226; | &#8226; |  |  |  | 3 minuti |
| [Analisi dell'insieme di credenziali delle chiavi](../log-analytics/log-analytics-azure-key-vault.md) |Windows | | | | | |su notifica |
| [Valutazione di malware](../log-analytics/log-analytics-malware.md) |Windows |&#8226; |&#8226; | | |&#8226; |Ogni ora |
| [Monitoraggio delle prestazioni di rete](../log-analytics/log-analytics-network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP esegue handshake ogni 5 secondi e i dati vengono inviati ogni 3 minuti |
| [Analisi Office 365 (anteprima)](../operations-management-suite/oms-solution-office-365.md) |Windows | | | | | |su notifica |
| **Soluzione di gestione** | **Piattaforma** | **Microsoft Monitoring Agent** | **Agente di Operations Manager** | **Archiviazione di Azure** | **È necessario Operations Manager?** | **Dati dell'agente di Operations Manager inviati tramite il gruppo di gestione** | **Frequenza di raccolta** |
| [Analisi di Service Fabric](../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 minuti |
| [Elenco dei servizi](../operations-management-suite/operations-management-suite-service-map.md) | Windows e Linux | &#8226; | &#8226; |  |  |  | 5 secondi |
| [SQL Assessment](../log-analytics/log-analytics-sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 giorni |
| [SurfaceHub](../log-analytics/log-analytics-surface-hubs.md) |Windows |&#8226; | | | | |all'arrivo |
| [Valutazione System Center Operations Manager (anteprima)](../log-analytics/log-analytics-scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | 7 giorni |
| [Gestione degli aggiornamenti](../operations-management-suite/oms-solution-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |almeno 2 volte al giorno e 15 minuti dopo l'installazione di un aggiornamento |
| [Preparazione dell'aggiornamento](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 giorni |
| [Monitoraggio VMware (deprecato)](../log-analytics/log-analytics-vmware.md) | Linux | &#8226; |  |  |  |  | 3 minuti |
| [Wire Data 2.0 (anteprima)](../log-analytics/log-analytics-wire-data.md) |Windows (2012 R2/8.1 o versioni successive) |&#8226; |&#8226; | | | | 1 minuto |




## <a name="next-steps"></a>Passaggi successivi
* Informazioni su come [creare query](../log-analytics/log-analytics-log-searches.md) per analizzare i dati raccolti dalle soluzioni di gestione.
