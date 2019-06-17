---
Description: 使用通用 Windows 平台 (UWP) Api，以強化您的 Windows 10 使用者的桌面應用程式。
title: 在傳統型應用程式中使用 UWP Api
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4846a29e914ffed15e4c3dea938cc51cefd566e0
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131940"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>在傳統型應用程式中呼叫 UWP Api

您可以使用通用 Windows 平台 (UWP) Api 來揭露給 Windows 10 使用者您桌面應用程式中新增新式體驗。

首先，設定您的專案所需的參考。 然後，呼叫 UWP Api，從您的程式碼新增至您的桌面應用程式中體驗 Windows 10。 您可以建立個別的 Windows 10 使用者，或散發到所有使用者，不論哪一個版本的 Windows 執行相同的二進位檔。

僅在封裝中的傳統型應用程式中支援某些 UWP Api [MSIX 封裝](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。 如需詳細資訊，請參閱 <<c0> [ 可用的 UWP Api](desktop-to-uwp-supported-api.md)。

## <a name="set-up-your-project"></a>將專案設定

您必須對專案進行一些變更，以便使用 UWP API。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>.NET 專案修改成使用 Windows 執行階段 Api

1. 開啟 **\[參考管理員\]** 對話方塊，選擇 **\[瀏覽\]** 按鈕，然後選取 **\[所有檔案\]** 。

    ![新增參考對話方塊](images/desktop-to-uwp/browse-references.png)

2. 新增這些檔案的參考。

  |檔案|Location|
  |--|--|
  |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
  |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
  |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

3. 在 **\[屬性\]** 視窗中，將每個 *.winmd* 檔案的 **\[複製本機\]** 欄位設定為 **\[False\]** 。

    ![複製本機欄位](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>修改C++專案，以使用 Windows 執行階段 Api

使用[ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)若要使用 Windows 執行階段 Api。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。

若要設定您的專案，如C++/WinRT，請參閱[修改 Windows 桌面應用程式專案，以加入C++/WinRT 支援](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

## <a name="add-windows-10-experiences"></a>新增 Windows 10 體驗

現在，您可以新增當使用者在 Windows 10 上執行您的應用程式時提供的現代化體驗了。 請使用這個設計流程。

:white_check_mark:**首先，決定您想要新增何種的體驗**

您有許多選擇。 比方說，您可以使用簡化採購單的訂單流程[賺錢術 Api](/windows/uwp/monetize)，或[將您的應用程式的注意力導向](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)當您有共用有趣的東西，例如新圖片，另一位使用者已張貼。

![快顯通知](images/desktop-to-uwp/toast.png)

即使使用者忽略或關閉您的訊息，他們仍可以在控制中心再次看到這些訊息，按一下即可開啟您的應用程式。 這會增加與您的應用程式，並有額外的好處，讓您的應用程式會顯示與作業系統密切整合。 我們將示範您經驗的程式碼稍後在本文中。

請瀏覽[UWP 文件](/windows/uwp/get-started/)如更多想法。

:white_check_mark:**決定是否以增強或擴充**

您通常會聽我們使用詞彙*加強*並*擴充*，因此我們需要一點時間來說明每種詞彙平均值。

我們使用詞彙*增強*來描述您可以直接從 （是否您已選擇 MSIX 封裝中的應用程式封裝） 的傳統型應用程式呼叫的 Windows 執行階段 Api。 當您選擇的 Windows 10 體驗時，了解的 Api，您需要建立它，然後查看該 API 是否[這份清單](desktop-to-uwp-supported-api.md)。 這是一份您可以直接從傳統型應用程式呼叫的 Api。 如果您的 API 未顯示在此清單中，這是因為與該 API 相關的功能只能在 UWP 處理程序中執行。 通常，其中包括例如 UWP 地圖控制項或 Windows Hello 安全性提示，要求轉譯 UWP XAML 的 Api。

> [!NOTE]
> 雖然不能呼叫通常呈現 UWP XAML 的 Api，直接從您的桌面，您可以使用替代方法。 如果您想要裝載 UWP XAML 控制項或其他自訂的視覺效果體驗，您可以使用[XAML 群島](xaml-islands.md)（從 Windows 10 版本 1903年） 和[視覺分層](visual-layer-in-desktop-apps.md)（於 Windows 10 1803年版開始）。 這些功能可以使用封裝或未封裝的桌面應用程式中。

如果您選擇將封裝中的 MSIX 封裝傳統型應用程式，另一個選項是*擴充*藉由將您的方案中的 UWP 專案的應用程式。 桌面專案仍然是您的應用程式的進入點，但 UWP 專案可讓您存取所有的 Api，不會出現在[這份清單](desktop-to-uwp-supported-api.md)。 傳統型應用程式可以與 UWP 處理序通訊所使用的 app service 和我們有關如何設定中有許多指導方針。 如果您想要新增體驗需要 UWP 專案，請參閱 < [UWP 元件與擴充](desktop-to-uwp-extend.md)。

:white_check_mark:**參考 API 合約**

如果您可以直接從您的桌面應用程式呼叫 API，請開啟瀏覽器並搜尋該 API 的參考主題。
在 API 摘要的下方，您可以找到描述該 API 之 API 協定的表格。 以下是表格範例：

![API 協定表格](images/desktop-to-uwp/contract-table.png)

如果您有 .NET 型傳統型應用程式，請新增該 API 協定的參考，然後將該檔案的 **\[複製本機\]** 屬性設定為 **\ [False\]** 。 如果您有 C++ 型專案，請將包含此協定之資料夾的路徑新增至 **\[其他 Include 目錄\]** 。

:white_check_mark:**呼叫的 Api 來新增您的體驗**

以下是我們之前討論過的，您要用來顯示通知視窗的程式碼。 這些 Api 會出現在這[清單](desktop-to-uwp-supported-api.md)讓您可以將這段程式碼新增至您的桌面應用程式，並立即執行它。

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

您可以將您的應用程式，適用於 Windows 10 現代化而不需要建立新的分支和維護個別的程式碼基底。

如果您想為 Windows 10 使用者建置不同的二進位檔，請使用條件式編譯。 如果您傾向組建一組二進位檔然後部署到所有 Windows 使用者，請使用執行階段檢查。

讓我們快速檢視每個選項。

### <a name="conditional-compilation"></a>條件式編譯

您可以保留一個程式碼基底，並針對 Windows 10 使用者編譯一組二進位檔。

首先，將新的組建設定新增到您的專案。

![組建設定](images/desktop-to-uwp/build-config.png)

針對該組建組態，建立常數，識別呼叫 Windows 執行階段 Api 的程式碼。  

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

您可以為所有 Windows 使用者編譯一組二進位檔，不考慮他們執行什麼 Windows 版本。 您的應用程式會呼叫 Windows 執行階段 Api 只有當使用者在執行您的應用程式封裝的應用程式在 Windows 10 上。

將執行階段檢查新增至您的程式碼的最簡單方式是安裝此 Nuget 套件：[傳統型橋接器 Helper](https://www.nuget.org/packages/DesktopBridge.Helpers/) ，然後使用``IsRunningAsUWP()``方法來呼叫 Windows 執行階段 Api 的所有程式碼的閘道。 請參閱此部落格文章以取得詳細資料：[傳統型橋接器-識別應用程式的內容](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)。

## <a name="related-samples"></a>相關範例

* [Hello World 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [次要磚](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [存放區 API 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [實作 UWP UpdateTask 的 WinForms 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP 範例傳統型應用程式橋接器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>支援與意見反應

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
