---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Creare una definizione di applicazione gestita | Microsoft Docs
description: Esempio di script dell'interfaccia della riga di comando di Azure - Creare una definizione di applicazione gestita
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 1cc407e07aee307b00116aad1c44bf8e2b97e33e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432474"
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Creare una definizione di applicazione gestita con l'interfaccia della riga di comando di Azure

Questo script pubblica una definizione di applicazione gestita in un catalogo di servizi. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script di esempio

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa il comando seguente per creare la definizione di applicazione gestita. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [az managedapp definition create](https://docs.microsoft.com/cli/azure/managedapp/definition#az-managedapp-definition-create) | Crea una definizione di applicazione gestita. Fornire il pacchetto che contiene i file necessari. |


## <a name="next-steps"></a>Passaggi successivi

* Per un'introduzione alle applicazioni gestite, vedere [Panoramica di Applicazione gestita di Azure](../overview.md).
* Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure).
