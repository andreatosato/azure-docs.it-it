---
title: Introduzione ai dispositivi gemelli dell'hub IoT di Azure (.NET/.NET) | Microsoft Docs
description: Come usare i dispositivi gemelli dell'hub IoT di Azure per aggiungere tag e quindi usare una query dell'hub IoT. Usare Azure IoT SDK per dispositivi per Node.js per implementare l'app per dispositivo simulato e Azure IoT SDK per servizi per .NET per implementare un'app di servizio che aggiunge i tag ed esegue la query dell'hub IoT.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: dobett
ms.openlocfilehash: 340453448d38db66558e59edb845f2caf4454cf9
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43104151"
---
# <a name="get-started-with-device-twins-netnet"></a>Introduzione ai dispositivi gemelli (.NET/.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Al termine di questa esercitazione si avranno queste due app console .NET:

* **CreateDeviceIdentity**, un'app .NET che crea un'identità del dispositivo e la chiave di sicurezza associata per connettere l'app per dispositivo simulato.

* **AddTagsAndQuery**, un'app back-end .NET, che aggiunge tag ed effettua una query dei dispositivi gemelli.

* **ReportConnectivity.js**, un dispositivo .NET che simula un dispositivo che si connette all'hub IoT con l'identità del dispositivo creata prima e segnala la condizione della connettività.

> [!NOTE]
> L'articolo [Azure IoT SDK](iot-hub-devguide-sdks.md) riporta informazioni sui componenti Azure IoT SDK che consentono di compilare le app back-end e per dispositivi.
> 

Per completare l'esercitazione, sono necessari gli elementi seguenti:

* Visual Studio 2017.
* Un account Azure attivo. Se non si dispone di un account, è possibile crearne uno [gratuito](http://azure.microsoft.com/pricing/free-trial/) in pochi minuti.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Creare l'app di servizio

In questa sezione si crea un'app console .NET (usando C#) e si aggiungono i metadati della posizione al dispositivo gemello associato a **myDeviceId**. Viene quindi eseguita una query nei dispositivi gemelli archiviati nell'hub IoT selezionando i dispositivi situati negli Stati Uniti e quindi quelli che segnalano una rete cellulare.

1. In Visual Studio aggiungere un progetto desktop di Windows classico in Visual C# usando il modello di progetto **Applicazione console** . Assegnare al progetto il nome **AddTagsAndQuery**.
   
    ![Nuovo progetto desktop di Windows classico in Visual C#](./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png)

2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **AddTagsAndQuery** e quindi fare clic su **Gestisci pacchetti NuGet**.

3. Nella finestra **Gestisci pacchetti NuGet** selezionare **Sfoglia** e cercare **Microsoft.Azure.Devices**. Selezionare **Installa** per installare il pacchetto **Microsoft.Azure.Devices** e accettare le condizioni per l'utilizzo. Questa procedura scarica, installa e aggiunge un riferimento al pacchetto NuGet [Azure IoT SDK per dispositivi](https://www.nuget.org/packages/Microsoft.Azure.Devices/) e alle relative dipendenze.
   
    ![Finestra Gestione pacchetti NuGet](./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png)

4. Aggiungere le istruzione `using` seguenti all'inizio del file **Program.cs** :

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

5. Aggiungere i campi seguenti alla classe **Program** . Sostituire il valore del segnaposto con la stringa di connessione dell'hub IoT creato nella sezione precedente.

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. Aggiungere il metodo seguente alla classe **Program** :

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```
   
    L'oggetto **RegistryManager** espone tutti i metodi necessari per interagire con i dispositivi gemelli dal servizio. Il codice precedente inizializza prima di tutto l'oggetto **registryManager**, quindi recupera il dispositivo gemello per **myDeviceId** e infine ne aggiorna i tag con le informazioni sulla posizione desiderate.
   
    Dopo l'aggiornamento, il codice precedente esegue due query: la prima seleziona solo i dispositivi gemelli tra quelli situati nello stabilimento **Redmond43** e la seconda affina la query per selezionare solo i dispositivi che sono connessi anche tramite la rete cellulare.
   
    Si noti che il codice precedente, quando crea l'oggetto **query**, specifica un numero massimo di documenti restituiti. L'oggetto **query** contiene una proprietà booleana **HasMoreResults** che è possibile usare per richiamare i metodi **GetNextAsTwinAsync** più volte per recuperare tutti i risultati. Per i risultati che non sono dispositivi gemelli, ad esempio i risultati delle query di aggregazione, è disponibile un metodo chiamato **GetNextAsJson**.

7. Aggiungere infine le righe seguenti al metodo **Main** :

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

8. In Esplora soluzioni aprire **Imposta progetti di avvio** e assicurarsi che **Azione** per il progetto **AddTagsAndQuery** sia impostata su **Avvio**. Compilare la soluzione.

9. Eseguire l'applicazione facendo clic con il pulsante destro del mouse sul progetto **AddTagsAndQuery** e selezionando **Debug**, seguito da **Avvia nuova istanza**. Nei risultati si noterà un dispositivo per la query che cerca tutti i dispositivi situati in **Redmond43** e nessuno per la query che limita i risultati ai dispositivi che usano una rete cellulare.
   
    ![Risultati della query nella finestra](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

Nella sezione successiva si crea un'app per dispositivo che segnala le informazioni sulla connettività e modifica il risultato della query nella sezione precedente.

## <a name="create-the-device-app"></a>Creare l'app per dispositivo

In questa sezione si crea un'app console .NET che si connette all'hub come **myDeviceId** e quindi aggiorna le proprietà segnalate per poter contenere le informazioni relative alla connessione usando una rete cellulare.

1. In Visual Studio aggiungere un progetto desktop di Windows classico in Visual C# usando il modello di progetto **Applicazione console** . Denominare il progetto **ReportConnectivity**.
   
    ![Nuova app per il dispositivo di Windows classico in Visual C#](./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png)
    
2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **ReportConnectivity**, quindi fare clic su **Gestisci pacchetti NuGet...**.

3. Nella finestra **Gestione pacchetti NuGet** selezionare **Sfoglia** e cercare **Microsoft.Azure.Devices.Client**. Selezionare **Installa** per installare il pacchetto **microsoft.azure.devices.client** e accettare le condizioni d'uso. Questa procedura scarica, installa e aggiunge un riferimento al pacchetto [NuGet Azure IoT SDK per dispositivi](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) e alle relative dipendenze.
   
    ![App client della finestra Gestione pacchetti NuGet](./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png)

4. Aggiungere le istruzione `using` seguenti all'inizio del file **Program.cs** :

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. Aggiungere i campi seguenti alla classe **Program** . Sostituire il valore del segnaposto con la stringa di connessione del dispositivo annotato nella sezione precedente.

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Aggiungere il metodo seguente alla classe **Program** :

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    L'oggetto **Client** espone tutti i metodi necessari per interagire con i dispositivi gemelli dal dispositivo. Il codice riportato sopra inizializza l'oggetto **Client** e quindi recupera il dispositivo gemello per **myDeviceId**.

7. Aggiungere il metodo seguente alla classe **Program** :

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");
            
            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   Il codice sopra riportato aggiorna le proprietà segnalate di **myDeviceId** con le informazioni di connettività.

8. Aggiungere infine le righe seguenti al metodo **Main** :

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

9. In Esplora soluzioni aprire **Imposta progetti di avvio...** e assicurarsi che **Azione** per il progetto **ReportConnectivity** sia impostato su **Avvio**. Compilare la soluzione.

10. Eseguire l'applicazione facendo clic con il pulsante destro del mouse sul progetto **ReportConnectivity** e selezionando **Debug**, seguito da **Avvia nuova istanza**. Si dovrebbero vedere le informazioni gemelle e quindi l'invio della connettività come *proprietà segnalata*.
   
    ![Eseguire l'app per dispositivi per segnalare la connettività](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)
       
11. Ora che il dispositivo ha segnalato le informazioni sulla connettività, verrà visualizzato in entrambe le query. Eseguire l'app .NET **AddTagsAndQuery** per eseguire nuovamente la query. Questa volta **myDeviceId** verrà visualizzato nei risultati di entrambe le query.
   
    ![Connettività del dispositivo segnalata correttamente](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato configurato un nuovo hub IoT nel Portale di Azure ed è stata quindi creata un'identità del dispositivo nel registro di identità dell'hub IoT. Sono stati aggiunti i metadati del dispositivo come tag da un'app back-end ed è stata scritta un'app per dispositivo simulato per segnalare le informazioni sulla connettività del dispositivo nel dispositivo gemello. Si è anche appreso come effettuare una query di queste informazioni usando il linguaggio di query simile a SQL dell'hub IoT.

Per altre informazioni, vedere le risorse seguenti:

* per inviare dati di telemetria dai dispositivi, vedere l'esercitazione [Inviare dati di telemetria da un dispositivo a un hub IoT](quickstart-send-telemetry-dotnet.md);

* per configurare i dispositivi usando le proprietà desiderate del dispositivo gemello, vedere l'esercitazione [Usare le proprietà desiderate per configurare i dispositivi](tutorial-device-twins.md);

* per controllare i dispositivi in modo interattivo, ad esempio per attivare un ventilatore da un'app controllata dall'utente, vedere l'esercitazione [Usare i metodi diretti](quickstart-control-device-node.md).
