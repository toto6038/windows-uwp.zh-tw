---
author: Xansky
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: 檢閱 Microsoft Advertising 程式庫的版本資訊。
title: Advertising 程式庫的版本資訊
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, ads, advertising, release notes, 廣告, 版本資訊
ms.localizationpriority: medium
ms.openlocfilehash: dbe932eb9391a4de0304b4be42944b2bced3287a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5888747"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising 程式庫的版本資訊




本節提供目前版本 Microsoft Advertising 程式庫的版本資訊。 這些程式庫支援 windows 10、 Windows8.1、 Windows Phone 8.1 及 WindowsPhone8 的 XAML 和 JavaScript/HTML 應用程式。

## <a name="installation"></a>安裝


Microsoft Advertising 程式庫屬於 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 的一部分。 如需有關如何安裝 SDK 的詳細資訊，請參閱[安裝 Microsoft Advertising SDK](install-the-microsoft-advertising-libraries.md)。

## <a name="uninstall-previous-versions"></a>解除安裝之前的版本

在您安裝最新的 Microsoft Advertising SDK 之前，強烈建議您先解除安裝所有先前的 SDK 執行個體。 如需詳細資訊，請參閱[安裝 Microsoft Advertising SDK](install-the-microsoft-advertising-libraries.md)。

## <a name="target-architecture-specific-build-outputs"></a>目標架構特定的建置輸出

使用 Microsoft Advertising 程式庫時，您在專案中將無法以 **\[任何 CPU\]** 為目標。 如果專案的目標是 **\[任何 CPU\]**，當您將參照新增到 Microsoft Advertising 程式庫之後，便可能會在專案中看到警告。 如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 如需詳細資訊，請參閱[已知問題](known-issues-for-the-advertising-libraries.md)。

## <a name="c-support"></a>C++ 支援

Microsoft Advertising 程式庫 (其中包括 **AdControl** 和 **InterstitialAd** 類別) 支援使用 Windows 執行階段互通性 (*interop*) 以C++ 和 DirectX 撰寫的 app。 如需使用 XAML 和 C++ 程式設計的詳細資訊與相關範例，請參閱[類型系統](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)。

## <a name="no-toolbox-control"></a>沒有工具箱控制項

在 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 中目前版本的 Microsoft Advertising 程式庫中，沒有任何可供將 **AdControl** 或 **InterstitialAd** 拖曳到您 App 中設計介面的工具箱控制項。 如需在您的標記和程式碼中新增這些控制項的相關指示，請參閱[開發人員逐步解說](developer-walkthroughs.md)。

## <a name="latitude-and-longitude-properties-no-longer-available"></a>緯度和經度屬性不再提供

**AdControl** 類別不再有適用於 UWP app 的 **Latitude** 和 **Longitude** 屬性。 相反地，廣告控制項內建的程式碼會偵測，並代替 app 將這些值傳送至廣告伺服器。


 

 
