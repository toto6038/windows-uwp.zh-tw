---
Description: You can use Partner Center to run experiments for your Universal Windows Platform (UWP) apps with A/B testing.
title: 使用 A/B 測試執行應用程式實驗
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: d4f5271d70cefea99a9caff04e7203e05043440c
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335489"
---
# <a name="run-app-experiments-with-ab-testing"></a>使用 A/B 測試執行應用程式實驗

您可以使用合作夥伴中心來定義您可以從您的通用 Windows 平台 (UWP) 應用程式，在執行階段擷取的遠端變數，您可以將這些值的變化測試您的使用者來找出針對推動所需的使用者行為最有效的值。 您的 App 可以使用遠端變數來設定 App 體驗，例如 App 內購買、註冊流程、標題和廣告放置。

您的 A/B 測試目的應該是找出遠端變數值的變化，以透過提供更吸引人的 App 體驗為您取得改善的轉換率 (例如更多的 App 內購買)。 您已找出成功的變化後，您就可以立即結束實驗，並啟用該變化對整個使用者群從合作夥伴中心，而不必重新發佈您的應用程式。

## <a name="create-and-run-an-ab-test"></a>建立和執行 A/B 測試

若要建立和執行 A/B 測試，請遵循下列步驟：

1. [建立專案與定義遠端變數在合作夥伴中心](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)。 這個專案包括您實驗的變數和預設變數值。  
2. [編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。 使用 Microsoft Store Services SDK 中的 API，取得您在合作夥伴中心中建立的專案的遠端變數的值、 使用此資料來修改您測試，此功能的行為，並將檢視事件和轉換事件傳送至合作夥伴中心。
3. [定義您在合作夥伴中心中的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 在您的專案中建立能定義 A/B 測試唯一目標和變化的實驗。
4. [執行和管理您的實驗，在合作夥伴中心 ashboard](manage-your-experiment.md)。 啟用實驗，並使用合作夥伴中心來檢閱實驗的結果並完成實驗。

如需示範端對端處理程序的逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="requirements"></a>需求

A / B 測試在合作夥伴中心支援僅適用於 UWP app。

您必須先設定您的開發電腦，才可以使用 A/B 測試執行實驗︰

* 依照[這裡](../get-started/get-set-up.md)的指示來設定適用於 UWP 開發的開發電腦。
* [安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了用於實驗的 API 之外，此 SDK 也提供其他功能的 API，例如顯示廣告以及將您的客戶引導至「意見反應中樞」來收集有關您 App 的意見反應。

## <a name="best-practices"></a>最佳做法

如需最實用的結果，我們建議您在使用 A/B 測試執行實驗時遵循下列建議︰

* 請考慮執行只有兩項變化的實驗，並以隨機化 50/50 分割分佈進行變化指派。
* 執行實驗至少 2 - 4 週，以收集具統計顯著性和可行的足夠資料。

<span id="terms" />

## <a name="related-terms"></a>相關詞彙

|  詞彙  |  定義  |
|--------|--------------|
| 專案    |   具有預設值的遠端變數集合，可供您的 App 使用 Microsoft Store Services SDK 進行存取。 專案也可以選擇包含一或多個共用相同遠端變數的實驗。  |
| 實驗    |   一組能定義使用者將會接收之 A/B 測試的參數。 實驗是在專案的範圍中定義，而每個實驗皆由以下項目組成： <p></p><ul><li>*「檢視事件」*，表示使用者開始檢視屬於實驗一部分之變化的時候。</li><li>具有 *「轉換事件」* 的一或多個目標，表示何時達到目標。</li><li>一或多個 *「變化」*，定義您的實驗所使用的變數資料。 *「控制項」* 變化會使用預設變數值，該值是在專案中針對實驗所定義。 除了控制項變化之外，實驗通常會有至少一個額外變化，其中含有針對實驗的唯一變數值。 </li></ul>          |
| 專案識別碼    |   將您的應用程式與您的合作夥伴中心帳戶中的專案相關聯的唯一識別碼。 您必須使用此識別碼以連接的 A / B 測試服務在您的應用程式程式碼，以接收變化資料並到合作夥伴中心回報檢視和轉換事件。 如需詳細資訊，請參閱[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。<p></p><p>每個專案 (及專案中的所有實驗) 皆會與單一專案識別碼相關聯。 您可使用專案識別碼來協助區別不同的實驗組合。 例如，您可能會將一組實驗發行給組織當中的測試者，並將另一組實驗僅發行給您 App 的外部使用者。  如果 App 會實作多個實驗，則它可參考多個專案識別碼。</p>         |
| 變化    |   您在實驗中進行測試的一或多個變數集合。 每個實驗至少必須具有一個變數和兩個變化 (包括控制項)。 實驗最多可具有五個變化。           |
| 變數    |  您的 App 用來初始化屬性或 App 中的某些其他值的值。 在實驗進行期間，變數的值會根據各個變化而有所變更。 結束實驗之後，系統會透過您選擇發行至 App 所有使用者的變化來指派變數值。 變數可為下列類型：字串、布林值、雙精度浮點數及整數。
| 檢視事件    |  表示使用者在開始檢視屬於您實驗一部份的變化時，代表其活動的任意字串。 通常，這是您程式碼中的事件名稱。 您的應用程式程式碼會將此檢視事件字串傳送到合作夥伴中心，在使用者開始檢視變化時。 如需詳細資訊，請參閱[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。
| 轉換事件    |  代表實驗目的之目標的任意字串。 通常，這是您程式碼中的事件名稱。 您的應用程式程式碼會將此轉換事件字串傳送到合作夥伴中心，當使用者達到。 如需詳細資訊，請參閱[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)。  

## <a name="related-topics"></a>相關主題

* [建立專案與定義遠端變數在合作夥伴中心](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
