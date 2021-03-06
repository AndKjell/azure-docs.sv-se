---
title: Återställa lokala lösenord för Linux på Azure Virtual Machines | Microsoft Docs
description: Introducera stegen för att återställa det lokala Linux-lösenordet på Azure VM
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 37b02b657278782ea82f3eb129335793a0b5c98d
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414602"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Återställa lokala lösenord för Linux på Azure Virtual Machines

Den här artikeln beskrivs flera metoder för att återställa lösenord för lokala Linux virtuell dator (VM). Om användarkontot har upphört att gälla eller om du bara vill skapa ett nytt konto, kan du använda följande metoder för att skapa ett nytt lokalt administratörskonto och återfå åtkomst till den virtuella datorn.

## <a name="symptoms"></a>Symtom

Du kan inte logga in till den virtuella datorn och du får ett meddelande som anger att det lösenord som du använde är felaktig. Du kan dessutom använda VMAgent för att återställa lösenordet på Azure portal.

## <a name="manual-password-reset-procedure"></a>Manuell proceduren för lösenordsåterställning.

1.  Ta bort den virtuella datorn och hålla anslutna diskar.

2.  Koppla OS-enhet som en datadisk till en annan temporala virtuell dator på samma plats.

3.  Kör följande kommando för SSH på den tillfälliga virtuella datorn blir en superanvändare.

    ```bash
    sudo su
    ```

4.  Kör **fdisk -l** eller titta på systemloggarna för att hitta den nyligen anslutna disken. Leta upp enhetsnamnet att montera. Sedan på den tillfälliga virtuella datorn, titta i relevanta loggfilen.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Följande är exempel på utdata från grep-kommandot:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  Skapa en monteringspunkt som kallas **tempmount**.

    ```bash
    mkdir /tempmount
    ```

6.  Montera OS-disken på monteringspunkten. Vanligtvis måste monteras *sdc1* eller *sdc2*. Detta beror på den värdbaserade partitionen i */etc* katalogen från disk bruten dator.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Skapa kopior av grundläggande credential filer innan du gör några ändringar:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  Återställ användarens lösenord som du behöver:

    ```bash
    passwd <<USER>> 
    ```

9.  Flytta ändrade filer till rätt plats på den bruten datorns disk.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Gå tillbaka till roten och demontera disken.

    ```bash
    cd /
    umount /tempmount
    ```

11. Koppla bort disken från hanteringsportalen.

12. Återskapa den virtuella datorn.

## <a name="next-steps"></a>Nästa steg

* [Felsöka Azure virtuell dator genom att koppla OS-disken till en annan virtuell Azure-dator](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: Hur du tar bort och distribuera en virtuell dator från virtuell Hårddisk](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
