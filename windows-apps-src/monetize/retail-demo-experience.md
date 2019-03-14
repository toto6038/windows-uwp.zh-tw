---
title: 將零售示範 (RDX) 功能加入至您的應用程式
description: 零售示範模式中，可協助您展現您的應用程式，在零售銷售前準備您的應用程式。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, 零售示範應用程式
ms.localizationpriority: medium
ms.openlocfilehash: b66435dd7c94762874461b48e19e9a60224f287b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596753"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>將零售示範 (RDX) 功能加入至您的應用程式

讓客戶試用電腦和裝置在銷售的前可以直接進入正題，請在 您的 Windows 應用程式中包含零售示範模式。

當客戶在零售商店，他們預期能夠試用示範的電腦和裝置。 它們通常會耗費相當長的時間試用應用程式透過區塊[零售示範體驗 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)。

您可以設定您的應用程式以提供同時在不同的體驗_正常_或是_零售_模式。 比方說，如果您的應用程式的安裝程序，您可以在零售模式中，跳過它，並預先填入範例資料和預設設定的應用程式，讓它們可以直接進入正題。

從客戶觀點來看，沒有一個應用程式。 為了協助區別兩種模式的客戶，我們建議零售模式在您的應用程式時，它會顯示文字 「 零售 」 以突顯的方式標題列中，或在適當的位置。

除了 Microsoft Store 應用程式的需求，RDX 感知應用程式也必須相容於 RDX 安裝程式、 清理及更新程序，以確保客戶擁有一致地正面的體驗，在零售商店。

## <a name="design-principles"></a>設計原則

* **顯示您的最佳**。 您可以使用 零售示範體驗來展示您的應用程式在哪裡。 這是可能在第一次您的客戶會看到您的應用程式，因此顯示它們的最佳部分 ！

* **顯示快速**。 客戶可能沒有那麼多耐心 - 能讓使用者越快體驗到您 App 的實際價值越好。

* **簡化的劇本**。 零售示範體驗是針對您的應用程式值的電梯簡報。

* **專注於經驗**。 讓使用者有時間消化您的內容。 雖然讓他們快速進入最精彩的部分很重要，但設計適當的停頓可協助他們盡情享受這項體驗。

## <a name="technical-requirements"></a>技術需求

這是 RDX 感知應用程式的目的來展示您的應用程式，零售客戶的最佳，必須符合技術需求，並遵循 Microsoft Store 已針對所有零售示範體驗應用程式的隱私權法規。

這可用來當做一份檢查清單可協助您做好驗證程序，並提供清楚的測試程序。 請注意，您不僅要針對驗證程序維護這些需求，只要您的 App 持續在零售示範裝置上執行，您也必須針對零售示範體驗 App 的整個生命週期維護這些需求。

### <a name="critical-requirements"></a>重要的需求

將從所有零售示範裝置儘速移除 RDX 感知的應用程式不符合這些重要的需求。

* **不要再問個人識別資訊 (PII)**。 這包括登入資訊、 Microsoft 帳戶資訊或連絡詳細資料。

* **無錯誤體驗**。 您的 App 必須在執行時不發生任何錯誤。 此外，不應向使用零售示範裝置的客戶顯示任何錯誤快顯畫面或通知。 錯誤反思造成負面的應用程式本身、 品牌、 裝置的品牌、 裝置製造商的品牌和 Microsoft 的品牌。

* **付費應用程式必須有試用模式**。 您的應用程式可能需要一套免費或包含[試用版模式](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)。 客戶不會想要為零售商店中的體驗支付費用。

### <a name="high-priority-requirements"></a>高優先順序的需求

不符合這些優先需求的 RDX 感知應用程式需要立即調查的修正程式。 如果找不到立即的修正程式，此 App 便可能從所有的零售示範裝置中被移除。

* **令人印象深刻的離線體驗**。 您的應用程式必須示範很棒的離線體驗，因為在零售地點約 50%的裝置已離線。 這是要確保與您 App 進行離線互動的客戶仍然能夠享有有意義且正面的體驗。

* **更新內容體驗**。 您的應用程式應該永遠不會提示在線上的更新。 如果需要更新，它們應該執行無訊息模式。

* **沒有匿名通訊**。 由於客戶使用零售示範裝置是匿名的使用者，他們不應該能夠從裝置訊息或共用的內容。

* **使用清除程序提供一致的體驗**。 每個客戶在走近零售示範裝置時都應該有相同的體驗。 您的應用程式應該使用[清理程序](#cleanup-process)每次使用之後，返回相同的預設狀態。 我們不想下一步 以查看在最後一個客戶留下客戶。 這包括計分板、成就及解鎖項目。

* **適當內容的保留時間**。 所有應用程式內容必須是指派給青少年或較低的評等類別。 若要進一步了解，請參閱[取得您的應用程式評等的 IARC](https://www.globalratings.com/for-developers.aspx)並[ESRB 分級](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中等優先順序的需求

「Windows 零售商店」小組可以直接接觸開發人員，來發起一個有關如何修正這修問題的討論。

* **能夠透過各種裝置已成功執行**。 應用程式必須在所有裝置，包括具有低階規格上順利執行。 如果不符合最小規格的裝置上安裝應用程式，就必須清楚地告知使用者這應用程式。 最低裝置需求必須公開，以便讓 App 一律能夠以高效能模式執行。

* **符合零售商店的應用程式大小需求**。 App 大小必須小於 800 MB。 請連絡 Windows 零售商店小組直接的進一步討論，如果您 RDX 感知的應用程式不符合大小需求。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API:準備您的程式碼示範模式

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
[ **IsDemoModeEnabled** ](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)中的屬性[ **RetailInfo** ](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo)公用程式類別，這是組件的[Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile)命名空間，在 Windows 10 SDK 中，為的布林值指標可用來指定應用程式執行所在-哪一個程式碼路徑_正常_模式或_零售_模式。

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

## <a name="cleanup-process"></a>清除程序

清除開始兩分鐘之後是購物者就會停止與裝置互動。 零售示範播放時，Windows 會開始重設的連絡人、 相片和其他應用程式中的任何範例資料。 視裝置而定，這可能需要 1-5 分鐘，才能完全回到正常重設所有項目。 這可確保在零售商店中的每個客戶可以靠著裝置並與裝置互動時，有相同的體驗。

步驟 1：Cleanup
* 所有 Win32 和市集 App 都會被關閉
* 在已知資料夾 (例如 \[圖片\]、\[影片\]、\[音樂\]、\[文件\]、\[已儲存的相片\]、\[手機相簿\]、\[桌面\] 及 \[下載\] 資料夾) 中的所有檔案都會被刪除 
* 非結構化和結構化漫遊狀態都會被刪除
* 結構化本機狀態會被刪除

步驟 2：設定
* 適用於離線的裝置：資料夾維持空白
* 適用於線上的裝置：零售示範資產可以推送至裝置，從 Microsoft Store

### <a name="store-data-across-user-sessions"></a>在使用者工作階段之間儲存資料

若要將資料儲存在使用者工作階段之間，您可以儲存中的資訊__ApplicationData.Current.TemporaryFolder__預設值為清除程序不會自動刪除此資料夾中的資料。 請注意儲存使用該資訊*LocalState*清理程序期間會刪除。

### <a name="customize-the-cleanup-process"></a>自訂清除程序

若要自訂清除程序，實作`Microsoft-RetailDemo-Cleanup`到您的應用程式的應用程式服務。

案例需要自訂的清除邏輯，其中包括執行大量的設定，下載並快取資料，或不想*LocalState*要刪除的資料。

步驟 1：宣告_Microsoft RetailDemo 清除_服務在您的應用程式資訊清單。
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

步驟 2：實作自訂的清除邏輯下_AppdataCleanup_ case 函式使用以下範例範本。
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

* [儲存和擷取應用程式資料](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [如何建立和使用應用程式服務](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [當地語系化應用程式內容](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [零售示範體驗 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
