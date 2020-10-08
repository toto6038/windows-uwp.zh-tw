---
description: 本主題詳細說明將 [C#](/visualstudio/get-started/csharp) 專案中的原始程式碼移植到其在 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 中的對等項目時所涉及的技術詳細資料。
title: 從 C# 移到 C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 遷移, C#
ms.localizationpriority: medium
ms.openlocfilehash: 353ca9922bc633efa5f53b2c3a3f4d7a4cad5986
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750614"
---
# <a name="move-to-cwinrt-from-c"></a>從 C# 移到 C++/WinRT

本主題詳細說明將 [C#](/visualstudio/get-started/csharp) 專案中的原始程式碼移植到其在 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 中的對等項目時所涉及的技術詳細資料。

如需移植其中一個通用 Windows 平台 (UWP) 應用程式範例的案例研究，請參閱附屬主題：[將剪貼簿範例從 C# 移植到 C++/WinRT](./clipboard-to-winrt-from-csharp.md)。 您可以遵循逐步解說來獲得移植的方法和體驗，然後再自行移植範例。

## <a name="how-to-prepare-and-what-to-expect"></a>如何準備，以及預期的事項

[將剪貼簿範例從 C# 移植到 C++/WinRT](./clipboard-to-winrt-from-csharp.md) 的案例研究會說明將專案移植到 C++/WinRT 時所需的各種軟體設計決策範例。 因此，您應先了解現有程式碼的運作方式，再準備進行移植。 如此一來，您就可以大致了解應用程式的功能，以及程式碼的結構，然後您所做的決定將會讓您向前邁進，並以正確的方向進行。

就預期的移植變更種類而言，您可以將其分為四個類別。

- [**移植語言投影**](#changes-that-involve-the-language-projection)。 Windows 執行階段 (WinRT) 會「投影」到各種程式設計語言中。 每一個語言投影都會設計成所述程式設計語言慣用的型態。 針對 C#，某些 Windows 執行階段類型會投影為 .NET 類型。 例如，您會將 [**System.Collections.Generic.IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1) 轉譯回 [**Windows.Foundation.Collections.IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)。 此外，在 C# 中，某些 Windows 執行階段作業會投影為方便的 C# 語言功能。 例如，您會在 C# 使用 `+=` 運算子語法來註冊事件處理委派。 因此，您會把這類語言功能轉譯回要執行的基本作業 (在此範例中為事件註冊)。
- [**移植語言語法**](#changes-that-involve-the-language-syntax)。 這些變更有許多都是簡單的機械轉換，也就是將符號取代為另一個符號。 例如，將點 (`.`) 變更為雙冒號 (`::`)。
- [**移植語言程序**](#changes-that-involve-procedures-within-the-language)。 這其中有一些是簡單的重複變更 (例如將 `myObject.MyProperty` 變更為 `myObject.MyProperty()`)。 而其他則是更深入的變更 (例如，將牽涉到使用 **System.Text.StringBuilder** 的程序移植到涉及使用 **std::wostringstream** 的程序)。
- [**C++/WinRT 特定的移植相關工作**](#porting-related-tasks-that-are-specific-to-cwinrt)。 Windows 執行階段的特定詳細資料會透過 C# 在幕後進行處理。 這些詳細資料會在 C++/WinRT 中明確執行。 例如，您可以使用 `.idl` 檔案來定義您的執行階段類別。

此主題的其餘部分會根據該分類法進行結構化。

## <a name="changes-that-involve-the-language-projection"></a>涉及語言投射的變更

| 類別 | C# | C++/WinRT | 另請參閱 |
| -------- | -- | --------- | -------- |
|不具類型的物件|`object` 或 [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[移植 **EnableClipboardContentChangedNotifications** 方法](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|投影命名空間|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|集合的大小|`collection.Count`|`collection.Size()`|[移植 **BuildClipboardFormatsOutputString** 方法](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|典型集合類型|[**IList\<T\>** ](/dotnet/api/system.collections.generic.ilist-1)，然後 [新增] 以新增元素。|[**IVector\<T\>** ](/uwp/api/windows.foundation.collections.ivector-1)，然後 [附加] 以新增元素。 如果您使用 **std::vector** 任何位置，則 **push_back** 以新增元素。||
|唯讀集合類型|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[移植 **BuildClipboardFormatsOutputString** 方法](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|事件處理常式委派為類別成員|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[移植 **EnableClipboardContentChangedNotifications** 方法](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|撤銷事件處理常式委派|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[移植 **EnableClipboardContentChangedNotifications** 方法](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|關聯容器|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|向量成員存取|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>註冊/撤銷事件處理常式

在 C++/WinRT 中，您有數個語法選項可供您註冊/撤銷事件處理常式委派，如[藉由在 C++/WinRT 使用委派來處理事件](./handle-events.md)中所述。 另請參閱[移植 **EnableClipboardContentChangedNotifications** 方法](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications).

例如，有時候事件收件者 (處理事件的物件) 即將終結時，您可以撤銷事件處理常式，讓事件來源 (引發事件的物件) 不會呼叫已終結的物件。 請參閱[撤銷已註冊的委派](./handle-events.md#revoke-a-registered-delegate)。 在這種情況下，請為事件處理常式建立 **event_token** 成員變數。 如需範例，請參閱[移植 **EnableClipboardContentChangedNotifications** 方法](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)。

您也可以在 XAML 標記中註冊事件處理常式。

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

在 C# 中，**OpenButton_Click** 方法可以是私有的，而且 XAML 仍可將其連線到 *OpenButton* 所引發的 [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。

在 C++/WinRT 中，「如果您想要在 XAML 標記中註冊 **OpenButton_Click** 方法」，則該方法在[實作類型](./author-apis.md)中必須是公用的。 如果您只在命令式程式碼中註冊事件處理常式，則事件處理常式不需要是公用的。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

或者，您可以讓註冊 XAML 頁面成為實作類型的同伴，並且讓 **OpenButton_Click** 成為私有的。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

最後一個案例是您要移植的 C# 專案從標記*繫結*到事件處理常式 (如需更多案例背景，請參閱 [Functions in x:Bind](../data-binding/function-bindings.md))。

```xaml
<Button x:Name="OpenButton" Click="{x:Bind OpenButton_Click}" />
```

您可以將該標記變更為更簡單的 `Click="OpenButton_Click"`。 或者，如果想要的話，也可以保留該標記不變。 為了加以支援，您只需要在 IDL 中宣告事件處理常式。

```idl
void OpenButton_Click(Object sender, Windows.UI.Xaml.RoutedEventArgs e);
```

> [!NOTE]
> 將函式宣告為 `void` (即使您*實作*為[「射後不理」(Fire and Forget)](./concurrency-2.md#fire-and-forget))。

## <a name="changes-that-involve-the-language-syntax"></a>涉及語言語法的變更

| 類別 | C# | C++/WinRT | 另請參閱 |
| -------- | -- | --------- | -------- |
|存取修飾詞|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[移植 **Button_Click** 方法](./clipboard-to-winrt-from-csharp.md#button_click)|
|存取資料成員|`this.variable`|`this->variable`||
|非同步動作|`async Task ...`|`IAsyncAction ...`||
|非同步作業|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|射後不理 (Fire-and-forget) 方法 (暗指非同步)|`async void ...`|`winrt::fire_and_forget ...`|[移植 **CopyButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|存取列舉常數|`E.Value`|`E::Value`|[移植 **DisplayChangedFormats** 方法](./clipboard-to-winrt-from-csharp.md#displaychangedformats)|
|合作等待|`await ...`|`co_await ...`|[移植 **CopyButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|作為私人欄位的預測類型集合|`private List<MyRuntimeClass> myRuntimeClasses = new List<MyRuntimeClass>();`|`std::vector`<br>`<MyNamespace::MyRuntimeClass>`<br>`m_myRuntimeClasses;`||
|GUID 結構|`private static readonly Guid myGuid = new Guid("C380465D-2271-428C-9B83-ECEA3B4A85C1");`|`winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };`||
|命名空間分隔符號|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[移植 **UpdateStatus** 方法](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|取得類型物件|`typeof(MyType)`|`winrt::xaml_typename<MyType>()`|[移植 **Scenarios** 屬性](./clipboard-to-winrt-from-csharp.md#scenarios)|
|方法的參數宣告|`MyType`|`MyType const&`|[參數傳遞](./concurrency.md#parameter-passing)|
|非同步方法的參數宣告|`MyType`|`MyType`|[參數傳遞](./concurrency.md#parameter-passing)|
|呼叫靜態方法|`T.Method()`|`T::Method()`||
|字串|`string` 或 **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[C++/WinRT 中的字串處理](./strings.md)|
|字串常值|`"a string literal"`|`L"a string literal"`|[移植建構函式 **Current** 和 **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|推斷 (或推算) 的類型|`var`|`auto`|[移植 **BuildClipboardFormatsOutputString** 方法](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|使用指示詞|`using A.B.C;`|`using namespace A::B::C;`|[移植建構函式 **Current** 和 **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|逐字/原始字串常值|`@"verbatim string literal"`|`LR"(raw string literal)"`|[移植 **DisplayToast** 方法](./clipboard-to-winrt-from-csharp.md#displaytoast)|

> [!NOTE]
> 如果標頭檔不包含指定命名空間的 `using namespace` 指示詞，則您必須完整限定該命名空間的所有類型名稱；或者至少要限定在足以讓編譯器找到這些名稱的範圍。 如需範例，請參閱[移植 **DisplayToast** 方法](./clipboard-to-winrt-from-csharp.md#displaytoast)。

### <a name="porting-classes-and-members"></a>移植類別與成員

針對每個 C# 類型，您都必須決定要將其移植到 Windows 執行階段類型，還是一般 C++ 類別/結構/列舉。 如需詳細資訊，以及說明如何做出這些決策的詳細範例，請參閱[將剪貼簿範例從 C# 移植到 C++/WinRT](./clipboard-to-winrt-from-csharp.md)。

C# 屬性通常會成為存取子函式、更動子函式和支援資料成員。 如需詳細資訊和範例，請參閱[移植 **IsClipboardContentChangedEnabled** 屬性](./clipboard-to-winrt-from-csharp.md#isclipboardcontentchangedenabled)。

對於非靜態欄位，請將其設為 [實作類型](./author-apis.md)的資料成員。

C# 靜態欄位會成為 C++/WinRT 靜態存取子和 (或) 更動子涵式。 如需詳細資訊和範例，請參閱[移植建構函式 **Current** 和 **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)。

同樣地，對於成員函式，您必須決定每個成員函式是否屬於 IDL，或是否為實作類型的公用或私有成員函式。 如需詳細資訊，以及如何決定的範例，請參閱 [**MainPage** 類型的 IDL](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type)。

### <a name="porting-xaml-markup-and-asset-files"></a>移植 XAML 標記和資產檔案

在[將剪貼簿範例從 C# 移植到 C++/WinRT](./clipboard-to-winrt-from-csharp.md) 案例中，我們可以在 C# 和 C++/WinRT 專案中使用「相同」XAML 標記 (包括資源) 和資產檔案。 在某些情況下，您需要編輯標記來達到此目的。 請參閱[複製完成移植 **MainPage** 所需的 XAML 和樣式](./clipboard-to-winrt-from-csharp.md#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage)。

## <a name="changes-that-involve-procedures-within-the-language"></a>涉及語言內程序的變更

| 類別 | C# | C++/WinRT | 另請參閱 |
| -------- | -- | --------- | -------- |
|非同步方法中的存留期管理|不適用|`auto lifetime{ get_strong() };` 或<br>`auto lifetime = get_strong();`|[移植 **CopyButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|處置|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[移植 **CopyImage** 方法](./clipboard-to-winrt-from-csharp.md#copyimage)|
|建構物件|`new MyType(args)`|`MyType{ args }` 或<br>`MyType(args)`|[移植 **Scenarios** 屬性](./clipboard-to-winrt-from-csharp.md#scenarios)|
|建立未初始化的參考|`MyType myObject;`|`MyType myObject{ nullptr };` 或<br>`MyType myObject = nullptr;`|[移植建構函式 **Current** 和 **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|使用引述將物件建構到變數中|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` 或 <br>`auto myObject{ MyType(args) };` 或 <br>`auto myObject = MyType{ args };` 或 <br>`auto myObject = MyType(args);` 或 <br>`MyType myObject{ args };` 或 <br>`MyType myObject(args);`|[移植 **Footer_Click** 方法](./clipboard-to-winrt-from-csharp.md#footer_click)|
|在無需引數的情況下，將物件建構到變數中|`var myObject = new T();`|`MyType myObject;`|[移植 **BuildClipboardFormatsOutputString** 方法](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|物件初始化速記|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|大量向量作業|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[移植 **CopyButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|反復查看集合|`foreach (var v in c)`|`for (auto&& v : c)`|[移植 **BuildClipboardFormatsOutputString** 方法](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|攔截例外狀況|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[移植 **PasteButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|例外狀況詳細資料|`ex.Message`|`ex.message()`|[移植 **PasteButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|取得屬性值|`myObject.MyProperty`|`myObject.MyProperty()`|[移植 **NotifyUser** 方法](./clipboard-to-winrt-from-csharp.md#notifyuser)|
|設定屬性值|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|遞增屬性值|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>針對字串，請切換至產生器||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|Windows 執行階段字串的語言字串|不適用|`winrt::hstring{ s }`||
|字串建立|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[字串建立](#string-building)|
|字串插補|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 和 (或) [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[移植 **OnNavigatedTo** 方法](./clipboard-to-winrt-from-csharp.md#onnavigatedto)|
|用於比較的空字串|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[移植 **UpdateStatus** 方法](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|建立空白字串|`var myEmptyString = String.Empty;`|`winrt::hstring myEmptyString{ L"" };`||
|字典作業|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|類型轉換 (失敗時擲回)|`(MyType)v`|`v.as<MyType>()`|[移植 **Footer_Click** 方法](./clipboard-to-winrt-from-csharp.md#footer_click)|
|類型轉換 (失敗時為 null)|`v as MyType`|`v.try_as<MyType>()`|[移植 **PasteButton_Click** 方法](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|具有 x:Name 的 XAML 元素是屬性|`MyNamedElement`|`MyNamedElement()`|[移植建構函式 **Current** 和 **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|切換至 UI 執行緒|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync**或 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[移植 **NotifyUser** 方法](./clipboard-to-winrt-from-csharp.md#notifyuser)，以及[移植 **HistoryAndRoaming** 方法](./clipboard-to-winrt-from-csharp.md#historyandroaming)|
|XAML 頁面中命令式程式碼的 UI 元素結構|請參閱 [UI 元素結構](#ui-element-construction)|請參閱 [UI 元素結構](#ui-element-construction)||

下列各節將詳細說明資料表中的某些項目。

### <a name="ui-element-construction"></a>UI 元素建構

這些程式碼範例示範 XAML 頁面的命令式程式碼中的 UI 元素結構。

```csharp
var myTextBlock = new TextBlock()
{
    Text = "Text",
    Style = (Windows.UI.Xaml.Style)this.Resources["MyTextBlockStyle"]
};
```

```cppwinrt
TextBlock myTextBlock;
myTextBlock.Text(L"Text");
myTextBlock.Style(
    winrt::unbox_value<Windows::UI::Xaml::Style>(
        Resources().Lookup(
            winrt::box_value(L"MyTextBlockStyle")
        )
    )
);
```

### <a name="tostring"></a>ToString()

C# 類型會提供 [Object.ToString](/dotnet/api/system.object.tostring) 方法。

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT 不會直接提供此功能，但您可以轉向使用替代方案。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT 也針對有限的類型數量支援 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)。 您必須為想要字串化的任何其他類型新增多載。

| Language | 將 int 字串化 | 將列舉字串化 |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

在將列舉字串化的情況下，您需要提供 **winrt::to_hstring** 的實作。

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

資料繫結通常會隱含地使用這些字串化作業。

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

這些繫結會執行所繫結屬性的 **winrt::to_hstring**。 在第二個範例 (**StatusEnum**) 的情況下，您必須提供自己的 **winrt::to_hstring** 多載，否則會收到編譯器錯誤。

另請參閱[移植 **Footer_Click** 方法](./clipboard-to-winrt-from-csharp.md#footer_click)。

### <a name="string-building"></a>字串建立

針對字串建立，C# 有內建的 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 類型。

| 類別 | C# | C++/WinRT |
| -------- | -- | --------- |
| 字串建立 | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| 附加 Windows 執行階段字串，保留 null | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| 加入新行 |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| 存取結果 | `s = builder.ToString();` | `ws = builder.str();` |

另請參閱[移植 **BuildClipboardFormatsOutputString** 方法](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)，以及[移植 **DisplayChangedFormats** 方法](./clipboard-to-winrt-from-csharp.md#displaychangedformats)。

### <a name="running-code-on-the-main-ui-thread"></a>在主要 UI 執行緒上執行程式碼 

此範例取自[條碼掃描器範例](/samples/microsoft/windows-universal-samples/barcodescanner/)。

當您想要在 C# 專案的主要 UI 執行緒上執行作業時，通常會使用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 方法，如下所示。

```csharp
private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Do work on the main UI thread here.
    });
}
```

使用 C++/WinRT 來表達更為容易。 請注意，我們接受參數值是假設我們想要在第一個暫止點 (此案例中為 `co_await`) 之後存取它們。 如需詳細資訊，請參閱[參數傳遞](./concurrency.md#parameter-passing)。

```cppwinrt
winrt::fire_and_forget Watcher_Added(DeviceWatcher sender, winrt::DeviceInformation args)
{
    co_await Dispatcher();
    // Do work on the main UI thread here.
}
```

如果您需要以預設值以外的優先順序來執行工作，請參閱 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 函式，其有採用 priority 的多載。 如需示範如何等候對 **winrt::resume_foreground** 呼叫的程式碼範例，請參閱[考量使用執行緒親和性程式設計](./concurrency-2.md#programming-with-thread-affinity-in-mind)。

## <a name="porting-related-tasks-that-are-specific-to-cwinrt"></a>C++/WinRT 特定的移植相關工作

### <a name="define-your-runtime-classes-in-idl"></a>定義 IDL 中的執行階段類別

請參閱 [**MainPage** 類型的 IDL](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type) 及[合併 `.idl` 檔案](./clipboard-to-winrt-from-csharp.md#consolidate-your-idl-files)。

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>包含您需要的 C++/WinRT Windows 命名空間標頭檔案

在 C++/WinRT 中，每當您要使用 Windows 命名空間的類型時，您必須包含對應的 C++/WinRT Windows 命名空間標頭檔案。 如需範例，請參閱[移植 **NotifyUser** 方法](./clipboard-to-winrt-from-csharp.md#notifyuser)。

### <a name="boxing-and-unboxing"></a>Box 處理和 Unbox 處理

C# 會自動將純量 Box 處理為物件。 C++/WinRT 會要求您明確地呼叫 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函式。 這兩種語言都需要您明確地進行 Unbox 處理。 請參閱[使用 C++/WinRT 進行 Box 處理和 Unbox 處理](./boxing.md)。

在後續表格中，我們將使用下列定義。

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 操作 | C# | C++/WinRT|
|-|-|-|
| Box 處理 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unbox 處理 | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

如果您嘗試將 null 指標 Unbox 處理為某個實值類型，則 C++/CX 和 C# 會引發例外狀況。 C++/WinRT 會將此視為程式設計錯誤，並且毀損。 在 C++/WinRT 中，如果您想處理物件不是您所認為類型的情況，請使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式。

| 案例 | C# | C++/WinRT|
|-|-|-|
| 進行已知整數的 Unbox 處理 |`i = (int)o;` | `i = unbox_value<int>(o);` |
| 如果 o 為 null | `System.NullReferenceException` | 毀損 |
| 如果 o 不是已 Box 處理的 int | `System.InvalidCastException` | 毀損 |
| 進行 int 的 Unbox 處理，若為 null 則使用遞補；若為其他任何項目則會毀損 | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 可能的話，進行 int 的 Unbox 處理；其他任何項目使用遞補 | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

如需範例，請參閱[移植 **OnNavigatedTo** 方法](./clipboard-to-winrt-from-csharp.md#onnavigatedto)，以及[移植 **Footer_Click** 方法](./clipboard-to-winrt-from-csharp.md#footer_click)。

#### <a name="boxing-and-unboxing-a-string"></a>進行字串的 Box 處理和 Unbox 處理

字串在某些方面是實值類型，而在其他方面則是參考類型。 C# 和 C++/WinRT 會以不同的方式處理字串。

ABI 類型 [**HSTRING**](/windows/win32/winrt/hstring) 是參考計數字串的指標。 但是它並非衍生自 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，因此在技術上並不是「物件」。 此外, null **HSTRING** 代表空字串。 將非衍生自 **IInspectable** 的項目包裝在 [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_) 內，即可完成 Box 處理，而 Windows 執行階段會以 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 物件形式提供標準實作 (自訂類型會回報為 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype))。

C# 表示作為參考類型的 Windows 執行階段字串；而 C++/WinRT 會將字串投影為實值類型。 這表示已進行 Box 處理的 null 字串可以有不同的表示法 (取決於您達成的方式)。

| 行為 | C# | C++/WinRT|
|-|-|-|
| 宣告 | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| 字串類型類別 | 參考類型 | 值類型 |
| null  **HSTRING** 投影為 | `""` | `hstring{}` |
| Null 和 `""` 相同嗎？ | 否 | 是 |
| Null 的有效性 | `s = null;`<br>`s.Length` 引發 NullReferenceException | `s = hstring{};`<br>`s.size() == 0` (有效) |
| 如果將 Null 字串指派給物件 | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 如果將 `""` 指派給物件 | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本 Box 處理和 Unbox 處理。

| 操作 | C# | C++/WinRT|
|-|-|-|
| 進行字串的 Box 處理 | `o = s;`<br>空字串會變成非 Null 物件。 | `o = box_value(s);`<br>空字串會變成非 Null 物件。 |
| 進行已知字串的 Unbox 處理 | `s = (string)o;`<br>Null 物件會變成 Null 字串。<br>InvalidCastException (如果不是字串)。 | `s = unbox_value<hstring>(o);`<br>Null 物件損毀。<br>如果不是字串，則會損毀。 |
| 將可能的字串進行 Unbox 處理 | `s = o as string;`<br>Null 物件或非字串會變成 Null 字串。<br><br>或者<br><br>`s = o as string ?? fallback;`<br>Null 或非字串會變成遞補。<br>保留空字串。 | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 或非字串會變成遞補。<br>保留空字串。 |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>讓類別可供 {Binding} 標記延伸使用

如果您想要使用 {binding} 標記延伸將資料繫結至您的資料類型，請參閱[使用 {Binding} 宣告的繫結物件](../data-binding/data-binding-in-depth.md#binding-object-declared-using-binding)。

### <a name="consuming-objects-from-xaml-markup"></a>取用 XAML 標記中的物件

在 C# 專案中，您可以使用來自 XAML 標記的私有成員和具名元素。 但是在 C++/WinRT 中，使用 XAML [ **{x:Bind} 標記延伸**](../xaml-platform/x-bind-markup-extension.md) 取用的所有實體都必須公開於 IDL 中。

此外，布林值的繫結會在 C# 中顯示 `true` 或 `false`，但是在 C++/WinRT 中顯示 **Windows.Foundation.IReference`1\<Boolean\>** 。

如需詳細資訊和程式碼範例，請參閱[使用標記中的物件](./binding-property.md#consuming-objects-from-xaml-markup)。

### <a name="making-a-data-source-available-to-xaml-markup"></a>讓資料來源可供 XAML 標記使用

在 C++/WinRT 2.0.190530.8 版和更新版本中，[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 會建立可觀察的向量，其同時支援 **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** 和 **IObservableVector\<IInspectable\>** 。 如需範例，請參閱[移植 **Scenarios** 屬性](./clipboard-to-winrt-from-csharp.md#scenarios)。

您可以撰寫如下所示的 **Midl 檔案 (.idl)** (另請參閱[將執行階段類別分解成 Midl 檔檔案 (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl))。

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

其實作方式如下所示。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

如需詳細資訊，請參閱 [XAML 項目控制項；繫結至 C++/WinRT 集合](./binding-collection.md)與[使用 C++/WinRT 的集合](./collections.md)。

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>讓資料來源可供 XAML 標記使用 (在 C++/WinRT 2.0.190530.8 之前)

XAML 資料繫結要求項目來源實作 **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** ，以及下列其中一個介面組合。

- **IObservableVector\<IInspectable\>**
- **IBindableVector** 和 **INotifyCollectionChanged**
- **IBindableVector** 和 **IBindableObservableVector**
- **IBindableVector** 本身 (不會回應變更)
- **IVector\<IInspectable\>**
- **IBindableIterable** (會逐一查看元素並儲存至私用集合)

在執行階段無法偵測 **IVector\<T\>** 等一般介面。 每個 **IVector\<T\>** 都有不同的介面識別碼 (IID)，這是 **T**的函式。任何開發人員都可以任意擴充 **T** 集合，所以顯然 XAML 繫結程式碼永遠不會知道要查詢的完整集合。 該限制不是 C# 的問題，因為每個實作 **IEnumerable\<T\>** 的 CLR 物件都會實作 **IEnumerable**。 在 ABI 層級，這表示每個實作 **IObservableVector\<T\>** 的物件都會自動實作 **IObservableVector\<IInspectable\>** 。

C++/WinRT 不提供該保證。 如果 C++/WinRT 執行階段類別會實作 **IObservableVector\<T\>** ，我們無法假設也會提供 **IObservableVector\<IInspectable\>** 的實作。

因此，前一個範例需要如下所示。

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

其實作方式。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

如果您需要存取 *m_bookSkus* 中的物件，則必須將其 QI 回到 **Bookstore::BookSku**。

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>衍生類別

為了從執行階段類別衍生，基底類別必須「可組合」。 C# 不要求您採取任何特殊步驟，即可讓類別變為可組合，而 C++/WinRT 則會要求您採取步驟。 您可使用[未密封的關鍵字](/uwp/midl-3/intro#base-classes)，指出您希望類別可作為基底類別使用。

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

在[實作類型](./author-apis.md)的標頭檔案中，您必須先包含基底類別標頭檔，才可包含衍生類別的自動產生標頭。 否則，您會收到錯誤，例如「此類型當作運算式使用並不合法」。

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>重要 API
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C# 教學課程](/visualstudio/get-started/csharp)
* [C++/WinRT](./index.md)
* [深入了解資料繫結](../data-binding/data-binding-in-depth.md)