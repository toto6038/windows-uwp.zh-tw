---
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must create a project and define your remote variables in Partner Center.
title: 在合作夥伴中心建立實驗專案
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: acfd654f02cb7fb727d35271175e59966e2abdc4
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7837333"
---
# <a name="create-an-experiment-project-in-partner-center"></a>在合作夥伴中心建立實驗專案

若要開始進行實驗，在合作夥伴中心中建立實驗[專案](run-app-experiments-with-a-b-testing.md#terms)以供您的應用程式並定義您的應用程式可以存取的遠端變數。

下列指示說明建立專案的核心步驟。 如需示範建立及執行實驗之端對端程序的詳細逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="instructions"></a>指示

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **您的應用程式**下方，選取您要建立實驗的 app。
3. 在瀏覽窗格中，選取 **\[服務\]**，然後選取 **\[實驗\]**。
4. 在 **\[實驗\]** 頁面中，按一下 **\[專案\]** 區段中的 **\[新增專案\]** 按鈕。 如果您已經建立一或多個專案，這些專案就會列在 **\[專案\]** 區段中。
5. 在 **\[新增專案\]** 頁面中，輸入您的新專案名稱。
6. 在 **\[遠端變數\]** 區段中，新增您想要提供給此專案中所有實驗使用的[變數](run-app-experiments-with-a-b-testing.md#terms)，並定義每個變數的預設值。 您在此處指定的預設值會用於實驗的控制群組，以及所有未參與實驗的使用者。
  1. 如果 **\[遠端變數\]** 區段已摺疊，請按一下區段標題上的 **\[顯示\]**。
  2. 按一下 **\[新增變數\]** 來建立每個您想要在這個專案中提供給任何實驗的新變數，然後輸入變數名稱和變數的預設值。
  3. 當您完成新增變數之後，按一下 **\[儲存\]**。
3. 在 **\[SDK 整合\]** 區段中，記下[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。 當您[編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)，您必須參考此專案識別碼在程式碼中讓您能夠接收變化資料並向合作夥伴中心報告檢視和轉換事件。

> [!NOTE]
> 當專案中的實驗處為使用中時，您無法編輯、新增或移除遠端變數。 此限制可針對作用中的實驗協助保護控制群組資料的完整性。


## <a name="next-steps"></a>後續步驟

建立專案之後，您可以[編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)以開始在 app 中擷取遠端變數值，而您可以[在專案中建立實驗](define-your-experiment-in-the-dev-center-dashboard.md)。

## <a name="related-topics"></a>相關主題

* [編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)
