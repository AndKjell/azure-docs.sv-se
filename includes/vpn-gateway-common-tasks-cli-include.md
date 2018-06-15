---
title: ta med fil
description: ta med fil
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e021a1b109f735dee16d75c05c26ab35e599a3d9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197711"
---
### <a name="to-view-local-network-gateways"></a>Visa lokala nätverksgatewayer

Du kan visa en lista över lokala nätverks-gatewayer med kommandot [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#az_network_local_gateway_list).

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="to-verify-the-shared-key-values"></a>Kontrollera delade nyckelvärden

Kontrollera att värdet för den delade nyckeln är samma värde som du använde till VPN-enhetens konfiguration. Om inte ska du antingen köra anslutningen igen med värdet från enheten eller uppdatera enheten med värdet från resultatet. Värdena måste matcha. Om du vill visa den delade nyckeln använder du [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#az_network_vpn_connection_list).

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="to-view-the-vpn-gateway-public-ip-address"></a>Visa den offentliga IP-adressen för VPN-gatewayen

Använd kommandot [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_list) för att hitta den offentliga IP-adressen till din virtuella nätverksgateway. För att läsningen ska gå lätt är utdata formaterade för att visa listan över offentliga IP-adresser i tabellformat.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
