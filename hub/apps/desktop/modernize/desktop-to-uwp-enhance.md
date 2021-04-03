---
description: 使用 Windows 執行階段 API 增強適用於 Windows 10 使用者的傳統型應用程式。
title: 在傳統型應用程式中呼叫 Windows 執行階段 API
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 754cc0d4d230172ec8fc6b1ee7c253e10404c370
ms.sourcegitcommit: 86630e2163a87f6d6e02db9598a3e43f2d227cb6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2021
ms.locfileid: "106272988"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>在傳統型應用程式中呼叫 Windows 執行階段 API

您可以使用通用 Windows 平台 (UWP) API，在您的傳統型應用程式中加入讓 Windows 10 使用者驚艷的現代化體驗。

首先，使用必要的參考來設定您的專案。 然後，從您的程式碼呼叫 Windows 執行階段 API，以將 Windows 10 體驗加入您的傳統型應用程式。 您可以針對 Windows 10 使用者另行建置，也可以散發相同的二進位碼給所有使用者，無論他們執行什麼 Windows 版本。

某些 Windows 執行階段 API 僅在具有[套件識別資料](modernize-packaged-apps.md)的傳統型應用程式中才支援。 如需詳細資訊，請參閱[可用的 Windows 執行階段 API](desktop-to-uwp-supported-api.md)。

## <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>修改 .NET 專案以使用 Windows 執行階段 API

.NET 專案有數個選項：

* 從 .NET 5 開始，您可以在專案檔中新增目標 Framework 標記 (TFM) ，以存取 WinRT Api。 將 Windows 10 版本 1809 或更新版本設為目標的專案支援此選項。
* 若為舊版 .NET，您可以安裝 `Microsoft.Windows.SDK.Contracts` NuGet 套件，以將所有必要參考新增至您的專案。 將 Windows 10 版本 1803 或更新版本設為目標的專案支援此選項。
* 如果您的專案多目標 .NET 5 (或更新版本) 和較早的 .NET 版本，您可以將專案檔設定為使用這兩個選項。

### <a name="net-5-use-the-target-framework-moniker-option"></a>.NET 5：使用目標 Framework 標記選項

只有使用 .NET 5 (或更新版本) 和目標 Windows 10 版本1809或更新版本的 OS 版本的專案才支援這個選項。 如需更多關於此案例的背景資訊，請參閱[這篇部落格文章](https://blogs.windows.com/windowsdeveloper/2020/09/03/calling-windows-apis-in-net5/)。

1. 在 Visual Studio 中開啟您的專案，在 [方案總管] 中您的專案上按一下滑鼠右鍵，然後選擇 [編輯專案檔]。 您的專案檔看起來應該類似這樣。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. 將 **TargetFramework** 元素的值取代為下列其中一個字串：

    * **net5.0-windows10.0.17763.0**：如果您的應用程式將 Windows 10 版本 1809 設為目標，請使用此值。
    * **net5.0-windows10.0.18362.0**：如果您的應用程式將 Windows 10 版本 1903 設為目標，請使用此值。
    * **net5.0-windows10.0.19041.0**：如果您的應用程式將 Windows 10 版本 2004 設為目標，請使用此值。

    例如，下列元素適用於將 Windows 10 版本 2004 設為目標的專案。

    ```xml
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    ```

3. 儲存您的變更並關閉專案檔。

### <a name="earlier-versions-of-net-install-the-microsoftwindowssdkcontracts-nuget-package"></a>舊版 .NET：安裝 Microsoft.Windows.SDK.Contracts NuGet 套件

如果您的應用程式使用 .NET Core 3.x 或 .NET Framework，請使用此選項。 將 Windows 10 版本 1803 或更新作業系統版本設為目標的專案支援此選項。

1. 請確定已啟用[套件參考](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 [工具] -> [NuGet 套件管理員]-> [套件管理員設定]。
    2. 確定已針對 [預設套件管理格式] 選取 [PackageReference]。

2. 在 Visual Studio 中開啟您的專案，在 [方案總管] 中您的專案上按一下滑鼠右鍵，然後選擇 [管理 NuGet 套件]。

3. 在 [NuGet 套件管理員] 視窗中，選取 [瀏覽] 索引標籤，然後搜尋 `Microsoft.Windows.SDK.Contracts`。

4. 找到 `Microsoft.Windows.SDK.Contracts` 套件之後，請在 [NuGet 套件管理員] 的右窗格中，根據您想要設為目標的 Windows 10 版本，選取您要安裝的套件 [版本]：

    * **10.0.19041.xxxx**：針對 Windows 10 版本 2004 選擇此版本。
    * **10.0.18362.xxxx**：針對 Windows 10 版本 1903 選擇此版本。
    * **10.0.17763.xxxx**：針對 Windows 10 版本 1809 選擇此版本。
    * **10.0.17134.xxxx**：針對 Windows 10 版本 1803 選擇此版本。

5. 按一下 [安裝]。

### <a name="configure-projects-that-multi-target-different-versions-of-net"></a>設定將不同版本 .NET 設為多重目標的專案

如果您的專案多目標 .NET 5 (或更新版本) 和較早的版本 (包括 .NET Core 3.x 和 .NET Framework) ，您可以設定專案檔，以使用目標 Framework 標記來自動提取 .NET 5 的 WinRT API 參考，並 `Microsoft.Windows.SDK.Contracts` 針對較早的版本使用 NuGet 套件。

1. 在 Visual Studio 中開啟您的專案，在 [方案總管] 中您的專案上按一下滑鼠右鍵，然後選擇 [編輯專案檔]。 下列範例示範應用程式的專案檔，該應用程式使用 .NET Core 3.1。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. 將檔案中的 **TargetFramework** 元素取代為 **TargetFrameworks** 元素 (請注意複數)。 在此元素中，為您想要設為目標的所有 .NET 版本指定目標 Framework Moniker，以分號區隔。 

    * 若是 .NET 5 或更新版本，請使用下列其中一個目標 Framework 名字：
        * **net5.0-windows10.0.17763.0**：如果您的應用程式將 Windows 10 版本 1809 設為目標，請使用此值。
        * **net5.0-windows10.0.18362.0**：如果您的應用程式將 Windows 10 版本 1903 設為目標，請使用此值。
        * **net5.0-windows10.0.19041.0**：如果您的應用程式將 Windows 10 版本 2004 設為目標，請使用此值。
    * 若為 .NET Core 3.x，請使用 **netcoreapp3.0** 或 **netcoreapp3.1**。
    * 若為 .NET Framework，請使用 **net46**。

    下列範例示範如何針對 Windows 10 2004 版) ，以多目標為 .NET Core 3.1 和 .NET 5 (。

    ```xml
    <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
    ```

3. 在 **PropertyGroup** 元素後面，新增 **PackageReference** 專案，其中包含的條件陳述式會針對您應用程式設為目標的任何 .NET Core 3.x 或 .NET Framework 版本安裝 `Microsoft.Windows.SDK.Contracts` NuGet 套件。 **PackageReference** 元素必須是 **ItemGroup** 元素的子項。 以下範例示範 .NET Core 3.1 的操作方法。

    ```xml
    <ItemGroup>
      <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                        Include="Microsoft.Windows.SDK.Contracts"
                        Version="10.0.19041.0" />
    </ItemGroup>
    ```

    完成時，您的專案檔看起來應該類似這樣。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
        <UseWPF>true</UseWPF>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                         Include="Microsoft.Windows.SDK.Contracts"
                         Version="10.0.19041.0" />
      </ItemGroup>
    </Project>
    ```

4. 儲存您的變更並關閉專案檔。

## <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>修改 C++ Win32 專案以使用 Windows 執行階段 API

使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) 來取用 Windows 執行階段 API。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。

設定專案使其適用於 C++/WinRT：

* 針對新專案，您可以安裝 [C++/WinRT Visual Studio 延伸模組 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)，並使用該延伸模組隨附的其中一個 C++/WinRT 專案範本。
* 針對現有的專案，您可以在專案中安裝 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件。

如需這些選項的詳細資訊，請參閱[本文章](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-windows-10-experiences"></a>新增 Windows 10 體驗

現在，您可以新增當使用者在 Windows 10 上執行您的應用程式時提供的現代化體驗了。 請使用以下設計流程。

:white_check_mark:**首先，決定您想要新增的體驗**

您有許多選擇。 例如，您可以使用[創造營收 API](/windows/uwp/monetize) 簡化採購訂單流程，或透過分享一些有趣事物來[吸引別人關注您的應用程式](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)，例如另一位使用者張貼的新圖片。

![快顯通知](images/desktop-to-uwp/toast.png)

即使使用者忽略或關閉您的訊息，他們仍可以在控制中心再次看到這些訊息，按一下即可開啟您的應用程式。 如此可提高使用者與您應用程式的互動程度，並獲得您的應用程式與作業系統深度整合的額外好處。 本文稍後會顯示該體驗的程式碼。

請造訪 [UWP 文件](/windows/uwp/get-started/)看看更多的構想。

:white_check_mark:**決定要增強或擴充**

您會經常聽到我們使用「增強」、「擴充」這些詞彙，所以我們想花一點時間來解釋這些詞彙到底是什麼意思。

我們使用「增強」一詞來描述可以直接從您的傳統型應用程式呼叫的 Windows 執行階段 API (無論您是否選擇在 MSIX 套件中封裝您的應用程式)。 當您選擇 Windows 10 體驗後，找出您需要建立的 API，然後查看該 API 是否出現在[此清單](desktop-to-uwp-supported-api.md)中。 這個清單列出您可以直接從您的傳統型應用程式呼叫的 API。 如果您的 API 未顯示在此清單中，這是因為與該 API 相關的功能只能在 UWP 程序中執行。 很多時候，這包括轉譯 UWP XAML 的 API，例如 UWP 地圖控制項或 Windows Hello 安全性提示。

> [!NOTE]
> 雖然轉譯 UWP XAML 的 API 通常無法直接從您的傳統型應用程式呼叫，但您還是可以使用替代做法。 如果您想要裝載 UWP XAML 控制項或其他自訂視覺效果體驗，可以使用 [XAML Islands](xaml-islands.md) (從 Windows 10 版本1903 開始) 和[視覺圖層](visual-layer-in-desktop-apps.md) (從 Windows 10 版本 1803 開始)。 這些功能可以在已封裝或未封裝的傳統型應用程式中使用。

如果您已選擇將您的傳統型應用程式封裝在 MSIX 套件中，另一個選項是將 UWP 專案新增至您的方案，藉此「擴充」應用程式。 桌面專案仍是您應用程式的進入點，但 UWP 專案可讓您存取未出現在[此清單](desktop-to-uwp-supported-api.md)中的所有 API。 傳統型應用程式可以透過使用應用程式服務來與 UWP 程序通訊，而且我們提供大量關於如何設定的指導方針。 如果您想要新增需要 UWP 專案的體驗，請參閱[透過 UWP 元件擴充](desktop-to-uwp-extend.md)。

:white_check_mark:**參考 API 協定**

如果您可以直接從傳統型應用程式呼叫 API，請開啟瀏覽器並搜尋該 API 的參考主題。
在 API 摘要的下方，您可以找到描述其 API 協定的表格。 以下是表格範例：

![API 協定表格](images/desktop-to-uwp/contract-table.png)

如果您有 .NET 傳統型應用程式，請新增指向該 API 協定的參考，然後將該檔案的 [複製本機] 屬性設定為 [False]。 如果您有 C++ 專案，請將包含此協定之資料夾的路徑新增至 [其他 Include 目錄]。

:white_check_mark:**呼叫 API 以新增您的體驗**

以下是我們之前討論過的，您要用來顯示通知視窗的程式碼。 這些 API 出現在[此清單](desktop-to-uwp-supported-api.md)中，因此您可以將此程式碼新增到您的傳統型應用程式並立即執行。

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

若要深入了解通知，請參閱[調適型和互動式快顯通知](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)。

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>支援 Windows XP、Windows Vista 和 Windows 7/8 安裝基礎

您不需要建立新的分支並維護個別程式碼基底，即可針對 Windows 10 現代化您的應用程式。

如果您想為 Windows 10 使用者建置不同的二進位檔，請使用條件式編譯。 如果您傾向組建一組二進位檔然後部署到所有 Windows 使用者，請使用執行階段檢查。

讓我們快速看過每個選項。

### <a name="conditional-compilation"></a>條件式編譯

您可以保留一個程式碼基底，並針對 Windows 10 使用者編譯一組二進位檔。

首先，將新的組建設定新增到您的專案。

![組建設定](images/desktop-to-uwp/build-config.png)

對於該組建設定，請建立常數，用來識別呼叫 Windows 執行階段 API 的程式碼。  

對於 .NET 型專案，此常數稱為 **條件式編譯常數**。

![條件式編譯常數](images/desktop-to-uwp/compilation-constants.png)

對於 C++ 型專案，此常數稱為 **前置處理器定義**。

![前置處理器定義常數](images/desktop-to-uwp/pre-processor.png)

將該常數新增至任何 UWP 程式碼區塊之前。

```csharp
[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}
```

```C++
#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif
```

只有當您的使用中組建設定已定義該常數時，編譯器才會組建該程式碼。

### <a name="runtime-checks"></a>執行階段檢查

您可以為所有 Windows 使用者編譯一組二進位檔，不考慮他們執行什麼 Windows 版本。 只有當使用者以 Windows 10 上的封裝應用程式執行您的應用程式時，您的應用程式才會呼叫 Windows 執行階段 API。

將執行階段檢查新增至程式碼的最簡單方式，就是安裝此 Nuget 套件：[Desktop Bridge Helpers](https://www.nuget.org/packages/DesktopBridge.Helpers/)，然後使用 ``IsRunningAsUWP()`` 方法來關閉所有會呼叫 Windows 執行階段 API 的程式碼。 如需更多詳細資料，請參閱此部落格文章：[傳統型橋接器 - 識別應用程式的內容](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context)。

## <a name="related-samples"></a>相關範例

* [Hello World 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [次要磚](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Microsoft Store API 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [可實作 UWP UpdateTask 的 WinForms 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [接至 UWP 的傳統型應用程式橋接器範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>尋找問題的解答

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標籤](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在這裡](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)發問。
