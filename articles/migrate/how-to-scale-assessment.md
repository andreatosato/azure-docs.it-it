---
title: Individuazione e valutazione della scalabilità tramite Azure Migrate | Microsoft Docs
description: Questo articolo descrive come valutare un elevato numero di computer locali usando il servizio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/25/2018
ms.author: raynew
ms.openlocfilehash: 1f049b3e05ac17e416379762a0bced8340ae25d5
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666544"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Individuare e valutare un ambiente VMware di grandi dimensioni

Azure Migrate ha un limite di 1500 computer per progetto; questo articolo descrive come valutare un elevato numero di macchine virtuali (VM) locali con [Azure Migrate](migrate-overview.md).   

## <a name="prerequisites"></a>Prerequisiti

- **VMware**: le macchine virtuali di cui si intende eseguire la migrazione devono essere gestite dal server vCenter versione 5.5, 6.0 o 6.5. È inoltre necessario disporre di un unico host ESXi versione 5.0 o successiva per distribuire la macchina virtuale che funge da agente di raccolta.
- **Account vCenter**: occorre avere un account di sola lettura per accedere al server vCenter. Azure Migrate usa questo account per individuare le macchine virtuali.Azure Migrate usa questo account per individuare le macchine virtuali locali.
- **Autorizzazioni**: nel server vCenter è necessario disporre delle autorizzazioni per creare una macchina virtuale importando un file in formato OVA.
- **Impostazioni delle statistiche**: prima di iniziare la distribuzione, è consigliabile impostare le statistiche del server vCenter sul livello 3. Il livello delle statistiche deve essere impostato su 3 per ogni intervallo di raccolta giornaliero, settimanale e mensile. Se si imposta un livello inferiore a 3 per uno dei tre intervalli di raccolta, viene eseguita la valutazione, ma non vengono raccolti i dati sulle prestazioni per l'archiviazione e la rete. I consigli relativi alle dimensioni si baseranno quindi sui dati delle prestazioni per la CPU e la memoria e sui dati di configurazione per le schede del disco e di rete.


### <a name="set-up-permissions"></a>Impostare le autorizzazioni

Azure Migrate deve avere accesso ai server VMware per l'individuazione automatica delle VM per la valutazione. L'account VMware necessita delle autorizzazioni seguenti:

- Tipo di utente: almeno un utente di sola lettura
- Autorizzazioni: Data Center object (Oggetto data center) > Propagate to Child Object (Propaga a oggetto figlio), role=Read-only (ruolo=Sola lettura)
- Dettagli: l'utente viene assegnato a livello di data center e ha accesso a tutti gli oggetti nel data center.
- Per limitare l'accesso, assegnare il ruolo No access (Nessun accesso) con Propagate to child object (Propaga a oggetto figlio) agli oggetti figlio (host vSphere, archivi dati, VM e reti).

Se si esegue la distribuzione in un ambiente tenant, ecco un modo per impostare questa funzionalità:

1.  Creare un utente per ogni tenant e, tramite [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal), assegnare autorizzazioni di sola lettura a tutte le macchine virtuali appartenenti a un particolare tenant. Usare quindi queste credenziali per l'individuazione. RBAC assicura che l'utente vCenter corrispondente possa accedere solo al tenant della macchina virtuale specifica.
2. È possibile impostare RBAC per utenti tenant diversi, come descritto nell'esempio seguente per Utente#1 e Utente#2:

    - In **Nome utente** e **Password** specificare le credenziali dell'account di sola lettura che verranno usate dall'agente di raccolta per individuare le macchine virtuali in
    - Datacenter1 - assegnare autorizzazioni di sola lettura per Utente#1 e Utente#2. Non propagare tali autorizzazioni per tutti gli oggetti figlio, perché le autorizzazioni verranno impostate in macchine virtuali singole.

      - VM1 (Tenant#1) (autorizzazione di sola lettura all'Utente#1)
      - VM2 (Tenant#1) (autorizzazione di sola lettura all'Utente#1)
      - VM3 (Tenant#2) (autorizzazione di sola lettura all'Utente#2)
      - VM4 (Tenant#2) (autorizzazione di sola lettura all'Utente#2)

   - Se si esegue l'individuazione usando le credenziali dell'Utente#1, verranno individuate solo VM1 e VM2.

## <a name="plan-your-migration-projects-and-discoveries"></a>Pianificare i progetti di migrazione e le individuazioni

Un singolo agente di raccolta Azure Migrate supporta l'individuazione di più server vCenter (uno dopo l'altro) e supporta anche l'individuazione su più progetti di migrazione (uno dopo l'altro). L'agente di raccolta opera in un modello fire-and-forget; una volta fatta l'individuazione, è possibile usare lo stesso agente di raccolta per raccogliere dati da un server vCenter diverso o inviarli a un progetto di migrazione diverso.

Pianificare le individuazioni e le valutazioni in base ai limiti seguenti:

| **Entità** | **Limite di computer** |
| ---------- | ----------------- |
| Project    | 1.500             |
| Individuazione  | 1.500             |
| Valutazione | 1.500             |

Tenere presente le considerazioni seguenti sulla pianificazione:

- Quando si esegue un'individuazione tramite l'agente di raccolta di Azure Migrate, è possibile impostare l'ambito di individuazione su una cartella, un data center, un cluster o un host del server vCenter.
- Per eseguire più individuazioni, verificare nel server vCenter che le macchine virtuali che si vuole individuare siano incluse in cartelle, data center, cluster o host che supportano il limite di 1.500 computer.
- Ai fini della valutazione, è consigliabile tenere i computer con interdipendenze all'interno dello stesso progetto e della stessa valutazione. Nel server vCenter verificare quindi che i computer dipendenti si trovino nello stesso data center, la stessa cartella o lo stesso cluster per la valutazione.

A seconda dello scenario, è possibile dividere le individuazioni come stabilito di seguito:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Più server vCenter con meno di 1500 macchine virtuali

Se si dispone di più server vCenter nell'ambiente e il numero totale di macchine virtuali è inferiore a 1500, è possibile usare un singolo agente di raccolta e un singolo progetto di migrazione per individuare tutte le macchine virtuali in tutti i server vCenter. Poiché l'agente di raccolta consente di individuare un server vCenter alla volta, è possibile eseguire lo stesso agente di raccolta su tutti i server vCenter, uno dopo l'altro e puntare l'agente di raccolta allo stesso progetto di migrazione. Dopo aver completato tutte le individuazioni è possibile creare le valutazioni per i computer.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Più server vCenter con più di 1500 macchine virtuali

Se si dispone di più server vCenter con meno di 1500 macchine virtuali per ciascun server vCenter, ma più di 1500 macchine virtuali tra tutti i server vCenter, è necessario creare più progetti di migrazione (un progetto di migrazione può contenere solo 1500 macchine virtuali). È possibile ottenere questo risultato creando un progetto di migrazione per server vCenter e suddividere le individuazioni. È possibile usare un singolo agente di raccolta per individuare ciascun server vCenter (uno dopo l'altro). Se si desidera che le individuazioni inizino contemporaneamente, è anche possibile distribuire più appliance ed eseguire le individuazioni in parallelo.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Più di 1500 macchine in un singolo server vCenter

Se si hanno più di 1500 macchine virtuali in un singolo server vCenter, è necessario suddividere l'individuazione in più progetti di migrazione. Per suddividere le individuazioni, è possibile sfruttare il campo Ambito nell'appliance e specificare l'host, il cluster, la cartella o il data center che si desidera individuare. Ad esempio, se si dispone di due cartelle nel server vCenter, una con 1000 macchine virtuali (Cartella1) e l'altro con 800 macchine virtuali (Cartella2), è possibile usare un singolo agente di raccolta ed eseguire due individuazioni. Nella prima individuazione, è possibile specificare Cartella1 come ambito e indirizzarla al primo progetto di migrazione. Al termine della prima individuazione è possibile usare lo stesso l'agente di raccolta, modificare l'ambito con Cartella2 e i dettagli del secondo progetto di migrazione ed eseguire la seconda individuazione.

### <a name="multi-tenant-environment"></a>Ambienti multi-tenant

Se si dispone di un ambiente condiviso da più tenant e non si desidera individuare le macchine virtuali di un tenant nella sottoscrizione di un altro tenant, è possibile usare il campo Ambito nell'appliance dell'agente di raccolta per impostare l'ambito di individuazione. Se i tenant condividono gli host, creare una credenziale con accesso in sola lettura alle macchine virtuali che appartengono al tenant specifico e quindi usare le credenziali nell'appliance dell'agente di raccolta e specificare Ambito come host per eseguire l'individuazione. In alternativa, è inoltre possibile creare cartelle nel server vCenter (si supponga cartella1 per tenant1 e cartella2 per tenant2), nell'host condiviso, spostare le macchine virtuali per tenant1 nella cartella1 e per tenant2 nella cartella2 e quindi impostare di conseguenza l'ambito delle individuazioni nell'agente di raccolta specificando la cartella appropriata.

## <a name="discover-on-premises-environment"></a>Individuazione dell'ambiente locale

Quando il piano è pronto, è possibile avviare l'individuazione delle macchine virtuali locali:

### <a name="create-a-project"></a>Creare un progetto

Creare un progetto di Azure Migrate in base alle necessità:

1. Nel portale di Azure fare clic su **Crea una risorsa**.
2. Cercare **Azure Migrate** e selezionare il servizio **Azure Migrate** nei risultati della ricerca. Selezionare quindi **Crea**.
3. Specificare un nome di progetto e la sottoscrizione di Azure per il progetto.
4. Creare un nuovo gruppo di risorse.
5. Specificare il percorso in cui si vuole creare il progetto e quindi selezionare **Crea**. Si noti che è comunque possibile valutare le macchine virtuali in un percorso di destinazione diverso. Il percorso specificato per il progetto viene usato per archiviare i metadati raccolti da macchine virtuali locali.

### <a name="set-up-the-collector-appliance"></a>Configurare l'appliance dell'agente di raccolta

Azure Migrate crea una macchina virtuale locale definita appliance dell'agente di raccolta. Questa macchina virtuale individua le macchine virtuali VMware locali e invia i relativi metadati al servizio Azure Migrate. Per configurare l'appliance dell'agente di raccolta, si scarica un file con estensione ova e lo si importa nell'istanza del server vCenter locale.

#### <a name="download-the-collector-appliance"></a>Scaricare l'appliance dell'agente di raccolta

In presenza di più progetti, occorre scaricare l'appliance dell'agente di raccolta una sola volta nel server vCenter. Dopo avere scaricato e configurato l'appliance, la si esegue per ogni progetto e si specificano la chiave e l'ID di progetto univoci.

1. Nel progetto di Azure Migrate selezionare **Attività iniziali** > **Individua e valuta** > **Individua macchine virtuali**.
2. In **Individua macchine virtuali** fare clic su **Scarica** per scaricare il file con estensione ova.
3. In **Copiare le credenziali del progetto** copiare l'ID e la chiave del progetto. Queste informazioni sono necessarie per configurare l'agente di raccolta.


#### <a name="verify-the-collector-appliance"></a>Verificare l'appliance dell'agente di raccolta

Prima di distribuire il file con estensione ova, verificarne la sicurezza:

1. Nel computer in cui è stato scaricato il file aprire una finestra di comando con privilegi di amministratore.

2. Eseguire il comando seguente per generare il valore hash per il file con estensione ova:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Esempio di utilizzo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Verificare che il valore hash generato corrisponda alle impostazioni seguenti.

    Per OVA versione 1.0.9.14

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
    SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
    SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c
    
    Per OVA versione 1.0.9.12

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | d0363e5d1b377a8eb08843cf034ac28a
    SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
    SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

    Per OVA versione 1.0.9.8

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | b5d9f0caf15ca357ac0563468c2e6251
    SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
    SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

    Per OVA versione 1.0.9.7

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

    Per OVA versione 1.0.9.5

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

    Per OVA versione 1.0.9.2

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

### <a name="create-the-collector-vm"></a>Creare la macchina virtuale dell'agente di raccolta

Importare il file scaricato nel server vCenter:

1. Nella console di vSphere Client selezionare **File** > **Distribuire il modello OVF**.

    ![Distribuire il modello OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. Nella procedura guidata Distribuire il modello OVF > **Origine** specificare il percorso del file con estensione ova.
3. In **Name** (Nome) e **Location** (Posizione) specificare un nome descrittivo per la macchina virtuale dell'agente di raccolta e l'oggetto dell'inventario in cui verrà ospitata la macchina virtuale.
4. In **Host/Cluster** specificare l'host o il cluster in cui verrà eseguita la macchina virtuale dell'agente di raccolta.
5. Nell'area relativa ai dati di archiviazione specificare la destinazione di archiviazione per la macchina virtuale dell'agente di raccolta.
6. In **Disk Format** (Formato disco) specificare il tipo e la dimensione del disco.
7. In **Network Mapping** (Mapping di rete) specificare la rete a cui si connetterà la macchina virtuale dell'agente di raccolta. La rete richiede la connettività Internet per l'invio dei metadati ad Azure.
8. Rivedere e confermare le impostazioni e quindi selezionare **Fine**.

### <a name="identify-the-id-and-key-for-each-project"></a>Identificare la chiave e l'ID di ogni progetto

Se si hanno più progetti, assicurarsi di identificare l'ID e la chiave di ognuno. La chiave è necessaria quando si esegue l'agente di raccolta per individuare le macchine virtuali.

1. Nel progetto selezionare **Attività iniziali** > **Individua e valuta** > **Individua macchine virtuali**.
2. In **Copiare le credenziali del progetto** copiare l'ID e la chiave del progetto.
    ![Copiare le credenziali del progetto](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="set-the-vcenter-statistics-level"></a>Impostare il livello delle statistiche vCenter
Di seguito è riportato l'elenco dei contatori delle prestazioni che vengono raccolti durante l'individuazione. Per impostazione predefinita, i contatori sono disponibili a vari livelli nel server vCenter.

È consigliabile impostare il livello comune più alto (livello 3) per le statistiche in modo che tutti i contatori vengano raccolti correttamente. Se vCenter è impostato su un livello inferiore, è possibile che solo alcuni contatori vengano raccolti completamente, mentre i restanti vengono impostati su 0. La valutazione potrebbe di conseguenza generare dati incompleti.

La tabella seguente indica anche i risultati della valutazione che saranno compromessi se un determinato contatore non viene raccolto.

| Contatore                                 | Level | Livello per dispositivo | Impatto sulla valutazione                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| cpu.usage.average                       | 1     | ND               | Dimensione e costi consigliati della macchina virtuale         |
| mem.usage.average                       | 1     | ND               | Dimensione e costi consigliati della macchina virtuale         |
| virtualDisk.read.average                | 2     | 2                | Dimensione disco, costi di archiviazione e dimensione della macchina virtuale |
| virtualDisk.write.average               | 2     | 2                | Dimensione disco, costi di archiviazione e dimensione della macchina virtuale |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Dimensione disco, costi di archiviazione e dimensione della macchina virtuale |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Dimensione disco, costi di archiviazione e dimensione della macchina virtuale |
| net.received.average                    | 2     | 3                | Dimensione della macchina virtuale e costi della rete             |
| net.transmitted.average                 | 2     | 3                | Dimensione della macchina virtuale e costi della rete             |

> [!WARNING]
> Se è stato appena impostato un livello più alto per le statistiche, la generazione dei contatori delle prestazioni richiederà fino a un giorno. È quindi consigliabile eseguire l'individuazione dopo un giorno.

### <a name="run-the-collector-to-discover-vms"></a>Eseguire l'agente di raccolta per individuare le macchine virtuali

Per ogni individuazione da eseguire, è necessario eseguire l'agente di raccolta per individuare le macchine virtuali nell'ambito richiesto. Eseguire le individuazioni una di seguito all'altra. Le individuazioni simultanee non sono supportate e per ogni individuazione deve essere definito un ambito diverso.

1.  Nella console di vSphere Client fare clic con il pulsante destro del mouse su VM > **Open Console** (Apri console).
2.  Specificare le preferenze relative alla lingua, al fuso orario e alla password per l'appliance.
3.  Sul desktop selezionare il collegamento **Esegui agente di raccolta**.
4.  Nell'agente di raccolta di Azure Migrate aprire **Set Up Prerequisites** (Configura prerequisiti) ed eseguire queste operazioni:

    a. Accettare le condizioni di licenza e leggere le informazioni di terze parti.

    L'agente di raccolta verifica che la macchina virtuale abbia accesso a Internet.

    b. Se la macchina virtuale accede a Internet tramite un proxy, selezionare **Impostazioni proxy** e specificare l'indirizzo e la porta di ascolto del proxy. Se il proxy richiede l'autenticazione, specificare le credenziali.

    L'agente di raccolta verifica che il servizio dell'agente di raccolta sia in esecuzione. Il servizio è installato per impostazione predefinita nella macchina virtuale dell'agente di raccolta.

    c. Scaricare e installare VMware PowerCLI.

5.  In **Specify vCenter Server details** (Specificare i dettagli del Server vCenter) eseguire queste operazioni:
    - Specificare il nome completo (FQDN) o l'indirizzo IP del server vCenter.
    - In **Nome utente** e **Password** specificare le credenziali dell'account di sola lettura che verranno usate dall'agente di raccolta per individuare le macchine virtuali nel server vCenter.
    - In **Ambito raccolta** selezionare un ambito per l'individuazione delle macchine virtuali. L'agente di raccolta può individuare solo le macchine virtuali all'interno dell'ambito specificato. L'ambito può essere impostato su una cartella, un data center o un cluster, ma non deve contenere più di 1.000 macchine virtuali.

6.  In **Il progetto di migrazione specifica** specificare l'ID e la chiave per il progetto. Se questi valori non sono stati copiati, aprire il portale di Azure dalla macchina virtuale dell'agente di raccolta. Nella pagina **Panoramica** del progetto selezionare **Individua macchine virtuali** e copiare i valori.  
7.  In **Visualizzare lo stato di raccolta** monitorare il processo di individuazione e verificare che i metadati raccolti dalle macchine virtuali siano inclusi nell'ambito. L'agente di raccolta indica un tempo di individuazione approssimativo.


#### <a name="verify-vms-in-the-portal"></a>Verificare le macchine virtuali nel portale

Il tempo di individuazione dipende dal numero di macchine virtuali da individuare. L'individuazione di 100 macchine virtuali termina in genere circa un'ora dopo l'esecuzione dell'agente di raccolta.

1. Nel progetto di Azure Migrate selezionare **Gestisci** > **Macchine virtuali**.
2. Verificare che le macchine virtuali da individuare siano visualizzate nel portale.


## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come [creare un gruppo](how-to-create-a-group.md) per la valutazione.
- [Altre informazioni](concepts-assessment-calculation.md) sul modo in cui vengono calcolate le valutazioni.
