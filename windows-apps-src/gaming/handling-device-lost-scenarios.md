---
author: mtoepke
title: "處理 Direct3D 11 中的裝置已移除案例"
description: "本主題說明在移除或重新初始化圖形卡之後，應如何重建 Direct3D 與 DXGI 裝置介面鏈結。"
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
translationtype: Human Translation
ms.sourcegitcommit: 5ed3815397b076ab3ee14fd3c22b235b46da5f09
ms.openlocfilehash: b88d85c78ba5d08718b7e2c844f94beb71e5134a

---

# <a name="span-iddevgaminghandlingdevice-lostscenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>處理 Direct3D 11 中的裝置已移除案例


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本主題說明在移除或重新初始化圖形卡之後，應如何重建 Direct3D 與 DXGI 裝置介面鏈結。

在 DirectX 9 中，應用程式可能會遇到「[裝置遺失](https://msdn.microsoft.com/library/windows/desktop/bb174714)」狀況，導致 D3D 裝置進入無法操作狀態。 例如，當全螢幕的 Direct3D 9 應用程式失去焦點時，Direct3D 裝置會變成「遺失」；使用遺失的裝置嘗試繪圖將會失敗，並且不會顯示訊息。 Direct3D 11 使用虛擬圖形裝置介面，可允許多個程式共用相同的實體圖形裝置，並可避免應用程式失去 Direct3D 裝置控制權的情況。 不過，圖形卡的可用性仍可能變更。 例如：

-   圖形驅動程式已升級。
-   系統從省電圖形卡變更為效能圖形卡。
-   圖形裝置停止回應，並且重設。
-   以實體方式附加或移除圖形卡。

當這類情況發生時，DXGI 會傳回錯誤碼，表示必須重新初始化 Direct3D 裝置，而且必須重建裝置資源。 此逐步解說說明 Direct3D 11 app 與遊戲如何能偵測和回應圖形卡重設、移除或變更的情況。 程式碼範例是從隨附於 Microsoft Visual Studio 2015 的 DirectX 11 App (通用 Windows) 範本中提供的。

# <a name="instructions"></a>指示

### <a name="spanspanstep-1"></a><span></span>步驟 1：

在轉譯迴圈中包含裝置已移除錯誤的檢查。 藉由呼叫 [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) (或 [**Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 等) 來呈現框架。 然後檢查它是否傳回 [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) 或 **DXGI\_ERROR\_DEVICE\_RESET**。

首先，範本會儲存 DXGI 交換鏈結傳回的 HRESULT：

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

在處理呈現框架的其他所有工作後，範本會檢查是否有裝置已移除錯誤。 如有需要，它會呼叫方法以處理裝置已移除狀況：

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>步驟 2：

此外，也在回應視窗大小變更時包含裝置已移除錯誤的檢查。 這很適合用來檢查 [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) 或 **DXGI\_ERROR\_DEVICE\_RESET**，因為：

-   調整交換鏈結大小需要呼叫基礎 DXGI 介面卡，這樣可能會傳回裝置已移除錯誤。
-   應用程式可能已移至連接其他圖形裝置的監視器。
-   當某個圖形裝置移除或重設後，桌面解析度通常會變更，造成視窗大小變更。

範本會檢查 [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577) 傳回的 HRESULT：

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>步驟 3：

只要您的 App 收到 [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) 錯誤，它就必須重新初始化 Direct3D 裝置，並重新建立任何相依於裝置的資源。 釋出以舊版 Direct3D 裝置建立之圖形裝置資源的任何參照；那些資源現在已經失效，而且必須釋出對交換鏈結的所有參照後才能再建立新的參照。

HandleDeviceLost 方法會釋出交換鏈結，並通知應用程式元件釋出裝置資源：

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

接著，它會建立新的交換鏈結，並重新初始化裝置管理類別所控制且相依於裝置的資源。

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

在裝置與交換鏈結重新建立後，它會通知應用程式元件重新初始化與裝置相依的資源：

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

在 HandleDeviceLost 方法結束後，控制權會交還給轉譯迴圈，以繼續繪製下一個框架。

## <a name="remarks"></a>備註


### <a name="investigating-the-cause-of-device-removed-errors"></a>調查裝置已移除錯誤的原因

如果 DXGI 裝置已移除錯誤問題持續發生，可能代表您的圖形程式碼在繪製常式期間正產生無效狀況。 這也可能代表硬體故障或圖形驅動程式有錯誤。 如果要調查裝置已移除錯誤的原因，請在呼叫 [**ID3D11Device::GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526) 之後再釋出 Direct3D 裝置。 此方法會傳回六個可能的 DXGI 錯誤碼的其中一個，表示裝置已移除錯誤的原因：

-   **DXGI\_ERROR\_DEVICE\_HUNG**：因為 app 傳送了無效的圖形命令組合，圖形驅動程式已停止回應。 如果重複出現這個錯誤，很可能是您的 app 導致裝置當機，必須對 app 進行偵錯。
-   **DXGI\_ERROR\_DEVICE\_REMOVED**：圖形裝置已透過實體方式移除、關閉，或發生驅動程式升級的狀況。 這是偶爾會發生的正常情況，您的 app 或遊戲需依照本主題的說明來重建裝置資源。
-   **DXGI\_ERROR\_DEVICE\_RESET**：圖形裝置因為命令格式錯誤而失敗。 如果重複出現這個錯誤，可能表示您的程式碼正在傳送無效的繪圖命令。
-   **DXGI\_ERROR\_DRIVER\_INTERNAL\_ERROR**：圖形驅動程式發生錯誤，並且已重設裝置。
-   **DXGI\_ERROR\_INVALID\_CALL**：應用程式提供的參數資料無效。 即使只遇到這個錯誤一次，也可能代表是您的程式碼導致裝置已移除狀況，必須對程式碼進行偵錯。
-   **S\_OK**：若在沒有使目前的圖形裝置無效的情況下啟用、停用或重設圖形裝置，便會傳回這個錯誤碼。 例如，如果某個 app 正在使用 [Windows Advanced Rasterization Platform (WARP)](https://msdn.microsoft.com/library/windows/desktop/gg615082)，且某個硬體介面卡變成可用時，便會傳回這個錯誤碼。

下列程式碼將擷取 [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) 錯誤碼，並將錯誤碼列印至偵錯主控台。 在 HandleDeviceLost 方法的開頭插入此程式碼：

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

如需詳細資料， 請參閱 [**GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526) 與 [**DXGI\_ERROR**](https://msdn.microsoft.com/library/windows/desktop/bb509553)。

### <a name="testing-device-removed-handling"></a>測試裝置已移除處理

Visual Studio 的「開發人員命令提示字元」支援針對與 Visual Studio 圖形診斷相關之 Direct3D 事件擷取及播放的命令列工具「dxcap」。 您可以在您的 app 執行時使用命令列選項「-forcetdr」，這會強制執行「GPU 逾時偵錯與復原」事件，藉以觸發 DXGI\_ERROR\_DEVICE\_REMOVED，並讓您可以測試您的錯誤處理程式碼。

> **注意** DXCap 和其支援的 DLL 會做為「Windows 10 圖形工具」的一部分安裝到 system32/syswow64。「Windows 10 圖形工具」已不再透過 Windows SDK 的方式散佈。 它們是改為以選用 OS 元件的方式，透過「圖形工具」功能隨選安裝提供，且必須安裝才能啟用並使用「Windows 10 圖形工具」。 如何安裝 Windows 10 圖形工具的詳細資訊可以在這裡找到： <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>

 

 

 







<!--HONumber=Dec16_HO1-->


