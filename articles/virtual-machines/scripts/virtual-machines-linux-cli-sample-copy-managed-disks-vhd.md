---
title: Skriptexempel för Azure CLI – Exportera/kopiera underliggande VHD för hanterade diskar till ett lagringskonto | Microsoft Docs
description: Skriptexempel för Azure CLI – Exportera/kopiera underliggande VHD för hanterade diskar till ett lagringskonto
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/17/2018
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: c5f06a8c8fb707a2bf0451f8e9ed391ac0c5bad9
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045262"
---
# <a name="exportcopy-the-underlying-vhd-of-a-managed-disk-to-a-storage-account-with-cli"></a>Exportera/kopiera en underliggande VHD från en hanterad disk till ett lagringskonto med CLI

Det här skriptet exporterar den underliggande virtuella hårddisken från en hanterad disk till ett lagringskonto i samma eller i en annan region. Först genererar skriptet SAS-URI:n för den hanterade disken och använder den sedan för att kopiera den virtuella hårddisken till ett lagringskonto. Använd det här skriptet för att kopiera dina hanterade diskar för regional utökning. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Förklaring av skript

Det här skriptet använder följande kommandon för att generera SAS-URI:n för en hanterad disk och kopierar den underliggande virtuella hårddisken till ett lagringskonto med hjälp av SAS-URI:n. Varje kommando i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [az disk grant-access](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Genererar skrivskyddad SAS som används för att kopiera den underliggande VHD-filen till ett lagringskonto eller för att ladda ned den till en lokal plats  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy#az_storage_blob_copy_start) | Kopierar en blob asynkront från ett lagringskonto till ett annat |

## <a name="next-steps"></a>Nästa steg

[Skapa en hanterad disk från en virtuell hårddisk](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Skapa en virtuell dator från en hanterad disk](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Mer information om Azure CLI finns i [Azure CLI-dokumentationen](https://docs.microsoft.com/cli/azure).

Ytterligare CLI-skriptexempel för virtuella datorer och hanterade diskar finns i [Dokumentation för virtuella Azure Linux-datorer](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
