---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Creare una sottoscrizione di Griglia di eventi di Azure | Microsoft Docs
description: Lo script dell'interfaccia della riga di comando di Azure in questo argomento mostra come creare una sottoscrizione di Griglia di eventi di Azure a livello di account per modifiche dello stato dei processi.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/11/2018
ms.author: juliako
ms.openlocfilehash: 5293ac55fdb13bba85996f5ed81034d4ebeeff12
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722882"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Esempio dell'interfaccia della riga di comando: Creare una sottoscrizione di Griglia di eventi di Azure 

Lo script dell'interfaccia della riga di comando di Azure in questo articolo mostra come creare una sottoscrizione di Griglia di eventi di Azure a livello di account per modifiche dello stato dei processi.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, questo articolo richiede la versione 2.0.20 o successiva dell'interfaccia della riga di comando di Azure. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script di esempio

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Passaggi successivi

Per altri esempi, vedere [Azure CLI samples](../cli-samples.md) (Esempi dell'interfaccia della riga di comando di Azure).
