---
title: 適用于 Windows 的 Rust 和 *windows* 包裝箱
description: 使用 *windows* 包裝箱和呼叫 windows api。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、microsoft、learning rust、windows 上適用于初學者的 rust、使用 vs code 的 rust、rust for windows
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 1ba0acf298a3bd492ceac9adccafcac857f67cba
ms.sourcegitcommit: f7c7a2ae6367e114a8b9d438963082440cd24043
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315061"
---
# <a name="rust-for-windows-and-the-windows-crate"></a>適用于 Windows 的 Rust 和 *windows* 包裝箱

&nbsp;
> [!VIDEO https://www.youtube.com/embed/LajquCjHXK4]

## <a name="introducing-rust-for-windows"></a>Windows Rust 簡介

在 [Windows 上使用 Rust 主題進行開發的總覽](overview.md) 中，我們示範了一個簡單的應用程式，可輸出 *Hello，world！* 回應。 但是，您不只可以 *在 windows 上* 使用 Rust，也可以使用 Rust 來撰寫 *適用于 windows 的* 應用程式。

適用于 Windows (的 Rust 目前為預覽表單) 是 Windows 的最新語言投影。 適用于 Windows 的 Rust 可讓您直接順暢地使用任何 Windows API (過去、現在及未來 [的](https://crates.io/crates/windows)) ， (的建立 *者是二進位* 或程式庫的 Rust 術語，以及（或）組建成一個) 的原始程式碼。

無論是永恆價值函式（例如 [CreateEventW](/windows/win32/api/synchapi/nf-synchapi-createeventw) 和 [WaitForSingleObject](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject)）、強大的圖形引擎（例如 [Direct3D](/windows/win32/direct3d12/directx-12-programming-guide)）、傳統的視窗化功能（例如 [CreateWindowExW](/windows/win32/api/winuser/nf-winuser-createwindowexw) 和 [DispatchMessageW](/windows/win32/api/winuser/nf-winuser-dispatchmessagew)），或較新的使用者介面 (UI) 架構（例如 [撰寫](/uwp/api/windows.ui.composition) 和 [Xaml](/uwp/api/windows.ui.xaml)）， [ *windows* 中的](https://crates.io/crates/windows) 您都有提及。

[Win32metadata](https://github.com/microsoft/win32metadata)專案的目標是提供 Win32 api 的中繼資料。 此中繼資料描述 API 介面 &mdash; 強型別 api 簽章、參數和類型。 如此一來，就可以透過 Rust (以及 c # 和 c + +) 等語言，以自動化且完整的方式來投射整個 Windows API。 另請參閱 [讓 Win32 api 更容易存取更多語言](https://blogs.windows.com/windowsdeveloper/2021/01/21/making-win32-apis-more-accessible-to-more-languages/)。

作為 Rust 的開發人員，您將會使用貨物 (Rust 的封裝管理工具) &mdash; 以及 `https://crates.io` (Rust 社區的「建立者」 &mdash; 登錄) 來管理專案中的相依性。 好消息是，您可以從 Rust apps 參考 [ *windows* 包裝箱](https://crates.io/crates/windows) ，然後立即開始呼叫 windows api。 您也可以在中找到 [ *Windows* 的 Rust 檔](https://docs.rs/windows/0.3.1/windows/) `https://docs.rs` 。

類似于 [c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/)， [Rust for Windows](https://github.com/microsoft/windows-rs) 是在 GitHub 上開發的開放原始碼語言投影。 如果您有關于 Rust for Windows 的問題，或如果您想要回報問題，請使用 [適用于 windows 的 Rust](https://github.com/microsoft/windows-rs) 存放庫。

Rust for Windows 存放庫也有一些您可以遵循的 [簡單範例](https://github.com/microsoft/windows-rs/tree/master/examples) 。 而且有一個絕佳的範例應用程式，採用 Robert Mikhayelyan 的 [掃雷](https://github.com/robmikh/minesweeper-rs)形式。

## <a name="contribute-to-rust-for-windows"></a>參與適用于 Windows 的 Rust

[適用于 Windows 的 Rust](https://github.com/microsoft/windows-rs) 歡迎您投稿！

* 識別並修正[原始程式碼](https://github.com/microsoft/windows-rs/tree/master/src)中的 Bug

## <a name="rust-documentation-for-the-windows-api"></a>Windows API 的 Rust 檔

Rust for Windows 受益于 Rust 開發人員所喜愛的完美工具鏈。 但是，如果您手邊的整個 Windows API 看起來有點令人望而生畏，也有 [Rust 的 WINDOWS api 檔](https://microsoft.github.io/windows-docs-rs/doc/bindings/Windows/)。

此資源基本上記載了 Windows Api 和類型投射到慣用 Rust 的方式。 您可以使用它來流覽或搜尋您需要知道的 Api，以及您需要知道如何呼叫的 Api。

## <a name="writing-an-app-with-rust-for-windows"></a>使用適用于 Windows 的 Rust 撰寫應用程式

下一個主題是 [RSS 讀取器教學](rss-reader-rust-for-windows.md)課程，我們將在此逐步解說使用 Rust for Windows 撰寫簡單的應用程式。

## <a name="related"></a>相關參考

* [使用 Rust 在 Windows 上進行開發的總覽](overview.md)
* [RSS 讀取程式教學課程](rss-reader-rust-for-windows.md)
* [*Windows* 包裝箱](https://crates.io/crates/windows)
* [*Windows* 包裝箱的檔](https://docs.rs/windows/0.3.1/windows/)
* [Win32 中繼資料](https://github.com/microsoft/win32metadata)
* [讓 Win32 Api 更能供更多語言存取](https://blogs.windows.com/windowsdeveloper/2021/01/21/making-win32-apis-more-accessible-to-more-languages/)
* [Windows API 的 Rust 檔](https://microsoft.github.io/windows-docs-rs/doc/bindings/windows/)
* [適用于 Windows 的 Rust](https://github.com/microsoft/windows-rs)
* [掃雷範例應用程式](https://github.com/robmikh/minesweeper-rs)
