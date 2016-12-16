---
author: TylerMSFT
title: "處理檔案啟用"
description: "App 可以登錄為特定檔案類型的預設處理常式。"
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
translationtype: Human Translation
ms.sourcegitcommit: ed7aee6add80d31b48006d9dec9e207c449a1912
ms.openlocfilehash: ffcfa8991e9eb73b8d6a47bb7dd1cd23220097e0

---

# <a name="handle-file-activation"></a>處理檔案啟用


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

App 可以登錄為特定檔案類型的預設處理常式。 Windows 傳統型應用程式和「通用 Windows 平台」(UWP) app 皆可登錄為預設檔案處理常式。 如果使用者選擇您的 App 作為特定檔案類型的預設處理常式，則在啟動該類型的檔案時就會啟用您的 App。

建議只有當您希望處理某個檔案類型的所有檔案啟動時，才登錄該檔案類型。 如果您的應用程式只需在內部使用檔案類型，就不需要登錄為預設處理常式。 如果您選擇登錄某個檔案類型，則在針對該檔案類型啟用您的應用程式時，您必須為使用者提供預期的功能。 例如，圖片檢視器應用程式需要登錄以顯示 .jpg 檔案。 如需檔案關聯的詳細資訊，請參閱[檔案類型與 URI 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700321)。

這些步驟示範如何登錄自訂檔案類型 .alsdk，以及如何在使用者啟動 .alsdk 檔案時啟用您的 app。

> **注意** 在 UWP App 中，會將特定的 URI 和副檔名保留給內建 App 和作業系統使用。 如果嘗試以保留的 URI 或副檔名登錄 app，該嘗試將會被忽略。 如需詳細資訊，請參閱[保留的檔案和 URI 配置名稱](reserved-uri-scheme-names.md)。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>步驟 1：在封裝資訊清單中指定擴充點


App 僅會接受封裝資訊清單中列示之副檔名的啟用事件。 以下是如何指示應 app 處理具有 `.alsdk` 副檔名的檔案。

1.  在 [方案總管] 中按兩下 package.appxmanifest，以開啟資訊清單設計工具。 選取 [宣告] 索引標籤，然後在 [可用宣告] 下拉式清單中選取 [檔案類型關聯]，然後按一下 [新增]。 請參閱[以程式設計方式撰寫識別碼](https://msdn.microsoft.com/library/windows/desktop/cc144152)，了解檔案關聯使用的識別碼詳細資料。

    以下簡短說明可在資訊清單設計工具中填寫的每個欄位：

| 欄位 | 說明 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **顯示名稱** | 指定一組檔案類型的顯示名稱。 顯示名稱用於在 [控制台] 的[設定預設程式](https://msdn.microsoft.com/library/windows/desktop/cc144154)中識別檔案類型。 |
| **標誌** | 指定用於桌面以及在 [控制台] 的[設定預設程式](https://msdn.microsoft.com/library/windows/desktop/cc144154)中識別檔案類型的標誌。 如果沒有指定標誌，則會使用應用程式的小標誌。 |
| **資訊提示** | 指定一組檔案類型的[資訊提示](https://msdn.microsoft.com/library/windows/desktop/cc144152)。 當使用者的滑鼠游標暫留在這類檔案的圖示上時，就會顯示這個工具提示文字。 |
| **名稱** | 為共用相同顯示名稱、標誌、資訊提示以及編輯旗標的一組檔案類型選擇一個名稱。 選擇可以在所有 app 更新都維持一致的群組名稱。 **注意** [名稱] 必須全都是小寫字母。 |
| **內容類型** | 為特定檔案類型指定 MIME 內容類型，例如 **image/jpeg**。 **允許之內容類型的重要事項：**以下是您無法在套件資訊清單中輸入的 MIME 內容類型清單 (依字母順序排序)，因為它們已經被保留或禁止使用：**application/force-download**、**application/octet-stream**、**application/unknown**、**application/x-msdownload**。 |
| **檔案類型** | 指定要登錄的檔案類型，前面加上句號，例如 “.jpeg”。 **保留和禁止的檔案類型：**請參閱[保留 URI 配置名稱和檔案類型](reserved-uri-scheme-names.md)，以取得因為已被保留或禁止使用，而無法為 UWP app 登錄之內建 app 的檔案類型清單 (依字母順序排序)。 |

2.  在 \[名稱\] 中輸入 `alsdk`。
3.  輸入 `.alsdk` 做為 [檔案類型]。
4.  輸入「images\Icon.png」做為 [標誌]。
5.  按下 Ctrl+S 以將變更儲存至 package.appxmanifest。

上述步驟會將和這個一樣的 [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) 元素新增至封裝資訊清單。 **windows.fileTypeAssociation** 類別指示 app 處理具有 `.alsdk` 副檔名的檔案。

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

## <a name="step-2-add-the-proper-icons"></a>步驟 2：新增適當圖示


成為檔案類型預設程式的 app，會在系統的各個地方顯示它們的圖示。 例如，這些圖示會顯示在：

-   Windows 檔案總管清單檢視、操作功能表及功能區
-   預設程式控制台
-   檔案選擇器
-   [開始] 畫面上的搜尋結果

請調整為相符的 app 磚標誌外觀，並使用 app 的背景色彩，而不要讓圖示變成透明。 請將標誌延伸至邊緣，且沒有邊框間距。 在白色背景上測試您的圖示。 如需範例圖示，請參閱[關聯啟動範例](http://go.microsoft.com/fwlink/p/?LinkID=620490)。
![方案總管及影像資料夾中檔案的檢視。 「icon.targetsize」和「smalltile-sdk」皆有 16、32、48 及 256 像素的版本](images/seviewofimages.png)

## <a name="step-3-handle-the-activated-event"></a>步驟 3：處理啟用的事件


[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) 事件處理常式會接收所有的檔案啟用事件。

> [!div class="tabbedCodeSnippets"]
```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```
```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
       // TODO: Handle file activation
       // The number of files received is args->Files->Size
       // The first file is args->Files->GetAt(0)->Name
}
```
```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

    > **Note**  When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

建議讓 app 針對每個會開啟新頁面的啟用事件建立新的 XAML 框架。 透過這種方式，新 XAML 框架的瀏覽上一頁堆疊將不會包含應用程式在暫停時任何先前可能存在於目前視窗上的內容。 決定針對啟動和檔案協定使用單一 XAML 框架的應用程式，應該先清除框架瀏覽日誌上的頁面，然後再瀏覽到新頁面。

當透過檔案啟用啟動時，app 應該考慮包含能讓使用者回到 app 頁面頂端的 UI。

## <a name="remarks"></a>備註


您接收的檔案可能來自不受信任的來源。 建議您先驗證檔案內容，然後才在上面執行動作。 如需輸入驗證的詳細資訊，請參閱[撰寫安全程式碼](http://go.microsoft.com/fwlink/p/?LinkID=142053)

> **注意**：本文章適用於撰寫通用 Windows 平台 (UWP) App 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## <a name="related-topics"></a>相關主題

**完整範例**

* [關聯啟動範例](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**概念**

* [預設程式](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [檔案類型與通訊協定關聯模型](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**工作**

* [啟動檔案的預設 app](launch-the-default-app-for-a-file.md)
* [處理 URI 啟用](handle-uri-activation.md)

**指導方針**

* [檔案類型與 URI 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700321)

**參考資料**
* [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
* [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

 

 



<!--HONumber=Dec16_HO1-->


