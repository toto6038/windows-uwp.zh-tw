---
title: 處理檔案啟用
description: App 可以登錄為特定檔案類型的預設處理常式。
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 28188acfd3999c0b384326f013a0ba1bdf71a34f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370748"
---
# <a name="handle-file-activation"></a>處理檔案啟用

**重要的 Api**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

您的應用程式可以註冊成為特定檔案類型的預設處理常式。 Windows 傳統型應用程式和「通用 Windows 平台」(UWP) app 皆可登錄為預設檔案處理常式。 如果使用者選擇您的 App 作為特定檔案類型的預設處理常式，則在啟動該類型的檔案時就會啟用您的 App。

建議只有當您希望處理某個檔案類型的所有檔案啟動時，才登錄該檔案類型。 如果您的應用程式只需在內部使用檔案類型，就不需要登錄為預設處理常式。 如果您選擇登錄某個檔案類型，則在針對該檔案類型啟用您的應用程式時，您必須為使用者提供預期的功能。 例如，圖片檢視器應用程式需要登錄以顯示 .jpg 檔案。 如需檔案關聯的詳細資訊，請參閱[檔案類型與 URI 的指導方針](https://docs.microsoft.com/windows/uwp/files/index)。

這些步驟示範如何登錄自訂檔案類型 .alsdk，以及如何在使用者啟動 .alsdk 檔案時啟用您的 app。

> **附註**  UWP 應用程式，在特定 Uri 和檔案的延伸模組保留給使用內建應用程式和作業系統。 如果嘗試以保留的 URI 或副檔名登錄 app，該嘗試將會被忽略。 如需詳細資訊，請參閱[保留的檔案和 URI 配置名稱](reserved-uri-scheme-names.md)。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>步驟 1：指定封裝資訊清單中的擴充點

App 僅會接受封裝資訊清單中列示之副檔名的啟用事件。 以下是如何指示應 app 處理具有 `.alsdk` 副檔名的檔案。

1.  在 [方案總管]  中按兩下 package.appxmanifest，以開啟資訊清單設計工具。 選取 [宣告] 索引標籤，然後在 [可用宣告] 下拉式清單中選取 [檔案類型關聯]，然後按一下 [新增]。     請參閱[以程式設計方式撰寫識別碼](https://docs.microsoft.com/windows/desktop/shell/fa-progids)，了解檔案關聯使用的識別碼詳細資料。

    以下簡短說明可在資訊清單設計工具中填寫的每個欄位：

| 欄位 | 描述 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **顯示名稱** | 指定一組檔案類型的顯示名稱。 顯示名稱用於在 [控制台] 的[設定預設程式](https://docs.microsoft.com/windows/desktop/shell/default-programs)中識別檔案類型。  |
| **標誌** | 指定用於桌面以及在 [控制台] 的[設定預設程式](https://docs.microsoft.com/windows/desktop/shell/default-programs)中識別檔案類型的標誌。  如果沒有指定標誌，則會使用應用程式的小標誌。 |
| **資訊提示** | 指定一組檔案類型的[資訊提示](https://docs.microsoft.com/windows/desktop/shell/fa-progids)。 當使用者的滑鼠游標暫留在這類檔案的圖示上時，就會顯示這個工具提示文字。 |
| **名稱** | 為共用相同顯示名稱、標誌、資訊提示以及編輯旗標的一組檔案類型選擇一個名稱。 選擇可以在所有 app 更新都維持一致的群組名稱。 **注意**  [名稱] 必須全都是小寫字母。 |
| **內容類型** | 為特定檔案類型指定 MIME 內容類型，例如 **image/jpeg**。 **允許的內容類型有關的重要注意事項：** 以下是依字母順序列出，因為它們會保留，或禁止，無法輸入到封裝資訊清單的 MIME 內容類型：**應用程式/強制下載**，**應用程式/八位元資料流**，**應用程式/不明**，**應用程式/x-msdownload**。 |
| **檔案類型** | 指定要登錄的檔案類型，前面加上句號，例如 “.jpeg”。 **保留和禁止的檔案類型：** 請參閱[保留的 URI 配置名稱和檔案類型](reserved-uri-scheme-names.md)字母的檔案類型，因為它們會保留，或禁止，無法註冊您的 UWP 應用程式的內建應用程式清單。 |

2.  輸入 `alsdk` 做為 [名稱]  。
3.  輸入 `.alsdk` 做為 [檔案類型]。 
4.  輸入 「 映像\\Icon.png 」 為標誌。
5.  按下 Ctrl+S 以將變更儲存至 package.appxmanifest。

上述步驟會將和這個一樣的 [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 元素新增至封裝資訊清單。 **windows.fileTypeAssociation** 類別指示 app 處理具有 `.alsdk` 副檔名的檔案。

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>步驟 2：新增適當的圖示

成為檔案類型預設程式的 app，會在系統的各個地方顯示它們的圖示。 例如，這些圖示會顯示在：

-   Windows 檔案總管清單檢視、操作功能表及功能區
-   預設程式控制台
-   檔案選擇器
-   [開始] 畫面上的搜尋結果

連同您的專案包含 44x44 圖示，讓您的標誌可以出現在這些位置中。 請調整為相符的 app 磚標誌外觀，並使用 app 的背景色彩，而不要讓圖示變成透明。 請將標誌延伸至邊緣，且沒有邊框間距。 在白色背景上測試您的圖示。 請參閱[磚和圖示資產的指導方針](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets)，以取得圖示的詳細資訊。

## <a name="step-3-handle-the-activated-event"></a>步驟 3：處理已啟動的事件

[  **OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated) 事件處理常式會接收所有的檔案啟用事件。

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> 當透過檔案合約啟動，請確定該的 [上一頁] 按鈕會將使用者帶回啟動應用程式的畫面，不適用於應用程式的先前的內容。

我們建議您建立新的 XAML**框架**開啟新的頁面每個啟用事件。 這樣一來，新的 XAML 框架瀏覽 backstack 未包含任何先前的內容應用程式可能會對目前的視窗時暫停。 如果您決定要使用單一的 XAML**框架**上市，以及檔案合約，然後您應清除的頁面**框架**的之前瀏覽至新的頁面巡覽日誌。

檔案啟用透過啟動您的應用程式時，您應該考慮包括 UI，可讓使用者返回應用程式的最上層頁面。

## <a name="remarks"></a>備註

您接收的檔案可能來自不受信任的來源。 建議您先驗證檔案內容，然後才在上面執行動作。

## <a name="related-topics"></a>相關主題

### <a name="complete-example"></a>完整範例

* [關聯的啟動範例](https://go.microsoft.com/fwlink/p/?LinkID=231484)

### <a name="concepts"></a>概念

* [預設程式](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [檔案類型和通訊協定關聯模型](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>工作

* [啟動檔案的預設應用程式](launch-the-default-app-for-a-file.md)
* [處理 URI 啟用](handle-uri-activation.md)

### <a name="guidelines"></a>指導方針

* [檔案類型與 Uri 的指導方針](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>參考資料
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
