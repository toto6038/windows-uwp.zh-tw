---
title: 新增使用者介面
description: 瞭解如何將2D 使用者介面重迭新增至 DirectX UWP 遊戲。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 使用者介面, directx
ms.localizationpriority: medium
ms.openlocfilehash: b6b59bd4f42d31e1f29cc1af298199b42cce6781
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409537"
---
# <a name="add-a-user-interface"></a>新增使用者介面

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

既然我們的遊戲已經有3D 視覺效果，就可以把重點放在新增一些2D 元素，讓遊戲能夠提供有關遊戲狀態的意見反應給播放程式。 這可以藉由在3D 圖形管線輸出之上加入簡單的功能表選項和標題顯示元件來完成。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至[Direct3D 範例遊戲](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

使用 Direct2D，將一些使用者介面圖形和行為新增至 UWP DirectX 遊戲，包括：
- 列印頭顯示，包括[移動外觀控制器](tutorial--adding-controls.md)boundry 矩形
- 遊戲狀態功能表


## <a name="the-user-interface-overlay"></a>使用者介面重疊


雖然有許多方法可以在 DirectX 遊戲中顯示文字和使用者介面元素，但我們將著重在使用[Direct2D](/windows/desktop/Direct2D/direct2d-portal)。 我們也會針對文字元素使用[DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) 。


Direct2D 是一組2D 繪圖 Api，用來繪製以圖元為基礎的基本和效果。 當您開始使用 Direct2D 時，最好能夠輕鬆地進行一些工作。 複雜的配置和介面行為會花費相當多的時間並需要長時間的規劃。 如果您的遊戲需要複雜的使用者介面，例如在模擬和策略遊戲中找到的介面，請考慮改為使用 XAML。

> [!NOTE]
> 如需在 UWP DirectX 遊戲中使用 XAML 開發使用者介面的詳細資訊，請參閱[擴充範例遊戲](tutorial-resources.md)。

Direct2D 不是特別針對使用者介面或 HTML 和 XAML 之類的版面配置所設計。 它不會提供如清單、方塊或按鈕等使用者介面元件。 它也不會提供 div、資料表或格線等版面配置元件。


針對此範例遊戲，我們有兩個主要的 UI 元件。
1. 分數和遊戲控制項的列印頭顯示。
2. 用來顯示遊戲狀態文字和選項的重迭，例如 [暫停資訊] 和 [層級啟動] 選項。

### <a name="using-direct2d-for-a-heads-up-display"></a>為平視顯示器使用 Direct2D

下圖顯示此範例的遊戲內標題顯示。 它既簡單又整齊，可以讓播放程式專注于流覽3D 世界和疑難排解目標。 良好的介面或列印頭顯示，絕對不能讓播放程式在遊戲中處理和回應事件的能力變得複雜。

![遊戲重疊的螢幕擷取畫面](images/simple-dx-game-ui-overlay.png)

重迭包含下列基本基本型別。
- [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)右上角的文字，通知玩家 
    - 成功的點擊
    - 播放程式已製作的照片數
    - 層級的剩餘時間
    - 目前的層級數目 
- 用來形成交叉線的兩條相交線段
- [[移動外觀控制器](tutorial--adding-controls.md)] 界限右下角的兩個矩形。 


重迭的遊戲內標題顯示狀態會在[**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)類別的[**GameHud：： Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)方法中繪製。 在此方法中，代表 UI 的 Direct2D 重迭會更新，以反映點擊次數、剩餘時間和層級編號的變更。

如果已初始化遊戲，我們會將 `TotalHits()` 、 `TotalShots()` 和新增 `TimeRemaining()` 至[**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l)緩衝區，並指定列印格式。 然後，我們可以使用[**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext)方法來繪製它。 我們會對目前的層級指標執行相同的動作、繪製空的數位來顯示未完成的層級（例如➀），以及➊之類的填滿數位，以顯示特定層級已完成。


下列程式碼片段會逐步解說適用于的**GameHud：： Render**方法的進程 
- 使用[* * ID2D1RenderTarget：:D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))建立點陣圖
- 使用[ **D2D1：： RECTF**將 UI 區域分割成矩形](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- 使用**DrawText**製作文字元素

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
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
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
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
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

將方法進一步細分之後， [**GameHud：： Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)方法的這個部分會以[**ID2D1RenderTarget：:D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))繪製移動和引發矩形，並使用[**ID2D1RenderTarget：:D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline)的兩個呼叫來交叉分析。

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

在**GameHud：： Render**方法中，我們會在變數中儲存遊戲視窗的邏輯大小 `windowBounds` 。 這會使用 [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources**類別的方法。 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

取得遊戲視窗的大小對於 UI 程式設計而言是不可或缺的。 視窗的大小會以稱為 Dip （裝置獨立圖元）的度量提供，其中 DIP 的定義為1/96。 當繪製發生時，Direct2D 會將繪圖單位調整為實際圖元，使用 [每英寸的 Windows 點（DPI）] 設定來執行此動作。 同樣地，當您使用[**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)繪製文字時，可以針對字型的大小指定 dip 而不是點。 DIP 以浮點數表示。 

### <a name="displaying-game-state-info"></a>顯示遊戲狀態資訊

除了列印頭顯示外，範例遊戲還有一個代表六個遊戲狀態的重迭。 所有狀態的功能都是具有文字的大型黑色矩形基本類型，可供播放程式讀取。 不會繪製「移動外觀控制器」矩形和十字線，因為它們在這些狀態中不是作用中。

重迭是使用[**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h)類別建立的，可讓我們切換出顯示的文字，以配合遊戲的狀態。

![重迭的狀態和動作](images/simple-dx-game-ui-finaloverlay.png)

重迭分為兩個區段： [**狀態**] 和 [**動作**]。 **狀態**secton 會進一步細分為 [**標題** **] 和 [** 內文] 矩形。 [**動作**] 區段只有一個矩形。 每個矩形都有不同的用途。

-   `titleRectangle`包含標題文字。
-   `bodyRectangle`包含本文文字。
-   `actionRectangle`包含通知播放程式採取特定動作的文字。

遊戲有六種可設定的狀態。 遊戲的狀態是使用重迭的**狀態**部分來傳達。 **狀態**矩形會使用與下列狀態對應的幾個方法來更新。

- 載入
- 初始開始/高分數統計資料
- 層級開始
- 遊戲已暫停
- 遊戲結束
- 遊戲獲勝


重迭的**動作**部分會使用[**GameInfoOverlay：： SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)方法進行更新，允許將動作文字設定為下列其中一項。
- 「再次播放 ...」
- 「層級載入，請稍候 ...」
- 「按一下即可繼續 ...」
- None

> [!NOTE]
> 這兩種方法都會在[代表遊戲狀態](#representing-game-state)一節中進一步討論。

視遊戲中的狀況而定，[**狀態**] 和 [**動作**] 區段的文字欄位會進行調整。
我們來看一下如何初始化和繪製這六種狀態的重迭。

### <a name="initializing-and-drawing-the-overlay"></a>初始化並繪製重疊

六個**狀態**狀態有一些常見的專案，因此所需的資源和方法非常類似。
    - 它們全都會在螢幕的中央使用黑色矩形作為其背景。
    - 顯示的文字為 [**標題**]**或 [** 內文文字]。
    - 文字會使用 Segoe UI 字型，並繪製在後端矩形的上方。 


範例遊戲有四種方法，可在建立重迭時進入播放。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[**GameInfoOverlay：： GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78)函式會初始化重迭，並維護我們將用來向播放程式顯示資訊的點陣圖介面。 此函式會從傳遞給它的[**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device)物件取得 factory，它會用它來建立覆迭物件本身可以繪製的[**ID2D1DeviceCoNtext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) 。 [IDWriteFactory::CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay：： CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)是我們用來建立將用來繪製文字之筆刷的方法。 若要這樣做，我們會取得[**ID2D1DeviceCoNtext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2)物件，它可以建立和繪製幾何，加上筆墨和漸層網格轉譯等功能。 接著，我們會使用[**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush)來建立一系列的彩色筆刷，以繪製 folling UI 元素。
- 矩形背景的黑色筆刷
- 狀態文字的白色筆刷
- 動作文字的橙色筆刷

#### <a name="deviceresourcessetdpi"></a>DeviceResources：： SetDpi

[**DeviceResources：： SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)方法會設定視窗的每英寸點。 當 DPI 變更時，會呼叫這個方法，而且需要進行進行調整，這會在遊戲視窗調整大小時發生。 更新 DPI 之後，這個方法也會呼叫[**DeviceResources：： CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) ，以確保每次調整視窗大小時都會重新建立所需的資源。

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources

[**GameInfoOverlay：： CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225)方法是我們的所有繪圖發生的地方。 以下是方法步驟的大綱。
- 針對**標題**、內文和**動作****文字，會**建立三個矩形來區段外的 UI 文字。
    ```cppwinrt 
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

- 建立名為的點陣圖 `m_levelBitmap` ，並使用**CreateBitmap**將目前的 DPI 納入考慮。
- `m_levelBitmap`設定為使用[**ID2D1DeviceCoNtext：： SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget)的2d 轉譯目標。
- 點陣圖會使用[**ID2D1RenderTarget：： Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear)，清除每個圖元變為黑色。
- 呼叫[**ID2D1RenderTarget：： BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw)以起始繪圖。 
- 呼叫**DrawText**時，會 `m_titleString` `m_bodyString` `m_actionString` 使用對應的**ID2D1SolidColorBrush**，在 approperiate 矩形中繪製儲存在、和中的文字。
- 呼叫[**ID2D1RenderTarget：： EndDraw**](ID2D1RenderTarget::EndDraw) ，以停止上的所有繪製作業 `m_levelBitmap` 。
- 另一個點陣圖是使用名為的**CreateBitmap** `m_tooSmallBitmap` 來建立，以做為回復使用，只會顯示顯示設定對遊戲而言太小。
- 重複處理在上繪製的 `m_levelBitmap` 程式 `m_tooSmallBitmap` ，這次只會 `Paused` 在主體中繪製字串。




現在，我們只需要六種方法來填入六個重迭狀態的文字！

### <a name="representing-game-state"></a>代表遊戲狀態


遊戲中的六個重迭狀態，在**GameInfoOverlay**物件中有一個對應的方法。 這些方法會繪製各種重疊，玩家可從此了解遊戲的明確資訊。 這種通訊會以**標題**和**主體**字串來表示。 由於範例已在初始化和[**GameInfoOverlay：： CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)方法時，設定此資訊的資源和配置，因此它只需要提供重迭狀態特定的字串。

重迭的**狀態**部分是使用下列其中一個方法的呼叫來設定。

遊戲狀態 | 狀態設定方法 | 狀態欄位
:----- | :------- | :---------
載入 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title** (標題)</br>正在載入資源 </br>**本文**</br> 以累加方式列印 "."，表示正在載入活動。
初始開始/高分數統計資料 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title** (標題)</br>高分</br> **本文**</br> 完成的層級# </br>總點數#</br>總快照數#
層級開始 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title** (標題)</br>二級#</br>**本文**</br>層級目標描述。
遊戲已暫停 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title** (標題)</br>遊戲已暫停</br>**本文**</br>None
遊戲結束 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title** (標題)</br>遊戲結束</br> **本文**</br> 完成的層級# </br>總點數#</br>總快照數#</br>完成的層級#</br>高分#
遊戲獲勝 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title** (標題)</br>你贏了！</br> **本文**</br> 完成的層級# </br>總點數#</br>總快照數#</br>完成的層級#</br>高分#

透過[**GameInfoOverlay：： CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134)方法，範例會宣告三個對應至重迭特定區域的矩形區域。

記住這些區域的用途，讓我們看看其中一個狀態特定方法 (**GameInfoOverlay::SetGameStats**)，並了解重疊是如何繪製的。

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

使用**GameInfoOverlay**物件初始化的 Direct2D 裝置內容，這個方法會使用背景筆刷，以黑色填滿標題和主體矩形。 它會使用白色文字筆刷，在標題矩形中繪製 "High Score" 文字字串，並在內文矩形中繪製包含更新遊戲狀態資訊的字串。


動作矩形是由**GameMain**物件上的方法的後續呼叫[**GameInfoOverlay：： SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)所更新，它會提供**GameInfoOverlay：： SetAction**所需的遊戲狀態資訊，以判斷播放程式的正確訊息，例如「點按以繼續」。

在[**GameMain：： SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661)方法中，會選擇任何指定狀態的覆迭，如下所示：

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
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

現在遊戲可以根據遊戲狀態，將文字資訊傳達給播放程式，而我們也有辦法切換在遊戲中顯示的內容。

### <a name="next-steps"></a>後續步驟

在下一個主題中，[新增控制項](tutorial--adding-controls.md)，我們將探討播放玩家與範例遊戲的互動方式，以及輸入如何改變遊戲的狀態。
