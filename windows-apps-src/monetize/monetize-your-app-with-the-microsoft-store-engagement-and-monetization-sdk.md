---
author: mcleanbyron
Description: "Microsoft Store Engagement and Monetization SDK 提供了一些可讓您在應用程式中新增更多功能的程式庫和工具，以協助您產生更高獲利及增加客戶。"
title: Microsoft Store Engagement and Monetization SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: de85956c7c1d2a0ba509d61ee8928b412f057f8a
ms.openlocfilehash: 481cf2aab806a1f9ce368256a9df8930cbc756c1

---

# Microsoft Store Engagement and Monetization SDK

Microsoft Store Engagement and Monetization SDK 提供了一些程式庫和工具來協助您產生更高獲利及增加客戶，例如在您的 app 中顯示廣告以及使用 A/B 測試執行實驗。 這個 SDK 取代了 Microsoft Universal Ad Client SDK，它會隨著時間而進化，以納入新的客戶參與和獲利功能。


## SDK 中提供的功能

Microsoft Store Engagement and Monetization SDK 提供了一些支援下列功能的程式庫和工具。

### 對 UVP app 使用 A/B 測試執行實驗

在通用 Windows 平台 (UWP) app 中執行 A/B 測試，在將功能發給每個人之前，對部分客戶測量功能的有效性。 當您在開發人員中心儀表板中定義實驗之後，請使用 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 類別來取得您 app 中實驗的變化，使用此資料來修改您正在測試的功能行為，以及使用 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法將檢視事件和轉換事件傳送至開發人員中心。 最後，使用您的儀表板來檢視結果並管理體驗。

如需詳細資訊，請參閱[使用 A/B 測試執行實驗](run-app-experiments-with-a-b-testing.md)。

### UWP app 的 app 意見反應

在您的 UWP app 中使用[Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx)類別將您的 Windows 10 客戶引導至意見反應中樞，以便他們在其中提交問題、建議及附議。 然後，在開發人員中心儀表板的[意見反應報告](../publish/feedback-report.md)中管理此意見反應。

如需詳細資訊，請參閱[從您的 app 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)。

>**注意：** **意見反應**報告目前只提供給已加入[開發人員中心測試人員計畫](../publish/dev-center-insider-program.md)的開發人員帳戶。

### 在您的 App 中顯示廣告

在 UWP app 以及 Windows 8.1 和 Windows Phone 8.x app 中顯示來自 Microsoft 的橫幅廣告或插入式影片廣告，以增加收入。 利用廣告流量分配來顯示來自多個廣告網路提供者的廣告，也可以將您的廣告投放率最大化。

如需詳細資訊，請參閱[在您的 app 中顯示廣告](display-ads-in-your-app.md)。

>**注意：**舊版 Universal Ad Client SDK、Ad Mediator 延伸模組和 Microsoft Advertising SDK 中的廣告功能現在包含在 Microsoft Store Monetization and Engagement SDK 中。

### API 參考

如需 SDK 中 API 的相關參考文件，請參閱 [Microsoft Store Engagement and Monetization SDK API 參考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)。

## 安裝 SDK

若要安裝 Microsoft Store Monetization and Engagement SDK：

1.  關閉 Visual Studio 2013 或 Visual Studio 2015 的所有執行個體，並解除安裝任何舊版的 Universal Ad Client SDK、Ad Mediator 延伸模組或 Microsoft Advertising SDK。
2.  下載並安裝 [SDK](http://aka.ms/store-em-sdk)。 可能需要數分鐘的時間來安裝。 請務必等到程序完成為止。
3.  重新啟動 Visual Studio。

Microsoft 會定期發行具有效能改進與新功能的新版 Microsoft Store Monetization and Engagement SDK。 如果您現有的專案使用 Microsoft Store Monetization and Engagement SDK，而您想要使用最新的版本，只需下載並安裝最新版的 SDK。

舊版 Universal Ad Client SDK、Ad Mediator 延伸模組和 Microsoft Advertising SDK 中的廣告功能現在包含在 Microsoft Store Monetization and Engagement SDK 中。 如果您有現有的 Visual Studio 2015 或 Visual Studio 2013 專案使用先前其中一個版本的廣告功能，您可以繼續處理您的專案，而不需要在安裝 Microsoft Store Monetization and Engagement SDK 之後進行任何變更。

>**附註：**若要隨著 Visual Studio 2015 安裝 Microsoft Store Engagement and Monetization SDK，您必須安裝適用於通用 Windows app 的 Visual Studio Tools 1.1 版或更新版本。 如需這個適用於通用 Windows app 之 Visual Studio Tools 的更新詳細資訊，請參閱[版本資訊](http://go.microsoft.com/fwlink/?LinkID=624516)。

## SDK 中的架構套件

Microsoft Store Monetization and Engagement SDK 中的下列程式庫已設定為*架構套件*：

* Microsoft.Advertising.dll (僅適用於 UWP app 專案)。 這個程式庫包含 **Microsoft.Advertising** 和 **Microsoft.Advertising.WinRT.UI** 命名空間中的廣告 API。

這表示您在開發電腦上安裝 SDK 之後，每當我們發佈具有修正程式和效能改進的新版程式庫時，此程式庫就會透過 Windows Update 自動更新。 這有助於確保您的開發電腦上永遠安裝最新版的程式庫。

此外，在使用者安裝使用此程式庫的 app 版本之後，每當我們發佈具有修正程式和效能改進的新版程式庫時，此程式庫也會在其裝置上自動更新。 這表示使用者永遠會有最新版的程式庫，您不需要將已更新的應用程式版本發佈到市集。

不過，如果我們發行的新版 SDK 在此程式庫中引進新的 API 或功能，您必須安裝最新版的 SDK 才能使用這些功能。 在此情況下，您也需要將已更新的 app 發佈到市集。

SDK 中的其他程式庫 (包括適用於其他目標平台的 Microsoft.Advertising.dll 和適用於廣告流量分配的程式庫) 目前並未設定為架構程式庫。

## 相關主題

* [使用 A/B 測試執行實驗](run-app-experiments-with-a-b-testing.md)
* [Microsoft Store Engagement and Monetization SDK API 參考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [從您的 app 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)



<!--HONumber=Jun16_HO4-->


