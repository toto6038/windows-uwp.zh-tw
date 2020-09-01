---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: 瞭解如何安裝 Microsoft Advertising SDK，以在 Windows 10 通用 Windows 平臺 (UWP) 應用程式中顯示廣告。
title: 安裝 Microsoft Advertising SDK
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 廣告, 安裝, SDK, 廣告庫
ms.localizationpriority: medium
ms.openlocfilehash: a7ec56281c5f1d441d3808fa91491d0d290018f3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155492"
---
# <a name="install-the-microsoft-advertising-sdk"></a>安裝 Microsoft Advertising SDK

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

若要在您的適用於 Windows 10 的 UWP app 中顯示廣告，請安裝 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)。 這個 SDK 是 Visual Studio 2015 和更新版本的擴充功能。

> [!NOTE]
> 如果您正在開發 JavaScript/HTML UWP 應用程式，且您已安裝 Windows 10 SDK 版本 10.0.14393 (年度更新) 或更新版本，您也必須安裝 [WinJS](https://github.com/winjs/winjs) 程式庫。 這個程式庫原本包含在舊版的 Windows 10 SDK 中，但是從 Windows 10 SDK 版本 10.0.14393 (年度更新版) 起必須另外安裝。

<span id="install-msi" />

## <a name="install-via-msi"></a>透過 MSI 安裝

透過 MSI 安裝程式安裝 Microsoft Advertising SDK：

1.  關閉所有 Visual Studio 執行個體。

2. 如果您先前已安裝任何版本的 Microsoft Advertising SDK、Universal Ad Client SDK、Ad Mediator 擴充功能或 Microsoft Store Engagement and Monetization SDK，請將這些 SDK 版本解除安裝。 或者，也可以開啟 **\[命令提示字元\]** 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何舊廣告 SDK 版本：
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  下載並安裝 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)。 可能需要幾分鐘的時間來安裝。 請務必等到程序完成為止。

4.  重新啟動 Visual Studio。

5.  如果您現有的專案參考來自任何舊版 Microsoft Advertising SDK、通用廣告用戶端 SDK 或 Microsoft Store Engagement and Monetization SDK 的廣告庫，建議您在 Visual Studio 中開啟您的專案，然後清除並重建您的專案 (在 **\[方案總管\]** 中您的專案節點上按一下滑鼠右鍵並選擇 **\[清除\]**，然後在您的專案節點上再次按一下滑鼠右鍵並選擇 **\[重建\]**)。

  否則，如果您是第一次在專案中使用 Microsoft Advertising SDK，您現在便已準備好[新增 Microsoft Advertising SDK 的參考](#reference)。

<span id="install-nuget" />

## <a name="install-via-nuget"></a>透過 NuGet 安裝

若要透過 NuGet 在特定 UWP 專案中安裝 Microsoft Advertising SDK：

1.  關閉所有 Visual Studio 執行個體。

2.  如果您先前已安裝任何版本的 Microsoft Advertising SDK、Universal Ad Client SDK、Ad Mediator 擴充功能或 Microsoft Store Engagement and Monetization SDK，請將這些 SDK 版本解除安裝。 或者，也可以開啟 **\[命令提示字元\]** 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何舊廣告 SDK 版本：
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  啟動 Visual Studio，然後開啟您要在其中使用 Microsoft Advertising SDK 的專案。
    > [!NOTE]
    > 如果您的專案已經包含來自先前 MSI 安裝之 SDK 的程式庫參考，請從您的專案中移除這些參考。 這些參考的旁邊將會有警告圖示，因為在先前的步驟中已移除它們所參考的程式庫。

4. 在 Visual Studio 中，按一下 [專案]**** 和 [管理 NuGet 套件]****。

5. 在搜尋方塊中，輸入 **Microsoft.Advertising.XAML** (適用於 XAML 專案) 或 **Microsoft.Advertising.JS** (適用於 JavaScript/HTML 專案) 並安裝對應的套件。 套件完成安裝後，儲存您的方案。
    > [!NOTE]
    > 如果 **\[輸出\]** 視窗回報 *Install-Package* 錯誤，指出指定的路徑太長，您可能需要設定讓 NuGet 將套件解壓縮至路徑比預設位置短的替代位置。 若要這樣做，請將 `repositoryPath` 值新增到您電腦上的 nuget.config 檔案中，然後將它指派至可解壓縮 NuGet 套件的較短資料夾路徑。 如需詳細資訊，請參閱 NuGet 文件中的[這篇文章](/nuget/consume-packages/configuring-nuget-behavior)。 或者，您也可以嘗試將您的 Visual Studio 專案移至路徑較短的替代資料夾。

6. 關閉您的方案，然後重新開啟它。

7.  如果您的專案已經參考來自透過 NuGet 安裝之舊版 Microsoft Advertising SDK 的程式庫，而您已將專案更新至新版 SDK，建議您清除並重建您的專案 (在 **\[方案總管\]** 中您的專案節點上按一下滑鼠右鍵並選擇 **\[清除\]**，然後在您的專案節點上再次按一下滑鼠右鍵並選擇 **\[重建\]**)。

  否則，如果您是第一次在專案中使用 SDK，您現在便已準備好[新增 Microsoft Advertising SDK 的參考](#reference)。

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>新增Microsoft Advertising SDK 的參考

安裝 Microsoft Advertising SDK 之後，請依照這些指示，在專案中參考 SDK 以便使用廣告 API。

1. 在 Visual Studio 中，開啟您的專案。
    > [!NOTE]
    > 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft Advertising SDK 的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

2. 在**方案總管**中，以滑鼠右鍵按一下 [**參考**]，然後選取 [**加入參考**]。

3. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK\]** 旁邊的核取方塊 (適用於 XAML app) 或 **\[適用於 JavaScript 的 Microsoft Advertising SDK\]** 旁邊的核取方塊 (適用於使用 JavaScript 和 HTML 建置的應用程式)。

4.  在 [參考管理員]**** 中，按一下 [確定]。

如需示範如何開始使用廣告 API 的逐步解說，請參閱下列文章：

* [插播式廣告](interstitial-ads.md)
* [原生廣告](native-ads.md)
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 JAVAscript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>了解 Microsoft Advertising SDK 中的架構套件

適用於 UWP App 的 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 中的 Microsoft.Advertising.dll 程式庫是設定為*架構套件*。 這個程式庫包含 [Microsoft.Advertising](/uwp/api/microsoft.advertising) 和 [Microsoft.Advertising.WinRT.UI](/uwp/api/microsoft.advertising.winrt.ui) 命名空間中的廣告 API。

因為這個程式庫是架構套件，這意謂著在使用者安裝使用這個程式庫的 App 版本之後，每當我們發佈具有修正程式和效能改進的新程式庫版本時，便會在其裝置上透過 Windows Update 自動更新這個程式庫。 這有助於確保您客戶的裝置上一律會安裝最新的可用程式庫版本。

如果我們發行的新版 SDK 在這個程式庫中引進新的 API 或功能，您將必須安裝最新版的 SDK 才能使用這些功能。 在此情況下，您也需要將已更新的 App 發佈到「Microsoft Store」。