---
title: 開始使用 Windows UI 程式庫
description: 如何安裝及使用 Windows UI 程式庫。
ms.topic: reference
ms.date: 05/08/2020
keywords: windows 10, uwp, 工具組 sdk
ms.openlocfilehash: d96efb2f3de3084d74e06e70ff2811a944604f56
ms.sourcegitcommit: 47899c30a39087bca1f058a4395cf58daacf5ae9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85345472"
---
# <a name="getting-started-with-the-windows-ui-library"></a>開始使用 Windows UI 程式庫

[WinUI 2.4](release-notes/winui-2.4.md) 是 WinUI 的最新穩定版本，生產環境中的應用程式應使用此版本。

程式庫會以 NuGet 套件的形式來提供，可供新增至任何新的或現有的 Visual Studio 專案。

> [!NOTE]
> 如需有關試用 WinUI 3.0 早期預覽版的詳細資訊，請參閱 [WinUI 3.0 Preview 1](../winui3/index.md)。

## <a name="download-and-install-the-windows-ui-library"></a>下載並安裝 Windows UI 程式庫

1. 下載 [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)，並務必在 Visual Studio 安裝程式中選擇 [通用 Windows 平台開發] 工作負載。

2. 開啟現有專案，或使用 [Visual C#] -> [Windows] -> [通用] 底下的 [空白應用程式] 範本建立新專案 (也可以使用適用於您語言投影的適當範本來建立)。  

    > [!IMPORTANT]
    > 若要使用 WinUI 2.4，您必須在專案屬性中設定 TargetPlatformVersion >= 10.0.18362.0 和 TargetPlatformMinVersion >= 10.0.15063.0。

3. 在 [方案總管] 面板中，以滑鼠右鍵按一下專案名稱，然後選取 [管理 NuGet 套件]。 選取 [瀏覽] 索引標籤，然後搜尋 **Microsoft.UI.Xaml** 或 **WinUI**。 然後選擇您要使用的 [[Windows UI 程式庫 NuGet 套件]](nuget-packages.md)。
**Microsoft.UI.Xaml** 套件包含適用於所有應用程式的 Fluent 控制項和功能。  
您可以選擇性地核取 [包含發行前版本]，以查看包含實驗性新功能的最新發行前版本。

    ![NuGet 套件](images/ManageNugetPackages.png "管理 NuGet 套件映像")

    ![NuGet 套件](images/NugetPackages.png)

4. 將 Windows UI (WinUI) 佈景主題資源新增至 App.xaml 資源。 視您是否有額外的應用程式資源而定，您可以透過兩種方式來執行此動作。

    a. 如果您沒有其他應用程式資源，請將 `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` 新增至您的 Application.Resources：

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. 否則，如果您有多組應用程式資源，則請將 `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` 新增至 Application.Resources.MergedDictionaries：

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > 資源新增至 ResourceDictionary 的順序會影響其套用順序。 `XamlControlsResources` 字典會覆寫許多預設的資源索引鍵，因此應先將其新增至 `Application.Resources`，以免其覆寫您應用程式中的任何其他自訂樣式或資源。 如需如何載入資源的詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。

5. 將工具組的參考新增至 XAML 頁面和程式碼後置頁面。

    * 在 XAML 頁面中，於頁面頂端新增參考

        ```xaml
        xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
        ```

    * 在您的程式碼中 (如果您想要使用類型名稱，而不加以限定)，您可以新增 using 指示詞。

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>C++/WinRT 專案的其他步驟

當您將 NuGet 套件新增至 C++/WinRT 專案時，工具會在專案的 `\Generated Files\winrt` 資料夾中產生一組投影標頭。 若要將這些標頭檔帶入專案中，讓這些新類型的參考可以解析，您可以移至先行編譯標頭檔 (通常是 `pch.h`) 並納入這些類型。 以下範例包含為 **Microsoft.UI.Xaml** 套件所產生的標頭檔。

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

如需如何將 Windows UI 程式庫的簡單支援新增至 C++/WinRT 專案的完整逐步解說，請參閱 [C++/WinRT Windows UI 程式庫簡單範例](/windows/uwp/cpp-and-winrt-apis/simple-winui-example)。

## <a name="contributing-to-the-windows-ui-library"></a>參與 Windows UI 程式庫

WinUI 是裝載於 GitHub 上的開放原始碼專案。

我們歡迎您在 [Windows UI 程式庫存放庫](https://aka.ms/winui)中提出錯誤回報、功能要求和社群程式碼。

## <a name="other-resources"></a>其他資源

如果您不熟悉 UWP，建議您造訪開發人員入口網站上的 [UWP 開發入門](https://developer.microsoft.com/windows/getstarted)頁面。
