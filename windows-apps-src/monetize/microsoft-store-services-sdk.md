---
author: mcleanbyron
Description: "Microsoft Store Services SDK 提供了一些可讓您在 App 中新增功能的程式庫和工具，以協助您產生更高獲利及增加客戶。"
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 98675e9cb06b05e55d49ca625818626aea5a5346

---

# Microsoft Store Services SDK

Microsoft Store Services SDK 提供了一些程式庫和工具來協助您在通用 Windows 平台 (UWP) app 中產生更高獲利及增加客戶，例如在您的 App 中顯示廣告，以及使用 A/B 測試執行實驗。 此 SDK 會隨時間進化，以包括新的業務開發與創造營收功能。


## SDK 中提供的功能

Microsoft Store Services SDK 提供支援下列功能的程式庫和工具。

### 對 UWP app 使用 A/B 測試執行實驗

在通用 Windows 平台 (UWP) app 中執行 A/B 測試，在將功能釋出給每個人之前，對部分客戶測量功能的有效性。 當您在開發人員中心儀表板中定義實驗之後，請使用 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 類別來取得您 App 中實驗的變化，使用此資料來修改您正在測試的功能行為，然後使用 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法將檢視事件和轉換事件傳送至開發人員中心。 最後，使用您的儀表板來檢視結果並管理實驗。

如需詳細資訊，請參閱[使用 A/B 測試執行實驗](run-app-experiments-with-a-b-testing.md)。

### UWP app 的 app 意見反應

在您的 UWP app 中使用 [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) 類別將您的 Windows 10 客戶引導至意見反應中樞，以便他們在其中提交問題、建議及附議。 然後，在開發人員中心儀表板的[意見反應報告](../publish/feedback-report.md)中管理此意見反應。

如需詳細資訊，請參閱[從您的 app 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)。

### 在您的 App 中顯示廣告

在 UWP app 中顯示來自 Microsoft 的橫幅廣告或影片插入式廣告來增加營收。 利用廣告流量分配來顯示來自多個廣告網路提供者的廣告，也可以將您的廣告投放率最大化。

如需詳細資訊，請參閱[在您的 App 中顯示廣告](display-ads-in-your-app.md)。

>**注意**&nbsp;&nbsp;Microsoft Store Services SDK 僅支援 UWP app。 若要在 Windows 8.1 與 Windows Phone 8.x App 中顯示廣告，請使用[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

### API 參考資料

如需 Microsoft Store Services SDK 中 API 的相關參考文件，請參閱 [Microsoft Store Services SDK API 參考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)。

## 安裝 SDK

安裝 Microsoft Store Services SDK：

1.  關閉 Visual Studio 2013 或 Visual Studio 2015 的所有執行個體，並將所有舊版的 Microsoft Store Engagement and Monetization SDK、Universal Ad Client SDK、Ad Mediator 延伸模組或 Microsoft Advertising SDK 解除安裝。
2.  下載並安裝 [SDK](http://aka.ms/store-em-sdk)。 可能需要數分鐘的時間來安裝。 請務必等到程序完成為止。
3.  重新啟動 Visual Studio。

Microsoft 會定期發行具有效能改進與新功能的新版 Microsoft Store Services SDK。 如果您現有的專案使用 Microsoft Store Services SDK，而您想要使用最新的版本，只需下載並安裝最新版的 SDK。

舊版 Microsoft Store Engagement and Monetization SDK、Universal Ad Client SDK、Ad Mediator 延伸模組和 Microsoft Advertising SDK 中適用於 UWP app 的廣告功能，現已包含在 Microsoft Store Services SDK 中。 如果您有現有的 UWP 專案是使用先前其中一個版本的廣告功能，您可以在安裝 Microsoft Store Services SDK 之後繼續處理您的專案，而不需要進行任何變更。

>**注意** 若要使用 Visual Studio 2015 安裝 Microsoft Store Services SDK，您必須安裝適用於通用 Windows App 的 Visual Studio Tools 1.1 版或更新版本。 如需這個適用於通用 Windows App 之 Visual Studio Tools 的更新詳細資訊，請參閱[版本資訊](http://go.microsoft.com/fwlink/?LinkID=624516)。

## SDK 中的架構套件

下列 Microsoft Store Services SDK 中的程式庫已設定為*架構套件*：

* Microsoft.Advertising.dll。 這個程式庫包含 [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) 和 [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx) 命名空間中的廣告 API。
* Microsoft.Services.Store.Engagement.dll。 這個程式庫包含 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx) 命名空間中的 API。

這表示您在開發電腦上安裝 SDK 之後，每當我們發佈具有修正程式和效能改進的新版程式庫時，這些程式庫就會透過 Windows Update 自動更新。 這有助於確保您的開發電腦上永遠安裝最新版的程式庫。

此外，在使用者安裝使用這些程式庫的 App 版本之後，每當我們發佈具有修正程式和效能改進的新版程式庫時，程式庫也會在其裝置上自動更新。 這表示使用者永遠會有最新版的程式庫，您不需要將已更新的 App 版本發佈到市集。

不過，如果我們發行的新版 SDK 在這些程式庫中引進新的 API 或功能，您必須安裝最新版的 SDK 才能使用那些功能。 在此情況下，您也需要將已更新的 App 發佈到市集。

## 相關主題

* [Microsoft Store Services SDK API 參考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [使用 A/B 測試執行實驗](run-app-experiments-with-a-b-testing.md)
* [從您的 App 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)
* [在您的 App 中顯示廣告](display-ads-in-your-app.md)



<!--HONumber=Sep16_HO2-->


