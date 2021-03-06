---
title: Supporto di Archiviazione tabelle di Azure in Azure Cosmos DB | Microsoft Docs
description: Informazioni sull'interazione tra l'API di tabella di Azure Cosmos DB e le tabelle di Archiviazione di Azure.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 114286b45df5f47e81bd2b990c8b50c8b7b7a482
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43185367"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Sviluppo con l'API di tabella di Azure Cosmos DB e Archiviazione tabelle di Azure

L'API di tabella di Azure Cosmos DB e Archiviazione tabelle di Azure condividono lo stesso modello di dati di tabella ed espongono le stesse operazioni di creazione, eliminazione, aggiornamento ed esecuzione di query tramite i relativi SDK. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Sviluppo con l'API di tabella di Azure Cosmos DB

L'[API Table di Azure Cosmos DB](table-introduction.md) attualmente dispone di quattro SDK per lo sviluppo: 
- [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget) .NET SDK. Questa libreria include le stesse classi e firme di metodi disponibili nella versione pubblica dell'[SDK di Archiviazione di Microsoft Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ma permette anche di connettersi agli account Azure Cosmos DB tramite l'API Table. Si noti che la libreria `Microsoft.Azure.CosmosDB.Table` è attualmente disponibile solo per .NET Standard. Non è ancora disponibile per .NET Core.
- [Python SDK](table-sdk-python.md). Il nuovo SDK Python di Azure Cosmos DB è l'unico SDK che supporta l'archiviazione tabelle di Azure in Python. Questo SDK si connette sia ad Archiviazione tabelle di Azure che all'API Table di Azure Cosmos DB.
- [Java SDK](table-sdk-java.md). Questo SDK di Archiviazione di Azure è in grado di connettersi agli account di Azure Cosmos DB tramite l'API Table.
- [Node.js SDK](table-sdk-nodejs.md). Questo SDK di Archiviazione di Azure è in grado di connettersi agli account di Azure Cosmos DB tramite l'API Table.

Per altre informazioni sull'uso dell'API di tabella, vedere l'articolo relativo alle [domande frequenti sullo sviluppo con l'API di tabella](faq.md#table).

## <a name="developing-with-azure-table-storage"></a>Sviluppo con Archiviazione tabelle di Azure

Archiviazione tabelle di Azure include questi SDK disponibili per lo sviluppo:

- [WindowsAzure.Storage .NET SDK](https://www.nuget.org/packages/WindowsAzure.Storage/). Questa libreria consente di utilizzare il servizio di archiviazione tabelle.
- [Python SDK](table-sdk-python.md). Anche Azure Cosmos DB Table SDK per Python supporta il servizio di archiviazione tabelle.
- [Azure Storage per Java](https://github.com/azure/azure-storage-java). Questo SDK di archiviazione di Azure fornisce una libreria client i n Java per l'utilizzo di Archiviazione tabelle di Azure.
- [Node.js SDK](table-sdk-nodejs.md). Questo SDK fornisce un pacchetto Node.js e una libreria client JavaScript compatibile con browser per utilizzare il servizio di archiviazione tabelle.
- [Modulo di PowerShell AzureRmStorageTable](https://www.powershellgallery.com/packages/AzureRmStorageTable/1.0.0.7). Questo modulo di PowerShell include cmdlet che utilizzano le tabelle di archiviazione.
- [Libreria client di archiviazione di Azure per C++](https://github.com/Azure/azure-storage-cpp/). Questa libreria consente di creare applicazioni per Archiviazione di Azure.
- [Libreria client delle tabelle di Archiviazione di Azure per Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Questo progetto fornisce un pacchetto Ruby che rende più semplice accedere al servizio tabelle di Archiviazione di Azure.
- [Libreria client PHP delle tabelle di Archiviazione di Azure](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Questo progetto fornisce una libreria client PHP che rende più semplice accedere al servizio tabelle di Archiviazione di Azure.


   





