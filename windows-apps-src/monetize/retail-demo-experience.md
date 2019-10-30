---
title: 將零售示範（RDX）功能新增至您的應用程式
description: 準備您的應用程式以進行零售示範模式，協助在零售業銷售樓層展示您的應用程式。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, 零售示範應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 5be39760ee2b8837cfb9b0809a354262e790970b
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2019
ms.locfileid: "73051995"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>將零售示範（RDX）功能新增至您的應用程式

在您的 Windows 應用程式中加入零售示範模式，讓在銷售場所試用電腦和裝置的客戶可以直接開始試用。

當客戶在零售商店時，他們希望能夠試用電腦和裝置的示範。 他們通常會花費相當長的時間，透過[零售示範體驗（RDX）](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)在應用程式中播放。

您可以設定您的應用程式，以在_一般_或_零售_模式中提供不同的體驗。 例如，如果您的應用程式是以安裝程式開始進行，您可以在零售模式中略過它，並使用範例資料和預設設定預先填入應用程式，讓他們可以直接前往。

從客戶的觀點來看，只有一個應用程式。 為了協助客戶區別這兩種模式，建議您在應用程式處於零售模式時，在標題列或適當的位置中，以醒目方式顯示「零售」這個字。

除了應用程式的 Microsoft Store 需求之外，RDX 感知應用程式也必須與 RDX 安裝程式、清除和更新流程相容，以確保客戶在零售商店擁有一致的正面體驗。

## <a name="design-principles"></a>設計原則

* **展現您的最佳效果**。 使用零售示範體驗來展示應用程式 rocks 的原因。 這可能是您的第一次客戶看到您的應用程式，因此請將其顯示為最佳部分！

* **快速顯示**。 客戶可能沒有那麼多耐心 - 能讓使用者越快體驗到您 App 的實際價值越好。

* **讓故事保持簡單**。 零售示範體驗是應用程式價值的電梯。

* **專注于體驗**。 讓使用者有時間消化您的內容。 雖然讓他們快速進入最精彩的部分很重要，但設計適當的停頓可協助他們盡情享受這項體驗。

## <a name="technical-requirements"></a>技術需求

由於 RDX 感知應用程式的目的是要向零售客戶展示您的應用程式，因此他們必須符合技術需求，並遵守 Microsoft Store 針對所有零售示範體驗應用程式所提供的隱私權規定。

這可以用來做為檢查清單，以協助您準備進行驗證程式，並在測試程式中提供清楚的說明。 請注意，您不僅要針對驗證程序維護這些需求，只要您的 App 持續在零售示範裝置上執行，您也必須針對零售示範體驗 App 的整個生命週期維護這些需求。

### <a name="critical-requirements"></a>重要需求

不符合這些重要需求的 RDX 感知應用程式，將會儘快從所有零售示範裝置中移除。

* **不要要求個人識別資訊（PII）** 。 這包括登入資訊、Microsoft 帳戶資訊或連絡人詳細資料。

* **錯誤-免費體驗**。 您的 App 必須在執行時不發生任何錯誤。 此外，不應向使用零售示範裝置的客戶顯示任何錯誤快顯畫面或通知。 錯誤反映了應用程式本身、品牌、裝置品牌、裝置的 manufacturer's 品牌和 Microsoft 品牌的負面影響。

* **付費應用程式必須具有試用模式**。 您的應用程式必須是免費或包含[試用模式](https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)。 客戶不會想要為零售商店中的體驗支付費用。

### <a name="high-priority-requirements"></a>高優先順序需求

不符合這些高優先順序需求的 RDX 感知應用程式，必須立即調查修正程式。 如果找不到立即的修正程式，此 App 便可能從所有的零售示範裝置中被移除。

* **記住離線體驗**。 您的應用程式需要示範絕佳的離線體驗，因為在零售位置上，大約50% 的裝置已離線。 這是要確保與您 App 進行離線互動的客戶仍然能夠享有有意義且正面的體驗。

* **已更新內容體驗**。 在線上時，您的應用程式永遠不會提示更新。 如果需要更新，則應該以無訊息模式執行。

* **無匿名通訊**。 因為使用零售示範裝置的客戶是匿名使用者，所以他們不應該能夠從裝置訊息或共用內容。

* **使用清除程式來提供一致的體驗**。 每個客戶在走近零售示範裝置時都應該有相同的體驗。 您的應用程式應該使用[清除進程](#cleanup-process)，在每次使用之後回到相同的預設狀態。 我們不想讓下一個客戶看到最後一個客戶留下的內容。 這包括計分板、成就及解鎖項目。

* **保留適當的內容**。 所有應用程式內容都必須獲派「青少年」或「較低」的評等類別。 若要深入瞭解，請參閱[讓您的應用程式依 IARC](https://www.globalratings.com/for-developers.aspx)和[ESRB 評等評分](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中優先順序需求

「Windows 零售商店」小組可以直接接觸開發人員，來發起一個有關如何修正這修問題的討論。

* **能夠成功地在各種裝置上執行**。 應用程式必須在所有裝置上順利執行，包括具有低端規格的裝置。 如果應用程式安裝在不符合最小規格的裝置上，應用程式就必須清楚告知使用者這種情況。 最低裝置需求必須公開，以便讓 App 一律能夠以高效能模式執行。

* **符合零售商店應用程式的大小需求**。 App 大小必須小於 800 MB。 如果您的 RDX 感知應用程式不符合大小需求，請直接聯絡 Windows 零售商店小組以進一步討論。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API：準備您的程式碼以進行示範模式

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
[**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo)公用程式類別中的[**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)屬性是 windows 10 SDK 中的[windows. Profile](https://docs.microsoft.com/uwp/api/windows.system.profile)命名空間的一部分，用來做為布林值指標，以指定應用程式執行時所使用的程式碼路徑-_正常_模式或_零售_模式。

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo。屬性

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

```cpp
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

## <a name="cleanup-process"></a>清除進程

當購物者停止與裝置互動之後，就會開始清除兩分鐘。 零售示範會進行播放，而 Windows 會開始重設連絡人、相片和其他應用程式中的任何範例資料。 視裝置而定，這可能需要1-5 分鐘的時間，才能將所有內容全部恢復正常。 這可確保零售商店中的每個客戶都能引導至裝置，並在與裝置互動時擁有相同的體驗。

步驟1：清除
* 所有 Win32 和 Microsoft Store  App 都會被關閉
* 在已知資料夾 (例如 __\[圖片\]__ 、 __\[影片\]__ 、 __\[音樂\]__ 、 __\[文件\]__ 、 __\[已儲存的相片\]__ 、 __\[手機相簿\]__ 、 __\[桌面\]__ 及 __\[下載\]__ 資料夾) 中的所有檔案都會被刪除
* 非結構化和結構化漫遊狀態都會被刪除
* 結構化本機狀態會被刪除

步驟2：設定
* 針對離線裝置：資料夾會維持空白
* 針對線上裝置：可從 Microsoft Store 將零售示範資產推送到裝置

### <a name="store-data-across-user-sessions"></a>跨使用者會話儲存資料

若要跨使用者會話儲存資料，您可以將資訊儲存在__ApplicationData__中，因為預設清除進程不會自動刪除此資料夾中的資料。 請注意，在清除過程中，會刪除使用*LocalState*儲存的資訊。

### <a name="customize-the-cleanup-process"></a>自訂清除進程

若要自訂清除程式，請將 `Microsoft-RetailDemo-Cleanup` app service 部署到您的應用程式中。

需要自訂清除邏輯的情況包括執行廣泛的設定、下載和快取資料，或不想要刪除*LocalState*資料。

步驟1：在您的應用程式資訊清單中宣告_Microsoft RetailDemo-清除_服務。
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

步驟2：使用下列範例範本，在_AppdataCleanup_ case 函式下執行您的自訂清除邏輯。
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

* [儲存和取出應用程式資料](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [如何建立和使用 app service](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [當地語系化應用程式內容](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [零售示範體驗（RDX）](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
