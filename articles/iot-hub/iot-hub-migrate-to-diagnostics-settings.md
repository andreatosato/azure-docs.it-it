---
title: Migrazione dell'hub IoT di Azure alla funzionalità Impostazioni di diagnostica | Microsoft Docs
description: Come aggiornare l'hub IoT di Azure per l'uso della funzionalità Impostazioni di diagnostica anziché Monitoraggio operazioni per monitorare lo stato delle operazioni nell'hub IoT in tempo reale.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 85bdb4b4802283c591e4d7a9e8b14ae74fa44e8d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666723"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Eseguire la migrazione dell'hub IoT dalla funzionalità Monitoraggio operazioni a Impostazioni di diagnostica

I clienti che usano la funzionalità [Monitoraggio operazioni][lnk-opsmon] per tenere traccia delle operazioni nell'hub IoT possono eseguire la migrazione di tale flusso di lavoro alla funzionalità [Impostazioni di diagnostica di Azure][lnk-diagnostics-settings], una funzionalità di Monitoraggio di Azure. La funzionalità Impostazioni di diagnostica fornisce informazioni di diagnostica a livello di risorse per numerosi servizi di Azure.

La funzionalità Monitoraggio operazioni dell'hub IoT è deprecata e verrà rimossa nelle versioni future. In questo articolo viene descritta la procedura dettagliata per spostare i carichi di lavoro dalla funzionalità Monitoraggio operazioni alla funzionalità Impostazioni di diagnostica. Per altre informazioni sulla sequenza temporale relativa alla funzionalità deprecata, vedere [Monitorare le soluzioni IoT di Azure con Monitoraggio di Azure e Integrità risorse di Azure][lnk-blog-announcement].

## <a name="update-iot-hub"></a>Aggiornare l'hub IoT

Per aggiornare l'hub IoT nel portale di Azure, attivare la funzionalità Impostazioni di diagnostica e quindi disattivare la funzionalità Monitoraggio operazioni.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Disattivare la funzionalità Monitoraggio operazioni

Dopo aver verificato la nuova impostazione della funzionalità Impostazioni di diagnostica nel flusso di lavoro, è possibile disattivare la funzionalità Monitoraggio operazioni. 

1. Nel menu dell'hub IoT selezionare **Monitoraggio operazioni**.
1. In ciascuna categoria di monitoraggio selezionare **Nessuno**.
1. Salvare le modifiche apportate all'impostazione.

## <a name="update-applications-that-use-operations-monitoring"></a>Aggiornare le applicazioni che usano la funzionalità Monitoraggio operazioni

Gli schemi delle funzionalità Monitoraggio operazioni e Impostazioni di diagnostica variano leggermente. È importante aggiornare subito le applicazioni che usano la funzionalità Monitoraggio operazioni per eseguire il mapping allo schema usato dalla funzionalità Impostazioni di diagnostica. 

La funzionalità Impostazioni di diagnostica consente inoltre di tenere traccia di cinque nuove categorie. Dopo avere aggiornato le applicazioni per lo schema esistente, aggiungere le nuove categorie:

- Operazioni da cloud a dispositivi gemelli
- Operazioni da dispositivi gemelli a cloud
- Query dei dispositivi gemelli
- Operazioni dei processi
- Metodi diretti

Per le strutture di schemi specifiche, vedere [Informazioni sullo schema per la funzionalità Impostazioni di diagnostica][lnk-diagnostics-schema].

## <a name="next-steps"></a>Passaggi successivi

- [Monitorare l'integrità dell'hub IoT di Azure ed eseguire la diagnostica rapida dei problemi][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/
[lnk-monitor]: iot-hub-monitor-resource-health.md
