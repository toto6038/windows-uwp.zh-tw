---
description: 本文示範使用套件擴充功能將封裝的桌面應用程式與檔案總管整合的不同方式。
title: 整合已封裝的桌面應用程式與檔案總管
ms.date: 02/08/2021
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e3e7aaffc86a152530c933291321c30c2e7c507d
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335484"
---
# <a name="integrate-a-packaged-desktop-app-with-file-explorer"></a>整合已封裝的桌面應用程式與檔案總管

某些 Windows 應用程式會定義檔案總管擴充功能，以新增可讓客戶執行應用程式相關選項的內容功能表項目。 舊版的 Windows 應用程式部署技術（例如 MSI 和 ClickOnce）會透過登錄來定義檔案總管擴充功能。 登錄中有一系列的 hive，可控制檔案總管擴充功能和其他類型的 Shell 延伸模組。 這些安裝程式通常會建立一系列的登錄機碼，以設定要包含在內容功能表中的各種專案。

如果您使用 [MSIX](/windows/msix/)封裝您的 Windows 應用程式，則會將登錄虛擬化，因此您的應用程式無法透過登錄註冊檔案總管擴充功能。 相反地，您必須透過封裝延伸模組定義您的檔案總管延伸模組，您可以在封裝資訊清單中定義此延伸模組。 本文說明數種方式來進行這項操作。

您可以 [在 GitHub 上](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample)找到本文中所使用的完整範例程式碼。

## <a name="add-a-context-menu-entry-that-supports-startup-parameters"></a>新增支援啟動參數的內容功能表項目

整合檔案總管最簡單的方式之一，就是定義套件延伸模組，在使用者以滑鼠右鍵按一下檔案總管中的特定檔案類型時，將應用程式新增至內容功能表中的可用應用程式清單。 如果使用者開啟您的應用程式，您的延伸模組可以將參數傳遞至您的應用程式。

此案例有幾項限制：

* 它只適用于與 [檔案類型關聯功能](/windows/uwp/launch-resume/handle-file-activation)的組合。 您可以在內容功能表中，只針對與主要應用程式相關聯的檔案類型顯示其他選項 (例如，您的應用程式支援在檔案總管) 中按兩下檔案來開啟檔案。
* 只有當您的應用程式設定為該檔案類型的預設值時，才會顯示內容功能表中的選項。
* 唯一支援的動作是啟動應用程式的主要可執行檔 (也就是連接到 [開始] 功能表專案) 的相同可執行檔。 不過，每個動作都可以指定不同的參數，當應用程式啟動時，您可以使用這些參數，以瞭解哪個動作觸發了執行並執行不同的工作。

儘管有這些限制，但這種方法對於許多案例都已足夠。 例如，如果您要建立影像編輯器，則可以輕鬆地在內容功能表中加入專案來調整影像大小，這會直接使用 wizard 啟動影像編輯器，以啟動調整大小的程式。

### <a name="implement-the-context-menu-entry"></a>執行內容功能表項目

若要支援此案例，請將具有類別的 [擴充](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension) 功能專案新增 `windows.fileTypeAssociation` 至您的套件資訊清單。 這個元素必須加入為[應用程式](/uwp/schemas/appxpackage/uapmanifestschema/element-application)專案下[Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions)元素的子系。

下列範例將示範應用程式的註冊，此應用程式會啟用具有副檔名之檔案的內容功能表 `.foo` 。 這個範例會指定 `.foo` 延伸模組，因為這是一個偽造的延伸模組，通常不會向任何指定電腦上的其他應用程式註冊。 如果您需要管理可能已採用 (例如 .txt 或 .jpg) 的檔案類型，請記住，在您的應用程式設定為該檔案類型的預設值之前，您將無法看到此選項。 此範例是 GitHub 上相關範例中 [package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) 檔案的摘錄。

```xml
<Extensions>
  <uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="foo" Parameters="&quot;%1&quot;">
      <uap:SupportedFileTypes>
        <uap:FileType>.foo</uap:FileType>
      </uap:SupportedFileTypes>
      <uap2:SupportedVerbs>
        <uap3:Verb Id="Resize" Parameters="&quot;%1&quot; /p">Resize file</uap3:Verb>
      </uap2:SupportedVerbs>
    </uap3:FileTypeAssociation>
  </uap3:Extension>
</Extensions>
```

此範例假設在資訊清單中的根項目中宣告了下列命名空間和別名 `<Package>` 。

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap uap2 uap3 rescap">
  ...
</Package>
```

[FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)元素會將您的應用程式與您想要支援)  (s 的檔案類型產生關聯。 如需詳細資訊，請參閱 [將您的已封裝應用程式與一組檔案類型建立關聯](desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types)。 以下是與此元素相關的最重要專案。

| 屬性或元素 | Description |
|----------------------|-------------|
| `Name` 屬性 | 比對您想要註冊的延伸模組名稱減去上述範例中的點 (， `foo`) 。 |
| `Parameters` 屬性 | 包含當使用者按兩下具有這類副檔名的檔案時，您想要傳遞給應用程式的參數。 通常，您至少 `%1` 會傳遞，這是包含所選檔案之路徑的特殊參數。 如此一來，當您按兩下檔案時，應用程式就會知道它的完整路徑，而且可以載入它。 |
| [SupportedFileTypes](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-supportedfiletypes) 元素 | 指定您想要註冊的延伸模組 () 名稱，包括此範例中的點 (`.foo`) 。 您可以指定多個 `<FileType>` 您想要支援更多檔案類型的專案。 |

若要定義內容功能表整合，您也必須加入 [SupportedVerbs](/uwp/schemas/appxpackage/uapmanifestschema/element-uap2-supportedverbs) 子項目。 這個元素包含一個或多個 [動詞](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素，這些元素會定義使用者以滑鼠右鍵按一下檔案總管中具有 foo 副檔名的檔案時，將列出的選項。 如需詳細資訊，請參閱 [將選項新增至具有特定檔案類型之檔案的內容功能表](desktop-to-uwp-extensions.md#add-options-to-the-context-menus-of-files-that-have-a-certain-file-type)。 以下是與 [動詞](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素相關的最重要專案。

| 屬性或元素 | Description |
|----------------------|-------------|
| `Id` 屬性 | 指定動作的唯一識別碼。|
| `Parameters` 屬性 | 如同 [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 元素， [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素的這個屬性會包含使用者按一下內容功能表項目時傳遞至應用程式的參數。 一般而言，除了用 `%1` 來取得所選檔案路徑的特殊參數之外，您還必須傳遞一或多個參數來取得內容。 這可讓您的應用程式瞭解它是從內容功能表項目中開啟。  |
| 元素值 | [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb)元素的值包含要顯示在內容功能表項目中的標籤 (在此範例中，將檔案 **大小調整**) 。 |

### <a name="access-the-startup-parameters-in-your-app-code"></a>存取應用程式程式碼中的啟動參數

您的應用程式接收參數的方式取決於您所建立的應用程式類型。 例如，WPF 應用程式通常會在類別的方法中處理啟動事件引數 `OnStartup` `App` 。 您可以檢查是否有啟動參數，並根據結果，採取最適當的動作 (例如開啟應用程式的特定視窗，而不是主要的) 。

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        if (e.Args.Contains("Resize"))
        {
            // Open a specific window of the app.
        }
        else
        {
            MainWindow main = new MainWindow();
            main.Show();
        }
    }
}
```

下列螢幕擷取畫面示範上一個範例所建立的重 **設大小** 檔案內容功能表項目。

![快速鍵功能表中的 [檔案大小調整] 命令的螢幕擷取畫面](images/file-explorer/resize-file.png)

## <a name="support-generic-files-or-folders-and-perform-complex-tasks"></a>支援一般檔案或資料夾，並執行複雜的工作

雖然在封裝資訊清單中使用 [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 延伸模組（如上一節所述）對於許多案例都已足夠，但您可能會發現它有所限制。 這兩個最大的挑戰如下：

* 您只能處理與您相關聯的檔案類型。 例如，您無法處理一般資料夾。
* 您只能使用一連串的參數啟動應用程式。 您無法執行先進的作業，例如啟動另一個可執行檔或執行工作，而不需要開啟主要應用程式。

若要達成這些目標，您必須建立 [Shell 擴充](/windows/win32/shell/shell-exts)功能，以提供更強大的方式來與檔案總管整合。 在此案例中，您會建立一個 DLL，其中包含管理檔案內容功能表所需的所有專案，包括標籤、圖示、狀態和要執行的工作。 因為這項功能是在 DLL 中執行，所以您幾乎可以使用一般的應用程式執行所有動作。 在您執行 DLL 之後，您必須透過您在封裝資訊清單中定義的擴充功能來註冊它。

> [!NOTE]
> 本節所述的程式有一項限制。 在目的電腦上安裝包含延伸模組的 MSIX 封裝之後，必須先重新開機檔案總管，才能載入 Shell 擴充功能。 若要完成此動作，使用者可以重新開機電腦，也可以使用 **工作管理員** 重新開機 **explorer.exe** 進程。

### <a name="implement-the-shell-extension"></a>執行 Shell 擴充功能

Shell 擴充功能是以 [COM (元件物件模型) ](/windows/win32/com/component-object-model--com--portal)為基礎。 您的 DLL 會公開一或多個在系統登錄中註冊的 COM 物件。 Windows 會探索這些 COM 物件，並將您的擴充功能與檔案總管整合。 由於您要將程式碼與 Windows Shell 整合，因此效能和記憶體使用量很重要。 因此，這些類型的擴充功能通常是以 c + + 建立。

如需說明如何執行 Shell 擴充功能的範例程式碼，請參閱 GitHub 上相關範例中的 [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) 專案。 此專案是以 Windows 桌面範例中的 [這個範例](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb) 為基礎，它有幾項修訂，可讓範例更容易與最新版本的 Visual Studio 搭配使用。

此專案包含許多不同工作的未定案程式碼，例如動態 vs 靜態功能表和手動註冊 DLL。 如果您要使用 MSIX 封裝您的應用程式，則不需要此程式碼，因為封裝支援將會為您處理這些工作。 [ExplorerCommandVerb .cpp](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/ExplorerCommandVerb.cpp)檔案包含操作功能表的執行，這是本逐步解說所需的主要程式碼檔案。

Key 函數是 `CExplorerCommandVerb::Invoke` 。 這是當使用者按一下內容功能表中的專案時，所叫用的函式。 在此範例中，為了將對效能的影響降到最低，此作業會在另一個執行緒上執行，因此您實際上會在中找到實際的實作為 `CExplorerCommandVerb::_ThreadProc` 。

```cpp
DWORD CExplorerCommandVerb::_ThreadProc()
{
    IShellItemArray* psia;
    HRESULT hr = CoGetInterfaceAndReleaseStream(_pstmShellItemArray, IID_PPV_ARGS(&psia));
    _pstmShellItemArray = NULL;
    if (SUCCEEDED(hr))
    {
        DWORD count;
        psia->GetCount(&count);

        IShellItem2* psi;
        HRESULT hr = GetItemAt(psia, 0, IID_PPV_ARGS(&psi));
        if (SUCCEEDED(hr))
        {
            PWSTR pszName;
            hr = psi->GetDisplayName(SIGDN_DESKTOPABSOLUTEPARSING, &pszName);
            if (SUCCEEDED(hr))
            {
                WCHAR szMsg[128];
                StringCchPrintf(szMsg, ARRAYSIZE(szMsg), L"%d item(s), first item is named %s", count, pszName);

                MessageBox(_hwnd, szMsg, L"ExplorerCommand Sample Verb", MB_OK);

                CoTaskMemFree(pszName);
            }

            psi->Release();
        }
        psia->Release();
    }

    return 0;
}
```

當使用者以滑鼠右鍵按一下檔案或資料夾時，此函式會顯示訊息方塊，其中包含所選檔案或資料夾的完整路徑。 如果您想要以其他方式自訂 Shell 擴充功能，您可以擴充範例中的下列函式：

- 您可以變更 [GetTitle](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettitle) 函式，以自訂內容功能表中專案的標籤。
- 您可以變更 [GetIcon](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-geticon) 函式來自訂顯示在內容功能表中專案附近的圖示。
- 您可以變更 [GetTooltip](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettooltip) 函式來自訂當您將專案暫留在內容功能表中時所顯示的工具提示。

### <a name="register-the-shell-extension"></a>註冊 Shell 擴充功能

由於 Shell 延伸模組是以 COM 為基礎，因此，必須將執行 DLL 公開為 COM 伺服器，讓 Windows 可以將它與檔案總管整合。 一般來說，這是藉由將稱為 CLSID) 的唯一識別碼 (指派給 COM 伺服器，並在系統登錄的特定 hive 中登錄來完成。 在 [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) 專案中，擴充功能的 CLSID `CExplorerCommandVerb` 定義于 [.dll](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/Dll.h) 檔案中。

```cpp
class __declspec(uuid("CC19E147-7757-483C-B27F-3D81BCEB38FE")) CExplorerCommandVerb;
```

當您在 MSIX 套件中封裝 Shell 擴充 DLL 時，您會遵循類似的方法。 不過，GUID 必須在封裝資訊清單中註冊，而不是登錄[，如下所述。](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)

在您的套件資訊清單中，從將下列命名空間新增至您的 **package** 元素開始。

```xml
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:desktop5="http://schemas.microsoft.com/appx/manifest/desktop/windows10/5"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10" 
  IgnorableNamespaces="desktop desktop4 desktop5 com">
    
    ...
</Package>
```

若要註冊 CLSID，請新增 [com。](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) 具有類別目錄的延伸模組元素 `windows.comServer` 。 這個元素必須加入為[應用程式](/uwp/schemas/appxpackage/uapmanifestschema/element-application)專案下[Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions)元素的子系。 此範例是 GitHub 上相關範例中 [package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) 檔案的摘錄。

```xml
<com:Extension Category="windows.comServer">
  <com:ComServer>
    <com:SurrogateServer DisplayName="ContextMenuSample">
      <com:Class Id="CC19E147-7757-483C-B27F-3D81BCEB38FE" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
    </com:SurrogateServer>
  </com:ComServer>
</com:Extension>
```

[Com： Class](/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver-class)元素中有兩個要設定的重要屬性。

| 屬性 | 描述 |
|----------------------|-------------|
| `Id` 屬性 | 這必須與您想要註冊之物件的 CLSID 相符。 在此範例中，這是在與類別相關聯的檔案中宣告的 CLSID `Dll.h` `CExplorerCommandVerb` 。 |
| `Path` 屬性 | 這必須包含公開 COM 物件之 DLL 的名稱。 此範例會在封裝的根目錄中包含 DLL，因此可以只指定專案所產生的 DLL 名稱 `ExplorerCommandVerb` 。 |

接下來，加入另一個註冊檔案內容功能表的延伸模組。 若要這樣做，請將具有類別的 [desktop4： Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 元素新增 `windows.fileExplorerContextMenus` 至您的套件資訊清單。 您也必須將這個專案加入為[應用程式](/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素下[Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions)元素的子系。

```xml
<desktop4:Extension Category="windows.fileExplorerContextMenus">
  <desktop4:FileExplorerContextMenus>
    <desktop5:ItemType Type="Directory">
      <desktop5:Verb Id="Command1" Clsid="CC19E147-7757-483C-B27F-3D81BCEB38FE" />
    </desktop5:ItemType>
  </desktop4:FileExplorerContextMenus>
</desktop4:Extension>
```

[Desktop4： Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension)元素底下有兩個要設定的重要屬性。

| 屬性或元素 | Description |
|----------------------|-------------|
| `Type`[desktop5： ItemType](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-itemtype)的屬性 | 這會定義您想要與內容功能表產生關聯的專案類型。 如果您想要顯示所有檔案，它可能是星號 (`*`) ; 它可能是特定的副檔名 (`.foo`) ; 也可以在 (`Directory`) 資料夾中使用。 |
| `Clsid`[desktop5： Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-verb)的屬性 | 這必須符合您先前在套件資訊清單檔案中註冊為 COM 伺服器的 CLSID。 |

### <a name="configure-the-dll-in-the-package"></a>在套件中設定 DLL

在此範例中包含可執行 Shell 擴充 (的 DLL，在 MSIX 封裝的根目錄中 **ExplorerCommandVerb.dll**) 。 如果您使用 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，最簡單的解決方法是將 dll 複製並貼到專案中，並確定 dll 檔案屬性的 [ **複製到輸出目錄** ] 選項設定為 [ **如果較新時才複製**]。

若要確定封裝一律包含最新版本的 DLL，您可以將 [建立後事件](/visualstudio/ide/specifying-custom-build-events-in-visual-studio) 新增至 Shell 擴充功能專案，如此一來，每次建立時，就會將 DLL 複製到 Windows 應用程式封裝專案。

### <a name="restart-file-explorer"></a>重新開機檔案總管

安裝 Shell 擴充功能封裝之後，您必須先重新開機檔案總管，才能載入 Shell 擴充功能。 這是透過 MSIX 套件部署和註冊的 Shell 擴充功能的限制。

若要測試 Shell 擴充功能，請重新開機您的電腦，或使用 **工作管理員** 重新開機 **explorer.exe** 進程。 這麼做之後，您應該就可以在內容功能表中看到專案。

![自訂內容功能表項目的螢幕擷取畫面](images/file-explorer/folder-context-menu.png)

如果您按一下該函式， `CExplorerCommandVerb::_ThreadProc` 就會呼叫該函式，以顯示包含所選資料夾路徑的訊息方塊。

![自訂快顯視窗的螢幕擷取畫面](images/file-explorer/popup.png)
