---
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must define your experiment in Partner Center.
title: 在合作夥伴中心定義您的實驗
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 7818d9e251233c757618d60abaa156d294afb4b5
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933675"
---
# <a name="define-your-experiment-in-partner-center"></a>在合作夥伴中心定義您的實驗

在您[建立專案與定義遠端變數在合作夥伴中心](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)並[編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)之後，您準備好在專案中建立實驗。 當您建立實驗時，您將會定義目標及使用者將接收的變化。

如需示範建立及執行實驗的端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>建立實驗

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **您的應用程式**下方，選取您要建立實驗的 app。
3. 在瀏覽窗格中，選取 **[服務]**，然後選取 **[實驗]**。
4. 在 **[實驗]** 頁面上，在專案表格中找出您要新增實驗的專案，然後對該專案按一下 **[新增實驗]** 連結。
5. 在 **[實驗名稱]** 欄位中，輸入您可用來輕鬆識別實驗的名稱。 建立實驗後，此名稱會出現在您 App **[實驗]** 頁面上現有實驗的清單中，以及出現在專案的頁面上。
6. 如果您想要在實驗作用中進行編輯，按一下 **[可編輯的實驗]** 核取方塊。 僅在您是透過內部測試建立實驗以進行所有變化的驗證時，才選取此方塊。 如需詳細資訊，請參閱[建立內部測試的實驗](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments)。
    > [!NOTE]
    > 如果您建立的實驗將會發行給客戶 (也就是實驗與專案識別碼相關聯，而該識別碼是在提供給客戶的應用程式版本中使用)，請勿選取此方塊。 編輯作用中的實驗，將會使實驗結果無效。

7. 在 **[專案名稱]** 下拉式清單中，會自動選取目前的專案。 如果您想要將新實驗新增到不同專案，可以在此處選取該專案。 否則，請不要變更此選項。
8.   請記下[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。 當您[編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)，您必須參考此識別碼在程式碼中讓您能夠接收變化資料並向合作夥伴中心報告檢視和轉換事件。
9. 在 **[檢視事件]** 區塊中，在 **[檢視事件名稱]** 欄位中輸入適用於您實驗的[檢視事件](run-app-experiments-with-a-b-testing.md#terms)名稱。
10. 在 **[目標和轉換事件]** 區段中，至少為您的實驗定義一個目標：
  * 在 **[目標名稱]** 欄位中，輸入您目標的描述性名稱。 您執行實驗後，此名稱會出現在實驗的結果摘要中。
  * 在 **[轉換事件名稱]** 欄位中，輸入此目標的[轉換事件](run-app-experiments-with-a-b-testing.md#terms)名稱。
  * 在 **[目標]** 欄位中，根據您是否要將轉換事件發生次數最大化或最小化，選擇 **[最大化]** 或 **[最小化]**。 此資訊會在實驗的結果摘要中使用。

> [!NOTE]
> 合作夥伴中心僅會回報第一個轉換事件每個使用者檢視在 24 小時內。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止當目標設為將執行轉換之使用者人數最大化時，單一使用者會扭曲一組範例使用者的實驗結果。

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>針對實驗定義遠端變數及變化

接下來，針對實驗定義遠端[變數](run-app-experiments-with-a-b-testing.md#terms)及[變化](run-app-experiments-with-a-b-testing.md#terms)。

1. 在 **[遠端變數和變化]** 區段中，您應該會看到以下兩個預設變化：**[變化 A (控制項)]** 和 **[變化 B]**。若您想要更多變化，請按一下 **[新增變化]**。 或者，您亦可重新命名每個變化。
2. 根據預設，變化會平均散佈至您的 App 使用者。 若您想要選擇特定的散佈百分比，請清除 **[平均散佈]** 核取方塊，然後在 **[散佈 (%)]** 列中輸入百分比。
3. 將遠端變數加入到您的變化。 在本區段底部的下拉式控制項中，選擇您要加入的每個變數，然後按一下 **\[加入變數\]**。
    > [!NOTE]
    > 此控制項中列出的變數繼承自專案以供實驗使用。 變數的預設值 (如專案中所定義) 會自動指派給控制項變化。 如果您想要建立並未在此處列出的新變數，請移至相關的專案頁面，然後從該處加入變數。

4. 編輯實驗中每個唯一變化的變數值 (也就是除了控制項變化之外的變化)。

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>儲存並啟用實驗

完成輸入您實驗的必要欄位後，按一下 **[儲存]** 以儲存您的實驗。

若您滿意實驗參數且已準備將其啟用，則可開始從您的 App 收集實驗資料，然後按一下 **[啟用]**。 實驗作用中時，您的應用程式可以擷取變化變數，並向合作夥伴中心報告檢視和轉換事件。 如需詳細資訊，請參閱[執行和管理您的實驗，在合作夥伴中心](manage-your-experiment.md)。

> [!IMPORTANT]
> 一個專案一次僅能包含一個作用中的實驗。 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時選取 **[可編輯的實驗]** 核取方塊。 我們建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>建立內部測試的實驗

您可能想要利用受控制的對象來測試實驗 (例如一組內部測試人員)，並確認所有的變化如預期般運作，然後才將實驗針對客戶啟用。 您可以透過建立已選取 **[可編輯的實驗]** 選項的實驗來達成。

若要在發行給客戶之前先測試實驗，請遵循此程序：

1. 建立兩個專案：一個用於 App 的公用組建，另一個用於僅提供給測試對象的 App 私人組建。 以下的指示將這些專案分別稱為公用專案與測試專案。
2. 當您[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)時，在 App 的公用組建中參照公用專案的專案識別碼。 在 App 的私人組建中，參照測試專案的專案識別碼。
3. 在測試專案中建立實驗，並選取該實驗 **[可編輯的實驗]** 選項。
4. 在測試專案中啟用實驗。 對某一個變化配置 100% 散佈，並確認此變化針對您的測試者如預期般運作。 針對其他變化重複此程序。
5. 在您確認變化如預期般運作後，在測試專案中對實驗進行任何最後變更。 當您準備好對客戶發行實驗時，將實驗複製到公用專案。 在這個實驗中，請勿選取 **[可編輯的實驗]** 選項。
4. 請確定複製的實驗中，目標變化的散佈正確無誤。
5. 啟用複製的實驗以向您的客戶發行實驗。

## <a name="next-steps"></a>後續步驟

您在合作夥伴中心中定義您的實驗，並在您的應用程式中程式碼實驗之後，您已經準備好[執行和管理您的實驗，在合作夥伴中心](manage-your-experiment.md)。

## <a name="related-topics"></a>相關主題

* [建立專案與定義遠端變數在合作夥伴中心](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)
