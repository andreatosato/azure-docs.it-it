---
title: Limitazioni di Azure Cloud Shell | Microsoft Docs
description: Panoramica delle limitazioni di Azure Cloud Shell
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: juluk
ms.openlocfilehash: 135496e17ae884db580922aa31f6824b2e7fd934
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855985"
---
# <a name="limitations-of-azure-cloud-shell"></a>Limitazioni di Azure Cloud Shell

Di seguito vengono descritte le limitazioni note di Azure Cloud Shell:

## <a name="general-limitations"></a>Limitazioni generali

### <a name="system-state-and-persistence"></a>Persistenza e stato del sistema

Il computer che distribuisce la sessione Cloud Shell è temporaneo e viene riciclato dopo 20 minuti di inattività della sessione. Cloud Shell richiede che sia montata una condivisione file di Azure. La sottoscrizione, quindi, deve essere in grado di configurare le risorse di archiviazione per accedere a Cloud Shell. Altre considerazioni di cui tenere conto:

* Con l'archiviazione montata vengono rese persistenti soltanto le modifiche apportate all'interno della directory `$Home`.
* Le condivisioni file di Azure possono essere implementate solo dall'interno dell'[area assegnata](persisting-shell-storage.md#mount-a-new-clouddrive).
  * In Bash, eseguire `env` per trovare l'area geografica impostata come `ACC_LOCATION`.

### <a name="browser-support"></a>Supporto browser

Cloud Shell supporta le versioni più recenti di Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox e Apple Safari. Safari in modalità privata non è supportato.

### <a name="copy-and-paste"></a>Copiare e incollare

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Per un determinato utente, può essere attiva una sola shell

Gli utenti possono avviare solo un tipo di shell contemporaneamente, **Bash** o **PowerShell**. Tuttavia, si possono avere più istanze di Bash o PowerShell in esecuzione contemporaneamente. Lo scambio tra Bash o PowerShell ha come conseguenza il riavvio di Cloud Shell e le sessioni esistenti vengono terminate.

### <a name="usage-limits"></a>Limiti di consumo

Cloud Shell è pensato per l'uso interattivo e qualsiasi sessione non interattiva in esecuzione prolungata viene quindi interrotta senza preavviso.

## <a name="bash-limitations"></a>Limitazioni Bash

### <a name="user-permissions"></a>Autorizzazioni utente

Le autorizzazioni sono impostate come utenti normali senza accesso SUDO. Qualsiasi installazione esterna alla directory `$Home` non è permanente.

### <a name="editing-bashrc"></a>Modifica di .bashrc

Fare attenzione quando si modifica il file con estensione bashrc, poiché questa operazione può provocare errori imprevisti in Cloud Shell.

## <a name="powershell-limitations"></a>Limitazioni PowerShell

### <a name="azuread-module-name"></a>Nome modulo `AzureAD`

Il nome del modulo `AzureAD` è attualmente `AzureAD.Standard.Preview`. Il modulo fornisce la stessa funzionalità.

### <a name="sqlserver-module-functionality"></a>Funzionalità del modulo `SqlServer`

Il modulo `SqlServer` incluso in Cloud Shell include solo il supporto delle versioni precedenti di PowerShell Core. In particolare, `Invoke-SqlCmd` non è ancora disponibile.

### <a name="default-file-location-when-created-from-azure-drive"></a>Percorso file predefinito quando creato dall'unità Azure:

Usando dei cmdlet di PowerShell, gli utenti non possono creare i file sotto l'unità Azure. Quando gli utenti creano nuovi file con altri strumenti, ad esempio vim o nano, i file vengono salvati nella cartella `$HOME` per impostazione predefinita. 

### <a name="gui-applications-are-not-supported"></a>Le applicazioni GUI non sono supportate

Se l'utente esegue un comando che determina la generazione di una finestra di dialogo di Windows, come `Connect-AzureAD` o `Connect-AzureRmAccount`, viene visualizzato un messaggio di errore, ad esempio: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>Il completamento scheda arresta PSReadline in modo anomalo

Se la proprietà EditMode dell'utente in PSReadline è impostata su Emacs, l'utente tenta di visualizzare tutte le possibilità tramite il completamento scheda e le dimensioni della finestra sono troppo piccole per visualizzare tutte le possibilità, per cui PSReadline si arresterà in modo anomalo.

### <a name="large-gap-after-displaying-progress-bar"></a>Ampio spazio vuoto dopo aver visualizzato l'indicatore di stato

Se l'utente esegue un'azione che visualizza un indicatore di stato, come il completamento di una scheda nell'unità `Azure:`, è possibile che il cursore non sia impostato correttamente e che venga visualizzato uno spazio vuoto dove prima era presente l'indicatore di stato.

### <a name="random-characters-appear-inline"></a>Inline vengono visualizzati caratteri casuali

La sequenza di posizione del cursore, ad esempio `5;13R`, può essere visualizzata nell'input dell'utente.  Questi caratteri possono essere rimossi manualmente.

## <a name="next-steps"></a>Passaggi successivi

[Risoluzione dei problemi di Cloud Shell](troubleshooting.md) <br>
[Guida introduttiva di Bash](quickstart.md) <br>
[Avvio rapido di PowerShell](quickstart-powershell.md)
