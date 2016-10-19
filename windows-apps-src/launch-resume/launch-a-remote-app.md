---
author: TylerMSFT
title: "啟動遠端裝置上的 app"
description: "了解如何使用專案 Rome 啟動遠端裝置上的 app。"
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: d8c3783d68a1b3b216058790d84255a7fb4b612c

---

# 啟動遠端裝置上的 app

本文說明如何在遠端裝置上啟動通用 Windows 平台 (UWP) app 或 Windows 傳統型應用程式。

自 Windows 10 (版本 1607) 開始，UWP app 可以在另一部也執行 Windows 10 (版本 1607 或更新版本) 的裝置上，從遠端啟動 UWP app 或 Windows 傳統型應用程式。

在遠端裝置上啟動應用程式的案例之一，是為了讓使用者能在一部裝置上開始工作，並在另一部裝置上完成。 例如，您在開車回家的途中聆聽手機上的音樂，您可以在到家時，使用遠端啟動將播放工作交由 Xbox 繼續進行。 您可以將資料傳遞至遠端 app，將內容提供給遠端 app 以繼續執行工作。

## 新增 RemoteSystem 功能

為了在遠端裝置上啟動您的 app，您必須將 &lt;uap3:Capability Name="remoteSystem"/&gt; 功能新增至 app 套件資訊清單。 您可以使用套件資訊清單設計工具，在 [功能]**** 索引標籤上選取 [遠端系統]**** 來新增，或手動執行套件資訊清單設計工具的動作，並將下列項目新增至您的 Package.appxmanifest 檔案。

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## 尋找遠端裝置

您必須先找到想要連線的裝置。 [探索遠端裝置](discover-remote-devices.md)會詳細討論如何執行這個動作。 這裡我們將使用簡單的方法，不用裝置或連線類型來進行篩選。 我們將建立會監看遠端裝置的遠端系統監控程式，並撰寫事件處理常式以在發現使用相同 Microsoft 帳戶的裝置或其遭到移除時收到通知。 這樣會提供一組遠端裝置。

這些範例中的程式碼假設您的檔案中已有 `using Windows.System.RemoteSystems` 陳述式。

[!code-cs[主要](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

進行遠端啟動之前的第一件事，就是您必須執行呼叫 `RemoteSystem.RequestAccessAsync()`。 檢查傳回值，以確定您的 app 能夠存取遠端裝置。 這項檢查失敗的可能原因之一是，您可能還沒有將 `remoteSystem` 功能新增到您的 app。

在發現到我們能夠連線的裝置或其不再可用時，都會呼叫系統監控程式事件處理常式。 我們將使用這些事件處理常式，不時更新一份可連線裝置的清單。

[!code-cs[主要](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

我們會使用「字典」利用遠端系統識別碼來追蹤裝置。 使用 ObservableCollection 來保留我們可列舉的裝置清單。 ObservableCollection 也可以更容易將裝置清單繫結到 UI，但在這個範例中將不會這樣做。

[!code-cs[主要](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

在您的 app 啟動程式碼中新增對 `BuildDeviceList()` 的呼叫後，再嘗試啟動遠端 app。

## 啟動遠端裝置上的 app

將您想要連線的裝置傳送至 [RemoteLauncher.LaunchUri](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx) API，以從遠端啟動 app。 這個函式有三個多載。 簡單來說，這個範例會示範如何指定 URI，以啟用遠端裝置上的 app。 在這個範例中，URI 會在遠端電腦上開啟地圖 app，以 3D 方式檢視太空針塔。

其他 **RemoteLauncher.LaunchUriAsync** 多載可讓您指定的選項，還有像是無法在遠端裝置啟動可處理 URI 的 app 時，可用以檢視的網站 URI，以及可選擇用來在遠端裝置上啟動 URL 的套件系列名稱清單。 您也能以索引鍵/值組的形式提供資料。 您可能會將資料傳遞至您在遠端裝置上啟用的 app，將內容提供給遠端 app，例如當您要將一部裝置的播放工作交由另一部裝置進行時，會提供要播放的歌曲名稱和目前的播放位置。

在實際使用時，您可能會提供 UI 以選擇想要使用的裝置。 但為了簡化這個範例，我們會直接使用清單中的第一項來進行遠端呼叫。

[!code-cs[主要](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

從 **RemoteLauncher.LaunchUriAsync()** 傳回的 [RemoteLaunchUriStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelaunchuristatus.aspx)，會提供遠端啟動是否成功以及為什麼不成功的相關資訊。

## 相關主題

[遠端系統 API 參考](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[已連線的 app 與裝置 (專案 "Rome") 概觀](connected-apps-and-devices.md)  
[遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )示範如何探索遠端系統、啟動遠端系統上的 app，以及使用應用程式服務在兩個系統上執行的應用程式之間傳送訊息。



<!--HONumber=Aug16_HO5-->


