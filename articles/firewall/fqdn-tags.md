---
title: Panoramica dei tag FQDN per Firewall di Azure
description: Informazioni sui tag FQDN in Firewall di Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 536c0915cae17aa6f4201c62eae5f5b077805274
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46999478"
---
# <a name="fqdn-tags-overview"></a>Panoramica dei tag FQDN

Un tag FQDN rappresenta un gruppo di nomi di dominio completi associati ai ben noti servizi Microsoft. È possibile usare un tag FQDN nelle regole delle applicazioni per consentire il necessario passaggio del traffico in uscita attraverso il firewall.

>[!NOTE]
>La funzionalità dei tag FQDN è attualmente disponibile solo in Azure PowerShell e REST.

Ad esempio, per consentire manualmente il passaggio del traffico di Windows Update attraverso il firewall, è necessario creare più regole dell'applicazione in base alla documentazione Microsoft. Con i tag FQDN, è possibile creare una regola dell'applicazione e includere il tag **Windows Update** permettendo al traffico diretto agli endpoint di Microsoft Windows Update di passare attraverso il firewall.

Non è possibile creare tag FQDN personalizzati, né è possibile specificare quali FQDN sono inclusi all'interno di un tag. Microsoft gestisce gli FQDN inclusi nel tag FQDN e aggiorna quest'ultimo in caso di modifica degli FQDN. 

<!--- screenshot of application rule with a FQDN tag.-->

La tabella seguente mostra i tag FQDN attuali che è possibile usare. Microsoft gestisce questi tag e periodicamente ne aggiungerà altri.

|Tag FQDN  |DESCRIZIONE  |
|---------|---------|
|Windows Update     |Consente l'accesso in uscita a Microsoft Update come descritto in [Come configurare un firewall per gli aggiornamenti software](https://technet.microsoft.com/library/bb693717.aspx).|
|Diagnostica per Windows|Consente l'accesso in uscita a tutti gli [endpoint di Diagnostica Windows](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|Microsoft Active Protection Service (MAPS)|Consente l'accesso in uscita a [MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|Ambiente del servizio app|Consente l'accesso in uscita al traffico della piattaforma Ambiente del servizio app. Questo tag non copre gli endpoint di archiviazione ed SQL specifici del cliente creati da Ambiente del servizio app. Tali endpoint devono essere abilitati tramite [Endpoint di servizio](../virtual-network/tutorial-restrict-network-access-to-resources.md) o aggiunti manualmente.|
|Backup di Azure|Consente l'accesso in uscita ai servizi Backup di Azure.

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come distribuire Firewall di Azure, vedere [Esercitazione: Distribuire e configurare Firewall di Azure tramite il portale di Azure](tutorial-firewall-deploy-portal.md).