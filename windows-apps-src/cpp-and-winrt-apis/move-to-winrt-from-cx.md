---
description: 本主題說明將 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 專案中的原始程式碼移植到其在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的對等項目時所涉及的技術詳細資料。
title: 從 C++/CX 移到 C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 移轉, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c5f8b9548bba704a7035b014ca3728db8bcbcc16
ms.sourcegitcommit: 7dcf74b11aa0cb2f3ff4ab10caf26ba769f96dfb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2020
ms.locfileid: "80662388"
---
# <a name="move-to-cwinrt-from-ccx"></a>從 C++/CX 移到 C++/WinRT

本主題說明將 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 專案中的原始程式碼移植到其在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的對等項目時所涉及的技術詳細資料。

## <a name="porting-strategies"></a>移植策略

如果您想要將 C++/CX 程式碼逐漸移植到 C++/WinRT，您可以使用此策略。 C++/CX 和 C++/WinRT 程式碼可以共存在相同的專案中，但 XAML 編譯器支援與 Windows 執行階段元件除外。 對於這些兩個例外狀況，您必須將相同專案中的目標設為 C++/CX 或 C++/WinRT。

> [!IMPORTANT]
> 如果您的專案組建了 XAML 應用程式，那麼其中一種建議的工作流程是，先使用一個 C++/WinRT 專案範本，在 Visual Studio 中建立新項目 (請參閱[適用於 C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 然後，開始從 C++/CX 專案複製原始程式碼和標記。 您可以使用 [專案]  \> [新增項目...]  來新增 XAML 頁面\>**Visual C++**  > **空白頁 (C++/WinRT)** 。
>
> 或者，您可以使用 Windows 執行階段元件，在移植 XAML C++/CX 專案時，將程式碼從中分解出來。 請盡可能將 C++/CX 程式碼移動到元件中，再將 XAML 專案變更為 C++/WinRT。 或是將 XAML 專案保留為 C++/CX，而建立新的 C++/WinRT 元件，並開始將 C++/CX 程式碼從 XAML 專案移植到元件中。 您也可以在同個解決方案中，一起使用 C++/CX 元件專案與 C++/WinRT 元件專案，從應用程式專案中參照這兩個元件專案，並逐漸從一個專案移植到另一個專案。 如需在相同專案中使用兩種語言投影的更多詳細資料，請參閱 [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 以及根命名空間 **Windows** 的 Windows SDK 宣告類型。 投影到 C++/WinRT 的 Windows 類型有與 Windows 類型相同的完整名稱，但它放在 C++ **winrt** 命名空間。 這些不同的命名空間，可讓您以自己的速度從 C++/CX 移植至 C++/WinRT。

請記住上述例外狀況，將 C++/CX 專案移植到 C++/WinRT 的第一個步驟是手動新增 C++/WinRT 支援 (請參閱[適用於 C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 若要這麼做，請將 [Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)安裝到您的專案中。 在 Visual Studio 中開啟專案，按一下 [專案]  \>[管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝]  以安裝適用於該專案的套件。 該變更的其中一個效果，是會關閉專案中支援的 C++/CX。 最好關閉支援，這樣組建訊息有助於找出 (並移植) C++/CX 上所有的相依性，或您可以重新開啟支援 (在專案屬性中，**C/C++** \>**一般**\>**使用 Windows 執行階段擴充功能**\>**是 (/ZW)** )，請逐漸移植。

或者，在 Visual Studio 中使用 C++/WinRT 專案屬性頁面，將下列屬性新增至 `.vcxproj` 檔案檔案。 如需類似自訂選項 (可微調 `cppwinrt.exe` 工具的行為) 清單，請參閱 Microsoft.Windows.CppWinRT NuGet 套件[讀我檔案](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

接下來，務必將專案屬性 [一般]  \>[目標平台版本]  設為 10.0.17134.0 (Windows 10，版本 1803) 或更高版本。

在您先行編譯的標頭檔案 (通常是 `pch.h`) 中，加入 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果加入了任何 C++/WinRT 投影的 Windows API 標頭 (例如 `winrt/Windows.Foundation.h`)，則不需要像這樣明確地加入 `winrt/base.h`，因為會自動包含在內。

如果您的專案也會使用 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 類型，請參閱[從 WRL 移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="file-naming-conventions"></a>檔案命名慣例

### <a name="xaml-markup-files"></a>XAML 標記檔案

| | C++/CX | C++/WinRT |
| - | - | - |
| **開發人員 XAML 檔案** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl (請參閱下方) |
| **產生的 XAML 檔案** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

請注意，C++/WinRT 會從 `*.h` 和 `*.cpp` 檔案名稱中移除 `.xaml`。

C++/WinRT 會新增其他開發人員檔案，即 **Midl 檔案 (.idl)** 。 C++/CX 會在內部自動產生此檔案，並在其中新增每個公開和受保護的成員。 在 C++/WinRT 中，您會自行新增和撰寫檔案。 如需詳細資料、程式碼範例，以及撰寫 IDL 的逐步說明，請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

另請參閱[將執行階段類別分解成 Midl 檔案 (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)

### <a name="runtime-classes"></a>執行階段類別

C++/CX 不會在標頭檔的名稱上強加限制；通常會將多個執行階段類別定義放入單一標頭檔中 (尤其適用於小型類別)。 但是 C++/WinRT 會要求每個執行階段類別有自己的標頭檔，並以類別名稱命名。 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

在 C++/CX 中，對 XAML 自訂控制項使用不同命名的標頭檔較不常見 (但仍屬合法)。 您必須將這些標頭檔重新命名，以符合類別名稱。

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>標頭檔需求

C++/Cx 不會要求您包含任何特殊的標頭檔，因為它會從 `.winmd` 檔案在內部自動產生標頭檔。 在 C++/CX 中，通常會針對依名稱取用的命名空間使用 `using` 指示詞。

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

`using namespace Windows::Media::Playback` 指示詞可讓我們在沒有命名空間前置詞的情況下撰寫 `MediaPlaybackItem`。 我們也觸及了 `Windows.Media.Core` 命名空間，因為 `item->VideoTracks->GetAt(0)` 會傳回 **Windows.Media.Core.VideoTrack**。 但我們不必在任何地方輸入名稱 **VideoTrack**，因此不需要 `using Windows.Media.Core` 指示詞。

但是 C++/WinRT 會要求您包含對應至您用的每個命名空間的標頭檔，即使您未替該檔案命名亦然。

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

另一方面，即使 **MediaPlaybackItem.AudioTracksChanged** 事件的類型為 **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** ，我們也不需要包含 `winrt/Windows.Foundation.Collections.h`，因為我們不會使用該事件。

C++/WinRT 也會要求您包含 XAML 標記所用命名空間的標頭檔。

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

使用 **Rectangle** 類別表示您必須新增此 include。

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

如果您忘記標頭檔，則會順利編譯所有項目，但您會因為遺漏 `consume_` 類別而收到連結器錯誤。

## <a name="parameter-passing"></a>參數傳遞

撰寫 C++/CX 原始程式碼時，會傳遞 C++/CX 類型做為控制帽 (\^) 參考的函式參數。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

在 C++/WinRT 中，針對同步函式，依預設應該使用 `const&` 參數。 如此可避免複本以及連鎖額外負荷。 但您的協同程式應該使用透過值傳遞，以確保他們透過值擷取，並避免存留期問題 (如需詳細資訊，請參閱[使用 C++/WinRT 進行並行和非同步作業](concurrency.md))。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 物件基本上是一個值，用於保留介面指標到支援 Windows 執行階段物件。 當您複製 C++/WinRT 物件時，編譯器會複製封裝的介面指標，遞增其參考次數。 最終複本的破壞會導致參考次數遞減。 因此，只有在必要時才會產生複本的額外負荷。

## <a name="variable-and-field-references"></a>變數和欄位參考資料

撰寫 C++/CX 原始程式碼時，使用控制帽 (\^) 變數參考 Windows 執行階段物件，以及箭頭 (-&gt;) 運算子以取值控制帽變數。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

藉由移除其控制帽，並將箭頭運算子 (-&gt;) 變更為點運算子 (.)，對於移植到對等項目 C++/WinRT 程式碼有很大的幫助。 C++/WinRT 投影類型為值，而非指標。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

C++/CX hat (^) 參考的預設建構函式會將其初始化為 null。 以下是 C++/CX 程式碼範例，我們在其中建立了一個正確類型的變數/欄位，但尚未初始化。 換句話說，其最初並非是參考到 **TextBlock**；我們打算稍後再指派一個參考。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

如需了解 C++/WinRT 中的對等項目，請參閱[延遲初始化](consume-apis.md#delayed-initialization)。

## <a name="properties"></a>[內容]

C++/CX 語言擴充功能包括了屬性的概念。 當撰寫 C++/CX 原始程式碼時，可以如欄位一般存取其屬性。 標準 C++ 不具屬性的概念，因此在 C++/WinRT 中，您要呼叫 get 和 set 函式。

在接下來的範例中，**XboxUserId**、**UserState**、**PresenceDeviceRecords**，和 **Size** 都是屬性。

### <a name="retrieving-a-value-from-a-property"></a>從屬性擷取一個值

以下是如何使用 C++/CX 取得屬性值的方法。

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

對等項目 C++/WinRT 原始程式碼呼叫與屬性具有相同名稱，但不含任何參數的函式。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

請注意，**PresenceDeviceRecords** 函式傳回 Windows 執行階段物件，其本身有一個 **Size** 函式。 由於傳回的物件也是 C++/WinRT 投影類型，因此我們使用點運算子取值以呼叫 **Size**。

### <a name="setting-a-property-to-a-new-value"></a>將屬性設定為新值

將屬性設定為新值，按照類似的模式。 首先要看 C++/CX。

```cppcx
record->UserState = newValue;
```

若要執行 C++/WinRT 中的對等項目，請您呼叫具有與屬性相同名稱的函式，並傳遞引數。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>建立類別的執行個體

透過控制代碼使用 C++/CX 物件，通常稱它為控制帽 (\^) 參考。 透過 `ref new` 關鍵字建立新的物件，依序呼叫 [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 以啟動新的執行階段類別執行個體。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 物件是值；讓您可以在堆疊上配置它，或做為物件的欄位。 您「從不」  使用 `ref new` (或 `new`) 配置 C++/WinRT 物件。 幕後仍持續呼叫 **RoActivateInstance**。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

如果將資源初始化的成本相當高，則常會延遲初始化，直到有實際需求時再進行。 如先前所述，C++/CX hat (^) 參考的預設建構函式會將其初始化為 null。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

已將相同的程式碼移植至 C++/WinRT。 請注意 **std:: nullptr_t** 建構函式的實作。 如需該建構函式的詳細資訊，請參閱[延遲初始化](consume-apis.md#delayed-initialization)。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="how-the-default-constructor-affects-collections"></a>預設建構函式對於集合的影響

C++ 集合類型會使用預設建構函式，這可能導致非預期的物件結構。

| 案例 | C++/CX | C++/WinRT (不正確) | C++/WinRT (正確) |
| - | - | - | - |
| 本機變數，最初是空的 | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| 成員變數，最初是空的 | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| 全域變數，最初是空的 | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| 空白參考的向量 | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| 在對應中設定值 | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| 空白參考的陣列 | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| 配對 | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

### <a name="more-about-collections-of-empty-references"></a>深入了解空白參考的集合

每當您的 C++/CX 中有 **Platform::Array\^** 時 (請參閱[移植 **Platform::Array\^** ](#port-platformarray))，您可以選擇將其移植到 C++/WinRT 中的 **std::vector** (事實上，任何連續的容器皆可)，而不將其保留為陣列。 選擇 **std::vector** 有某些優點。

例如，雖然在建立空白參考固定大小的向量時有速記 (請參閱上表)，但在建立空白參考的*陣列*時則沒有這類速記。 您必須針對陣列中的每個元素重複 `nullptr`。 如果您的元素過少，則會有額外的預設建構元素。

針對向量，您可以在初始化時為其填入空白參考 (如上表所示)，或者，您可以在初始化後，使用如下列的程式碼填入空白參考。

```cppwinrt
std::vector<TextBox> boxes(10); // 10 default-constructed TextBoxes.
boxes.resize(10, nullptr); // 10 empty references.
```

### <a name="more-about-the-stdmap-example"></a>關於 **std::map** 範例的詳細資訊

**std::map** 的 `[]` 下標運算子運作方式如下。

- 如果在對應中找到索引鍵，則會傳回現有值的參考 (您可加以覆寫)。
- 如果在對應中找不到索引鍵，則在由索引鍵 (如可移動，則已移動) 和「預設建構值」  組成的對應中建立新項目，並傳回此值的參考 (您可接著覆寫)。

換句話說，`[]` 運算子一律會在對應中建立專案。 這不同於 C#、Java 和 JavaScript。

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>從基礎執行階段類別轉換至衍生的執行階段類別

讓您所知的 reference-to-base 參考到衍生類型的物件，算是相當常見。 在 C++/CX 中，要使用 `dynamic_cast` 將 reference-to-base *轉換*為 reference-to-derived。 `dynamic_cast` 實際上只是對 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 的隱藏呼叫。 這是一個典型的範例&mdash;您在處理相依性屬性變更事件，而且希望從 **DependencyObject** 轉換回擁有相依性屬性的實際類型。

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

對等項目 C++/WinRT 程式碼藉由呼叫 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 函式，取代 `dynamic_cast`，該函式會封裝 **QueryInterface**。 您也可以選擇呼叫 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，如果未傳回查詢所需的介面 (要求類型的預設介面)，則會擲回例外狀況。 這是 C++/WinRT 程式碼範例。

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="derived-classes"></a>衍生類別

為了從執行階段類別衍生，基底類別必須「可組合」  。 C++/CX 不要求您採取任何特殊步驟，即可讓類別變為可組合，而 C++/WinRT 則會要求您採取步驟。 您可使用[未密封的關鍵字](/uwp/midl-3/intro#base-classes)，指出您希望類別可作為基底類別使用。

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

在實作標頭類別中，您必須先包含基底類別標頭檔，才可包含衍生類別的自動產生標頭。 否則，您會收到錯誤，例如「此類型當作運算式使用並不合法」。

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

## <a name="event-handling-with-a-delegate"></a>使用委派的事件處理

以下是在 C++/CX 中處理事件的一般範例，這種情形下，是使用 lambda 函式做為委派。

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

您可以選擇實作委派做為可用功能，或做為指標成員函式，而不是 lambda 函式。 如需詳細資訊，請參閱[透過使用 C++/WinRT 中的委派來處理事件](handle-events.md)。

如果您正從在內部使用事件和委派的 C++/CX 程式碼基底進行移植 (並非所有二進位檔案)，則 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 可協助您在 C++/WinRT 中複寫該模式。 另請參閱[參數化委派、簡單的訊號，以及專案中的回呼](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)。

## <a name="revoking-a-delegate"></a>撤銷委派

您在 C++/CX 中使用 `-=` 運算子以撤銷前一個事件註冊。

```cppcx
myButton->Click -= token;
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
myButton().Click(token);
```

如需詳細資訊以及選項，請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

## <a name="boxing-and-unboxing"></a>Box 處理和 Unbox 處理

C++/CX 會自動將純量 Box 處理為物件。 C++/WinRT 會要求您明確地呼叫 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函式。 這兩種語言都需要您明確地進行 Unbox 處理。 請參閱[使用 C++/WinRT 進行 Box 處理和 Unbox 處理](/windows/uwp/cpp-and-winrt-apis/boxing)。

在後續表格中，我們將使用下列定義。

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| 操作 | C++/CX | C++/WinRT|
|-|-|-|-|
| Box 處理 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unbox 處理 | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

如果您嘗試將 null 指標 Unbox 處理為某個實值類型，則 C++/CX 和 C# 會引發例外狀況。 C++/WinRT 會將此視為程式設計錯誤，並且毀損。 在 C++/WinRT 中，如果您想處理物件不是您所認為類型的情況，請使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式。

| 案例 | C++/CX | C++/WinRT|
|-|-|-|-|
| 進行已知整數的 Unbox 處理 | `i = (int)o;` | `i = unbox_value<int>(o);` |
| 如果 o 為 null | `Platform::NullReferenceException` | 毀損 |
| 如果 o 不是已 Box 處理的 int | `Platform::InvalidCastException` | 毀損 |
| 進行 int 的 Unbox 處理，若為 null 則使用遞補；若為其他任何項目則會毀損 | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 可能的話，進行 int 的 Unbox 處理；其他任何項目使用遞補 | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>進行字串的 Box 處理和 Unbox 處理

字串在某些方面是實值類型，而在其他方面則是參考類型。 C++/CX 和 C++/WinRT 會以不同的方式處理字串。

ABI 類型 [**HSTRING**](/windows/win32/winrt/hstring) 是參考計數字串的指標。 但是它並非衍生自 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，因此在技術上並不是「物件」  。 此外, null **HSTRING** 代表空字串。 將非衍生自 **IInspectable** 的項目包裝在 [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_)內，即可完成 Box 處理，而 Windows 執行階段會以 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 物件形式提供標準實作 (自訂類型會回報為 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype))。

C++/CX 表示作為參考類型的 Windows 執行階段字串；而 C++/WinRT 會將字串投影為實值類型。 這表示已進行 Box 處理的 null 字串可以有不同的表示法 (取決於您達成的方式)。

此外，C++/CX 允許您對 null **String^** 進行取值，在此情況下其行為就像字串 `""`。

| 行為 | C++/CX | C++/WinRT|
|-|-|-|
| 宣告 | `Object^ o;`<br>`String^ s;` | `IInspectable o;`<br>`hstring s;` |
| 字串類型類別 | 參考類型 | 值類型 |
| null  **HSTRING** 投影為 | `(String^)nullptr` | `hstring{}` |
| Null 和 `""` 相同嗎？ | 是 | 是 |
| Null 的有效性 | `s = nullptr;`<br>`s->Length == 0` (有效) | `s = hstring{};`<br>`s.size() == 0` (有效) |
| 如果將 Null 字串指派給物件 | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 如果將 `""` 指派給物件 | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本 Box 處理和 Unbox 處理。

| 操作 | C++/CX | C++/WinRT|
|-|-|-|
| 進行字串的 Box 處理 | `o = s;`<br>空字串會變成 nullptr。 | `o = box_value(s);`<br>空字串會變成非 Null 物件。 |
| 進行已知字串的 Unbox 處理 | `s = (String^)o;`<br>Null 物件會變成空字串。<br>InvalidCastException (如果不是字串)。 | `s = unbox_value<hstring>(o);`<br>Null 物件損毀。<br>如果不是字串，則會損毀。 |
| 將可能的字串進行 Unbox 處理 | `s = dynamic_cast<String^>(o);`<br>Null 物件或非字串會變成空字串。 | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 或非字串會變成遞補。<br>保留空字串。 |

## <a name="concurrency-and-asynchronous-operations"></a>並行和非同步作業

平行模式程式庫 (PPL) (例如 [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)) 已更新為支援 C++/CX hat (^) 參考。

對於 C++/WinRT，您應該改用協同程式和 `co_await` 。 如需詳細資訊和程式碼範例，請參閱[使用 C++/WinRT 的並行和非同步作業](/windows/uwp/cpp-and-winrt-apis/concurrency)。

## <a name="consuming-objects-from-xaml-markup"></a>取用 XAML 標記中的物件

在 C++/CX 專案中，您可以使用來自 XAML 標記的私有成員和具名元素。 但是在 C++/WinRT 中，使用 XAML [ **{x:Bind} 標記延伸**](/windows/uwp/xaml-platform/x-bind-markup-extension) 取用的所有實體都必須公開於 IDL 中。

此外，布林值的繫結會在 C++/CX 中顯示 `true` 或 `false`，但是在 C++/WinRT 中顯示 **Windows.Foundation.IReference`1\<Boolean\>** 。

如需詳細資訊和程式碼範例，請參閱[使用標記中的物件](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup)。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>將 C++/CX **平台**類型對應至 C++/WinRT 類型

C++/CX 在**平台**命名空間中提供幾種資料類型。 這些類型不是標準 C++，因此您只能在啟用 Windows 執行階段語言擴充功能時使用它們 (Visual Studio 專案屬性 **C/C++**  > **一般** >  **使用 Windows 執行階段擴充功能** > **是 (/ZW)** )。 下列表格有助於您從**平台**類型移植至 C++/WinRT 中的對等項目。 完成後，由於 C++/WinRT 是標準的 C++，您可以關閉 `/ZW` 選項。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | 請參閱[移植 **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagile_ref"></a>將 **Platform::Agile\^** 移植到 **winrt::agile_ref**

C++/CX 中的 **Platform::Agile\^** 類型代表可從任何執行緒存取的 Windows 執行階段類別。 C++/WinRT 對等項目是 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)。

在 C++/CX 中。

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

在 C++/WinRT 中。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>移植 **Platform::Array\^**

在 C++/CX 要求您使用陣列的情況下，C++/WinRT 可讓您使用任何連續的容器。 若要了解 **std::vector** 為何是不錯的選擇，請參閱[預設建構函式對於集合的影響](#how-the-default-constructor-affects-collections)。

因此，每當 C++/CX 中有 **Platform::Array\^** 時，使用初始設定式清單 (**std::array** 或 **std::vector**) 都將是您的移植選項之一。 如需詳細資訊以及程式碼範例，請參閱[標準初始化清單](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists)和[標準陣列和向量](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)。

### <a name="port-platformexception-to-winrthresult_error"></a>將 **Platform::Exception\^** 移植到 **winrt::hresult_error**

Windows 執行階段 API 傳回非 S\_OK HRESULT 時，C++/CX 中產生 **Platform::Exception\^** 類型。 C++/WinRT 對等項目是 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

若要移植至 C++/WinRT，請將所有使用 **Platform::Exception\^** 的程式碼變更為使用 **winrt::hresult_error**。

在 C++/CX 中。

```cppcx
catch (Platform::Exception^ ex)
```

在 C++/WinRT 中。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT 提供這些例外類別。

| 例外狀況類型 | 基底類別 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | 呼叫 [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

請注意，每個類別 (透過 **hresult_error** 基底類別) 提供一個 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 函式，傳回錯誤的 HRESULT，以及一個[**訊息**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function)函式，傳回該 HRESULT 的字串表示方法。

以下是在 C++/CX 中擲回例外狀況的範例。

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的對等項目。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>將 **Platform::Object\^** 移植到 **winrt::Windows::Foundation::IInspectable**

就像所有的 C++/WinRT 類型，**winrt::Windows::Foundation::IInspectable** 是一種值類型。 以下是您如何將該類型的變數初始化為 null 的方法。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>將 **Platform::String\^** 移植到 **winrt::hstring**

**Platform::String\^** 等同於 Windows 執行階段 HSTRING ABI 類型。 對於 C++/WinRT，對等項目是 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，您可以使用 C++ 標準程式庫寬字串類型 (例如 **std::wstring**) 來呼叫 Windows 執行階段 API，及/或寬字串常值。 如需詳細資訊和程式碼範例，請參閱 [C++/WinRT 中的字串處理](strings.md)。

使用 C++/CX，您可以存取 [**Platform::String::Data**](https://docs.microsoft.com/cpp/cppcx/platform-string-class?view=vs-2019#data) 屬性來擷取字串做為 C-style **const wchar_t\*** 陣列 (例如，將它傳遞至 **std::wcout**)。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

若要使用 C++/WinRT 進行相同的動作，您可以使用 [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 函式，取得 null 終止的 C 式字串版本，就如同您可以從 **std::wstring** 取得一樣。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

實作採用或傳回字串的 API 時，您通常會變更任何使用 **Platform::String\^** 來使用 **winrt::hstring** 的 C++/CX 程式碼。

以下是採用字串的 C++/CX API 範例。

```cppcx
void LogWrapLine(Platform::String^ str);
```

對於 C++/WinRT，您可以在 [MIDL 3.0](/uwp/midl-3) 中宣告這類 API。

```idl
// LogType.idl
void LogWrapLine(String str);
```

然後，C++/WinRT 工具鏈將為您產生看起來像這樣的原始程式碼。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX 類型會提供 [Object::ToString](/cpp/cppcx/platform-object-class#tostring) 方法。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/WinRT 不會直接提供此功能，但您可以轉向使用替代方案。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT 也針對有限的類型數量支援 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)。 您必須為想要字串化的任何其他類型新增多載。

| Language | 將 int 字串化 | 將列舉字串化 |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
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

#### <a name="string-building"></a>字串建立

C++/CX 和 C++/WinR 會延遲為標準 **std::wstringstream** 以便建立字串。

| | C++/CX | C++/WinRT |
|-|-|-|
| 附加字串，保留 null | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| 附加字串，在第一個 null 停止 | `stream << s->Data();` | `stream << s.c_str();` |
| 擷取結果 | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>更多範例

在下列範例中，*ws* 是 **std::wstring**類型的變數。 此外，雖然 C++/CX 可以從 8 位元字串建構 **Platform::String**，但是 C++/WinRT 不會這麼做。

| 操作 | C++/CX | C++/WinRT |
| - | - | - |
| 從常值建構字串 | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| 從 **std:: wstring** 轉換，保留 null | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| 從 **std::wstring** 轉換，在第一個 null 停止 | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| 轉換為 **std:: wstring**，保留 null | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| 轉換為 **std::wstring**，在第一個 null 停止 | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| 將常值傳遞至方法 | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| 將 **std:: wstring** 傳遞至方法 | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

## <a name="important-apis"></a>重要 API
* [winrt::delegate struct template](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [以 C++/WinRT 撰寫事件](author-events.md)
* [透過 C++/WinRT 的並行和非同步作業](concurrency.md)
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)
* [Microsoft 介面定義語言 3.0 參考資料](/uwp/midl-3)
* 從 WRL 移到 [C++/WinRT](move-to-winrt-from-wrl.md)
* [C++/WinRT 中的字串處理](strings.md)
