---
author: joannaleecy
title: 建立零售示範體驗應用程式
description: 建立「零售示範體驗」(RDX) 應用程式，一種在零售示範模式及一般模式下都能啟動的單一應用程式
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 零售示範應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 19a22e09484943d63988cef6bb6a7e7c09e016dd
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691014"
---
#  <a name="create-a-retail-demo-experience-rdx-app"></a>建立零售示範體驗 (RDX) 應用程式

當客戶走進零售商店或地點時，他們會預期有最新的電腦和行動電話展示，而這些展示的裝置即稱為零售示範裝置。
零售示範裝置和安裝在這些裝置上的內容負責大部分的商店客戶體驗，因為客戶通常會花費相當多的時間把玩這些裝置。

安裝在零售商店中這些電腦和行動電話上的 App 必須是零售示範體驗 (RDX) App。 本文概述如何設計和開發要安裝在零售商店中電腦和行動示範裝置上的零售示範版 App。

零售示範體驗 App 是採用單一組建的形式，可以用_一般_或_零售_這兩種不同的模式之一來啟動。
從我們客戶的觀點來看，只有一個 App，而為了協助我們的客戶區分這兩種版本，建議您讓 App 在以零售模式執行時，在標題列或適當的位置明顯地顯示「零售」字樣。

除了適用於 App 的「Microsoft Store」需求之外，RDX App 還必須與零售示範裝置設定、清理及更新系統完全相容，才能確保客戶在零售商店有一致的正面體驗。

## <a name="design-principles"></a>設計原則

### <a name="show-your-best"></a>展現最好的一面

使用零售示範體驗來展示您的應用程式為何出色。  這有可能是客戶第一次看到您的應用程式，因此請將最好的一面展現出來！

### <a name="show-it-fast"></a>展示速度

客戶可能沒有那麼多耐心 - 能讓使用者越快體驗到您 App 的實際價值越好。

### <a name="keep-the-story-simple"></a>簡化說明

請記住，零售示範體驗是您 App 價值的一種電梯簡報手法。

### <a name="focus-on-the-experience"></a>將焦點放在體驗上

讓使用者有時間消化您的內容。  雖然讓他們快速進入最精彩的部分很重要，但設計適當的停頓可協助他們盡情享受這項體驗。

## <a name="technical-requirements"></a>技術需求

由於零售示範體驗 App 的目的是要將您 App 的最好一面展示給零售客戶，因此 App 必須符合下列技術需求，並遵守「Microsoft Store」的所有零售示範體驗 App 隱私權規定。
這也可以用來作為檢查清單以協助您為驗證程序做準備，以及讓測試程序更為透明。 請注意，您不僅要針對驗證程序維護這些需求，只要您的 App 持續在零售示範裝置上執行，您也必須針對零售示範體驗 App 的整個生命週期維護這些需求。

### <a name="critical-level-requirements"></a>重要層級需求

不符合這些重要需求的 RDX App 將被儘快從所有零售示範裝置中移除。

* 不要求輸入個人識別資訊 (PII)

    App 不被允許向客戶要求輸入任何個人識別資訊。  這包括 Microsoft 帳戶資訊、連絡人詳細資料等。

* 無錯誤的體驗

    您的 App 必須在執行時不發生任何錯誤。 此外，不應向使用零售示範裝置的客戶顯示任何錯誤快顯畫面或通知。 這相當重要，因為我們想要向客戶展示最好的一面，而最好的一面應該沒有任何錯誤。
    另一個原因則是錯誤對 App 本身、您的品牌、App 執行所在的裝置、裝置的製造商品牌以及 Microsoft 品牌來說，是一種負面反映。

* 付費 App 必須提供試用模式

    為了讓 App 能被安裝在零售示範裝置上，您的 App 必須是免費 App 或提供已確立的「試用」模式。  客戶不會想要為零售商店中的體驗支付費用。 如需詳細資訊，請參閱[在試用版中排除或限制某些功能](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)

### <a name="high-priority-requirements"></a>高優先順序需求

針對不符合這些高優先順序需求的 RDX App，您必須進行調查來立即修正。 如果找不到立即的修正程式，此 App 便可能從所有的零售示範裝置中被移除。

* 令人印象深刻的離線體驗

    您的零售示範體驗 App 必須示範絕佳的離線體驗，因為有大約 50% 的裝置在零售地點都是處於離線狀態。 這是要確保與您 App 進行離線互動的客戶仍然能夠享有有意義且正面的體驗。

* 更新內容體驗

    若要提供絕佳的體驗，您的 App 必須隨時保持在最新狀態，並且客戶不應在您 App 處於線上時看見應用程式更新提示。

* 無匿名通訊

    由於使用零售示範裝置的客戶是匿名使用者，因此他們不應該能夠從該裝置傳送訊息或共用內容。

* 利用清理程序提供一致的體驗

    每個客戶在走近零售示範裝置時都應該有相同的體驗。 您的 App 應該利用[清理程序](#clean-up-process)在每次使用後回到相同的預設狀態，因為我們不希望下一個客戶看到上一個客戶留下的東西。  這包括計分板、成就及解鎖項目。

* 年齡適當的內容

    所有零售示範體驗 App 內容都必須被指派 13 歲以上或分級層級更低的類別。 如需詳細資訊，請參閱[由 IARC 為您的 App 分級](https://www.globalratings.com/for-developers.aspx)和 [ESRB 分級](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中優先順序需求

「Windows 零售商店」小組可以直接接觸開發人員，來發起一個有關如何修正這修問題的討論。

* 能夠在一系列的裝置上成功執行

    零售示範體驗 App 必須在所有裝置上都順暢執行，包括低階規格的裝置。 如果零售示範體驗 App 是安裝在不符合執行該 App 之最低規格的裝置上，該 App 必須清楚將此資訊告知使用者。 最低裝置需求必須公開，以便讓 App 一律能夠以高效能模式執行。

* 符合零售商店 App 大小需求

    App 大小必須小於 800 MB。 如果您的零售示範體驗 App 不符合大小需求，請直接連絡「Windows 零售商店」小組來進行進一步的討論。

## <a name="preparing-codebase-for-retail-demo-mode-development"></a>準備零售示範模式開發的程式碼基底

[**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) 公用程式類別中的 [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 屬性是 Windows 10 SDK 中 [Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) 命名空間中的一部分，可用來作為布林值指標，以指定您的應用程式在哪個程式碼路徑上執行 - _「一般」_ 模式或 _「零售」_ 模式。

當 [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 傳回 true 時，您可以使用 [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) 來查詢裝置相關的一組屬性，以建置一個自訂程度更高的零售示範體驗。 這些屬性包括 [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername)、[**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize)、[**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 等。


## <a name="clean-up-process"></a>清理程序

清理程序可用來在零售示範裝置於固定的持續時間內沒有任何互動時，自動將該裝置重設回原始預設設定。 這是要確保零售商店中的每位使用者都可以在走近裝置並與其互動時，獲得完全相同預設所要提供的體驗。 開發零售示範體驗 App 時，請務必了解何時及如何觸發清理程序、進行預設清除程序期間會發生什麼情況，以及了解如何根據您想要提供的零售示範體驗來自訂這個清除程序。

### <a name="when-does-clean-up-begin"></a>何時開始清理？

清理序列會在達到一段特定的裝置閒置時間之後開始進行。 閒置時間是從沒有任何來自裝置上觸控、滑鼠及鍵盤的輸入時開始計算。

#### <a name="desktoppc"></a>桌上型電腦/電腦

在閒置時間達 120 秒之後，閒置吸引 App 影片會開始在裝置上播放。 5 秒之後，清理程序會開始運作。

#### <a name="phone"></a>手機

在閒置時間達 60 秒之後，閒置吸引 App 影片會開始在裝置上播放，而且清理程序會立即開始運作。

### <a name="what-happens-during-a-default-clean-up-process"></a>在進行預設清理程序期間會發生什麼情況？

#### <a name="step-1-clean-up"></a>步驟 1：清理。
* 所有 Win32 和 Microsoft Store  App 都會被關閉
* 在已知資料夾 (例如 __\[圖片\]__、__\[影片\]__、__\[音樂\]__、__\[文件\]__、__\[已儲存的相片\]__、__\[手機相簿\]__、__\[桌面\]__ 及 __\[下載\]__ 資料夾) 中的所有檔案都會被刪除
* 非結構化和結構化漫遊狀態都會被刪除
* 結構化本機狀態會被刪除

#### <a name="step-2-set-up"></a>步驟 2：設定
* 針對離線裝置：資料夾會維持空白
* 針對線上裝置：可從 Microsoft Store 將零售示範資產推送到裝置

### <a name="how-to-store-data-across-user-sessions"></a>如何跨使用者工作階段儲存資料？

如果您想要跨使用者工作階段儲存資料，您可以將資訊儲存在 __ApplicationData.Current.TemporaryFolder__ 中，因為預設清理程序不會自動刪除此資料夾中的資料。 請注意，在進行清理程序期間，會刪除使用 *LocalState* 來儲存的資訊。

### <a name="how-to-customize-the-clean-up-process"></a>如何自訂清理程序？

如果您想要自訂清理程序，您將必須在您的 App 中實作 `Microsoft-RetailDemo-Cleanup` App 服務。

需要有自訂清理邏輯的案例包括執行耗費大量資源的設定、下載和快取資料，或是不想讓 *LocalState* 資料被刪除。

步驟 1：在您的應用程式資訊清單中宣告 _Microsoft-RetailDemo-Cleanup_ 服務。
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

步驟 2：使用下面的範例範本，在 _AppdataCleanup_ case 函式底下實作您的自訂清理邏輯。
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>相關連結

* [儲存和擷取 App 資料](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [如何建立和使用 App 服務](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [將 App 當地語系化](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)


 

 
