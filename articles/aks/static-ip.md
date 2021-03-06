---
title: Använda en statisk IP-adress med belastningsutjämnare för Azure Kubernetes Service (AKS)
description: Lär dig hur du skapar och använder en statisk IP-adress med belastningsutjämnare för Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 8aab091ed992a946cd78ecf4f0c8fdfff4185a08
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407560"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Använda en statisk offentlig IP-adress med belastningsutjämnare för Azure Kubernetes Service (AKS)

Som standard gäller endast offentliga IP-adress som tilldelats en belastningsutjämningsresurs som skapats av ett AKS-kluster för livslängden för den här resursen. Om du tar bort Kubernetes-tjänst, raderas också associerade belastningsutjämnare och IP-adress. Om du vill tilldela en specifik IP-adress eller behålla en IP-adress för omdistribuerade Kubernetes-tjänster kan du skapa och använda en statisk offentlig IP-adress.

Den här artikeln visar hur du skapar en statisk offentlig IP-adress och tilldela den till ditt Kubernetes-tjänst.

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln förutsätter att du har ett befintligt AKS-kluster. Om du behöver ett AKS-kluster finns i snabbstarten om AKS [med Azure CLI] [ aks-quickstart-cli] eller [med Azure portal][aks-quickstart-portal].

Du också ha Azure CLI version 2.0.46 eller senare installerat och konfigurerat. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI][install-azure-cli].

## <a name="create-a-static-ip-address"></a>Skapa en statisk IP-adress

När du skapar en statisk offentlig IP-adress för användning med AKS, IP-adressresurs måste skapas i den **noden** resursgrupp. Hämta resursgruppens namn med den [az aks show] [ az-aks-show] kommandot och lägga till den `--query nodeResourceGroup` frågeparameter. I följande exempel hämtar noden resursgruppen för AKS-klusternamnet *myAKSCluster* i resursgruppens namn *myResourceGroup*:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Nu skapa en statisk offentlig IP-adress med det [az nätverket offentliga ip-skapa] [ az-network-public-ip-create] kommando. Ange noden resursgruppens namn som hämtades i föregående kommando och sedan ett namn för IP-Adressen kan du hantera resurs, som *myAKSPublicIP*:

```azurecli
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

IP-adressen visas enligt följande komprimerade exempel på utdata:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
````

Du kan senare får den offentliga IP-adress med hjälp av den [az network public-ip-listan] [ az-network-public-ip-list] kommando. Ange namnet på resursgruppen noden och sedan fråga efter den *ipAddress* som visas i följande exempel:

```azurecli
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Skapa en tjänst med statisk IP-adress

När du skapar en tjänst med den statiska offentliga IP-adressen till den `loadBalancerIP` egenskapen och värdet för den statiska offentliga IP-adress till YAML-manifestet. Skapa en fil med namnet `load-balancer-service.yaml` och kopiera följande YAML. Ange dina egna offentliga IP-adressen som skapades i föregående steg.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Skapa tjänsten och distribution med den `kubectl apply` kommando.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="troubleshoot"></a>Felsöka

Om den statiska IP-adressen som definierats i den *loadBalancerIP* egenskapen för Kubernetes tjänstmanifestet finns inte eller har inte skapats i resursgruppen nod, skapat en belastningsutjämnare tjänsten misslyckas. Felsök genom att granska service skapande händelser med den [kubectl beskriver] [ kubectl-describe] kommando. Ange namnet på tjänsten som anges i YAML-manifest som visas i följande exempel:

```console
kubectl describe service azure-load-balancer
```

Information om Kubernetes service-resurs visas. Den *händelser* i slutet av följande Exempelutdata tyda på att den *användaren inte gick att hitta den angivna IP-adressen*. Kontrollera att du har skapat statiska offentliga IP-adress i resursgruppen noden och att den IP-adressen som anges i tjänstmanifestet Kubernetes är korrekt i dessa scenarier.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Nästa steg

För ytterligare kontroll över nätverkstrafik till dina program kan du i stället [skapa en ingress-kontrollant][aks-ingress-basic]. Du kan också [skapa en ingress-kontrollant med en statisk offentlig IP-adress][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
