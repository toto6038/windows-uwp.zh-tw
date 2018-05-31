---
author: stevewhims
description: 有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。
title: 有關 C++/WinRT 的常見問題集
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、常見、提問、問題、faq
ms.localizationpriority: medium
ms.openlocfilehash: aad5c5ed2123af39ebb6aff0c9098586ce958196
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832032"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>有關 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的常見問題集
> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>C++/WinRT [Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix) 需求是什麼？
[VSIX](https://aka.ms/cppwinrt/vsix) 強制執行 10.0.17134.0 (Windows 10，版本 1803 ) 的最小 Windows SDK 目標版本。 您也需要 Visual Studio 2017 版本 15.6，或更新版本。 您可以識別一個專案，其藉由 `.vcxproj`檔案中的 `<PropertyGroup Label="Globals">` 裡的 `<CppWinRTEnabled>true</CppWinRTEnabled>` 存在使用 VSIX。 如需詳細資訊，請參閱 [適用於 C++/WinRT 的 Visual Studio 支援，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="whats-a-runtime-class"></a>什麼是*執行階段類別*？
執行階段類別是可透過現代化 COM 介面啟動與使用的類型，一般經過可執行的界限。 不過，執行階段類別也可在實作它的編譯單位中使用。 您可以在介面定義語言 (IDL) 中宣告執行階段類別，並可以在使用 C++/WinRT 的標準 C++ 中實作它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*投影類型*和*實作類型*的意思是什麼？
如果您只是*使用*一個 Windows 執行階段類別 (執行階段類別)，則您可以專用*投影類型*來處理。 C++/WinRT 是一個*語言投影*，因此投影類型是 Windows 執行階段介面一部分，其使用 C++/WinRT *投影*到 C++ 裡。 如需詳細資訊，請參閱 [使用 C++/WinRT 使用 API](consume-apis.md)。

*實作類型*包含執行階段類別的實作，因此它僅在實作執行階段類別的專案中可用。 當您在實作執行階段類別的專案中執行時 (Windows 執行階段元件專案，或使用 XAML UI 的專案)，請務必將熟悉適用於執行階段類別的實作類型間的區別，且代表執行階段類別的投影類型投影至 C++/WinRT。 如需詳細資訊，請參閱 [使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否應該實作 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，該如何進行？
如果在其解構程式中您有釋出資源的執行階段類別，且設計該執行階段類別從其實作編譯單位之外使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)，我們建議您也實作 **IClosable** 以便支援不確定完成的語言使用您的執行階段類別。 請確定不論是否解構函式都會釋出您的資源，[**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close)，或兩者都呼叫。 可以任意呼叫 **IClosable::Close** 數次。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我需要在我使用的執行階段類別上呼叫 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) 嗎？
**IClosable** 存在以支援不確定完成的語言。 因此，您就不應該呼叫 C++/WinRT 的 **IClosable::Close**，除非在非常少數案例中，牽涉到關機競爭或半致命採用。 如果您正使用 **Windows.UI.Composition** 類型，以此為例，您可能會遇到想在一組序列中處置物件的案例，允許 C++/WinRT 包裝函式的解構函式為您執行工作，會是替代方案。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我需要在執行階段類別的 IDL 中宣告一個建構函式嗎？
只有當設計此執行階段類別從其實作編譯單位之外來使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)。 如需 IDL 中完整的目的與宣告建構函式結果的詳細資訊，請參閱 [執行階段類別建構函式](author-apis.md#runtime-class-constructors)。