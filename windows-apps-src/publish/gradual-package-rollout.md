---
author: JnHs
Description: When you publish an update to a submission, you can choose to gradually roll out the updated packages to a percentage of your app’s customers on Windows 10.
title: 漸進式套件推出
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 65d578a6-4e26-484c-90af-b2cd916f3634
ms.localizationpriority: medium
ms.openlocfilehash: 407ffb5fdebdc90a63ed7f65b4e97f8358dc58c8
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2018
ms.locfileid: "3381653"
---
# <a name="gradual-package-rollout"></a>漸進式套件推出

當您發佈提交的更新時，您可以選擇以漸進方式推出給已更新套件以您 app 的客戶的百分比來表示在 Windows 10 （包括 Xbox） 上。 這可讓您監視特定套件的意見反應和分析資料，以確保您在更廣泛地推出更新之前，就已經信心滿滿。 您隨時可以增加百分比 (或停止更新)，而不必建立新的提交。 

> [!IMPORTANT]
> 您的推出選項適用於所有套件，但是僅適用於執行可支援套件正式發行前小眾測試版之作業系統 (Windows.Desktop 組建 10586 或更新版本；Windows.Mobile 組建 10586.63 或更新版本，以及 Xbox) 的客戶，包括透過[市集管理 (線上) 授權](organizational-licensing.md)和[商務用 Microsoft 網上商店](https://businessstore.microsoft.com/store)或[教育用 Microsoft 網上商店](https://educationstore.microsoft.com/store)取得應用程式的所有客戶。 使用漸進式套件推出時，舊版作業系統的客戶將不會從最新的提交取得套件，直到您完成套件推出為止，如下所述。

請注意，您所有的客戶都會看到您與最新的提交一起輸入的市集清單詳細資料。 推出設定僅適用於客戶收到的套件 (針對新的下載數和現有客戶的更新)。

> [!TIP]
> 套件推出會將套件散佈到您指定百分比的隨機選取客戶。 若要將特定套件散佈到您指定的所選客戶，您可以使用套件正式發行前小眾測試版。 如果您想要以漸進的方式，將更新散佈到您其中一個正式發行前小眾測試版群組，您也可以將推出結合套件正式發行前小眾測試版。


## <a name="setting-the-rollout-percentage"></a>設定推出百分比

您可以選擇在更新提交的 **\[套件\]** 頁面上推出您的更新。 若要這樣做，請核取說明 **\[發佈此提交之後逐漸推出更新 (僅限 Windows 10 客戶)\]** 的方塊。 接著，輸入首次發佈提交時，應該取得更新的客戶的百分比。 例如，如果您想要從將更新僅推出到一小部分的 App 客戶開始，您可以輸入 5。

按一下 **\[更新\]** 以儲存您的選取項目。 在您的 App 完成認證程序之後，將會根據您指定的百分比，將套件散佈給客戶 (針對新的下載數及現有客戶的更新)。


## <a name="adjusting-the-rollout-after-the-submission-is-published"></a>發佈提交後調整推出

若要在發佈提交後調整推出，移至您 App 的 [概觀] 頁面。 您可以拖曳選取器以變更從您的最新提交取得套件之客戶的百分比。 按一下 **\[更新\]** 以儲存您的選取項目。 接著，將會根據您指定的百分比，開始將套件散佈給客戶 (針對新的下載數及現有客戶的更新)。


## <a name="completing-the-rollout"></a>完成推出

在您可以建立新的提交之前，必須完成套件推出。 您可以**完成**推出，並將套件散佈給您的所有客戶，或**停止**推出，以停止散佈最新的套件。

如果您對更新有信心，而且想要將其提供給所有客戶，按一下 **\[完成套件推出\]**，將最新的套件散佈給您的所有客戶。

> [!TIP]
> 將推出百分比變更為 100% 並不會確保您的所有客戶將會從最新的提交取得套件，因為部分客戶可能使用不支援推出的作業系統版本。 您必須完成推出才能停止散佈舊版套件，並將所有現有的客戶更新為新版套件。

如果您發現更新有問題，而且您不想要再散佈該更新，您可以按一下 **\[停止套件推出\]**，停止從最新的提交散佈套件。 一旦您停止套件推出之後，這些套件將不會再散佈給任何客戶；只有來自先前提交的套件才會用於任何新的或更新的客戶。 不過，已經有新版套件的任何客戶都將保留這些套件；這些套件將不會回復到先前的版本。 若要將更新提供給這些客戶，您需要使用您希望他們取得的套件，建立新的提交。 請注意，如果您在下一個提交中使用漸進式推出，擁有您所停止之套件的客戶將會以他們取得已停止之套件的相同順序，取得新的更新。 新的推出將會介於您上次完成的提交與您最新的提交之間；一旦您停止套件推出之後，這些套件將不會再散佈給任何客戶。
