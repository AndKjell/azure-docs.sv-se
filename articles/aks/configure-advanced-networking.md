---
title: Konfigurerade avancerade nätverk i Azure Kubernetes Service (AKS)
description: Lär dig hur du konfigurerar avancerade nätverk i Azure Kubernetes Service (AKS), inklusive distribution av ett AKS-kluster till ett befintligt virtuellt nätverk och undernät.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 87c3ab9624116e9c1c61041531fdf5d3b26117e1
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381227"
---
# <a name="configure-advanced-networking-in-azure-kubernetes-service-aks"></a>Konfigurera avancerade nätverk i Azure Kubernetes Service (AKS)

Som standard AKS-kluster används *grundläggande* nätverk som skapar och konfigurerar ett virtuellt nätverk och undernät för användning med klustret. För ytterligare kontroll över dessa alternativ för nätverksfunktioner, till exempel IP-adressintervall kan du istället använda *avancerade* nätverk. Med avancerade nätverk kan du också skapa ett AKS-kluster i ett befintligt virtuellt nätverk och undernät. Det här befintliga virtuella nätverket tillhandahåller ofta anslutning till ett lokalt nätverk med hjälp av Azure ExpressRoute eller VPN för plats-till-plats.

Den här artikeln visar hur du använder avancerade nätverk för att skapa och använda ett virtuellt nätverk med ett AKS-kluster. Mer allmän information om nätverk finns i [nätverk begrepp för Kubernetes och AKS][aks-network-concepts].

## <a name="prerequisites"></a>Förutsättningar

* Det virtuella nätverket för AKS-klustret måste tillåta utgående internet-anslutning.
* Skapa inte fler än ett AKS-kluster i samma undernät.
* AKS-kluster får inte använda `169.254.0.0/16`, `172.30.0.0/16`, eller `172.31.0.0/16` för Kubernetes service-adressintervall.
* Tjänstens huvudnamn som används av AKS-klustret måste ha minst [Nätverksdeltagare](../role-based-access-control/built-in-roles.md#network-contributor) behörigheter på undernätet i det virtuella nätverket. Om du vill definiera en [anpassad roll](../role-based-access-control/custom-roles.md) istället för att använda den inbyggda rollen som Nätverksdeltagare, krävs följande behörigheter:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planera IP-adresser för ditt kluster

Kluster som har konfigurerats med avancerade nätverk kräver ytterligare planering. Storleken på det virtuella nätverket och dess undernät måste hantera såväl antalet poddar som du planerar att köra som antalet noder för klustret.

IP-adresser för poddarna och klustrets noder tilldelas från det angivna undernätet i det virtuella nätverket. Varje nod har konfigurerats med en primär IP-adress, vilket är den IP-Adressen på noden och 30 ytterligare IP-adresser redan har konfigurerats av Azure CNI som har tilldelats poddar som schemalagts att noden. När du skalar ut ditt kluster konfigureras varje nod på samma sätt med IP-adresser från undernätet.

Plan för IP-adress för ett AKS-kluster består av en virtuell nätverks-, minst ett undernät för noder och poddar och Kubernetes service-adressintervall.

| Adressintervall / Azure resurs | Gränser och storlek |
| --------- | ------------- |
| Virtuellt nätverk | Azure-nätverket kan vara så stora/8 som, men är begränsad till 65 536 konfigurerade IP-adresser. |
| Undernät | Måste vara tillräckligt stor för att hantera noder, poddar och alla Kubernetes och Azure-resurser som kan etableras i klustret. Om du distribuerar en intern Azure Load Balancer, exempelvis dess frontend IP-adresser tilldelas från klustret undernätet inte offentliga IP-adresser. <p/>Att beräkna *minsta* undernätets storlek: `(number of nodes) + (number of nodes * pods per node)` <p/>Exempel för ett kluster med 50 noder: `(50) + (50 * 30) = 1,550` (/ 21 eller större) |
| Kubernetes service-adressintervall | Det här intervallet bör inte används av alla nätverkselement på eller ansluten till det här virtuella nätverket. CIDR-tjänstadress måste vara mindre än /12. |
| IP-adress för Kubernetes DNS-tjänsten | IP-adress inom Kubernetes service-adressintervall som ska användas av klustertjänstidentifiering (kube-dns). |
| Docker bridge-adress | IP-adress (i CIDR-notation) som används som Docker-brygga IP-adress på noder. Standardvärdet 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Maximal poddar per nod

Standard maximala antalet poddar per nod i ett AKS-kluster varierar mellan grundläggande och avancerade nätverks- och metoden för klusterdistribution.

| Distributionsmetod | Basic | Advanced | Kan konfigureras vid distribution |
| -- | :--: | :--: | -- |
| Azure CLI | 110 | 30 | Ja (upp till 110) |
| Resource Manager-mall | 110 | 30 | Ja (upp till 110) |
| Portalen | 110 | 30 | Nej |

### <a name="configure-maximum---new-clusters"></a>Konfigurera maximal – nya kluster

Det går att konfigurera det maximala antalet poddar per nod *endast vid tidpunkten för distribution av kluster*. Om du distribuerar med Azure CLI eller med en Resource Manager-mall kan ange du maximal poddarna per nodvärde så mycket som 110.

* **Azure CLI**: Ange den `--max-pods` argumentet när du distribuerar ett kluster med den [az aks skapa] [ az-aks-create] kommando. Det högsta värdet är 110.
* **Resource Manager-mall**: Ange den `maxPods` -egenskapen i den [ManagedClusterAgentPoolProfile] objekt när du distribuerar ett kluster med en Resource Manager-mall. Det högsta värdet är 110.
* **Azure-portalen**: du kan inte ändra det maximala antalet poddar per nod när du distribuerar ett kluster med Azure-portalen. Avancerade nätverk kluster är begränsade till 30 poddar per nod när du distribuerar med hjälp av Azure portal.

### <a name="configure-maximum---existing-clusters"></a>Konfigurera maximal – befintliga kluster

Du kan inte ändra de maximala poddarna per nod i ett befintligt AKS-kluster. Du kan justera antalet endast när du först distribuera klustret.

## <a name="deployment-parameters"></a>Distributionsparametrarna

När du skapar ett AKS-kluster, konfigureras följande parametrar för avancerade nätverksfunktioner:

**Virtuellt nätverk**: det virtuella nätverket dit du vill distribuera Kubernetes-klustret. Om du vill skapa ett nytt virtuellt nätverk för klustret, väljer *Skapa nytt* och följ stegen i den *skapa virtuellt nätverk* avsnittet. Information om begränsningar och kvoter för ett Azure-nätverk finns i [Azure-prenumeration och tjänstbegränsningar, kvoter och begränsningar](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Undernät**: undernätet i det virtuella nätverket där du vill distribuera klustret. Om du vill skapa ett nytt undernät i det virtuella nätverket för klustret, väljer *Skapa nytt* och följ stegen i den *skapa undernät* avsnittet.

**Kubernetes-tjänst-adressintervall**: det här är en uppsättning virtuella IP-adresser som Kubernetes tilldelar [services] [ services] i klustret. Du kan använda alla privata adressintervall som uppfyller följande krav:

* Får inte vara i virtuella nätverkets IP-adressintervall för ditt kluster
* Får inte överlappa med andra virtuella nätverk som virtuellt klusternätverk peer-datorer
* Får inte överlappa eventuella lokala IP-adresser
* Får inte vara inom intervallen `169.254.0.0/16`, `172.30.0.0/16`, eller `172.31.0.0/16`

Även om det är tekniskt möjligt att ange ett service-adressintervall inom samma virtuella nätverk som klustret rekommenderas detta så inte. Oväntade funktionssätt kan bero på att överlappande IP-adressintervall som används. Mer information finns i den [vanliga frågor och svar](#frequently-asked-questions) i den här artikeln. Mer information om Kubernetes-tjänster finns i [Services] [ services] i Kubernetes-dokumentationen.

**IP-adress för Kubernetes DNS-tjänsten**: IP-adress för klustrets DNS-tjänsten. Den här adressen måste vara inom den *Kubernetes-tjänst-adressintervall*.

**Docker Bridge-adress**: IP-adress och nätmask ska tilldelas Docker bridge. Den här IP-adressen får inte vara inom det virtuella nätverket IP-adressintervallet på klustret.

## <a name="configure-networking---cli"></a>Konfigurera nätverk – CLI

När du skapar ett AKS-kluster med Azure CLI kan konfigurera du också avancerade nätverk. Använd följande kommandon för att skapa ett nytt AKS-kluster med avancerade nätverksfunktioner som är aktiverad.

Hämta först undernät resurs-ID för det befintliga undernätet där AKS-klustret ska anslutas:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Använd den [az aks skapa] [ az-aks-create] med den `--network-plugin azure` argument för att skapa ett kluster med avancerade nätverk. Uppdatera den `--vnet-subnet-id` värdet med undernät-ID som samlas in i föregående steg:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Konfigurera nätverk – portal

Följande skärmbild från Azure-portalen visar ett exempel på hur du konfigurerar dessa inställningar när du skapar för AKS-klustret:

![Avancerad konfiguration av nätverk i Azure portal][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

Följande frågor och svar gäller den **Avancerat** nätverkskonfiguration.

* *Kan jag distribuera virtuella datorer i klusterundernätet mitt?*

  Nej. Distribuera virtuella datorer i undernätet som används av ditt Kubernetes-kluster stöds inte. Virtuella datorer kan distribueras i samma virtuella nätverk, men i ett annat undernät.

* *Kan jag konfigurera per pod nätverksprinciper?*

  Nej. Per pod nätverksprinciper är för närvarande stöds inte.

* *Är det maximala antalet poddar distribueras till en nod kan konfigureras?*

  Ja, när du distribuerar ett kluster med Azure CLI eller en Resource Manager-mall. Se [maximalt poddar per nod](#maximum-pods-per-node).

  Du kan inte ändra det maximala antalet poddar per nod i ett befintligt kluster.

* *Hur konfigurerar jag ytterligare egenskaper för det undernät som jag skapade när du skapar för AKS-klustret? Till exempel Tjänsteslutpunkter.*

  Den fullständiga listan över egenskaper för virtuellt nätverk och undernät som du skapar när du skapar för AKS-klustret kan konfigureras på sidan virtuellt standardnätverk konfiguration i Azure-portalen.

* *Kan jag använda ett annat undernät i mitt kluster virtuella nätverk för den* **Kubernetes-tjänst-adressintervall**?

  Det rekommenderas inte, men den här konfigurationen är möjligt. Service-adressintervall är en uppsättning virtuella IP-adresser (VIP) som Kubernetes tilldelar tjänster i ditt kluster. Azure-nätverk har inga insyn i Kubernetes-klustret service IP-adressintervall. På grund av bristen på insyn i klustrets service-adressintervall är det möjligt att skapa ett nytt undernät senare i det virtuella nätverk i klustret som överlappar adressintervallet service. Om en sådan överlappning inträffar kan Kubernetes tilldela en tjänst en IP-adress som används redan av en annan resurs i undernätet orsaka oväntade funktionssätt eller fel. Genom att se till att du använder ett adressintervall utanför klustrets virtuella nätverk, kan du undvika detta överlappar varandra.

## <a name="next-steps"></a>Nästa steg

### <a name="networking-in-aks"></a>Nätverksfunktionerna i AKS

Mer information om nätverk i AKS i följande artiklar:

- [Använda en statisk IP-adress med belastningsutjämnare för Azure Kubernetes Service (AKS)](static-ip.md)
- [Använda en intern belastningsutjämnare med Azure Container Service (AKS)](internal-lb.md)

- [Skapa en grundläggande ingress-kontrollant med extern nätverksanslutning][aks-ingress-basic]
- [Aktivera HTTP-programmet routning tillägg][aks-http-app-routing]
- [Skapa en ingress-kontrollant som använder ett privat internt nätverk och IP-adress][aks-ingress-internal]
- [Skapa en ingress-kontrollant med en dynamisk offentlig IP-adress och konfigurera kryptera vi för att automatiskt generera TLS-certifikat][aks-ingress-tls]
- [Skapa en ingress-kontrollant med en statisk offentlig IP-adress och konfigurera kryptera vi för att automatiskt generera TLS-certifikat][aks-ingress-static-tls]

### <a name="acs-engine"></a>ACS-motor

[Azure Container Service Engine (ACS-motor)] [ acs-engine] är ett projekt med öppen källkod som genererar Azure Resource Manager-mallar som du kan använda för att distribuera Docker-aktiverade kluster i Azure. Kubernetes, DC/OS, Swarm-läge och Swarm-initierare kan distribueras med ACS-motor.

Kubernetes-kluster som skapas med ACS-motorn ha stöd för både den [kubenet] [ kubenet] och [Azure CNI] [ cni-networking] plugin-program. Därför stöds både grundläggande och avancerade nätverksscenarier av ACS-motor.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
