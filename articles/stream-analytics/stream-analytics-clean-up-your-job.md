---
title: Pulire un processo di Analisi di flusso di Azure
description: Questo articolo illustra come eliminare i processi di Analisi di flusso di Azure.
services: stream-analytics
author: mamccrea
manager: kfile
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 580d05909ff3c94c982be5353b3b5e86a78fc43f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969341"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Pulire un processo di Analisi di flusso di Azure

È possibile eliminare facilmente i processi di Analisi di flusso di Azure tramite il portale di Azure, Azure PowerShell, Azure SDK per .NET o API REST.

>[!NOTE] 
>Quando si arresta un processo di Analisi di flusso di Azure, i dati vengono mantenuti solo nell'archiviazione di input e output come ad esempio Hub eventi o Database SQL di Azure. Se è necessario rimuovere dati da Azure, assicurarsi di seguire il processo di rimozione per le risorse di input e output del processo di Analisi di flusso.

## <a name="stop-a-job-in-azure-portal"></a>Arrestare un processo nel portale di Azure

1. Accedere al [portale di Azure](https://portal.azure.com). 

2. Individuare il processo di Analisi di flusso in esecuzione e selezionarlo.

3. Nella pagina del processo di Analisi di flusso selezionare **Arresta** per arrestare il processo. 

   ![Arresta processo](./media/stream-analytics-clean-up-your-job/stop-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Eliminare un processo nel portale di Azure

1. Accedere al portale di Azure. 

2. Individuare il processo di Analisi di flusso di Azure esistente e selezionarlo.

3. Nella pagina del processo di Analisi di flusso selezionare **Elimina** per eliminare il processo. 

   ![Elimina processo](./media/stream-analytics-clean-up-your-job/delete-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Arrestare o eliminare un processo con PowerShell

Per arrestare un processo con PowerShell, usare il cmdlet [Stop-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0). Per eliminare un processo con PowerShell, usare il cmdlet [Remove-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0).

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Arrestare o eliminare un processo con Azure SDK per .NET

Per arrestare un processo con Azure SDK per .NET, usare il metodo [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet). Per eliminare un processo con Azure SDK per .NET, usare il metodo [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet).

## <a name="stop-or-delete-a-job-using-rest-api"></a>Arrestare o eliminare un processo con API REST

Per arrestare un processo con API REST, vedere il metodo [Stop](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop). Per eliminare un processo con API REST, vedere il metodo [Delete](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete).