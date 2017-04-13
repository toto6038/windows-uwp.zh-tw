---
author: msatranjr
title: "發佈 Managed Windows 執行階段元件"
description: "您可以透過檔案複製來發佈自己的 Windows 執行階段元件。"
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 70ef1ab7bc31fde2f0d4744394c1ae69c8caf7fd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="distributing-a-managed-windows-runtime-component"></a>發佈 Managed Windows 執行階段元件


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

您可以透過檔案複製來發佈自己的 Windows 執行階段元件。 不過，如果元件包含許多檔案，使用者就必須等待冗長的安裝過程。 此外，放置檔案時發生的錯誤或參考設定失敗都可能會造成他們的問題。 您可以將複雜元件封裝成 Visual Studio 擴充功能 SDK，方便安裝與使用。 使用者只需為整個封裝設定一個參考。 如 MSDN Library 中的[尋找及使用 Visual Studio 擴充功能](https://msdn.microsoft.com/library/vstudio/dd293638.aspx)所述，使用者可以透過 **\[擴充功能和更新\]** 對話方塊輕鬆地尋找並安裝您的元件。

## <a name="planning-a-distributable-windows-runtime-component"></a>規劃可發佈的 Windows 執行階段元件

為二進位檔案 (例如您的 .winmd 檔案) 選擇唯一的名稱。 建議您遵循下列格式，以確保名稱的唯一性：

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

您的二進位檔案將安裝於 app 封裝中，可能會與其他開發人員的二進位檔案放在一起。 請參閱 MSDN Library 上[做法：建立軟體開發套件](https://msdn.microsoft.com/library/hh768146.aspx)中的＜擴充功能 SDK＞一節。

請先考慮元件的複雜性，再決定其發佈方式。 符合下列條件時，建議使用擴充功能 SDK 或類似的封裝管理員：

-   您的元件包含多個檔案。
-   您為多個平台 (例如 x86 和 ARM) 提供不同的元件版本。
-   您同時提供元件的偵錯與發行版本。
-   您的元件包含只能在設計階段使用的檔案和組件。

如果符合上述多個條件，擴充功能 SDK 就特別有用。

> **注意**  如果是複雜元件，NuGet 封裝管理系統可提供替代性開放原始碼來取代擴充功能 SDK。 與擴充功能 SDK 一樣，NuGet 可讓您建立封裝來簡化複雜元件的安裝作業。 如需 NuGet 封裝和 Visual Studio 擴充功能 SDK 的比較，請參閱 MSDN Library 中的[使用 NuGet 和擴充功能 SDK 兩種方式新增參考](https://msdn.microsoft.com/library/jj161096.aspx)。

## <a name="distribution-by-file-copy"></a>藉由檔案複製發佈

如果元件是由單一 .winmd 檔案組成，或是由 .winmd 檔案和資源索引 (.pri) 檔案組成，您只需讓使用者複製 .winmd 檔案即可。 使用者可以將檔案放在專案中的任何地方、使用 **\[加入現有項目\]** 對話方塊來將 .winmd 檔案加入專案，然後使用 [參考管理員] 對話方塊來建立參考。 如果您包含 .pri 檔案或 .xml 檔案，請告知使用者將這些檔案與 .winmd 檔案放在一起。

> **注意**  當您建置 Windows 執行階段元件時，Visual Studio 一律會產生 .pri 檔案，即使專案不含任何資源也一樣。 如果您有適用於元件的測試 app，則可在 bin\debug\AppX 資料夾中檢查 app 封裝的內容，以判斷是否使用了 .pri 檔案。 如果來自元件的 .pri 檔案並未出現在其中，則您就不需發佈該元件。 或者，您也可以使用 [MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx) 工具，傾印來自於 Windows 執行階段元件專案的資源檔。 例如，您可以在 Visual Studio 命令提示字元視窗中輸入： makepri dump /if MyComponent.pri /of MyComponent.pri.xml 如需 .pri 檔案的詳細資訊，請參閱[資源管理系統 (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)。

## <a name="distribution-by-extension-sdk"></a>藉由擴充功能 SDK 發佈

複雜元件通常包括 Windows 資源，但請參閱上一節中關於偵測空白 .pri 檔案的注意事項。

**建立擴充功能 SDK**

1.  確定您已安裝 Visual Studio SDK。 您可以從 [Visual Studio 下載](https://www.visualstudio.com/downloads/download-visual-studio-vs)頁面下載 Visual Studio SDK。
2.  使用 VSIX 專案範本建立新專案。 您可以在 Visual C# 或 Visual Basic 底下的 [擴充性] 分類中找到此範本。 此範本是隨著 Visual Studio SDK 一起安裝。 ([逐步解說：使用 C# 或 Visual Basic 建立 SDK](https://msdn.microsoft.com/library/jj127119.aspx) 或[逐步解說：使用 C++ 建立 SDK](https://msdn.microsoft.com/library/jj127117.aspx) 會透過非常簡單的案例，來示範此範本的使用方式。 )
3.  判斷 SDK 的資料夾結構。 資料夾結構的開頭位於 VSIX 專案的根層級，並且包含 **References**、**Redist** 及 **DesignTime** 資料夾。

    -   **References** 是二進位檔案的位置，您的使用者可以針對這些檔案進行程式設計。 擴充功能 SDK 會在使用者的 Visual Studio 專案中建立這些檔案的參考。
    -   **Redist** 是必須與二進位檔案一同發佈之其他檔案的位置，位於使用您元件所建立的 app 中。
    -   **DesignTime** 是只有當開發人員在建立會用到您元件的 app 時所使用之檔案的位置。

    您可以在上述每一個資料夾中建立組態資料夾。 可以使用的名稱包含 debug、retail 和 CommonConfiguration。 CommonConfiguration 資料夾是用來存放零售或偵錯組建所使用的相同檔案。 如果您只發佈元件的零售組建，就能將所有檔案放在 CommonConfiguration 中，然後省略另外兩個資料夾。

    在每個組態資料夾中，您可以針對平台特定的檔案提供架構資料夾。 如果您針對所有平台使用相同的檔案，則可提供名為 neutral 的單一資料夾。 如需資料夾結構的詳細資訊 (包括其他架構資料夾名稱)，請參閱 MSDN Library 中的[做法：建立軟體開發套件](https://msdn.microsoft.com/library/hh768146.aspx)。 (該文章將討論平台 SDK 和擴充功能 SDK。 您可能發現摺疊關於平台 SDK 的章節，可有效避免混淆。 )

4.  建立 SDK 資訊清單檔案。 資訊清單會指定名稱和版本資訊、SDK 支援的架構、.NET Framework 版本，以及 Visual Studio 如何使用 SDK 等其他相關資訊。 如需詳細資訊和範例，請參閱[做法：建立軟體開發套件](https://msdn.microsoft.com/library/hh768146.aspx)。
5.  建置和發佈擴充功能 SDK。 如需包括當地語系化和簽署 VSIX 封裝在內的詳細資訊，請參閱 MSDN Library 中的＜VSIX 部署＞。

## <a name="related-topics"></a>相關主題

* [建立軟體開發套件](https://msdn.microsoft.com/library/hh768146.aspx)
* [NuGet 封裝管理系統](https://github.com/NuGet/Home)
* [資源管理系統 (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)
* [尋找及使用 Visual Studio 擴充功能](https://msdn.microsoft.com/library/dd293638.aspx)
* [MakePRI.exe 命令選項](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx)
