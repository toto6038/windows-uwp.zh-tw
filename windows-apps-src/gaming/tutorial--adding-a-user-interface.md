---
author: abbycar
title: 新增使用者介面
description: 了解如何將 2D 使用者介面重疊新增至 DirectX UWP 遊戲。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.author: abigailc
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 使用者介面, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9962cc9043bd650390721715ca73b2e85a219c25
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7571177"
---
# <a name="add-a-user-interface"></a>新增使用者介面


現在，我們的遊戲有現成的其 3D 視覺效果，是時候將焦點放在新增一些 2D 元素，讓遊戲可以將遊戲狀態的相關意見反應提供給玩家。 這可以透過新增簡易選單選項以及平視顯示器元件上方的 3d 圖形管線輸出。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

使用 Direct2D，將數個使用者介面圖形和行為新增到我們 UWP DirectX 遊戲包括：
- 平視顯示器，包括[移動視角控制器](tutorial--adding-controls.md)界限矩形
- 遊戲狀態功能表


## <a name="the-user-interface-overlay"></a>使用者介面重疊


雖然有許多方式可以在 DirectX 遊戲中顯示文字和使用者介面元素，我們將焦點上使用[Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx)。 我們也會使用[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)文字元素。


Direct2D 是一組 2D 繪圖 Api 可用來繪製像素為基礎的基本類型及效果。 當您開始使用 Direct2D，最好是讓事情保持簡單。 複雜的配置和介面行為會花費相當多的時間並需要長時間的規劃。 如果您的遊戲需要複雜的使用者介面，例如模擬和戰略遊戲中，請考慮改為使用 XAML。

> [!NOTE]
> 開發 UWP DirectX 遊戲中的使用 XAML 使用者介面的詳細資訊，請參閱[延伸遊戲範例](tutorial-resources.md)。

Direct2D 不被專為使用者介面或像 HTML 和 XAML 的版面配置。 它不會提供使用者介面元件，例如清單、 方塊或按鈕。 它也不會提供像是 table、 或格線配置元件。


針對此遊戲範例中，我們有兩個主要 UI 元件。
1. 平視顯示器，分數和遊戲內控制項。
2. 覆疊用來顯示遊戲狀態文字和選項，例如暫停資訊及關卡開始選項。

### <a name="using-direct2d-for-a-heads-up-display"></a>為平視顯示器使用 Direct2D

下圖顯示遊戲內平視顯示器範例。 它是簡單、 不凌亂，允許玩家專注在瀏覽 3D 世界和射擊目標。 好的介面或平視顯示器必須永遠不會讓更加複雜的玩家處理和應對遊戲事件的能力。

![遊戲重疊的螢幕擷取畫面](images/simple-dx-game-ui-overlay.png)

覆疊是由下列基本類型所組成。
- [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)通知玩家的右上角的文字 
    - 成功的點擊數
    - 擷取畫面已進行玩家數目
    - 層級的剩餘時間
    - 目前的層級數目 
- 兩個交集線段用來形成跨釘
- [移動視角控制器](tutorial--adding-controls.md)界限的底部角落的兩個矩形。 


[**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)類別的[**Gamehud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)方法中繪製重疊的遊戲內平視顯示器狀態。 此方法，請在 Direct2D 重疊，表示我們的 UI 會更新以反映中的時間其餘，和層級數、 點擊數的變更。

如果已初始化遊戲，我們將新增`TotalHits()`， `TotalShots()`，以及`TimeRemaining()`來[**swprintf_s**](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l)緩衝區，並指定列印格式。 我們接著繪製[**DrawText**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848)方法。 我們這樣做的相同目前的層級指示器，繪製空的數字顯示未完成的層級，例如 ➀ 和填滿的數字像 ➊ 來顯示特定層級已完成。


下列程式碼片段會逐步引導**Gamehud**方法的程序 
- 建立點陣圖 using [* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- 去除 UI 區域成使用[**D2D1::RectF**矩形](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- 使用**DrawText**讓文字元素

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

中斷方法向下向進一步，這段[**Gamehud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)方法繪製我們的移動和射擊矩形[**ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)，與使用兩個呼叫[**ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)交叉線。

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

**Gamehud**方法中，我們儲存遊戲的視窗中的邏輯大小`windowBounds`變數。 這會使用[`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources**類別的方法。 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 取得遊戲視窗的大小是不可或缺的 UI 程式設計。 視窗的大小會獲得 dip （裝置獨立像素），其中 DIP 定義為 1/96 英吋度量單位。 Direct2D 會繪圖單位縮放為實際像素繪圖發生時，如此一來使用 Windows 每英吋點數 (DPI) 設定。 同樣地，當您繪製文字時，使用[**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)，指定 Dip，而不是點大小的字型。 DIP 以浮點數表示。

 

### <a name="displaying-game-state-info"></a>顯示遊戲狀態的資訊

除了平視顯示器，遊戲範例還有一個重疊，代表六個遊戲狀態。 所有狀態的都功能讓玩家閱讀的文字與大型黑色矩形基本類型。 因為它們不是使用這些狀態中，將不會繪製移動視角控制器矩形和交叉線。

使用[**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h)類別，讓我們查看何種文字會顯示，以配合遊戲的狀態切換建立覆疊。

![狀態和動作的重疊](images/simple-dx-game-ui-finaloverlay.png)

覆疊分成兩個區段：**狀態**和**動作**。 **狀態**secton 進一步細分成**標題**和**內文**矩形。 [**動作**] 區段只會有一個矩形。 每個矩形會有不同的用途。

-   `titleRectangle` 包含的標題文字。
-   `bodyRectangle` 包含內文文字。
-   `actionRectangle` 包含通知玩家採取特定動作的文字。

遊戲有六個可設定的狀態。 使用重疊的**狀態**部分來傳達遊戲的狀態。 **狀態**矩形會更新使用的一些下列狀態與對應的方法。

- 正在載入
- 初始開始/高分數的統計資料
- 關卡開始
- 遊戲暫停
- 遊戲結束
- 贏得遊戲


重疊的**動作**部分會使用[**Directxapp**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)的方法，讓文字設定為下列其中一項動作進行更新。
- 「 點選以再次玩遊戲...」
- 「 層級載入，請稍候...」
- 「 點選以繼續...」
- 無

> [!NOTE]
> 這兩種方法將會討論進一步[代表遊戲狀態](#representing-game-state)] 區段中。

根據什麼遊戲、**狀態**和**動作**] 區段中事被調整文字欄位。
讓我們看看如何初始化和繪製這些六個狀態的重疊。

### <a name="initializing-and-drawing-the-overlay"></a>初始化並繪製重疊

**六個狀態**有一些共同點，如此一來資源，方法他們需要非常類似。
    - 在螢幕中央使用黑色矩形時，它們都做為其背景。
    - 顯示的文字是**標題**或**內文**文字。
    - 文字使用 Segoe UI 字型，且繪製在黑色矩形的上方。 


遊戲範例有四個方法建立覆疊時隨附能播放。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[**GameInfoOverlay::GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78)建構函式會初始化重疊，維護點陣圖表面，我們將使用上顯示給玩家的資訊。 建構函式會從傳遞給它，它會使用來建立重疊物件自己可以繪製到[**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478)物件取得 factory。 [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>Gameinfooverlay:: Createdevicedependentresources
[**Gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)是我們建立將用來繪製我們的文字的筆刷的方法。 若要這樣做，我們會取得可建立一個[**ID2D1DeviceContext2**](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789)物件，並繪製的幾何，再加上功能，例如筆跡和漸層網格轉譯。 然後，我們會建立一系列的彩色的筆刷來繪製下列 UI 元素使用[**ID2D1SolidColorBrush**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) 。
- 矩形背景的黑色筆刷
- 白色筆刷的狀態文字
- 動作的文字橙色筆刷

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
[**DeviceResources::SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)方法設定視窗的 dpi。 DPI 已變更，且必須是時呼叫這個方法，取得遊戲視窗調整大小時，會發生這事進行調整。 更新之後 DPI，這個方法也會呼叫[**deviceresources:: Createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487)以確定每次調整視窗大小會重新建立所需的資源。


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
[**GameInfoOverlay::CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225)方法就是我們的所有繪製都發生的位置。 以下是方法的步驟的概略說明。
- 關閉**標題**、**內文**，以及**控制**文字的 UI 文字的區段會建立三個矩形。
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

- 點陣圖會建立名為`m_levelBitmap`，目前的 DPI 列入使用**CreateBitmap**的帳戶。
- `m_levelBitmap` 我們的 2D 轉譯目標使用[**ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)設定。
- 點陣圖已清除所做的每個像素的黑色使用[**ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772)。
- [**ID2D1RenderTarget::BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768)呼叫來起始繪圖。 
- **DrawText**呼叫來繪製文字儲存在`m_titleString`， `m_bodyString`，並`m_actionString`中使用相對應的**ID2D1SolidColorBrush**描繪矩形。
- [**ID2D1RenderTarget::EndDraw**](ID2D1RenderTarget::EndDraw)稱為停止上的所有繪製作業`m_levelBitmap`。
- 另一個點陣圖會建立使用**CreateBitmap**名為`m_tooSmallBitmap`做為後援，只顯示的顯示設定是否太小，該遊戲中使用。
- 重複上繪製的程序`m_levelBitmap`的`m_tooSmallBitmap`，只繪製字串這次`Paused`本文中。




現在我們需要所有都是六種方法，以填滿我們的六個重疊狀態的文字 ！

### <a name="representing-game-state"></a>代表遊戲狀態


六個重疊狀態在遊戲中的每一個都有對應的方法**GameInfoOverlay**物件中。 這些方法會繪製各種重疊，玩家可從此了解遊戲的明確資訊。 此通訊的**標題**和**內文**的字串表示。 因為範例已經設定的資源和配置的這項資訊時，它會初始化和[**gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)方法，它只需要提供重疊狀態的特定的字串。

重疊的**狀態**部分會呼叫其中一個下列方法來設定。

遊戲狀態 | 狀態設定方法 | 狀態欄位
:----- | :------- | :---------
正在載入 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>載入資源 </br>**Body**</br> 以遞增方式列印 」。 」 暗示載入活動。
初始開始/高分數的統計資料 | [Gameinfooverlay:: Setgamestats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>高分數</br> **Body**</br> 層級完成 # </br>總計點數 #</br>總流行 #
關卡開始 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br>層級 #</br>**Body**</br>層級的客觀描述。
遊戲暫停 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>遊戲暫停</br>**Body**</br>無
遊戲結束 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>遊戲結束</br> **Body**</br> 層級完成 # </br>總計點數 #</br>總流行 #</br>層級完成 #</br>高分數 #
贏得遊戲 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>您已贏得遊戲 ！</br> **Body**</br> 層級完成 # </br>總計點數 #</br>總流行 #</br>層級完成 #</br>高分數 #




使用[**GameInfoOverlay::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134)方法，範例會宣告分別對應到重疊的特定區域的三個矩形區域。



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

使用**GameInfoOverlay**物件初始化的 Direct2D 裝置內容，這個方法會填入標題和內文矩形以黑色使用背景筆刷。 它會使用白色文字筆刷，在標題矩形中繪製 "High Score" 文字字串，並在內文矩形中繪製包含更新遊戲狀態資訊的字串。


動作矩形會更新為**GameMain**物件，提供**Directxapp**判斷正確的訊息，以所需的遊戲狀態資訊的方法[**Directxapp**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)的後續呼叫玩家，例如 「 點選以繼續 」。

針對任何指定的狀態，重疊會選擇[**GameMain::SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661)方法，就像這樣：

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

遊戲現在已可根據遊戲的狀態，玩家文字資訊的方式，我們有一種切換顯示的內容給他們整個遊戲的方式。

### <a name="next-steps"></a>後續步驟

在下一個主題[新增控制項](tutorial--adding-controls.md)中，我們會說明玩家如何與遊戲範例互動，以及輸入如何變更遊戲狀態。



 




