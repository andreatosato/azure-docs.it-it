---
title: Informazioni sull'endpoint predefinito dell'hub IoT di Azure | Microsoft Docs
description: "Guida per gli sviluppatori: descrizione dell'uso dell'endpoint predefinito compatibile con l'hub eventi per la lettura dei messaggi da dispositivo a cloud."
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 767c91e4926e553b63b8331ac99edcd7823d2c13
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44055016"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Leggere messaggi da dispositivo a cloud dall'endpoint predefinito

Per impostazione predefinita, i messaggi vengono instradati all'endpoint per servizi predefinito (**messaggi/eventi**) compatibile con [Hub eventi][lnk-event-hubs]. Questo endpoint è attualmente esposto solo con il protocollo [AMQP][lnk-amqp] sulla porta 5671. Un hub IoT espone le proprietà seguenti per consentire il controllo dell'endpoint di messaggistica predefinito **messages/events**, compatibile con Hub eventi.

| Proprietà            | DESCRIZIONE |
| ------------------- | ----------- |
| **Numero di partizioni** | Impostare questa proprietà in fase di creazione per definire il numero di [partizioni][lnk-event-hub-partitions] per l'inserimento di eventi da dispositivo a cloud. |
| **Tempo di conservazione**  | Questa proprietà specifica per quanti giorni i messaggi vengono conservati dall'hub IoT. Il valore predefinito è un giorno, ma può essere aumentato a sette giorni. |

L'hub IoT consente inoltre di gestire i gruppi di consumer nell'endpoint di ricezione predefinito da dispositivo a cloud.

Per impostazione predefinita, tutti i messaggi che non corrispondono in modo esplicito a una regola di routing vengono scritti nell'endpoint predefinito. Se si disattiva questa route di fallback, i messaggi che non corrispondono in modo esplicito a una regola di routing verranno eliminati.

È possibile modificare il tempo di conservazione a livello di codice con le [API REST del provider di risorse dell'hub IoT][lnk-resource-provider-apis] oppure con il [portale di Azure][lnk-management-portal].

L'hub IoT espone l'endpoint **messaggi/eventi** predefinito per permettere ai servizi back-end di leggere i messaggi da dispositivo a cloud ricevuti dall'hub. L'endpoint è compatibile con Hub eventi e consente quindi di usare uno dei meccanismi supportati da tale servizio per la lettura dei messaggi.

## <a name="read-from-the-built-in-endpoint"></a>Eseguire la lettura dall'endpoint predefinito

Quando si usa [Azure Service Bus SDK per .NET][lnk-servicebus-sdk] o [Hub eventi - Host processore di eventi][lnk-eventprocessorhost], è possibile usare qualsiasi stringa di connessione dell'hub IoT con le autorizzazioni corrette. Usare quindi **messaggi/eventi** come nome dell'hub eventi.

Quando si usano SDK o integrazioni del prodotto non compatibili con l'hub IoT, è necessario recuperare un endpoint e un nome compatibili con l'hub eventi:

1. Accedere al [portale di Azure][lnk-management-portal] e passare all'hub IoT.
1. Fare clic su **Endpoint**.
1. Nella sezione **Endpoint predefiniti** fare clic su **Eventi**. 
1. Si apre una pagina delle proprietà che contiene i valori seguenti: **endpoint compatibile con l'hub eventi**, **nome compatibile con l'hub eventi**, **partizioni**, **tempo di conservazione** e **gruppi di consumer**.

    ![Impostazioni da dispositivo a cloud][img-eventhubcompatible]

L'SDK dell'hub IoT richiede il nome dell'endpoint dell'hub IoT, ossia **messagi/eventi** come mostrato in **Endpoint**.

Se l'SDK usato richiede un valore **Nome host** o **Spazio dei nomi**, rimuovere lo schema da **Endpoint compatibile con l'hub eventi**. Ad esempio, se l'endpoint compatibile con l'hub eventi è **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, il **Nome host** sarà **iothub-ns-myiothub-1234.servicebus.windows.net**. Lo **spazio dei nomi** sarà **iothub-ns-myiothub-1234**.

È quindi possibile usare qualsiasi criterio di accesso condiviso con autorizzazioni **ServiceConnect** per connettersi all'hub eventi specificato.

Se si deve compilare una stringa di connessione dell'hub eventi con le informazioni precedenti, è possibile usare il modello seguente:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

Di seguito sono elencati gli SDK e le integrazioni che è possibile usare con gli endpoint compatibili con l'hub eventi esposti dall'hub IoT:

* [Client Java di Hub eventi](https://github.com/Azure/azure-event-hubs-java).
* [Spout di Apache Storm](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). È possibile visualizzare il [codice sorgente dello spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) su GitHub.
* [Integrazione Apache Spark](../hdinsight/spark/apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sugli endpoint dell'hub IoT, vedere [Endpoint dell'hub IoT][lnk-endpoints].

Le [guide introduttive][lnk-get-started] illustrano come inviare messaggi da dispositivo a cloud da dispositivi simulati e come leggere i messaggi dall'endpoint predefinito. Per altri dettagli, vedere l'esercitazione [Elaborare messaggi da dispositivo a cloud dell'hub IoT usando route][lnk-d2c-tutorial].

Per instradare i messaggi da dispositivo a cloud verso endpoint personalizzati, vedere [Use message routes and custom endpoints for device-to-cloud messages][lnk-custom] (Usare route messaggi ed endpoint personalizzati per i messaggi da dispositivo a cloud).

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph
[lnk-amqp]: https://www.amqp.org/
