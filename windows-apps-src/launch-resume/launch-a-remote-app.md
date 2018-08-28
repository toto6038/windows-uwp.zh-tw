---
author: PatrickFarley
title: 啟動遠端裝置上的 App
description: 了解如何使用專案 Rome 啟動遠端裝置上的應用程式。
ms.author: pafarley
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 連線裝置、 遠端系統、 羅馬、 專案羅馬
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 58a420d73ba4a0cd51f909fd5d7d417af1cfb38f
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2018
ms.locfileid: "2889357"
---
# <a name="launch-an-app-on-a-remote-device"></a>啟動遠端裝置上的 App

本文說明如何以啟動遠端裝置上的 Windows 應用程式。

自 Windows 10 (版本 1607) 開始，UWP app 可以在另一部也執行 Windows 10 (版本 1607 或更新版本) 的裝置上，從遠端啟動 UWP app 或 Windows 傳統型應用程式，但前提是這兩部裝置是使用相同的 Microsoft 帳戶 (MSA) 所登入。 這是 Project Rome 的最簡單使用案例。

遠端啟動功能會啟用工作導向的使用者體驗；使用者可以在其中一部裝置上啟動工作，並在另一部上完成該工作。 例如，如果使用者正在車上聆聽手機上的音樂，則當他們到家時可將播放功能交由 Xbox One 繼續執行。 遠端啟動允許應用程式將內容相關的資料傳遞至已啟動的遠端應用程式，以便挑選停止工作的位置。

## <a name="preliminary-setup"></a>初步設定

### <a name="add-the-remotesystem-capability"></a>新增 remoteSystem 功能

為了讓您的應用程式能夠啟動遠端裝置上的應用程式，您必須將 `remoteSystem` 功能新增至應用程式套件資訊清單。 您可以使用套件資訊清單設計工具，在 \[功能\]**** 索引標籤上選取 \[遠端系統\]**** 來新增此功能，或手動將下列程式碼行新增至專案的 _Package.appxmanifest_ 檔案。

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>啟用跨裝置共用

此外，用戶端裝置必須設為允許跨裝置共用。 此設定預設為啟用，可從 \[設定：系統\]******** > \[共用體驗\]**** > \[跨裝置共用\]**** 存取。 

![共用體驗設定頁面](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>尋找遠端裝置

您必須先找到想要連線的裝置。 [探索遠端裝置](discover-remote-devices.md)會詳細討論如何執行這個動作。 這裡我們將使用簡單的方法，不用裝置或連線類型來進行篩選。 我們將建立會監看遠端裝置的遠端系統監控程式，並針對在探索到裝置或其遭到移除時所引發的事件撰寫處理常式。 這將會提供一組遠端裝置。

這些範例的程式碼要求您的類別檔案中必須有 `using Windows.System.RemoteSystems` 陳述式。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

進行遠端啟動之前的第一件事，就是您必須執行呼叫 `RemoteSystem.RequestAccessAsync()`。 檢查傳回值，以確定您的 app 能夠存取遠端裝置。 這項檢查失敗的可能原因之一是，您可能還沒有將 `remoteSystem` 功能新增到您的 app。

在發現到我們能夠連線的裝置或其不再可用時，都會呼叫系統監控程式事件處理常式。 我們將使用這些事件處理常式，不時更新一份可連線裝置的清單。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


我們會使用**字典**，利用遠端系統識別碼來追蹤裝置。 使用 **ObservableCollection** 來保留我們可列舉的裝置清單。 **ObservableCollection** 也能更容易將裝置清單繫結到 UI，但在這個範例中將不會這樣做。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

在您的應用程式啟動程式碼中新增對 `BuildDeviceList()` 的呼叫後，再嘗試啟動遠端應用程式。

## <a name="launch-an-app-on-a-remote-device"></a>啟動遠端裝置上的 App

將您想要連線的裝置傳送至 [**RemoteLauncher.LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx) API，從遠端啟動 app。 這個方法有三個多載。 簡單來說，這個範例會示範如何指定 URI，以啟用遠端裝置上的 app。 在這個範例中，URI 會在遠端電腦上開啟地圖 app，以 3D 方式檢視太空針塔。

其他 **RemoteLauncher.LaunchUriAsync** 多載可讓您指定選項，例如，無法在遠端裝置上啟動適當的 app 時，可用以檢視的網站 URI，以及可用來在遠端裝置上啟動 URI 的選擇性套件系列名稱清單。 您也能以索引鍵/值組的形式提供資料。 您可能會將資料傳遞至您啟用的 app，以便將內容提供給遠端 app，例如，當您要將一部裝置的播放工作交由另一部裝置進行時，會提供要播放的歌曲名稱和目前的播放位置。

在實際案例中，您可能會提供 UI 來選擇想要設為目標的裝置。 但為了簡化這個範例，我們只會使用清單中的第一部裝置。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

從 **RemoteLauncher.LaunchUriAsync()** 傳回的 [**RemoteLaunchUriStatus**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelaunchuristatus.aspx) 物件，會提供遠端啟動是否成功以及為什麼不成功的相關資訊。

## <a name="related-topics"></a>相關主題

[遠端系統 API 參考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[已連線的 App 與裝置 (專案 Rome) 概觀](connected-apps-and-devices.md)  
[探索遠端裝置](discover-remote-devices.md)  
[遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)示範如何探索遠端系統、啟動遠端系統上的 app，以及使用 app 服務在兩個系統上執行的 app 之間傳送訊息。
