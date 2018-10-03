---
author: jwmsft
description: 說明如何針對使用 C++、C# 或 Visual Basic 的 Windows 執行階段 app 定義及實作自訂相依性屬性。
title: 自訂相依性屬性
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.author: jimwalk
ms.date: 07/12/2018
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
ms.openlocfilehash: ddeccfe4c5e198afd77eaa4a81fc017543291ba1
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4315908"
---
# <a name="custom-dependency-properties"></a>自訂相依性屬性

我們將在此處說明如何針對使用 C++、C# 或 Visual Basic 的 Windows 執行階段應用程式定義和實作您自己的相依性屬性。 我們會列出為什麼應用程式開發人員及元件撰寫人員要建立自訂相依性屬性的理由。 我們不但描述了自訂相依性屬性的實作步驟，也會描述一些可以改善相依性屬性的效能、可用性或多樣性的最佳做法。

## <a name="prerequisites"></a>先決條件

我們假設您已經閱讀過[相依性屬性概觀](dependency-properties-overview.md)，而且也了解現有相依性屬性使用者對於相依性屬性的觀點。 為了遵循這個主題中的範例，您也必須了解 XAML 並知道如何使用 C++、C# 或 Visual Basic 撰寫基本的 Windows 執行階段應用程式。

## <a name="what-is-a-dependency-property"></a>什麼是相依性屬性？

若要支援樣式、資料繫結、動畫以及屬性的預設值，則應實作為相依性屬性。 相依性屬性值不會儲存為類別上的欄位，它們會由 xaml 架構儲存並使用索引鍵來參照，該索引鍵會在呼叫 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 方法將屬性登錄到 Windows 執行階段屬性系統時進行擷取。   相依性屬性只能由衍生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的類型使用。 但是 **DependencyObject** 位於類別階層中很高的位置，所以用於 UI 和呈現的大部分類別也可以支援相依性屬性。 如需相依性屬性以及本文件中一些詞彙及描述它們慣例的詳細資訊，請參閱[相依性屬性概觀](dependency-properties-overview.md)。

Windows 執行階段中相依性屬性的範例包括：[**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395)、[**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 以及 [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702) (這只是其中一部分)。

慣例是由類別公開的每個相依性屬性都具有類型 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 的對應 **public static readonly** 屬性，它會在相同的類別上公開，而且提供相依性屬性的識別碼。 識別碼名稱會遵循這個慣例：相依性屬性的名稱，並在名稱最後面附加字串 "Property"。 例如，**Control.Background** 屬性的對應 **DependencyProperty** 識別碼是 [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396)。 識別碼會儲存登錄時的相依性屬性資訊，之後只要有其他操作需要使用相依性屬性 (如呼叫 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361))，就可以使用。

## <a name="property-wrappers"></a>屬性包裝函式

相依性屬性通常具有包裝函式實作。 如果沒有包裝函式，取得或設定屬性的唯一方法就是使用相依性屬性公用程式方法 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 和 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 並將識別碼當作參數傳遞給它們。 這對於表面上是屬性的物件而言不是尋常的做法。 但是如果使用包裝函式，那麼您的程式碼以及參考相依性屬性的任何其他程式碼就可以使用直接的物件屬性語法，這對您所使用的語言而言是很自然的語法。

如果您自己實作自訂相依性屬性並希望它成為公用的且容易被呼叫，那麼也請定義屬性包裝函式。 將相依性屬性的基本資訊報告給反映或靜態分析處理程序時，屬性包裝函式也非常實用。 具體而言，包裝函式就是放置 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 之類屬性的地方。

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>何時將屬性實作為相依性屬性

每當您在類別上實作公用讀/寫屬性時，只要您的類別衍生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)，您就可以選擇將屬性當作相依性屬性來使用。 有時將屬性的標準技術與私用欄位搭配是適當的做法。 將自訂屬性定義為相依性屬性也不一定是必要或適當的方式。 如何選擇取決於您希望屬性支援什麼情況而定。

如果您想要屬性支援 Windows 執行階段或 Windows 執行階段應用程式的一或多個功能時，就可以考慮將屬性實作為相依性屬性：

- 透過 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 設定屬性
- 做為與 [**{Binding}**](binding-markup-extension.md) 繫結之資料的有效目標屬性
- 透過 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) 支援動畫值
- 當下列情況變更屬性的值時回報：
  - 屬性系統自己採取的動作
  - 環境
  - 使用者動作
  - 讀取和寫入樣式

## <a name="checklist-for-defining-a-dependency-property"></a>定義相依性屬性的檢查清單

定義相依性屬性可以想成是一組概念。 這些概念不一定是依序執行的步驟，因為數個概念可以放置在實作的單行程式碼中。 這個清單只提供簡略概觀。 我們稍後會在這個主題中詳細說明每個概念，並為您示範不同語言的範例程式碼。

- 在屬性系統中登錄屬性名稱 (呼叫 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829))，指定擁有者類型及屬性值的類型。
  - [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 有一個必要參數需要使用屬性中繼資料。 為此指定 **null**，或者如果您想要屬性變更行為，或可以透過呼叫 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 還原的中繼資料預設值，請指定 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.propertymetadata) 執行個體。
- 在擁有者類型上將 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 識別碼定義為 **public static readonly** 屬性成員。
- 定義包裝函式屬性，遵循您實作的語言中所使用的屬性存取子模型。 包裝函式屬性名稱必須符合 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 中使用的 *name* 字串。 實作 **get** 和 **set** 存取子，將包裝函式連接到它所包裝的相依性屬性，方法是呼叫 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 和 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 並將您自己的屬性識別碼當作參數傳遞。
- (選用) 將 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 之類的屬性放置到包裝函式中。

> [!NOTE]
> 如果您正在定義自訂附加的屬性，則通常會省略包裝函式。 相反地，您要撰寫 XAML 處理器可以使用的不同樣式存取子。 請參閱[自訂附加屬性](custom-attached-properties.md)。 

## <a name="registering-the-property"></a>登錄屬性

為了讓您的屬性可以成為相依性屬性，您必須將屬性登錄到 Windows 執行階段屬性系統所維護的屬性儲存區中。  若要登錄屬性，您可以呼叫 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 方法。

若為 Microsoft .NET 語言 (C# 和 Microsoft Visual Basic)，您可以在類別的內文中呼叫 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) (在類別內，但在任何成員定義外)。 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 方法呼叫也會提供識別碼做為傳回值。 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 呼叫通常是做為靜態建構函式或是類型 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 的 **public static readonly** 屬性初始化的一部份 (做為您類別的一部份)。 這個屬性會公開您相依性屬性的識別碼。 以下是 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 呼叫的範例。

> [!NOTE]
> 相依性屬性登錄為一部分的識別碼屬性定義是典型的實作，但您也可以在類別靜態建構函式中登錄相依性屬性。 如果您需要一行以上的程式碼來初始化相依性屬性，這個方法比較適用。

針對 C + + /CX，您必須選擇如何分割標頭和程式碼檔案之間的實作。 典型的分割方式是在標頭將識別碼本身宣告為 **public static** 屬性，搭配 **get** 實作但不使用 **set**。 **get** 實作會參考私用欄位，它是未初始化的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 執行個體。 您也可以宣告包裝函式以及包裝函式的 **get** 和 **set** 實作。 在這個情況下，標頭會包含一些基本的實作。 如果包裝函式需要 Windows 執行階段屬性，那麼標頭中也會包含屬性。 在程式碼檔案中放置 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 呼叫，位置是在只會在應用程式第一次初始化時執行的協助程式函式中。 使用 **Register** 的傳回值來填入您在標頭中宣告的靜態但未初始化的識別碼，您可以在實作檔案的根範圍內初始設為 **nullptr**。

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> C + /CX 程式碼，為什麼您有私用欄位，而且表面[**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)的公用唯讀屬性是，這樣其他呼叫者使用您相依性屬性也可以使用屬性系統公用程式 Api 要求的原因成為公用的識別碼。 如果讓識別碼保持私用，別人就無法使用這些公用程式 API。 這類 API 的範例和案例包括 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 或 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) (選用)、[**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357)、[**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358)、[**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) 以及 [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836)。 您無法為此使用公用欄位，因為 Windows 執行階段中繼資料的規則並不允許公用欄位。

## <a name="dependency-property-name-conventions"></a>相依性屬性名稱慣例

相依性屬性有命名慣例；除了例外情況之外，請在所有情況中遵循這種命名慣例。 相依性屬性本身具有基本名稱 (在前面的範例中是 "Label")，它是以 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 的第一個參數來指定的。 這個名稱在每個登錄類型中必須是唯一的，而且這種唯一性的需求也適用於任何繼承的成員。 經由基本類型而繼承的相依性屬性會被視為登錄類型的一部分；而繼承的屬性名稱不可被重複登錄。

> [!WARNING]
> 雖然您提供以下可以是任何字串識別碼的名稱，是在您所選擇的語言進行程式設計中有效，但您通常會想要能夠太在 XAML 中設定您相依性屬性。 為了能夠在 XAML 中設定，您選擇的屬性名稱必須是有效的 XAML 名稱。 如需詳細資訊，請參閱 [XAML 概觀](xaml-overview.md)。

在您建立識別碼屬性時，請合併登錄時的屬性名稱與尾碼 "Property" (例如，"LabelProperty")。 這個屬性就是您相依性屬性的識別碼，當您在自己的屬性包裝函式中進行 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 和 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 呼叫時，它就會是您的輸入。 它也會由屬性系統和其他 XAML 處理器所使用，例如
[**{x:Bind}**](x-bind-markup-extension.md)

## <a name="implementing-the-wrapper"></a>實作包裝函式

您的屬性包裝函式應該在 **get** 實作中呼叫 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)，以及在 **set** 實作中呼叫 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)。

> [!WARNING]
> 在所有除了例外情況下，您的包裝函式實作應該執行只在[**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)與[**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)作業。 否則，透過 XAML 和透過程式碼設定的屬性會出現不同的行為。 為求效率，當設定相依性屬性時 XAML 剖析器會略過包裝函式；並透過 **SetValue** 與備份存放區通訊。

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>自訂相依性屬性的屬性中繼資料

屬性中繼資料被指派給相依性屬性時，會針對屬性擁有者類型的每個執行個體或它的子類別，將相同的中繼資料套用到這個屬性。 在屬性中繼資料中，您可以指定兩種行為：

- 屬性系統指派給所有屬性的預設值。
- 只要偵測到屬性值變更時，屬性系統內部自動叫用的靜態回呼方法。

### <a name="calling-register-with-property-metadata"></a>使用屬性中繼資料呼叫登錄

在先前呼叫 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 的範例中，我們為 *propertyMetadata* 參數傳遞了 Null 值。 若要讓相依性屬性提供預設值或使用屬性變更的回呼，您必須定義提供下列一或兩項功能的 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 執行個體。

通常您會在 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 的參數內提供 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 做為內嵌建立的執行個體。

> [!NOTE]
> 如果您正在定義[**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812)實作，您必須使用公用程式方法[**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702099) ，而不是呼叫[**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)建構函式來定義**PropertyMetadata**執行個體。

下一個範例參考一個具有 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 值的 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 執行個體，修改了先前顯示的 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 範例。 "OnLabelChanged" 回呼的實作稍後會在本節中說明。

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>預設值

您可以指定某個相依性屬性的預設值，使得該屬性在未設定的情況下始終傳回特定的預設值。 這個值可以不同於該屬性類型的固有預設值。

如果未指定預設值，相依性屬性的參考類型的預設值為 Null，或是值類型或語言基本類型的預設類型 (例如，整數為 0 或字串為空字串)。 建立預設值的主要原因在於當您在屬性上呼叫 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 的時候會還原這個值。 在每個屬性上建立預設值可能比在建構函式中建立預設值更方便，特別是對值類型而言。 不過，對於參考類型來說，請確定建立預設值不會建立不想要的單一執行個體模式。 如需詳細資訊，請參閱這個主題中後面的[最佳做法](#best-practices)。

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> 不會向登錄[**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371)的預設值。 這樣做會混淆屬性使用者，而且會在屬性系統內會造成不想要的結果。

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

在某些情況下，您會為在一個以上的 UI 執行緒上使用的物件定義相依性屬性。 如果您正在定義多個應用程式使用的資料物件，或是用在一個以上的應用程式的控制項，可能就是這種情況。 您可以提供 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 實作 (而非預設值執行個體，其已繫結至登錄屬性的執行緒)，以在不同的 UI 執行緒之間交換物件。 基本上，[**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 會定義預設值的 Factory。 **CreateDefaultValueCallback** 傳回的值始終與正在使用物件的目前的 UI **CreateDefaultValueCallback** 執行緒關聯。

若要定義指定 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 的中繼資料，必須呼叫 [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115) 以傳回中繼資料執行個體；[**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 建構函式沒有包含 **CreateDefaultValueCallback** 參數的簽章。

[**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 典型的實作模式是建立新的 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 類別、將 **DependencyObject** 的每個屬性的特定屬性值設定為想要的預設值，然後透過 **CreateDefaultValueCallback** 方法的傳回值傳回新類別以做為 **Object** 參考。

### <a name="property-changed-callback-method"></a>屬性變更回呼方法

您可以定義屬性變更回呼方法以便定義您的屬性與其他相依性屬性的互動，或是在屬性變更時更新內部屬性或物件的狀態。 如果叫用您的回呼，就表示屬性系統已判斷其中有已變更的有效屬性值。 因為回呼方法是靜態的，所以回呼的 *d* 參數很重要，因為它會告訴您類別的哪個執行個體報告了變更。 典型的實作會使用事件資料的 [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) 屬性並以某種方法來處理這個值，常見的做法是在當作 *d* 傳遞的物件上執行一些其他的變更。 對於屬性變更的其他回應是拒絕 **NewValue** 報告的值、還原 [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365)，或是將值設定成套用到 **NewValue** 的程式設計限制式。

這個接下來的範例示範 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 實作。 它將您在前面的 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 範例中看到的參考方法實作為 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 的建構引數的一部分。 這個回呼所述的案例說明類別也具有名為 "HasLabelValue" 的計算唯讀屬性 (未顯示實作)。 只要重新評估 "Label" 屬性，就會叫用這個回呼方法，而且回呼能夠讓相依的計算值與相依性屬性的變更同步。

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>結構與列舉的屬性變更行為

如果 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 的類型是列舉或結構，即使結構或列舉值的內部值未變更，也可以叫用回呼。 這與系統基元 (如字串，只有在值變更時才會叫用) 不同。 這是這些值的 Box 和 Unbox 操作在內部產生的副作用。 如果您的屬性使用 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 方法，而且您的值是列舉或結構，您必須自行轉換值，並使用 now-cast 值可用的超載比較運算子，以比較 [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) 與 [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364)。 或者，如果沒有這類運算子 (自訂結構可能會發生這種情況)，您可能必須比較個別的值。 如果結果是值並未變更，您通常會選擇什麼都不做。

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>最佳做法

若要以最佳做法定義自訂的相依性屬性，請考量下列幾點。

### <a name="dependencyobject-and-threading"></a>DependencyObject 和執行緒

所有的 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 執行個體都必須在 UI 執行緒上建立，而這個執行緒與 Windows 執行階段 app 所顯示的目前 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 關聯。 雖然每個 **DependencyObject** 都必須在主 UI 執行緒上建立，但是只要呼叫 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616)，即可使用其他緒行緒的發送器參考來存取物件。

[**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的執行緒層面都是相關的，因為它通常表示只有在 UI 執行緒上執行的程式碼才可以變更或甚至是讀取相依性屬性的值。 在一般的 UI 程式碼中通常可以避免緒行緒處理的問題，因為它能夠正確使用 **async** 模式及背景工作者執行緒。 通常您只會在定義自己的 **DependencyObject** 類型並且嘗試在 **DependencyObject** 不適用的資料來源或其他案例中使用這些類型時，才會遇到 **DependencyObject** 相關的執行緒處理問題。

### <a name="avoiding-unintentional-singletons"></a>避免不想要的單一執行個體

如果您宣告使用參考類型的相依性屬性，而且在建立 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 的程式碼中為這個參考類型呼叫建構函式，就可能出現不想要的單一執行個體。 因為所有相依性屬性使用時只會共用 **PropertyMetadata** 的一個執行個體，所以會嘗試共用您建構的單個參考類型。 接著您透過相依性屬性所設定之這個值類型的任何子屬性，可能會以您不想要的方式傳播到其他物件。

如果您想要非 Null 值，可以使用類別建構函式來設定參考類型相依性屬性的初始值，但是請注意，基於[相依性屬性概觀](dependency-properties-overview.md)的用途，它會被視為本機值。 如果您的類別支援範本的話，較適當的做法是使用範本。 避免出現單一執行個體模式但仍然提供有用預設值的另一種方式，是在為該類別提供適用預設值的參考類型上公開靜態屬性。

### <a name="collection-type-dependency-properties"></a>集合類型相依性屬性

集合類型相依性屬性需要考慮一些其他的實作問題。

集合類型相依性屬性在 Windows 執行階段 API 中相對是較為少見的。 在大多數情況下，您可以在項目是 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 子類別的地方使用集合，但是集合屬性本身會被當作傳統的 CLR 或 C++ 屬性來實作。 因為使用相依性屬性時，一些典型案例可能不適合使用集合。 例如：

- 您通常不會為集合建立動畫效果。
- 您通常不會使用樣式或範本在集合中預先填入項目。
- 雖然繫結到集合是主要的案例，但是集合不一定必須是相依性屬性才能成為繫結來源。 對於繫結目標來說，最常見的做法是使用 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) 或 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) 的子類別來支援集合項目，或是使用檢視模型模式。 如需與集合相互繫結的詳細資訊，請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)。
- 透過 **INotifyPropertyChanged** 或 **INotifyCollectionChanged** 之類的介面，或是從 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) 衍生集合類型，可以較好的處理集合變更的通知。

儘管如此，還是有適用於集合類型相依性屬性的案例。 接下來三個小節提供一些如何實作集合類型相依性屬性的指導方針。

### <a name="initializing-the-collection"></a>初始化集合

建立相依性屬性時，可以透過相依性屬性中繼資料來建立預設值。 但是請小心，不要使用單一靜態集合作為預設值。 而是必須特意將集合值設定成唯一 (執行個體) 集合，做為集合屬性之擁有者類別的類別建構函式邏輯一部分。

### <a name="change-notifications"></a>變更通知

將集合定義為相依性屬性時，並不會因為屬性系統叫用 "PropertyChanged" 回呼方法的本質，而為集合中的項目自動提供變更通知。 如果您希望收到集合或集合項目的通知—例如資料繫結案例—請實作 **INotifyPropertyChanged** 或 **INotifyCollectionChanged** 介面。 如需詳細資訊，請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)。

### <a name="dependency-property-security-considerations"></a>相依性屬性安全性考量

將相依性屬性宣告為公用屬性。 將相依性屬性識別碼宣告為 **public static readonly** 成員。 即使您嘗試宣告語言允許的其他存取層級 (如 **protected**)，您仍然可以透過與屬性系統 API 合併使用的識別碼來存取相依性屬性。 將相依性屬性識別碼宣告為內部或私用並沒有任何作用，因為這樣一來屬性系統會無法正常運作。

包裝函式屬性只是為了便利性而設，套用到包裝函式的安全性機制只需要改為呼叫 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 或 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 就可以被略過。 所以請將包裝函式保持為公用的；否則您只會讓您的屬性較不易被正當的呼叫者使用，而且不會提供任何實質的安全性優勢。

Windows 執行階段不提供將自訂相依性屬性登錄為唯讀的方法。

### <a name="dependency-properties-and-class-constructors"></a>相依性屬性與類別建構函式

類別建構函式不應該呼叫虛擬方法是一個基本的原則。 這是因為呼叫建構函式可以完成衍生類別建構函式的基本初始化，而且在建構的物件執行個體尚未完全初始化的時候，可能會透過建構函式進入虛擬方法。 當您從已經自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 衍生的類別衍生時，請記住屬性系統本身會在內部呼叫和公開虛擬方法作為服務的一部分。 為了避免在執行階段初始化出現這類問題，請不要在類別的建構函式內設定相依性屬性值。

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>針對 C++/CX 應用程式登錄相依性屬性

在 C++/CX 中登錄屬性的實作比 C# 更需要技巧，這是因為兩者都需要分成標頭和實作檔，同時也因為在實作檔案的根範圍內進行初始化就是一個不良做法所致 (Visual C++ 元件延伸 (C++/CX) 會將靜態初始設定式程式碼從根範圍直接放入 **DllMain**，其中 C# 編譯器會將靜態初始設定式指派至類別，從而避免 **DllMain** 載入鎖定問題)。 此處的最佳做法是宣告一個協助程式函式，為類別進行所有的相依性屬性登錄，而且每個類別都需要一個函式。 然後，針對應用程式取用的每個自訂類別，您必須參照每個想要使用的自訂類別所公開的協助程式登錄函式。 在 `InitializeComponent` 之前，於 [**Application 建構函式**](https://msdn.microsoft.com/library/windows/apps/br242325) (`App::App()`) 中呼叫每個協助程式登錄函式一次。 例如，該建構函式只會在第一次真正參照應用程式時執行，如果是暫停的應用程式繼續執行，它將不會再次執行。 此外，如先前的 C++ 登錄範例所示，在每個 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 呼叫周圍的 **nullptr** 檢查非常重要：這可保證函式中不會有其他呼叫者可以登錄該屬性兩次。 如果沒有這類檢查，第二個登錄呼叫可能會因為屬性名稱重複而毀損您的應用程式。 如果您需要查看範例程式碼的 C++/CX 版本，可以在 [XAML 使用者和自訂控制項範例](http://go.microsoft.com/fwlink/p/?linkid=238581)中查看這個實作模式。

## <a name="related-topics"></a>相關主題

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
- [相依性屬性概觀](dependency-properties-overview.md)
- [XAML 使用者和自訂控制項範例](http://go.microsoft.com/fwlink/p/?linkid=238581)
 