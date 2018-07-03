---
author: normesta
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 在 WPF 和 Windows Form 應用程式中裝載 UWP 控制項
ms.author: normesta
ms.date: 05/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 4823654bce3373ace5b04ced8ec14c4b6c1b6f1d
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862067"
---
# <a name="host-uwp-controls-in-wpf-and-windows-forms-applications"></a>在 WPF 和 Windows Form 應用程式中裝載 UWP 控制項

> [!NOTE]
> 正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

我們將 UWP 控制項放到桌面上，以便您可以 Fluent Design 功能提升外觀、感覺和現有 WPF 或 Windows 應用程式的功能。 以下有兩種做法。

首先，您可以直接將控制項加入至 WPF 或 Windows Forms 專案的設計介面，然後像設計工具中的任何其他控制項一樣來使用它們。  請立即嘗試新的 **WebView** 控制項。 這個控制項使用 Microsoft Edge 轉譯引擎，但到目前為止，這個控制項只有 UWP 應用程式才能使用。 您可以在最新版本的 [Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中找到 **WebView**。

很快，您就可以存取更多的 Fluent Design 功能：我們將會提供控制項，可讓您裝載各種 UWP 控制項。 在未來版本的 Windows 社群工具組中尋找此控制項和許多其他控制項。

以下可快速查看這些控制項在架構上的組織方式。 此圖表中使用的名稱會隨時變更。  

![裝載控制項架構](images/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。  您將新增到您的設計工具的控制項，是隨附於 Windows 社群工具組中的 Nuget 套件。

這些新的控制項都有限制，所以在您使用前，請花一些時間檢閱什麼尚未支援，以及哪些只有利用因應措施才能使用。

### <a name="whats-supported"></a>支援的功能

大部分，支援全部，除非以下明確提出。

### <a name="whats-supported-only-with-workarounds"></a>只有利用因應措施才能使用

:heavy_check_mark︰在多個視窗內裝載多個收件匣控制項。 您將必須將每個視窗置於其本身的執行緒。

:heavy_check_mark：使用 ``x:Bind`` 和裝載的控制項。 您將必須在 .NET Standard 程式庫中宣告資料模型。

:heavy_check_mark：以 C# 為基礎的第三方控制項。 如果您有第三方控制項的原始程式碼，您可以對它進行編譯。

### <a name="whats-not-yet-supported"></a>不支援的功能

:no_entry_sign：無縫跨應用程式與裝載的控制項運作的協助工具。

:no_entry_sign：當地語系化您新增到不包含 Windows 應用程式套件的應用程式中的控制項內容。

:no_entry_sign：在不包含 Windows 應用程式套件的應用程式內以 XAML 製作的資產參考。

:no_entry_sign：正確對應 DPI 和縮放尺寸變更的控制項。

:no_entry_sign：新增  **WebView** 控制項到自訂使用者控制項 (開啟執行緒、關閉執行緒或退出處理器)。

:no_entry_sign：[顯色醒目提示](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 效果。

:no_entry_sign：輸入控制項用的內嵌筆跡 @Places 和 @People。

:no_entry_sign：指派快速鍵。

:no_entry_sign： C++ 型協力廠商控制項。

:no_entry_sign︰裝載自訂使用者控制項。

我們會持續改善桌面 Fluent 的體驗，所以這份清單中的項目也可能會變更。  
