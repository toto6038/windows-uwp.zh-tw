---
author: mtoepke
title: 適用於遊戲的移動視角控制項
description: 了解如何將傳統的滑鼠和鍵盤移動視角控制項 (也稱為用滑鼠視角 (mouselook) 控制項) 加入到您的 DirectX 遊戲。
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 移動視角, 控制項
ms.localizationpriority: medium
ms.openlocfilehash: 219d014eb03803ace440dc1c1773043a9ecbc99f
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6979748"
---
# <a name="span-iddevgamingtutorialaddingmove-lookcontrolstoyourdirectxgamespanmove-look-controls-for-games"></a><span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>適用於遊戲的移動視角控制項



了解如何將傳統的滑鼠和鍵盤移動視角控制項 (也稱為用滑鼠視角 (mouselook) 控制項) 加入到您的 DirectX 遊戲。

我們也會討論觸控裝置的移動視角支援：將螢幕左下區域定義為移動控制器 (行為類似方向輸入)，以及將螢幕的其他區域定義為視角控制器 (將相機放置在玩家上次觸碰的位置中央)。

如果您對這種控制項概念不熟悉，請想像一下：在這個 3D 空間中，鍵盤 (或觸控式方向輸入鍵) 控制您的腿，而您的腿只能前後或左右移動。 滑鼠 (或觸控指標) 控制您的頭。 轉動頭即可看往一個方向：左右、上下，或平面中的任一方向。 如果視角中有射擊目標，您應該使用滑鼠讓相機視角對準這個目標，然後按前進按鍵來接近目標，或是按後退按鍵以遠離目標。 若要繞著目標移動，您應該讓相機視角對準這目標，並同時向左或向右移動。 您會發現，這些控制方式可讓您在 3D 環境中非常順暢地進行瀏覽！

這些控制項在遊戲中通稱為 WASD 控制項，您可以使用 W、A、S 及 D 按鍵來控制 x-z 立體固定相機的移動方向，滑鼠則是用來控制相機在 x 和 y 軸上的旋轉方式。

## <a name="objectives"></a>目標


-   將基本移動視角控制項加入使用滑鼠和鍵盤 (以及使用觸控式螢幕) 的 DirectX 遊戲中。
-   實作用來瀏覽 3D 環境的第一人稱相機。

## <a name="a-note-on-touch-control-implementations"></a>觸控控制項實作的注意事項


我們會針對觸控控制項實作兩個控制器：移動控制器 (處理與相機視點相對的 x-z 立體移動) 以及視角控制器 (瞄準相機的視點)。 我們的移動控制器會對應到鍵盤的 WASD 按鍵，而視角控制器則是對應到滑鼠。 但是在觸控控制項中，我們不但需要定義可輸入方向的螢幕區域或虛擬 WASD 按鍵，也需要定義作為視角控制項輸入區域的螢幕其他區域。

我們的螢幕看起來像這樣。

![移動視角控制器配置](images/movelook-touch.png)

當您移動螢幕左下角的觸控指標 (不是滑鼠 ！) 時，任何向上的移動會讓相機向前移動。 任何向下的移動則會讓相機向後移動。 而在移動控制器的指標範圍內左右移動相機就會左右移動。 在這個範圍外 (就是視角控制器)，您只需觸碰或拖曳相機即可調整您要面對的方向。

## <a name="set-up-the-basic-input-event-infrastructure"></a>設定基本的輸入事件基礎結構


首先，我們必須建立用來處理滑鼠和鍵盤輸入事件的控制項類別，並根據該輸入來更新相機視角。 因為我們要實作移動視角控制項，所以呼叫 **MoveLookController**。

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

現在，讓我們建立一個標頭來定義移動視角控制器的狀態以及它的第一人稱相機，並建立基本方法和事件處理常式來實作 控制項及更新相機狀態。

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


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

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

我們的程式碼包含 4 組私用欄位。 讓我們看看每個欄位的用途。

我們首先定義一些欄位來儲存相機視角的更新資訊。

-   **m\_position** 是 3D 場景中使用場景座標的相機位置 (即為視圖平面)。
-   **m\_pitch** 是相機的上下移動，或以弧形繞著視圖平面 x 軸上下旋轉。
-   **m\_yaw** 是相機的左右偏移，或以弧形繞著視圖平面 y 軸左右旋轉。

現在，讓我們定義用來存放控制器狀態和位置資訊的欄位。 我們先為觸控式移動控制器定義所需的欄位。 (移動控制器的鍵盤實作並沒有特別的需求。 我們只需要使用特定處理常式讀取鍵盤事件即可。)

-   **m\_moveInUse** 指示移動控制器是否正在使用中。
-   **m\_movePointerID** 是目前移動指標的唯一識別碼。 當我們檢查指標識別碼值時，會使用它來區別視角指標和移動指標。
-   **m\_moveFirstDown** 是玩家在螢幕上第一次觸碰移動控制器指標區域的點。 我們稍後會使用這個值來設定靜止區域，不讓細微的移動造成視角抖動。
-   **m\_movePointerPosition** 是螢幕上玩家目前將指標移過去的目標點。 我們透過檢查它與 **m\_moveFirstDown** 的相對位置，以判斷玩家想要移動的方向。
-   **m\_moveCommand** 是移動控制器最後計算出來的命令：上 (前)、下 (後)、左、右。

現在，我們定義視角控制器的欄位 (針對滑鼠與觸控實作)。

-   **m\_lookInUse** 指示視角控制項是否正在使用中。
-   **m\_lookPointerID** 是目前視角指標的唯一識別碼。 當我們檢查指標識別碼值時，會使用它來區別視角指標和移動指標。
-   **m\_lookLastPoint** 是場景座標中的最後一點 (從先前的畫面中擷取而來)。
-   **m\_lookLastDelta** 是目前的 **m\_position** 和 **m\_lookLastPoint** 之間計算出來的差異。

最後，讓我們為 6 個角度的移動方向定義 6 個布林值，我們會使用這些值來指示每個方向移動動作的目前狀態 (開或關)：

-   **m\_forward**、**m\_back**、**m\_left**、**m\_right**、**m\_up** 及 **m\_down**。

我們使用 6 個事件處理常式來擷取更新控制器狀態的輸入資料：

-   **OnPointerPressed**。 玩家按下代表遊戲螢幕指標的滑鼠左鍵，或觸碰螢幕。
-   **OnPointerMoved**。 玩家移動滑鼠來移動遊戲螢幕上的指標，或在螢幕上拖曳觸控指標。
-   **OnPointerReleased**。 玩家放開代表遊戲螢幕指標的滑鼠左鍵，或停止觸碰螢幕。
-   **OnKeyDown**。 玩家按下按鍵。
-   **OnKeyUp**。 玩家放開按鍵。

最後，我們使用這些方法和屬性來初始化、存取以及更新控制器的狀態資訊。

-   **Initialize**。 我們的 app 會呼叫這個事件處理常式來初始化控制項，並將它們附加到描述我們顯示視窗的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 物件。
-   **SetPosition**。 我們的 app 會呼叫這個方法來設定場景區域中控制項的 x、y 及 z 座標。
-   **SetOrientation**。 我們的 app 會呼叫這個方法來設定相機的上下移動和左右偏移。
-   **get\_Position**。 我們的 app 會存取這個屬性來取得場景區域中相機的目前位置。 把這個屬性當作將目前相機位置傳送到 app 的方法。
-   **get\_LookPoint**。 我們的應用程式會存取這個屬性來取得控制器相機目前面對的視點。
-   **Update**。 讀取移動和視角控制器的狀態並更新相機位置。 您會從 app 的主迴圈持續呼叫這個方法，進而重新整理相機控制器資料以及場景區域中的相機位置。

到目前為止，您已經有了實作移動視角控制項所需的全部元件了。 所以，讓我們將這些元件連結起來。

## <a name="create-the-basic-input-events"></a>建立基本輸入事件


Windows 執行階段事件發送器提供了要讓 **MoveLookController** 類別執行個體處理的 5 個事件：

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)

這些事件都在 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 類型上實作。 我們假設您有一個要使用的 **CoreWindow** 物件。 如果您不知道如何取得這個物件，請參閱[如何設定您的通用 Windows 平台 (UWP) C++ app 來顯示 DirectX 檢視](https://msdn.microsoft.com/library/windows/apps/hh465077)。

如果在 app 執行時引發了這些事件，處理常式會更新我們在私用欄位中定義的控制器狀態資訊。

首先，讓我們填入滑鼠和觸控指標事件處理常式。 在第一個事件處理常式 **OnPointerPressed()** 中，我們要從 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 中取得指標的 x-y 座標，這個類別可在使用者於視角控制器區域中按一下滑鼠或觸控螢幕時，管理我們的顯示。

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

這個事件處理常式會檢查指標是否不是滑鼠 (因為在這個範例中同時支援滑鼠和觸控)，以及指標是否位於移動控制器區域。 如果上述兩個條件為 True，則它會測試 **m\_moveInUse** 是否為 False，藉此檢查指標是否剛剛被按下，並特別檢查這個按一下的動作是否與之前的移動或視角輸入無關。 如果是這樣，處理常式會擷取移動控制器區域中發生按下動作的點，並將 **m\_moveInUse** 設成 True，這樣再次呼叫這個處理常式時，它 就不會覆寫移動控制器輸入互動的起點。 它也會將移動控制器指標識別碼更新為目前指標的識別碼。

如果指標是滑鼠或者如果觸控指標沒有在移動控制器區域中，那麼它就一定是在視角控制器區域中。 它會將 **m\_lookLastPoint** 設成使用者按下滑鼠按鍵或觸控按下的目前位置，並重設距離差異，然後將視角控制器指標識別碼更新為目前指標的識別碼。 它也會將視角控制器的狀態設成作用中。

**OnPointerMoved**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

每當指標移動時 (以此為例，即在使用者拖曳觸控式螢幕指標，或按住滑鼠左鍵並移動滑鼠指標的情況下)，都會引發 **OnPointerMoved** 事件處理常式。 如果指標識別碼與移動控制器指標的識別碼相同，那麼它就是移動指標；不然我們會檢查它是否為視角控制器的作用中指標。

如果它是移動控制器，我們就只要更新指標位置。 只要 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 事件持續引發，我們就會一直更新它，因為我們要將最後位置與使用 **OnPointerPressed** 事件處理常式擷取的最初位置進行比對。

如果它是視角控制器，就會比較複雜。 我們需要計算新的視點並把相機放在它中央，因此我們會計算最後視點與目前螢幕位置的距離差異，然後乘以縮放係數，讓視角移動相對於螢幕移動的距離縮小或放大。 我們會使用這個值計算上下移動或左右偏移。

最後，當玩家停止移動滑鼠或觸控螢幕時，我們需要停用移動或視角控制器行為。 我們使用 **OnPointerReleased** ([**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 引發時呼叫它)，將 **m\_moveInUse** 或 **m\_lookInUse** 設成 FALSE 並關閉相機平移移動，然後將指標識別碼全部用零來表示。

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

到這裡，我們處理了所有的觸控式螢幕事件。 現在，讓我們處理鍵盤移動控制器的按鍵輸入事件。

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

只要按下任何一個按鍵，這個事件處理常式就會將對應的方向移動狀態設成 True。

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

而放開按鍵時，這個處理常式就會將它設回 False。 當我們呼叫 **Update** 時，它會檢查這些方向移動狀態，並據此移動相機。 這比觸控實作稍稍簡單一點！

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>初始化觸控控制項以及控制器狀態


讓我們開始執行事件，並初始化所有的控制器狀態欄位。

**Initialize**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to recieve touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

**Initialize** 會將 app 的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 執行個體參考當作一個參數，並將我們已開發的事件處理常式登錄到該 **CoreWindow** 上的適當事件。 在應用程式啟動時，它會初始化移動和視角指標的識別碼，將觸控式螢幕移動控制器實作的命令向量設成零，並將相機視角設成正前方。

## <a name="getting-and-setting-the-position-and-orientation-of-the-camera"></a>取得和設定相機的位置和方向


讓我們定義一些方法來取得和設定檢視區中的相機位置。

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## <a name="updating-the-controller-state-info"></a>更新控制器狀態資訊


現在，我們要執行計算來將 **m\_movePointerPosition** 中追蹤到的指標座標資訊，轉換為使用世界座標系統的新座標資訊。 應用程式會在我們每次重新整理主應用程式迴圈時，呼叫這個方法。 所以此時我們會計算要傳送給 app 的新視點位置資訊，以便在投影到檢視區前更新檢視矩陣。

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

因為我們不希望玩家在使用觸控式移動控制器時發生抖動的情形，所以會在指標周圍建立一個靜止區域 (直徑為 32 個像素)。 我們同時也會加入速度 (命令值加上移動增率)。 (您可以根據自己的喜愛調整這個行為，根據指標在移動控制器區域中的移動距離來減慢或加速移動速率。)

當我們計算速度時，也會將從移動和視角控制器收到的座標，轉譯為實際視點的移動 (我們會將這個移動傳送給計算場景視圖矩陣的方法)。 首先，先反轉 x 座標，因為如果使用視角控制器來按一下移動或左右拖曳，場景中的視點會以相反的方向旋轉，相機可能會在中央軸線上晃動。 接著，我們交換 y 和 z 軸，因為移動控制器上的向上/向下鍵按下動作或觸碰拖曳動作 (讀取為 y 軸行為)，應該轉譯為將視點移入或移出螢幕的相機動作 (z 軸)。

玩家視點的最後位置會是最後位置加上計算的速度，而且當它呼叫 **get\_Position** 方法 (最可能發生在設定每個畫面時) 時轉譯器就會讀取這個資料。 之後，我們要將移動命令重設為零。

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>使用新相機位置來更新視圖矩陣


我們可以取得相機聚焦的場景區域座標，只需通知應用程式就可以更新它 (例如，在主應用程式迴圈中每 60 秒一次)。 這個虛擬程式碼會建議您可以實作的呼叫行為：

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

恭喜！ 您已經在遊戲中實作觸控式螢幕和鍵盤/滑鼠輸入觸控控制項的基本移動視角控制項！



 

 




