---
title: Modalità di distribuzione di Azure Resource Manager| Microsoft Docs
description: Viene descritto come specificare se usare una modalità di distribuzione completa o incrementale con Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2018
ms.author: tomfitz
ms.openlocfilehash: c8c6c5499e1cea04bc5bdffbb5c07b53b96182e2
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2018
ms.locfileid: "42142358"
---
# <a name="azure-resource-manager-deployment-modes"></a>Modelli di distribuzione Azure Resource Manager
Quando si distribuiscono le risorse, specificare se la distribuzione è un aggiornamento incrementale o completo.  La differenza principale tra le due modalità è il modo in cui Resource Manager gestisce le risorse esistenti nel gruppo di risorse che non sono presenti nel modello.
La modalità predefinita è incrementale.

## <a name="incremental-and-complete-deployments"></a>Distribuzioni incrementali e complete
Durante la distribuzione della risorsa:

* Nella modalità di completamento, Resource Manager **elimina** le risorse esistenti nel gruppo di risorse che non sono specificate nel modello. 
* Nella modalità incrementale, Resource Manager **lascia invariate** le risorse esistenti nel gruppo di risorse che non sono specificate nel modello.

Per entrambe le modalità, Resource Manager prova a effettuare il provisioning di tutte le risorse specificate nel modello. Se la risorsa esiste già nel gruppo di risorse e le relative impostazioni sono identiche, l'operazione non comporta alcuna modifica. Se si modificano le impostazioni per una risorsa, il provisioning viene effettuato con le nuove impostazioni della risorsa. Se si prova ad aggiornare il percorso o il tipo di una risorsa esistente, la distribuzione ha esito negativo e restituisce un errore. È invece necessario distribuire una nuova risorsa con il percorso o il tipo necessari.

## <a name="example-result"></a>Risultati di esempio

Per illustrare la differenza tra le modalità incrementale e completa, si consideri lo scenario seguente.

Il **gruppo di risorse esistente** contiene:

* Risorsa A
* Risorsa B
* Risorsa C

Il **modello** definisce:

* Risorsa A
* Risorsa B
* Risorsa D

Quando viene implementato in modalità **incrementale**, il gruppo di risorse contiene:

* Risorsa A
* Risorsa B
* Risorsa C
* Risorsa D

Quando viene implementato in modalità **completa**, la risorsa C viene eliminata. Il gruppo di risorse contiene:

* Risorsa A
* Risorsa B
* Risorsa D

## <a name="set-deployment-mode"></a>Impostare la modalità di distribuzione

Per impostare la modalità di distribuzione durante la distribuzione con PowerShell, usare il parametro `Mode`.

```powershell
New-AzureRmResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json 
```

Per impostare la modalità di distribuzione durante la distribuzione con interfaccia della riga di comando di Azure, usare il parametro `mode`.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Quando si usa un [modello collegato o nidificato](resource-group-linked-templates.md), è necessario impostare la proprietà `mode` su `Incremental`. Solo i modelli a livello di radice supportano la modalità di distribuzione completa.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Passaggi successivi
* Per altre informazioni sulla creazione dei modelli, vedere [Creazione di modelli di Gestione risorse di Azure](resource-group-authoring-templates.md).
* Per informazioni sulla distribuzione delle risorse, vedere [Distribuire un'applicazione con un modello di Gestione risorse di Azure](resource-group-template-deploy.md).
* Per visualizzare le operazioni di un provider di risorse, vedere [Azure REST API](/rest/api/) (API REST di Azure).

