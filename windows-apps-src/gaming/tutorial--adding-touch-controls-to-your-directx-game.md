---
title: 適用於遊戲的觸控控制項
description: 了解如何在使用 DirectX 的通用 Windows 平台 (UWP) C++ 遊戲中新增基本的觸控控制項。
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 觸控, 控制項, directx, 輸入
ms.localizationpriority: medium
ms.openlocfilehash: 546d36e26489563720f5aad8a0c9f81f85649e5a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168282"
---
# <a name="touch-controls-for-games"></a>適用於遊戲的觸控控制項



了解如何在使用 DirectX 的通用 Windows 平台 (UWP) C++ 遊戲中新增基本的觸控控制項。 我們將示範如何在 Direct3D 環境中加入觸控控制項來移動固定面相機，透過拖曳手指或手寫筆來移動相機的視角。

您可以將這些控制項納入遊戲，讓玩家可以在 3D 環境 (例如地圖或比賽場) 中拖曳、捲動或移動瀏覽。 例如，在策略或益智遊戲中，您可以使用這些控制項左右移動瀏覽，讓玩家檢視比螢幕還大的遊戲環境。

> **注意**   我們的程式碼也適用于以滑鼠為基礎的移動控制項。 Windows 執行階段 API 已將指標相關事件抽取出來，因此它們可以處理觸控或滑鼠指標事件。

 

## <a name="objectives"></a>目標


-   建立簡單的觸控拖曳控制項，以便在 DirectX 遊戲中移動瀏覽固定面相機。

## <a name="set-up-the-basic-touch-event-infrastructure"></a>設定基本的觸控事件基礎結構


首先，我們要定義基本的控制器類型，在這個範例為 **CameraPanController**。 在這裡，我們將控制器定義為一個抽象的概念，也就是使用者可以執行的一組行為。

**CameraPanController** 類別是會定期重新整理的相機控制器狀態資訊集合，可提供方法讓 app 從它的更新迴圈取得這些資訊。

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

現在，讓我們建立一個定義相機控制器狀態的標頭，以及實作相機控制器互動的基本方法和事件處理常式。

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

public:

    // Methods to get input from the UI pointers
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

私用欄位包含相機控制器目前的狀態。 我們來看看這些狀態。

-   **m \_ 位置** 是相機在場景空間中的位置。 在這個範例中，z 座標值固定在 0。 我們可以使用 DirectX::XMFLOAT2 來表示這個值，但是為了顧及這個範例及未來的擴充性，我們使用 DirectX::XMFLOAT3。 我們會透過 **get \_ Position** 屬性將此值傳遞給應用程式本身，讓它可以據以更新區。
-   **m \_ panInUse** 是一個布林值，指出移動流覽作業是否為作用中; 更明確地說，玩家是否觸及螢幕並移動攝影機。
-   **m \_ panPointerID** 是指標的唯一識別碼。 我們不會在這個範例使用它，但這是在控制器狀態類別與特定指標間建立關聯的最佳做法。
-   **m \_ panFirstDown** 是螢幕上的點，播放程式會先觸及螢幕，或在攝影機平移動作期間按一下滑鼠。 我們稍後會使用這個值來設定靜止區域，以避免觸碰到螢幕時，或稍微移動滑鼠時造成抖動。
-   **m \_ panPointerPosition** 是播放程式目前移動指標的畫面上的點。 我們使用它來判斷播放程式想要移動的方向（相對於 **m \_ panFirstDown**）。
-   **m \_ panCommand** 是相機控制器的最後計算命令：向上、向下、向左或向右。 因為我們使用固定在 x-y 平面上的相機，所以這裡可以改用 DirectX::XMFLOAT2 值。

我們使用這 3 個事件處理常式來更新相機控制器狀態資訊。

-   **OnPointerPressed** 是玩家在觸控式螢幕上按下手指且指標移動到按下位置的座標時，app 會呼叫的事件處理常式。
-   **OnPointerMoved** 是玩家在觸控式螢幕上撥動手指時，app 會呼叫的事件處理常式。 它會使用拖曳路徑的新座標進行更新。
-   **OnPointerReleased** 是玩家從觸控式螢幕上移開按住的手指時，app 會呼叫的事件處理常式。

最後，我們使用這些方法和屬性來初始化、存取以及更新相機控制器狀態資訊。

-   **Initialize** 是一個事件處理常式，app 會呼叫它來初始化控制項，並將它們附加到描述顯示視窗的 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 物件。
-   **SetPosition** 是 app 呼叫來設定場景區域中控制項的 x、y 以及 z 座標的方法。 請注意，這個教學課程中 z 座標會一直保持為 0。
-   **取得 \_Position** 是應用程式存取的屬性，可用來取得場景空間中相機的目前位置。 您使用這個屬性做為告知 app 相機目前位置的方法。
-   **取得 \_FixedLookPoint** 是我們的應用程式存取的屬性，可用來取得控制器相機所面對的目前點。 在這個範例中，它是鎖定與 x-y 平面垂直。
-   **Update** 是讀取控制器狀態並更新相機位置的方法。 您將從 App 的主迴圈中持續呼叫這個 &lt;something&gt;，以重新整理相機控制器資料及場景區域中相機的位置。

您現在已經了解實作觸控控制項所需的全部元件。 您可以偵測發生觸控或滑鼠指標事件的時間和位置，以及發生的動作。 您可以設定相機與場景區域的相對位置和方向，並追蹤變更。 最後，您可以將新的相機位置傳送到呼叫應用程式。

現在，我們將這些設定連繫起來。

## <a name="create-the-basic-touch-events"></a>建立基本觸控事件


Windows 執行階段事件調派程式提供 3 個我們要應用程式處理的事件：

-   [**PointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed)
-   [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved)
-   [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased)

這些事件都在 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 類型上實作。 我們假設您有一個要使用的 **CoreWindow** 物件。 如需詳細資訊，請參閱[如何設定您的 UWP C++ app 來顯示 DirectX 檢視](/previous-versions/windows/apps/hh465077(v=win.10))。

由於我們的 app 會在執行時觸發這些事件，因此處理常式會更新我們在私用欄位中定義的相機控制器狀態資訊。

首先，我們填入觸控指標事件處理常式。 在第一個事件處理常式 (**OnPointerPressed**) 中，當使用者觸碰螢幕或按一下滑鼠時，我們會取得指標的 x-y 座標 (從管理顯示的 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 取得)。

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

我們會使用這個處理常式，讓目前的 **CameraPanController** 實例知道，您可以藉由將 **m \_ panInUse** 設定為 TRUE，將相機控制器視為使用中。 如此一來，當 app 呼叫 **Update** 時，會使用目前的位置資料來更新檢視區。

我們已經建立當使用者觸控螢幕或在顯示視窗中按一下時，相機移動的基本值，現在必須決定當使用者按住螢幕拖曳或按住滑鼠按鍵移動時應該執行的動作。

只要玩家在螢幕上拖曳指標讓它移動時，就會啟動 **OnPointerMoved** 事件處理常式。 我們需要讓 app 知道指標的目前位置，而這就是我們的方式。

**OnPointerMoved**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

最後，我們需要在玩家停止觸控螢幕時停用相機平移動作。 我們會使用 **OnPointerReleased**，它會在 [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) 引發時呼叫，以將 **m \_ panInUse** 設定為 FALSE，並關閉相機平移移動，然後將指標識別碼設定為0。

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>初始化觸控控制項以及控制器狀態


讓我們連結事件並初始化相機控制器的所有基本狀態欄位。

**初始 化**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

**Initialize** 會將 app 的 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 執行個體參考當作一個參數，並將我們已開發的事件處理常式登錄到該 **CoreWindow** 上的適當事件。

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>取得和設定相機控制器的位置


讓我們定義一些方法，以取得和設定場景區域中相機控制項的位置。

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**SetPosition** 是公用方法，如果我們需要將相機控制器位置設定在特定點上，可以從 app 進行呼叫。

**取得 \_Position** 是最重要的公用屬性：這是我們的應用程式在場景空間中取得相機控制器目前位置的方式，因此可以據此更新區。

**取得 \_FixedLookPoint** 是公用屬性，在此範例中，會取得對 x-y 平面而言是正常的外觀點。 如果您想要為固定相機建立更多斜視角度，那麼計算 x、y 及 z 的座標值時，您可以變更這個方法來使用三角函數 sin 和 cos。

## <a name="updating-the-camera-controller-state-information"></a>更新相機控制器狀態資訊


現在，我們會執行計算，將在 **m \_ panPointerPosition** 中追蹤的指標座標資訊轉換成我們的3d 場景空間各自的新座標資訊。 應用程式會在我們每次重新整理主應用程式迴圈時，呼叫這個方法。 此時我們會計算要傳送給 app 的新位置 資訊，用於投影到檢視區前更新檢視矩陣。

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

因為我們不希望觸控或滑鼠抖動而讓相機移動瀏覽不穩定，所以要在指標周圍建立一個靜止區域，直徑為 32 個像素。 我們也可以設定速度比，在這個範例是 1:1，在指標穿越靜止區域時追蹤像素。 您可以調整這個行動，減慢或加速移動速率。

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>使用新相機位置來更新視圖矩陣


我們現在可以取得相機聚焦的場景區域座標，而且可以指定 app 更新的時間 (例如，在主應用程式迴圈每 60 秒更新一次)。 這個虛擬程式碼會建議您可以實作的呼叫行為：

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

恭喜！ 您已經在遊戲中實作一組簡單的相機移動瀏覽觸控控制項。


 

 

 