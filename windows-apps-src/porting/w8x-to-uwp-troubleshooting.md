---
author: mcleblanc
description: "強烈建議您將此移植指南從頭到尾讀一遍，但是我們也了解您急著想要儘快進入建置及執行專案的階段。"
title: "將 Windows 執行階段 8.x 移植到 UWP 的疑難排解"
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: b30d26041718a74b9e2f3b9b93440e8fdf02b6c5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>將 Windows 執行階段 8.x 移植到 UWP 的疑難排解

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

前一個主題是[移植專案](w8x-to-uwp-porting-to-a-uwp-project.md)。

強烈建議您將此移植指南從頭到尾讀一遍，但是我們也了解您急著想要儘快進入建置及執行專案的階段。 為此，您可以暫時將任何非必要的程式碼標成註解或移除，稍後再回來清償該負債。 本主題中的疑難排解問題和解決方式表格雖然無法用來替代閱讀接下來的幾個主題，但是在這個階段可能對您很有幫助。 在您進展到後續的主題時，隨時可以回來參考這個表格。

## <a name="tracking-down-issues"></a>追蹤問題

XAML 剖析例外狀況可能難以診斷，特別是如果例外狀況中的錯誤訊息沒有意義。 請確定偵錯工具已設定為擷取第一個可能發生的例外狀況 (以嘗試並擷取早期剖析例外狀況)。 您可以在偵錯工具檢查例外狀況變數，以判斷 HRESULT 或訊息是否有任何有用的資訊。 同時檢查 Visual Studio 的輸出視窗當中是否有 XAML 剖析器輸出的錯誤訊息。

如果您的 app 終止，而您所知的只有在 XAML 標記剖析期間擲回未處理的例外狀況，則有可能是遺失資源參考的結果 (也就是資源有適用於通用 8.1 應用程式的索引鍵，但沒有適用於 Windows 10 應用程式的索引鍵，例如一些系統 **TextBlock** 樣式索引鍵)。 或者，可能是在 **UserControl**、自訂控制項或自訂配置面板內擲回例外狀況。

真的別無他法時，才採用二元分割法。 從「頁面」移除一半的標記，然後重新執行應用程式。 這樣您就會知道錯誤是出在您所移除的那半部內 (無論如何，您現在都應該還原這個部分)，還是出在您*未*移除的那半部內。 不斷針對包含錯誤的那一半重複分割程序，直到您準確找出問題為止。

## <a name="targetplatformversion"></a>TargetPlatformVersion

本節說明如果在 Visual Studio 中開啟 Windows 10 專案，當您看見下列訊息時該怎麼辦：「需要 Visual Studio 更新。 一個或多個專案需要平台 SDK <version>，但該 SDK 可能未安裝或包含在 Visual Studio 的未來更新中。」

-   首先，判斷您已針對 Windows 10 安裝的 SDK 版本號碼。 瀏覽到 **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\<versionfoldername>** 並記下 *<versionfoldername>* (以四等分標記法「主要編號.次要編號.組建編號.修訂編號」的方式)。
-   開啟您的專案檔案以進行編輯，然後尋找 `TargetPlatformVersion` 和 `TargetPlatformMinVersion` 元素。 將它們編輯如下，使用您在磁碟上找到的四等分標記法版本號碼取代 *<versionfoldername>*：

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>疑難排解問題和解決方式

此表格中的解決方式資訊是要為您提供足夠的資訊來解除困境。 隨著您詳閱後續的主題，您將會發現有關這當中每一個問題的進一步詳細資料。

| 問題 | 解決方式 |
|---------|--------|
| 在 Visual Studio 中開啟 Windows 10 專案時，您看見下列訊息：「需要 Visual Studio 更新。 一個或多個專案需要平台 SDK &lt;version&gt;，但該 SDK 可能未安裝或包含在 Visual Studio 的未來更新中。」 | 請參閱本主題中的 [TargetPlatformVersion](#targetplatformversion) 一節。 |
| 在 xaml.cs 檔案中呼叫 InitializeComponent 時，會擲回 System.InvalidCastException。| 當您有多個 xaml 檔案 (至少有一個是 MRT 限定的) 會共用同一個 xaml.cs 檔案，而且元素所包含的 x:Name 屬性在這兩個 xaml 檔案間並不一致時，就會發生此情況。 請嘗試為這兩個 xaml 檔案中的相同元素新增相同名稱，或是一併省略名稱。 |
| 在裝置上執行時，應用程式終止運作，或從 Visual Studio 啟動時，您看到下列錯誤：「無法啟用 Windows 市集應用程式 \[…\]。 啟用要求失敗，錯誤為「Windows 無法與目標應用程式通訊。 這通常表示目標應用程式的處理序已中止。 \[…\]”. | 問題可能出在初始化期間您自己「頁面」中或繫結屬性 (或其他類型) 中執行的命令式程式碼。 或者，也可能是在應用程式終止時，正在剖析即將顯示的 XAML 檔案 (如果是從 Visual Studio 啟動，那將會是啟動頁面) 的情況下發生。 尋找無效的資源索引鍵及 (或) 嘗試本主題＜追蹤問題＞一節中的一些指導方針。|
| XAML 剖析器或編譯器，或是執行階段例外狀況發出下列錯誤：「*無法解析資源 "<resourcekey>"。*」。 | 資源索引鍵不適用於通用 Windows 平台 (UWP) 應用程式 (舉例來說，這是含有一些 Windows Phone 資源的情況)。 找到正確的對等資源並更新您的標記。 您可能會立即遇到的範例是系統索引鍵，例如 `PhoneAccentBrush`。 |
| C# 編譯器發出下列錯誤：「*在 \[...\] 中找不到類型或命名空間名稱 '<name>'*」、「*命名空間 \[...\] 中沒有類型或命名空間名稱 '<name>'*」或「*類型或命名空間名稱 '<name>' 不存在於目前內容中*」。 | 這可能表示類型已實作於擴充功能 SDK 中 (雖然在有些情況下，不會以這麼直接的方式解決)。 使用 [Windows API](https://msdn.microsoft.com/library/windows/apps/bg124285) 參考內容以判斷實作該 API 的擴充功能 SDK，然後使用 Visual Studio 的 **Add** > **Reference** 命令將該 SDK 的參照新增到您的專案。 如果您的 App 針對稱為通用裝置系列的 API 集進行設計，請務必先使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 類別在執行階段測試擴充功能 SDK 是否存在，然後再予以呼叫 (這稱為調適型程式碼)。 如果通用 API 存在，則一律偏好擴充功能 SDK 中的 API。 如需詳細資訊，請參閱[擴充功能 SDK](w8x-to-uwp-porting-to-a-uwp-project.md)。 |

下一個主題是[移植 XAML 與 UI](w8x-to-uwp-porting-xaml-and-ui.md)。

