---
author: jwmsft
description: 說明如何將 XAML 附加屬性當作相依性屬性來實作，以及如何定義讓附加屬性可以在 XAML 中使用所需的存取子慣例。
title: 自訂附加屬性
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.author: jimwalk
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: ce26242f1f5093afcbfb652a7d1736897975cb3a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2018
ms.locfileid: "4429290"
---
# <a name="custom-attached-properties"></a>自訂附加屬性

「附加屬性」** 是一種 XAML 概念。 附加屬性通常會定義為特殊格式的相依性屬性。 這個主題說明如何將附加屬性當作相依性屬性來實作，以及如何定義讓附加屬性可以在 XAML 中使用所需的存取子慣例。

## <a name="prerequisites"></a>先決條件

我們假設您了解現有相依性屬性使用者對相依性屬性的觀點，而且也已經閱讀了[相依性屬性概觀](dependency-properties-overview.md)。 您必須也要閱讀[附加屬性概觀](attached-properties-overview.md)。 為了遵循這個主題中的範例，您也必須了解 XAML 並知道如何使用 C++、C# 或 Visual Basic 撰寫基本的 Windows 執行階段應用程式。

## <a name="scenarios-for-attached-properties"></a>附加屬性的案例

當定義類別以外的類別需要屬性設定機制的時候，就可以建立附加屬性。 最常見的案例是配置和服務支援。 現有配置屬性的範例包括 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772)。 在配置案例中，做為配置控制元素之子元素的元素，可以個別對它們的父元素表達配置需求，每個都會將父元素定義的屬性值設定為附加屬性。 Windows 執行階段 API 中服務支援案例的範例是一組 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 的附加屬性，如 [**ScrollViewer.IsZoomChainingEnabled**](https://msdn.microsoft.com/library/windows/apps/br209561)。

> [!WARNING]
> Windows 執行階段 XAML 實作的侷限是您無法動畫顯示自訂附加的屬性。

## <a name="registering-a-custom-attached-property"></a>登錄自訂附加屬性

如果定義的附加屬性純粹只是在其他類型上使用，登錄屬性的類別不一定要衍生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 不過，如果您遵循讓附加屬性也是相依性屬的典型模式，則存取子的 target 參數需要使用 **DependencyObject**，如此一來，您就可以使用支援屬性儲存區。

宣告類型 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 的 **public** **static** **readonly** 屬性，即可將附加屬性定義為相依性屬性。 使用 [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833) 方法的傳回值來定義這個屬性。 屬性名稱必須符合您指定為 **RegisterAttached** *name* 參數的附加屬性名稱，最後要加上字串 "Property"。 這是根據代表的屬性命名相依性屬性識別碼的既定慣例。

定義自訂附加屬性與自訂相依性屬性的主要差異在於定義存取子或包裝函式的方式。 而不是使用[自訂相依性屬性](custom-dependency-properties.md)中所述的包裝函式技術，您必須也提供靜態 **取得 * * * PropertyName*和 **設定 * * * PropertyName*方法存取子做為附加屬性。 存取子主要是由 XAML 剖析器使用，不過任何其他呼叫者也可以使用它們在非 XAML 案例中設定值。

> [!IMPORTANT]
> 如果您沒有正確定義存取子，XAML 處理器就無法存取您的附加的屬性，並嘗試使用它的任何人將可能收到 XAML 剖析器錯誤。 此外，設計和程式碼編寫工具遇到參考組件中的自訂相依性屬性時，通常會使用命名識別碼的 "\*Property" 慣例。

## <a name="accessors"></a>存取子

**Get**_PropertyName_ 存取子的簽章必須是這個。

`public static` _valueType_ **Get**_PropertyName_ `(DependencyObject target)`

針對 Microsoft Visual Basic，它是這個。

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_valueType_`)`

*target* 物件可以是實作中更具體的類型，但是必須衍生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 *valueType* 傳回值也可以是實作中的更具體類型。 基本 **Object** 類型是可以被接受的，不過通常您會希望您的附加屬性強制執行類型安全技術。 建議的類型安全技術是在 getter 和 setter 簽章中使用類型。

簽章 **設定 * * * PropertyName*存取子必須是這個。

`public static void Set`_PropertyName_` (DependencyObject target , `_valueType_` value)`

針對 Visual Basic，它是這個。

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_valueType_`)`

*target* 物件可以是實作中更具體的類型，但是必須衍生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 *value* 物件及它的 *valueType* 可以是實作中的更具體類型。 請記住，這個方法的值是 XAML 處理器在標記中遇到附加屬性時，來自於 XAML 處理器的輸入。 您使用的類型必須取得類型轉換或現有標記延伸的支援，這樣才能從屬性值建立適當的類型 (最終只是字串)。 基本 **Object** 類型是可以接受的，但是通常您會想要更進一步的類型安全性。 若要這樣做，請將類型強制放到存取子中。

> [!NOTE]
> 它也是可以定義附加的屬性的預定的用法是透過屬性元素語法。 在該情況下，您不需要輸入值的轉換，但必須確保您想要的值可以在 XAML 中建構。 [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) 是現有附加屬性的一個範例，這個屬性僅支援屬性元素用法。

## <a name="code-example"></a>程式碼範例

這個範例示範自訂附加屬性的相依性屬性登錄 (使用 [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833) 方法) 以及 **Get** 和 **Set** 存取子。 在範例中，附加屬性名稱是 `IsMovable`。 所以，存取子必須命名為 `GetIsMovable` 和 `SetIsMovable`。 附加屬性的擁有者是名為 `GameService` 的服務類別，這個類別沒有自己的 UI；其用途只是在使用 **GameService.IsMovable** 附加屬性時提供附加屬性服務。

定義附加的屬性，在 C + + /CX 是較為複雜。 您必須決定如何切分標頭與程式碼檔案。 另外，您應該將識別碼公開為只包含一個 **get** 存取子的屬性，原因請參閱[自訂相依性屬性](custom-dependency-properties.md)中的討論。 在 C + + /CX，您必須定義這個屬性欄位關係明確而不.NET **readonly**關鍵字和隱含信賴憑證者支援的簡單的屬性。 當應用程式第一次啟動，但在載入任何需要附加屬性的 XAML 頁面之前，您也需要在只執行一次的協助程式函式內執行附加屬性的登錄。 一般會針對任何和全部相依性或附加屬性呼叫屬性登錄協助程式函式的位置，是從 app.xaml 檔案程式碼中的 **App** / [**Application**](https://msdn.microsoft.com/library/windows/apps/br242325) 建構函式內呼叫。

```csharp
public class GameService : DependencyObject
{
    public static readonly DependencyProperty IsMovableProperty = 
    DependencyProperty.RegisterAttached(
      "IsMovable",
      typeof(Boolean),
      typeof(GameService),
      new PropertyMetadata(false)
    );
    public static void SetIsMovable(UIElement element, Boolean value)
    {
        element.SetValue(IsMovableProperty, value);
    }
    public static Boolean GetIsMovable(UIElement element)
    {
        return (Boolean)element.GetValue(IsMovableProperty);
    }
}
```

```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        Boolean IsMovable;
    }
}

// GameService.h
...
    bool IsMovable(){ return winrt::unbox_value<bool>(GetValue(m_IsMovableProperty)); }
    void IsMovable(bool value){ SetValue(m_IsMovableProperty, winrt::box_value(value)); }
    Windows::UI::Xaml::DependencyProperty IsMovableProperty(){ return m_IsMovableProperty; }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}

// GameService.cpp
#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="using-your-custom-attached-property-in-xaml"></a>在 XAML 中使用您的自訂附加屬性

定義您的附加屬性並將它的支援成員包含為自訂類型的一部分之後，您接著必須讓 XAML 可以使用定義。 若要這樣做，您必須對應將要參考包含相關類別程式碼命名空間的 XAML 命名空間。 在已經將附加屬性定義為程式庫一部分的情況中，您必須包含這個程式庫，讓它成為應用程式之應用程式套件的一部分。

XAML 的 XML 命名空間對應通常會放置在 XAML 頁面的根元素中。 例如，針對包含前面程式碼片段中所示之附加屬性定義的命名空間 `UserAndCustomControls` 中名為 `GameService` 的類別，其對應看起來就會像這樣。

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

使用對應即可在符合您目標定義的任何元素上設定您的 `GameService.IsMovable` 附加屬性，包括 Windows 執行階段定義的現有類型。

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

如果您在某個元素上設定屬性且該元素也在同一個對應的 XML 命名空間中，仍然需要在附加屬性名稱上包含前置詞。 這是因為前置詞會限定擁有者類型。 您不能假設附加屬性的屬性一定位於與包含屬性的元素相同的 XML 命名空間內；不過，根據一般的 XML 規則，屬性可以從元素繼承命名空間。 例如，如果您在自訂類型的 `ImageWithLabelControl` 上設定 `GameService.IsMovable` (未顯示定義)，而且即使這兩者是在對應到相同前置詞的同一個程式碼命名空間中定義的，XAML 仍然會是這個。

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> 如果您正在撰寫 XAML UI 搭配 c + +，您必須包含定義附加的屬性，任何時間的自訂類型的標頭 XAML 頁面使用該類型。 每個 XAML 頁面都有一個相關聯的 .xaml.h 程式碼後置標頭。 這裡是您應該包含 (使用 **\#include**) 附加屬性擁有者類型定義標頭的地方。

## <a name="value-type-of-a-custom-attached-property"></a>自訂附加屬性的值類型

做為自訂附加屬性值類型的類型會影響使用方式、定義或同時影響兩者。 附加屬性的值類型會在數個位置宣告：在 **Get** 與 **Set** 存取子兩個方法的簽章中，以及做為 [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833) 呼叫的 *propertyType* 參數。

最常見的附加屬性 (自訂或其他) 值類型是簡單字串。 這是因為附加屬性通常是用於 XAML 屬性，而將字串當作值類型能讓屬性變得更為精簡。 使用原生轉換為字串方法的其他基本類型 (例如整數、雙精度浮點數或列舉值) 也是附加屬性常用的值類型。 您可以使用其他值類型—不支援原生字串轉換的值類型—當作附加屬性值。 不過，這就取決於您要選擇使用或實作它：

- 您可以讓附加屬性保持原狀，但是附加屬性只能支援附加屬性是屬性元素且值被宣告為物件元素的使用方法。 在這種情況下，屬性類型不一定要支援使用 XAML 作為物件元素。 若為現有的 Windows 執行階段參考類別，請檢查 XAML 語法來確保類型支援 XAML 物件元素使用方法。
- 您可以讓附加屬性保持原狀，但是透過 XAML 參考技術 (如 **Binding** 或可以使用字串表示的 **StaticResource**) 僅在使用屬性時使用它。

## <a name="more-about-the-canvasleft-example"></a>進一步了解 **Canvas.Left** 範例

在前面的附加屬性用法範例中，我們示範了設定 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 附加屬性的不同方法。 但是，這對於 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 與您物件的互動方式有何改變，以及在何時發生？ 我們將進一步探討這個特定的範例，因為如果您實作附加屬性，看看當典型的附加屬性擁有者類別在其他物件上發現它的附加屬性值時，會對這些值做哪些其他處理，將是一件有趣的事。

[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 的主要功能是成為 UI 中的絕對位置配置容器。 **Canvas** 的子項會儲存在基底類別定義的屬性 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 中。 在所有的面板中，**Canvas** 是唯一使用絕對位置的面板。 當新增屬性而屬性可能只與 **Canvas** 和它們身為 **UIElement** 子元素的特定 **UIElement** 案例相關時，會將常見的 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 類型的物件模型塞得臃腫不堪。 將 **Canvas** 的配置控制項屬性定義成任何 **UIElement** 都可以使用的附加屬性可以讓物件模型保持簡潔。

為了能夠成為實際的面板，[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 具有會覆寫架構層級的 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 和 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法的行為。 這是 **Canvas** 實際檢查其子項是否有附加屬性值的位置。 **Measure** 和 **Arrange** 模式都有部分是迴圈，會逐一查看任何內容，而面板具有 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 屬性，可以明確指出哪個應該視為面板子項。 因此，**Canvas** 配置行為會逐一查看這些子項，並且在每個子項進行靜態的 [**Canvas.GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) 和 [**Canvas.GetTop**](https://msdn.microsoft.com/library/windows/apps/br209270) 呼叫，以查看那些附加屬性是否包含非預設值 (預設值為 0)。 然後，會使用這些值並根據每個子項提供的特定值，以賦予每個子項在 **Canvas** 可用配置空間中的絕對位置，然後使用 **Arrange** 來進行認可。

程式碼看起來就像這個虛擬程式碼。

```syntax
protected override Size ArrangeOverride(Size finalSize)
{
    foreach (UIElement child in Children)
    {
        double x = (double) Canvas.GetLeft(child);
        double y = (double) Canvas.GetTop(child);
        child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
    }
    return base.ArrangeOverride(finalSize); 
    // real Canvas has more sophisticated sizing
}
```

> [!NOTE]
> 如需面板運作方式的詳細資訊，請參閱[XAML 自訂面板概觀](https://msdn.microsoft.com/library/windows/apps/mt228351)。

## <a name="related-topics"></a>相關主題

* [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)
* [附加屬性概觀](attached-properties-overview.md)
* [自訂相依性屬性](custom-dependency-properties.md)
* [XAML 概觀](xaml-overview.md)
