---
title: Exempel filteruttryck som är möjligt med Azure Machine Learning databearbetning | Microsoft Docs
description: Det här dokumentet innehåller en uppsättning exempel på filteruttryck som är möjligt med Azure Machine Learning förberedelse av data
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: a389007dea344de461b23b96f2942686005aa119
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982400"
---
# <a name="sample-of-filter-expressions-python"></a>Exempel på filteruttryck (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Innan du läser den här bilagan läsa [Python extensibility översikt](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Filtrera med liknande test
Filtrera i endast de rader där värdet för Col2 (numeriska) är större än 4. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Filtrera med flera kolumner 
Filtrera i endast de rader där Kol1 innehåller värdet **bra** och Col2 innehåller (numeriska) värdet 1. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Testa filtret mot null
Filtrera i endast de rader där Kol1 har ett null-värde. 
```python
    pd.isnull(row["Col1"])
```
