---
description: 針對將 Windows Phone Silverlight 移植到 UWP 時可能發生的問題進行疑難排解。
title: 將 Windows Phone Silverlight 移植到 UWP 的疑難排解
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b196408a769e204513d46b67cd13c31f0d2e8b9c
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2021
ms.locfileid: "105939033"
---
#  <a name="troubleshooting-porting-windows-phone-silverlight-to-uwp"></a>將 Windows Phone Silverlight 移植到 UWP 的疑難排解


前一個主題是[移植專案](wpsl-to-uwp-porting-to-a-uwp-project.md)。

強烈建議您將此移植指南從頭到尾讀一遍，但是我們也了解您急著想要儘快進入建置及執行專案的階段。 為此，您可以暫時將任何非必要的程式碼標成註解或移除，稍後再回來清償該負債。 本主題中的疑難排解問題和解決方式表格雖然無法用來替代閱讀接下來的幾個主題，但是在這個階段可能對您很有幫助。 在您進展到後續的主題時，隨時可以回來參考這個表格。

## <a name="tracking-down-issues"></a>追蹤問題

XAML 剖析例外狀況可能難以診斷，特別是如果例外狀況中的錯誤訊息沒有意義。 請確定偵錯工具已設定為擷取第一個可能發生的例外狀況 (以嘗試並擷取早期剖析例外狀況)。 您可以在偵錯工具檢查例外狀況變數，以判斷 HRESULT 或訊息是否有任何有用的資訊。 同時檢查 Visual Studio 的輸出視窗當中是否有 XAML 剖析器輸出的錯誤訊息。

如果您的 app 終止，而您知道的唯一事項是在 XAML 標記剖析期間擲回未處理的例外狀況，則那有可能是所參考的資源遺失的結果 (也就是資源有適用於 Windows Phone Silverlight app 的索引鍵，但沒有適用於 Windows 10 app 的索引鍵，例如一些系統 **TextBlock** 樣式索引鍵)。 或者，它可能是在 **UserControl**、自訂控制項或自訂版面配置面板內部擲回的例外狀況。

真的別無他法時，才採用二元分割法。 從「頁面」移除一半的標記，然後重新執行應用程式。 然後，您會知道錯誤是否在您所移除的一半內， (您現在應該在任何情況下還原) 或在 *未移除的* 一半中還原。 不斷針對包含錯誤的那一半重複分割程序，直到您準確找出問題為止。

## <a name="targetplatformversion"></a>TargetPlatformVersion

本節說明如果在 Visual Studio 中開啟 Windows 10 專案，當您看見下列訊息時該怎麼辦：「需要 Visual Studio 更新。 一個或多個專案需要平台 SDK &lt;version&gt;，但該 SDK 可能未安裝或包含在 Visual Studio 的未來更新中。」

-   首先，判斷您已針對 Windows 10 安裝的 SDK 版本號碼。 流覽至 **C： \\ Program Files (x86) \\ Windows 套件 \\ 10 \\ 包含 \\ &lt; versionfoldername &gt;** ，並記下 versionfoldername，其將採用四種標記法： " *&lt; &gt;*"。
-   開啟您的專案檔案以進行編輯，然後尋找 `TargetPlatformVersion` 和 `TargetPlatformMinVersion` 元素。 編輯它們以如下所示，將 *&lt; versionfoldername &gt;* 取代為您在磁片上找到的四個標記法版本號碼：

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>疑難排解問題和解決方式

此表格中的解決方式資訊是要為您提供足夠的資訊來解除困境。 隨著您詳閱後續的主題，您將會發現有關這當中每一個問題的進一步詳細資料。

| 徵狀 | 補救方法 |
|---------|--------|
| XAML 剖析器或編譯器會提供錯誤「_名稱 " &lt; typename &gt; " 不存在於命名空間 [...] 中」。_ | 如果 &lt;typename&gt; 是自訂類型，請在 XAML 標記內的命名空間前置詞宣告中，將 "clr-namespace" 變更為 "using"，然後移除任何組件語彙基元。 對於平台類型，這表示該類型不適用於通用 Windows 平台 (UWP)，因此請尋找對等的類型並更新您的標記。 您可能會立即遇到的範例為 `phone:PhoneApplicationPage` 與 `shell:SystemTray.IsVisible`。 | 
| XAML 剖析器或編譯器會提供錯誤「_成員 "成員 &lt; 名稱 &gt; " 無法辨識或無法存取。_」 或 _&lt; &gt; 在類型 [...] 中找不到 "propertyname" 屬性_。 | 在您已經移植某些類型名稱 (例如根 **Page**) 之後，就會開始出現這些錯誤。 該成員或屬性不適用於 UWP，因此請尋找對等的成員或屬性並更新您的標記。 您可能會立即遇到的範例為 `SupportedOrientations` 與 `Orientation`。 |
| XAML 剖析器或編譯器會提供錯誤「可 _附加屬性 [...]找不到 [...]。_" 或「_未知的可附加成員 [...]_」。 | 這可能是類型而非附加的屬性所造成，在此情況下，您將會先有類型錯誤，而此錯誤將在您修正它之後消失。 您可能會立即遇到的範例為 `phone:PhoneApplicationPage.Resources` 與 `phone:PhoneApplicationPage.DataContext`。 | 
|XAML 剖析器或編譯器或執行時間例外狀況會提供錯誤「_無法解析資源 " &lt; resourcekey &gt; "_」。 | 資源索引鍵不適用於通用 Windows 平台 (UWP) App。 找到正確的對等資源並更新您的標記。 您可能會立即遇到的範例為系統 **TextBlock** 樣式索引鍵，例如 `PhoneTextNormalStyle`。 |
| C # 編譯器會提供「找 _不到類型或命名空間名稱 ' &lt; name '」錯誤 &gt; [...]_」或「_類型或命名空間名稱 ' &lt; name ' 不 &gt; 存在於命名空間 [...]_」或「_&lt; &gt; 目前內容中沒有類型或命名空間名稱_' name '」的錯誤。 | 這可能表示編譯器還不知道類型的正確 UWP 命名空間。 請使用 Visual Studio 的 [**解析**] 命令來修正這個問題。 <br/>如果 API 不在稱為通用裝置系列的這組 API 中 (換句話說，在擴充功能 SDK 中實作 API)，然後使用[擴充功能 SDK](wpsl-to-uwp-porting-to-a-uwp-project.md)。<br/>可能會有其他移植比較沒那麼簡單的狀況。 您可能會立即遇到的範例為 `DesignerProperties` 與 `BitmapImage`。 | 
|在裝置上執行時，應用程式會終止，或從 Visual Studio 啟動時，您會看到「無法啟動 Windows 執行階段8.x 應用程式 [...] 錯誤。 啟用要求失敗，錯誤為「Windows 無法與目標應用程式通訊。 這通常表示目標應用程式的處理序已中止。 […]”. | 問題可能出在初始化期間您自己「頁面」中或繫結屬性 (或其他類型) 中執行的命令式程式碼。 或者，也可能是在應用程式終止時，正在剖析即將顯示的 XAML 檔案 (如果是從 Visual Studio 啟動，那將會是啟動頁面) 的情況下發生。 尋找無效的資源索引鍵和 (或) 嘗試本主題[追蹤問題](#tracking-down-issues)一節中的一些指導方針。|
| _XamlCompiler 錯誤 WMC0055：無法將文字值 ' &lt; 您的串流幾何 ' 指派給 &gt; 類型 ' RectangleGeometry ' 的屬性 ' 剪輯 '_ | 在 UWP 中，為 [Microsoft DirectX](/windows/desktop/directx) 和 XAML C++ UWP App 的類型。 |
| _XamlCompiler 錯誤 WMC0001：XML 命名空間 [...] 中的類型 'RadialGradientBrush' 不明_ | UWP 沒有 **RadialGradientBrush** 類型。 請從標記中移除 **RadialGradientBrush**，並使用一些其他類型的 [Microsoft DirectX](/windows/desktop/directx) 和 XAML C++ UWP App。 |
| _XamlCompiler 錯誤 WMC0011：元素 ' &lt; UIElement type ' 上有未知的成員 ' OpacityMask ' &gt;_ | UWP [Microsoft DirectX](/windows/desktop/directx) 和 XAML C++ UWP App。 |
| _SYSTEM.NI.DLL 中發生類型 ' System.runtime.interopservices.outattribute. COMException ' 的第一個例外狀況。其他資訊：應用程式呼叫已針對不同執行緒封送處理的介面。從 HRESULT (例外狀況： 0x8001010E (RPC_E_WRONG_THREAD) ) 。_ | 您正在進行的工作必須在 UI 執行緒上完成。 呼叫 [**CoreWindow.GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread))。 |
| 動畫正在執行，但是在其目標屬性上沒有任何效果。 | 請讓動畫獨立運作，或在其上設定 `EnableDependentAnimation="True"`。 請參閱[動畫](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| 在 Visual Studio 中開啟 Windows 10 專案時，您看見下列訊息：「需要 Visual Studio 更新。 一個或多個專案需要平台 SDK &lt;version&gt;，但該 SDK 可能未安裝或包含在 Visual Studio 的未來更新中。」 | 請參閱本主題中的 [TargetPlatformVersion](#targetplatformversion) 一節。 |
| 在 xaml.cs 檔案中呼叫 InitializeComponent 時，會擲回 System.InvalidCastException。 | 當您有多個 xaml 檔案 (至少有一個是 MRT 限定的) 會共用同一個 xaml.cs 檔案，而且元素所包含的 x:Name 屬性在這兩個 xaml 檔案間並不一致時，就會發生此情況。 請嘗試為這兩個 xaml 檔案中的相同元素新增相同名稱，或是一併省略名稱。 |

下一個主題是[移植 XAML 與 UI](wpsl-to-uwp-porting-xaml-and-ui.md)。