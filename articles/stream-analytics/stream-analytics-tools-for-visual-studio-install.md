---
title: Konfigurera Azure Stream Analytics-verktyg för Visual Studio
description: Den här artikeln beskriver installationskraven och hur du ställer in Azure Stream Analytics-verktyg för Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 54eef98d85337f14ff9e10837f97ccd28a58afdf
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223465"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Installera Azure Stream Analytics-verktyg för Visual Studio
Azure Stream Analytics-verktyg stöd för Visual Studio 2017, 2015 och 2013. Den här artikeln beskriver hur du installerar och avinstallerar verktygen.

Mer information om hur du använder verktygen finns i [Stream Analytics-verktyg för Visual Studio](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Installera
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Ladda ned [Visual Studio 2017 (15.3 eller senare)](https://www.visualstudio.com/). Versionerna Enterprise (Ultimate/Premium), Professional och Community stöds. Versionen Express stöds inte. 
* Stream Analytics-verktyg är en del av den **Azure development** och **datalagring och bearbetning av** arbetsbelastningar i Visual Studio 2017. Aktivera någon av dessa två arbetsbelastningar som en del av Visual Studio-installationen.

Aktivera den **datalagring och bearbetning av** arbetsbelastning som visas:

![Arbetsbelastningen för lagring och bearbetning av data har valts](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Aktivera den **Azure development** arbetsbelastning som visas:

![Arbetsbelastningen Azure development har valts](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)

* I Verktyg-menyn väljer du **tillägg och uppdateringar**. Hitta Azure Data Lake och Stream Analytics-verktyg i installerade tillägg och klicka på **uppdatering** att installera det senaste tillägget. 

![Visual Studio-tillägg och uppdateringar](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-extensions-updates.png)

### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Installera Visual Studio 2015 eller Visual Studio 2013 uppdatering 4. Versionerna Enterprise (Ultimate/Premium), Professional och Community stöds. Versionen Express stöds inte. 
* Installera Microsoft Azure SDK för .NET version 2.7.1 eller senare med hjälp av den [installationsprogram för webbplattform](http://www.microsoft.com/web/downloads/platform.aspx).
* Installera [Azure Stream Analytics-verktyg för Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## <a name="update"></a>Uppdatering

### <a name="visual-studio-2017"></a>Visual Studio 2017
Den nya versionen påminnelsen som visas i Visual Studio-meddelandet.

![Visual Studio nya version påminnelse](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-new-version-reminder-vs-tools.png)

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 och Visual Studio 2015
De installerade Stream Analytics-verktyg för Visual Studio Sök automatiskt efter nya versioner. Följ anvisningarna i popup-fönstret för att installera den senaste versionen. 


## <a name="uninstall"></a>Avinstallera

### <a name="visual-studio-2017"></a>Visual Studio 2017
Dubbelklicka på installationsprogrammet för Visual Studio och välj **ändra**. Rensa den **Azure Data Lake och Stream Analytics Tools** kryssrutan från antingen den **datalagring och bearbetning av** arbetsbelastning eller **Azure development** arbetsbelastning.

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 och Visual Studio 2015
Gå till Kontrollpanelen och avinstallera **Microsoft Azure Data Lake och Stream Analytics tools för Visual Studio**.





