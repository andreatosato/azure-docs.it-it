---
title: Estensione Windows VM Snapshot per Backup di Azure | Microsoft Docs
description: Portare il backup coerente dell'applicazione della macchina virtuale da Azure Backup usando l'estensione VM Snapshot
services: backup, virtual-machines-windows
documentationcenter: ''
author: trinadhk
manager: jeconnoc
editor: ''
ms.assetid: 57759670-0baa-44db-ae14-8cdc00d3a906
ms.service: backup, virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: trinadhk
ms.openlocfilehash: 8426a2472a28cf287dfe574cb80da56108394ae8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33944863"
---
# <a name="vm-snapshot-windows-extension-for-azure-backup"></a>Estensione Windows VM Snapshot per Backup di Azure

## <a name="overview"></a>Panoramica

Backup di Azure fornisce supporto nel backup dei carichi di lavoro da locale al cloud e nel backup delle risorse cloud nel vault dei Servizi di ripristino. Backup di Azure usa l'estensione VM Snapshot per eseguire un backup coerente dell'applicazione della macchina virtuale di Azure senza la necessità di arrestare la VM. L'estensione VM Snapshot è pubblicata e supportata da Microsoft come parte del servizio Backup di Azure. Backup di Azure installerà l'estensione come parte del primo backup pianificato attivato dopo l'attivazione del backup. Questo documento descrive in dettaglio le piattaforme, le configurazioni e le opzioni di distribuzione supportate per l'estensione VM Snapshot.

## <a name="prerequisites"></a>prerequisiti

### <a name="operating-system"></a>Sistema operativo
Per un elenco dei sistemi operativi supportati, consultare i [sistemi operativi supportati da Backup di Azure](../../backup/backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup)

### <a name="internet-connectivity"></a>Connettività Internet

Per l'estensione VM Snapshot è necessario che la macchina virtuale di destinazione sia connessa a Internet quando viene eseguito un backup della macchina virtuale.

## <a name="extension-schema"></a>Schema dell'estensione

Il codice JSON riportato di seguito mostra lo schema dell'estensione VM Snapshot. L'estensione richiede l'ID dell'attività che identifica il processo di backup che ha attivato la snapshot sulla macchina virtuale, lo stato dell'URI del BLOB, dove viene scritto lo stato della snapshot, l'ora di inizio pianificata della snapshot, registra l'URI del BLOB, dove i registri corrispondenti all'attività della snapshot vengono scritti, objstr- rappresentazione oggettiva di dischi VM e metadati.  Poiché queste impostazioni devono essere trattate come dati sensibili, occorre memorizzarle in una configurazione protetta. I dati della configurazione protetta dell'estensione macchina virtuale di Azure vengono crittografati, per essere poi decrittografati solo nella macchina virtuale di destinazione. Si noti che si consiglia di passare queste impostazioni dal servizio di Backup di Azure solo come parte del processo di backup.

```json
{
  "type": "extensions",
  "name": "VMSnapshot",
  "location":"<myLocation>",
  "properties": {
    "publisher": "Microsoft.RecoveryServices",
    "type": "VMSnapshot",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "locale":"<location>",
      "taskId":"<taskId used by Azure Backup service to communicate with extension>",
      "commandToExecute": "snapshot",
      "commandStartTimeUTCTicks": "<scheduled start time of the snapshot task>",
      "vmType": "microsoft.compute/virtualmachines"
    },
    "protectedSettings": {
      "objectStr": "<blob SAS uri represenattion of VM sent by Azure Backup service to extension>",
      "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
      "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
    }
  }
}
```

### <a name="property-values"></a>Valori delle proprietà

| NOME | Valore/Esempio | Tipo di dati |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| taskId | e07354cf-041e-4370-929f-25a319ce8933_1 | stringa |
| commandStartTimeUTCTicks | 6.36458E+17 | stringa |
| locale | it-it | stringa |
| objectStr | Codifica di sas uri array- "blobSASUri": ["https:\/\/sopattna5365.blob.core.windows.net\/vhds\/vmwin1404ltsc201652903941.vhd?sv=2014-02-14&sr=b&sig=TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw", "https:\/\/sopattna8461.blob.core.windows.net\/vhds\/vmwin1404ltsc-20160629-122418.vhd?sv=2014-02-14&sr=b&sig=5S0A6YDWvVwqPAkzWXVy%2BS%2FqMwzFMbamT5upwx05v8Q%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw", "https:\/\/sopattna8461.blob.core.windows.net\/bootdiagnostics-vmwintu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/vmubuntu1404ltsc-20160629-122541.vhd?sv=2014-02-14&sr=b&sig=X0Me2djByksBBMVXMGIUrcycvhQSfjYvqKLeRA7nBD4%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw", "https:\/\/sopattna5365.blob.core.windows.net\/vhds\/vmwin1404ltsc-20160701-163922.vhd?sv=2014-02-14&sr=b&sig=oXvtK2IXCNqWv7fpjc7TAzFDpc1GoXtT7r%2BC%2BNIAork%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw", "https:\/\/sopattna5365.blob.core.windows.net\/vhds\/vmwin1404ltsc-20170705-124311.vhd?sv=2014-02-14&sr=b&sig=ZUM9d28Mvvm%2FfrhJ71TFZh0Ni90m38bBs3zMl%2FQ9rs0%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw"] | stringa |
| logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | stringa |
| statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | stringa |



## <a name="template-deployment"></a>Distribuzione del modello

Le estensioni macchina virtuale di Azure possono essere distribuite con i modelli di Azure Resource Manager. Tuttavia, il modo consigliato per aggiungere un'estensione VM Snapshot a una macchina virtuale è quello di abilitare il backup sulla macchina virtuale. Ciò può essere ottenuto attraverso un modello di Gestione risorse.  Un esempio di modello di Gestione risorse che consente il backup su una macchina virtuale è disponibile nella [raccolta di avvio rapido di Azure](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/).


## <a name="azure-cli-deployment"></a>Distribuzione dell'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure può essere usata per abilitare il backup su una macchina virtuale. Dopo aver abilitato il backup, il primo processo di backup programmato installerà l'estensione VM Snapshot sulla macchina virtuale.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Risoluzione dei problemi e supporto

### <a name="troubleshoot"></a>Risolvere problemi

I dati sullo stato delle distribuzioni dell'estensione possono essere recuperati nel portale di Azure e tramite l'interfaccia della riga di comando di Azure. Per visualizzare lo stato di distribuzione delle estensioni per una determinata VM, eseguire il comando seguente nell'interfaccia della riga di comando di Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

L'output dell'esecuzione dell'estensione viene registrato nel file seguente:

```
C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot
```

### <a name="error-codes-and-their-meanings"></a>Codici di errore e relativi significati

Le informazioni sulla risoluzione dei problemi possono essere consultate nella [Guida alla risoluzione dei problemi per i backup delle macchine virtuali di Azure](../../backup/backup-azure-vms-troubleshoot.md).

### <a name="support"></a>Supporto

Per ricevere assistenza in relazione a qualsiasi punto di questo articolo, contattare gli esperti di Azure nei [forum MSDN e Stack Overflow relativi ad Azure](https://azure.microsoft.com/support/forums/). In alternativa, è possibile archiviare un evento imprevisto di supporto tecnico di Azure. Accedere al [sito del supporto di Azure](https://azure.microsoft.com/support/options/) e selezionare l'opzione desiderata per ottenere supporto. Per informazioni sull'uso del supporto di Azure, leggere le [Domande frequenti sul supporto di Azure](https://azure.microsoft.com/support/faq/).
