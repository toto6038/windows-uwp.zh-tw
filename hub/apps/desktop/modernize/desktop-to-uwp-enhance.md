---
Description: 使用通用 Windows 平臺 (UWP) Api, 為 Windows 10 使用者增強您的桌面應用程式。
title: 在桌面應用程式中使用 UWP Api
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 44ea01bbc2200c1b028ed41e7c6a2845c7a1568b
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643366"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>在桌面應用程式中呼叫 UWP Api

您可以使用通用 Windows 平臺 (UWP) Api, 將現代化體驗新增至適用于 Windows 10 使用者的桌面應用程式。

首先, 使用必要的參考來設定您的專案。 然後, 從您的程式碼呼叫 UWP Api, 以將 Windows 10 體驗新增至您的桌面應用程式。 您可以分別為 Windows 10 使用者建立, 或將相同的二進位檔散發給所有使用者, 無論他們執行的是哪一個版本的 Windows。

某些 UWP Api 僅在[MSIX 套件](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)中封裝的桌面應用程式中受到支援。 如需詳細資訊, 請參閱[可用的 UWP api](desktop-to-uwp-supported-api.md)。

## <a name="set-up-your-project"></a>設定您的專案

您必須對專案進行一些變更，以便使用 UWP API。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>修改 .NET 專案以使用 Windows 執行階段 Api

.NET 專案有兩個選項:

* 如果您的應用程式以 Windows 10 1803 版或更新版本為目標, 您可以安裝 NuGet 套件, 以提供所有必要的參考。
* 或者, 您也可以手動新增參考。

#### <a name="to-use-the-nuget-option"></a>使用 NuGet 選項

1. 請確定已啟用[套件參考](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files):

    1. 在 Visual Studio 中, 按一下 **工具-> NuGet 套件管理員-> 套件管理員設定**。
    2. 請確定已選取 [**預設套件管理格式**] 的 [ **PackageReference** ]。

2. 在 Visual Studio 中開啟專案時, 在**方案總管**中以滑鼠右鍵按一下您的專案, 然後選擇 [**管理 NuGet 封裝**]。

3. 在 [ **NuGet 套件管理員**] 視窗中, 確認已選取 [**包含發行**前版本]。 然後, 選取 [**流覽**] 索引標籤`Microsoft.Windows.SDK.Contracts`, 並搜尋。

4. 找到套件之後, 在 [ **NuGet 套件管理員**] 視窗的右窗格中, 根據您想要設為目標的 Windows 10 版本, 選取您想要安裝的套件**版本:** `Microsoft.Windows.SDK.Contracts`

    * **10.0.18362. xxxx-預覽**:針對 Windows 10 版本1903選擇此程式。
    * **10.0.17763. xxxx-預覽**:針對 Windows 10 版本1809選擇此程式。
    * **10.0.17134. xxxx-預覽**:針對 Windows 10 版本1803選擇此程式。

5. 按一下 **\[安裝\]** 。

#### <a name="to-add-the-required-references-manually"></a>手動新增必要的參考

1. 開啟 **\[參考管理員\]** 對話方塊，選擇 **\[瀏覽\]** 按鈕，然後選取 **\[所有檔案\]** 。

    ![新增參考對話方塊](images/desktop-to-uwp/browse-references.png)

2. 新增這些檔案的參考。

    |檔案|Location|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows winmd|C:\Program Files (x86) \windows Kits\10\UnionMetadata\\ < *sdk 版本*> \Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86) \windows Kits\10\References\\ < *sdk 版本*> \Windows.Foundation.UniversalApiContract\<*版本*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86) \windows Kits\10\References\\ < *sdk 版本*> \Windows.Foundation.FoundationContract\<*版本*>|

3. 在 **\[屬性\]** 視窗中，將每個 *.winmd* 檔案的 **\[複製本機\]** 欄位設定為 **\[False\]** 。

    ![複製本機欄位](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>修改C++ Win32 專案以使用 Windows 執行階段 api

使用[ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)來取用 Windows 執行階段 api。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。

若要設定/WinRT 的C++專案:

* 針對新的專案, 您可以安裝[ C++/WinRT Visual Studio 擴充功能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) , 並使用該C++延伸模組中包含的其中一個/WinRT 專案範本。
* 針對現有的專案, 您可以在專案中安裝[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件。

如需這些選項的詳細資訊, 請參閱[這篇文章](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-windows-10-experiences"></a>新增 Windows 10 體驗

現在，您可以新增當使用者在 Windows 10 上執行您的應用程式時提供的現代化體驗了。 請使用這個設計流程。

:white_check_mark:**首先, 決定您想要新增的體驗**

您有許多選擇。 例如, 您可以使用[營收 api](/windows/uwp/monetize)來簡化您的訂單流程, 或在有有趣的共用時[直接注意您的應用程式](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts), 例如另一位使用者已張貼的新圖片。

![快顯通知](images/desktop-to-uwp/toast.png)

即使使用者忽略或關閉您的訊息，他們仍可以在控制中心再次看到這些訊息，按一下即可開啟您的應用程式。 這會增加與您的應用程式互動, 並讓您的應用程式與作業系統緊密整合的額外優點。 我們將在本文稍後說明該體驗的程式碼。

如需更多想法, 請流覽[UWP 檔](/windows/uwp/get-started/)。

:white_check_mark:**決定是否要增強或擴充**

您通常會聽到我們使用「*增強*」和「*延伸*」這兩個詞彙, 所以我們會花點時間說明每個詞彙的意義。

我們使用「*增強*」一詞來描述您可以直接從桌面應用程式呼叫的 Windows 執行階段 api (無論您是否選擇在 MSIX 套件中封裝應用程式)。 當您選擇了 Windows 10 體驗時, 請識別您需要建立的 Api, 然後查看該 API 是否出現在[此清單](desktop-to-uwp-supported-api.md)中。 這是您可以從桌面應用程式直接呼叫的 Api 清單。 如果您的 API 未顯示在此清單中，這是因為與該 API 相關的功能只能在 UWP 處理程序中執行。 這些通常包括呈現 UWP XAML 的 Api, 例如 UWP 地圖控制項或 Windows Hello 安全性提示。

> [!NOTE]
> 雖然轉譯 UWP XAML 的 Api 通常無法直接從您的桌面呼叫, 但您還是可以使用替代方法。 如果您想要裝載 UWP XAML 控制項或其他自訂視覺效果體驗, 您可以使用[XAML 島](xaml-islands.md)(從 windows 10 版本1903開始) 和[視覺效果層](visual-layer-in-desktop-apps.md)(從 windows 10 版本1803開始)。 這些功能可以在已封裝或未封裝的桌面應用程式中使用。

如果您已選擇將桌面應用程式封裝在 MSIX 套件中, 另一個選項是將 UWP 專案新增至您的方案, 以*擴充*應用程式。 桌面專案仍然是您應用程式的進入點, 但 UWP 專案可讓您存取未出現在[此清單](desktop-to-uwp-supported-api.md)中的所有 api。 桌面應用程式可以使用應用程式服務與 UWP 進程通訊, 我們有許多有關如何設定的指引。 如果您想要新增需要 UWP 專案的體驗, 請參閱[使用 uwp 元件擴充](desktop-to-uwp-extend.md)。

:white_check_mark:**參考 API 合約**

如果您可以直接從您的桌面應用程式呼叫 API, 請開啟瀏覽器並搜尋該 API 的參考主題。
在 API 摘要的下方，您可以找到描述該 API 之 API 協定的表格。 以下是表格範例：

![API 協定表格](images/desktop-to-uwp/contract-table.png)

如果您有 .NET 型傳統型應用程式，請新增該 API 協定的參考，然後將該檔案的 **\[複製本機\]** 屬性設定為 **\ [False\]** 。 如果您有 C++ 型專案，請將包含此協定之資料夾的路徑新增至 **\[其他 Include 目錄\]** 。

:white_check_mark:**呼叫 Api 以新增您的體驗**

以下是我們之前討論過的，您要用來顯示通知視窗的程式碼。 這些 Api 會出現在此[清單](desktop-to-uwp-supported-api.md)中, 因此您可以將此程式碼新增至您的桌面應用程式, 並立即加以執行。

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

```C++
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

若要深入了解通知，請參閱[調適型和互動式快顯通知](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)。

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>支援 Windows XP、Windows Vista 和 Windows 7/8 安裝基礎

您可以為 Windows 10 現代化您的應用程式, 而不需要建立新的分支並維護個別的程式碼基底。

如果您想為 Windows 10 使用者建置不同的二進位檔，請使用條件式編譯。 如果您傾向組建一組二進位檔然後部署到所有 Windows 使用者，請使用執行階段檢查。

讓我們快速檢視每個選項。

### <a name="conditional-compilation"></a>條件式編譯

您可以保留一個程式碼基底，並針對 Windows 10 使用者編譯一組二進位檔。

首先，將新的組建設定新增到您的專案。

![組建設定](images/desktop-to-uwp/build-config.png)

針對該組建設定, 建立常數, 以識別呼叫 Windows 執行階段 Api 的程式碼。  

對於 .NET 型專案，此常數稱為**條件式編譯常數**。

![前置處理器](images/desktop-to-uwp/compilation-constants.png)

對於 C++ 型專案，此常數稱為**前置處理器定義**。

![前置處理器](images/desktop-to-uwp/pre-processor.png)

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

您的使用中組建設定已定義該常數時，編譯器才會組建該程式碼。

### <a name="runtime-checks"></a>執行階段檢查

您可以為所有 Windows 使用者編譯一組二進位檔，不考慮他們執行什麼 Windows 版本。 您的應用程式只會在使用者以 Windows 10 上的封裝應用程式執行您的應用程式時, 才呼叫 Windows 執行階段 Api。

將執行時間檢查新增至程式碼的最簡單方式, 就是安裝此 Nuget 套件:[桌面橋接器](https://www.nuget.org/packages/DesktopBridge.Helpers/)協助程式, 然後使用``IsRunningAsUWP()``方法來關閉所有呼叫 Windows 執行階段 api 的程式碼。 如需更多詳細資料, 請參閱此 blog 文章:[桌面橋接器-識別應用程式的內容](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)。

## <a name="related-samples"></a>相關範例

* [Hello World 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [次要磚](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Store API 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [WinForms 應用程式, 可執行 UWP UpdateTask](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [桌面應用程式橋接至 UWP 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>支援與意見反應

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
