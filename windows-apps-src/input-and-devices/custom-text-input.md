---
author: Karl-Bridge-Microsoft
Description: "Windows.UI.Text.Core 命名空間中的核心文字 API 讓通用 Windows 平台 (UWP) App 能夠接收來自 Windows 裝置上所支援之任何文字服務的文字輸入。"
title: "自訂文字輸入概觀"
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 31f10b862ba53f2ba51f3936a73e874466590b30

---

# 自訂文字輸入

[**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) 命名空間中的核心文字 API 讓通用 Windows 平台 (UWP) App 能夠接收來自 Windows 裝置上所支援之任何文字服務的文字輸入。 這類 API 十分類似[文字服務架構](https://msdn.microsoft.com/library/windows/desktop/ms629032) API，其中的 App 不需要具備文字服務的詳細知識。 這讓 App 能夠接收任何語言以及來自任何輸入類型的文字，例如鍵盤、語音或手寫筆。


**重要 API**

-   [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)
-   [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)


## 為什麼要使用核心文字 API？


對於許多 App 而言，XAML 或 HTML文字方塊控制項就足以用來進行文字輸入和編輯。 不過，如果您的 App 會處理複雜的文字案例 (例如文書處理 App)，您可能需要自訂文字編輯控制項的彈性。 您可以使用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 鍵盤 API 來建立文字編輯控制項，但是這些 API 不提供接收組合式文字輸入的方式，而這類文字輸入是用來支援東亞語言的必要方式。

當您需要建立自訂的文字編輯控制項時，請改用 [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) API。 這些 API 的設計目的是為您提供許多使用任何語言來處理文字輸入的彈性，可讓您提供最適合 App 的文字經驗。 內建於核心文字 API 的文字輸入與編輯控制項可以接收來自 Windows 裝置上所有現有文字輸入法的文字輸入、來自以[文字服務架構](https://msdn.microsoft.com/library/windows/desktop/ms629032)為基礎的輸入法 (IME) 的文字輸入，以及電腦上的手寫到行動裝置上的 WordFlow 鍵盤 (可提供自動校正、預測及聽寫)。

## 架構


以下是文字輸入系統的簡單表示法。

-   「應用程式」代表裝載使用核心文字 API 建置之自訂編輯控制項的 UWP App。
-   [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) API會透過 Windows 來協助與文字服務進行通訊。 文字編輯控制項和文字服務之間的通訊主要是透過 [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) 物件來處理，此物件會提供方法和事件，協助進行通訊。

![核心文字架構圖](images/coretext/architecture.png)

## 文字範圍與選取項目


編輯控制項提供文字輸入的空間，而使用者預期在此空間的任一處編輯文字。 我們將在此處說明核心文字 API 所使用的文字放置系統，以及如何在此系統中呈現範圍與選取項目。

### 應用程式插入號位置

與核心文字 API 搭配使用的文字範圍是以插入號位置來表示。 「應用程式插入號位置 (ACP)」是以零起始的數字，指出從緊接在插入號前開始之文字資料流的字元數目，如下所示。

![文字資料流圖表範例](images/coretext/stream-1.png)
### 文字範圍與選取項目

文字範圍與選取項目是透過 [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201) 結構來呈現，其中包含兩個欄位：

| 欄位                  | 資料類型                                                                 | 說明                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | 範圍的開始位置是緊接在第一個字元之前的 ACP。 |
| **EndCaretPosition**   | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | 範圍的結束位置是緊接在最後一個字元之後的 ACP。     |

 

例如，在先前所示的文字範圍中，範圍 [0, 5] 指出 "Hello" 這個字。 **StartCaretPosition** 一律必須小於或等於 **EndCaretPosition**。 範圍 \[5, 0\] 無效。

### 插入點

目前的插入號位置 (通常稱為插入點) 是藉由設定 **StartCaretPosition**，使其等於 **EndCaretPosition** 來表示。

### 不連續的選取項目

有一些編輯控制項支援不連續的選取項目。 例如，Microsoft Office App 支援多個任意選取項目，而且有許多原始程式碼編輯器支援選取欄。 不過，核心文字 API 不支援不連續的選取項目。 編輯控制項只能報告單一的連續選取項目，最常見的是不連續選取項目的作用中子範圍。

以下列文字資料流為例：

![範例文字資料流圖表](images/coretext/stream-2.png) 有兩個選項：\[0, 1\] and \[6, 11\]。 編輯控制項只能報告這其中一個項目；\[0, 1\] 或 \[6, 11\]。

## 使用文字


[**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) 類別可透過 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件、[**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件及 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 方法，來啟用 Windows 和編輯控制項之間的文字流向。

編輯控制項會透過 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件來接收文字，這類事件是在使用者與輸入法 (例如，鍵盤、語音或 INE) 互動時所產生。

當您變更編輯控制項中的文字時 (例如，將文字貼入控制項)，需要呼叫 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 來通知 Windows。

如果文字服務需要新文字，則會引發 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件。 您必須在 **TextRequested** 事件處理常式中提供新文字。

### 接受文字更新

編輯控制項通常應該會接受文字更新要求，因為它們代表使用者想要輸入的文字。 在 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件處理常式中，編輯控制項預期要有下列動作：

1.  在 [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) 中指定的位置上，插入 [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) 中指定的文字。
2.  將選取項目放置於 [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) 中指定的位置上。
3.  通知系統，已藉由將 [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 設為 [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237) 成功進行更新。

例如，這是編輯控制項在使用者輸入 "d" 之前的狀態。 插入點是在 \[10, 10\]。

![範例文字資料流圖表](images/coretext/stream-3.png) 當使用者輸入 "d"，會引發包含下列 [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958229) 資料的 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件：

-   [ **Range** ](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [ **Text** ](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [ **NewSelection** ](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

在編輯控制項中，套用指定的變更，並將 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 設為 **Succeeded**。 以下是控制項在套用變更之後的狀態。

![文字資料流圖表範例](images/coretext/stream-4.png)
### 拒絕文字更新

您有時無法套用文字更新，因為要求的範圍是在不得變更的編輯控制項區域內。 在此情況下，您不應該套用任何變更。 而是改為通知系統，已藉由將 [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 設為 [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237)，來讓更新失敗。

以只接受一個電子郵件地址的編輯控制項為例。 由於電子郵件地址不能包含空格，所以必須拒絕空格，因此，在針對空格鍵引發 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件時，您只需在編輯控制項中將 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 設為 **Failed** 即可。

### 通知文字變更

有時您的編輯控制項會對文字進行變更，例如，在貼上或自動校正文字時。 在這些情況下，您必須呼叫 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 方法，來通知這些變更的文字服務。

例如，這是編輯控制項在使用者貼上 "World" 之前的狀態。 插入點是在 \[6, 6\]。

![範例文字資料流圖表](images/coretext/stream-5.png) 使用者執行貼上動作，而編輯控制項最終會以下列文字結束：

![範例文字資料流圖表](images/coretext/stream-4.png) 發生這種情況時，您應該使用下列引數呼叫 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)：

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

隨後會有一或多個 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件，您可在其中處理更新文字服務正在使用的文字。

### 覆寫文字更新

在編輯控制項中，您可能想要覆寫文字更新，以提供自動校正功能。

例如，假設有個編輯控制項可提供將縮寫形式化的校正功能。 這是編輯控制項在使用者輸入空格鍵來觸發校正功能之前的狀態。 插入點是在 \[3, 3\]。

![範例文字資料流圖表](images/coretext/stream-6.png) 使用者按下空格鍵，並引發對應的 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件。 編輯控制項接受文字更新。 這是編輯控制項在完成校正之前短暫的狀態。 插入點是在 \[4, 4\]。

![範例文字資料流圖表](images/coretext/stream-7.png) 在 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件處理常式以外，編輯控制項會進行下列校正。 這是編輯控制項在完成校正之後的狀態。 插入點是在 \[5, 5\]。

![範例文字資料流圖表](images/coretext/stream-8.png) 發生這種情況時，您應該使用下列引數呼叫 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)：

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

隨後會有一或多個 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件，您可在其中處理更新文字服務正在使用的文字。

### 提供要求的文字

請務必讓文字服務具備正確的文字，以提供像是自動校正或預測的功能，特別是已經存在於編輯控制項的文字，例如，來自載入文件或編輯控制項插入的文字，如先前小節所述。 因此，每當引發 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件時，您就必須針對指定的範圍提供目前位於編輯控制項中的文字。

有時 [**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958221) 中的 [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958227) 會指定您的編輯控制項無法依原樣容納的範圍。 例如，**Range** 大於 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件當時的編輯控制項大小，或者 **Range** 的結尾已超出範圍。 在這些情況下，您應該傳回任何合理的範圍，通常是要求範圍的子集。

## 相關文章


**封存範例**
* [XAML 文字編輯範例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 







<!--HONumber=Jun16_HO5-->


