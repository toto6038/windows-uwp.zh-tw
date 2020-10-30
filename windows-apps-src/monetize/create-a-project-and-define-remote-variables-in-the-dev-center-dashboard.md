---
description: 您必須先建立專案，並在合作夥伴中心中定義遠端變數，才能在通用 Windows 平臺 (UWP) 應用程式中執行實驗。
title: 在合作夥伴中心建立實驗專案
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 19eccb6ebc7556fb3dc2c6c11969361e19f802f3
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032761"
---
# <a name="create-an-experiment-project-in-partner-center"></a>在合作夥伴中心建立實驗專案

若要開始進行實驗，請在合作夥伴中心中為您的應用程式建立測試 [專案](run-app-experiments-with-a-b-testing.md#terms) ，並定義您的應用程式可以存取的遠端變數。

下列指示說明建立專案的核心步驟。 如需示範建立及執行實驗之端對端程序的詳細逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="instructions"></a>Instructions

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **您的應用程式** 下方，選取您要建立實驗的 app。
3. 在流覽窗格中，選取 [ **服務** ]，然後選取 [ **實驗** ]。
4. **在 [測試] 頁面上** ，按一下 [ **專案** ] 區段中的 [ **新增專案** ] 按鈕。 如果您已經建立一或多個專案，這些專案會列在 [ **專案** ] 區段中。
5. 在 [ **新增專案** ] 頁面中，輸入新專案的名稱。
6. 在 [ **遠端變數** ] 區段中，新增您想要在此專案中的所有實驗中使用的 [變數](run-app-experiments-with-a-b-testing.md#terms) ，並定義每個變數的預設值。 您在此處指定的預設值會用於實驗的控制群組，以及所有未參與實驗的使用者。
  1. 如果 [ **遠端變數** ] 區段已折迭，請按一下區段標題上的 [ **顯示** ]。
  2. 按一下 [ **加入變數** ]，以建立您想要在此專案中的任何實驗使用的新變數，並輸入變數名稱和變數的預設值。
  3. 當您完成新增變數時，請按一下 [ **儲存** ]。
3. 在 [ **SDK 整合** ] 區段中，記下 [ [專案識別碼](run-app-experiments-with-a-b-testing.md#terms) ] 值。 當您 [為應用程式撰寫程式碼以進行實驗](code-your-experiment-in-your-app.md)時，您必須在程式碼中參考此專案識別碼，才能接收變化資料和報表檢視，以及將事件轉換成合作夥伴中心。

> [!NOTE]
> 當專案中的實驗處為使用中時，您無法編輯、新增或移除遠端變數。 此限制可針對作用中的實驗協助保護控制群組資料的完整性。


## <a name="next-steps"></a>下一步

建立專案之後，您可以[編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)以開始在 app 中擷取遠端變數值，而您可以[在專案中建立實驗](define-your-experiment-in-the-dev-center-dashboard.md)。

## <a name="related-topics"></a>相關主題

* [編寫實驗用的應用程式程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行應用程式實驗](run-app-experiments-with-a-b-testing.md)
