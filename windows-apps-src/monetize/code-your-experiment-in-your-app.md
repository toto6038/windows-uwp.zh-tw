---
Description: 若要在通用 Windows 平台 (UWP) app 中運用 A/B 測試執行實驗，您必須在 App 中編寫實驗的程式碼。
title: 編寫實驗用的應用程式程式碼
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 2e00c3f8d7f1f41d6b44743ebb663b09575fb21a
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334946"
---
# <a name="code-your-app-for-experimentation"></a>編寫實驗用的應用程式程式碼

之後您[建立專案，並在合作夥伴中心中定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)，您已準備好更新您的通用 Windows 平台 (UWP) 應用程式中的程式碼：
* 收到合作夥伴中心遠端變數的值。
* 使用遠端變數為您的使用者設定 App 體驗。
* 事件記錄到合作夥伴中心，指出當使用者擁有檢視您的實驗，而且執行所需的動作 (也稱為*轉換*)。

若要將此行為新增到您的 App 中，您將使用 Microsoft Store Services SDK 所提供的 API。

下列各節會說明取得實驗的變化和事件記錄到合作夥伴中心的一般程序。 您的程式碼應用程式進行測試之後，您可以[定義在合作夥伴中心內的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 如需示範建立及執行實驗的端對端程序的逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

> [!NOTE]
> 有些測試中 Microsoft Store Services SDK 的 Api 會使用[非同步模式](../threading-async/asynchronous-programming-universal-windows-platform-apps.md)從合作夥伴中心擷取資料。 這意謂著這些方法的部分執行可能在方法被呼叫後才發生，因此您 App 的 UI 可以在作業完成時持續回應。 非同步模式會要求您的 App 在呼叫 API 時使用 **async** 關鍵字和 **await** 運算子，如本文的程式碼範例所示範。 根據慣例，非同步方法會以 **Async** 作為結尾。

## <a name="configure-your-project"></a>設定您的專案

若要開始著手，請在您的開發電腦上安裝 Microsoft Store Services SDK，然後將必要的參考新增到您的專案中。

1. [安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]，然後按一下 [加入參考]。
3. 在 \[參考管理員\] 中，展開 \[通用 Windows\]，然後按一下 \[擴充功能\]。
4. 在 SDK 清單中，選取 [Microsoft Engagement Framework] 旁邊的核取方塊，然後按一下 [確定]。

> [!NOTE]
> 本文中的程式碼範例假設您的程式碼檔案有 **System.Threading.Tasks** 和 **Microsoft.Services.Store.Engagement** 命名空間的 **using** 陳述式。

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>取得變化資料並記錄實驗的檢視事件

在專案中，找出您想要在實驗中修改之功能的程式碼。 加入程式碼，擷取的一種變化的資料，使用此資料來修改您要測試，此功能的行為，然後登實驗的檢視事件 A / B 測試在合作夥伴中心內的服務。

您所需的特定程式碼將取決於您的 App，但以下範例將示範基本程序。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

[!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

下列步驟詳細說明此程序的重要部分。

1. 宣告[StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)物件，表示目前的變化指派並[StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger)物件，您將使用記錄檔檢視和轉換合作夥伴中心的事件。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. 宣告一個指派給您所要擷取的實驗之[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)的字串變數。
    > [!NOTE]
    > 取得專案識別碼時您[在合作夥伴中心建立專案](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)。 以下所示的專案識別碼僅供範例用途使用。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. 呼叫靜態的 [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) 方法來取得實驗目前快取的變化指派，然後將實驗的專案識別碼傳遞給此方法。 這個方法會傳回 [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) 物件，此物件會透過 [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation) 屬性提供對變化指派的存取權。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. 檢查 [IsStale](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) 屬性，以判斷是否需要使用來自伺服器的遠端變化指派來重新整理快取的變化指派。 如果需要重新整理，請呼叫靜態的 [GetRefreshedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) 方法，以檢查是否有來自伺服器的更新變化指派，並重新整理本機快取變化。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. 使用 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 物件的 [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean)、[GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble)、[GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) 或 [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 方法，取得變化指派的值。 在每個方法中，第一個參數是您想要擷取的變數名稱 （這是相同的名稱，為您輸入在合作夥伴中心內的變化）。 第二個參數是預設值，這個方法應傳回，如果您不能擷取合作夥伴中心中的指定的值，（例如，如果沒有網路連線），並沒有變化快取的版本。

    下列範例使用 [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 取得名為 *buttonText* 的變數，並指定 **Grey Button** 的預設變數值。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. 在您的程式碼中，使用變數值來修改您要測試之功能的行為。 例如，下列程式碼會將 *buttonText* 值指派給您 App 中某個按鈕的內容。 此範例假設您已經在專案中的其他地方定義這個按鈕。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. 最後，登[檢視事件](run-app-experiments-with-a-b-testing.md#terms)實驗到 A / B 測試在合作夥伴中心內的服務。 請將 ```logger``` 欄位初始化為 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 物件，並呼叫 [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 方法。 傳遞[StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)物件，表示目前的變化指派 （此物件會提供到合作夥伴中心事件相關的內容） 和您的實驗的檢視事件的名稱。 這必須符合您輸入您的實驗，在合作夥伴中心內檢視事件名稱。 您的程式碼應該在使用者開始檢視屬於實驗一部分的變化時記錄檢視事件。

    下列範例說明如何記錄名為 **userViewedButton** 的檢視事件。 在此範例中，實驗的目標是讓使用者按一下 App 中的按鈕，以便在 App 擷取到變化資料 (在此案例中為按鈕文字) 之後記錄檢視事件，然後將它指派給按鈕的內容。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>轉換事件記錄到合作夥伴中心

接下來，新增 記錄檔的程式碼[轉換事件](run-app-experiments-with-a-b-testing.md#terms)到 A / B 測試在合作夥伴中心內的服務。 您的程式碼應該在使用者達到實驗目標時記錄轉換事件。 您所需的特定程式碼將取決於您的 App，但以下是一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在當使用者達到其中一個實驗目標的目標時會執行的程式碼中，再次呼叫 [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 方法，並傳遞 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 物件以及實驗的轉換事件名稱。 這必須符合其中一個您輸入您的實驗，在合作夥伴中心轉換事件名稱。

    下列範例會記錄來自按鈕之 **Click** 事件處理常式且名為 **userClickedButton** 的轉換事件。 在此範例中，實驗的目標是讓使用者按一下按鈕。

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>後續步驟

在您的 App 中編寫實驗程式碼之後，您就準備好進行下列步驟︰
1. [在合作夥伴中心內定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 建立定義檢視事件、轉換事件以及 A/B 測試唯一變化的實驗。
2. [執行和管理您的實驗，在合作夥伴中心](manage-your-experiment.md)。


## <a name="related-topics"></a>相關主題

* [建立專案，並在合作夥伴中心中定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [在合作夥伴中心內定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [管理您的實驗，在合作夥伴中心](manage-your-experiment.md)
* [建立並執行您的第一個實驗以 A / B 測試](create-and-run-your-first-experiment-with-a-b-testing.md)
* [應用程式的實驗執行 A / B 測試](run-app-experiments-with-a-b-testing.md)
