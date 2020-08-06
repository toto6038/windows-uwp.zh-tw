---
Description: 輸入法 (IME) 是一種軟體元件，可讓使用者以不容易在標準鍵盤上呈現的語言來輸入文字。
title: '輸入法編輯器 (IME) '
label: Input Method Editors (IME)
template: detail.hbs
keywords: ime、輸入法編輯器、輸入、互動
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 438c53a0f3fbec1fdac0206bde3584c738759de4
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87840004"
---
# <a name="input-method-editors-ime"></a>輸入法編輯器 (IME) 

輸入法 (IME) 是一種軟體元件，可讓使用者以不容易在標準鍵盤上呈現的語言來輸入文字。 這通常是因為使用者書寫語言中的字元數，例如各種東亞語言。

使用者會輸入 IME 所解讀的按鍵組合，而不是每個出現在單一鍵盤按鍵上的單一字元。 IME 會產生符合一組索引鍵筆劃的字元，或可供選擇的候選字元清單。 選取的字元接著會插入至使用者與之互動的編輯控制項。

> [!NOTE]
> Ime 可以同時支援硬體鍵盤和螢幕上或觸控式鍵盤。

您的應用程式不需要直接與 IME 互動。 輸入法會內建在系統中，就像觸控鍵盤一樣。 如果您的應用程式有文字輸入，而您想要以需要輸入法的語言支援文字輸入，您應該測試端對端客戶體驗以取得文字輸入。 這可讓您修正任何問題，例如調整您的 UI，使其不會被觸控鍵盤或 IME 候選視窗 pixels occluded。

## <a name="creating-an-ime"></a>建立輸入法

為了讓所有使用者都能獲得絕佳的輸入體驗，Microsoft 會針對各種語言產生隨附的 Ime。

除了內建的 Ime 外，您還可以建立自己的自訂 Ime，讓使用者可以安裝和使用，就像是內建的輸入法一樣。

所有的 Ime 都是在 Windows 系統中執行，它會強化以停止惡意的 Ime，並改善所有 Ime 的安全性和使用者體驗。

自訂 Ime 可以連結到預設的觸控鍵盤並使用其版面配置，讓使用者可以使用其 IME 搭配觸控鍵盤。 不過，您無法提供自己的獨立觸控鍵盤，而且自訂 Ime 無法使用適用于觸控鍵盤的內建 Ime 的某些功能。

## <a name="requirements-for-imes"></a>Ime 的需求

協力廠商輸入法必須符合下列需求：

- 必須經過數位簽署
- 必須是[文字服務架構 (TSF) ](/windows/win32/tsf/text-services-framework)感知，並已正確設定適當的輸入法旗標
- 必須遵循輸入法中所述的指導方針[ (輸入法) 需求](input-method-editor-requirements.md)和[設計和程式碼 Windows 應用程式](/windows/uwp/design/)

不符合這些需求的協力廠商輸入法會遭到封鎖而無法執行。

> [!NOTE]
> 舊版的自訂 Ime 可以在桌面應用程式中執行，但在 Windows 應用程式中會遭到封鎖。

此外，Windows Defender 也會從系統移除惡意的 Ime。 因此，請務必熟悉 IME 編碼需求。 如需詳細資訊，請參閱[輸入法 (IME) 需求的編輯器](input-method-editor-requirements.md)。

## <a name="design-guidelines-for-imes"></a>Ime 的設計方針

如需有關輸入法的最佳作法和設計指導方針的詳細資訊，請參閱[輸入法 (IME) 需求](input-method-editor-requirements.md)。 一般而言，所有的 IME Ui 都需要：

- 遵循 Windows 執行階段應用程式的 UX 指導方針
- 避免強制回應的體驗，並且只在需要時顯示輸入法視窗
- 包含僅限黑色和白色的圖示

## <a name="related-topics"></a>相關主題

- [輸入法編輯器 (IME) 需求](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfCoNtextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
