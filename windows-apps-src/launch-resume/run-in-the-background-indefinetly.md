---
author: TylerMSFT
title: 在背景無限期執行
description: 使用 extendedExecutionUnconstrained 功能，在背景無限期執行背景工作或延伸執行工作階段。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 背景工作，延伸執行，資源，限制，背景工作
ms.author: twhitney
ms.date: 10/3/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb6e0735620c4a940d3414f22aaa4f09bb608424
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5704085"
---
# <a name="run-in-the-background-indefinitely"></a>在背景無限期執行

為了提供最佳體驗給使用者，Windows 對通用 Windows 平台 (UWP) app 的資源加以限制。 系統會提供前景應用程式最多記憶體和執行時間，而背景應用程式得到的則較少。 這樣就能保護使用者避免前景應用程式效能不彰和電池電力嚴重流失。

不過，撰寫 UWP app 供個人使用的開發人員 (也就是，側載不會在 Microsoft Store 中發佈的應用程式) 或撰寫企業 UWP app 的開發人員，可能會想要在沒有任何背景或延伸執行節流的情況下使用裝置上所有可用的資源。 企業營運及個人 UWP 應用程式可以使用 Windows Creators Update (版本 1703) 中的 API 來關閉節流。 請注意，如果應用程式使用這些 API，就無法進入 Microsoft Store。

## <a name="run-while-minimized"></a>在最小化時執行

UWP app 不在前景執行時，會進入暫停狀態。 在桌面上，當使用者將應用程式最小化時，就會發生這種情況。 應用程式會使用延伸執行工作階段，以便在最小化時繼續執行。 Microsoft Store 接受的延伸執行 API 在[透過延伸執行延後應用程式暫停](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)中有詳細說明。

如果您在開發不打算提交到 Microsoft Store 的應用程式，則可以使用 `extendedExecutionUnconstrained` 功能受限的 [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession)，讓應用程式可以在最小化時繼續執行，而不考慮裝置的能源狀態。  

`extendedExecutionUnconstrained` 功能在應用程式資訊清單中會當做受限功能加入。 如需受限功能的詳細資訊，請參閱[應用程式功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

_Package.appxmanifest_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

當您使用 `extendedExecutionUnconstrained` 功能時，使用的會是 [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 和 [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason)，而不是 [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) 和 [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason)。 建立工作階段、設定成員以及非同步要求延伸的同樣模式仍然適用： 

```cs
var newSession = new ExtendedExecutionForegroundSession();  
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;  
newSession.Description = "Long Running Processing";  
newSession.Revoked += SessionRevoked;  
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();  
switch (result)  
{  
    case ExtendedExecutionResult.Allowed:  
        DoLongRunningWork();  
        break;  

    default:  
    case ExtendedExecutionResult.Denied:  
        DoShortRunningWork();  
        break;  
}
```

您可以在應用程式出現於前景時立即要求此延伸執行工作階段。 無約束的延伸執行工作階段不受能源配額及作業系統省電模式限制。 只要有工作階段物件的參考存在，應用程式就繼續保持在執行中狀態，而不會進入暫停狀態。 如果使用者關閉應用程式，將會撤銷工作階段。

註冊 **Revoked** 事件可讓應用程式執行任何必要的清理工作。 在暫止狀態中，您可以使用 [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) 建立延伸執行工作階段，以便在應用程式終止以及從記憶體移除之前儲存使用者資料。

## <a name="run-background-tasks-indefinitely"></a>無限期執行背景工作

在通用 Windows 平台中，背景工作是不使用任何形式的使用者介面在背景中執行的處理序。 背景工作在遭到取消後，通常可能最多再執行 25 秒。 有些執行較久的工作還會進行檢查以確保背景工作不會閒置或佔用記憶體。 在 Windows Creators Update (版本 1703) 中，引進了 [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) 受限功能來移除這些限制。 **extendedBackgroundTaskTime** 功能在應用程式資訊清單中會當做受限功能加入。

_Package.appxmanifest_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

這項功能會移除執行時間限制和閒置工作看門狗。 背景工作啟動後，無論是由觸發程序或應用程式服務呼叫所啟動，一旦對 **Run** 方法提供的 [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 執行延遲，就可以無限期執行。 如果應用程式設定已設定為 **\[由 Windows 管理\]**，仍然可能會套用能源配額，當 \[省電模式\] 為啟用狀態時，便無法啟動其背景工作。這可以使用作業系統設定來變更。 詳細資訊可在[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)中取得。

通用 Windows 平台會監視背景工作執行，以確保正常的電池使用時間和順暢的前景應用程式體驗。 不過，個人應用程式和企業營運應用程式可以使用延伸執行和 **extendedBackgroundTaskTime**，建立不考慮裝置資源可用性而執行任意長時間的應用程式。

請注意，**extendedExecutionUnconstrained** 和 **extendedBackgroundTaskTime** 功能可以覆寫 UWP app 的預設原則，可能會導致大量電池電力流失。 使用這些功能之前，請先確認預設延伸執行及背景工作時間原則無法符合您的需求，並在電池電力有限的條件下執行測試，以了解應用程式對裝置產生的影響。

## <a name="see-also"></a>請參閱

[移除背景工作資源限制](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)
