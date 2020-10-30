---
description: 您必須在合作夥伴中心中定義實驗，才能在通用 Windows 平臺 (UWP) 應用程式中執行實驗。
title: 在合作夥伴中心定義您的實驗
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 909fa6b36f0b6dc51a3bb6eac117abae3b8b73ae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033211"
---
# <a name="define-your-experiment-in-partner-center"></a>在合作夥伴中心定義您的實驗

在您 [建立專案並在合作夥伴中心中定義遠端變數並在](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) 測試 [應用程式的程式碼](code-your-experiment-in-your-app.md)之後，您就可以在專案中建立實驗。 當您建立實驗時，您將會定義目標及使用者將接收的變化。

如需示範建立及執行實驗的端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>建立實驗

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **您的應用程式** 下方，選取您要建立實驗的 app。
3. 在流覽窗格中，選取 [ **服務** ]，然後選取 [ **實驗** ]。
4. **在 [測試] 頁面上** ，識別您要在 [專案] 資料表中新增實驗的專案，然後按一下該專案的 [ **加入實驗** ] 連結。
5. 在 [ **實驗名稱** ] 欄位中，輸入您可以用來輕鬆識別實驗的名稱。 建立實驗之後，此名稱會出現在您應用程式的測試頁面和專案頁面 **上的現有** 實驗清單中。
6. 如果您想要在實驗處於作用中狀態時進行編輯，請按一下 [ **可編輯的實驗** ] 核取方塊。 僅在您是透過內部測試建立實驗以進行所有變化的驗證時，才選取此方塊。 如需詳細資訊，請參閱[建立內部測試的實驗](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments)。
    > [!NOTE]
    > 如果您建立的實驗將會發行給客戶 (也就是實驗與專案識別碼相關聯，而該識別碼是在提供給客戶的應用程式版本中使用)，請勿選取此方塊。 編輯作用中的實驗，將會使實驗結果無效。

7. 在 [ **專案名稱** ] 下拉式清單中，會自動選取目前的專案。 如果您想要將新實驗新增到不同專案，可以在此處選取該專案。 否則，請不要變更此選項。
8.   請記下[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。 當您 [為應用程式撰寫程式碼以進行實驗](code-your-experiment-in-your-app.md)時，您必須在程式碼中參考此識別碼，才能接收變化資料和報表檢視，以及將事件轉換成合作夥伴中心。
9. 在 [ **view** 事件] 區段中，于 [ **view event name** ] 欄位中輸入實驗的 [view 事件](run-app-experiments-with-a-b-testing.md#terms)名稱。
10. 在 [ **目標與轉換事件** ] 區段中，為您的實驗定義至少一個目標：
  * 在 [ **目標名稱** ] 欄位中，輸入您的目標的描述性名稱。 您執行實驗後，此名稱會出現在實驗的結果摘要中。
  * 在 [ **轉換事件名稱** ] 欄位中，為此目標輸入 [轉換事件](run-app-experiments-with-a-b-testing.md#terms) 的名稱。
  * 在 [ **目標** ] 欄位中，選擇 [ **最大化** ] 或 [ **最小化** ]，取決於您想要最大化或最小化轉換事件的發生次數。 此資訊會在實驗的結果摘要中使用。

> [!NOTE]
> 合作夥伴中心只會在24小時的時間內，報告每個使用者視圖的第一個轉換事件。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止當目標設為將執行轉換之使用者人數最大化時，單一使用者會扭曲一組範例使用者的實驗結果。

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>針對實驗定義遠端變數及變化

接下來，針對實驗定義遠端[變數](run-app-experiments-with-a-b-testing.md#terms)及[變化](run-app-experiments-with-a-b-testing.md#terms)。

1. 在 [ **遠端變數和變化** ] 區段中，您應該會看到兩個預設的變化， **(控制項)** 和 **變化 B** 的變化。如果您想要更多的變化，請按一下 [ **新增變化** ]。 或者，您亦可重新命名每個變化。
2. 根據預設，變化會平均散佈至您的 App 使用者。 如果您想要選擇特定的分配百分比，請清除 [ **平均散發** ] 核取方塊，然後在 **散發 (% )** 資料列中輸入百分比。
3. 將遠端變數加入到您的變化。 在此區段底部的下拉式清單控制項中，選擇您要加入的每個變數，然後按一下 [ **加入變數** ]。
    > [!NOTE]
    > 此控制項中列出的變數繼承自專案以供實驗使用。 變數的預設值 (如專案中所定義) 會自動指派給控制項變化。 如果您想要建立並未在此處列出的新變數，請移至相關的專案頁面，然後從該處加入變數。

4. 編輯實驗中每個唯一變化的變數值 (也就是除了控制項變化之外的變化)。

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>儲存並啟用實驗

當您完成輸入實驗的必要欄位時，請按一下 [ **儲存** ] 以儲存您的實驗。

如果您對實驗的參數感到滿意，並且準備好啟用它，以便開始從您的應用程式收集實驗資料， **請按一下 [啟動]** 。 當實驗處於作用中狀態時，您的應用程式可以將變異變數和報表檢視和轉換事件取出至合作夥伴中心。 如需詳細資訊，請參閱 [在合作夥伴中心中執行及管理您的實驗](manage-your-experiment.md)。

> [!IMPORTANT]
> 一個專案一次僅能包含一個作用中的實驗。 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時選取了 [ **可編輯的實驗** ] 核取方塊。 我們建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>建立內部測試的實驗

您可能想要利用受控制的對象來測試實驗 (例如一組內部測試人員)，並確認所有的變化如預期般運作，然後才將實驗針對客戶啟用。 您可以藉由建立已選取 [ **可編輯] 實驗** 選項的實驗來完成此動作。

若要在發行給客戶之前先測試實驗，請遵循此程序：

1. 建立兩個專案：一個用於 App 的公用組建，另一個用於僅提供給測試對象的 App 私人組建。 以下的指示將這些專案分別稱為公用專案與測試專案。
2. 當您[編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)時，在 App 的公用組建中參照公用專案的專案識別碼。 在 App 的私人組建中，參照測試專案的專案識別碼。
3. 在測試專案中建立實驗，然後選取實驗的 **可編輯實驗** 選項。
4. 在測試專案中啟用實驗。 對某一個變化配置 100% 散佈，並確認此變化針對您的測試者如預期般運作。 針對其他變化重複此程序。
5. 在您確認變化如預期般運作後，在測試專案中對實驗進行任何最後變更。 當您準備好對客戶發行實驗時，將實驗複製到公用專案。 在此實驗中，請勿選取 [ **可編輯的實驗** ] 選項。
4. 請確定複製的實驗中，目標變化的散佈正確無誤。
5. 啟用複製的實驗以向您的客戶發行實驗。

## <a name="next-steps"></a>下一步

在合作夥伴中心中定義您的實驗並在應用程式中編寫實驗的程式碼之後，您就可以開始 [在合作夥伴中心中執行及管理您的實驗](manage-your-experiment.md)。

## <a name="related-topics"></a>相關主題

* [在合作夥伴中心中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的應用程式程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行應用程式實驗](run-app-experiments-with-a-b-testing.md)
