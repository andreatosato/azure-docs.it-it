---
title: Aggiungere un'entità servizio al ruolo di amministratore del server Azure Analysis Services | Microsoft Docs
description: Informazioni su come aggiungere un'entità servizio di automazione al ruolo di amministratore del server
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 779a202fccd3ff56c174ebc1ebbf3c4adfdd8c7b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441740"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Aggiungere un'entità servizio al ruolo di amministratore del server 

 Per automatizzare le attività di PowerShell, un'entità servizio deve avere privilegi di **amministratore del server** nel server Analysis Services gestito. Questo articolo descrive come aggiungere un'entità servizio al ruolo di amministratore del server in un server Azure Analysis Services.

## <a name="before-you-begin"></a>Prima di iniziare
Prima di completare questa attività, è necessario aver creato un'entità servizio registrata in Azure Active Directory.

[Creare un'entità servizio - Portale di Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Creare un'entità servizio - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Autorizzazioni necessarie
Per completare questa attività, è necessario avere autorizzazioni di [amministratore del server](analysis-services-server-admins.md) nel server Azure Analysis Services. 

## <a name="add-service-principal-to-server-administrators-role"></a>Aggiungere un'entità servizio al ruolo di amministratore del server

1. In SSMS connettersi al server Azure Analysis Services.
2. In **Proprietà server** > **Sicurezza** fare clic su **Aggiungi**.
3. In **Selezionare un utente o un gruppo** cercare l'app registrata per nome, selezionarla e quindi fare clic su **Aggiungi**.

    ![Cercare l'account dell'entità servizio](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Verificare l'ID account dell'entità servizio e quindi fare clic su **OK**.
    
    ![Cercare l'account dell'entità servizio](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Per le operazioni server con i cmdlet di AzureRM, l'entità servizio che esegue l'utilità di pianificazione deve appartenere anche al ruolo **Proprietario** per la risorsa in [Controllo degli accessi in base al ruolo di Azure](../role-based-access-control/overview.md). 

## <a name="related-information"></a>Informazioni correlate

* [Scaricare il modulo PowerShell di SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Scaricare SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


