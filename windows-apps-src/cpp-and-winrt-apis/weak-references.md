---
author: stevewhims
description: C++/WinRT 弱式參考支援是使用付費，其中您不需要支付任何項目除非針對 IWeakReferenceSource 查詢您的物件。
title: C++/WinRT 中的弱式參考資料
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、弱式、參考資料
ms.localizationpriority: medium
ms.openlocfilehash: 69294115af93ec464abfe908df948c8ff5504efc
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842452"
---
# <a name="weak-references-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的弱式參考資料
您應該可以不用這麼頻繁以這種方法來設計自己的 C++/WinRT API，避免循環參考和弱式參考的需求。 不過，談到 XAML 為基礎的 UI frameworkL 的原生實作&mdash;因為架構的歷史設計&mdash; C++/WinRT 中的弱式參考機制才能處理循環參考。 XAML 以外，您不太需要使用弱式參考 (雖然理論上，沒有任何 XAML 專用的弱式參考)。

針對您宣告的任何指定類型，當弱式參考是必要的，是否無法馬上看得出來是 C++/WinRT。 因此，C++/WinRT 自動在結構範本[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 上提供弱式參考支援，您的 C++/WinRT 類型直接或間接從其中衍生。 它是使用付費，其中您不需要支付任何項目除非確實針對 [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609) 查詢您的物件。 且您可以明確選擇[退出該支援](#opting-out-of-weak-reference-support)。

## <a name="code-examples"></a>程式碼範例
[**Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref)結構範本是取得類別執行個體的弱式參考選項之一。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```
或者，您可以使用 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 協助程式函式。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

建立弱式參考並不會影響物件本身的參考計數。只會導致配置控制區。 該控制區負責實作弱式參考語意。 然後您可以嘗試將弱式參考升級為強式參考，如果成功了，請使用它。

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

提供的一些其他強式參考仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) 呼叫遞增參考計數並將強式參考傳回給該呼叫者。

## <a name="a-weak-reference-to-the-this-pointer"></a>*this* 指標的弱式參考
C++/WinRT 物件直接或間接從結構範本 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 衍生。 [**Implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) 受保護的成員函式將弱式參考傳回給 C++/WinRT 物件的 *this* 指標。 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)取得一個強式參考。

## <a name="opting-out-of-weak-reference-support"></a>不使用弱式參考資料支援
弱式參考資料支援會自動執行。 但您可以藉由傳遞 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 標記結構做為基底類別的範本引數，明確地選擇退出物件支援。

如果您直接從 **winrt::implements** 衍生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

如果您撰寫一個執行階段類別。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

不論標記結構在 variadic 參數套件中出現的位置。 如果您要求弱式參考一個退出的類型，然後編譯器會協助您使用「*這僅適用於弱式參考支援*」。

## <a name="important-apis"></a>重要 API
* [implements::get_weak function](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::make_weak 函式範本](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 標記結構](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 結構範本](/uwp/cpp-ref-for-winrt/weak-ref)
