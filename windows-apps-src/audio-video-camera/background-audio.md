---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: 本文說明當您的 app 在背景執行時如何播放媒體。
title: 在背景播放媒體
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a0c816470f4a6caf79cb3370a39bc76abb7ef878
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364031"
---
# <a name="play-media-in-the-background"></a>在背景播放媒體
本文說明如何設定您的 app，當 app 從前景移至背景時，該媒體仍能繼續播放。 這表示即使使用者已最小化您的 App、回到主畫面，或以其他方式從您的 App 離開之後，您的 App 仍可繼續播放音訊。 

背景音訊播放的案例包含：

-   **長時間執行的播放清單：** 使用者會短暫叫用前景 app 來選取和開始播放清單，完成這些動作後，使用者預期播放清單會在背景持續播放。

-   **使用工作切換器：** 使用者會短暫叫用前景 app 來開始播放音訊，然後使用工作切換器，切換到另一個開啟的 app。 使用者預期音訊會在背景持續播放。

本文中描述的背景音訊實作可讓您的 app 在所有 Windows 裝置上執行，包括行動裝置、桌上型電腦及 Xbox。

> [!NOTE]
> 本文中的程式碼是從 UWP [背景音訊範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)改編而來。

## <a name="explanation-of-one-process-model"></a>單處理序模型的說明
從 Windows 10 版本 1607 開始，引進了新的單一處理序模型，可大幅簡化啟用背景音訊的處理序。 之前，除了前景 app 之外還要求您的 app 能夠管理背景處理程序，然後在這兩個處理程序之間手動傳遞狀態變更。 在新模型中，您只需將背景音訊功能新增到您的應用程式資訊清單，而您的 app 將會在移至背景時自動繼續播放音訊。 有兩個新的應用程式週期事件 ([**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 和 [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)) 可讓您的 app 知道它進入和離開背景的時機。 當您的 app 移到轉換為背景或轉換為前景的處理程序時，系統強制執行的記憶體限制可能會變更，讓您能夠使用這些事件來檢查您目前的記憶體耗用量並釋出資源，以維持在限制之下。

藉由消除複雜的跨處理程序通訊與狀態管理，新模型可讓您藉由大幅減少程式碼，更快速地實作背景音訊。 不過，目前的回溯相容性版本仍然支援兩個處理程序的模型。 如需詳細資訊，請參閱[舊版的背景音效模型](legacy-background-media-playback.md)。

## <a name="requirements-for-background-audio"></a>背景音訊的需求
當您的 app 處於背景時，該 app 必須符合下列需求才能播放音訊。

* 將**背景媒體播放**功能新增到您的應用程式資訊清單，如本文稍後所述。
* 如果您的 app 會停用 **MediaPlayer** 與系統媒體傳輸控制項 (SMTC) 的自動整合 (例如，將 [**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 屬性設為 false)，則您必須實作與 SMTC 的手動整合，以啟用背景媒體播放。 如果您使用 **MediaPlayer** 以外的 API (例如 [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph)) 來播放音訊，而且想要在您的應用程式 移至背景時繼續播放音訊，也必須手動與 SMTC 整合。 最低的 SMTC 整合需求請參閱[系統媒體傳輸控制項的手動控制項](system-media-transport-controls.md)的＜針對背景音訊使用系統媒體傳輸控制項＞一節中所述。
* 當您的 app 處於背景時，您必須維持在系統針對背景 app 所設定的記憶體使用量限制之下。 本文稍後將提供處於背景時用於管理記憶體的指導方針。

## <a name="background-media-playback-manifest-capability"></a>背景媒體播放資訊清單功能
若要啟用背景音訊，您必須將背景媒體播放功能新增到應用程式資訊清單檔案 Package.appxmanifest。 

**使用資訊清單設計工具以將功能新增到應用程式資訊清單**

1.  在 Microsoft Visual Studio 的 **方案總管**中，按兩下 **package.appxmanifest** 專案以開啟應用程式資訊清單的設計工具。
2.  選取 [功能] 索引標籤。
3.  選取 [ **背景媒體播放** ] 核取方塊。

若要藉由手動編輯應用程式資訊清單 xml 來設定功能，請先確認已在 **Package** 元素中定義 *uap3* 命名空間前置詞。 如果沒有，請新增它，如下所示。
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

接下來，將 *backgroundMediaPlayback* 功能新增到 **Capabilities** 元素︰
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

## <a name="handle-transitioning-between-foreground-and-background"></a>處理前景與背景之間的轉換
當您的 app 從前景移到背景時，會引發 [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 事件。 當您的 app 回到前景時，會引發 [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 事件。 因為這些是應用程式週期事件，您應該在建立 app 時登錄這些事件的處理常式。 在預設專案範本中，這表示將它新增到 App.xaml.cs 中的 **App** 類別建構函式。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetRegisterEvents":::

建立變數以追蹤您目前是否正在背景中執行。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetDeclareBackgroundMode":::

引發 [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 事件時，請設定追蹤變數，以指出您目前正在背景中執行。 您不應該在 **EnteredBackground** 事件中執行長時間執行的工作，因為這會在轉換到背景時導致使用者看見緩慢的轉換過程。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetEnteredBackground":::

在 [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 事件處理常式中，您應該設定追蹤變數，以指出您的 App 已不在背景執行。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetLeavingBackground":::

### <a name="memory-management-requirements"></a>記憶體管理需求
處理前景和背景之間轉換最重要的部分，便是管理 App 使用的記憶體。 因為在背景中執行將會減少系統允許您 App 保留的記憶體資源，所以，您也應該註冊 [**AppMemoryUsageIncreased**](/uwp/api/windows.system.memorymanager.appmemoryusageincreased) 和 [**AppMemoryUsageLimitChanging**](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) 事件。 引發這些事件時，您應該檢查 App 目前的記憶體使用量與目前的限制，然後視需求降低記憶體使用量。 如需降低在背景執行時的記憶體使用量，請參閱[當 App 移至背景時釋出記憶體](../launch-resume/reduce-memory-usage.md)。

## <a name="network-availability-for-background-media-apps"></a>背景媒體 App 的網路可用性
所有網路感知的媒體來源 (不是從資料流或檔案建立的媒體來源) 會在擷取遠端內容時，讓網路連線保持使用中狀態，並且在不需擷取遠端內容時釋放網路連線。 [**MediaStreamSource**](/uwp/api/Windows.Media.Core.MediaStreamSource) 特別依賴應用程式使用 [**SetBufferedRange**](/uwp/api/windows.media.core.mediastreamsource.setbufferedrange)，正確地向平台報告正確的緩衝範圍。 針對整個內容完整進行緩衝處理之後，就不會再代替 app 保留網路了。

如果您需要在無法下載媒體時，於背景中進行網路呼叫，則必須將它們包裝於適當的工作中，例如 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 或 [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)。 如需詳細資訊，請參閱 [使用背景工作支援您的應用程式](../launch-resume/support-your-app-with-background-tasks.md)。

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)
* [背景音訊範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 
