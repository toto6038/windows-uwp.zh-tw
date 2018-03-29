---
author: TylerMSFT
title: 處理 URI 啟用
description: 了解如何登錄應用程式，以使它成為統一資源識別項 (URI) 配置名稱的預設處理常式。
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.author: twhitney
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: high
ms.openlocfilehash: 754fa7c1fe805b45b33be1d560d07c22646d497c
ms.sourcegitcommit: 444eaccbdcd4be2f1a1e6d4ce5525ba57e363b56
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2018
---
# <a name="handle-uri-activation"></a>處理 URI 啟用

**重要 API**

-   [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
-   [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

了解如何登錄 App 以使它成為「統一資源識別項」(URI) 配置名稱的預設處理常式。 Windows 傳統型應用程式和「通用 Windows 平台」(UWP) app 皆可登錄為 URI 配置名稱的預設處理常式。 如果使用者選擇您的 App 做為 URI 配置名稱的預設處理常式，則每次啟動該類型的 URI 時都會啟用您的 App。

建議您只有當您希望處理某個 URI 配置類型的所有 URI 啟動時，才登錄該 URI 配置名稱。 如果您選擇登錄某個 URI 配置名稱，則在針對該 URI 配置名稱啟用您的 app 時，您必須為使用者提供預期的功能。 例如，登錄 mailto: URI 配置名稱的 app 必須開啟新的電子郵件訊息，以便讓使用者撰寫新的電子郵件。 如需 URI 關聯的詳細資訊，請參閱[檔案類型與 URI 的指導方針和檢查清單](https://msdn.microsoft.com/library/windows/apps/hh700321)。

這些步驟示範如何登錄自訂 URI 配置名稱 `alsdk://`，以及如何在使用者啟動 `alsdk://` URI 時啟用您的 App。

> **注意**：在 UWP app 中，會將特定的 URI 和副檔名保留給內建應用程式和作業系統使用。 如果嘗試以保留的 URI 或副檔名登錄 app，該嘗試將會被忽略。 請在[保留 URI 配置名稱和檔案類型](reserved-uri-scheme-names.md)中，參閱 URI 配置依字母排列的檔案類型清單，以了解遭保留或禁止而不能為 UWP app 登錄的檔案類型。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>步驟 1：在封裝資訊清單中指定擴充點

App 僅會接受封裝資訊清單中列示之 URI 配置名稱的啟用事件。 以下是如何指示 app 處理 `alsdk`URI 配置名稱的方法。

1.  在 **\[方案總管\]** 中按兩下 package.appxmanifest，以開啟資訊清單設計工具。 選取 **\[宣告\]** 索引標籤，然後在 **\[可用宣告\]** 下拉式清單中選取 **\[通訊協定\]**，然後按一下 **\[新增\]**。

    以下簡短說明可在資訊清單設計工具中為通訊協定填寫的每個欄位 (如需詳細資訊請參閱 [**AppX 封裝資訊清單**](https://msdn.microsoft.com/library/windows/apps/dn934791))：

| 欄位 | 說明 |
|-------|-------------|
| **標誌** | 在 **\[控制台\]** 的[設定預設程式](https://msdn.microsoft.com/library/windows/desktop/cc144154)中指定用來識別 URI 配置名稱的標誌。 如果沒有指定標誌，則會使用 app 的小標誌。 |
| **顯示名稱** | 在 **\[控制台\]** 的[設定預設程式](https://msdn.microsoft.com/library/windows/desktop/cc144154)中指定用來識別 URI 配置名稱的顯示名稱。 |
| **名稱** | 選擇 URI 配置的名稱。 |
|  | **注意**  [名稱] 必須全都是小寫字母。 |
|  | **保留和禁止的檔案類型** 請參閱[保留 URI 配置名稱和檔案類型](reserved-uri-scheme-names.md)，以取得因為已經被保留或禁止使用，而無法為 UWP app 登錄之 URI 配置的字母排序清單。 |
| **執行檔** | 指定通訊協定預設的啟動執行檔。 如果沒有指定，則會使用 App 的執行檔。 如果已指定，字串長度必須介於 1 到 256 個字元，且結尾必須為「.exe」，同時不能包含下列字元：&gt;、&lt;、:、"、&#124;、? 或 \*。 如果已指定，則也會使用**進入點**。 如果沒有指定**進入點**，則會使用針對 App 定義的進入點。 |
| **進入點** | 指定處理通訊協定延伸的工作。 這必須符合 Windows 執行階段類型的完整命名空間名稱。 如果沒有指定，則會使用 app 的進入點。 |
| **開始頁面** | 處理擴充點的網頁 |
| **資源群組** | 您可以用來基於資源管理目的、將延伸啟用群組在一起的標記。 |
| **所需的檢視** (僅限 Windows) | 指定 **\[所需的檢視\]** 欄位，指示當針對 URI 配置名稱啟動 app 時，app 視窗所需的空間大小。 **\[所需的檢視\]** 的可能值為 **Default**、**UseLess**、**UseHalf**、**UseMore** 或 **UseMinimum**。 <br/>**注意**  Windows 在判斷目標應用程式的最終視窗大小時會考量多種不同因素，例如來源應用程式的喜好設定、螢幕上的應用程式數目以及螢幕方向等。 設定 **\[所需的檢視\]** 並無法保證目標 app 的特定視窗行為。<br/> 行動裝置系列上不支援**行動裝置系列：所需的檢視**。 |
2.  輸入 `images\Icon.png` 做為 **\[標誌\]**。
3.  輸入 `SDK Sample URI Scheme` 做為 **\[顯示名稱\]**
4.  輸入 `alsdk` 做為 **\[名稱\]**。
5.  按下 Ctrl+S 以將變更儲存至 package.appxmanifest。

    這樣會將和這個一樣的 [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) 元素新增至封裝資訊清單。 **windows.protocol** 類別指示 app 處理 `alsdk` URI 配置名稱。

    ```xml
    <Applications>
        <Application Id= ... >
            <Extensions>
                <uap:Extension Category="windows.protocol">
                  <uap:Protocol Name="alsdk">
                    <uap:Logo>images\icon.png</uap:Logo>
                    <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
                  </uap:Protocol>
                </uap:Extension>
          </Extensions>
          ...
        </Application>
   <Applications>
    ```

## <a name="step-2-add-the-proper-icons"></a>步驟 2：新增適當圖示

成為 URI 配置名稱預設程式的應用程式，會在系統的各個地方顯示它們的圖示，例如 [預設程式] 控制台。 針對此用途，會連同您的專案包含 44x44 圖示。 請調整為相符的 app 磚標誌外觀，並使用 app 的背景色彩，而不要讓圖示變成透明。 請將標誌延伸至邊緣，且沒有邊框間距。 在白色背景上測試您的圖示。 請參閱[磚和圖示資產的指導方針](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets)，以取得圖示的詳細資訊。

## <a name="step-3-handle-the-activated-event"></a>步驟 3：處理啟用的事件

[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件處理常式會收到所有啟用事件。 **Kind** 屬性指示啟用事件的類型。 這個範例是設定來處理 [**Protocol**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.activation.activationkind.aspx#Protocol) 啟用事件。

> [!div class="tabbedCodeSnippets"]
> ```cs
> public partial class App
> {
>    protected override void OnActivated(IActivatedEventArgs args)
>   {
>       if (args.Kind == ActivationKind.Protocol)
>       {
>          ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
>          // TODO: Handle URI activation
>          // The received URI is eventArgs.Uri.AbsoluteUri
>       }
>    }
> }
> ```
> ```vb
> Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
>    If args.Kind = ActivationKind.Protocol Then
>       ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
>       
>       ' TODO: Handle URI activation
>       ' The received URI is eventArgs.Uri.AbsoluteUri
>  End If
> End Sub
> ```
> ```cpp
> void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
> {
>    if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
>    {
>       Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs =
>           dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
>       
>       // TODO: Handle URI activation  
>       // The received URI is eventArgs->Uri->RawUri
>    }
> }
> ```

> **注意**  當透過「通訊協定協定」啟動時，請確定 [返回] 按鈕會將使用者帶回到啟動應用程式的畫面，而不是應用程式先前的內容。

下列程式碼透過其 URI 以程式設計方式啟動 App：

```cs
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

如需關於如何透過 URI 啟動 App 的詳細資訊，請參閱[啟動 URI 的預設 App](launch-default-app.md)。

建議 app 針對每個開啟新頁面的啟用事件建立新的 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 透過這種方式，新 XAML **Frame** 的瀏覽上一頁堆疊將不會包含 app 在暫停時任何先前可能存在於目前視窗上的內容。 決定針對啟動和檔案協定使用單一 XAML **Frame** 的 app，應該先清除 **Frame** 瀏覽日誌上的頁面，然後再瀏覽到新頁面。

當透過通訊協定啟用啟動時，app 應該考慮包含能讓使用者回到 app 頂端頁面的 UI。

## <a name="remarks"></a>備註

任何 app 或網站都可以使用您的 URI 配置名稱，包含惡意 app 或網站。 因此您透過 URI 取得的任何資料都可能來自不受信任的來源。 建議您絕對不要採用透過 URI 接收的參數來執行永久動作。 例如，URI 參數可以用來啟動 app 進入使用者的帳戶頁面，但是建議您絕對不要使用它們直接修改使用者的帳戶。

> **注意**  如果您要為應用程式建立新的 URI 配置名稱，請務必遵循 [RFC 4395](http://go.microsoft.com/fwlink/p/?LinkID=266550) 中的指導方針。 這樣可確保您的名稱符合 URI 配置的標準。

> **注意**  當透過「通訊協定協定」啟動時，請確定 [返回] 按鈕會將使用者帶回到啟動應用程式的畫面，而不是應用程式先前的內容。

建議 app 針對每個開啟新 URI 目標的啟用事件建立新的 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 透過這種方式，新 XAML **Frame** 的瀏覽上一頁堆疊將不會包含 app 在暫停時任何先前可能存在於目前視窗上的內容。

如果您決定讓 app 針對啟動和通訊協定協定使用單一 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)，請先清除 **Frame** 瀏覽日誌上的頁面，然後再瀏覽到新頁面。 當透過通訊協定協定啟動時，請考慮在 app 中包含能讓使用者回到 app 頂端的 UI。

## <a name="related-topics"></a>相關主題

**完整範例**

* [關聯啟動範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

**概念**

* [預設程式](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [檔案類型與 URI 關聯模型](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**工作**

* [啟動 URI 的預設 app](launch-default-app.md)
* [處理檔案啟用](handle-file-activation.md)

**指導方針**

* [檔案類型與 URI 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700321)

**參考資料**

* [AppX 封裝資訊清單](https://msdn.microsoft.com/library/windows/apps/dn934791)
* [Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224742)
* [Windows.UI.Xaml.Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330)~~

 

 
