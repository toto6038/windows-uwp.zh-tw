---
author: martinekuan
title: 在 C++ 中建立基本 Windows 執行階段元件，然後從 JavaScript 或 C\# 呼叫該元件#
description: 本逐步解說示範如何建立可從 JavaScript、C# 或 Visual Basic 呼叫的基本 Windows 執行階段元件 DLL。
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# 逐步解說：在 C++ 中建立基本 Windows 執行階段元件，然後從 JavaScript 或 C 呼叫該元件#


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[正式發行前可能會進行大幅度修改之預先發行的產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

本逐步解說示範如何建立可從 JavaScript、C# 或 Visual Basic 呼叫的基本 Windows 執行階段元件 DLL。 開始本逐步解說之前，請確定您了解一些概念，例如：抽象二進位介面 (ABI)、ref 類別，以及讓 ref 類別更容易使用的 Visual C++ 元件擴充功能。 如需詳細資訊，請參閱[在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 語言參考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。

## 建立 C++ 元件 DLL


此範例會先建立元件專案，但您也可以先建立 JavaScript 專案。 順序並不重要。

請注意，元件的主要類別包含屬性和方法定義的範例，以及事件宣告。 提供這些項目只是為了示範它的進行方式。 這些並不是必要項目，而且在此範例中，我們會以自己的程式碼來取代所有產生的程式碼。

## **建立 C++ 元件專案**

1.  在 Visual Studio 功能表列上，依序選擇 [檔案]、[新增] 及 [專案]****。
2.  在 [**新增專案**] 對話方塊的左窗格中，展開 [**Visual C++**]，然後選取通用 Windows app 的節點。
3.  在中央窗格，選取 [**Windows 執行階段元件**]，然後將專案命名為 WinRT\_CPP。
4.  選擇 [確定]**** 按鈕。

## **將可啟用的類別加入至元件**

1.  可啟用的類別是用戶端程式碼可以使用 **new** 運算式 (在 Visual Basic 中是 **New**，在 C++ 中則是 **ref new**) 建立的類別。 在您的元件中，您會將它宣告為 **public ref class sealed**。 事實上，Class1.h 和 .cpp 檔案已經有 ref 類別。 您可以變更名稱，但在這個範例中，我們會使用預設名稱 -- Class1。 如有必要，您可以在元件中定義其他 ref 類別或一般類別。 如需 ref 類別的詳細資訊，請參閱[類型系統 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx)。

2.  將下列 \#include 指示詞加入至 Class1.h：

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## 執行應用程式


在 [方案總管] 中開啟專案節點的捷徑功能表，然後選擇 [設定為啟始專案]****，選取 C# 專案或 JavaScript 專案做為啟始專案。 然後，按 F5 開始執行並偵錯，或是按 Ctrl+F5 開始執行而不偵錯。

## 在物件瀏覽器中檢查您的元件 (選擇性)


在 [物件瀏覽器] 中，您可以檢查 .winmd 檔案中定義的所有 Windows 執行階段類型。 這包含 Platform 命名空間和預設命名空間中的類型。 不過，由於 Platform::Collections 命名空間中的類型是定義於標頭檔 collections.h (而非 winmd 檔案) 中，因此它們不會出現在 [物件瀏覽器] 中。

## **檢查元件**

1.  在功能表列上，依序選擇 **[檢視] 和 [物件瀏覽器]** (Ctrl+Alt+J)。
2.  在 [物件瀏覽器] 的左窗格中，展開 [WinRT\_CPP] 節點，以顯示在您的元件中定義的類型和方法。

## 偵錯提示


若想獲得較佳的偵錯經驗，請從公用 Microsoft 符號伺服器下載偵錯符號：

## **下載偵錯符號**

1.  在功能表列上，依序選擇 [工具] 和 [選項]****。
2.  在 [選項]**** 對話方塊中，展開 [偵錯]****，然後選取 [符號]****。
3.  選取 [Microsoft 符號伺服器]****，然後選擇 [確定]**** 按鈕。

第一次下載這些符號時可能需要一些時間。 若要縮短下載時間，當您下次按 F5 時，請指定用來快取符號的本機目錄。

對含有元件 DLL 的 JavaScript 方案進行偵錯時，您可以設定偵錯工具以啟用逐步執行指令碼或逐步執行元件中的機器碼，但不可兩者同時啟用。 若要變更設定，請在 [方案總管] 中開啟 JavaScript 專案節點的捷徑功能表，然後依序選擇 [屬性]、[偵錯] 及 [偵錯工具類型]****。

請務必在封裝設計工具中選取適當的功能。 您可以藉由開啟 Package.appxmanifest 檔案來開啟封裝設計工具。 例如，如果您嘗試以程式設計方式存取 [圖片] 資料夾中的檔案，請務必在封裝設計工具中的 [功能]**** 窗格中選取 [圖片庫]**** 核取方塊 。

如果 JavaScript 程式碼無法辨識元件中的公用屬性或方法，請確定您在 JavaScript 中使用 Camel 命名法的大小寫慣例。 例如，`ComputeResult` C++ 方法必須當做 JavaScript 中的 `computeResult` 來參考。

如果從方案中移除 C++ Windows 執行階段元件專案，也必須手動從 JavaScript 專案中移除該專案的參考。 若未執行此動作，後續的偵錯或建置作業將無法執行。 之後如有必要，您可以加入 DLL 的組件參考。

## 相關主題

* [在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=May16_HO2-->


