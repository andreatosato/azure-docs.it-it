---
title: Query di base di Azure Resource Graph
description: Usare Azure Resource Graph per eseguire alcune query di base.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: b0d8466f1deaa82260aaebee5398f1b8fbea851d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965552"
---
# <a name="starter-resource-graph-queries"></a>Query di base di Resource Graph

Il primo passaggio per capire le query con Azure Resource Graph è conoscere i fondamenti del [linguaggio di query](../concepts/query-language.md). Se non si ha familiarità con KQL, è consigliabile impararne le nozioni di base per capire come comporre richieste in grado di individuare le risorse desiderate.

Si esamineranno le query di base seguenti:

> [!div class="checklist"]
> - [Contare le risorse di Azure](#count-resources)
> - [Elencare le risorse ordinate per nome](#list-resources)
> - [Mostrare tutte le macchine virtuali ordinate per nome in ordine decrescente](#show-vms)
> - [Mostrare le prime cinque macchine virtuali per nome e tipo di sistema operativo](#show-sorted)
> - [Contare le macchine virtuali per tipo di sistema operativo](#count-os)
> - [Mostrare le risorse che contengono archivi](#show-storage)
> - [Elencare tutti gli indirizzi IP pubblici](#list-publicip)
> - [Contare le risorse che hanno indirizzi IP configurati per sottoscrizione](#count-resources-by-ip)
> - [Elencare le risorse con un valore di tag specifico](#list-tag)
> - [Elencare tutti gli account di archiviazione con un valore di tag specifico](#list-specific-tag)

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free) prima di iniziare.

## <a name="language-support"></a>Supporto per le lingue

Azure Resource Graph è supportato dall'interfaccia della riga di comando di Azure tramite un'estensione e da Azure PowerShell tramite un modulo. Prima di eseguire le query seguenti, verificare che l'ambiente sia pronto. Per i passaggi necessari per installare e convalidare l'ambiente shell preferito, vedere le informazioni relative all'[interfaccia della riga di comando di Azure](../first-query-azurecli.md#add-the-resource-graph-extension) e ad [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module).

## <a name="count-resources"></a>Contare le risorse di Azure

Questa query restituisce il numero di risorse di Azure esistenti nelle sottoscrizioni a cui l'utente ha accesso. È anche una query valida da usare per verificare che nella shell preferita siano installati i componenti di Azure Resource Graph appropriati e funzionino correttamente.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Elencare le risorse ordinate per nome

Senza limitazioni a particolari tipi di risorsa o a determinate proprietà, questa query restituisce solo il **nome**, il **tipo** e il **percorso** delle risorse di Azure, ma usa `order by` per ordinarle in base alla proprietà **name** in ordine crescente (`asc`).

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>Mostrare tutte le macchine virtuali ordinate per nome in ordine decrescente

Anziché ottenere tutte le risorse di Azure, se si vuole solo un elenco di macchine virtuali, il cui tipo è `Microsoft.Compute/virtualMachines`, è possibile cercare le corrispondenze della proprietà **type** nei risultati.
Analogamente alla query precedente, `desc` cambia `order by` in decrescente. Nella ricerca di corrispondenze per tipo, `=~` induce Resource Graph a non distinguere fra maiuscole e minuscole.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="show-sorted"></a>Mostrare le prime cinque macchine virtuali per nome e tipo di sistema operativo

Questa query userà `limit` per recuperare solo cinque record corrispondenti che vengono ordinati in base al nome. Il tipo della risorsa di Azure è `Microsoft.Compute/virtualMachines`. `project` indica ad Azure Resource Graph quali proprietà includere.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Contare le macchine virtuali per tipo di sistema operativo

Continuando dalla query precedente, le risorse sono ancora limitate al tipo `Microsoft.Compute/virtualMachines`, ma non è più presente il limite sul numero di record restituiti.
Invece, sono stati usati `summarize` e `count()` per definire come raggruppare e aggregare i valori in base a una proprietà, che in questo esempio è `properties.storageProfile.osDisk.osType`. Per un esempio dell'aspetto di questa stringa nell'oggetto completo, vedere le informazioni sull'[esplorazione delle risorse e l'individuazione delle macchine virtuali](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Un modo diverso per scrivere la stessa query consiste nell'applicare `extend` a una proprietà assegnando un nome temporaneo da usare all'interno della query, in questo caso **os**. **os** viene quindi usato da `summarize` e `count()` come nell'esempio precedente.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> Tenere presente che, sebbene `=~` consenta la ricerca di corrispondenze senza distinzione fra maiuscole e minuscole, per usare le proprietà, ad esempio **properties.storageProfile.osDisk.osType**, nella query è necessario che le maiuscole/minuscole siano corrette. Se le maiuscole/minuscole sono errate, la query può comunque restituire un valore, ma il raggruppamento o il riepilogo non saranno corretti.

## <a name="show-storage"></a>Mostrare le risorse che contengono archivi

Invece di definire in modo esplicito il tipo da individuare, questa query di esempio troverà tutte le risorse di Azure che contengono (`contains`) la parola **storage**.

```Query
where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "where type contains 'storage' | distinct type"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Elencare tutti gli indirizzi IP pubblici

Analogamente alla query precedente, trovare gli elementi in cui il tipo contenga la parola **publicIPAddresses**. Questa query si basa su tale modello per escludere i risultati in cui **properties.ipAddress** è null, in modo da restituire solo **properties.ipAddress** e limitare (`limit`) i risultati ai primi 100. Potrebbe essere necessario eseguire l'escape delle virgolette a seconda della shell in uso.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

## <a name="count-resources-by-ip"></a>Contare le risorse che hanno indirizzi IP configurati per sottoscrizione

Usando la query dell'esempio precedente e aggiungendo `summarize` e `count()`, è possibile ottenere un elenco per sottoscrizione delle risorse con indirizzi IP configurati.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

## <a name="list-tag"></a>Elencare le risorse con un valore di tag specifico

È possibile limitare i risultati mediante proprietà diverse dal tipo di risorsa di Azure, ad esempio un tag. In questo esempio si applica alle risorse di Azure un filtro basato sul nome di tag **Environment** con il valore **Internal**.

```Query
where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name"
```

Se fosse necessario specificare anche i tag di quelle risorse e i relativi valori, questo esempio potrebbe essere espanso aggiungendo la proprietà **tag** alla parola chiave `project`.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Elencare tutti gli account di archiviazione con un valore di tag specifico

Combinando la capacità di filtro dell'esempio precedente con l'applicazione di filtri basati sul tipo di risorsa di Azure mediante la proprietà **type**, è possibile limitare la ricerca per tipi specifici di risorse di Azure con un nome di tag e un valore specifici.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> Questo esempio usa `==` per la corrispondenza anziché l'operatore condizionale `=~`. `==` rappresenta una corrispondenza con distinzione fra maiuscole e minuscole.

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni sul [linguaggio di query](../concepts/query-language.md)
- Informazioni su come [esplorare le risorse](../concepts/explore-resources.md)
- Vedere esempi di [query avanzate](advanced.md)