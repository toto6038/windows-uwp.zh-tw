---
author: PatrickFarley
title: "與遠端應用程式服務通訊"
description: "使用專案 &quot;Rome&quot; 與遠端裝置上執行的應用程式服務交換訊息。"
translationtype: Human Translation
ms.sourcegitcommit: c90304b7ca3f7185fca9146aa2303b09cba5ab9a
ms.openlocfilehash: bff77a63d0f88907410c74d4dce19fb422c1bd3f

---

# 與遠端應用程式服務通訊

除了使用 URI 啟動遠端裝置上的 app，您還可以在遠端裝置上執行「app 服務」**和與之通訊。 任何 Windows 裝置都可以當成家用或目標裝置，或兩者來使用。 這讓您不需要將應用程式帶到前景，就能以不拘的方式與連接的裝置互動。

## 設定目標裝置上的應用程式服務
您必須已在目標裝置上安裝應用程式服務的提供者，才能在遠端裝置上執行應用程式服務。 本指南會使用可在 [Windows 通用範例儲存機制](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)上找到的隨機數字產生器應用程式服務。 如需您自己的應用程式服務要如何撰寫的指示，請參閱[建立和取用應用程式服務](how-to-create-and-consume-an-app-service.md)。

不論您正在使用現成的應用程式服務或要撰寫自己的，都需要進行一些編輯，才能讓服務與遠端系統相容。 在 Visual Studio 中，移至應用程式服務提供者的專案，然後選取其 Package.appxmanifest 檔案。 以滑鼠右鍵按一下，然後選取 [檢視程式碼]**** 以檢視檔案的完整內容。 尋找將專案定義為應用程式服務的 **Extension** 元素，並命名其父專案。

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

將 **AppService** 元素的命名空間變更為 **uap3**，並新增 **SupportsRemoteSystems** 屬性︰

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

若要使用這個新命名空間中的元素，您必須在資訊清單檔案的頂端新增命名空間定義。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

建置您的應用程式服務提供者專案，並部署到目標裝置。

## 以家用裝置的應用程式服務為目標
用以呼叫遠端應用程式服務的「那個」**裝置，需要有提供遠端系統功能的 app。 這可以加入目標裝置上提供應用程式服務那一個的 app (在這種情況下，您會在兩個裝置上安裝相同的 app)，或放入完全不同的 app。

本節中的程式碼需要下列 **using** 陳述式，才能依現況執行：

[!code-cs[主要](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


您必須先將 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection) 物件具現化，就如同您在本機呼叫應用程式服務一樣。 [建立和取用應用程式服務](how-to-create-and-consume-an-app-service.md)當中會涵蓋這個處理序的更多詳細資料。 在這個範例中，當成目標的應用程式服務是隨機數字產生器服務。

> [!NOTE]
> 其假設 [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 物件已在會呼叫下列方法的程式碼內，利用一些方法取得 。 如需這要如何設定的指示，請參閱[啟動遠端 app](launch-a-remote-app.md)。

[!code-cs[主要](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

接下來，為預定的遠端裝置建立 [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) 物件。 然後用來針對該裝置開啟 **AppServiceConnection**。 請注意，下列範例大幅簡化錯誤處理和報告，以保持簡潔。

[!code-cs[主要](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

此時，您應該針對遠端電腦上的應用程式服務開啟連線。

## 透過遠端連線交換服務特定訊息

接下來，您能以 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset) 物件的形式，對服務傳送訊息和接收和接收服務的訊息 (如需詳細資訊，請參閱[建立和取用應用程式服務](how-to-create-and-consume-an-app-service.md))。 隨機數字產生器服務會利用索引鍵 `"minvalue"` 與 `"maxvalue"`，取得兩個整數做為輸入，隨機選取其範圍內的整數，然後以索引鍵 `"Result"` 將值傳回呼叫的處理序。

[!code-cs[主要](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

現在您已連線到遠端裝置上的應用程式服務，可在該裝置上執行操作，然後收到送至家用裝置的回應資料。

## 相關主題

[已連線的 app 與裝置 (專案 "Rome") 概觀](connected-apps-and-devices.md)  
[啟動遠端 app](launch-a-remote-app.md)  
[建立和使用應用程式服務](how-to-create-and-consume-an-app-service.md)  
[遠端系統 API 參考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )示範如何探索遠端系統、啟動遠端系統上的 app，以及使用應用程式服務在兩個系統上執行的應用程式之間傳送訊息。



<!--HONumber=Aug16_HO3-->


