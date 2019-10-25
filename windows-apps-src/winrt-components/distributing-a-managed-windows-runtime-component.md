---
title: 散發 managed Windows 執行階段元件
description: 您可以透過檔案複製來散發您的 Windows 執行階段元件。
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690390"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>散發 managed Windows 執行階段元件

您可以透過檔案複製來散發您的 Windows 執行階段元件。 不過，如果您的元件是由許多檔案所組成，則對您的使用者而言，安裝可能會很繁瑣。 此外，放置檔案或無法設定參考的錯誤，可能會對它們造成問題。 您可以將複雜元件封裝為 Visual Studio 擴充功能 SDK，以方便安裝和使用。 使用者只需要為整個封裝設定一個參考。 他們可以使用 [**擴充功能和更新**] 對話方塊輕鬆地尋找並安裝元件，如[尋找和使用 Visual Studio 延伸](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)模組中所述。

## <a name="planning-a-distributable-windows-runtime-component"></a>規劃可散發 Windows 執行階段元件

為二進位檔案選擇唯一的名稱，例如您的 winmd 檔案。 我們建議採用下列格式以確保唯一性：

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

您的二進位檔案會安裝在應用程式套件中，可能會有其他開發人員的二進位檔案。 請參閱 how [to：建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中的「延伸模組 sdk」。

若要決定如何散發您的元件，請考慮它的複雜程度。 在下列情況中，建議使用延伸模組 SDK 或類似的套件管理員：

-   您的元件包含多個檔案。
-   您可以為多個平臺（例如 x86 和 ARM）提供元件的版本。
-   您可以提供元件的「偵錯工具」和「發行」版本。
-   您的元件具有只在設計階段使用的檔案和元件。

如果上述多個條件成立，延伸模組 SDK 就特別有用。

> **請注意**  針對複雜元件，NuGet 套件管理系統提供延伸模組 sdk 的開放原始碼替代方案。 如同延伸模組 Sdk，NuGet 可讓您建立封裝，以簡化複雜元件的安裝。 如需 NuGet 封裝和 Visual Studio 擴充功能 Sdk 的比較，請參閱[使用 Nuget 與擴充功能 sdk 來新增參考](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015)。

## <a name="distribution-by-file-copy"></a>依檔案複製散發

如果您的元件是由單一 winmd 檔案或 winmd 檔案和資源索引（pri）檔案所組成，您可以直接將 winmd 檔案提供給使用者進行複製。 使用者可以將檔案放在專案中的任何位置，使用 [**加入現有專案**] 對話方塊將 winmd 檔案加入至專案，然後使用 [參考管理員] 對話方塊來建立參考。 如果您包含一個 pri 檔案或 .xml 檔案，請指示使用者將這些檔案與 winmd 檔案放在一起。

> **請注意**，當您建立 Windows 執行階段元件時，即使您的專案未包含任何資源，Visual Studio 一律會產生一個 pri 檔案  。 如果您有元件的測試應用程式，您可以藉由檢查 bin 中的應用程式套件內容，\\debug\\AppX 資料夾來判斷是否使用 pri 檔案。 如果元件中的 pri 檔案未出現在該處，則不需要將它散發。 或者，您可以使用[MakePRI](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))來傾印 Windows 執行階段元件專案中的資源檔。 例如，在 Visual Studio 命令提示字元 視窗中，輸入： makepri dump/if Mycomponent 之下，您可以在[資源管理系統（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))中閱讀更多有關 pri 檔案的資訊。

## <a name="distribution-by-extension-sdk"></a>延伸模組 SDK 的散發

複雜的元件通常包含 Windows 資源，但請參閱上一節中關於偵測空的 pri 檔案的注意事項。

**建立擴充功能 SDK**

1.  請確定您已安裝 Visual Studio SDK。 您可以從[Visual Studio 下載](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) 頁面下載 Visual Studio SDK。
2.  使用 VSIX 專案範本建立新的專案。 您可以在 [擴充性] C#分類中的 [視覺效果] 或 [Visual Basic] 底下找到範本。 此範本會安裝為 Visual Studio SDK 的一部分。 （[逐步解說： C#使用或 Visual Basic 建立 sdk](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015)或[逐步解說：使用C++建立 sdk ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)，示範如何在非常簡單的案例中使用此範本。 )
3.  判斷 SDK 的資料夾結構。 資料夾結構會從 VSIX 專案的根層級開始，其中包含**參考**、可轉散發**套件和** **DesignTime**資料夾。

    -   **參考**是您的使用者可以對其進行程式設計的二進位檔案位置。 延伸模組 SDK 會在使用者的 Visual Studio 專案中建立這些檔案的參考。
    -   **可轉散發套件是在**使用您的元件所建立的應用程式中，必須與您的二進位檔案一起散發之其他檔案的位置。
    -   **DesignTime**是只有在開發人員建立使用您的元件的應用程式時，才會使用的檔案位置。

    在上述每個資料夾中，您都可以建立設定資料夾。 允許的名稱為 debug、retail 和 CommonConfiguration。 CommonConfiguration 資料夾適用于零售或 debug 組建所使用的相同檔案。 如果您只發佈元件的零售組建，您可以將所有專案放在 CommonConfiguration 中，並省略其他兩個資料夾。

    在每個設定資料夾中，您可以提供平臺特定檔案的架構資料夾。 如果您針對所有平臺都使用相同的檔案，您可以提供名為「中性」的單一資料夾。 您可以在[如何：建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中找到資料夾結構的詳細資料，包括其他架構資料夾名稱。 （該文章討論平臺 Sdk 和擴充功能 Sdk。 您可能會發現，折迭平臺 Sdk 的相關章節會很有用，以避免混淆。 )

4.  建立 SDK 資訊清單檔案。 資訊清單會指定名稱和版本資訊、SDK 支援的架構、.NET 版本，以及 Visual Studio 使用 SDK 之方式的其他相關資訊。 您可以在[如何：建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中找到詳細資料和範例。
5.  建立並散發延伸模組 SDK。 如需深入資訊，包括當地語系化和簽署 VSIX 封裝，請參閱[Vsix 部署](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015)。

## <a name="related-topics"></a>相關主題

* [建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [NuGet 套件管理系統](https://github.com/NuGet/Home)
* [資源管理系統（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [尋找和使用 Visual Studio 延伸模組](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [MakePRI .exe 命令選項](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
