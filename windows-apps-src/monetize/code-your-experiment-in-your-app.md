---
author: Xansky
Description: To run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must code the experiment in your app.
title: 編寫實驗用的 app 程式碼
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 21c6eab0e021f703c8103462a8459ea190693a10
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614064"
---
# <a name="code-your-app-for-experimentation"></a>編寫實驗用的 app 程式碼

在您[在開發人員中心儀表板中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)之後，就可以準備更新通用 Windows 平台 (UWP) app 中的程式碼，以：
* 從 Windows 開發人員中心收到遠端變數值。
* 使用遠端變數為您的使用者設定 App 體驗。
* 將指出使用者何時檢視實驗並執行所需動作 (也稱為「轉換」) 的事件記錄至「開發人員中心」。

若要將此行為新增到您的 App 中，您將使用 Microsoft Store Services SDK 所提供的 API。

下列小節描述取得實驗變化及將事件記錄至「開發人員中心」的一般程序。 編寫實驗用的 App 程式碼之後，就可以[在開發人員中心儀表板中定義實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 如需示範建立及執行實驗的端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

> [!NOTE]
> 某些編寫實驗用 Microsoft Store Services SDK 中的 Api 來擷取資料從開發人員中心使用[非同步模式](../threading-async/asynchronous-programming-universal-windows-platform-apps.md)。 這意謂著這些方法的部分執行可能在方法被呼叫後才發生，因此您 App 的 UI 可以在作業完成時持續回應。 非同步模式會要求您的 App 在呼叫 API 時使用 **async** 關鍵字和 **await** 運算子，如本文的程式碼範例所示範。 根據慣例，非同步方法會以 **Async** 作為結尾。

## <a name="configure-your-project"></a>設定您的專案

若要開始著手，請在您的開發電腦上安裝 Microsoft Store Services SDK，然後將必要的參考新增到您的專案中。

1. [安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]，然後按一下 [加入參考]。
3. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。
4. 在 SDK 清單中，選取 **\[Microsoft Engagement Framework\]** 旁邊的核取方塊，然後按一下 **\[確定\]**。

> [!NOTE]
> 這篇文章中的程式碼範例假設您的程式碼檔案有**使用** **System.Threading.Tasks**和**Windows.applicationmodel.activation**命名空間的陳述式。

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>取得變化資料並記錄實驗的檢視事件

在專案中，找出您想要在實驗中修改之功能的程式碼。 新增會擷取變化資料的程式碼、使用此資料來修改您要測試之功能的行為，然後將您實驗的檢視事件記錄至「開發人員中心」中的 A/B 測試服務。

您所需的特定程式碼將取決於您的 App，但以下範例將示範基本程序。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

下列步驟詳細說明此程序的重要部分。

1. 宣告一個代表目前變化指派的 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 物件，以及一個您將用來把檢視和轉換事件記錄至「開發人員中心」的 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 物件。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. 宣告一個指派給您所要擷取的實驗之[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)的字串變數。
    > [!NOTE]
    > 取得一個專案識別碼，當您[建立在開發人員中心儀表板中的專案](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)。 以下所示的專案識別碼僅供範例用途使用。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. 呼叫靜態的 [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) 方法來取得實驗目前快取的變化指派，然後將實驗的專案識別碼傳遞給此方法。 這個方法會傳回 [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) 物件，此物件會透過 [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation) 屬性提供對變化指派的存取權。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. 檢查 [IsStale](htthttps://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) 屬性，以判斷是否需要使用來自伺服器的遠端變化指派來重新整理快取的變化指派。 如果需要重新整理，請呼叫靜態的 [GetRefreshedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) 方法，以檢查是否有來自伺服器的更新變化指派，並重新整理本機快取變化。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. 使用 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 物件的 [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean)、[GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble)、[GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) 或 [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 方法，取得變化指派的值。 在每個方法中，第一個參數是您想要擷取的變化名稱 (與您在開發人員中心儀表板中輸入的變化名稱相同)。 第二個參數是如果方法無法從開發人員中心擷取指定值 (例如，如果沒有網路連線)，而且變化的快取版本無法使用，方法應傳回的預設值。

    下列範例使用 [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 取得名為 *buttonText* 的變數，並指定 **Grey Button** 的預設變數值。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. 在您的程式碼中，使用變數值來修改您要測試之功能的行為。 例如，下列程式碼會將 *buttonText* 值指派給您 App 中某個按鈕的內容。 此範例假設您已經在專案中的其他地方定義這個按鈕。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. 最後，將實驗的[檢視事件](run-app-experiments-with-a-b-testing.md#terms)記錄至「開發人員中心」的 A/B 測試服務。 請將 ```logger``` 欄位初始化為 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 物件，並呼叫 [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 方法。 傳遞代表目前變化指派的 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 物件 (此物件會將事件的相關內容提供給「開發人員中心」) 以及實驗的檢視事件名稱。 這必須符合您在「開發人員中心」儀表板中為實驗輸入的檢視事件名稱。 您的程式碼應該在使用者開始檢視屬於實驗一部分的變化時記錄檢視事件。

    下列範例說明如何記錄名為 **userViewedButton** 的檢視事件。 在此範例中，實驗的目標是讓使用者按一下 App 中的按鈕，以便在 App 擷取到變化資料 (在此案例中為按鈕文字) 之後記錄檢視事件，然後將它指派給按鈕的內容。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-dev-center"></a>將轉換事件記錄至開發人員中心

接下來，新增會將[轉換事件](run-app-experiments-with-a-b-testing.md#terms)記錄至「開發人員中心」中 A/B 測試服務的程式碼。 您的程式碼應該在使用者達到實驗目標時記錄轉換事件。 您所需的特定程式碼將取決於您的 App，但以下是一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在當使用者達到其中一個實驗目標的目標時會執行的程式碼中，再次呼叫 [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 方法，並傳遞 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 物件以及實驗的轉換事件名稱。 這必須符合您在「開發人員中心」儀表板中為實驗輸入的其中一個轉換事件名稱。

    下列範例會記錄來自按鈕之 **Click** 事件處理常式且名為 **userClickedButton** 的轉換事件。 在此範例中，實驗的目標是讓使用者按一下按鈕。

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>後續步驟

在您的 App 中編寫實驗程式碼之後，您就準備好進行下列步驟︰
1. [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 建立定義檢視事件、轉換事件以及 A/B 測試唯一變化的實驗。
2. [在開發人員中心儀表板中執行和管理您的實驗](manage-your-experiment.md)。


## <a name="related-topics"></a>相關主題

* [在開發人員中心儀表板中建立專案與定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)
