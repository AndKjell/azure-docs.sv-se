---
title: Hantera åtkomst med RBAC och REST-API – Azure | Microsoft Docs
description: Lär dig mer om att hantera åtkomst för användare, grupper och program, med hjälp av rollbaserad åtkomstkontroll (RBAC) och REST API. Detta innefattar hur du listar åtkomst, ger åtkomst och tar bort åtkomst.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 859a410a4ff9204e8e52fbd2cc3b38823f4bb830
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435226"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>Hantera åtkomst med RBAC och REST API

[Rollbaserad åtkomstkontroll (RBAC)](overview.md) är sättet som du hantera åtkomst till resurser i Azure. Den här artikeln beskriver hur du hanterar åtkomst för användare, grupper och program med RBAC och REST API.

## <a name="list-access"></a>Visar åtkomst

I RBAC lista för att lista åtkomstförsök kommer du rolltilldelningar. Vill se rolltilldelningar kan du använda en av de [rolltilldelningar – lista](/rest/api/authorization/roleassignments/list) REST API: er. För att förfina dina resultat, anger du ett scope och eventuellt ett filter. För att anropa API: et, måste du ha åtkomst till den `Microsoft.Authorization/roleAssignments/read` igen i det specificerade omfånget. Flera [inbyggda roller](built-in-roles.md) beviljas åtkomst till den här åtgärden.

1. Börja med följande begäran:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. I URI: N, Ersätt *{omfång}* med den omfattning som du vill visa en lista över rolltilldelningar.

    | Omfång | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Prenumeration |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resursgrupp |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resurs |

1. Ersätt *{filter}* med villkor som du vill använda för att filtrera rolltilldelningslista.

    | Filter | Beskrivning |
    | --- | --- |
    | `$filter=atScope()` | Lista över rolltilldelningar för endast det angivna omfånget, exklusive rolltilldelningar i subscopes. |
    | `$filter=principalId%20eq%20'{objectId}'` | Lista över rolltilldelningar för en angiven användare, grupp eller tjänstens huvudnamn. |
    | `$filter=assignedTo('{objectId}')` | Lista över rolltilldelningar för en viss användare, inklusive de som ärvts från grupper. |

## <a name="grant-access"></a>Bevilja åtkomst

För att skapa åtkomst i RBAC skapar du rolltilldelningar. Du kan skapa en rolltilldelning med den [rolltilldelningar – skapa](/rest/api/authorization/roleassignments/create) REST API och ange säkerhetsobjekt, rolldefinitionen och omfång. För att anropa detta API, måste du ha åtkomst till den `Microsoft.Authorization/roleAssignments/write` igen. Av de inbyggda rollerna, endast [ägare](built-in-roles.md#owner) och [administratör för användaråtkomst](built-in-roles.md#user-access-administrator) beviljas åtkomst till den här åtgärden.

1. Använd den [rolldefinitioner – lista](/rest/api/authorization/roledefinitions/list) REST API eller se [inbyggda roller](built-in-roles.md) att hämta identifieraren för den rolldefinition som du vill tilldela.

1. Använd ett GUID-verktyg för att generera en unik identifierare som används för tilldelning rollidentifieraren. Identifieraren har formatet: `00000000-0000-0000-0000-000000000000`

1. Börja med följande begäran och brödtext:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. I URI: N, Ersätt *{omfång}* med omfånget för rolltilldelningen.

    | Omfång | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Prenumeration |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resursgrupp |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resurs |

1. Ersätt *{roleAssignmentName}* med GUID-identifierare rolltilldelningen.

1. I begärandetexten, Ersätt *{subscriptionId}* med ditt prenumerations-ID.

1. Ersätt *{roleDefinitionId}* med rollen definition identifierare.

1. Ersätt *{principalId}* med objekt-ID för användaren, gruppen eller tjänstens huvudnamn som ska tilldelas rollen.

## <a name="remove-access"></a>Tar bort åtkomst

I RBAC kan du ta bort en rolltilldelning för att ta bort åtkomst. Ta bort en rolltilldelning med den [rolltilldelningar – ta bort](/rest/api/authorization/roleassignments/delete) REST API. För att anropa detta API, måste du ha åtkomst till den `Microsoft.Authorization/roleAssignments/delete` igen. Av de inbyggda rollerna, endast [ägare](built-in-roles.md#owner) och [administratör för användaråtkomst](built-in-roles.md#user-access-administrator) beviljas åtkomst till den här åtgärden.

1. Hämta rollen tilldelning identifierare (GUID). Den här identifieraren returneras när du först skapa rolltilldelningen eller hämta dem genom att ange rolltilldelningar.

1. Börja med följande begäran:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. I URI: N, Ersätt *{omfång}* med omfattning för att ta bort rolltilldelningen.

    | Omfång | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Prenumeration |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resursgrupp |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resurs |

1. Ersätt *{roleAssignmentName}* med GUID-identifierare rolltilldelningen.

## <a name="next-steps"></a>Nästa steg

- [Distribuera resurser med Resource Manager-mallar och Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API-referens](/rest/api/azure/)
- [Skapa anpassade roller med hjälp av REST-API](custom-roles-rest.md)