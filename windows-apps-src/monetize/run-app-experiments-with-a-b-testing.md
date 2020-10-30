---
description: 您可以使用合作夥伴中心，透過 A/B 測試，對通用 Windows 平臺 (UWP) 應用程式執行實驗。
title: 使用 A/B 測試執行應用程式實驗
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: c38eee9e3be7d6ea85b56fd5ad3aa3d62ae751b9
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029764"
---
# <a name="run-app-experiments-with-ab-testing"></a>使用 A/B 測試執行應用程式實驗

您可以使用合作夥伴中心定義可在執行時間從您的通用 Windows 平臺 (UWP) 應用程式中取出的遠端變數，也可以使用您的使用者來測試這些值的變化，以找出最有效的值來駕駛所需的使用者行為。 您的 App 可以使用遠端變數來設定 App 體驗，例如 App 內購買、註冊流程、標題和廣告放置。

您的 A/B 測試目的應該是找出遠端變數值的變化，以透過提供更吸引人的 App 體驗為您取得改善的轉換率 (例如更多的 App 內購買)。 在您識別出成功的變化之後，您可以立即結束實驗，並為整個使用者的使用者提供合作夥伴中心的變化，而不需要重新發佈您的應用程式。

## <a name="create-and-run-an-ab-test"></a>建立和執行 A/B 測試

若要建立和執行 A/B 測試，請遵循下列步驟：

1. [建立專案，並在合作夥伴中心中定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)。 這個專案包括您實驗的變數和預設變數值。  
2. [編寫應用程式的程式碼以進行實驗](code-your-experiment-in-your-app.md)。 使用 Microsoft Store Services SDK 中的 API，從您在合作夥伴中心中所建立的專案取得遠端變數值、使用此資料修改您正在測試之功能的行為，以及將 view 事件和轉換事件傳送至合作夥伴中心。
3. [在合作夥伴中心中定義您的實驗 ](define-your-experiment-in-the-dev-center-dashboard.md)。 在您的專案中建立能定義 A/B 測試唯一目標和變化的實驗。
4. [在合作夥伴中心 ashboard 中執行及管理您的實驗](manage-your-experiment.md)。 啟用您的實驗，並使用合作夥伴中心來檢查實驗的結果，並完成實驗。

如需示範端對端處理程序的逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="requirements"></a>需求

只有 UWP 應用程式支援合作夥伴中心中的 A/B 測試。

您必須先設定您的開發電腦，才可以使用 A/B 測試執行實驗︰

* 依照[這裡](../get-started/get-set-up.md)的指示來設定適用於 UWP 開發的開發電腦。
* [安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了用於實驗的 API 之外，此 SDK 也提供其他功能的 API，例如顯示廣告以及將您的客戶引導至「意見反應中樞」來收集有關您 App 的意見反應。

## <a name="best-practices"></a>最佳作法

如需最實用的結果，我們建議您在使用 A/B 測試執行實驗時遵循下列建議︰

* 請考慮執行只有兩項變化的實驗，並以隨機化 50/50 分割分佈進行變化指派。
* 執行實驗至少 2 - 4 週，以收集具統計顯著性和可行的足夠資料。

<span id="terms" />

## <a name="related-terms"></a>相關詞彙

|  詞彙  |  定義  |
|--------|--------------|
| Project    |   具有預設值的遠端變數集合，可供您的 App 使用 Microsoft Store Services SDK 進行存取。 專案也可以選擇包含一或多個共用相同遠端變數的實驗。  |
| 實驗    |   一組能定義使用者將會接收之 A/B 測試的參數。 實驗是在專案的範圍中定義，而每個實驗皆由以下項目組成： <p></p><ul><li>表示使用者開始查看屬於實驗一部分之變化的 *視圖事件* 。</li><li>具有 *轉換事件* 的一或多個目標，指出達到目標的時間。</li><li>定義您的實驗所使用之變數資料的一或多個 *變化* 。 *控制項* 變異會使用專案中為實驗定義的預設變數值。 除了控制項變化之外，實驗通常會有至少一個額外變化，其中含有針對實驗的唯一變數值。 </li></ul>          |
| 專案識別碼    |   唯一的識別碼，可將您的應用程式與合作夥伴中心帳戶中的專案產生關聯。 您必須使用此識別碼與應用程式程式碼中的 A/B 測試服務連接，以接收合作夥伴中心的變化資料和報表檢視和轉換事件。 如需詳細資訊，請參閱[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。<p></p><p>每個專案 (及專案中的所有實驗) 皆會與單一專案識別碼相關聯。 您可使用專案識別碼來協助區別不同的實驗組合。 例如，您可能會將一組實驗發行給組織當中的測試者，並將另一組實驗僅發行給您 App 的外部使用者。  如果 App 會實作多個實驗，則它可參考多個專案識別碼。</p>         |
| 變化程度    |   您在實驗中進行測試的一或多個變數集合。 每個實驗至少必須具有一個變數和兩個變化 (包括控制項)。 實驗最多可具有五個變化。           |
| 變數    |  您的 App 用來初始化屬性或 App 中的某些其他值的值。 在實驗進行期間，變數的值會根據各個變化而有所變更。 結束實驗之後，系統會透過您選擇發行至 App 所有使用者的變化來指派變數值。 變數可為下列類型：字串、布林值、雙精度浮點數及整數。
| 檢視事件    |  表示使用者在開始檢視屬於您實驗一部份的變化時，代表其活動的任意字串。 通常，這是您程式碼中的事件名稱。 當使用者開始查看變化時，您的應用程式程式碼會將此 view 事件字串傳送至合作夥伴中心。 如需詳細資訊，請參閱[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。
| 轉換事件    |  代表實驗目的之目標的任意字串。 通常，這是您程式碼中的事件名稱。 當使用者達到目標時，您的應用程式程式碼會將此轉換事件字串傳送至合作夥伴中心。 如需詳細資訊，請參閱[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。  

## <a name="related-topics"></a>相關主題

* [在合作夥伴中心中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的應用程式程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
