---
description: 本主題深入探討 C++/WinRT 2.0 功能，該功能可協助您診斷在堆疊上建立實作類型物件的錯誤，而不是使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 系列的協助程式。
title: 診斷直接配置
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 直接, 堆疊, 配置, 已投影, 實作
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "68372789"
---
# <a name="diagnosing-direct-allocations"></a>診斷直接配置

如[使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis) 所述，當您建立執行類型的物件時，您應該使用協助程式的 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 系列來執行這項操作。 本主題深入探討 C++/WinRT 2.0 功能，該功能可協助您診斷堆疊上實作類型的直接配置物件錯誤。

這類錯誤可能會變成不可思議的損毀，而這種情況難以偵錯且相當耗時。 這是一項重要功能，值得您瞭解背景。

## <a name="setting-the-scene-with-mystringable"></a>使用 **MyStringable** 設定場景

首先，讓我們考量簡單的 [**IStringable**](/uwp/api/windows.foundation.istringable) 實作。

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

現在假設您需要呼叫預計以 **IStringable** 作為引數的函式 (從您的實作)。

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

問題在於 **MyStringable** 類型「不是」  **IStringable**。

- **MyStringable** 類行為 **IStringable** 介面的實作。
- **IStringable**類型為投影的類型。

> [!IMPORTANT]
> 請務必瞭解「實作類型」  與「投影類型」  之間的差別。 如需基本概念和詞彙，請務必閱讀[使用 C++/WinRT 取用 API](consume-apis.md) 和[使用 C++/WinRT 撰寫 API](author-apis.md)。

實作與投影之間的空間可能難以理解。 事實上，若要嘗試讓實作更加類似於投影，此實作會為其實作的每個投影類型提供隱含的轉換。 這並不表示我們可以直接這麼做。

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

而我們需要取得參考，才能將轉換運算子當作解析呼叫的候選項目。

```cppwinrt
void Call()
{
    Print(*this);
}
```

這是可行的。 隱含轉換提供從實作類型到投影類型的 (非常有效) 轉換，這對許多案例而言非常方便。 如果沒有該設備，許多實作類型會顯示難以撰寫。 假設您只使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 函式範本 (或 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)) 來配置實作，則一切都很好。

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>C++/WinRT 1.0 的可能陷阱

隱含轉換仍可能讓您陷入困境。 請考慮使用這個無用的協助程式函式。

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

或甚至只是明顯無害的陳述式。

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

可惜的是，諸如此類的程式碼因為該隱含轉換，而使用了 C++/WinRT 1.0 進行編譯。  (非常嚴重的) 問題是我們可能會傳回投影類型，其指向其支援記憶體位於暫時堆疊上的參考計數物件。

以下是以 C++/WinRT 1.0 編譯的其他內容。

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

原始指標是危險且耗費人力的錯誤 (Bug) 來源。 如果您不需要，請勿使用。 C++/WinRT 設法讓讓一切變得有效率，但不曾強迫您使用原始指標。 以下是以 C++/WinRT 1.0 編譯的其他內容。

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

這在數個層級上都是錯誤的。 相同物件有兩個不同的參考計數。 Windows 執行階段 (之前為傳統 COM) 是以與 **std::shared_ptr** 不相容的內建參考計數為基礎。 **std::shared_ptr** 當然也有許多有效的應用程式，但是當您共用 Windows 執行階段 (和傳統 COM) 物件時，完全不需要這麼做。 最後，也會使用 C++/WinRT 1.0 進行編譯。

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

這也是個值得懷疑的問題。 唯一擁有權相對於 **MyStringable** 內建參考計數的共用存留期。

## <a name="the-solution-with-cwinrt-20"></a>採用 C++/WinRT 2.0 的解決方案

使用 C++/WinRT 2.0 時，直接配置實類型的所有嘗試都會導致編譯器錯誤。 這是最理想的錯誤類型，而且比神秘的執行時間錯誤好多了。

當您需要進行實作時，您可以直接使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 或 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)，如上所示。 現在，如果您忘了這麼做，您就會看到編譯器錯誤，其以名為 **use_make_function_to_create_this_object**的抽象函式參考暗示這點。 這不完全是 `static_assert`；但很接近。 這仍然是偵測所有所述錯誤的最可靠方法。

這表示我們需要對實作加諸一些次要條件約束。 假設我們不依賴覆寫來偵測直接配置，則 **winrt::make**函式範本必須透過覆寫來滿足抽象虛擬函數。 其做法是使用可提供覆寫的 `final` 類別，從實作衍生。 此程序有幾件需要觀察的事項。

首先，虛擬函式只會出現在偵錯組建中。 這表示偵測不會影響最佳化組建中的 vtable 大小。

第二，由於 **winrt::make** 使用的衍生類別為 `final`，這表示即使您先前選擇不要將實作類別標示為 `final`，也會發生最佳化工具可能推斷的去虛擬化。 這就算是一項改善。 相反的是，您的實作「不能」  是 `final`。 同樣地，因為具現化類型一律為 `final`，所以不會有任何影響。

第三，沒有什麼能阻止您將實作中的任何虛擬函式標示為 `final`。 當然，C++/WinRT 與傳統 COM 和 WRL 之類的部署非常不同，其中有關實作的一切有可能都是虛擬的。 在 C++/WinRT 中，虛擬分派受限於應用程式二進位介面 (ABI) (一律為 `final`)，而您的實方法會依賴編譯階段或靜態多型。 這可避免不必要的執行階段多型，同時也表示 C++/WinRT 實作中的虛擬函式沒什麼理由。 這是一個非常好用的功能，可致使內嵌更可預測。

第四，由於 **winrt::make** 會插入衍生類別，所以您的執行不能有私人解構函式。 私人解構函數很常用於傳統 COM 實作，同樣因為一切都是虛擬的，而且通常會直接處理原始指標，因此很容易意外呼叫 `delete` 而非 [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release)。 C++/WinRT 設法讓您難以直接處理原始指標。 而且您必須「確實」  設法在 C++/WinRT 中取得您可能在其上呼叫 `delete` 的原始指標。 值語義表示您正在處理值和參考；很少使用指標。

因此，C++/WinRT 會挑戰我們對於撰寫傳統 COM 程式碼所代表意義的先入為主概念。 這非常合理，因為 WinRT 並不是傳統 COM。 傳統 COM 是 Windows 執行階段的元件語言。 這不該是您每天撰寫的程式碼。 然而，C++/WinRT 可讓您撰寫更像現代 C++ 但不像傳統 COM 的程式碼。

## <a name="important-apis"></a>重要 API
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 函式範本](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis)