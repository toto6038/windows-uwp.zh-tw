---
Description: 輸入法編輯器 (IME) 是一種軟體元件，可讓使用者以無法在標準的標準鍵盤上輕鬆表示的語言輸入文字。
title: '輸入法編輯器 (IME) '
label: Input Method Editors (IME)
template: detail.hbs
keywords: 輸入法、輸入法編輯器、輸入、互動
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e7782dea8cd634fd9fe3bac4a3e4c870cd680e9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159942"
---
# <a name="input-method-editors-ime"></a>輸入法編輯器 (IME) 

輸入法編輯器 (IME) 是一種軟體元件，可讓使用者以無法在標準的標準鍵盤上輕鬆表示的語言輸入文字。 這通常是因為使用者撰寫語言中的字元數，例如各種東亞語言。

使用者輸入 IME 所解釋的按鍵組合，而不是每個單一鍵盤按鍵上出現的單一字元。 IME 會產生符合一組按鍵筆劃的字元，或可供選擇的候選字元清單。 然後選取的字元會插入使用者正在與其互動的編輯控制項。

> [!NOTE]
> Ime 可同時支援硬體鍵盤和螢幕或觸控鍵盤。

您的應用程式不需要直接與 IME 互動。 IME 內建在系統中，就如同觸控鍵盤一樣。 如果您的應用程式有文字輸入，而且您想要在需要輸入法的語言中支援文字輸入，您應該測試文字輸入的端對端客戶體驗。 這可讓您修正任何問題，例如調整您的 UI，使其不會被觸控鍵盤或 IME 候選視窗 pixels occluded。

## <a name="creating-an-ime"></a>建立 IME

為了讓所有使用者都能獲得絕佳的輸入經驗，Microsoft 會針對各種語言產生現成的輸入。

除了內建的 Ime 之外，您還可以建立自己的自訂 Ime，讓使用者可以安裝及使用，就像使用內建的 IME 一樣。

所有 Ime 都是在 Windows 系統中執行，這是為了停止惡意的 Ime，以及改善所有 Ime 的安全性與使用者經驗而強化。

自訂 Ime 可以連結至預設的觸控鍵盤並使用其版面配置，讓使用者可以使用其 IME 搭配觸控鍵盤。 不過，您無法為自訂 Ime 提供自己的獨立觸控鍵盤，以及觸控鍵盤的內建 Ime 的某些功能。

## <a name="requirements-for-imes"></a>Ime 的需求

協力廠商 IME 必須符合下列需求：

- 必須經過數位簽署
- 必須 [文字服務架構 (TSF) ](/windows/win32/tsf/text-services-framework) 感知，並正確設定適當的 IME 旗標
- 必須遵循輸入方法編輯器中所述的指導方針 [ (IME) 需求](input-method-editor-requirements.md) 和 [設計和程式碼 Windows 應用程式](../index.md)

不符合這些需求的協力廠商輸入法會被封鎖而無法執行。

> [!NOTE]
> 舊版的自訂 Ime 可以在桌面應用程式中執行，但在 Windows 應用程式中會遭到封鎖。

此外，Windows Defender 也會從系統中移除惡意的 Ime。 因此，請務必熟悉 IME 編碼需求。 如需詳細資訊，請參閱 [輸入方法編輯器 (IME) 需求](input-method-editor-requirements.md)。

## <a name="design-guidelines-for-imes"></a>Ime 的設計方針

如需有關輸入法的最佳作法和設計指導方針的詳細資訊，請閱讀 [輸入方法編輯器 (IME) 需求](input-method-editor-requirements.md) 。 一般情況下，所有的 IME Ui 都需要：

- 遵循 Windows 執行階段應用程式的 UX 指導方針
- 避免強制回應的體驗，並只在需要時才顯示 IME 視窗
- 包含僅限黑色和白色的圖示

## <a name="related-topics"></a>相關主題

- [輸入法編輯器 (IME) 需求](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfCoNtextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)