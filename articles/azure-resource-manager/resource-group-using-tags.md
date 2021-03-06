---
title: Applicare tag alle risorse Azure per l'organizzazione logica | Documentazione Microsoft
description: Mostra come applicare i tag per organizzare le risorse Azure per la fatturazione e la gestione.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: conceptual
ms.date: 08/10/2018
ms.author: tomfitz
ms.openlocfilehash: df9c218c275367852885e67ac2649926ba1d31d3
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42141539"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Usare tag per organizzare le risorse di Azure

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

Per applicare tag alle risorse, l'utente deve avere accesso in scrittura a quel tipo di risorsa. Per applicare tag a tutti i tipi di risorse, usare il ruolo [Collaboratore](../role-based-access-control/built-in-roles.md#contributor). Per applicare tag a un solo tipo di risorsa, usare il ruolo di collaboratore per tale risorsa. Ad esempio, per applicare tag a macchine virtuali, usare il ruolo di [Collaboratore Macchina virtuale](../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="powershell"></a>PowerShell

Gli esempi di questo articolo richiedono la versione 6.0 o successiva di Azure PowerShell. Se non si dispone della versione 6.0 o successiva [aggiornare la versione](/powershell/azure/install-azurerm-ps).

Per visualizzare i tag esistenti per un *gruppo di risorse*, usare:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Lo script restituisce il formato seguente:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Per visualizzare i tag esistenti per una *risorsa con un ID risorsa specificato*, usare:

```powershell
(Get-AzureRmResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

In alternativa, per visualizzare i tag esistenti per una *risorsa con un nome e un gruppo di risorse specificati*, usare:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Per ottenere i *gruppi di risorse con un tag specifico*, usare:

```powershell
(Get-AzureRmResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

Per ottenere le *risorse con un tag specifico*, usare:

```powershell
(Get-AzureRmResource -Tag @{ Dept="Finance"}).Name
```

Per ottenere le *risorse con un nome di tag specifico*, usare:

```powershell
(Get-AzureRmResource -TagName Dept).Name
```

Ogni volta che si applicano tag a una risorsa o a un gruppo di risorse, si sovrascrivono i tag esistenti in tale risorsa o gruppo di risorse. È quindi necessario usare un approccio diverso se nella risorsa o nel gruppo di risorse esistono tag.

Per aggiungere tag a un *gruppo di risorse senza tag esistenti*, usare:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Per aggiungere tag a un *gruppo di risorse con tag esistenti*, recuperare i tag esistenti, aggiungere il nuovo tag e riapplicare i tag:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Per aggiungere tag a una *risorsa senza tag esistenti*, usare:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Per aggiungere tag a una *risorsa con tag esistenti*, usare:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved") 
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Per applicare tutti i tag da un gruppo di risorse alle risorse *senza mantenere i tag esistenti nelle risorse*, usare lo script seguente:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Per applicare tutti i tag da un gruppo di risorse alle risorse *mantenendo i tag esistenti nelle risorse non duplicate*, usare lo script seguente:

```powershell
$group = Get-AzureRmResourceGroup "examplegroup"
if ($group.Tags -ne $null) {
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Per rimuovere tutti i tag, passare una tabella hash vuota:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Per visualizzare i tag esistenti per un *gruppo di risorse*, usare:

```azurecli
az group show -n examplegroup --query tags
```

Lo script restituisce il formato seguente:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

In alternativa, per visualizzare i tag esistenti per una *risorsa con un nome, un tipo e un gruppo di risorse specificati*, usare:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Quando si scorre una raccolta di risorse si potrebbe voler visualizzare le risorse in base all'ID di risorsa. Un esempio completo viene illustrato più avanti in questo articolo. Per visualizzare i tag esistenti per una *risorsa con un ID risorsa specificato*, usare:

```azurecli
az resource show --id <resource-id> --query tags
```

Per ottenere i gruppi di risorse con un tag specifico, usare `az group list`:

```azurecli
az group list --tag Dept=IT
```

Per ottenere tutte le risorse che hanno un tag e un valore specifici, usare `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Ogni volta che si applicano tag a una risorsa o a un gruppo di risorse, si sovrascrivono i tag esistenti in tale risorsa o gruppo di risorse. È quindi necessario usare un approccio diverso se nella risorsa o nel gruppo di risorse esistono tag.

Per aggiungere tag a un *gruppo di risorse senza tag esistenti*, usare:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Per aggiungere tag a una *risorsa senza tag esistenti*, usare:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Per aggiungere tag a una risorsa che dispone già di tag, recuperare i tag esistenti, riformattare il valore e riapplicare i tag nuovi e già esistenti: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Per applicare tutti i tag da un gruppo di risorse alle risorse *senza mantenere i tag esistenti nelle risorse*, usare lo script seguente:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Per applicare tutti i tag da un gruppo di risorse alle risorse e *mantenere i tag esistenti nelle risorse*, usare lo script seguente:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Modelli

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portale

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>API REST

Sia il portale di Azure che PowerShell usano l' [API REST di Gestione Risorse](https://docs.microsoft.com/rest/api/resources/) dietro le quinte. Se è necessario integrare l'assegnazione di tag in un altro ambiente, è possibile ottenere i tag tramite un'operazione **GET** sull'ID di risorsa e aggiornare il set di tag tramite una chiamata **PATCH**.

## <a name="tags-and-billing"></a>Tag e fatturazione

È possibile usare i tag per raggruppare i dati di fatturazione. Ad esempio, se si eseguono più macchine virtuali per organizzazioni diverse, usare i tag per raggruppare l'utilizzo in base al centro di costo. È anche possibile usare i tag per classificare i costi in base all'ambiente di runtime; ad esempio, l'uso di fatturazione per le macchine virtuali in esecuzione nell'ambiente di produzione.

Le informazioni sui tag possono essere recuperate tramite l' [API di utilizzo della risorsa di Azure e della Rate Card](../billing/billing-usage-rate-card-overview.md) o il file di utilizzo con valori delimitati da virgole (CSV). Il file di utilizzo può essere scaricato dal [portale dell'account di Azure](https://account.windowsazure.com/) o dal [portale EA](https://ea.azure.com). Per altre informazioni sull'accesso a livello di codice alle informazioni sulla fatturazione, vedere [Ottenere informazioni dettagliate sul consumo delle risorse di Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Per le operazioni API REST, vedere [Riferimento API REST alla fatturazione di Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Quando si scarica il CSV di utilizzo per i servizi che supportano i tag di fatturazione, i tag vengono visualizzati nella colonna **Tag** . Per altre informazioni, vedere [Comprendere la fattura per Microsoft Azure](../billing/billing-understand-your-bill.md).

![Vedere i tag della fatturazione](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Passaggi successivi

* È possibile applicare restrizioni e convenzioni all'interno della sottoscrizione tramite criteri personalizzati. Un criterio che è stato definito potrebbe richiedere che per tutte le risorse sia impostato un determinato tag. Per altre informazioni, vedere [Informazioni su Criteri di Azure](../azure-policy/azure-policy-introduction.md)
* Per un'introduzione all'uso di Azure PowerShell durante la distribuzione delle risorse, vedere [Uso di Azure PowerShell con Gestione risorse di Azure](powershell-azure-resource-manager.md).
* Per un'introduzione all'uso dell'interfaccia della riga di comando di Azure durante la distribuzione delle risorse, vedere [Uso dell'interfaccia della riga di comando di Azure per Mac, Linux e Windows con Gestione risorse di Azure](xplat-cli-azure-resource-manager.md).
* Per un'introduzione all'uso del portale, vedere [Uso del portale di Azure per gestire le risorse di Azure](resource-group-portal.md).  
* Per indicazioni su come le aziende possono usare Resource Manager per gestire efficacemente le sottoscrizioni, vedere [Azure enterprise scaffold - prescriptive subscription governance](/azure/architecture/cloud-adoption-guide/subscription-governance) (Scaffolding aziendale Azure - Governance prescrittiva per le sottoscrizioni).
