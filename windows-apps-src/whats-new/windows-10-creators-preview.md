---
author: QuinnRadich
title: "Windows 10 Creators Update 預覽的新功能 - 開發 UWP app"
description: "Windows 10 Creators Update 預覽，持續提供通用 Windows 平台所支援的工具、功能及使用經驗。"
ms.assetid: 27a9ce65-c811-4f79-bf65-3493337199c8
keywords: "新功能, 預覽, 更新, 功能, 新, Windows 10, creators"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 29a888286062c59ea257cf0e43d3598d847a6588
ms.openlocfilehash: d36b84299b8e469624cef54f89c94744f3f4ae83
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-in-the-windows-10-creators-update-preview"></a>Windows 10 Creators Update 預覽的新功能

Windows 10 Creators Update 會持續提供通用 Windows 平台所支援的工具、功能及使用經驗。 在 Windows 10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](https://msdn.microsoft.com/library/windows/apps/bg124288)，或是探索[如何在 Windows 上使用現有的 App 程式碼](https://msdn.microsoft.com/library/windows/apps/mt238321)。

這些功能不公開提供，直到 Windows 10 Creators Update 發行為止。 現在，它們在預覽組建中可供 [Windows 測試人員](https://insider.windows.com/)存取使用。 此頁面可能會隨著有更多的可用文件而更新更多新功能的資訊。

如需這和其他 Windows 更新的重點功能詳細資訊，請參閱 [Windows 開發人員日網站](https://developer.microsoft.com/en-us/windows/projects/campaigns/windows-developer-day)和 [Windows 10 中有哪些酷炫功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 此外，請參閱 [Windows 開發人員平台功能](https://developer.microsoft.com/en-us/windows/platform/features)以取得過去與未來功能加入的高階概觀。

功能 | 描述
 :---- | :----
**現代化的 UWP 文件** | 現在 docs.microsoft.com 網站上有提供通用 Windows 平台文件。 您可以立即輕鬆地提交程式碼範例，提升您的技術專業能力，或提供意見反應。 如需指示，請參閱這部[開發人員短片。](https://channel9.msdn.com/Blogs/One-Dev-Minute/Modernizing-the-Windows-UWP-Docs)
**客戶訂單資料庫範例更新** | 在 GitHub 上[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)範例已經更新，可使用 Telerik 資料格控制和資料輸入驗證，這是其 UWP 套件 UI 的一部分。 UWP 套件的 UI 是超過 20 個控制項的集合，透過 .NET Foundation 以開放原始碼專案形式提供。
**筆跡分析** | Windows Ink 現在可以將筆墨筆劃分類為書寫或繪圖筆劃，並辨識文字、形狀和基本配置結構。
**適用於 Android 的 Project Rome SDK** | 適用於 UWP 的 Project Rome 功能已引進到 Android 平台。 您現在可以使用 Windows *或* Android 裝置從遠端啟動應用程式，並在任何 Windows 裝置上繼續工作。 請參閱正式[跨平台案例的 Project Rome 存放庫](https://github.com/Microsoft/project-rome)來開始。
**XAML 控制項** | ContentDialog 現在有三個按鈕：主要、次要和關閉。 您也可以將其中一個按鈕設定為預設動作。 <br> 使用 ShowAsMonochrome 屬性以單一色彩或全彩顯示點陣圖圖示。 <br> 使用新的 SelectionChangedTrigger 變更 ComboBox 處理透過鍵盤選取的方式。 <br> ListViewBase 上的新 PrepareConnectedAnimation 及 TryStartConnectedAnimationAsync API 讓連接的動畫搭配清單和資料格檢視更容易使用。 <br> 使用新的 Icon 屬性，新增圖示至 MenuFlyoutItem 或 MenuFlyoutSubItem。 <br> 使用 SvgImageSource 類別，透過 XAML 新增 SVG 影像。 <br> 使用 LoadedImageSource 類別，透過 XAML 新增組合表面。 <br> 使用 XAMLLight 類別和 UIElement.Lights 屬性，透過 XAML 新增 CompositionLight 效果。 <br> 使用 XamlCompositionBrushBase，透過 XAML 使用組合筆刷。

