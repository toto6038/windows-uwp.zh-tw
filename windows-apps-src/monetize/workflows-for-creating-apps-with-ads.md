---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: "深入了解開發和發佈包含廣告之 App 的端對端處理程序。"
title: "建立包含廣告之 App 的工作流程"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 0f832674f0e635f609f09fa15acead109b1d67ff


---

# 建立包含廣告之 App 的工作流程




若要在您的 App 中顯示廣告，您的 App 需要能從廣告網路接收廣告。 Microsoft 提供了一個 Web 服務，可讓 Windows 應用程式開發人員接收廣告。 當使用者按一下 App 中的廣告，您 (廣告的「發行者」**) 就可以從廣告的建立者 (「廣告客戶」**) 賺取金額。 從廣告客戶方面賺取的金額會使用您的帳戶支付給您。

下列高階步驟說明開發和發佈包含廣告之 App 的一般處理程序。

1.  開發階段：

    * 設定您的 Windows 開發人員中心帳戶。
    * 使用測試模式的值開發您的 App。

2.  預備發行：

    * 設定您的 App 接收即時廣告。
    * 將您的 App 提交至 Windows 開發人員中心，然後檢閱效能資料。

如需有關每個步驟的詳細資訊，請閱讀以下其對應的章節。

## 設定您的 Windows 開發人員中心帳戶

您必須具備 Windows 開發人員中心帳戶，才能發佈您的 App 與接收廣告。 無論您是否使用廣告流量分配，這都是成立的。 廣告相關的 App 管理也會在 Windows 開發人員中心中完成。 如果您曾使用 Microsoft pubCenter 來管理 App 中的廣告，這已經由 Windows 開發人員中心中的功能取代。

若要設定 Windows 開發人員中心帳戶，請造訪[首頁](https://dev.windows.com/windows-apps)。 Windows 開發人員中心[說明頁面](https://dev.windows.com/develop)可取得其他資訊。

## 使用測試模式值開發您的 App

使用下列逐步解說中的指示新增 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)，以在您的 App 中顯示廣告。

-   [插入式廣告](interstitial-ads.md)
-   [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
-   [HTML 5 和 JavaScript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
-   [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)

當您使用 **AdControl** 或 **InterstitialAd** 在您的 App 中顯示廣告時，您必須在您的程式碼中指定應用程式識別碼與廣告單位識別碼，以將您的 App 連結至您的 Windows 開發人員中心帳戶並提供廣告。 當您開發您的 App 時，使用測試應用程式識別碼和廣告單位識別碼值，以查看您的 App 於測試期間呈現廣告的方式。 這可讓您看到 App 在測試期間接收及呈現廣告的方式。 如需詳細資訊，請參閱[測試模式值](test-mode-values.md)。

如需示範如何使用 C# 和 C++ 將橫幅及影片插入式廣告新增至 JavaScript/HTML App 以及XAML App 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## 設定您的 App 接收即時廣告

在您完成測試您的 App 且準備好將 App 提交至 Windows 開發人員中心時，您必須更新您的 App 程式碼，以使用 [Windows 開發人員中心儀表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)的應用程式識別碼和廣告單位識別碼值。 如果您嘗試在實際 App 中使用測試值，您的 App 將不會收到即時廣告。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md)。

## 提交 App

完成 App 開發之後，您就可以使用 Windows 開發人員中心儀表板在 Windows 市集中發佈您的 App。 除了符合 Windows 市集中所有 App 的需求以外，顯示廣告的 App 也必須符合數個其他的需求。 如需詳細資訊，請參閱[提交包含廣告的 App 到 Windows 市集](submit-an-app-with-ads-to-the-windows-store.md)。

在您發佈 App 且在 Windows 市集中提供使用之後，您可以在開發人員中心儀表板中檢視您的[廣告績效報告](../publish/advertising-performance-report.md)。

 

 



<!--HONumber=Aug16_HO3-->


