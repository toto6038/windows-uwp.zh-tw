---
description: Photo Editor 是 UWP 範例應用程式，展示使用 C++/WinRT 語言投影進行開發。 範例應用程式可讓您從圖片庫擷取相片，然後以混合的相片效果編輯所選的影像。
title: Photo Editor C++/WinRT 範例應用程式
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, sample, application, photo, editor, 標準, 投影, 範例, 應用程式, 相片, 編輯器
ms.localizationpriority: medium
ms.openlocfilehash: 8c6f668ef3d92f968e75659b0ba1937abadb079c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606703"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Photo Editor C++/WinRT 範例應用程式
您可以從 [Photo Editor C++/WinRT 範例應用程式](https://github.com/Microsoft/Windows-appsample-photo-editor) GitHub 存放庫複製或下載範例應用程式。

Photo Editor 應用程式是通用 Windows 平台 (UWP) UWP 範例應用程式，展示使用 [C++/WinRT](intro-to-using-cpp-with-winrt.md) 語言投影進行開發。 範例應用程式可讓您從**圖**片庫擷取相片，然後以混合的相片效果編輯所選的影像。 在範例的原始程式碼中，您會看到一些常見的做法&mdash;例如使用 C++/WinRT 投影執行[資料繫結](binding-property.md)與[非同步動作和作業](concurrency.md)&mdash;。 以下是一些範例所展示的特定功能。
    
- 使用標準 C++17 語法和文件程式庫與 Windows 執行階段 (WinRT) API。
- 使用協同程式，包括 co_await、co_return、[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 和 [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)。
- 建立和使用自訂的 Windows 執行階段類別 (執行階段類別) 投影類型及實作類型。 如需有觀這些條款的詳細資訊，請參閱[使用 API 與 C++/WinRT](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。
- [事件處理](handle-events.md)，包括使用自動撤銷事件語彙基元。
- 針對影像效果，使用外部 Win2D NuGet 套件和 [Windows::UI::Composition](/uwp/api/windows.ui.composition)。
- XAML 資料繫結，包括 [{X:bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)。
- XAML 樣式和 UI 自訂，包括[連接動畫](../design/motion/connected-animation.md)。
