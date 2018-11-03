---
author: Xansky
Description: The Microsoft Store Services SDK provides libraries and tools that you can use to add features to your apps that help you make more money and gain customers.
title: 透過 Microsoft Store Services SDK 與客戶互動
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.author: mhopkins
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: b5f29062e00701f578b63f79935d17b1203afc0f
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5984907"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>透過 Microsoft Store Services SDK 與客戶互動

Microsoft Store Services SDK 提供功能，協助您吸引客戶在您的通用 Windows 平台 (UWP) 應用程式，例如將目標式的通知傳送到您的應用程式和執行 A / B 實驗中您的應用程式。 此 SDK 是適用於 Visual Studio 2015 及後續 Visual Studio 版本的擴充功能。

> [!NOTE]
> 若要在您的 UWP app 中顯示廣告，請使用 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 而非 Microsoft Store Services SDK。 Advertising 程式庫已從 Microsoft Store Services SDK 移至 Microsoft Advertising SDK。 如需詳細資訊，請參閱[在您的應用程式中顯示廣告](display-ads-in-your-app.md)。



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Microsoft Store Services SDK 支援的案例

Microsoft Store Services SDK 目前支援下列 UWP app 案例。 如需 API 參考文件，請參閱 [Microsoft Store Services SDK API 參考](https://docs.microsoft.com/uwp/api/overview/engagement)。

|  案例  |  描述   |
|------------|----------------|
|  [在您的 UWP app 中使用 A/B 測試來執行實驗](run-app-experiments-with-a-b-testing.md)    |  在「通用 Windows 平台」(UWP) app 中執行 A/B 測試，以在將功能釋出給每個人之前，先對部分客戶測量功能是否有效。 您在合作夥伴中心中定義實驗之後，使用[StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)類別來取得實驗變化中您的應用程式、 使用此資料來修改您測試，此功能的行為，然後使用 LogForVariation [](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)方法將檢視事件和轉換事件傳送至合作夥伴中心。 最後，使用合作夥伴中心來檢視結果並管理實驗。  |
|  [從您的 UWP app 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)    |  在您的 UWP app 中使用 [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 類別將您的 Windows10 客戶引導至「意見反應中樞」，以便他們在其中提交問題、建議及附議。 然後，管理此[意見反應報告](../publish/feedback-report.md)合作夥伴中心中的意見反應。 |
|  [設定您的 UWP app 以接收合作夥伴中心推播通知](configure-your-app-to-receive-dev-center-notifications.md)    |  在您的 UWP app 中使用[StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager)類別，來註冊您的應用程式以接收目標式推播通知您傳送給您使用合作夥伴中心的客戶。  |
|   [在您的 UWP 應用程式的使用方式報告，在合作夥伴中心記錄自訂事件](log-custom-events-for-dev-center.md)   |  在您的 UWP app 中使用[StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)類別，來與您在合作夥伴中心的應用程式相關聯的自訂事件記錄。 然後，檢閱自訂事件[使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)合作夥伴中心中的**自訂事件**] 區段中的次數總計。  |

<span id="prerequisites" />

## <a name="prerequisites"></a>必要條件

Microsoft Store Services SDK 需要：

* Visual Studio 2015 或更新版本。
* 與您的 Visual Studio 版本一起安裝的 Visual Studio Tools for Universal Windows Apps。

<span id="install" />

## <a name="install-the-sdk"></a>安裝 SDK

在開發電腦上安裝 Microsoft Store Services SDK 有兩個選項：

* **MSI 安裝程式**&nbsp;&nbsp;您可以透過[這裡](http://aka.ms/store-em-sdk)提供的 MSI 安裝程式來安裝 SDK。
* **NuGet 套件**&nbsp;&nbsp;您可將 SDK 安裝為 NuGet 套件。

Microsoft 會定期發行具有效能改進與新功能的新版 Microsoft Store Services SDK。 如果您現有的專案使用此 SDK，而您想要使用最新的版本，則您只需在開發電腦上下載並安裝最新版的 SDK 即可。

<span id="install-msi" />

### <a name="install-via-msi"></a>透過 MSI 安裝

透過 MSI 安裝程式安裝 Microsoft Store Services SDK：

1.  關閉所有 Visual Studio 執行個體。

2. 如果您先前已安裝 Microsoft Store Engagement and Monetization SDK、通用廣告用戶端 SDK 或 Ad Mediator 擴充功能，請立即將這些 SDK 解除安裝。 您也可以開啟 **\[命令提示字元\]** 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何舊 SDK 版本：
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  下載並安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。 可能需要幾分鐘的時間來安裝。 請務必等到程序完成為止。

4.  重新啟動 Visual Studio。

5.  如果您現有的專案參考來自任何舊版 Microsoft Store Services SDK, Microsoft Advertising SDK、Universal Ad Client SDK 或 Microsoft Store Engagement and Monetization SDK 的程式庫，建議您在 Visual Studio 中開啟您的專案，然後清除並重建您的專案 (在 **\[方案總管\]** 中您的專案節點上按一下滑鼠右鍵並選擇 **\[清除\]**，然後在您的專案節點上再次按一下滑鼠右鍵並選擇 **\[重建\]**)。

  否則，如果您是第一次在專案中使用 SDK，您現在便已準備好[將組件參考新增至專案](#references)。

<span id="install-nuget" />

### <a name="install-via-nuget"></a>透過 NuGet 安裝

透過 NuGet 安裝 Microsoft Store Services SDK 程式庫：

1.  關閉所有 Visual Studio 執行個體。

2. 如果您先前已安裝 Microsoft Store Engagement and Monetization SDK、通用廣告用戶端 SDK 或 Ad Mediator 擴充功能，請立即將這些 SDK 解除安裝。 您也可以開啟 **\[命令提示字元\]** 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何舊 SDK 版本：
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  啟動 Visual Studio，然後開啟您要在其中使用 Microsoft Store Services SDK 的專案。
    > [!NOTE]
    > 如果您的專案已經包含來自先前 MSI 安裝之 SDK 的程式庫參考，請從您的專案中移除這些參考。 這些參考的旁邊將會有警告圖示，因為在先前的步驟中已移除它們所參考的程式庫。

4. 在 Visual Studio 中，按一下 **\[專案\]** 和 **\[管理 NuGet 套件\]**。

5. 在搜尋方塊中，輸入 **Microsoft.Services.Store.Engagement** 並安裝 Microsoft.Services.Store.Engagement 套件。 套件完成安裝後，儲存您的方案。
    > [!NOTE]
    > 如果 **\[輸出\]** 視窗回報 *Install-Package* 錯誤，指出指定的路徑太長，您可能需要設定讓 NuGet 將套件解壓縮至路徑比預設位置短的替代位置。 若要這樣做，請將 ```repositoryPath``` 值新增到您電腦上的 nuget.config 檔案中，然後將它指派至可解壓縮 NuGet 套件的較短資料夾路徑。 如需詳細資訊，請參閱 NuGet 文件中的[這篇文章](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)。 或者，您也可以嘗試將您的 Visual Studio 專案移至路徑較短的替代資料夾。

6. 關閉包含您專案的 Visual Studio 方案，然後重新開啟方案。

7.  如果您的專案已經參考來自透過 NuGet 安裝之舊版 Microsoft Store Services SDK 的程式庫，而您已將專案更新至新版 SDK，建議您清除並重建您的專案 (在 **\[方案總管\]** 中您的專案節點上按一下滑鼠右鍵並選擇 **\[清除\]**，然後在您的專案節點上再次按一下滑鼠右鍵並選擇 **\[重建\]**)。

  否則，如果您是第一次在專案中使用 SDK，您現在便已準備好[將組件參考新增至專案](#references)。

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>將組件參考新增至專案

透過 MSI 安裝程式或 NuGet 安裝 Microsoft Store Services SDK 之後，請依照下列指示在您的 UWP 專案中參考 SDK 組件。

1. 在 Visual Studio 中，開啟您的專案。
    > [!NOTE]
    > 如果您的專案是以**任何 CPU** 為目標的 JavaScript App，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。

2. 在 **\[方案總管\]** 中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。

3. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[Microsoft Engagement Framework\]** 旁邊的核取方塊。 這可讓您使用 [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement) 命名空間中的 API。

3. 按一下 **\[確定\]**。

> [!NOTE]
> 如果您已透過 NuGet 安裝 SDK 程式庫，您的專案將會包含 **Microsoft.Services.Store.Engagement** 參考。 **Microsoft.Services.Store.Engagement** 參考代表 NuGet 套件 (而不是它當中的程式庫)，您可以忽略它。

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>了解 SDK 中的架構套件

Microsoft Store Services SDK 中的 Microsoft.Services.Store.Engagement.dll 程式庫設定為*架構套件*。 這個程式庫包含 [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement) 命名空間中的 API。

因為這個程式庫是架構套件，這意謂著在使用者安裝使用這個程式庫的 App 版本之後，每當我們發佈具有修正程式和效能改進的新程式庫版本時，便會在其裝置上透過 Windows Update 自動更新這個程式庫。 這有助於確保您客戶的裝置上一律會安裝最新的可用程式庫版本。

如果我們發行的新版 SDK 在這個程式庫中引進新的 API 或功能，您將必須安裝最新版的 SDK 才能使用這些功能。 在此情況下，您也需要將已更新的 App 發佈到「Microsoft Store」。

## <a name="related-topics"></a>相關主題

* [Microsoft Store Services SDK API 參考](https://docs.microsoft.com/uwp/api/overview/engagement)
* [使用 A/B 測試執行實驗](run-app-experiments-with-a-b-testing.md)
* [從您的 App 啟動意見反應中樞](launch-feedback-hub-from-your-app.md)
* [設定您的應用程式會接收到合作夥伴中心推播通知](configure-your-app-to-receive-dev-center-notifications.md)
* [為合作夥伴中心記錄自訂事件](log-custom-events-for-dev-center.md)
