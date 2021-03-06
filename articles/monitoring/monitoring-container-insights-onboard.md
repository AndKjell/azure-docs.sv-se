---
title: Hur du integrera Azure Monitor för behållare | Microsoft Docs
description: Den här artikeln beskrivs hur du publicerar och konfigurera Azure Monitor för behållare så att du kan förstå hur din behållare fungerar och vilka prestanda-relaterade problem har identifierats.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/04/2018
ms.author: magoedte
ms.openlocfilehash: 9fa0df0bbf363a7c751de460fd98740b4314f996
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831202"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Hur du integrera Azure Monitor för behållare
Den här artikeln beskriver hur du ställer in Azure Monitor för behållare för att övervaka prestanda för arbetsbelastningar som distribueras till Kubernetes-miljöer och finns på [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

## <a name="prerequisites"></a>Förutsättningar 
Innan du börjar bör du kontrollera att du har följande:

- En ny eller befintlig AKS-kluster.
- En behållare Log Analytics-agenten för Linux-versionen microsoft / oms:ciprod04202018 eller senare. Versionsnumret representeras av ett datum i följande format: *mmddyyyy*. Agenten installeras automatiskt under publiceringen av den här funktionen. 
- En Log Analytics-arbetsyta. Du kan skapa den när du aktiverar övervakning av din nya AKS-klustret eller låta publiceringsupplevelsen skapa en standardarbetsyta i standardresursgruppen för prenumerationen för AKS-kluster. Om du väljer att skapa den själv, kan du skapa den via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), via [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), eller i den [Azure-portalen](../log-analytics/log-analytics-quick-create-workspace.md).
- Log Analytics deltagarrollen, att aktivera behållarövervakning. Läs mer om hur du styr åtkomst till en Log Analytics-arbetsyta, [hantera arbetsytor](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Komponenter 

Du kan övervaka prestanda är beroende av en behållare Log Analytics-agenten för Linux som samlar in prestanda- och händelsedata från alla noder i klustret. Agenten har automatiskt distribuerat och registrerat med den angivna Log Analytics-arbetsytan när du aktiverar behållarövervakning. 

>[!NOTE] 
>Om du redan har distribuerat ett AKS-kluster kan aktivera du övervakning genom att använda Azure CLI eller en angiven Azure Resource Manager-mall, som visas längre fram i den här artikeln. Du kan inte använda `kubectl` om du vill uppgradera, ta bort, omdistribuera eller distribuera agenten. Mallen måste distribueras i samma resursgrupp som klustret ”.

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure Portal
Logga in på [Azure Portal](https://portal.azure.com). 

## <a name="enable-monitoring-for-a-new-cluster"></a>Aktivera övervakning av ett nytt kluster
Under distributionen kan aktivera du övervakning av ett nytt AKS-kluster i Azure portal eller med Azure CLI. Följ stegen i snabbstartsartikeln [distribuera ett kluster i Azure Kubernetes Service (AKS)](../aks/kubernetes-walkthrough-portal.md) om du vill aktivera från portalen. På den **övervakning** sidan för den **aktivera övervakning** väljer **Ja**, och välj en befintlig Log Analytics-arbetsyta eller skapa en ny. 

Om du vill aktivera övervakning av ett nytt AKS-kluster som skapats med Azure CLI följer du steg i snabbstartsartikeln under avsnittet [skapa AKS-kluster](../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Om du väljer att använda Azure CLI, måste du först installera och använda CLI lokalt. Du måste köra Azure CLI version 2.0.43 eller senare. För att identifiera din version, kör `az --version`. Om du behöver installera eller uppgradera Azure CLI kan du läsa [installera Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

När du har aktiverat övervakning och alla åtgärder för konfiguration har slutförts kan övervaka du prestanda för ditt kluster på något av två sätt:

* Direkt i AKS-kluster genom att välja **hälsotillstånd** i den vänstra rutan.
* Genom att välja den **övervakaren behållareinsikter** panel på sidan AKS-kluster för det markerade klustret. I Azure Monitor, i den vänstra rutan väljer **hälsotillstånd**. 

  ![Alternativ för att välja Azure Monitor för behållare i AKS](./media/monitoring-container-insights-onboard/kubernetes-select-monitoring-01.png)

När du har aktiverat övervakning, kan det ta ungefär 15 minuter innan du kan visa hälsomått för klustret. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Aktivera övervakning för befintliga hanterade kluster
Du kan aktivera övervakning av ett AKS-kluster som redan har distribuerats antingen med hjälp av Azure CLI, från portalen eller med den angivna Azure Resource Manager-mallen med hjälp av PowerShell-cmdleten `New-AzureRmResourceGroupDeployment`. 

### <a name="enable-monitoring-using-azure-cli"></a>Aktivera övervakning med hjälp av Azure CLI
Följande steg aktiverar övervakning av AKS-klustret med hjälp av Azure CLI. I det här exemplet måste du behöver inte per skapa eller ange en befintlig arbetsyta. Det här kommandot gör enklare för dig genom att skapa en standardarbetsyta i standardresursgruppen för AKS-kluster-prenumeration om det inte redan finns i regionen.  Standardarbetsytan skapade liknar formatet *DefaultWorkspace -<GUID>-<Region>*.  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

Utdata ska likna följande:

```azurecli
provisioningState       : Succeeded
```

Om du skulle hellre integrerar med en befintlig arbetsyta, kan du använda följande kommando för att ange den arbetsytan.

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG --workspace-resource-id <ExistingWorkspaceResourceID> 
```

Utdata ska likna följande:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-from-azure-monitor"></a>Aktivera övervakning från Azure Monitor
Om du vill aktivera övervakning av AKS-kluster i Azure-portalen från Azure Monitor, gör du följande:

1. I Azure-portalen väljer du **övervakaren**. 
2. Välj **behållare (förhandsgranskning)** i listan.
3. På den **skärm – behållare (förhandsgranskning)** väljer **icke-övervakas kluster**.
4. Från listan över ej övervakade kluster, hitta behållaren i listan och klicka på **aktivera**.   
5. På den **registrering för hälsotillstånd för behållare och loggar** om du har en befintlig Log Analytics-arbetsyta i samma prenumeration som klustret, markerar du den i den nedrullningsbara listan.  
    Listan förväljer standardarbetsytan och plats som AKS-behållare distribueras till i prenumerationen. 

    ![Aktivera AKS behållareinsikter övervakning](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Om du vill skapa en ny Log Analytics-arbetsyta för att lagra övervakningsdata från klustret, följer du anvisningarna i [skapa en Log Analytics-arbetsyta](../log-analytics/log-analytics-quick-create-workspace.md). Var noga med att skapa arbetsytan i samma prenumeration som AKS-behållare distribueras till. 
 
När du har aktiverat övervakning, kan det ta ungefär 15 minuter innan du kan visa hälsomått för klustret. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>Aktivera övervakning från AKS-kluster i portalen
Om du vill aktivera övervakning av din AKS-behållare i Azure-portalen, gör du följande:

1. I Azure-portalen väljer du **alla tjänster**. 
2. I listan över resurser, börjar du skriva **behållare**.  
    Filtreras listan baserat på dina indata. 
3. Välj **Kubernetes-tjänster**.  

    ![Länken Kubernetes-tjänster](./media/monitoring-container-insights-onboard/portal-search-containers-01.png)

4. I listan över behållare, väljer du en behållare.
5. På översiktssidan behållaren väljer **övervaka behållarens hälsa**.  
6. På den **registrering för hälsotillstånd för behållare och loggar** om du har en befintlig Log Analytics-arbetsyta i samma prenumeration som klustret, markerar du den i den nedrullningsbara listan.  
    Listan förväljer standardarbetsytan och plats som AKS-behållare distribueras till i prenumerationen. 

    ![Aktivera hälsoövervakning för AKS-behållare](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Om du vill skapa en ny Log Analytics-arbetsyta för att lagra övervakningsdata från klustret, följer du anvisningarna i [skapa en Log Analytics-arbetsyta](../log-analytics/log-analytics-quick-create-workspace.md). Var noga med att skapa arbetsytan i samma prenumeration som AKS-behållare distribueras till. 
 
När du har aktiverat övervakning, kan det ta ungefär 15 minuter innan du kan visa användningsdata för klustret. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Aktivera övervakning genom att använda en Azure Resource Manager-mall
Den här metoden innehåller två JSON-mallar. En mall anger konfigurationen för att aktivera övervakning och den andra innehåller parametervärden som du konfigurerar för att ange följande:

* AKS-behållare resurs-ID. 
* Den resursgrupp som klustret har distribuerats i.
* Log Analytics-arbetsyta och region för att skapa arbetsytan i. 

>[!NOTE]
>Mallen måste distribueras i samma resursgrupp som klustret.
>

Log Analytics-arbetsytan måste du skapa dem manuellt. För att skapa arbetsytan, du kan konfigurera det via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), via [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), eller i den [Azure-portalen](../log-analytics/log-analytics-quick-create-workspace.md).

Om du inte är bekant med begreppet att distribuera resurser med hjälp av en mall, se:
* [Distribuera resurser med Resource Manager-mallar och Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Distribuera resurser med Resource Manager-mallar och Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)

Om du väljer att använda Azure CLI, måste du först installera och använda CLI lokalt. Du måste köra Azure CLI version 2.0.27 eller senare. För att identifiera din version, kör `az --version`. Om du behöver installera eller uppgradera Azure CLI kan du läsa [installera Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Skapa och köra en mall

1. Kopiera och klistra in följande JSON-syntax i filen:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. Spara filen som **existingClusterOnboarding.json** till en lokal mapp.
3. Klistra in följande JSON-syntax i filen:

    ```json
    {
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. Redigera värdena för **aksResourceId** och **aksResourceLocation** med hjälp av värdena på den **översikt över AKS** för AKS-klustret. Värdet för **workspaceResourceId** är fullständiga resurs-ID för Log Analytics-arbetsytan, som innehåller namnet på arbetsytan. Också ange platsen för arbetsytan för **workspaceRegion**. 
5. Spara filen som **existingClusterParam.json** till en lokal mapp.
6. Nu är det dags att distribuera den här mallen. 

    * Använd följande PowerShell-kommandon i den mapp som innehåller mallen:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ClusterResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Konfigurationsändringen kan ta några minuter att slutföra. När det är klart visas ett meddelande som liknar följande och som innehåller resultatet:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Så här kör du följande kommando med hjälp av Azure-CLI:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Konfigurationsändringen kan ta några minuter att slutföra. När det är klart visas ett meddelande som liknar följande och som innehåller resultatet:

        ```azurecli
        provisioningState       : Succeeded
        ```
När du har aktiverat övervakning, kan det ta ungefär 15 minuter innan du kan visa hälsomått för klustret. 

## <a name="verify-agent-and-solution-deployment"></a>Kontrollera distributionen av agenten och lösning
Med agentversion *06072018* eller senare, kan du kontrollera att både agenten och lösningen har distribuerats. Du kan kontrollera endast agentdistribution med tidigare versioner av agenten.

### <a name="agent-version-06072018-or-later"></a>Agentversion 06072018 eller senare
Kör följande kommando för att kontrollera att agenten har distribuerats. 

```
kubectl get ds omsagent --namespace=kube-system
```

Utdata bör likna följande, vilket betyder att den har distribuerats korrekt:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Kontrollera distributionen av lösningen genom att köra följande kommando:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Utdata bör likna följande, vilket betyder att den har distribuerats korrekt:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Tidigare än 06072018 agentversion

Verifiera att agentversionen Log Analytics som publicerades före *06072018* har distribuerats korrekt, kör du följande kommando:  

```
kubectl get ds omsagent --namespace=kube-system
```

Utdata bör likna följande, vilket betyder att den har distribuerats korrekt:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Visa konfiguration med CLI
Använd den `aks show` för att hämta information om sådana som är den lösning som har aktiverats eller inte, vad är Log Analytics-arbetsyta resourceID och sammanfattningsinformation om klustret.  

```azurecli
az aks show -g <resoourceGroupofAKSCluster> -n <nameofAksCluster>
```

Efter ett par minuter kommandot har slutförts och returnerar JSON-formaterad information om lösningen.  Resultatet av kommandot ska visa övervakning tillägg profilen och liknar följande Exempelutdata:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```
## <a name="next-steps"></a>Nästa steg

* Om det uppstår problem vid försök att publicera lösningen kan du granska den [felsökningsguide](monitoring-container-insights-troubleshoot.md)

* Med övervakning aktiverat för att avbilda hälsomått för både noder i AKS och poddar är dessa health-mått tillgängliga i Azure-portalen. Läs hur du använder Azure Monitor för behållare i [visa Azure Kubernetes Service health](monitoring-container-insights-analyze.md).