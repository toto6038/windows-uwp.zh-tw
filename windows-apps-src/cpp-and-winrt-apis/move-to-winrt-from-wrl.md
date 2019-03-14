---
description: 本主題示範如何將 WRL 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 WRL 移到 C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 移轉, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e81f82fe823ee0fdf81741c89576adf268940d91
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630743"
---
# <a name="move-to-cwinrt-from-wrl"></a>從 WRL 移到 C++/WinRT
本主題說明如何移植[Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)程式碼，以在其對等[C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

第一個步驟中移植到 C + + /cli WinRT 是以手動方式加入 C + + /cli WinRT 支援加入至您的專案 (請參閱[Visual Studio 支援 C + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 若要這樣做，請安裝[Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)到您的專案。 開啟的專案在 Visual Studio 中，按一下**專案** \> **管理 NuGet 套件...**\> **瀏覽**中，輸入或貼上**Microsoft.Windows.CppWinRT**在 [搜尋] 方塊中，選取項目在搜尋結果中，然後按一下**安裝**安裝適用於該專案的套件。 這項變更的其中一個影響是，支援[C + + /CX](/cpp/cppcx/visual-c-language-reference-c-cx)專案中已關閉。 如果您在專案中使用 C++/CX，則您也可以讓支援保持關閉並更新 C++/CX 程式碼為 C++/WinRT (查看[從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md))。 您可以支援重新開啟或者 (在專案屬性中， **C/c + +** \> **一般** \> **取用的 Windows 執行階段延伸模組** \>**是 (/ZW)**)，並先專心移植您 WRL 的程式碼。 C + + /CX 和 C + + /cli WinRT 程式碼可以共存於相同專案中，除了 XAML 編譯器支援，以及 Windows 執行階段元件 (請參閱[移至 C + + WinRT 從 C + + /CX](move-to-winrt-from-cx.md))。

設定專案屬性**一般** \> **目標平台版本**到 10.0.17134.0 (Windows 10 1803年版) 或更新版本。

在您先行編譯的標頭檔案 (通常是 `pch.h`)，包含 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果包含任何 C++/WinRT 投影 Windows API 標頭 (例如，`winrt/Windows.Foundation.h`)，則您不需要像這樣明確包含 `winrt/base.h`，因為它會自動為您包含。

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>移植 WRL COM 智慧型指標 ([Microsoft: 110:: WRL::ComPtr](/cpp/windows/comptr-class))
使用任何程式碼移植**Microsoft::WRL::ComPtr\<T\>** 若要使用[ **winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr)。 以下是變更前後的程式碼範例。 在 *之後* 版本中，[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) 成員函式擷取基礎原始指標，如此一來便可以設定它。

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> 如果您有[ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr)的已插入擴充槽 （其內部的原始指標已經有一個目標） 和您想要重新基座，以指向不同的物件，則您必須先指派`nullptr`它&mdash;如下列程式碼範例所示。 如果沒有，則已經為插入擴充槽**com_ptr**將會繪製您注意的問題 (當您呼叫[ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)或是[ **com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) 藉由判斷提示其內部指標不 null。

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

在下一個範例中 (在 *之後* 版本)，[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) 成員函式擷取基礎原始指標做為無效指標的指標。

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

使用 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) 取代 **ComPtr::Get**。

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

當您想要將基礎的原始指標傳遞給需要指標的函式**IUnknown**，使用[ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function)免費函式，這個下一步 所示範例。

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>移植 WRL 模組 (Microsoft::WRL::Module)
您可以逐漸將 C++/WinRT 程式碼新增至使用 WRL 實作元件的現有投影，且會持續支援現有的 WRL 類別。 本章節示範方式。

如果您在 Visual Studio 中建立新的 **Windows 執行階段元件 (C++/WinRT)** 專案類型並組建，則為您產生檔案 `Generated Files\module.g.cpp`。 該檔案包含兩個的實用 C++/WinRT 函式（列於下方）的定義，您可以將其複製並新增到您的專案。 這些函式為 **WINRT_CanUnloadNow** 和 **WINRT_GetActivationFactory**，如您所見，他們有條件的呼叫 WRL 來支援您，不論您在移植的哪個階段。

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

一旦您在專案中取得這些函式，而不是直接呼叫 [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method)，請呼叫 **WINRT_GetActivationFactory**（其內部呼叫 WRL 函式）。 以下是變更前後的程式碼範例。

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

呼叫 **WINRT_CanUnloadNow**（ 其內部呼叫 WRL 函式），而非直接呼叫 [**Module::Terminate**](/cpp/windows/module-terminate-method)。 以下是變更前後的程式碼範例。

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>重要 API
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相關主題
* [簡介使用 C + + /cli WinRT](intro-to-using-cpp-with-winrt.md)
* [移至 C + + /cli WinRT 從 C + + /CX](move-to-winrt-from-cx.md)
* [Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
