---
author: abbycar
title: 新增控制項
description: 現在，我們看看遊戲範例如何在 3D 遊戲中實作移動視角控制項，以及如何開發基本的觸控控制項、滑鼠控制項以及遊戲控制器控制項。
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.author: abigailc
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 控制項, 輸入
ms.localizationpriority: medium
ms.openlocfilehash: bc5873486bdd9c4adf4ea74b10a240617143ad23
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6258030"
---
# <a name="add-controls"></a>新增控制項


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

一個好的通用 Windows 平台 (UWP) 遊戲可以支援多種介面。 潛在玩家可能會有 windows 10 上沒有實體按鈕連接，Xbox 控制器的電腦與平板電腦或最新傳統型遊戲也許有高效能滑鼠和遊戲鍵盤。 在我們的遊戲中，控制項是在 [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) 類別中實作。 這個類別彙總三種輸入 (滑鼠和鍵盤、觸控板及遊戲台) 到單一控制器。 最後的結果是第一人稱射擊遊戲使用可搭配多個裝置運作的一般標準移動視角控制項。

> [!NOTE]
> 如需有關控制項的詳細資訊，請參閱[適用於遊戲的移動視角控制項](tutorial--adding-move-look-controls-to-your-directx-game.md)和[適用於遊戲的觸控控制項](tutorial--adding-touch-controls-to-your-directx-game.md)。


## <a name="objective"></a>目標

此時，我們有會轉譯的遊戲，但我們無法四處移動我們的玩家或射擊目標。 我們會查看我們的遊戲如何在我們的 UWP DirectX 遊戲中實作下列類型輸入的第一人稱射擊的移動視角控制項。
- 鍵盤與滑鼠
- 觸控
- 遊戲台

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="common-control-behaviors"></a>通用控制項行為


觸控控制項和滑鼠/鍵盤控制項具有非常相似的核心實作。 在 UWP 應用程式中，指標就是螢幕上的點。 您可以滑動滑鼠或在觸控式螢幕上滑動手指來移動指標。 因此您只需登錄一組事件，不用理會玩家在移動和按下指標時使用的是滑鼠或觸控式螢幕。

在遊戲範例中初始化 **MoveLookController** 類別時，它會登錄四個指標專用的事件以及一個滑鼠專用的事件：

事件 | 描述
:------ | :-------
[**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) | 按下 (或按住) 滑鼠左鍵或右鍵，或觸碰觸控式螢幕。
[**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) |移動滑鼠，或在觸控式螢幕上進行拖曳動作。
[**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) |放開滑鼠左鍵，或是讓與觸控式螢幕接觸的物件離開觸控式螢幕。
[**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275) |指標移出主視窗。
[**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) | 滑鼠移動了特定距離。 請注意，我們只對滑鼠移動差異值有興趣，而不關心目前的 X-Y 位置。


只要 **MoveLookController** 在應用程式視窗中初始化，這些事件處理常式會設定為開始收聽使用者的輸入。
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

完整的 [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) 程式碼可以在 GitHub 上看到。


若要判定何時遊戲應該收聽特定輸入，**MoveLookController** 類別提供三種控制器專用的狀態，無論控制項類型為何：

狀態 | 描述
:----- | :-------
**無** | 這是控制器的初始化狀態。 遊戲還沒有預期任何控制器輸入，所以會忽略所有輸入。
**WaitForInput** | 控制器正在等候玩家確認來自遊戲的訊息，可使用滑鼠左鍵、觸控事件或遊戲台上的選單鍵。
**使用中** | 控制器在使用中遊戲播放模式。



### <a name="waitforinput-state-and-pausing-the-game"></a>WaitForInput 狀態和暫停遊戲

當遊戲暫停時，遊戲會進入 **WaitForInput** 狀態。 當玩家將指標移到遊戲主視窗外，或按下暫停按鈕 (P 按鍵或遊戲台 \[**開始** \] 按鈕)，就會發生此狀況。 **MoveLookController** 會登錄按鍵動作，並在呼叫 [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) 方法時通知遊戲迴圈。 此時，如果 **IsPauseRequested** 傳回 **true**，遊戲迴圈會在 **MoveLookController** 上呼叫 **WaitForPress**，讓控制器進入 **WaitForInput** 狀態。 


一旦在 **WaitForInput** 狀態，遊戲會停止處理幾乎所有遊戲輸入事件，直到它會返回 \[**使用中**\] 狀態。 例外的是暫停按鈕，按此按鈕會造成遊戲回復到使用中的狀態。 暫停按鈕以外，為了讓遊戲回復到**使用中**狀態玩家需要選取功能表項目。 



### <a name="the-active-state"></a>使用中狀態

在 \[**使用中**\] 狀態期間，**MoveLookController** 執行個體會處理來自所有啟用輸入裝置的輸入事件，並解譯玩家想做的事。 因此，它會更新玩家視角的速度和視角方向，並從遊戲迴圈呼叫 **Update** 之後與遊戲共用更新的資料。


所有指標輸入在 \[**使用中**\] 狀態中都會被追蹤，不同的指標 ID 會對應到個別的指標動作。
收到 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) 事件時，**MoveLookController** 會取得視窗建立的指標 ID 值。 指標 ID 代表特定的輸入類型。 例如，多點觸控裝置可以同時有多個不同的作用中輸入。 ID 可追蹤玩家目前使用哪一種輸入。 如果單一事件發生在觸控式螢幕的移動矩形中，就會指派一個指標 ID 來追蹤移動矩形中的任何指標事件。 而其他射擊矩形中的指標事件則會使用不同的指標 ID 另外追蹤。


> [!NOTE]
> 從滑鼠和遊戲台右搖桿的輸入也有分別處理的 ID。

將指標事件對應到特定遊戲動作後，就應該更新 **MoveLookController** 物件與主遊戲迴圈共用的資料。

呼叫時，遊戲範例中的 [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 方法會處理輸入，並更新速度和視角方向變數 (**m\_velocity** 和 **m\_lookdirection**)，遊戲迴圈接著會透過呼叫公用的 [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) 和 [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) 方法來擷取。

> [!NOTE]
> 有關 [**Update**](#the-update-method) 方法的更多詳細資料，稍後可在本頁面上看到。




遊戲迴圈可藉由在 **MoveLookController** 執行個體上呼叫 **IsFiring** 方法，測試玩家是否可以正常射擊。 **MoveLookController** 會檢查玩家是否按下三種輸入類型中的其中一種射擊按鈕。

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```









現在，讓我們更仔細地實作這三種控制項類型。

## <a name="adding-relative-mouse-controls"></a>新增相對滑鼠控制項


如果偵測到滑鼠的動作，我們要使用這個動作來決定相機的新上下移動和左右偏移。 我們執行的方式是透過實作相對滑鼠控制項，處理滑鼠已經移動的相對距離 (即移動開始與停止之間的差異值)，而不是記錄移動的絕對 x-y 像素座標。

若要這樣做，我們檢查 [**MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) 事件傳回的 [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://msdn.microsoft.com/library/windows/apps/hh758358) 引數物件上的 [**MouseDelta::X**](https://msdn.microsoft.com/library/windows/apps/hh758353) 和 **MouseDelta::Y** 欄位，獲取 X (水平移動) 與 Y (垂直移動) 座標。

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="adding-touch-support"></a>新增觸控支援

觸控控制項非常適合支援平板電腦的使用者。 此遊戲會透過區分螢幕的特定區域，每個區域配合特定遊戲內動作，來收集觸控輸入。
此遊戲的觸控輸入使用三個區域。

![移動視角觸控配置](images/simple-dx-game-controls-touchzones.png)

下列命令摘要我們的觸控控制行為。
使用者輸入 | 動作
:------- | :--------
移動矩形 | 觸控輸入會轉換為虛擬搖桿，其中垂直動作會轉譯為向前/向後移動位置，而水平動作會轉譯為向左/向右移動位置。
射擊矩形 | 射擊球體。
移動以外的觸控和射擊矩形 | 變更攝影機檢視的旋轉 (上下移動和左右偏移)。

**MoveLookController** 會檢查指標 ID，判斷事件發生的位置，並且採取下列其中一個動作：

-   如果 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 事件是在移動或射擊矩形中發生，請更新控制器的指標位置。
-   如果 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 事件是在螢幕的其他位置發生 (定義為視角控制項)，就會計算視角方向向量的上下和左右偏移變化。


一旦我們實作我們的觸控控制項，我們之前使用 Direct2D 繪製的矩形也會向玩家指出移動、射擊和查看區域的位置。


![觸控控制項](images/simple-dx-game-controls-showzones.png)


現在我們來看看我們如何實作每個控制項。


### <a name="move-and-fire-controller"></a>移動和射擊控制器
螢幕左下方象限中的移動控制器矩形是用做方向鍵。 在此空間內將您的拇指向左和向右滑動，可將玩家向左和向右移動，而向上和向下滑動則可將相機向前和向後移動。
設定好這個之後，輕觸螢幕右下方象限中的射擊控制器會射擊球體。

[**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) 和 [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) 方法會建立我們的輸入矩形，使用兩個 2D 向量來指定每個矩形在螢幕上的左上角和右下角位置。 


然後參數會指派給 **m\_fireUpperLeft** 和 **m\_fireLowerRight**，這可協助我們判斷使用者是否在矩形內觸碰。 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

如果螢幕調整大小，這些矩形會重新描繪為適當大小。


現在，我們已區分我們的控制項，是時候判斷使用者實際上使用它們的時間。
若要這樣做，我們在 **MoveLookController::InitWindow** 方法中設定了一些事件處理常式，因為使用者當時按下、移動或放開他們的指標。

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


我們會先判斷當使用者第一次使用 [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 方法在移動或射擊矩形內按下時，所發生的情況。
以下我們檢查他們觸摸控制項的位置，以及指標是否已在該控制器。 如果這是第一個手指觸控特定控制項，我們會執行下列動作。
- 以 2D 向量在 **m\_moveFirstDown** 或 **m\_fireFirstDown** 儲存共用的位置。
- 指派指標 ID 為 **m\_movePointerID** 或 **m\_firePointerID**。
- 設定適當的 **InUse** 旗標 (**m\_moveInUse** 或 **m\_fireInUse**) 為 `true`，因為我們已經有該控制項的使用中指標。


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


現在，我們已經判斷使用者是否碰觸移動或射擊控制項，我們會查看玩家是否用他們按下的手指進行任何移動。
使用 [**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) 方法，我們檢查哪些指標已經移動，然後以 2D 向量儲存其新的位置。  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


一旦使用者在控制項內做出手勢，它們將會釋放指標。 使用 [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 方法，我們判斷哪個指標已經釋放，並進行一系列的重設。


如果移動控制項已經釋放，我們會執行下列動作。
- 設定玩家在所有方向的速度為 `0`，以防止玩家在遊戲中移動。
- 將 **m\_moveInUse** 切換為 `false`，因為使用者不再觸碰移動控制器。
- 將移動指標 ID 設定為 `0`，因為移動控制器中不再有指標。

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


對於射擊控制項，如果它已經釋放，我們能做的就是切換 **m_fireInUse** 旗標為 `false`，射擊指標 ID 切換為 `0`，因為標射擊控制項中不再有指標。
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>視角控制器
我們會將螢幕未使用區域的觸控裝置指標事件視為視角控制器。 在此區域四處滑動手指會變更玩家相機的上下移動和左右偏移 (旋轉)。


如果在此區域的觸控裝置上引發了 **MoveLookController::OnPointerPressed** 事件，而且遊戲狀態設為 \[**使用中**\]，就會對它指派一個指標 ID。

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
您可以查看 [GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259) 上 **MoveLookController::OnPointerPressed** 方法的完整程式碼。




以下 **MoveLookController** 指派指標 ID 給觸發事件到對應至視角區域的特定變數的指標。 對於觸控，視角區域中發生， **m\_lookPointerID**變數會設定成引發事件的指標 ID。 布林值變數 **m\_lookInUse** 也會設定，以指出控制項尚未釋放。

現在，我們來看看遊戲範例如何處理 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 觸控螢幕事件。


在 **MoveLookController::OnPointerMoved** 方法中，我們查看何種指標 ID 已指派給事件。 如果這是 **m_lookPointerID**，我們會計算指標位置的變動。
然後，我們會使用此差異來計算應該變更多少旋轉。 最後，我們目前所在點，是我們可以更新要在遊戲中使用的 **m\_pitch** 和 **m\_yaw**，以變更玩家的旋轉。 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





我們最後看的是遊戲範例如何處理 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 觸控螢幕事件。
之後，使用者已完成觸控手勢，並將他們手指從螢幕移開，[**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 便會起始。
如果引發  [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 事件的指標 ID 是先前記錄移動指標的 ID，**MoveLookController** 會將速度設為 `0`，因為玩家已經停止觸碰視角區域。

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>新增滑鼠和鍵盤支援


這個遊戲具有鍵盤和滑鼠的下列控制項配置。

使用者輸入 | 動作
:------- | :--------
W | 將玩家向前移動
A | 將玩家向左移動
S | 將玩家向後移動
D | 將玩家向右移動
X | 將檢視向上移動
空格鍵 | 將檢視向下移動
P | 暫停遊戲
滑鼠移動 | 變更攝影機檢視的旋轉 (上下移動和左右偏移)
滑鼠左鍵 | 射擊球體


若使用鍵盤，遊戲範例會在 [**MoveLookController::InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) 方法中登錄兩個新的事件，分別為 [**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) 和 [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)。 這些事件會處理按鍵的按下和釋放。

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

雖然滑鼠使用指標，但是它的處理方式與觸控控制項稍微不同。 為配合我們的控制項配置，每當滑鼠移動時，**MoveLookController** 會旋轉相機，當按下滑鼠左鍵時則會射擊。


這是在 **MoveLookController** 的 [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 方法中處理。

在這個方法中，我們會查看何種指標裝置搭配 [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Input.PointerDeviceType) 列舉使用。 如果遊戲為 \[**使用中**\] 且 **PointerDeviceType** 不是 **Touch**，我們會假設其為滑鼠輸入。

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            break;
        }
```

當玩家停止按下其中一個滑鼠按鍵時，會引發 [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) 滑鼠事件，呼叫 [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 方法，然後輸入完成。 此時，若按下滑鼠左鍵並且現在放開，球體會停止射擊。 因為視角一直在啟用狀態，所以遊戲會繼續使用相同的滑鼠指標來追蹤進行中的視角事件。

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```



現在我們來看看我們將會支援的最後控制項類型：遊戲台。 遊戲台與觸控和滑鼠控制項的處理方式不同，因為它們不使用指標物件。 因此，將需要新增幾個新事件處理常式和方法。

## <a name="adding-gamepad-support"></a>新增遊戲台支援


針對此遊戲，會透過呼叫 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) API 來新增遊戲台支援。 這組 API 提供遊戲控制器輸入的存取，例如賽車方向盤和飛行桿。 


以下將是我們的遊戲台控制項。

使用者輸入 | 動作
:------- | :--------
左類比搖桿 | 移動玩家
右類比搖桿 | 變更攝影機檢視的旋轉 (上下移動和左右偏移)
RT 鍵 | 射擊球體
開始/選單按鈕 | 暫停或繼續遊戲




在 [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) 方法中，我們新增兩個新事件，以判斷遊戲台是否已經[新增](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105)或[移除](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114)。 下列事件會更新 **m_gamepadsChanged** 屬性。 這是在**UpdatePollingDevices**方法用來檢查已知遊戲台的清單是否有所變更。 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

### <a name="the-updatepollingdevices-method"></a>UpdatePollingDevices 方法

**MoveLookController** 執行個體的 [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) 方法會立即檢查是否已經連接遊戲台。 如果有，我們將使用 [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading) 開始讀取其狀態。 這會傳回 [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading) 結構，讓我們可查看哪些按鈕已按下或移動搖桿。


如果遊戲的狀態是 **WaitForInput**，我們只會聽取控制器的開始/選單鍵，如此遊戲可以繼續。


如果是 \[**使用中**\]，我們會檢查使用者的輸入，並判斷需要發生什麼遊戲內動作。
例如，如果使用者將左類比搖桿向特定方向移動，這可讓遊戲知道我們需要將玩家往搖桿移動的方向移動。 將搖桿往特定方向移動的距離必須登錄為大於**靜止區域**的半徑，否則不會發生任何動作。 您必須使用這個靜止區域半徑來防止「飄移」移動，也就是玩家將拇指放在控制器搖桿上的稍微移動。 沒有靜止區域，控制項可能對使用者過於敏感。


搖桿輸入在 x 和 y 軸是介於 -1 到 1 之間。 下列常數指定搖桿靜止區域的半徑。

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

使用此變數，我們將再開始處理可操作的搖桿輸入。 移動會在任一軸上以從 [-1, -.26] 或 [.26, 1] 的值發生。

![搖桿的靜止區域](images/simple-dx-game-controls-deadzone.png)

此 **UpdatePollingDevices** 方法處理左和右搖桿。
每個搖桿的 X 和 Y 值會檢查它們是否在靜止區域以外。 如果其中一個或兩個是，我們會更新對應的元件。
例如，如果左搖桿沿 X 軸向左移動，我們將會將 -1 加入 **x** 元件的 **m_moveCommand** 向量。 這個向量是將用於彙總在所有裝置的所有移動，以及稍後用來計算玩家應該移動到哪個位置。 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

類似左搖桿的控制項移動，右搖桿會控制相機的旋轉。


右搖桿行為會配合滑鼠和鍵盤控制項設定中滑鼠移動的行為。
如果搖桿在靜止區域以外，我們會計算目前指標位置和使用者現在將嘗試的視角位置之間的差異。
指標位置 (**pointerDelta**) 中的這個變更接下來會用於更新相機旋轉的上下移動和左右偏移，其稍後會在 **Update** 方法中套用。
**pointerDelta** 向量可能會看起來熟悉，因為它也可以用在 [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) 方法，來追蹤滑鼠指標和觸控輸入的指標位置的變更。


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

遊戲的控制項沒有射擊球體的功能，可能無法完成！


此 **UpdatePollingDevices** 方法也會檢查是否按下 RT 鍵。 若是，我們的 **m_firePressed** 屬性會轉換為 true，發出訊號給其球體應開始射擊的遊戲。
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>Update 方法

若要總結，讓我們再更深入 [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 方法。
這個方法合併玩家使用任何支援的輸入而進行的移動或旋轉，以產生速度向量並更新上下移動和左右偏移值，用於存取遊戲迴圈。


**Update** 方法會呼叫 [**UpdatePollingDevices**](#the-updatepollingdevices-method) 來更新控制器的狀態以開始進行。 這個方法也會收集遊戲台的任何輸入，並將其移動新增到 **m_moveCommand** 向量。 

在我們的 **Update** 方法中，我們接著執行下列輸入檢查。
- 如果玩家使用移動控制器矩形，則我們會判斷指標位置中的變更，然後使用其來計算使用者是否已經將指標退出控制器的靜止區域。 如果在靜止區域以外，**m_moveCommand** 向量屬性則會以虛擬搖桿值更新。
- 如果按下任何移動鍵盤輸入，**m_moveCommand** 向量的對應元件中會新增 `1.0f` 或 `-1.0f` 的值 &mdash; `1.0f` 為向前，`-1.0f` 為向後。


一旦所有移動輸入都納入考慮，則我們會透過一些計算執行 **m_moveCommand** 向量，以產生表示玩家在遊戲世界的方向的新向量。
然後我們在相關世界移動，然後套用到玩家做為該方向的速度。
最後，我們重設 **m_moveCommand** 向量為 `(0.0f, 0.0f, 0.0f)`，這樣的所有項目就能就緒準備下一個遊戲框架。


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>後續步驟

現在，我們新增了我們的控制項，還有一項功能我們需要新增，以建立身歷其境的遊戲：音效！
音樂和音效對所有遊戲都非常重要，讓我們接著討論[加入聲音](tutorial--adding-sound.md)。
 

 

 




