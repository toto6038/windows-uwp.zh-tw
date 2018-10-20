---
author: joannaleecy
title: 將零售示範 (RDX) 功能新增到您的應用程式
description: 準備零售示範模式，可協助展示您的應用程式在零售銷售地板上您的應用程式。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 零售示範應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 152c775c1b69bfd82d8969aed7e638f98646bdd7
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "5165621"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>將零售示範 (RDX) 功能新增到您的應用程式

Windows 應用程式中包括零售示範模式，讓客戶試用電腦和裝置上銷售地板可以跳中的權限。

客戶在零售商店中時，他們會預期要能夠試用示範的電腦和裝置。 它們通常會花費相當把玩其玩透過[零售示範體驗 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)應用程式的時間。

您可以提供不同的體驗，在_正常_或_零售_模式中設定您的應用程式。 例如，如果您的應用程式啟動時進行安裝程序，可能會跳過它在零售模式中，和預先填入之 app 的範例資料和預設設定，讓他們可以跳中的權限。

從我們客戶的觀點來看，則只有一個應用程式。 為了協助客戶區分兩種模式，我們建議，當您的應用程式處於零售模式，它會顯示為文字 「 零售 」 字樣在標題列或適當的位置。

除了應用程式的 Microsoft Store 需求，RDX 感知應用程式都必須也相容於 RDX 安裝、 清理及更新的程序，以確保客戶在零售商店有一致的正面體驗。

## <a name="design-principles"></a>設計原則

* **展現最好的一面**。 使用零售示範體驗來展示您的應用程式為何出色。 這有可能是第一次您的客戶將會看到您的應用程式，因此最好的一面它們出來 ！

* **顯示它快速**。 客戶可能沒有那麼多耐心 - 能讓使用者越快體驗到您 App 的實際價值越好。

* **簡化說明**。 零售示範體驗是針對您的應用程式值的升降舵上下移動。

* **在體驗上的焦點**。 讓使用者有時間消化您的內容。 雖然讓他們快速進入最精彩的部分很重要，但設計適當的停頓可協助他們盡情享受這項體驗。

## <a name="technical-requirements"></a>技術需求

RDX 感知 app 的目的是要展示給零售客戶的應用程式的最佳，因為它們必須符合技術的需求，並遵守 Microsoft Store 」 的所有零售示範體驗 app 隱私權規定。

這可以用做為一份檢查清單，來協助您準備進行驗證程序，並讓測試程序更為透明。 請注意，您不僅要針對驗證程序維護這些需求，只要您的 App 持續在零售示範裝置上執行，您也必須針對零售示範體驗 App 的整個生命週期維護這些需求。

### <a name="critical-requirements"></a>重要需求

將從所有零售示範裝置儘速移除不符合這些重要需求的 RDX 感知應用程式。

* **不要尋求個人識別資訊 (PII)**。 這包括登入資訊、 Microsoft 帳戶資訊或連絡人詳細資料。

* **無錯誤的體驗**。 您的 App 必須在執行時不發生任何錯誤。 此外，不應向使用零售示範裝置的客戶顯示任何錯誤快顯畫面或通知。 錯誤負面反映在應用程式本身、 您的品牌、 裝置的品牌、 裝置的製造商品牌以及 microsoft 品牌。

* **付費應用程式必須提供試用模式**。 您的 app 必須是免費或包含[試用模式](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)。 客戶不會想要為零售商店中的體驗支付費用。

### <a name="high-priority-requirements"></a>高優先順序需求

不符合這些高優先順序需求的 RDX 感知應用程式需要立即調查的修正程式。 如果找不到立即的修正程式，此 App 便可能從所有的零售示範裝置中被移除。

* **Memorable 離線體驗**。 您的 app 必須示範絕佳的離線體驗，因為有大約 50%的裝置在零售地點都離線。 這是要確保與您 App 進行離線互動的客戶仍然能夠享有有意義且正面的體驗。

* **已更新內容體驗**。 您的應用程式應該永遠不會進行更新時線上提示。 如果需要更新，它們應該執行以無訊息方式。

* **無匿名通訊**。 使用零售示範裝置的客戶是匿名使用者，因為他們不應該能夠從裝置訊息或共用內容。

* **提供一致的體驗，利用清理程序**。 每個客戶在走近零售示範裝置時都應該有相同的體驗。 您的應用程式應該使用[清理程序](#clean-up-process)來在每次使用後回到相同的預設狀態。 我們不希望下一個客戶看到什麼最後一個客戶留下的東西。 這包括計分板、成就及解鎖項目。

* **年齡適當的內容**。 所有應用程式內容都必須被指派 13 歲以上或較低的評等類別。 若要了解更多資訊，請參閱[由 IARC 分級您的應用程式的取得](https://www.globalratings.com/for-developers.aspx)和[ESRB 分級](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中優先順序需求

「Windows 零售商店」小組可以直接接觸開發人員，來發起一個有關如何修正這修問題的討論。

* **能夠在各種裝置上成功執行**。 應用程式必須可以在所有裝置，包括低階規格的裝置上執行。 如果不符合最低規格的裝置上安裝應用程式，app 必須清楚此資訊告知使用者。 最低裝置需求必須公開，以便讓 App 一律能夠以高效能模式執行。

* **符合零售商店 app 大小需求**。 App 大小必須小於 800 MB。 如果您 RDX 感知的應用程式不符合大小需求，請連絡 「 Windows 零售商店 」 小組直接來進一步的討論。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>屬性 API： 準備您的程式碼示範模式

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
在[**屬性**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo)公用程式類別中，也就是 Windows 10 SDK 中的[Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile)命名空間的一部分， [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)屬性來作為布林值指標指定您應用程式-執行的程式碼路徑_法向量_模式或_零售_模式。

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>RetailInfo.Properties

當 [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 傳回 true 時，您可以使用 [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) 來查詢裝置相關的一組屬性，以建置一個自訂程度更高的零售示範體驗。 這些屬性包括 [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername)、[**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize)、[**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 等。

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>清理程序

清理開始兩分鐘後購物停止與裝置互動。 零售示範播放時，以及 Windows 會開始重設任何連絡人、 相片和其他應用程式中的範例資料。 根據裝置，這可能需要花費 1-5 分鐘的時間來完全將所有項目重設回一般之間。 這樣可確保零售商店中的每個客戶可以走近裝置並與裝置互動時，有相同的體驗。

步驟 1： 清理
* 所有 Win32 和 Microsoft Store  App 都會被關閉
* 在已知資料夾 (例如 __\[圖片\]__、__\[影片\]__、__\[音樂\]__、__\[文件\]__、__\[已儲存的相片\]__、__\[手機相簿\]__、__\[桌面\]__ 及 __\[下載\]__ 資料夾) 中的所有檔案都會被刪除
* 非結構化和結構化漫遊狀態都會被刪除
* 結構化本機狀態會被刪除

步驟 2： 設定
* 針對離線裝置：資料夾會維持空白
* 針對線上裝置：可從 Microsoft Store 將零售示範資產推送到裝置

### <a name="store-data-across-user-sessions"></a>跨使用者工作階段儲存資料

若要跨使用者工作階段儲存資料，您可以在__Applicationdata.current.temporaryfolder\__儲存資訊，因為預設清理程序不會自動刪除此資料夾中的資料。 請注意，清理程序期間，會刪除使用*LocalState*儲存的資訊。

### <a name="customize-the-cleanup-process"></a>自訂清理程序

若要自訂清理程序，實作`Microsoft-RetailDemo-Cleanup`到您的應用程式的應用程式服務。

需要自訂清理邏輯的案例包括執行大量的設定、 下載和快取資料，或是不想讓*LocalState*資料被刪除。

步驟 1： 宣告您的應用程式資訊清單中的_Microsoft-RetailDemo-Cleanup_服務。
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

步驟 2： 實作您的自訂清理邏輯_AppdataCleanup_案例函式使用下面的範例範本底下。
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
* [零售示範體驗 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
