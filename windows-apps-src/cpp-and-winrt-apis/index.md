---
description: C++/WinRT 是 Windows 執行階段 (WinRT) API 的全新完全標準現代 C++17 語言投影，實作為標頭檔式的程式庫。
title: C++/WinRT
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影
ms.localizationpriority: medium
ms.openlocfilehash: 664fd22fc954403776e1becc31563a06d5fdd15b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8947319"
---
# <a name="cwinrt"></a>C++/WinRT

[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)是完全標準現代 C + + 17 語言投影適用於 Windows 執行階段 (WinRT) Api，實作為標頭檔案式的程式庫，以及設計用來提供您現代化 Windows api 第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 Windows 執行階段 API。 Windows SDK 包含 C++/WinRT；其在版本 10.0.17134.0 (Windows 10，版本 1803 ) 中引進。

C++/WinRT 適用於任何對寫出漂亮且快速的 Windows 程式碼感興趣的開發人員。 原因如下。

## <a name="the-case-for-cwinrt"></a>C++/WinRT 的案例
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

C++ 程式設計語言同時用於企業 *和* 獨立軟體廠商 (ISV) 區段，適用於需要高正確性、品質和效能價值的應用程式。 例如：系統程式設計；資源受限內嵌和行動裝置系統；遊戲和圖形；裝置驅動程式；以及業界、科學和醫療應用程式等等。

從語言的觀點來看，C++ 向來與類型豐富且輕量的撰寫和使用抽象概念有關。 但因為原始指標、原始迴圈和詳細的記憶體配置與 C++98 發行，語言有了大幅度的變更。 現代化 C++ (從 C++ 11 後續版本) 是關於清楚運算式的想法、簡單、可讀性和較低引入錯誤的可能性。

使用 C++，還有 C++/WinRT，適合撰寫與使用 Windows 執行階段 API。 這是 Microsoft 的建議替代方案為[C + + /CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)語言投影，與[Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。

當您使用 C++/WinRT 時，使用標準 C++ 資料類型、演算法及關鍵字。 投影會有自己的自訂資料類型，但在大部分案例中，您不需要了解它們因為它們從標準類型提供適當轉換，以及提供適當轉換至標準類型。 如此一來，可以繼續使用您已經習慣使用的標準 C++ 語言功能，以及您已經擁有的原始碼。 C++/WinRT 可讓您在任何 C++ 應用程式中很輕鬆呼叫 Windows 執行階段 API，從 Win32 到 UWP。

C++/WinRT 執行得更好，且比適用於 Windows 執行階段的任何其他語言選項所產生的二進位檔要小。 這甚至勝過直接使用 ABI 介面的手寫程式碼。 這是因為抽象概念使用現代化 C++ 慣用語法，Visual C++ 編譯器設計用來最佳化。 最新版的 Visual C++ 中包括神奇靜態、空白的基底類別、**strlen** elision，以及許多較新的最佳化，最新版特別針對 C++/WinRT 的效能做改善。

### <a name="topics-about-cwinrt"></a>主題有關 C + + /winrt

| 主題 | 描述 |
| - | - |
| [C++/WinRT 的簡介](intro-to-using-cpp-with-winrt.md) | C++/ WinRT&mdash;Windows 執行階段 API 的標準 C++ 語言投影的簡介。 |
| [開始使用 C++/WinRT](get-started.md) | 為了加快使用 C + + / WinRT，本主題逐步解說一個簡單的程式碼範例。 |
| [有何新在 C + + /winrt](news.md) | 新聞和變更至 C + + /winrt。 |
| [常見問答集](faq.md) | 有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。 |
| [疑難排解](troubleshooting.md) | 無論您是剪下新的程式碼或移植現有的應用程式，本主題中的疑難排解問題和解決方式表格可能對您會很有幫助。 |
| [Photo Editor C++/WinRT 範例應用程式](photo-editor-sample.md) | Photo Editor 是 UWP 範例應用程式，展示使用 C++/WinRT 語言投影進行開發。 範例應用程式可讓您從**圖片**庫擷取相片，然後以混合的相片效果編輯所選的影像。 | 
| [字串處理](strings.md) | 使用 C++/WinRT，您可以使用標準 C++ 寬字串類型呼叫 Windows 執行階段 API，或可以使用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 類型。 |
| [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md) | 使用 C++/WinRT，您可以使用標準 C++ 資料類型呼叫 Windows 執行階段 API。 |
| [Boxing 和 unboxing 純量數值到 IInspectable ](boxing.md) | 在傳遞至需要 **IInspectable** 的函示之前，必須將純量數值包裝在參考資料類別物件中。 該包裝處理程序稱為*boxing*值。 |
| [使用 C++/WinRT 使用 API ](consume-apis.md) | 本主題示範如何使用 C++/WinRT API，無論 Windows、第三方元件廠商或您自己是否實作它們。 |
| [使用 C++/WinRT 撰寫 API ](author-apis.md) | 本主題示範如何直接或間接使用 **winrt::implements** 基礎結構撰寫 C++/WinRT API。 |
| [使用 C++/WinRT 的錯誤處理](error-handling.md) | 本主題討論使用 C++/WinRT 程式設計時處理錯誤的策略。 |
| [使用委派來處理事件](handle-events.md) | 本主題示範如何註冊和撤銷使用 C++/WinRT 的事件處理委派。 |
| [撰寫事件](author-events.md) | 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。 |
| [使用 C++/WinRT 的集合](collections.md) | C + + /winrt 提供函式和您節省很多時間和精力當您想要實作和/或傳遞集合的基底類別。 |
| [並行和非同步作業](concurrency.md) | 本主題中示範的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件。 |
| [XAML 控制項；繫結至一個 C++/WinRT 屬性](binding-property.md) | 可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 本主題示範如何實作和使用可觀察屬性，以及如何將 XAML 控制項繫結至它。 |
| [XAML 項目控制項；繫結至一個 C++/WinRT 集合](binding-collection.md) | 可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 本主題示範實作和使用可觀察集合的方法，以及如何將 XAML 項目控制項繫結至它。 |
| [使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項](xaml-cust-ctrl.md) | 本主題會引導您完成的步驟建立簡單的自訂控制項使用 C + + /winrt。 您可以在此處資訊來建立您自己的豐富的功能和可自訂 UI 控制項上建置。 |
| [使用 C++/WinRT 來使用 COM 元件](consume-com.md) | 本主題使用完整的 Direct2D 程式碼範例，示範如何使用 C + + /winrt 取用 COM 類別和介面。 |
| [使用 C++/WinRT 撰寫 COM 元件](author-coclasses.md) | C + + WinRT 可協助您撰寫傳統 COM 元件，就如同它可協助您撰寫 Windows 執行階段類別。 |
| [從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md) | 本主題示範如何將 C++/CX 程式碼移植到其在 C++/WinRT 中的對等項目。 |
| [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md) | 本主題示範可用於 [C + + / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 與 C++/WinRT 物件之間轉換的協助程式函式。 |
| [從 WRL 移到 C++/WinRT](move-to-winrt-from-wrl.md) | 本主題示範如何將 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 程式碼移植到其在 C++/WinRT 中的對等項目。 |
| [C++/WinRT 與 ABI 之間的互通性](interop-winrt-abi.md) | 本主題示範如何在應用程式二進位介面 (ABI) 與 C++/WinRT 物件之間轉換。 |
| [強式和弱式參考，在 C + + /winrt](weak-references.md) | Windows 執行階段是計算參考次數系統;請務必讓您了解的重要性和區別，這類系統在強式和弱式參考。 |
| [敏捷式物件](agile-objects.md) | 敏捷式物件是可以從任何執行緒中存取的一個。 C++/WinRT 預設為敏捷式，但您可以選擇退出。 |

### <a name="topics-about-the-c-language"></a>C + + 語言相關主題

| 主題 | 描述 |
| - | - |
| [值類別，以及它們的參考](cpp-value-categories.md) | 本主題描述各種不同的值存在於 c + + 類別。 您將會相信有聽過的值和右，但有也會其他種類。 |

## <a name="important-apis"></a>重要 API
* [Winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
