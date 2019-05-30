---
description: 如果您是使用 Windows Phone Silverlight 應用程式開發人員，則您可以讓妥善使用您的技能組合和您的程式碼移到 Windows 10。
title: 從 Windows Phone Silverlight 移至 UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c9088f695fb70d171f0b9d5474a4a0f2a63cae05
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372419"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>從 Windows Phone Silverlight 移至 UWP


如果您是使用 Windows Phone Silverlight 應用程式開發人員，則您可以讓妥善使用您的技能組合和您的程式碼移到 Windows 10。 使用 Windows 10，您可以建立通用 Windows 平台 (UWP) 應用程式，也就是您的客戶可以將安裝到裝置的所有類型的單一應用程式封裝。 如需 Windows 10，UWP 應用程式，與概念調適性的程式碼和我們將在本移轉指南中提及的自適性 UI 的詳細背景，請參閱[通用 Windows 平台 (UWP) 應用程式指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

當您移植您的 Windows 10 應用程式的 Windows Phone Silverlight 應用程式時，您將能夠跟上的行動裝置的功能， [Windows Phone 8.1 中導入](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v%3dvs.105))，遠遠超越它們使用通用 Windows 平台 (UWP) 應用程式和模型和 UI 架構是在所有 Windows 10 裝置上通用。 這使得以一個程式碼為基底，以及單一應用程式套件，來支援電腦、平板電腦、手機與大量其他種類裝置成為可能。 而這將讓您 app 的潛在對象倍增，並藉由共用資料、購買的消費性產品等，創造新的可能性。 如需新功能的詳細資訊，請參閱[適用於 Windows 10 中的開發人員最新消息](https://dev.windows.com/getstarted/whats-new-windows-10)。

如果您選擇，您的應用程式的 Windows Phone Silverlight 版本和 Windows 10 版本都可以是可供客戶在相同的時間。

**附註**  本指南旨在協助您以手動方式連接埠您 Windows 10 的 Windows Phone Silverlight 應用程式。 除了使用本指南中的資訊移植您的 app 外，您還可嘗試 **Mobilize.NET 的 Silverlight Bridge** 的開發人員預覽以協助移植程序自動化。 此工具會分析您的應用程式原始程式碼，並將轉換成與其 UWP 的 Windows Phone Silverlight 控制項的參考和 Api。 因為此工具仍在開發人員預覽中，尚無法處理所有轉換案例。 不過，大部分的開發人員應可藉由開始使用此工具而節省一些時間和精力。 若要嘗試開發人員預覽，請瀏覽 [Mobilize.NET 的網站](https://go.microsoft.com/fwlink/p/?LinkId=624546)。

## <a name="xaml-and-net-or-html"></a>XAML 與 .NET 或 HTML？

Windows Phone Silverlight 有 Silverlight 4.0 和您計劃針對.NET Framework 的版本和一小部分的 UWP Api 為基礎的 XAML UI 架構。 因為您在 Windows Phone Silverlight 應用程式中使用 Extensible Application Markup Language (XAML)，很可能 XAML 會是您的選擇，您的 Windows 10 版本，因為會傳輸大部分的知識與經驗，因為將大部分的原始程式碼和您使用軟體模式。 甚至您的 UI 標記與設計也可以輕易地移植過去。 您將發現 Managed API、XAML 標記、UI 架構與工具全都令人熟悉不已，而且您可以在 UWP app 中使用 C++、C# 或 Visual Basic 來搭配 XAML。 您可能會訝異此程序相對上有多麼容易，即使過程中有一兩項挑戰。

請參閱[使用 C# 或 Visual Basic 建立通用 Windows 平台 (UWP) app 的藍圖](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))。

**附註**  Windows 10 支援更多的.NET Framework 與 Windows Phone 市集應用程式。 例如，Windows 10 有數個 System.ServiceModel。\*命名空間以及 System.Net、 System.Net.NetworkInformation 和 System.Net.Sockets。 因此，現在正是移植您的 Windows Phone Silverlight 並的.NET 程式碼只編譯，並在新的平台上運作。 請參閱[命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)。
重新編譯您現有的.NET 原始程式碼到 Windows 10 應用程式的另一個充分的理由是，您將受益於.NET Native，其中一種預先 just-in-time 編譯技術，將 MSIL 轉換成原生可執行的機器碼。 .NET 原生 app 比其對應的 MSIL 啟動更快、使用更少的記憶體，而且消耗較少的電池電力。

這個移植指南將著重在 XAML 上，但或者，您也可以建置在功能上對等的 app (呼叫許多相同的 UWP API) 建置方法是使用 JavaScript、階層式樣式表 (CSS) 及 HTML5 搭配「適用於 JavaScript 的 Windows Library」。 雖然 XAML 與 HTML 的 Windows 執行階段 UI 架構彼此並不相同，但是不論您選擇哪一個，都可在各種 Windows 裝置上通用。

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>以通用或行動裝置系列為目標

有一個選項是將您的應用程式移植成以通用裝置系列為目標的應用程式。 在此情況下，應用程式可以安裝到最多種類的裝置上。 如果應用程式呼叫只在行動裝置系列中實作的 API，您就可以使用調適型程式碼來保護那些呼叫。 或者，您可以選擇將應用程式移植成以行動裝置系列為目標的應用程式，這種情況您不需撰寫調適型程式碼。

## <a name="adapting-your-app-to-multiple-form-factors"></a>調整您的應用程式以適應多種尺寸

您在前面小節中所做的選擇將決定可用來執行應用程式的裝置範圍，而且此裝置範圍可能非常廣泛。 即使將您的應用程式限制只在行動裝置系列上執行，您還是必須支援非常多種不同的螢幕尺寸。 因此，由於您的應用程式將以之前未支援的尺寸執行，請用那些尺寸來測試 UI 並進行任何必要的變更，讓 UI 可針對各個尺寸適當地調整。 您可以把這個當成移植後工作或移植延展目標，而在 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) 案例研究中會有一些實際操作範例。

## <a name="approaching-porting-layer-by-layer"></a>逐層進行移植

-   **檢視**。 檢視 (連同檢視模型) 會構成您的應用程式 UI。 在理想的情況下，檢視是由繫結至檢視模型之可觀察屬性的標記所組成。 另一種模式 (常見且方便，但僅限短期) 是針對程式碼後置檔案中要直接操作 UI 元素的命令式程式碼。 不論是哪一種情況，您的大部分 UI 標記和設計 (甚至是操作 UI 元素的命令式程式碼) 都會直接移植。
-   **檢視模型和資料模型**。 即使您沒有正式遵守關注點分離模式 (例如 MVVM)，您的應用程式中也無法避免會有執行檢視模型和資料模型的函式。 檢視模型程式碼會利用 UI 架構命名空間中的類型。 檢視模型與資料模型程式碼也都會使用非視覺作業系統與 .NET API (包括用於資料存取的 API)。 其中大多數皆[可供 UWP app 使用](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))，因此您應該能夠原封不動地移植此程式碼中的大部分。 不過，請記住：檢視模型是檢視的一種模型或「抽象概念」。  檢視模型提供 UI 的狀態與行為，而檢視本身則提供視覺效果。 基於這個原因，您為了配合 UWP 允許您在其上執行的不同尺寸而調整的任何 UI，可能都需要有對應的檢視模型變更。 對於網路功能與呼叫雲端服務，通常會有使用 .NET 或 UWP API 間的選項。 如需涉及做出決定的因素，請參閱[雲端服務、網路功能及資料庫](wpsl-to-uwp-business-and-data.md)。
-   **雲端服務**。 您的應用程式某些部分 (也許佔相當大的部分) 可能是在雲端以服務的形式執行。 在用戶端裝置上執行的應用程式部分則會連線到那些服務。 這是分散式 app 中最有可能在移植用戶端部分時保持不變的部分。 如果您還沒有雲端服務，[Microsoft Azure 行動服務](https://azure.microsoft.com/services/mobile-services/)會是您 UWP app 的一個絕佳雲端服務選項，它提供強大的後端元件，可供通用 Windows 應用程式呼叫以取得服務，服務涵蓋範圍可從簡單的動態磚更新，到伺服器陣列可提供的那種高難度延展性。

在移植之前或在移植期間，請考量是否可藉由重構來改善您的 app，以便將目的類似的程式碼一起收集在一些分層中，而不是任意散佈。 將 UWP app 分解為如上所述的分層，可讓您更容易更正 app、加以測試，並進行後續的閱讀與維護。 您可以讓功能更容易重複使用及避免平台之間的一些 UI API 差異問題，只要依照 Model-View-ViewModel ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)) 模式即可。 這種模式會將您 app 的資料、商務及 UI 部分彼此分開。 即便是在 UI 中，它也能將狀態和行為分開，以及個別地將可測試項目從視覺效果分離。 使用 MVVM 時，您可以只撰寫一次您的資料與商務邏輯，然後在所有裝置上使用，而不需要擔心 UI 的問題。 您可能也可以在不同的裝置上重複使用大部分的檢視模型與檢視組件。

## <a name="one-or-two-exceptions-to-the-rule"></a>此規則的幾個例外

當您閱讀此移植指南時，您可以參考[命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)。 相當直接的對應是一般規則，而且命名空間與類別對應表格描述任何的例外。

在功能層級，好消息是 UWP 中很少有不支援的項目。 您的技能組合與原始程式碼大部分都能非常好地轉譯到 UWP app，您將在此移植指南的剩餘部分閱讀到相關資訊。 但是，以下是一些 Windows Phone Silverlight 功能，您可能會有用於其沒有 UWP 對等。

| 沒有 UWP 對等功能的功能 | Windows Phone Silverlight 功能的文件 |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA。 一般而言，使用 C++ 的 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 可以做為替代。 請參閱[開發遊戲](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10))和 [DirectX 與 XAML 互通性](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))。 | [XNA Framework 類別庫](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|鏡頭應用程式 | [適用於 Windows Phone 8 的功能濾鏡](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| 主題| 描述|
|------|------------| 
| [命名空間和類別的對應](wpsl-to-uwp-namespace-and-class-mappings.md) | 本主題提供 UWP 等的 Windows Phone Silverlight Api 的完整對應。 |
| [移轉專案](wpsl-to-uwp-porting-to-a-uwp-project.md) | 您開始在 Visual Studio 中建立新的 Windows 10 專案，並將檔案複製到其中的移植程序。 |
| [疑難排解](wpsl-to-uwp-troubleshooting.md) | 強烈建議您將此移植指南從頭到尾讀一遍，但是我們也了解您急著想要儘快進入建置及執行專案的階段。 為此，您可以暫時將任何非必要的程式碼標成註解或移除，稍後再回來清償該負債。 本主題中的疑難排解問題和解決方式表格雖然無法用來替代閱讀接下來的幾個主題，但是在這個階段可能對您很有幫助。 在您進展到後續的主題時，隨時可以回來參考這個表格。 |
| [移植的 XAML 和 UI](wpsl-to-uwp-porting-xaml-and-ui.md) | UWP 應用程式的宣告式 XAML 標記形式定義 UI 的作法是將彰顯從 Windows Phone Silverlight 中轉譯。 您會發現在更新系統資源索引鍵參考、變更某些元素類型名稱，以及將 "clr-namespace" 變更為 "using" 之後，大部分的標記都相容。 |
| [移植 I/O、 裝置和應用程式模型](wpsl-to-uwp-input-and-sensors.md) | 與裝置本身及其感應器整合的程式碼牽涉到從使用者輸入和輸出到使用者。 它也可能涉及資料處理。 但是這個程式碼通常不被認為是 UI 層或資料層。 這個程式碼包含與震動控制器、加速計、陀螺儀、麥克風和喇叭 (與語音辨識和合成交集)、(地理) 位置以及輸入形式 (例如觸控、滑鼠、鍵盤及手寫筆) 的整合。 |
| [移植商務和資料層](wpsl-to-uwp-business-and-data.md) | UI 的背後是商務與資料層。 這些層中的程式碼會呼叫作業系統和 .NET Framework API (例如背景處理、位置、相機、檔案系統、網路及其他資料存取)。 其中大多數皆[可供 UWP app 使用](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))，因此您應該能夠原封不動地移植此程式碼中的大部分。 |
| [移植外型規格和 UX](wpsl-to-uwp-form-factors-and-ux.md) | Windows app 在電腦、行動裝置與任何其他類型的裝置，都有相同的外觀及操作方式。 使用者介面、輸入及互動模式皆非常相似，而在裝置之間移動的使用者會欣然感受到熟悉的體驗。|
|[案例研究：Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | 本主題提供可移植到 Windows 10 UWP 應用程式非常簡單的 Windows Phone Silverlight 應用程式的個案研究。 使用 Windows 10，您可以建立單一應用程式封裝，您的客戶可以將安裝到各種不同的裝置，以及這是我們要在此案例研究。 |
| [案例研究：Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | 本案例研究-這是根據所提供的資訊[Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)— 與 Windows Phone Silverlight 應用程式來顯示群組中的資料開始**LongListSelector**。 在檢視模型中，每個 **Author** 類別執行個體都代表該作者所著之書籍的群組，而在 **LongListSelector** 中，我們可以檢視依作者分組的書籍清單，或是縮小來查看作者的捷徑清單。 |

## <a name="related-topics"></a>相關主題

**文件**
* [適用於 Windows 10 中的開發人員最新消息](https://dev.windows.com/getstarted/whats-new-windows-10)
* [通用 Windows 平台 (UWP) app 指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [使用的通用 Windows 平台 (UWP) 應用程式的藍圖C#或 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [什麼是適用於 Windows Phone 8 開發人員下一步](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**Magazine 文章**
* [Visual Studio Magazine:Windows Phone 8.1:轉寄的聚合一大進展](https://go.microsoft.com/fwlink/p/?LinkID=398541)

**簡報**
* [從 Windows Phone 的 Nokia 音樂走向 Windows 8 的故事](https://go.microsoft.com/fwlink/p/?LinkId=321521)
 

