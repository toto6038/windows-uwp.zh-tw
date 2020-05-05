---
description: Photo Editor 是 UWP 範例應用程式，展示使用 C++/WinRT 語言投影進行開發。 範例應用程式可讓您從圖片庫擷取相片，然後以混合的相片效果編輯所選的影像。
title: Photo Editor C++/WinRT 範例應用程式
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 範例, 應用程式, 相片, 編輯器
ms.localizationpriority: medium
ms.openlocfilehash: dcefe2ad8321ae85fcb814bbaead0bb0e5373300
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "81266906"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Photo Editor C++/WinRT 範例應用程式

> [!NOTE]
> 此範例是以 Windows 10 版本 1903 (10.0；Build 18362) 和 Visual Studio 2019 為目標並加以測試。 如果您想，您可以使用專案屬性將目標重定為 Windows 10 版本 1809 (10.0；Build 17763)，以及/或以 Visual Studio 2017 開啟範例。

如需複製或下載範例應用程式，請參閱程式碼範例庫上的 [Photo Editor C++/WinRT 範例應用程式](/samples/microsoft/windows-appsample-photo-editor/photo-editor-cwinrt-sample-application/)。

Photo Editor 應用程式是通用 Windows 平台 (UWP) UWP 範例應用程式，展示使用 [C++/WinRT](intro-to-using-cpp-with-winrt.md) 語言投影進行開發。 範例應用程式可讓您從**圖片**庫擷取相片，然後以混合的相片效果編輯所選的影像。 在範例的原始程式碼中，您會看到一些常見的做法&mdash;例如使用 C++/WinRT 投影執行[資料繫結](binding-property.md)與[非同步動作和作業](concurrency.md)&mdash;。 以下是一些範例所展示的特定功能。

- 使用標準 C++17 語法和文件程式庫與 Windows 執行階段 (WinRT) API。
- 使用協同程式，包括 co_await、co_return、[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 和 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1)。
- 建立和使用自訂的 Windows 執行階段類別 (執行階段類別) 投影類型及實作類型。 如需有觀這些條款的詳細資訊，請參閱[使用 API 與 C++/WinRT](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。
- [事件處理](handle-events.md)，包括使用自動撤銷事件語彙基元。
- 針對影像效果，使用外部 Win2D NuGet 套件和 [Windows::UI::Composition](/uwp/api/windows.ui.composition)。
- XAML 資料繫結，包括 [{X:bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)。
- XAML 樣式和 UI 自訂，包括[連接動畫](../design/motion/connected-animation.md)。
