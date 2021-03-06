---
title: Rimuovere il provider di risorse MySQL nello Stack di Azure | Documenti Microsoft
description: Informazioni su come è possibile rimuovere il provider di risorse MySQL dalla distribuzione di Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: d3a615e3b92a62709a787d0463dfa3148f14d07e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085792"
---
# <a name="remove-the-mysql-resource-provider"></a>Rimuovere il provider di risorse MySQL

Prima di rimuovere il provider di risorse MySQL, è necessario rimuovere tutte le dipendenze di provider. È necessario anche una copia del pacchetto di distribuzione che è stato utilizzato per installare il provider di risorse.

## <a name="dependency-cleanup"></a>Pulizia di dipendenza

Esistono diverse attività di pulizia da eseguire prima di eseguire lo script DeployMySqlProvider.ps1 per rimuovere il provider di risorse.

I tenant sono responsabili per le attività di pulizia seguenti:

* Eliminare tutti i database dal provider di risorse. (Eliminare i database tenant non eliminare i dati).
* Annullare la registrazione dallo spazio dei nomi di provider.

L'amministratore è responsabile per le seguenti attività di pulizia:

* Elimina il server di hosting dalla scheda di MySQL.
* Elimina tutti i piani che fanno riferimento l'Adapter di MySQL.
* Elimina le quote che sono associate con l'Adapter di MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Per rimuovere il provider di risorse MySQL

1. Verificare di aver rimosso tutte le esistente MySQL provider dipendenze della risorsa.

   >[!NOTE]
   >Disinstallazione del provider di risorse MySQL viene continuato anche se le risorse dipendenti utilizzano attualmente il provider di risorse.
  
2. Ottenere una copia del provider di risorse MySQL binario e quindi eseguire il programma di autoestrazione per estrarre il contenuto in una directory temporanea.
3. Ottenere una copia del provider di risorse SQL binario e quindi eseguire il programma di autoestrazione per estrarre il contenuto in una directory temporanea.
4. Aprire una finestra della console di PowerShell new con privilegi elevata e passare alla directory in cui sono stati estratti i file binari di risorsa provider MySQL.
5. Eseguire lo script DeployMySqlProvider.ps1 usando i parametri seguenti:
    - **Disinstallare**. Rimuove il provider di risorse e tutte le risorse associate.
    - **PrivilegedEndpoint**. L'indirizzo IP o nome DNS dell'endpoint con privilegi.
    - **CloudAdminCredential**. Le credenziali per l'amministratore del cloud, necessaria per accedere all'endpoint con privilegi.
    - **DirectoryTenantID**
    - **AzCredential**. Le credenziali per l'account amministratore del servizio Azure Stack. Utilizzare le stesse credenziali utilizzate per la distribuzione dello Stack di Azure.

## <a name="next-steps"></a>Passaggi successivi

[Servizi App offerta come PaaS](azure-stack-app-service-overview.md)
