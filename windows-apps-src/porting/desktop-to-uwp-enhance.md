---
author: normesta
Description: Enhance your desktop app for Windows 10 users by using Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: 增強您的 Windows 10 傳統型應用程式
ms.author: normesta
ms.date: 08/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aafe2d09fc27a2693ccf2c4c9d8f189aa0164a3c
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976506"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>增強您的 Windows 10 傳統型應用程式

您可以使用 UWP API 新增適用於 Windows 10 使用者的現代化體驗。

首先，設定您的專案。 然後，新增 Windows 10 體驗。 您可以針對 Windows 10 使用者另行建置，也可以散發完全相同的二進位碼給所有使用者，無論他們執行什麼 Windows 版本。

## <a name="first-set-up-your-project"></a>首先，設定您的專案

您必須對專案進行一些變更，以便使用 UWP API。

### <a name="modify-a-net-project-to-use-uwp-apis"></a>修改 .NET 專案來使用 UWP API

開啟 **\[參考管理員\]** 對話方塊，選擇 **\[瀏覽\]** 按鈕，然後選取 **\[所有檔案\]**。

![新增參考對話方塊](images/desktop-to-uwp/browse-references.png)

然後，新增這些檔案的參考。

|檔案|位置|
|--|--|
|System.Runtime.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk 版本*>\Windows.Foundation.UniversalApiContract\<*版本*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk 版本*>\Windows.Foundation.FoundationContract\<*版本*>|

在 **\[屬性\]** 視窗中，將每個 *.winmd* 檔案的 **\[複製本機\]** 欄位設定為 **\[False\]**。

![複製本機欄位](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-uwp-apis"></a>修改 C++ 專案來使用 UWP API

開啟您專案的屬性頁面。

在 **\[C/C++\]** 設定群組的 **\[一般\]** 設定中，將 **\[使用 Windows 執行階段擴充功能\]** 欄位設定為 **\[是 (/ZW)\]**。

   ![使用 Windows 執行階段擴充功能](images/desktop-to-uwp/consume-runtime-extensions.png)

開啟 **\[其他 #using 目錄\]** 對話方塊，並新增這些目錄。

* %VSInstallDir%\Common7\IDE\VC\vcpackages
* C:\Program Files (x86)\Windows Kits\10\UnionMetadata
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\<*最新版本*>
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.FoundationContract\<*最新版本*>

開啟 **\[其他 Include 目錄\]** 對話方塊，並新增此目錄 C:\Program Files (x86)\Windows Kits\10\Include\<*最新版本*>\um

![其他 include 目錄](images/desktop-to-uwp/additional-include.png)

在 **\[C/C++\]** 設定群組的 **\[程式碼產生\]** 設定中，將 **\[啟用最少重建\]** 設定為 **\[否 (/GM-)\]**。

![啟用最少重建](images/desktop-to-uwp/disable-min-build.png)


## <a name="add-windows-10-experiences"></a>新增 Windows 10 體驗

現在，您可以新增當使用者在 Windows 10 上執行您的應用程式時提供的現代化體驗了。 請使用這個設計流程。

:white_check_mark: **首先，決定您要新增哪些體驗**

您有許多選擇。 例如，您可以使用創造營收 API 簡化採購訂單流程，或透過分享一些有趣事物來吸引別人關注您的應用程式，例如另一位使用者張貼的新圖片。

![快顯通知](images/desktop-to-uwp/toast.png)

即使使用者忽略或關閉您的訊息，他們仍可以在控制中心再次看到這些訊息，按一下即可開啟您的應用程式。 如此可提高使用者與您應用程式的互動程度，並獲得您的應用程式與作業系統深度整合的額外好處。 我們稍後會顯示該體驗的程式碼。

請造訪我們的[開發人員中心](https://developer.microsoft.com/windows)以尋找靈感。

:white_check_mark: **決定要增強還是擴充**

您會經常聽到我們使用「增強」和「擴充」這些詞彙，所以我們想花一點時間來解釋這些詞彙到底是什麼意思。

我們使用「增強」一詞來說明您可以直接從您的傳統型應用程式呼叫的 UWP API。 當您選擇 Windows 10 體驗後，找出您需要建立的 API，然後查看該 API 是否出現在此[清單](desktop-to-uwp-supported-api.md)中。 這是您可以直接從您的傳統型應用程式呼叫的 API 清單。 如果您的 API 未顯示在此清單中，這是因為與該 API 相關的功能只能在 UWP 處理程序中執行。 很多時候，這包括顯示現代化 UI 的 API，例如 UWP 地圖控制項或 Windows Hello 安全性提示。

也就是說，如果您想在您的應用程式中包含這些體驗，只需透過將 UWP 專案加入您的方案來「擴充」應用程式即可。 桌面專案仍是您應用程式的進入點，但 UWP 專案可讓您存取未出現在此[清單](desktop-to-uwp-supported-api.md)中的所有 API。 傳統型應用程式可以透過使用 App 服務來與 UWP 處理程序通訊，而且我們提供大量關於如何設定的指導方針。 如果您想要新增需要 UWP 專案的體驗，請參閱[透過 UWP 擴充](desktop-to-uwp-extend.md)。

:white_check_mark: **參考 API 協定**

如果您可以直接從傳統型應用程式呼叫 API，請開啟瀏覽器並搜尋該 API 的參考主題。
在 API 摘要的下方，您可以找到描述該 API 之 API 協定的表格。 以下是表格範例：

![API 協定表格](images/desktop-to-uwp/contract-table.png)

如果您有 .NET 型傳統型應用程式，請新增該 API 協定的參考，然後將該檔案的 **\[複製本機\]** 屬性設定為 **\ [False\]**。 如果您有 C++ 型專案，請將包含此協定之資料夾的路徑新增至 **\[其他 Include 目錄\]**。

:white_check_mark: **呼叫 API 以新增體驗**

以下是我們之前討論過的，您要用來顯示通知視窗的程式碼。 這些 API 出現在此[清單](desktop-to-uwp-supported-api.md)中，因此您可以將此程式碼新增到您的傳統型應用程式並立即執行。

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

您不需要建立新的分支並維護個別程式碼基底，即可針對 Windows 10 現代化您的應用程式。

如果您想為 Windows 10 使用者建置不同的二進位檔，請使用條件式編譯。 如果您傾向組建一組二進位檔然後部署到所有 Windows 使用者，請使用執行階段檢查。

讓我們快速檢視每個選項。

### <a name="conditional-compilation"></a>條件式編譯

您可以保留一個程式碼基底，並針對 Windows 10 使用者編譯一組二進位檔。

首先，將新的組建設定新增到您的專案。

![組建設定](images/desktop-to-uwp/build-config.png)

對於該組建設定，請建立常數，用來識別呼叫 UWP API 的程式碼。  

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

您可以為所有 Windows 使用者編譯一組二進位檔，不考慮他們執行什麼 Windows 版本。 只有當使用者在 Windows 10 上以封裝應用程式執行您的應用程式時，您的應用程式才會呼叫 UWP API。

將執行階段檢查新增至程式碼的最簡單方式是安裝此 Nuget 套件：[傳統型橋接器協助程式](https://www.nuget.org/packages/DesktopBridge.Helpers/)，然後使用 ``IsRunningAsUWP()`` 方法來關閉所有 UWP 程式碼。 如需詳細資訊，請參閱此部落格文章：[傳統型橋接器 - 識別應用程式的內容](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/) (英文)。

## <a name="related-video"></a>相關影片

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>相關範例

* [Hello World 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [次要磚](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Microsoft Store API 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [實作 UWP UpdateTask 的 WinForms 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [傳統型應用程式橋接轉 UWP 的範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>支援與意見反應

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
