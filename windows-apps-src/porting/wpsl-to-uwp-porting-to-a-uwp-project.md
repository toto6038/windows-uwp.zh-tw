---
description: 您將在 Visual Studio 中建立新的 Windows 10 專案，然後將您的檔案複製到此專案中，以開始移植程序。
title: 將 Windows Phone Silverlight 專案移植到 UWP 專案
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73f90cb52f0bb5756c4e9b67783dbfd286289377
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167452"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>將 Windows Phone Silverlight 專案移植到 UWP 專案


前一個主題是[命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)。

您將在 Visual Studio 中建立新的 Windows 10 專案，然後將您的檔案複製到此專案中，以開始移植程序。

## <a name="create-the-project-and-copy-files-to-it"></a>建立專案並將檔案複製到其中

1.  啟動 Microsoft Visual Studio 2015，並建立新的空白應用程式 (Windows 通用) 專案。 如需詳細資訊，請參閱 [使用 (c #、c + +、Visual Basic) 的範本來開始 Windows 執行階段8.x 應用程式 ](/previous-versions/windows/apps/hh768232(v=win.10))。 新專案建置的應用程式套件 (appx 檔案) 將在所有裝置系列執行。
2.  在 Windows Phone Silverlight 應用程式專案中，找出您想要重複使用的所有原始程式碼檔案及視覺資產檔案。 使用 [檔案總管]，將資料模型、檢視模型、視覺資產、資源字典、資料夾結構，以及任何您想要重複使用的其他項目複製到新專案。 視需要在磁碟上複製或建立子資料夾。
3.  將檢視 (例如 MainPage.xaml 與 MainPage.xaml.cs) 一併複製到新專案節點。 同樣地，請視需要建立新的子資料夾，然後從專案移除現有的檢視。 但是在您覆寫或移除 Visual Studio 產生的檢視之前，請保留一份複本，因為可能稍後可用來供參考。 移植 Windows Phone Silverlight 應用程式的第一個階段著重在美化外觀以及能在裝置系列上運作良好。 稍後，您會將重點放在確認檢視能隨所有尺寸規格適當調整，也可以新增任何調適型程式碼，以充分利用特定的裝置系列。
4.  在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 選取您複製的檔案，在這些檔案上按一下滑鼠右鍵，然後按一下 **\[加入至專案\]**。 這將會自動包含它們的容器資料夾。 然後您可以視需要將 [**顯示所有檔案**] 切換成關閉。 如果您想要的話，也可以選擇替代的工作流程，就是先在 Visual Studio [**方案總管**] 中建立任何必要的子資料夾，然後使用 [**加入現有項目**]命令。 仔細檢查您視覺資產的 **\[建置動作\]** 是否已設定為 **\[內容\]**，而 **\[複製到輸出目錄\]** 是否已設定為 **\[不要複製\]**。
5.  在這個階段，命名空間和類別名稱的差異將會產生大量的建置錯誤。 例如，如果您開啟 Visual Studio 產生的檢視，您會看到它們的類型為 [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page)，而非 **PhoneApplicationPage**。 有許多 XAML 標記和命令式程式碼差異，在本移植指南接下來的主題中將會有詳細說明。 但是，您只需遵循下列一般步驟，就能快速進行：在 XAML 標記的命名空間前置詞宣告中，將 "clr 命名空間" 變更為 "using";使用 [命名空間和類別](wpsl-to-uwp-namespace-and-class-mappings.md) 對應主題，以及 Visual Studio 的 [ **尋找和取代** ] 命令，對您的原始程式碼進行大量變更 (例如，將 "system.string" 取代為 "app.xaml" ) ;在 Visual Studio 的命令式程式碼編輯器中，請使用內容功能表上的 [ **解決** 和 **組織 using** ] 命令，以取得更具目標的變更。

## <a name="extension-sdks"></a>擴充功能 SDK

您移植之應用程式將會呼叫的大部分通用 Windows 平台 (UWP) API 已經實作在一組稱為通用裝置系列的 API。 但是有些實作於擴充功能 SDK，而且 Visual Studio 只能辨識您的應用程式目標裝置系列或任何您參考的擴充功能 SDK 實作的 API。

如果您收到有關找不到命名空間或型別或成員的編譯錯誤，原因可能在此。 開啟 API 參考文件中的 API 主題，並瀏覽到＜需求＞一節：文中會告訴您實作裝置系列為何。 如果那不是您的目標裝置系列，則您必須擁有該裝置系列之擴充功能 SDK 的參考，您的專案才能使用 API。

按一下 [ **專案** &gt; **加入參考** &gt; **Windows 通用** &gt; **延伸** 模組]，然後選取適當的延伸模組 SDK。 例如，如果您想要呼叫的 Api 僅適用于行動裝置系列，而且是在 10.0. x. x. x. x. x. x 版中引進，則請選取 **UWP 的 Windows Mobile Extensions**。

如此便會將下列參考加入您的專案檔案：

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

名稱與版本號碼會和 SDK 安裝位置中的資料夾相符。 例如，上述資訊符合這個資料夾名稱：

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

除非您的 app 是專為實作 API 的裝置系列所設計，否則必須在呼叫之前，使用 [**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 類別來測試該 API 是否存在 (這稱為調適型程式碼)。 每當您的應用程式執行時都將會評估此條件，但只會將有 API 存在的裝置評估為 true，也才可供呼叫。 務必在檢查通用 API 是否存在之後，才使用擴充功能 SDK 與調適型程式碼。 下一節會提供一些範例。

另請參閱[應用程式套件資訊清單](#the-app-package-manifest)。

## <a name="maximizing-markup-and-code-reuse"></a>儘可能重複使用標記與程式碼

您將發現重構一些和/或新增調適型程式碼 (其會在下面說明)，可讓您儘可能重複使用可跨所有裝置系列運作的標記與程式碼。 以下是更多詳細資料。

-   所有裝置系列都通用的檔案不需要特殊考量。 那些檔案將由所有裝置系列上執行的應用程式使用。 這包括 XAML 標記檔案、命令式原始程式碼檔案以及資產檔案。
-   您的應用程式能夠偵測到執行的裝置系列，並瀏覽到專為該裝置系列設計的檢視。 如需詳細資訊，請參閱[偵測執行您 app 的平台](wpsl-to-uwp-input-and-sensors.md)。
-   有項可能很有用的類似技術，就是在別無他法時，為標記檔案或 **ResourceDictionary** 檔案 (或包含該檔案的資料夾) 指定特殊名稱，如此一來，只有在特定裝置系列執行您的 App 時，才會於執行階段自動載入。 此技術會在 [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) 案例研究中說明。
-   若要使用並非所有裝置系列都適用的功能 (例如，印表機、掃描器或相機按鈕)，您可以撰寫調適型程式碼。 請參閱本主題中[條件式編譯與調適型程式碼](#conditional-compilation-and-adaptive-code)的第三個範例。
-   如果您想支援 Windows Phone Silverlight 和 Windows 10 兩者，則您可以在專案間共用原始程式碼檔案。 以下說明：在 Visual Studio 中，以滑鼠右鍵按一下 **方案總管**中的專案，選取 [ **加入現有專案**]，選取要共用的檔案，然後按一下 [ **新增為連結**]。 將您的原始程式碼檔案儲存在檔案系統上的通用資料夾中，這是連結到那些檔案的專案可看見它們的資料夾，並記得將它們新增到原始檔控制。 如果您可以分解您的必要原始程式碼，讓大部分 (如果非全部) 檔案都能夠在這兩個平台上運作，您就不需要有兩份程式碼。 您可以儘可能將任何平台專屬邏輯，包裝在檔案中條件式編譯指示詞內，或包裝在執行階段條件內 (如有必要)。 請參閱下一節和 [C# 前置處理器指示詞](/dotnet/articles/csharp/language-reference/preprocessor-directives/index)。
-   「可攜式類別庫」是針對二進位等級，而不是原始程式碼等級的重複使用，它支援在 Windows Phone Silverlight 和 Windows 10 app 子集 (.NET Core) 都可用的 .NET API 子集。 「可攜式類別庫」組件與這些平台皆二進位相容。 請使用 Visual Studio 來建立針對「可攜式類別庫」設計的專案。 請參閱[使用可攜式類別庫進行跨平台開發](/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)。

## <a name="conditional-compilation-and-adaptive-code"></a>條件式編譯與調適型程式碼

如果您想要在單一程式碼檔案中支援 Windows Phone Silverlight 與 Windows 10，則您可以這麼做。 如果您在 [專案屬性] 頁面上查看 Windows 10 專案，您會看到專案將 WINDOWS UAP 定義 \_ 為條件式編譯符號。 一般而言，您可以使用下列邏輯來執行條件式編譯。

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

如果您有已在 Windows Phone Silverlight 應用程式與 Windows 執行階段8.x 應用程式之間共用的程式碼，則您可能已經具有如下邏輯的原始程式碼：

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

如果有，且您現在想另外支援 Windows 10，則您也可以那樣做。

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

您可能已經使用條件式編譯來限制對 Windows Phone 硬體返回按鈕的處理。 在 Windows 10 中，返回按鈕事件是通用的概念。 實作於硬體或軟體的返回按鈕都會引發 [**BackRequested**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 事件，因此要處理的就是這個事件。

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

您可能已經使用條件式編譯來限制對 Windows Phone 硬體相機按鈕的處理。 在 Windows 10 中，硬體相機按鈕是行動裝置系列專屬的概念。 因為應用程式套件將在所有裝置上執行，所以我們將編譯階段條件變更為使用調適型程式碼的執行階段條件。 為此，我們使用 [**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 類別查詢執行階段是否有 [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 類別存在。 **HardwareButtons** 定義在行動裝置的擴充功能 SDK，因此我們必須將該 SDK 的參照新增到專案，以供編譯這個程式碼。 不過請注意，處理常式只會在實作行動裝置擴充功能 SDK 中定義的裝置類型上執行，那就是行動裝置系列。 因此，請小心並只將下列程式碼用於存在的功能，雖然它達到效果的方式與條件式編譯不同。

```csharp
       // Note: Cache the value instead of querying it more than once.
        bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
            ("Windows.Phone.UI.Input.HardwareButtons");

        if (isHardwareButtonsAPIPresent)
        {
            Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
                this.HardwareButtons_CameraPressed;
        }

    ...

    private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
    {
        // Handle the event.
    }
```

另請參閱[偵測執行您 app 的平台](wpsl-to-uwp-input-and-sensors.md)。

## <a name="the-app-package-manifest"></a>應用程式套件資訊清單

您專案 (包括任何擴充功能 SDK 參考) 中的設定，會決定您應用程式能呼叫的 API 介面區。 但客戶實際上能夠透過市集安裝您應用程式的裝置集，是由您的應用程式套件資訊清單決定。 如需詳細資訊，請參閱 [**y**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)中的範例。

如何編輯應用程式套件資訊清單相當值得了解，因為後續的主題將討論如何將它用於各種宣告、功能，以及某些功能所需的其他設定。 您可以使用 Visual Studio 應用程式套件資訊清單編輯器來編輯它。 如果未顯示 **方案總管** ，請從 [ **View** ] 功能表中選擇它。 按兩下 **Package.appxmanifest**。 資訊清單編輯器視窗隨即開啟。 選取適當的索引標籤來進行變更，然後儲存變更。 您可能想要確保已移植 app 資訊清單中的 **pm:PhoneIdentity** 元素符合您正在移植之 app 的 app 資訊清單中的元素 (如需詳細資訊，請參閱 [**pm:PhoneIdentity**](/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity) 主題)。

請參閱 [Windows 10 的套件資訊清單結構參考](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)。

下一個主題是[疑難排解](wpsl-to-uwp-troubleshooting.md)。