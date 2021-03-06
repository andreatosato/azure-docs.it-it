---
title: Archiviazione BLOB di Azure nei dispositivi Azure IoT Edge | Microsoft Docs
description: Distribuire un modulo di archiviazione BLOB di Azure al dispositivo IoT Edge per archiviare i dati nei dispositivi perimetrali.
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 09/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b9e48eba4b46f024b056fe53b3b3df24feec802e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995670"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Archiviare i dati sui dispositivi perimetrali con l'archiviazione BLOB di Azure in IoT Edge (anteprima)

Archiviazione BLOB di Azure in IoT Edge offre una soluzione di archiviazione BLOB in blocchi nei dispositivi perimetrali. Un modulo di archiviazione BLOB nel dispositivo IoT Edge si comporta come un servizio BLOB di Azure, ma i BLOB in blocchi vengono archiviati localmente nel dispositivo IoT Edge. È possibile accedere ai BLOB usando gli stessi metodi di archiviazione di Azure SDK o bloccare le chiamate API BLOB che già utilizzate. 

Questo articolo fornisce istruzioni per la distribuzione di un'archiviazione BLOB di Azure nel contenitore di IoT Edge che esegue un servizio BLOB nel dispositivo IoT Edge. 

>[!NOTE]
>L'archiviazione BLOB di Azure in IoT Edge è in versione di [anteprima pubblica](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## <a name="prerequisites"></a>Prerequisiti

Un dispositivo Azure IoT Edge:

* È possibile usare il computer per lo sviluppo o una macchina virtuale come dispositivo perimetrale seguendo la procedura illustrata nella guida introduttiva per dispositivi [Linux](quickstart-linux.md) o [Windows](quickstart.md).
* L'archiviazione BLOB di Azure nel modulo di IoT Edge supporta le configurazioni di dispositivo seguenti:

   | Sistema operativo | Architettura |
   | ---------------- | ------------ |
   | Ubuntu Server 16.04 | AMD64 |
   | Ubuntu Server 18.04 | AMD64 |
   | Windows 10 IoT Core (aggiornamento di ottobre) | AMD64 |
   | Windows 10 IoT Enterprise (aggiornamento di ottobre) | AMD64 |
   | Windows Server 2019 | AMD64 |
   | Raspbian Stretch | ARM32 |

Risorse cloud:

* Un [hub IoT](../iot-hub/iot-hub-create-through-portal.md) di livello Standard in Azure. 


## <a name="deploy-blob-storage-to-your-device"></a>Distribuire l'archiviazione BLOB al dispositivo

L'archiviazione BLOB di Azure su IoT Edge fornisce tre immagini dei contenitori standard, due per i contenitori di Linux (architetture AMD64 e ARM32) e una per i contenitori di Windows (AMD64). Quando si usa una di queste immagini di modulo per distribuire l'archiviazione BLOB nel dispositivo IoT Edge, si forniscono tre tipi di informazioni per configurare l'istanza del modulo per il dispositivo:

* Un **nome dell'account** e la **chiave dell'account**. Per essere coerente con l'archiviazione di Azure, i moduli di archiviazione BLOB usano nomi e chiavi di account per gestire l'accesso. I nomi di account devono essere compresi tra i 3 e i 24 caratteri e includere lettere minuscole e numeri. Le chiavi dell'account devono essere codificate in Base64 e essere lunghe 64 byte. È possibile generare una chiave con strumenti quali [GeneratePlus](https://generate.plus/en/base64).
* Un'**opzione di archiviazione locale**. Nel modulo di archiviazione BLOB vengono archiviati BLOB in locale nel dispositivo IoT Edge, in modo da rendere persistenti i BLOB se il modulo viene arrestato o riavviato. Dichiarare il [volume](https://docs.docker.com/storage/volumes
) esistente o un percorso di cartella locale in cui i BLOB devono essere archiviati nel dispositivo. 

Esistono diversi modi per distribuire moduli in un dispositivo IoT Edge e tutti sono idonei all'archiviazione BLOB di Azure nei moduli di IoT Edge. I due metodi più semplici consistono nell'usare i modelli di Visual Studio Code o il portale di Azure. 

### <a name="azure-portal"></a>Portale di Azure

Per distribuire l'archiviazione BLOB tramite il portale di Azure, seguire i passaggi descritti in [Distribuire i moduli di Azure IoT Edge dal portale di Azure](how-to-deploy-modules-portal.md). Prima di passare alla distribuzione del modulo, copiare l'URI dell'immagine e preparare il contenitore di creazione opzioni a seconda del sistema operativo del contenitore. Usare questi valori nella sezione **Configurare un manifesto della distribuzione** dell'articolo di distribuzione. 

Specificare l'URI dell'immagine per il modulo di archiviazione BLOB: **mcr.microsoft.com/azure-blob-storage**. 
   
Usare il seguente modello JSON per il campo **Opzioni di creazione del contenitore**. Configurare il codice JSON con il nome account di archiviazione, la chiave account di archiviazione e l'associazione della directory di archiviazione.  
   
   ```json
   {
       "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
       ],
       "HostConfig":[
           "Binds":[
               "<storage directory bind>"
           ],
           "PortBindings":{
               "11002/tcp":[{"HostPort":"11002"}]
           }
       ]
   }
   ```   
   
Nelle opzioni di creazione JSON, aggiornare `\<your storage account name\>` con qualsiasi nome. Aggiornare `\<your storage account key\>` con una chiave Base64 a 64 byte. È possibile generare una chiave con strumenti quali [GeneratePlus](https://generate.plus/en/base64) che consente di selezionare la lunghezza in byte. Queste credenziali verranno usate per accedere all'archiviazione BLOB da altri moduli.

Nelle opzioni di creazione JSON, aggiornare `<storage directory bind>` in base al sistema operativo del contenitore. Specificare il nome di un [volume](https://docs.docker.com/storage/volumes/) o il percorso assoluto in una directory nel dispositivo IoT Edge in cui si desidera che il modulo BLOB archivi i suoi dati.  

   * Contenitori Linux: **\<percorso di archiviazione >:/blobroot**. Ad esempio, /srv/containerdata:/blobroot. O, my-volume:/blobroot. 
   * Contenitori Windows: **\<percorso di archiviazione>:C:/BlobRoot**. Ad esempio, C:/ContainerData:C:/BlobRoot. In alternativa, my-volume: C:/blobroot. 


Non è necessario specificare le credenziali del Registro di sistema per accedere all'archiviazione BLOB di Azure in IoT Edge, e non è necessario dichiarare tutte le route per la distribuzione. 

### <a name="visual-studio-code-templates"></a>Modelli di Visual Studio Code

Azure IoT Edge fornisce modelli di Visual Studio Code per assistere allo sviluppo di soluzioni per dispositivi perimetrali. Per questi passaggi, è necessario che [Visual Studio Code](https://code.visualstudio.com/) sia installato nel computer di sviluppo e configurato con l'[estensione Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Seguire i passaggi seguenti per creare una nuova soluzione IoT Edge con un modulo di archiviazione BLOB e configurare il manifesto della distribuzione. 

1. Selezionare **Visualizza** > **Riquadro comandi**. 

2. Nel riquadro comandi immettere ed eseguire il comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: Nuova soluzione IoT Edge). 

3. Seguire le istruzioni visualizzate per creare una nuova soluzione: 

   1. **Selezionare la cartella** - Passare alla cartella in cui si vuole creare la nuova soluzione.  
   
   2. **Specificare un nome della soluzione** - Immettere un nome per la soluzione o accettare quello predefinito.
   
   3. **Selezionare un modello di modulo** -Scegliere **modulo esistente (URL di invio completo dell'immagine)**.
   
   4. **Specificare un nome di modulo** -Immettere un nome facilmente riconoscibile per il modulo, ad esempio **azureBlobStorage**.
   
   5. **Specificare l'immagine Docker per il modulo** - Specificare l'URI dell'immagine: **mcr.microsoft.com/azure-blob-storage**

VS Code usa le informazioni specificate per creare una soluzione IoT Edge e quindi la carica in una nuova finestra. 

Il modello di soluzione crea un modello di manifesto della distribuzione che include l'immagine del modulo di archiviazione BLOB, ma è necessario configurare le opzioni per la creazione del modulo. 

1. Aprire **deployment.template.json** nell'area di lavoro della nuova soluzione e trovare la sezione **moduli**. 

2. Eliminare il modulo **tempSensor**, perché non necessario per questa distribuzione. 

3. Copiare e incollare il codice seguente nel campo **createOptions** del modulo di archiviazione BLOB: 

   ```json
   {\"Env\": [\"LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME\",\" LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY\"],\"HostConfig\": {\"Binds\": [\"<storage directory bind>\"],\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"11002\"}]}}}
   ```

   ![Aggiornare le opzioni di creazione del modulo](./media/how-to-store-data-blob/create-options.png)

4. Nelle opzioni di creazione JSON, aggiornare `<storage directory bind>` in base al sistema operativo del contenitore. Specificare il nome di un [volume](https://docs.docker.com/storage/volumes/) o il percorso assoluto in una directory nel dispositivo IoT Edge in cui si desidera che il modulo BLOB archivi i suoi dati.  

   * Contenitori Linux: **\<percorso di archiviazione >:/blobroot**. Ad esempio, /srv/containerdata:/blobroot. O, my-volume:/blobroot.
   * Contenitori Windows: **\<percorso di archiviazione>:C:/BlobRoot**. Ad esempio, C:/ContainerData:C:/BlobRoot. In alternativa, my-volume: C:/blobroot.

5. Salvare **deployment.template.json**.

6. Aprire **.env** nell'area di lavoro della soluzione. 

7. Non è necessario immettere i valori del Registro di sistema del contenitore per l'immagine di archiviazione BLOB, perché è pubblicamente disponibile. Aggiungere invece due nuove variabili di ambiente: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Specificare qualsiasi identificativo per il nome dell'account di archiviazione e indicare una chiave Base64 a 64 byte della chiave account di archiviazione. È possibile generare una chiave con strumenti quali [GeneratePlus](https://generate.plus/en/base64). Queste credenziali verranno usate per accedere all'archiviazione BLOB da altri moduli. 

9. Salvare **.env**. 

10. Fare clic con il pulsante destro del mouse su **deployment.template.json** e selezionare **Generare manifesto di distribuzione di IoT Edge**. 

Visual Studio Code richiede le informazioni fornite in deployment.template.json e con estensione .env e le usa per creare un nuovo file del manifesto di distribuzione. Il manifesto di distribuzione viene creato in una nuova cartella **config** nell'area di lavoro della soluzione. Dopo aver creato tale file, è possibile seguire i passaggi descritti in [Distribuire i moduli di Azure IoT Edge da Visual Studio Code](how-to-deploy-modules-vscode.md) oppure [Distribuire i moduli di Azure IoT Edge con l'interfaccia della riga di comando 2.0 di Azure](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Connettere il modulo di archiviazione BLOB

È possibile usare il nome dell'account e la chiave dell'account configurati per il modulo per accedere all'archiviazione BLOB nel dispositivo IoT Edge. 

Specificare il dispositivo IoT Edge come endpoint BLOB per qualsiasi richiesta di archiviazione effettuata. È possibile [Creare una stringa di connessione per un endpoint di archiviazione esplicita](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) usando le informazioni del dispositivo IoT Edge e il nome dell'account che è stato configurato. 

L'endpoint BLOB per l'archiviazione BLOB di Azure in IoT Edge è `http://<IoT Edge device hostname>:11002/<account name>`. 

## <a name="deploy-multiple-instances"></a>Distribuire più istanze

Se si desidera distribuire più istanze di archiviazione BLOB di Azure in IoT Edge, è sufficiente modificare la HostPort a cui si associa il modulo. I moduli di archiviazione BLOB espongono sempre la porta 11002 nel contenitore, ma è possibile dichiarare a quale porta sono associati nell'host. 

Modificare il modulo creare opzioni per modificare il valore HostPort:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Quando ci si connette ai moduli di archiviazione BLOB aggiuntivi, modificare l'endpoint in modo che punti alla porta host aggiornata. 

### <a name="try-it-out"></a>Provare il servizio

La documentazione dell'archiviazione BLOB di Azure include guide introduttive che forniscono il codice di esempio in linguaggi diversi. È possibile eseguire questi esempi per testare l'archiviazione BLOB di Azure in IoT Edge, modificando l'endpoint BLOB in modo che punti al modulo di archiviazione BLOB.

Le seguenti guide introduttive utilizzano linguaggi che sono supportati anche da IoT Edge, in modo che sia possibile distribuirle come moduli di IoT Edge con il modulo di archiviazione BLOB:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.JS](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="supported-storage-operations"></a>Operazioni di archiviazione supportate

I moduli di archiviazione BLOB in IoT Edge usano gli stessi SDK di archiviazione di Azure e sono coerenti con la versione 2018-03-28 dell'API di archiviazione di Azure per gli endpoint di BLOB in blocchi. Le versioni successive dipendono dalle esigenze dei clienti. 

Non tutte le operazioni di archiviazione BLOB di Azure sono supportate dall'archiviazione BLOB di Azure in IoT Edge. Le sezioni seguenti descrivono in dettaglio quali operazioni sono e non sono supportate. 

### <a name="account"></a>Account

Supportate: 
* Elencare i contenitori

Non supportate: 
* Ottenere e impostare le proprietà del servizio BLOB
* Richiedere un BLOB preliminare
* Ottenere le statistiche del servizio BLOB
* Ottenere le informazioni sull'account

### <a name="containers"></a>Contenitori

Supportate: 
* Creare ed eliminare un contenitore
* Ottenere i metadati ele proprietà del contenitore
* Elencare i BLOB

Non supportate: 
* Ottenere e impostare il contenitore ACL
* Lease del contenitore
* Impostare i metadati del contenitore

### <a name="blobs"></a>Blobs

Supportate: 
* Inserire, ottenere ed eliminare BLOB
* Ottenere e impostare le proprietà BLOB
* Ottenere e impostare i metadati BLOB

Non supportate: 
* Lease del BLOB
* Snapshot di BLOB
* Copiare e interrompere la copia di BLOB
* Annullare l'eliminazione di BLOB
* Impostare il livello di BLOB

### <a name="block-blobs"></a>BLOB in blocchi

Supportate: 
* Inserire il blocco
* Inserire e ottenere l'elenco del blocco

Non supportate:
* Inserire il blocco dall'URL

## <a name="next-steps"></a>Passaggi successivi

Informazioni sull'[Archiviazione BLOB di Azure](../storage/blobs/storage-blobs-introduction.md)

