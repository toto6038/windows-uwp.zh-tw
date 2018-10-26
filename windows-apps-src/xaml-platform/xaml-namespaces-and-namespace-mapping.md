---
author: jwmsft
description: 這個主題說明在大多數 XAML 檔案的根元素中找到的 XML/XAML 命名空間 (xmlns) 對應。 它也說明如何為自訂類型和組件產生類似的對應。
title: XAML 命名空間與命名空間對應
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a1aebe3d9aac460d444a5dffcd63142300c022b7
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5546521"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>XAML 命名空間與命名空間對應


這個主題說明在大多數 XAML 檔案的根元素中找到的 XML/XAML 命名空間 (**xmlns**) 對應。 它也說明如何為自訂類型和組件產生類似的對應。

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>XAML 命名空間與程式碼定義和類型程式庫的關聯

在 XAML 的一般用途以及應用在 Windows 執行階段應用程式的程式設計時，它主要是用於宣告物件、這些物件的屬性，並以階層方式來表達物件與屬性的關係。 您在 XAML 中宣告的物件，是由其他程式設計技術和語言所定義的類型程式庫或其他表示法來支援。 這些程式庫可能是：

-   Windows 執行階段的一組內建物件。 這是一組固定的物件，從 XAML 存取這些物件時會使用內部類型對應和啟用邏輯。
-   Microsoft 或協力廠商提供的分散式程式庫。
-   表示協力廠商控制項定義的程式庫，亦即您的應用程式納入及套件轉散發時所用的程式庫。
-   您自己的程式庫 (您專案的一部分)，保存您的部分或所有的使用者程式碼定義。

支援類型資訊與特定 XAML 命名空間定義相關聯。 XAML 架構 (例如 Windows 執行階段) 可以彙總多個組件與多個程式碼命名空間來對應到單一 XAML 命名空間。 這樣能夠讓 XAML 詞彙的概念涵蓋較大的程式設計架構或技術。 XAML 詞彙的規模可以相當龐大，例如，這份參考中所記載的大部分 Windows 執行階段應用程式 XAML 就構成單一份 XAML 詞彙。 XAML 詞彙也是可以延伸的：延伸的方式是透過將類型新增到支援程式碼定義，並確定在程式碼命名空間 (已經做為 XAML 詞彙的對應命名空間來源) 中包含類型。

在 XAML 命名空間建立執行階段物件表示方法時，XAML 處理器可以從與該 XAML 命名空間相關聯的支援組件查詢類型及成員。 這就是為什麼 XAML 是正規化及交換物件建構行為定義的有用方式，以及為什麼使用 XAML 做為 UWP 應用程式的 UI 定義技術的原因。

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>典型 XAML 標記使用方法中的 XAML 命名空間

XAML 檔案幾乎永遠在它的根元素中宣告預設的 XAML 命名空間。 預設 XAML 命名空間定義您可以宣告但不必使用前置詞來限定的元素。 例如，如果您宣告元素 `<Balloon />`，XAML 剖析器會預期有一個元素 **Balloon**，而且它在預設 XAML 命名空間是有效的。 相反地，如果 **Balloon** 不在定義的預設 XAML 命名空間中，您必須改為使用前置詞 (例如 `<party:Balloon />`) 來限制該元素名稱。 前置詞指示元素存在於預設命名空間以外的另一個 XAML 命名空間中，而且您必須先將 XAML 命名空間對應到前置詞 **party**，才能夠使用這個元素。 XAML 命名空間會套用到宣告它們的特定元素，也會套用到 XAML 結構中這個元素包含的任何元素。 因此，XAML 命名空間幾乎永遠在 XAML 檔案的根元素上宣告，以利用這種繼承的特性。

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>預設和 XAML 語言 XAML 命名空間宣告

在多數 XAML 檔案的根元素內有兩個 **xmlns** 宣告。 第一個宣告會將 XAML 命名空間對應為預設值： `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

這也是在數個舊版 Microsoft 技術使用的 XAML 命名空間識別碼，這些技術也使用 XAML 做為 UI 定義標記格式。 使用相同的識別碼是刻意的，而且當您將之前定義的 UI 移轉到使用 C++、C# 或 Visual Basic 的 Windows 執行階段 App 時，這種做法很有用。

第二個宣告會將 XAML 定義的語言元素的另一個 XAML 命名空間對應到 "x:" 前置詞： `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

這個 **xmlns** 值及它所對應的 "x:" 前置詞，也和數種使用 XAML 的舊版 Microsoft 技術中使用的定義相同。

這些宣告之間的關係為 XAML 是語言定義，而且 Windows 執行階段是使用 XAML 做為語言的一個實作，並定義 XAML 中參考之類型的特定詞彙。

XAML 語言會指定特定語言元素，這些元素中的每個元素都應該透過針對 XAML 命名空間而運作的 XAML 處理器實作來進行存取。 XAML 語言 XAML 命名空間的 "x:" 對應慣例後面接著專案範本、範例程式碼以及語言功能的文件。 XAML 語言命名空間定義數個即使是使用 C++、C# 或 Visual Basic 的基本 Windows 執行階段 app 也必須使用的常用功能。 例如，為了透過部分類別將任何程式碼後置加入到 XAML 檔案，您必須在相關的 XAML 檔案的根元素中將這個類別命名為 [x:Class 屬性](x-class-attribute.md)。 或者，XAML 頁面中定義為 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)中的索引資源的任何元素，就不一定必須在物件元素上設定 [x:Key 屬性](x-key-attribute.md)。

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>其他 XAML 命名空間

除了預設命名空間和 XAML 語言 XAML 命名空間 "x:" 之外，您可能也會看到 Microsoft Visual Studio 產生之 App 的初始預設 XAML 中其他對應的 XAML 命名空間。

### **<a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>d: (`http://schemas.microsoft.com/expression/blend/2008`)**

"d:" XAML 命名空間旨在提供設計工具支援，特別是針對 Microsoft Visual Studio 中 XAML 設計介面的設計工具支援。 "d:" XAML 命名空間啟用 XAML 元素上的設計工具或設計階段屬性。 這些設計工具屬性只會影響 XAML 行為的設計層面。 當應用程式執行時，如果 Windows 執行階段 XAML 剖析器載入了相同的 XAML，就會忽略設計工具屬性。 一般來說，設計工具屬性在任何 XAML 元素上都是有效的，但是實際上，只有特定案例才適合您自行套用設計工具屬性。 特別是許多設計工具屬性是為了在您開發使用資料繫結的 XAML 和程式碼時，能夠針對與資料內容及資料來源的互動提供更佳的使用經驗。

-   **d:DesignHeight 和 d:DesignWidth 屬性：** 這些屬性有時會套用到 Visual Studio 或其他 XAML 設計工具介面為您建立的 XAML 檔案的根元素。 例如，如果您將一個新的 **UserControl** 新增到您的應用程式專案，系統就會在已建立的 XAML 的 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) 根元素上設定這些屬性。 這些屬性可以讓您更容易設計 XAML 內容的組合，讓您可以預期到一旦將該 XAML 內容用於控制項執行個體或較大 UI 頁面的其他部分時，可能會有的一些配置限制。

   **注意：** 如果您從 Microsoft Silverlight 移轉 XAML，呈現整個 UI 頁面的根元素上仍可能會有這些屬性。 在這個情況下，您可能想要移除這些屬性。 在設計能夠良好處理縮放和檢視狀態的頁面配置上，XAML 設計工具的其他功能 (例如模擬器) 比起使用 **d:DesignHeight** 和 **d:DesignWidth** 的固定大小頁面配置來得有用。

-   **d:DataContext 屬性：** 您可以在頁面根元素或控制項上設定這個屬性，覆寫物件在其他情況下所具有的任何明確或繼承的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)。
-   **d:DesignSource 屬性：** 指定 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833) 的設計階段資料來源，會覆寫 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835)。
-   **d:DesignInstance 和 d:DesignData 標記延伸：** 這些標記延伸是用來為 **d:DataContext** 或 **d:DesignSource** 提供設計階段資料資源。 我們將不會在這裡完全載明如何使用設計階段資料資源。 如需詳細資訊，請參閱[設計階段屬性](http://go.microsoft.com/fwlink/p/?LinkId=272504)。 如需一些使用範例，請參閱[設計介面上適用於原型設計的範例資料](https://msdn.microsoft.com/library/windows/apps/mt517866)。

### **<a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

"mc:" 指示並支援讀取 XAML 的標記相容性模式。 一般而言，"d:" 前置詞是與屬性 **mc:Ignorable** 相關聯。 這項技術可以讓執行階段 XAML 剖析器忽略 "d:" 中的設計屬性。

### <a name="local-and-common"></a>**local:** 與 **common:**

"local:" 是一個前置詞，通常會針對範本化的 UWP app 專案，在 XAML 頁面內進行對應。 對應這個前置詞，以便參考建立來包含 [x:Class 屬性](x-class-attribute.md)的相同命名空間，以及適用於所有 XAML 檔案 (包含 app.xaml 在內) 的程式碼。 只要您在這個相同命名空間中定義任何想要在 XAML 中使用的自訂類別，就可以使用 **local:** 前置詞，在 XAML 中參考您的自訂類型。 來自範本化 UWP app 專案的相關前置詞為 **common:**。 這個前置詞會參考巢狀的 "Common" 命名空間 (其中包含像是轉換器和命令的公用程式類別)，而您可以在 [**方案總管**] 檢視的 Common 資料夾中找到定義。

### **<a name="vsm"></a>vsm:**

請勿使用。 "vsm:" 是有時可透過其他 Microsoft 技術匯入的舊版 XAML 範本中見到的前置詞。 命名空間原來可以解決傳統的命名空間工具問題。 您應該刪除用於 Windows 執行階段的任何 XAML 中 "vsm:" 的 XAML 命名空間定義，以及變更 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)、[**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) 以及相關物件的任何前置詞使用方法，以改用預設的 XAML 命名空間。 如需 XAML 移轉的詳細資訊，請參閱[將 Silverlight 或 WPF XAML/程式碼移轉到 Windows 執行階段 app](https://msdn.microsoft.com/library/windows/apps/br229571)。

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>將自訂類型對應到 XAML 命名空間和前置詞

您可以對應 XAML 命名空間，如此即可使用 XAML 來存取您自己的自訂類型。 換句話說，您對應程式碼命名空間的方式如同它存在於定義自訂類型的程式碼表示法中，並對它指派 XAML 命名空間以及前置詞以供使用。 適用於 XAML 的自訂類型可以在 Microsoft .NET 語言 (C# 或 Microsoft Visual Basic) 或 C++ 中定義。 對應是透過定義 **xmlns** 前置詞來建立的。 例如，`xmlns:myTypes` 定義新的 XAML 命名空間，它是由使用語彙基元 `myTypes:` 將所有用法加上前置詞來進行存取的。

**xmlns** 定義包含值以及前置詞命名。 值是包含在引號內的字串，後面跟著等號。 常見 XML 慣例是將 XML 命名空間與統一資源識別元 (URI) 相關聯，這就是唯一性與識別性的慣例。 您在預設 XAML 命名空間與 XAML 語言 XAML 命名空間，以及一些 Windows 執行階段 XAML 較少用的 XAML 命名空間，也會看到這種個慣例。 不過，對於對應自訂類型的 XAML 命名空間，並非指定 URI，您要使用語彙基元 "using:" 做為前置詞定義的開頭。 在 "using:" 語彙基元的後面，接著命名程式碼命名空間。

例如，若要對應讓您參考 "CustomClasses" 命名空間的 "custom1" 前置詞，並使用該命名空間或組件的類別在 XAML 中做為物件元素，您的 XAML 頁面應該在根元素上包含下列對應： `xmlns:custom1="using:CustomClasses"`

不需要對應相同頁面範圍的部分類別。 例如，您不需要前置詞來參考針對處理頁面上 XAML UI 定義的事件而定義的任何事件處理常式。 此外，如果從 Visual Studio 為使用 C++、C# 或 Visual Basic 的 Windows 執行階段應用程式產生專案，而這些專案的許多 XAML 起始頁面已經對應了 "local:" 前置詞，則它會參考專案指定的預設命名空間以及部分類別定義使用的命名空間。

### <a name="clr-language-rules"></a>CLR 語言規則

如果您以 .NET 語言 (C# 或 Microsoft Visual Basic) 撰寫支援程式碼，則您可能使用以點 (".") 做為命名空間名稱一部分的慣例，以便建立程式碼命名空間的概念性階層。 如果您的命名空間定義包含點，則這個點應該是您在 "using:" 語彙基元之後指定的值的一部分。

如果您的程式碼後置檔案或程式碼定義檔案是 C++ 檔案，則某些慣例仍舊依循 Common Language Runtime (CLR) 語言格式，因此在 XAML 語法上並無差異。 如果您使用 C++ 宣告巢狀命名空間，則當您指定 "using:" 語彙基元後面的值時，後續巢狀命名空間字串之間的分隔符號應該是 "." 而不是 "::"。

當您定義程式碼來與 XAML 搭配使用時，請勿使用巢狀型別 (例如，在類別內巢狀列舉)。 系統無法評估巢狀型別。 XAML 剖析器沒有任何方法能夠區分某一個點是巢狀型別名稱的一部分，而不是命名空間名稱的一部分。

## <a name="custom-types-and-assemblies"></a>自訂類型和組件

定義 XAML 命名空間之支援類型的組件名稱，不是在對應中指定的。 可用組件的邏輯是在應用程式定義層級加以控制的，而且它屬於基本應用程式部署及安全性原則的一部分。 在專案設定中，將您想要包含為 XAML 之程式碼定義來源的任何組件宣告為相依組件。 如需詳細資訊，請參閱[在 C# 和 Visual Basic 中建立 Windows 執行階段元件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)。

如果您是從主 app 定義或頁面定義參考自訂類型，這些類型不需要進一步進行相依組件設定即可使用，但是您仍然必須對應包含這些類型的程式碼命名空間。 常用慣例是為任何特定 XAML 頁面的預設程式碼命名空間對應前置詞 "local"。 這個慣例通常是包含在 XAML 專案的起始專案範本內。

## <a name="attached-properties"></a>附加屬性

如果您正在參考附加屬性，附加屬性名稱的 owner-type 部份必須位於預設的 XAML 命名空間或做為前置詞。 只有在極少數的情況下，才會將前置詞屬性從其元素中獨立出來，但有時會有一種情況必須這樣做，特別是針對自訂的附加屬性。 如需詳細資訊，請參閱[自訂附加屬性](custom-attached-properties.md)。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [XAML 語法指南](xaml-syntax-guide.md)
* [在 C# 和 Visual Basic 中建立 Windows 執行階段元件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Windows 執行階段應用程式的 C#、VB 及 C++ 專案範本](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [將 Silverlight 或 WPF XAML/程式碼移轉到 Windows 執行階段應用程式](https://msdn.microsoft.com/library/windows/apps/br229571)
 

