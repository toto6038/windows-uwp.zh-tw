---
title: 新增使用者介面
description: 了解如何將 DirectX UWP 遊戲的 2D 的使用者介面重疊。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 使用者介面, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609053"
---
# <a name="add-a-user-interface"></a>新增使用者介面


現在，我們的遊戲已就地具有其 3D 視覺效果，就可以專注於新增一些 2D 的項目，讓遊戲可以提供給玩家的遊戲狀態相關的意見反應。 這可藉由加入簡單的功能表選項，並在 3d 圖形之上的抬頭顯示元件管線輸出。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

使用 Direct2D，將許多使用者介面圖形和行為新增至我們 UWP DirectX 遊戲包括：
- 抬頭顯示，包括[移動外觀控制器](tutorial--adding-controls.md)界限的矩形
- 遊戲狀態功能表


## <a name="the-user-interface-overlay"></a>使用者介面重疊


雖然有許多方式來顯示文字和使用者介面項目中的 DirectX 遊戲，我們把重點放在使用[Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx)。 我們也會使用[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)文字項目。


Direct2D 是一組的 2D 繪圖 Api 用來繪製像素為基礎的基本型別和效果。 當您開始使用 Direct2D，最好是為了簡單起見。 複雜的配置和介面行為會花費相當多的時間並需要長時間的規劃。 如果您的遊戲需要複雜的使用者介面，例如那些常見於模擬及策略遊戲，請考慮改為使用 XAML。

> [!NOTE]
> 如需開發 UWP DirectX 遊戲中的具有 XAML 使用者介面的詳細資訊，請參閱[擴充遊戲範例](tutorial-resources.md)。

Direct2D 不被專為使用者介面或例如 HTML 和 XAML 的版面配置。 它並不提供使用者介面元件，例如清單、 方塊或按鈕。 它也不提供版面配置的元件，例如 div、 資料表或方格。


此遊戲的範例中，我們有兩個主要的 UI 元件。
1. 分數 和 遊戲中控制項的抬頭顯示。
2. 用來顯示遊戲狀態文字和選項，例如暫停資訊的覆疊，層級啟動選項。

### <a name="using-direct2d-for-a-heads-up-display"></a>為平視顯示器使用 Direct2D

下圖顯示範例遊戲中抬頭顯示器。 它是簡單且整齊，讓播放器將焦點放在瀏覽 3D 世界和疑難排解的目標。 良好的介面或平視顯示窗必須永遠不會使處理，並在遊戲中的事件做出回應的播放程式的能力。

![遊戲重疊的螢幕擷取畫面](images/simple-dx-game-ui-overlay.png)

覆疊是由下列的基本項目所組成。
- [**DirectWrite** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)通知的播放程式右上角的文字 
    - 成功的點擊數
    - 數字，播放程式所做的擷取畫面
    - 層級中的剩餘時間
    - 目前的層級數目 
- 兩個交集用來形成十字型游標的直線線段
- 兩個矩形的右下角[移動外觀控制器](tutorial--adding-controls.md)界限。 


繪製覆疊的遊戲中抬頭顯示器狀態[ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)方法[ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)類別。 在此方法中，代表我們的 UI 在 Direct2D 重疊會更新以反映中叫用，時間其餘，和層級數目的數字的變更。

如果已初始化遊戲，我們將新增`TotalHits()`， `TotalShots()`，並`TimeRemaining()`要[ **swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l)緩衝區，以及指定列印的格式。 我們可以再繪製它使用[ **DrawText** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848)方法。 我們這麼做為目前的層級指標中，繪製空的數字，以顯示等 ➀，未完成的層級和填滿的數字等 ➊ 以顯示 已完成在特定層級。


下列程式碼將逐步引導**GameHud::Render**方法的程序 
- 建立點陣圖，使用[* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- 去除 UI 區域，到使用矩形[ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- 使用**DrawText**使文字項目

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );
        
        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

重大的方法，此外，這段下[ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)方法繪製我們移動並引發具有矩形[ **ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)，並使用兩個呼叫的十字形[ **ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)。

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

在  **GameHud::Render**方法我們儲存 遊戲 視窗中的邏輯大小`windowBounds`變數。 這會使用[ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41)方法**DeviceResources**類別。 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 取得在遊戲視窗的大小是不可或缺的 UI 程式設計。 稱為 Dip （裝置獨立像素為單位），其中定義 DIP 為 1/96 英吋為單位的度量單位提供的視窗大小。 Direct2D 會調整繪圖的單位，以實際的像素繪製時，這種方式使用 Windows 為 dots per inch (DPI) 設定。 同樣地，當您繪製文字使用[ **DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)，您指定 Dip，而不是點字型的大小。 DIP 以浮點數表示。

 

### <a name="displaying-game-state-info"></a>顯示遊戲狀態資訊

除了平視顯示窗，遊戲的範例有覆疊，代表六個遊戲的狀態。 所有狀態的都功能與播放器讀取的文字是大型黑色長方形基本類型。 因為它們不是作用在這些狀態，將不會繪製移動外觀控制器矩形和十字形狀。

使用建立重疊[ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h)類別，讓我們切換移出以配合遊戲狀態顯示的文字。

![狀態和覆疊的動作](images/simple-dx-game-ui-finaloverlay.png)

覆疊會分成兩個區段：**狀態**並**動作**。 **狀態**secton 進一步細分為**標題**並**主體**矩形。 **動作**區段只能有一個矩形。 每個矩形會有不同的用途。

-   `titleRectangle` 包含標題文字。
-   `bodyRectangle` 包含主體文字。
-   `actionRectangle` 包含通知的播放器採取特定動作的文字。

遊戲的六個可設定的狀態。 傳達使用遊戲狀態**狀態**重疊的部分。 **狀態**矩形會更新使用多個對應具有下列狀態的方法。

- 正在載入
- 初始的開始/高分數的統計資料
- 層級開始
- 遊戲已暫停
- 遊戲結束
- 獲勝的遊戲


**動作**重疊部分會使用更新[ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)方法，讓動作文字設為下列其中之一。
- 點選以利再次播放...」
- 「 層級載入，請稍候...」
- 「 點選以繼續...」
- 無

> [!NOTE]
> 這兩種方法將會討論中進一步[代表遊戲狀態](#representing-game-state)一節。

根據發生什麼情況在遊戲中，**狀態**並**動作**區段的文字欄位會進行調整。
讓我們看看我們如何初始化及繪製這些六個狀態的覆疊。

### <a name="initializing-and-drawing-the-overlay"></a>初始化並繪製重疊

六個**狀態**狀態有共通的一些事項，讓資源和方法就必須透過非常類似。
    - 它們都會使用一個黑色長方形螢幕的中心做為其背景。
    - 顯示的文字形式是否**標題**或是**主體**文字。
    - 文字使用 Segoe UI 字型，而繪製後在矩形的頂端。 


此遊戲的範例有派上用場時建立覆疊的四種方法。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78)建構函式初始化覆疊，維護上顯示資訊給播放器，我們將使用的點陣圖介面。 建構函式會取得從原廠[ **ID2D1Device** ](https://msdn.microsoft.com/library/windows/desktop/hh404478)物件傳遞給它，用來建立[ **ID2D1DeviceContext** ](https://msdn.microsoft.com/library/windows/desktop/hh404479)若要可以繪製覆疊物件本身。 [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)是將用來繪製文字的筆刷建立我們的方法。 若要這樣做，我們會取得[ **ID2D1DeviceContext2** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789)物件可讓您建立和繪製幾何，再加上功能，例如筆跡和漸層網格轉譯。 接著我們建立一系列使用彩色的筆刷[ **ID2D1SolidColorBrush** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207)繪製下列 UI 項目。
- 黑色矩形背景的筆刷
- 狀態文字的白色筆刷
- 橘色動作文字的筆刷

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
[ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)方法設定視窗的 dpi。 DPI 已變更，且必須是時，會呼叫這個方法重新調整 恰好在遊戲視窗調整大小時。 在更新之後的 DPI，這個方法也會呼叫[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487)來進行確認必要的資源會重新建立每次調整視窗大小。


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
[ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225)方法是我們的繪圖發生的地方。 以下是方法的步驟的概述。
- 關閉的 UI 文字區段來建立三個矩形**標題**，**主體**，並**動作**文字。
    ```cpp 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- 點陣圖建立具名`m_levelBitmap`，納入帳戶使用的目前 DPI **CreateBitmap**。
- `m_levelBitmap` 已設定為使用您建立我們 2D 轉譯目標[ **ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)。
- 點陣圖已清除以進行黑使用每個像素[ **ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772)。
- [**ID2D1RenderTarget::BeginDraw** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768)呼叫以初始化繪圖。 
- **DrawText**繪製文字儲存在稱為`m_titleString`， `m_bodyString`，和`m_actionString`中使用對應的 approperiate 矩形**ID2D1SolidColorBrush**。
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw)呼叫來停止所有繪圖作業上`m_levelBitmap`。
- 使用建立另一個點陣圖**CreateBitmap**名為`m_tooSmallBitmap`來做為後援，僅顯示顯示組態是否太小遊戲。
- 重複程序所繪製`m_levelBitmap`for `m_tooSmallBitmap`，此時只繪製字串`Paused`主體中。




現在我們只需要六種方法來填入我們的六個重疊狀態的文字 ！

### <a name="representing-game-state"></a>代表遊戲狀態


每個遊戲中的六個重疊狀態有對應的方法**GameInfoOverlay**物件。 這些方法會繪製各種重疊，玩家可從此了解遊戲的明確資訊。 此通訊以表示**標題**並**主體**字串。 因為此範例已設定的資源和此項資訊的版面配置時已將其初始化，並使用[ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)方法，它只需要提供重疊的特定狀態的字串。

**狀態**重疊部分已設定其中一種下列方法呼叫。

遊戲狀態 | 狀態設定方法 | 狀態欄位
:----- | :------- | :---------
正在載入 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**標題**</br>正在載入資源 </br>**Body**</br> 以累加方式列印 」。 「 暗示載入活動。
初始的開始/高分數的統計資料 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**標題**</br>高的分數</br> **Body**</br> 層級完成 # </br>總計點 #</br>總計的各個畫面 #
層級開始 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**標題**</br>層級的 #</br>**Body**</br>層級目標的描述。
遊戲已暫停 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**標題**</br>遊戲已暫停</br>**Body**</br>無
遊戲結束 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**標題**</br>遊戲結束</br> **Body**</br> 層級完成 # </br>總計點 #</br>總計的各個畫面 #</br>層級完成 #</br>高評分 #
獲勝的遊戲 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**標題**</br>您贏了 ！</br> **Body**</br> 層級完成 # </br>總計點 #</br>總計的各個畫面 #</br>層級完成 #</br>高評分 #




具有[ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134)方法，此範例會宣告三個對應至覆疊的特定區域的矩形區域。



記住這些區域的用途，讓我們看看其中一個狀態特定方法 (**GameInfoOverlay::SetGameStats**)，並了解重疊是如何繪製的。

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

使用 Direct2D 的裝置內容的**GameInfoOverlay**初始化的物件，這個方法會填滿的標題和內文的矩形與使用的背景筆刷的黑色。 它會使用白色文字筆刷，在標題矩形中繪製 "High Score" 文字字串，並在內文矩形中繪製包含更新遊戲狀態資訊的字串。


後續呼叫會更新動作矩形[ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)上的方法從**GameMain**物件，提供所需的遊戲狀態資訊藉由**GameInfoOverlay::SetAction**來判斷正確的訊息給播放器，例如 「 點選以繼續 」。

在選擇指定的任何狀態的覆疊[ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661)方法如下：

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

現在遊戲的文字資訊傳達給播放器根據遊戲狀態，我們就可以切換顯示的內容，整個遊戲。

### <a name="next-steps"></a>後續步驟

在下一個主題[新增控制項](tutorial--adding-controls.md)中，我們會說明玩家如何與遊戲範例互動，以及輸入如何變更遊戲狀態。



 




