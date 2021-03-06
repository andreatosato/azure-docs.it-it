---
title: Guida introduttiva - Cluster Kubernetes Azure per Linux
description: Informazioni per creare rapidamente un cluster Kubernetes per contenitori Linux nel servizio contenitore di Azure con l'interfaccia della riga di comando di Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: quickstart
ms.date: 07/31/2018
ms.author: iainfou
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: f52551e9d57ccfc44502992b59412878c4092c0d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436904"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster"></a>Guida introduttiva: distribuire un cluster di Azure Kubernetes Service (AKS)

In questa guida introduttiva viene distribuito un cluster del servizio contenitore di Azure usando l'interfaccia della riga di comando di Azure. Un'applicazione multi-contenitore costituita dal front-end Web e da un'istanza di Redis viene quindi eseguita nel cluster. Al termine, l'applicazione è accessibile tramite Internet.

![Immagine del passaggio ad Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Questa guida introduttiva presuppone una conoscenza di base dei concetti relativi a Kubernetes. Per informazioni dettagliate in proposito, vedere la [documentazione di Kubernetes][kubernetes-documentation].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, questa guida introduttiva richiede la versione 2.0.43 o successiva dell'interfaccia della riga di comando di Azure. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure][azure-cli-install].

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Creare un gruppo di risorse con il comando [az group create][az-group-create]. Un gruppo di risorse di Azure è un gruppo logico in cui le risorse di Azure vengono distribuite e gestite. Quando si crea un gruppo di risorse, viene chiesto di specificare una posizione. Questa posizione è quella in cui le risorse vengono eseguite in Azure.

L'esempio seguente crea un gruppo di risorse denominato *myAKSCluster* nella località *eastus*.

```azurecli-interactive
az group create --name myAKSCluster --location eastus
```

Output:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myAKSCluster",
  "location": "eastus",
  "managedBy": null,
  "name": "myAKSCluster",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>Creare un cluster del servizio contenitore di Azure

Usare il comando [az aks create][az-aks-create] per creare un cluster del servizio contenitore di Azure. L'esempio seguente crea un cluster denominato *myAKSCluster* con un nodo. Il monitoraggio dell'integrità dei contenitori viene abilitato con il parametro *--enable-addons monitoring*. Per altre informazioni sul monitoraggio dell'integrità del contenitore, vedere [Monitorare l'integrità del servizio Kubernetes di Azure][aks-monitor].

```azurecli-interactive
az aks create --resource-group myAKSCluster --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

Dopo alcuni minuti, il comando viene completato e restituisce le informazioni in formato JSON sul cluster.

## <a name="connect-to-the-cluster"></a>Connettersi al cluster

Per gestire un cluster Kubernetes, usare [kubectl][kubectl], il client da riga di comando di Kubernetes.

Se si usa Azure Cloud Shell, `kubectl` è già installato. Se lo si vuole installare in locale, usare il comando [az aks install-cli][az-aks-install-cli].


```azurecli
az aks install-cli
```

Per configurare `kubectl` per la connessione al cluster Kubernetes, usare il comando [az aks get-credentials][az-aks-get-credentials]. Con questo passaggio si scaricano le credenziali e si configura l'interfaccia della riga di comando di Kubernetes per il loro uso.

```azurecli-interactive
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Per verificare la connessione al cluster, usare il comando [kubectl get][kubectl-get] per restituire un elenco dei nodi del cluster. La visualizzazione dei nodi può richiedere alcuni minuti.

```azurecli-interactive
kubectl get nodes
```

Output:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>Eseguire l'applicazione

Un file manifesto di Kubernetes definisce uno stato desiderato per il cluster, incluse le immagini del contenitore da eseguire. Per questo esempio, viene usato un manifesto per creare tutti gli oggetti necessari per eseguire l'applicazione Azure Vote. Questo manifesto include due [distribuzioni Kubernetes][kubernetes-deployment], una per le applicazioni Azure Vote Python e l'altra per un'istanza Redis. Vengono anche creati due [servizi di Kubernetes][kubernetes-service], un servizio interno per l'istanza Redis e un servizio esterno per l'accesso all'applicazione Azure Vote da Internet.

Creare un file denominato `azure-vote.yaml` e copiarvi il codice YAML seguente. Se si usa Azure Cloud Shell, questo file può essere creato usando vi o Nano come se si usasse un sistema virtuale o fisico.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Usare il comando [kubectl apply][kubectl-apply] per eseguire l'applicazione.

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Output:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Test dell'applicazione

Mentre l'applicazione viene eseguita, viene creato un [servizio di Kubernetes][kubernetes-service] che espone il front-end dell'applicazione a Internet. Il processo potrebbe richiedere alcuni minuti.

Per monitorare lo stato, usare il comando [kubectl get service][kubectl-get] con l'argomento `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

*EXTERNAL-IP* per il servizio *azure-vote-front* inizialmente viene visualizzato come *pending*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Dopo il passaggio di *EXTERNAL-IP* da *pending* a un *indirizzo IP*, usare `CTRL-C` per arrestare il processo kubectl watch.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Passare ora all'indirizzo IP esterno per visualizzare l'app Azure Vote.

![Immagine del passaggio ad Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Monitoraggio di stato e registri

Quando è stato creato il cluster del servizio Kubernetes di Azure, è stato abilitato il monitoraggio per acquisire le metriche di integrità sia per i nodi del cluster che per i pod. Queste metriche di integrità sono disponibili nel portale di Azure. Per altre informazioni sul monitoraggio dell'integrità del contenitore, vedere [Monitor Azure Kubernetes Service health][aks-monitor] (Monitorare l'integrità di Azure Kubernetes Service).

Per visualizzare lo stato corrente, i tempi di attività e l'utilizzo delle risorse per i pod di Azure Vote, seguire questa procedura:

1. In un Web browser passare al portale di Azure [https://portal.azure.com][azure-portal].
1. Selezionare il gruppo di risorse, ad esempio *myResourceGroup*, quindi selezionare il cluster del servizio Kubernetes di Azure, ad esempio *myAKSCluster*. 
1. Scegliere **Monitorare l'integrità dei contenitori**, selezionare lo spazio dei nomi **predefinito**, quindi selezionare **Contenitori**.

L'inserimento di questi dati nel portale di Azure potrebbe richiedere alcuni minuti. Vedere l'esempio seguente:

![Creare un cluster del servizio contenitore di Azure - 1](media/kubernetes-walkthrough/view-container-health.png)

Per visualizzare i log per il pod `azure-vote-front`, selezionare il collegamento **Visualizza log** sul lato destro dell'elenco dei contenitori. Questi log includono i flussi *stdout* e *stderr* del contenitore.

![Creare un cluster del servizio contenitore di Azure - 1](media/kubernetes-walkthrough/view-container-logs.png)

## <a name="delete-cluster"></a>Eliminare il cluster

Quando il cluster non è più necessario, usare il comando [az group delete][az-group-delete] per rimuovere il gruppo di risorse, il servizio contenitore e tutte le risorse correlate.

```azurecli-interactive
az group delete --name myAKSCluster --yes --no-wait
```

> [!NOTE]
> Quando si elimina il cluster, l'entità servizio di Azure Active Directory utilizzata dal cluster AKS non viene rimossa. Per istruzioni su come rimuovere l'entità servizio, vedere le [considerazioni sull'entità servizio AKS e la sua eliminazione][sp-delete].

## <a name="get-the-code"></a>Ottenere il codice

In questa guida introduttiva sono state usate immagini del contenitore già creato per creare una distribuzione di Kubernetes. Il codice dell'applicazione correlato, Dockerfile, e il file manifesto di Kubernetes sono disponibili su GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stato distribuito un cluster Kubernetes in cui è stata quindi distribuita un'applicazione multicontenitore.

Per altre informazioni sul servizio contenitore di Azure e l'analisi del codice completo per un esempio di distribuzione, passare all'esercitazione sul cluster Kubernetes.

> [!div class="nextstepaction"]
> [Esercitazione sul servizio contenitore di Azure][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
