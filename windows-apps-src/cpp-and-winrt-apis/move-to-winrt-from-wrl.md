---
author: stevewhims
description: 本主題示範如何將 WRL 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 WRL 移到 C++/WinRT
ms.author: stwhi
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 移轉, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 3b9afd50b2c360c11b103f676b91d512881eaa71
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2018
ms.locfileid: "2890807"
---
# <a name="move-to-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-from-wrl"></a>從 WRL 移到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
本主題示範如何將 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 程式碼移植到其在 C++/WinRT 中的對等項目。

移植 C+/WinRT 中的第一個步驟是手動新增 C++/WinRT 支援您的專案 (請參閱 [Visual Studio 支援 C++/WinRT，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix))。 若要這樣做，請編輯您的 `.vcxproj` 檔案、尋找 `<PropertyGroup Label="Globals">`，然後在群組屬性裡設定屬性 `<CppWinRTEnabled>true</CppWinRTEnabled>`。 某個效果的該變更為該支援[C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx)便會關閉專案中。 如果您在專案中使用 C++/CX，則您也可以讓支援保持關閉並更新 C++/CX 程式碼為 C++/WinRT (查看[從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md))。 或您可以將支援切換回來 (專案屬性中，**C/C++** \> **一般** \> ** 使用 Windows 執行階段延伸** \> **是 (/ZW)**)，並優先著重在移植您的 WRL 程式碼。 C + + CX 及 C + + WinRT 程式碼可以並存在同一個專案中，除了 XAML 編譯器支援及 Windows 執行階段元件 (請參閱[移至 C + + WinRT 從 C + + CX](move-to-winrt-from-cx.md))。

將專案屬性**一般** \> **目標平台版本**設置為 10.0.17134.0 (Windows 10，版本 1803) 或更高。

在您先行編譯的標頭檔案 (通常是 `pch.h`)，包含 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果包含任何 C++/WinRT 投影 Windows API 標頭 (例如，`winrt/Windows.Foundation.h`)，則您不需要像這樣明確包含 `winrt/base.h`，因為它會自動為您包含。

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>移植 WRL COM 智慧型指標 ([Microsoft: 110:: WRL::ComPtr](/cpp/windows/comptr-class))
移植任何使用 **Microsoft::WRL::ComPtr\<T\>** 的程式碼，使用 [**winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr)。 以下是變更前後的程式碼範例。 在 *之後* 版本中，[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) 成員函式擷取基礎原始指標，如此一來便可以設定它。

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> 如果您必須已經安裝[**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) （其內部的原始指標已經有目標） 與您想要重新座位它以指到不同的物件，則您必須先指派`nullptr`它&mdash;下面的程式碼範例所示。 如果您不要然後已經安裝**com_ptr**將繪出問題到注意 （當您呼叫[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)或[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)） 來判斷其內部的指標不 null。

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

當您想要將基礎原始指標傳遞給函數的預期**IUnknown**的指標時，使用[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function)免費函數，此下一個範例所示。

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

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>移植 WRL 模組 ([Microsoft: 110:: WRL::Module]())
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
* [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相關主題
* [C++/WinRT 的簡介](intro-to-using-cpp-with-winrt.md)
* [從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)
* [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
