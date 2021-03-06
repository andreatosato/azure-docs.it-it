---
title: Esempi di Azure PowerShell - Abilitare la scalabilità automatica basata su host | Microsoft Docs
description: Esempi di Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ca484a6f1b2dafa3688efe0e50bb2c35f954a8b6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38697416"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-powershell"></a>Ridimensionare automaticamente un set di scalabilità di macchine virtuali con PowerShell
Questo script crea un set di scalabilità di macchine virtuali che esegue Windows Server 2016 e usa metriche basate su host per la scalabilità automatica in base alle variazioni del carico CPU.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script di esempio
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/auto-scale-host-metrics/auto-scale-host-metrics.ps1 "Automatically scale a virtual machine scale set")]

## <a name="clean-up-deployment"></a>Pulire la distribuzione
Eseguire questo comando per rimuovere il gruppo di risorse, il set di scalabilità e tutte le risorse correlate.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Spiegazione dello script
Questo script usa i comandi seguenti per creare la distribuzione. Ogni elemento della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Crea il set di scalabilità di macchine virtuali e tutte le risorse di supporto, tra cui la rete virtuale, il servizio di bilanciamento del carico e le regole NAT. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Ottiene informazioni su un set di scalabilità di macchine virtuali. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Aggiunge un'estensione per VM per Script personalizzato per l'installazione di un'applicazione Web di base. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Aggiorna il modello del set di scalabilità di macchine virtuali per l'applicazione dell'estensione di VM. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Ottiene informazioni sull'indirizzo IP pubblico assegnato usato dal servizio di bilanciamento del carico. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Rimuove un gruppo di risorse e tutte le risorse contenute al suo interno. |

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sul modulo Azure PowerShell, vedere la [documentazione di Azure PowerShell](/powershell/azure/overview).

Altri esempi di script PowerShell per i set di scalabilità di macchine virtuali sono disponibili nella [documentazione dei set di scalabilità di macchine virtuali di Azure](../powershell-samples.md).