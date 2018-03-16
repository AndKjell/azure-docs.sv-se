---
title: "Storlekar för virtuella datorer stöds i Azure-stacken | Microsoft Docs"
description: "Referens för storlek på stöds Virtuella datorer i Azure-stacken."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: brenduns
ms.openlocfilehash: fd352d6d073c5eff0ddf17588df022ffa3caef8c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/16/2018
---
# <a name="virtual-machine-sizes-supported-in-azure-stack"></a>Storlekar för virtuella datorer stöds i Azure-stacken

Den här artikeln innehåller de storlekar för virtuella datorer (VM) som stöds av Azure-stacken. 


## <a name="general-purpose"></a>Generellt syfte

### <a name="basic-a"></a>Basic A
|Storlek - Size\Name |Virtuell processor     |Minne | Högsta temporär diskstorlek | Dataflöde för max OS-disk: (IOPS) | Max temp genomflödet (IOPS) | Maximal disk datagenomströmning (IOPS) | Maximalt antal nätverkskort |    
|-----------------|-----|---------|---------|-----|------|-----------|----|
|**A0\Basic_A0**  |1    |768 MB   | 20 GB   |300  | 300  |1 / 1 x 300  |1   |
|**A1\Basic_A1**  |1    |1,75 GB  | 40 GB   |300  | 300  |2 / 2 x 300  |1   |
|**A2\Basic_A2**  |2    |3,5 GB   | 60 GB   |300  | 300  |4 / 4 x 300  |1   |
|**A3\Basic_A3**  |4    |7 GB     | 120 GB  |300  | 300  |8 / 8 x 300  |1   |
|**A4\Basic_A4**  |8    |14 GB    | 240 GB  |300  | 300  |6 / 16 X 300 |1   |

### <a name="standard-a"></a>Standard A 
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |    
|----------------|--|------|----|----|----|-------|---------|
|**Standard_A0** |1 |0.768 |20  |500 |500 |1 × 500  |1 / 100  |
|**Standard_A1** |1 |1.75  |70  |500 |500 |2 × 500  |1 / 500  |
|**Standard_A2** |2 |3.5   |135 |500 |500 |4 × 500  |1 / 500  |
|**Standard_A3** |4 |7     |285 |500 |500 |8 × 500  |2/1 000 |
|**Standard_A4** |8 |14    |605 |500 |500 |16 × 500 |4/2 000 |
|**Standard_A5** |2 |14    |135 |500 |500 |4 × 500  |2/500  |
|**Standard_A6** |4 |28    |285 |500 |500 |8 × 500  |2/1 000 |
|**Standard_A7** |8 |56    |605 |500 |500 |16 × 500 |4/2 000 |


### <a name="d-series"></a>D-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|----------------|----|----|-----|----|------|------------|---------|
|**Standard_D1** |1   |3.5 |50   |500 |3000  |4 / 4 x 500   |1 / 500  |
|**Standard_D2** |2   |7   |100  |500 |6000  |8 / 8 x 500   |2/1 000 |
|**Standard_D3** |4   |14  |200  |500 |12000 |16 / 16 x 500 |4/2 000 |
|**Standard_D4** |8   |28  |400  |500 |24000 |32 / 32 x 500 |8/4 000 |


### <a name="ds-series"></a>DS-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|-----------------|----|----|-----|-----|------|-------------|---------|
|**Standard_DS1** |1   |3.5 |7    |1000 |4000  |4 / 4 x 2300   |1 / 500  |
|**Standard_DS2** |2   |7   |14   |1000 |8000  |8 / 8 x 2300   |2/1 000 |
|**Standard_DS3** |4   |14  |28   |1000 |16000 |16 / 16 x 2300 |4/2 000 |
|**Standard_DS4** |8   |28  |56   |1000 |32000 |32 / 32 x 2300 |8/4 000 |

### <a name="dv2-series"></a>Dv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3.5 |50   |500 |3000  |4 / 4 x 500   |1 / 500  |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8 / 8 x 500   |2/1 000 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16 / 16 x 500 |4/2 000 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32 / 32 x 500 |8/4 000 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64 / 64 x 500 |8 / 8000 |

### <a name="dsv2-series"></a>DSv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3.5 |7   |1000 |4000  |4 / 4 x 2300   |1 / 750  |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8 / 8 x 2300   |2/1 500 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16 / 16 x 2300 |4/3 000 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32 / 32 x 2300 |8/6 000 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64 / 64 x 2300 |8 / 10000 |


## <a name="memory-optimized"></a>Minnesoptimerad

### <a name="mo-d"></a>D-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|------------------|---|----|----|--------|------|------------|---------|
|**Standard_D11**  |2  |14  |100 |500     |6000  |8 / 8 x 500   |2/1 000 |
|**Standard_D12**  |4  |28  |200 |500     |12000 |16 / 16 x 500 |4/2 000 |
|**Standard_D13**  |8  |56  |400 |500     |24000 |32 / 32 x 500 |8/4 000 |
|**Standard_D14**  |16 |112 |800 |500     |48000 |64 / 64 x 500 |8 / 8000 |

### <a name="mo-ds"></a>DS-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|-------------------|---|----|----|--------|------|-------------|---------|
|**Standard_DS11**  |2  |14  |28  |1000    |8000  |8 / 8 x 2300   |2/1 000 |
|**Standard_DS12**  |4  |28  |56  |1000    |12000 |16 / 16 x 2300 |4/2 000 |
|**Standard_DS13**  |8  |56  |112 |1000    |32000 |32 / 32 x 2300 |8/4 000 |
|**Standard_DS14**  |16 |112 |224 |1000    |64000 |64 / 64 x 2300 |8 / 8000 |

### <a name="mo-dv2"></a>Dv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8 / 8 x 500    |2/1 500 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16 / 16 x 500  |4/3 000 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32 / 32 x 500  |8/6 000 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64 / 64 x 500  |8 / 10000 |


### <a name="mo-dsv2"></a>DSv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temporär lagring (GiB)  | Dataflöde för max OS-disken (IOPS) | Max temp genomflödet (IOPS) | Maximalt antal datadiskar / dataflöde (IOPS) | Högsta antal nätverkskort / förväntat nätverksbandbredd (Mbps) |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |8 / 8 x 2300    |2/1 500 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |16 / 16 x 2300  |4/3 000 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |32 / 32 x 2300  |8/6 000 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |64 / 64 x 2300  |8 / 10000 |


## <a name="next-steps"></a>Nästa steg

[Överväganden för virtuella datorer i Azure-stacken](azure-stack-vm-considerations.md)
