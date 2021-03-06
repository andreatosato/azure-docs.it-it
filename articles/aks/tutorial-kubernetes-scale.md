---
title: "Esercitazione su Kubernetes in Azure: ridimensionare un'applicazione"
description: In questa esercitazione sul servizio Kubernetes di Azure (AKS) viene illustrato come ridimensionare nodi e pod in Kubernetes e implementare la scalabilità automatica orizzontale dei pod.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5ffe7b4c7830500e5eeeeb61c57730d9a0d9df47
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2018
ms.locfileid: "41920088"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Esercitazione: Ridimensionare le applicazioni nel servizio Kubernetes di Azure (AKS)

Se sono state eseguite le esercitazioni, si dispone di un cluster Kubernetes funzionante nel servizio contenitore di Azure ed è stata distribuita l'app Azure Voting. In questa esercitazione, la quinta di sette, si aumenterà il numero di istanze dei pod nell'app e si proverà la scalabilità automatica dei pod. Si apprenderà anche come ridimensionare il numero di nodi delle macchine virtuali di Azure per modificare la capacità del cluster per l'hosting dei carichi di lavoro. Si apprenderà come:

> [!div class="checklist"]
> * Ridimensionare i nodi Kubernetes
> * Ridimensionare manualmente i pod Kubernetes che eseguono l'applicazione
> * Configurare la scalabilità automatica dei pod che eseguono il front-end dell'app

Nelle esercitazioni successive l'applicazione Azure Vote viene aggiornata a una nuova versione.

## <a name="before-you-begin"></a>Prima di iniziare

Nelle esercitazioni precedenti è stato creato un pacchetto di un'applicazione in un'immagine del contenitore, caricata poi nel Registro contenitori di Azure, ed è stato creato un cluster Kubernetes. L'applicazione è stata quindi eseguita nel cluster Kubernetes. Se questi passaggi non sono stati ancora eseguiti e si vuole procedere, tornare a [Esercitazione 1: Creare immagini del contenitore][aks-tutorial-prepare-app].

Per questa esercitazione è necessario eseguire l'interfaccia della riga di comando di Azure versione 2.0.38 o successiva. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure][azure-cli-install].

## <a name="manually-scale-pods"></a>Scalare manualmente i pod

Quando il front-end di Azure Vote e l'istanza di Redis sono stati distribuiti nelle esercitazioni precedenti, è stata creata una singola replica. Per visualizzare il numero e lo stato dei pod nel cluster, usare il comando [kubectl get][kubectl-get] come di seguito:

```console
kubectl get pods
```

L'output di esempio seguente illustra un pod front-end e un pod back-end:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Per modificare manualmente il numero di pod nella distribuzione *azure-vote-front*, usare il comando [kubectl scale][kubectl-scale]. L'esempio seguente aumenta il numero di pod front-end a *5*:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Eseguire di nuovo [kubectl get pods][kubectl-get] per verificare che Kubernetes crei i pod aggiuntivi. Dopo circa un minuto i pod aggiuntivi sono disponibili nel cluster:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Scalare automaticamente i pod

Kubernetes supporta la [scalabilità automatica orizzontale dei pod][kubernetes-hpa] per modificare il numero dei pod in una distribuzione a seconda dell'utilizzo della CPU o delle altre metriche selezionate. Il [server delle metriche][metrics-server] viene usato per fornire l'utilizzo delle risorse a Kubernetes. Per installare il server delle metriche, clonare il repository GitHub `metrics-server` e installare le definizioni di risorsa di esempio. Per visualizzare il contenuto di queste definizioni YAML, vedere [Metrics Server for Kuberenetes 1.8+][metrics-server-github] (Server delle metriche per Kuberenetes 1.8 +).

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Per usare la scalabilità automatica, i pod devono avere richieste e limiti di CPU definiti. Nella distribuzione di `azure-vote-front` il contenitore front-end richiede un valore di 0,25 della CPU, con un limite pari a 0,5 della CPU. Le impostazioni sono simili:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

L'esempio seguente usa il comando [kubectl autoscale][kubectl-autoscale] per ridimensionare automaticamente il numero di pod nella distribuzione *azure-vote-front*. Se l'utilizzo della CPU supera il 50%, la scalabilità automatica aumenta il numero di pod fino a un massimo di 10 istanze:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Per visualizzare lo stato della scalabilità automatica, usare il comando `kubectl get hpa` come di seguito:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Dopo pochi minuti con un carico minimo sull'app Azure Vote, il numero di repliche di pod si riduce automaticamente a tre. È possibile usare nuovamente `kubectl get pods` per visualizzare la rimozione dei pod non necessari.

## <a name="manually-scale-aks-nodes"></a>Ridimensionare manualmente i nodi AKS

Se è stato creato usando i comandi nell'esercitazione precedente, il cluster Kubernetes include un nodo. Se si prevede un numero maggiore o minore di carichi di lavoro dei contenitori nel cluster, è possibile modificare manualmente il numero di nodi.

Nell'esempio seguente il numero di nodi viene aumentato a tre nel cluster Kubernetes denominato *myAKSCluster*. Il completamento del comando richiede alcuni minuti.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

L'output è simile a:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state usate diverse funzionalità di scalabilità nel cluster Kubernetes. Si è appreso come:

> [!div class="checklist"]
> * Ridimensionare i nodi Kubernetes
> * Ridimensionare manualmente i pod Kubernetes che eseguono l'applicazione
> * Configurare la scalabilità automatica dei pod che eseguono il front-end dell'app

Passare all'esercitazione successiva per apprendere come aggiornare l'applicazione in Kubernetes.

> [!div class="nextstepaction"]
> [Aggiornare un'applicazione in Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
