---
title: Distribuire un contenitore Docker ASP.NET per Registro contenitori di Azure (ACR) | Microsoft Docs
description: Informazioni su come usare Visual Studio Tools per Docker per distribuire un'app Web ASP.NET Core in un registro contenitori
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: ''
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: mlearned
ms.openlocfilehash: 58df17b17de1d93683875b68dd7c6c087bc6d16d
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972309"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Distribuire un contenitore ASP.NET in un registro contenitori tramite Visual Studio
## <a name="overview"></a>Panoramica
Docker è un motore contenitore leggero, simile in qualche modo a una macchina virtuale, che è possibile usare per ospitare applicazioni e servizi.
Nell'esercitazione verrà usato Visual Studio per pubblicare l'applicazione in contenitori in un [Registro contenitori di Azure](https://azure.microsoft.com/services/container-registry).

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) prima di iniziare.

## <a name="prerequisites"></a>prerequisiti
Per completare questa esercitazione:

* Installare l'ultima versione di [Visual Studio 2017](https://azure.microsoft.com/downloads/) con il carico di lavoro "Sviluppo ASP.NET e Web"
* Installare [Docker per Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Creare un'app Web ASP.NET Core
La procedura seguente illustra la creazione di un'app ASP.NET Core di base che verrà usata in questa esercitazione.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. Pubblicare il contenitore in Registro contenitori di Azure
1. Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica**.
2. Nella finestra di dialogo Destinazione di pubblicazione, selezionare la scheda **Registro contenitori**.
3. Scegliere **Nuovo registro contenitori di Azure** e fare clic su **Pubblica**.
4. Inserire i valori desiderati in **Creare un nuovo Registro contenitori di Azure**.

    | Impostazione      | Valore consigliato  | DESCRIZIONE                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Prefisso DNS** | Nome globalmente univoco | Nome che identifica in modo univoco il registro contenitori. |
    | **Sottoscrizione** | Scegliere la sottoscrizione | Sottoscrizione di Azure da usare. |
    | **[Gruppo di risorse](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nome del gruppo di risorse in cui creare il registro contenitori. Per creare un nuovo gruppo di risorse scegliere **Nuovo**.|
    | **[SKU](https://docs.microsoft.com/azure/container-registry/container-registry-skus)** | Standard | Livello di servizio del registro contenitori  |
    | **Percorso del registro** | Un percorso vicino | Scegliere un Percorso in una [regione](https://azure.microsoft.com/regions/) nelle vicinanze o vicino ad altri servizi usati nel registro contenitori. |
    ![Finestra di dialogo Creare un'istanza di Registro contenitori di Azure di Visual Studio][0]
5. Fare clic su **Crea**

È possibile ora eseguire il pull del contenitore dal registro a qualsiasi host in grado di eseguire immagini Docker, ad esempio [Istanze di contenitore di Azure ](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
