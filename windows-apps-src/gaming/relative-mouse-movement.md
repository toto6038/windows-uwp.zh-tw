---
title: 相對滑鼠移動
description: 使用相對滑鼠控制項，這種控制項不使用系統游標，也不會傳回絕對螢幕座標，以追蹤遊戲中滑鼠移動間的像素差異。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, mouse, input,遊戲, 滑鼠, 輸入
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: 71985841e6c0fa764201c179fb12408581823e5e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8780386"
---
# <a name="relative-mouse-movement-and-corewindow"></a>相對滑鼠移動和 CoreWindow

在遊戲中，滑鼠是很常用的控制選項，許多玩家都很熟悉，而且滑鼠對許多遊戲類型而言也都是最基本的，包括第一人稱與第三人稱射擊遊戲，以及即時策略遊戲。 在這裡我們會討論相對滑鼠控制項的實作，這種控制項不使用系統游標，也不會傳回絕對螢幕座標；而是會追蹤滑鼠移動之間的像素差異。

有些應用程式 (例如遊戲) 會使用滑鼠做為更普遍的輸入裝置。 例如，3D 模組器可能使用滑鼠輸入模擬虛擬軌跡球，以設定 3D 物件的方向；或者，有的遊戲可能使用滑鼠，透過滑鼠外觀的控制項來變更檢視相機的方向。 

在這些案例中，應用程式都需要相對滑鼠資料。 相對滑鼠值表示滑鼠從最後一個框架移動了多遠，而不是視窗或螢幕中的絕對 x-y 座標值。 此外，應用程式經常會隱藏滑鼠游標，因為在操縱 3D 物件或場景時，與螢幕座標有關的游標位置是不相關的。 

當使用者採取將應用程式轉換成相對 3D 物件/場景操縱模式的動作時，應用程式必須： 
- 忽略預設的滑鼠處理。
- 啟用相對滑鼠處理。
- 將滑鼠游標設為 Null 指標 (nullptr)，加以隱藏。 

當使用者採取將應用程式從相對 3D 物件/場景操縱模式移出的動作時，應用程式必須： 
- 啟用預設/絕對滑鼠處理。
- 關閉相對滑鼠處理。 
- 將滑鼠游標設為非 Null 值 (讓滑鼠能夠被看見)。

> **注意**  
使用這個模式時，會在進入無游標的相對模式時保存絕對滑鼠游標的位置。 游標會在前次啟用相對滑鼠移動模式時所在的同一個螢幕座標位置上再次出現。

 

## <a name="handling-relative-mouse-movement"></a>處理相對滑鼠移動


若要存取相對滑鼠差異值，請登錄 [MouseDevice::MouseMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mousedevice.mousemoved.aspx) 事件，如此處所示。


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;   // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                     // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                     // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

本程式碼範例中的事件處理常式 **OnMouseMoved** 會根據滑鼠的移動來轉譯檢視。 滑鼠指標的位置會傳送到處理常式，做為 [MouseEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mouseeventargs.aspx) 物件。 

當您的應用程式變成處理相對滑鼠移動值時，會略過處理來自 [CoreWindow::PointerMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointermoved.aspx) 事件的絕對滑鼠資料。 不過，只有在滑鼠輸入的結果是 **CoreWindow::PointerMoved** 事件時 (相較於觸控輸入)，才會略過此輸入。 已將 [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) 設為 **nullptr** 來隱藏游標。 

## <a name="returning-to-absolute-mouse-movement"></a>返回絕對滑鼠移動

當應用程式結束 3D 物件或場景操縱模式，且不再使用相對滑鼠移動時 (例如返回功能表畫面時)，則返回一般的絕對滑鼠移動處理。 此時，會停止讀取相對滑鼠資料，重新啟動標準滑鼠 (及指標) 事件的處理，並將 [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) 設為非 Null 值。 

> **注意**  
當您的應用程式處於 3D 物件/場景操縱模式時 (處理相對滑鼠移動，且游標關閉)，滑鼠無法叫用邊緣 UI，例如常用鍵、上一頁堆疊或應用程式列。 因此，提供一個結束此特殊模式的機制是很重要的，例如最常使用的 **Esc** 鍵。

## <a name="related-topics"></a>相關主題

* [適用於遊戲的移動視角控制項](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [適用於遊戲的觸控控制項](tutorial--adding-touch-controls-to-your-directx-game.md)