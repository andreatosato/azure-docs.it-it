---
title: Creare una tabella di route dell'hub virtuale della rete WAN virtuale di Azure per l'indirizzamento a un'appliance virtuale di rete | Microsoft Docs
description: Tabella di route dell'hub virtuale della rete WAN virtuale per indirizzare il traffico a un'appliance virtuale di rete.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to work with routing tables for NVA.
ms.openlocfilehash: 56321c7c2239c0f1f0e73126d90a521e3cafb853
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984195"
---
# <a name="create-a-virtual-hub-route-table-to-steer-traffic-to-a-network-virtual-appliance"></a>Creare una tabella di route dell'hub virtuale per indirizzare il traffico a un'appliance virtuale di rete

Questo articolo illustra come indirizzare il traffico proveniente da un Hub virtuale a un'appliance virtuale di rete. 

![Diagramma della rete WAN virtuale](./media/virtual-wan-route-table/vwanroute.png)

In questo articolo viene spiegato come:

* Creare una rete WAN
* Creare un hub
* Creare connessioni di rete virtuale dell'hub
* Creare una route dell'hub
* Creare una tabella di route
* Applicare la tabella di route

## <a name="before-you-begin"></a>Prima di iniziare

Verificare di aver soddisfatto i criteri seguenti:

1. Disporre di un'appliance virtuale di rete (NVA), un software di terze parti di propria scelta, in genere con provisioning da Azure Marketplace (collegamento) in una rete virtuale.
2. Un indirizzo IP privato assegnato all'interfaccia di rete dell'appliance virtuale di rete. 
3. L'appliance virtuale di rete non può essere distribuita nell'hub virtuale. Deve essere distribuita in una rete virtuale separata. In questo articolo viene fatto riferimento alla rete virtuale come "rete virtuale della rete perimetrale".
4. La "rete virtuale della rete perimetrale" può avere una o più reti virtuali connesse ad essa. In questo articolo viene fatto riferimento alla rete virtuale come "rete virtuale spoke indiretta". Queste reti virtuali possono essere connesse alla rete virtuale della rete perimetrale usando il peering reti virtuali.
5. Verificare di avere 2 reti virtuali già create. Queste verranno usate come reti virtuali spoke. Per questo articolo, gli spazi di indirizzi della rete virtuale spoke sono 10.0.2.0/24 e 10.0.3.0/24. Se sono necessarie informazioni su come creare una rete virtuale, vedere [Creare una rete virtuale usando PowerShell](../virtual-network/quick-create-powershell.md).
6. Assicurarsi che non ci siano gateway di rete virtuale in nessuna rete virtuale.

## <a name="signin"></a>1. Accesso

Assicurarsi di installare la versione più recente dei cmdlet di PowerShell per Resource Manager. Per altre informazioni sull'installazione dei cmdlet di PowerShell, vedere [Come installare e configurare Azure PowerShell](/powershell/azure/overview). Questo elemento è importante perché le versioni precedenti dei cmdlet non contengono i valori correnti necessari per questo esercizio.

1. Aprire la console di PowerShell con privilegi elevati e accedere al proprio account Azure. Il cmdlet richiederà le credenziali di accesso. Dopo l'accesso vengono scaricate le impostazioni dell'account in modo che siano disponibili per Azure PowerShell.

  ```powershell
  Connect-AzureRmAccount
  ```
2. Ottenere un elenco delle sottoscrizioni di Azure.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Specificare la sottoscrizione da usare.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```

## <a name="rg"></a>2. Creare le risorse

1. Creare un gruppo di risorse.

  ```powershell
  New-AzureRmResourceGroup -Location "West US" -Name "testRG"
  ```
2. Creare una rete WAN virtuale.

  ```powershell
  $virtualWan = New-AzureRmVirtualWan -ResourceGroupName "testRG" -Name "myVirtualWAN" -Location "West US"
  ```
3. Creare un hub virtuale.

  ```powershell
  New-AzureRmVirtualHub -VirtualWan $virtualWan -ResourceGroupName "testRG" -Name "westushub" -AddressPrefix "10.0.1.0/24"
  ```

## <a name="connections"></a>3. Creare le connessioni

Creare le connessioni dell'hub di rete virtuale dalla rete virtuale spoke indiretta e dalla rete virtuale di rete perimetrale all'hub virtuale.

  ```powershell
  $remoteVirtualNetwork1= Get-AzureRmVirtualNetwork -Name “indirectspoke1” -ResourceGroupName “testRG”
  $remoteVirtualNetwork2= Get-AzureRmVirtualNetwork -Name “indirectspoke2” -ResourceGroupName “testRG”
  $remoteVirtualNetwork3= Get-AzureRmVirtualNetwork -Name “dmzvnet” -ResourceGroupName “testRG”

  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection1” -RemoteVirtualNetwork $remoteVirtualNetwork1
  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection2” -RemoteVirtualNetwork $remoteVirtualNetwork2
  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection3” -RemoteVirtualNetwork $remoteVirtualNetwork3
  ```

## <a name="route"></a>4. Creare una route dell'hub virtuale

Per questo articolo, gli spazi di indirizzi della rete virtuale spoke indiretta sono 10.0.2.0/24 e 10.0.3.0/24 e l'indirizzo IP privato dell'interfaccia di rete della rete virtuale della rete perimetrale è 10.0.4.5.

```powershell
$route1 = New-AzureRmVirtualHubRoute -AddressPrefix @("10.0.2.0/24", "10.0.3.0/24") -NextHopIpAddress "10.0.4.5"
```

## <a name="applyroute"></a>5. Creare una tabella di route dell'hub virtuale

Creare una tabella di route dell'hub virtuale, quindi applicarvi la route creata.
 
```powershell
$routeTable = New-AzureRmVirtualHubRouteTable -Route @($route1)
```

## <a name="commit"></a>6. Eseguire il commit delle modifiche

Eseguire il commit delle modifiche nell'hub virtuale.

```powershell
Set-AzureRmVirtualHub -VirtualWanId $virtualWan.Id -ResourceGroupName "testRG" -Name "westushub” -RouteTable $routeTable
```

## <a name="cleanup"></a>Pulire le risorse

Quando queste risorse non sono più necessarie, è possibile usare [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) per rimuovere il gruppo di risorse e tutte le risorse in esso contenute. Sostituire "myResourceGroup" con il nome del gruppo di risorse specifico ed eseguire il comando di PowerShell seguente:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla rete WAN virtuale, vedere la [panoramica sulla rete WAN virtuale](virtual-wan-about.md).