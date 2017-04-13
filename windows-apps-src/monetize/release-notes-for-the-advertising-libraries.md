---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "檢閱 Microsoft Store Services SDK 中 Microsoft Advertising 程式庫的版本資訊。"
title: "Advertising 程式庫的版本資訊"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, ads, advertising, release notes, 廣告, 版本資訊"
ms.openlocfilehash: f3d07df6e64c96e9070cb82bd7ac6e96b9cad1ee
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising 程式庫的版本資訊




本節提供 Microsoft Store Services SDK (適用於 UWP app)，以及適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK (適用於 Windows 8.1 和 Windows Phone 8.x App) 中 Microsoft Advertising 程式庫之目前版本的版本資訊。 這些程式庫支援 Windows 10、Windows 8.1、 Windows Phone 8.1 和 Windows Phone 8 的 XAML 和 JavaScript/HTML 應用程式。

## <a name="installation"></a>安裝


Microsoft Advertising 程式庫隨附於 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (適用於 UWP app)，以及適用於 [Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) (適用於 Windows 8.1 和 Windows Phone 8.x App) 中。 如需有關如何安裝 SDK 及其中所包含的程式庫詳細資訊，請參閱[安裝 Microsoft Advertising 程式庫](install-the-microsoft-advertising-libraries.md)。

若要取得適用於 Windows Phone 8.x Silverlight 專案的 Microsoft Advertising 組件，請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)，在 Visual Studio 中開啟您的專案，然後移至 **\[專案\]** > **\[加入已連接服務\]** > **\[Ad Mediator\]** 以自動下載組件。 完成後，若您不想使用廣告流量分配，請從您的專案中移除 Ad Mediator 參考。 如需詳細資訊，請參閱 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。


## <a name="uninstall-previous-versions"></a>解除安裝之前的版本

在您安裝 Microsoft Store Services SDK (適用於 UWP app) 或適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK (適用於 Windows 8.1 和 Windows Phone 8.x App) 之前，強烈建議您先將所有舊版的 Microsoft Universal Ad Client SDK 或 Microsoft Advertising SDK 執行個體解除安裝。

## <a name="target-architecture-specific-build-outputs"></a>目標架構特定的建置輸出

使用 Microsoft Advertising 程式庫時，您在專案中將無法以 **\[任何 CPU\]** 為目標。 如果專案的目標是 **\[任何 CPU\]**，當您將參照新增到 Microsoft Advertising 程式庫之後，便可能會在專案中看到警告。 如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 如需詳細資訊，請參閱[已知問題](known-issues-for-the-advertising-libraries.md)。

## <a name="c-support"></a>C++ 支援

Microsoft Advertising 程式庫 (其中包括 **AdControl** 和 **InterstitialAd** 類別) 支援使用 Windows 執行階段互通性 (*interop*) 以C++ 和 DirectX 撰寫的 app。 如需使用 XAML 和 C++ 程式設計的詳細資訊與相關範例，請參閱[類型系統](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx)。

## <a name="no-toolbox-control"></a>沒有工具箱控制項

在 Microsoft Store Services SDK 或適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK 中目前版本的 Microsoft Advertising 程式庫中，沒有任何可供將 **AdControl** 或 **InterstitialAd** 拖曳到您 App 中設計介面的工具箱控制項。 如需在您的標記和程式碼中新增這些控制項的相關指示，請參閱[開發人員逐步解說](developer-walkthroughs.md)。

## <a name="latitude-and-longitude-properties-no-longer-available"></a>緯度和經度屬性不再提供

**AdControl** 類別不再有適用於 UWP app 的 **Latitude** 和 **Longitude** 屬性。 相反地，廣告控制項內建的程式碼會偵測，並代替 app 將這些值傳送至廣告伺服器。


 

 
