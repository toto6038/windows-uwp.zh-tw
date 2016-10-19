---
author: PatrickFarley
title: "探索遠端裝置"
description: "了解如何使用專案 Rome 從 App 探索遠端裝置。"
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: cb1f9cf6915378203919fdf63bcebc935af74a30

---

# 探索遠端裝置
您的 App 可以使用無線網路、藍牙及雲端連線，來探索使用與探索裝置相同 Microsoft 帳戶登入的 Windows 型裝置。 也可以搜尋接受匿名連線的公用裝置，例如 Surface Hub 和 Xbox One。 遠端裝置不需要安裝任何特殊的軟體即可以搜尋。

> [!NOTE]
> 本指南假設您已經依照[啟動遠端 app](launch-a-remote-app.md) 中的步驟，將存取權授與遠端系統功能。

## 篩選一組可搜尋的裝置
您可以使用 [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) 搭配篩選器，縮小一組可搜尋的裝置範圍。 篩選器可以偵測探索類型 (本機網路與雲端連線)、裝置類型 (桌面、行動裝置、Xbox、集線器及 ) 以及可用性狀態 (使用遠端系統功能的裝置可用性狀態)。

因為物件會當成參數傳遞到其建構函式，所以在初始化 **RemoteSystemWatcher** 物件之前，必須先建構篩選物件。 下列程式碼可建立每種可用類型的篩選器，並新增至清單中。

> [!NOTE]
> 這些範例中的程式碼假設您的檔案中已有 `using Windows.System.RemoteSystems` 陳述式。

[!code-cs[主要](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

建立 [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) 物件的清單之後，即可傳遞至 **RemoteSystemWatcher** 的建構函式。

[!code-cs[主要](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

呼叫這個監控程式的 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) 方法時，只有偵測到符合下列所有條件的裝置時，才會引發 [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) 事件︰
* 可透過近端連線搜尋
* 其是桌面或電話
* 其歸類為可用

從該處開始，處理事件、擷取 [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 物件，以及連線到遠端裝置的程序，和[啟動遠端 app](launch-a-remote-app.md) 完全相同。 簡單來說，**RemoteSystem** 物件會儲存為 [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 物件的屬性，亦即每個 **RemoteSystemAdded** 事件的參數。

## 藉輸入的位址探索裝置
有些裝置可能無法透過掃描來與使用者關聯或搜尋到，但如果探索的 app 直接使用位址，仍然可以到達。  [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) 類別可用來表示遠端裝置的位址。 這通常會以 IP 位址的形式儲存，但也允許數個其他格式 (如需詳細資訊，請參閱 [**HostName 建構函式**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx))。

如果提供有效的 **HostName** 物件，就可以擷取**RemoteSystem** 物件。 如果位址資料無效，則會傳回 `null` 物件參照。

[!code-cs[主要](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## 相關主題
[已連線的 app 與裝置 (專案 "Rome")](connected-apps-and-devices.md)  
[啟動遠端 app](launch-a-remote-app.md)  
[遠端系統 API 參考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )示範如何探索遠端系統、啟動遠端系統上的 app，以及使用應用程式服務在兩個系統上執行的應用程式之間傳送訊息。



<!--HONumber=Aug16_HO5-->


