---
author: mcleanbyron
Description: "Microsoft Store Services SDK 提供了一些可讓您在 App 中新增功能的程式庫和工具，以協助您產生更高獲利及增加客戶。"
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 011b370b7bd7ad7c7d8f60281261b6da954e2256
ms.openlocfilehash: 840a5e76d409f547d55e558262af09c8fa36a544

---

# Microsoft Store Services SDK

Microsoft Store Services SDK 提供可協助您在「通用 Windows 平台」(UWP) app 中提升獲利及進行客戶業務開發的功能，例如在您的 App 中顯示廣告和執行 A/B 實驗。 此 SDK 是適用於 Visual Studio 2015 及後續 Visual Studio 版本的擴充功能。

## SDK 支援的案例

此 SDK 目前支援下列 UWP app 案例。 此 SDK 會隨時間進化來支援新的業務開發與營收創造案例。 如需有關此 SDK 中 API 的參考文件，請參閱 [Microsoft Store Services SDK API 參考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)。

|  案例  |  描述   |
|------------|----------------|
|  [在您的 UWP app 中使用 A/B 測試來執行實驗](run-app-experiments-with-a-b-testing.md)    |  在「通用 Windows 平台」(UWP) app 中執行 A/B 測試，以在將功能釋出給每個人之前，先對部分客戶測量功能是否有效。 在您於「開發人員中心」儀表板中定義實驗之後，請使用 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 類別來取得您 App 中實驗的變化、使用此資料來修改您正在測試的功能行為，然後使用 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法將檢視事件和轉換事件傳送至「開發人員中心」。 最後，使用您的儀表板來檢視結果並管理實驗。  |
|  [從您的 UWP app 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)    |  在您的 UWP app 中使用 [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) 類別將您的 Windows10 客戶引導至「意見反應中樞」，以便他們在其中提交問題、建議及附議。 然後，在「開發人員中心」儀表板的[意見反應報告](../publish/feedback-report.md)中管理此意見反應。 |
|  [設定您的 UWP app 以接收開發人員中心推播通知](configure-your-app-to-receive-dev-center-notifications.md)    |  在您的 UWP app 中使用 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 類別來登錄 App，以接收您使用「Windows 開發人員中心」儀表板傳送給您客戶的目標式推播通知。  |
|   [在開發人員中心中針對使用方式報告記錄您 UWP app 中的自訂事件](log-custom-events-for-dev-center.md)   |  在您的 UWP app 中使用 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 類別，以在「開發人員中心」中記錄與您 App 關聯的自訂事件。 然後，在「開發人員中心」儀表板中[使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)的 [自訂事件] 區段中，檢閱自訂事件的發生次數總計。  |
|  [在您的 UWP app 中顯示廣告](display-ads-in-your-app.md)    |  在您的 UWP app 中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 控制項來顯示橫幅廣告或插入式廣告以增加營收。<br/><br/>
            **注意**
            &nbsp;&nbsp;Microsoft Store Services SDK 僅支援適用於 Windows10 的 UWP app。 若要在 Windows8.1 與 Windows Phone 8.x App 中顯示廣告，請使用[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。  |

<span id="prerequisites" />
## 先決條件

Microsoft Store Services SDK 需要：

* Visual Studio 2015 或更新版本。
* 與您的 Visual Studio 版本一起安裝的 Visual Studio Tools for Universal Windows Apps。

>
            **注意**
            &nbsp;&nbsp;若要安裝 SDK 以搭配 Visual Studio 2015 使用，您必須已安裝 Visual Studio Tools for Universal Windows Apps 1.1 版或更新版本。 如需有關這項 Visual Studio Tools for Universal Windows Apps 更新的詳細資訊，請參閱[版本資訊](http://go.microsoft.com/fwlink/?LinkID=624516)。

<span id="install" />
## 安裝 SDK

在您的開發電腦上安裝 Microsoft Store Services SDK 以搭配 Visual Studio 2015 (或更新版本) 使用有兩個選項：

* 
            **MSI 安裝程式**
            &nbsp;&nbsp;您可以透過[這裡](http://aka.ms/store-em-sdk)提供的 MSI 安裝程式來安裝 SDK。 使用此選項時，SDK 程式庫會安裝在您開發電腦上的共用位置，以便供 Visual Studio 中的任何 UWP 專案參考。
* 
            **NuGet 套件**
            &nbsp;&nbsp;您可以使用 NuGet 為 Visual Studio 中的特定 UWP 專案安裝 SDK 程式庫。 使用此選項時，只會針對您已在其中安裝 NuGet 套件的專案安裝 SDK 程式庫。

Microsoft 會定期發行具有效能改進與新功能的新版 Microsoft Store Services SDK。 如果您現有的專案使用此 SDK，而您想要使用最新的版本，則您只需在開發電腦上下載並安裝最新版的 SDK 即可。

>
            **注意**
            &nbsp;&nbsp;若要安裝 SDK 以搭配 Visual Studio 2015 使用，您必須已安裝 Visual Studio Tools for Universal Windows Apps 1.1 版或更新版本。 如需有關這項 Visual Studio Tools for Universal Windows Apps 更新的詳細資訊，請參閱[版本資訊](http://go.microsoft.com/fwlink/?LinkID=624516)。

<span id="install-msi" />
### 透過 MSI 安裝

透過 MSI 安裝程式安裝 Microsoft Store Services SDK：

1.  關閉 Visual Studio 2015 (或更新版本) 的所有執行個體。 如果您先前已安裝任何版本的 Microsoft Advertising SDK、Universal Ad Client SDK、Ad Mediator 擴充功能或 Microsoft Store Engagement and Monetization SDK，請將這些 SDK 版本解除安裝。

2.  開啟 [命令提示字元] 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何舊廣告 SDK 版本：
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  下載並安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。 可能需要幾分鐘的時間來安裝。 請務必等到程序完成為止。

4.  重新啟動 Visual Studio。

5.  如果您現有的專案參考來自任何舊版 Microsoft Store Services SDK, Microsoft Advertising SDK、Universal Ad Client SDK 或 Microsoft Store Engagement and Monetization SDK 的程式庫，建議您在 Visual Studio 中開啟您的專案，然後清除並重建您的專案 (在 [方案總管] 中您的專案節點上按一下滑鼠右鍵並選擇 [清除]，然後在您的專案節點上再次按一下滑鼠右鍵並選擇 [重建])。

  否則，如果您是第一次在專案中使用此 SDK，您現在便已準備好[將適當的 Microsoft Store Services SDK 程式庫參考新增到您的專案](#references)。

<span id="install-nuget" />
### 透過 NuGet 安裝

透過 NuGet 為特定專案安裝 Microsoft Store Services SDK 程式庫：

1.  關閉 Visual Studio 2015 (或更新版本) 的所有執行個體。 如果您先前已安裝任何版本的 Microsoft Advertising SDK、Universal Ad Client SDK、Ad Mediator 擴充功能或 Microsoft Store Engagement and Monetization SDK，請將這些 SDK 版本解除安裝。

2.  開啟 [命令提示字元] 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何舊廣告 SDK 版本：
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  啟動 Visual Studio，然後開啟您要在其中使用 Microsoft Store Services SDK 程式庫的專案。

  >
            **注意**
            &nbsp;&nbsp;如果您的專案已經包含來自先前 MSI 安裝之 SDK 的程式庫參考，請從您的專案中移除這些參考。 這些參考的旁邊將會有警告圖示，因為在先前的步驟中已移除它們所參考的程式庫。

4. 在 Visual Studio 中，按一下 [專案] 和 [管理 NuGet 套件]。

5. 在搜尋方塊中，輸入 **Microsoft.Services.Store.SDK** 並安裝 Microsoft.Services.Store.SDK 套件。

  >
            **注意**
            &nbsp;&nbsp;如果 [輸出] 視窗回報 *Install-Package* 錯誤，指出指定的路徑太長，您可能需要設定讓 NuGet 將套件解壓縮至路徑比預設位置短的替代位置。 若要這樣做，請將 ```repositoryPath``` 值新增到您電腦上的 nuget.config 檔案中，然後將它指派至可解壓縮 NuGet 套件的較短資料夾路徑。 如需詳細資訊，請參閱 NuGet 文件中的[這篇文章](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)。 或者，您也可以嘗試將您的 Visual Studio 專案移至路徑較短的替代資料夾。

6. 關閉您的專案，然後重新開啟它。

7.  如果您的專案已經參考來自透過 NuGet 安裝之舊版 Microsoft Store Services SDK 的程式庫，而您已將專案更新至新版 SDK，建議您清除並重建您的專案 (在 [方案總管] 中您的專案節點上按一下滑鼠右鍵並選擇 [清除]，然後在您的專案節點上再次按一下滑鼠右鍵並選擇 [重建])。

  否則，如果您是第一次在專案中使用此 SDK，您現在便已準備好[將適當的 Microsoft Store Services SDK 程式庫參考新增到您的專案](#references)。

<span id="references" />
## 將 SDK 程式庫參考新增到您的專案

透過 MSI 安裝程式或 NuGet 安裝 Microsoft Store Services SDK 之後，請依照下列指示在您的 UWP 專案裝參考 SDK 程式庫。

1. 在 Visual Studio 中，開啟您的專案。

  >
            **注意**
            &nbsp;&nbsp;如果您的專案是以「任何 CPU」為目標的 JavaScript App，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。

2. 在 [方案總管] 中的 [參考] 上按一下滑鼠右鍵，然後選取 [加入參考]。

3. 在 [參考管理員] 中，展開 [通用 Windows]、按一下 [擴充功能]，然後選取下列其中一個項目旁邊的核取方塊。

  * 若要使用 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx) 命名空間中的 API 來進行客戶業務開發案例，請選取 **Microsoft Engagement Framework** 旁邊的核取方塊。 如果您想要[執行 A/B 試驗](run-app-experiments-with-a-b-testing.md)、[啟動意見反應中樞](launch-feedback-hub-from-your-app.md)、[從開發人員中心接收目標式推播通知](configure-your-app-to-receive-dev-center-notifications.md)或[將自訂事件記錄至開發人員中心](log-custom-events-for-dev-center.md)，請選擇此選項。

  * 若要使用 API 以[在您的 App 中顯示橫幅廣告或插入式廣告](display-ads-in-your-app.md)，請根據您的專案類型，選取 [適用於 XAML 的 Microsoft Advertising SDK] 或 [適用於 JavaScript 的 Microsoft Advertising SDK] 旁邊的核取方塊。

3. 按一下 [確定]。

>
            **注意**
            &nbsp;&nbsp;如果您已透過 NuGet 安裝 SDK 程式庫，則除了「適用於 XAML 的 Microsoft Advertising SDK」或「適用於 JavaScript 的 Microsoft Advertising SDK」之外，您的專案還會包含 **Microsoft.Services.Store.SDK** 參考。 
            **Microsoft.Services.Store.SDK** 參考代表 NuGet 套件 (而不是它當中的程式庫)，您可以忽略它。

<span id="framework" />
## 了解 SDK 中的架構套件

下列 Microsoft Store Services SDK 中的程式庫已設定為「架構套件」：

* Microsoft.Advertising.dll。 這個程式庫包含 [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) 和 [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx) 命名空間中的廣告 API。
* Microsoft.Services.Store.Engagement.dll。 這個程式庫包含 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx) 命名空間中的 API。

這意謂著在使用者安裝使用這些程式庫的 App 版本之後，每當我們發佈具有修正程式和效能改進的新程式庫版本時，便會在其裝置上透過 Windows Update 自動更新這些程式庫。 這有助於確保您客戶的裝置上一律會安裝最新的可用程式庫版本。

如果我們發行的新版 SDK 在這些程式庫中引進新的 API 或功能，您將必須安裝最新版的 SDK 才能使用這些功能。 在此情況下，您也需要將已更新的 App 發佈到「市集」。

## 相關主題

* [Microsoft Store Services SDK API 參考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [使用 A/B 測試執行實驗](run-app-experiments-with-a-b-testing.md)
* [從您的 App 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)
* [設定您的 App 以接收開發人員中心推播通知](configure-your-app-to-receive-dev-center-notifications.md)
* [為開發人員中心記錄自訂事件](log-custom-events-for-dev-center.md)
* [在您的 App 中顯示廣告](display-ads-in-your-app.md)



<!--HONumber=Nov16_HO1-->


