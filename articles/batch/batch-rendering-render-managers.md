---
title: Azure Batch rendering manager-stöd
description: Använda Azure för rendering med hjälp av Azure Batch rendering manager-integrering
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 066aab598628701bf7a60b0f4f20d996348fa5ce
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406729"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Med Azure Batch med rendering gruppen Chefer

Om du använder en befintlig återge lokal servergrupp, så är det mycket troligt att en hanterare för rendering styr rendering servergruppen kapacitet och rendera jobb.

Azure ger inbyggt stöd eller tillägg till populära rendering chefer. Du kan lägga till och ta bort Azure virtuella datorer, inklusive virtuella datorer med betala för det du använder programmet licensiering och lågprioriterade virtuella datorer.

Följande rendering chefer stöds:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal rendering](http://www.royalrender.de/)
* [Thinkbox tidsgräns](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Med hjälp av Azure med PipelineFX Qube

Skript och instruktioner för att aktivera Azure Batch-poolen virtuella datorer som ska användas som Qube arbetare är i [GitHub-lagringsplatsen](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Med hjälp av Azure med Royal rendering

Royal rendering har Azure och Azure Batch-integrering inbyggda, så att du kan utöka en renderingsgrupp med Azure-baserade virtuella datorer. En sammanfattning finns [hjälpfilerna](http://www.royalrender.de/help8/index.html?Cloudrendering.html).

Ett exempel på Royal rendera kund som använder Azure-integrering finns i den [Jellyfish bilder kundberättelsen](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Med hjälp av Azure med Thinkbox tidsgräns

Skript och instruktioner för att aktivera Azure Batch-poolen virtuella datorer som ska användas eftersom tidsgränsen slaves [GitHub-lagringsplatsen](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Nästa steg

Prova att använda Azure Batch-integrering för chefen rendering med hjälp av lämplig plugin-programmet och anvisningar om GitHub, om tillämpligt.