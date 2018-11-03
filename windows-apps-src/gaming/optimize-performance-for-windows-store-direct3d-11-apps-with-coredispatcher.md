---
author: mtoepke
title: 最佳化 UWP DirectX 遊戲的輸入延遲
description: 輸入延遲可能大幅影響遊戲的體驗，因此，最佳化輸入延遲可以使遊戲的感覺更完美。
ms.assetid: e18cd1a8-860f-95fb-098d-29bf424de0c0
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, directx, input latency, 遊戲, 輸入延遲
ms.localizationpriority: medium
ms.openlocfilehash: a2e92dc10dbcdc3a511c1b1a1271ae759cc03c60
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5989125"
---
#  <a name="optimize-input-latency-for-universal-windows-platform-uwp-directx-games"></a>最佳化通用 Windows 平台 (UWP) DirectX 遊戲的輸入延遲



輸入延遲可能大幅影響遊戲的體驗，因此，最佳化輸入延遲可以使遊戲的感覺更完美。 此外，適當的輸入事件最佳化可以增加電池使用時間。 了解如何選擇正確的 CoreDispatcher 輸入事件處理選項，可確保您的遊戲可以盡可能順暢地處理輸入。

## <a name="input-latency"></a>輸入延遲


輸入延遲是指系統回應使用者輸入所需的時間。 此回應通常是畫面上顯示內容的變更，或透過音訊回饋聽到的內容。

每個輸入事件 (無論是來自觸控指標、滑鼠指標或鍵盤) 都會產生一個事件處理常式需要處理的訊息。 現代的觸控數位板和遊戲周邊裝置以每個指標最低 100 Hz 的速度回報輸入事件，這表示應用程式每秒至少可以接收每個指標 (或按鍵輸入) 的 100 個事件。 如果多個指標同時發生，或使用精確度更高的輸入裝置 (例如，遊戲用滑鼠)，則會增加這個更新率。 事件訊息佇列可能會非常快速地填滿。

您務必了解遊戲的輸入延遲需求，讓事件可以透過最適合該情況的方式處理。 沒有一個解決方案適用於所有遊戲。

## <a name="power-efficiency"></a>電源效率


在輸入延遲的內容中，「電源效率」指的是某個遊戲使用多少 GPU。 使用 GPU 資源越少的遊戲，其電源效率越好，而且電池使用時間越長。 對於 CPU 而言也是如此。

如果某個遊戲可以用每秒少於 60 個畫面格 (目前在大多數顯示器上的最大轉譯速度) 繪製整個場景，而且不會降低使用者的體驗品質，繪製較少的場景通常會使電源更有效率。 有些遊戲更新畫面只為回應使用者輸入，因此，那些遊戲應該不會以每秒 60 個畫面格重複繪製相同的內容。

## <a name="choosing-what-to-optimize-for"></a>選擇要最佳化的內容


設計 DirectX 應用程式時，您需要做一些選擇。 應用程式需要每秒轉譯 60 個畫面格才能呈現順暢的動畫？或者只需要轉譯以回應輸入？ 應用程式需要有最低的輸入延遲？或者能夠容忍些許延遲？ 我的使用者是否期望我的應用程式對於電池使用有最妥善的利用？

這些問題的回答可能會將您的應用程式與下列其中一種案例對應：

1.  視需求轉譯。 此類別中的遊戲只需要更新畫面以回應特定類型的輸入。 因為應用程式不會重複轉譯相同的畫面格，所以電源效率良好；而因為應用程式大部分的時間是在等待輸入，所以輸入延遲很低。 棋盤遊戲與新聞閱讀程式可歸到此類別的應用程式範例。
2.  包含暫時動畫的視需求轉譯。 此案例與第一個案例類似，不同的是某些類型的輸入會啟動動畫，而不需仰賴使用者後續的輸入。 因為遊戲不會重複轉譯相同的畫面格，所以電源效率良好，而當遊戲未顯示動畫時，輸入延遲很低。 每次移動都顯示動畫的互動式兒童遊戲與棋盤遊戲可歸到此類別的應用程式範例。
3.  每秒轉譯 60 個畫面格。 在此案例中，遊戲會不斷更新畫面。 因為轉譯的畫面格數目達到顯示器可呈現的上限，所以電源效率不佳。 因為 DirectX 會在呈現內容時封鎖執行緒，所以輸入延遲很高。 這樣做可避免執行緒傳送的畫面格數目超過顯示器可向使用者顯示的數目。 第一人稱射擊的即時戰略遊戲與物理原理遊戲可歸到此類別的應用程式範例。
4.  每秒轉譯 60 個畫面格並達到最低的輸入延遲。 與案例 3 類似，app 會不斷更新畫面，因此電源效率將變差。 差別在於遊戲是在個別的執行緒上回應輸入，因此輸入處理不會因為呈現圖形到顯示器而被封鎖。 線上的多人遊戲、格鬥遊戲或律動/計時遊戲可歸到此類別，因為它們支援在極密集的事件時段內輸入操作。

## <a name="implementation"></a>實作


大部分的 DirectX 遊戲都是透過所謂的遊戲迴圈所驅動。 基本的演算法就是執行這些步驟，直到使用者結束遊戲或應用程式為止：

1.  處理輸入
2.  更新遊戲狀態
3.  繪製遊戲內容

當 DirectX 遊戲的內容經過轉譯，並準備好呈現到畫面上時，遊戲迴圈會等到 GPU 準備好接收新的畫面，然後才喚醒以再次處理輸入。

我們將會針對簡單的拼圖遊戲進行反覆處理，以便為稍早提到的每個案例顯示遊戲迴圈的實作。 與每個實作一起討論的決策點、優點以及取捨可以當做指南，協助您針對低延遲輸入以及電源效率最佳化您的 app。

## <a name="scenario-1-render-on-demand"></a>案例 1：視需求轉譯


拼圖遊戲的第一個反覆運算只會在使用者移動拼圖時更新畫面。 使用者可以選取一塊拼圖然後觸碰正確的目的地，將其拖曳到定位或貼齊定位。 在第二個情況下，這塊拼圖會在沒有動畫或效果的情況下跳到目的地。

此程式碼在使用 **CoreProcessEventsOption::ProcessOneAndAllPending** 的 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法內，有一個單一執行緒的遊戲迴圈。 使用這個選項會在佇列中分派所有目前可用的事件。 如果沒有任何事件擱置中，遊戲迴圈會等到有事件出現為止。

``` syntax
void App::Run()
{
    
    while (!m_windowClosed)
    {
        // Wait for system events or input from the user.
        // ProcessOneAndAllPending will block the thread until events appear and are processed.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

        // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
        // scene and present it to the display.
        if (m_updateWindow || m_state->StateChanged())
        {
            m_main->Render();
            m_deviceResources->Present();

            m_updateWindow = false;
            m_state->Validate();
        }
    }
}
```

## <a name="scenario-2-render-on-demand-with-transient-animations"></a>案例 2：包含暫時動畫的視需求轉譯


在第二個反覆運算中，遊戲經過修改，所以使用者選取一塊拼圖然後觸碰這塊拼圖的正確目的地時，拼圖會以動畫形式越過畫面，直到到達其目的地為止。

和之前一樣，此程式碼有一個使用 **ProcessOneAndAllPending** 的單一執行緒遊戲迴圈，以分派佇列中的輸入事件。 現在的差異是在動畫期間，迴圈改為使用 **CoreProcessEventsOption::ProcessAllIfPresent**，所以它不會等待新的輸入事件。 如果沒有任何事件擱置中，[**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 會立即傳回，並讓 app 呈現動畫中的下一個畫面。 當動畫完成時，迴圈會切換回 **ProcessOneAndAllPending** 以限制畫面更新。

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        // 2. Switch to a continuous rendering loop during the animation.
        if (m_state->Animating())
        {
            // Process any system events or input from the user that is currently queued.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // you are trying to present a smooth animation to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // Wait for system events or input from the user.
            // ProcessOneAndAllPending will block the thread until events appear and are processed.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

            // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
            // scene and present it to the display.
            if (m_updateWindow || m_state->StateChanged())
            {
                m_main->Render();
                m_deviceResources->Present();

                m_updateWindow = false;
                m_state->Validate();
            }
        }
    }
}
```

為支援 **ProcessOneAndAllPending** 和 **ProcessAllIfPresent** 之間的轉換，app 必須追蹤狀態以了解是否有動畫效果。 在拼圖應用程式中，您可以在 GameState 類別，透過加入可在遊戲迴圈期間呼叫的新方法來完成。 遊戲迴圈的動畫分支透過呼叫 GameState 的新 Update 方法更新動畫的狀態。

## <a name="scenario-3-render-60-frames-per-second"></a>案例 3：每秒轉譯 60 個畫面格


在第三個反覆運算中，應用程式顯示一個計時器，這個計時器會向使用者顯示他們已經花在處理拼圖上的時間。 由於它所顯示的經過時間以毫秒為單位，因此每秒必須轉譯 60 個畫面格，才能將顯示維持在最新狀態。

如同案例 1 和 2，應用程式有一個單一執行緒的遊戲迴圈。 與此案例的差異在於，由於應用程式一直在轉譯，所以不再需要和前兩個案例一樣追蹤遊戲狀態的變更。 因此，它可以預設為使用 **ProcessAllIfPresent** 處理事件。 如果沒有任何事件擱置中，**ProcessEvents** 會立即傳回，並繼續轉譯下一個畫面。

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            // 3. Continuously render frames and process system events and input as they appear in the queue.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // trying to present smooth animations to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // 3. If the window isn't visible, there is no need to continuously render.
            // Process events as they appear until the window becomes visible again.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

這是撰寫遊戲最簡單的方式，因為不需要追蹤其他狀態就可以判斷轉譯時機。 此方式可在計時器間隔達到最快的轉譯以及合理的輸入回應。

不過，這個簡單的開發方式要付出代價。 每秒轉譯 60 個畫面格所使用的電源比視需求轉譯還多。 當遊戲要變更每個畫面所顯示的內容時，最好使用 **ProcessAllIfPresent**。 此方式也會增加 16.7 毫秒的輸入延遲，因為 app 現在會在顯示器的同步間隔而非 **ProcessEvents** 封鎖遊戲迴圈。 由於每個畫面只會處理一次佇列 (60 Hz)，因此可能會捨棄一些輸入事件。

## <a name="scenario-4-render-60-frames-per-second-and-achieve-the-lowest-possible-input-latency"></a>案例 4：每秒轉譯 60 個畫面格並達到最低的輸入延遲


有些遊戲或許能夠忽略或補償案例 3 中所看到的輸入延遲增加。 不過，如果輸入延遲低對於遊戲的體驗以及玩家意見反應來說非常重要，每秒轉譯 60 個畫面格的遊戲就必須針對個別的執行緒處理輸入。

拼圖遊戲的第四個反覆運算建立在案例 3 之上，方法是，將遊戲迴圈的輸入處理和圖形轉譯分割成個別的執行緒。 每個遊戲迴圈擁有個別的執行緒可確保圖形輸出絕不會延遲輸入，不過，程式碼會因此變得更為複雜。 在案例 4 中，輸入執行緒會使用 [**CoreProcessEventsOption::ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217) 呼叫 [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)，以等待新的事件並分派所有可用的事件。 它會繼續這個行為，直到視窗關閉或遊戲呼叫 [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260) 為止。

``` syntax
void App::Run()
{
    // 4. Start a thread dedicated to rendering and dedicate the UI thread to input processing.
    m_main->StartRenderThread();

    // ProcessUntilQuit will block the thread and process events as they appear until the App terminates.
    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
}

void JigsawPuzzleMain::StartRenderThread()
{
    // If the render thread is already running, then do not start another one.
    if (IsRendering())
    {
        return;
    }

    // Create a task that will be run on a background thread.
    auto workItemHandler = ref new WorkItemHandler([this](IAsyncAction^ action)
    {
        // Notify the swap chain that this app intends to render each frame faster
        // than the display's vertical refresh rate (typically 60 Hz). Apps that cannot
        // deliver frames this quickly should set this to 2.
        m_deviceResources->SetMaximumFrameLatency(1);

        // Calculate the updated frame and render once per vertical blanking interval.
        while (action->Status == AsyncStatus::Started)
        {
            // Execute any work items that have been queued by the input thread.
            ProcessPendingWork();

            // Take a snapshot of the current game state. This allows the renderers to work with a
            // set of values that won't be changed while the input thread continues to process events.
            m_state->SnapState();

            m_sceneRenderer->Render();
            m_deviceResources->Present();
        }

        // Ensure that all pending work items have been processed before terminating the thread.
        ProcessPendingWork();
    });

    // Run the task on a dedicated high priority background thread.
    m_renderLoopWorker = ThreadPool::RunAsync(workItemHandler, WorkItemPriority::High, WorkItemOptions::TimeSliced);
}
```

在 Microsoft Visual Studio2015 的**DirectX 11 和 XAML App (通用 Windows)** 範本會將遊戲迴圈分割成多個執行緒類似的方式。 它使用 [**Windows::UI::Core::CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460) 物件啟動處理輸入專用的執行緒，同時建立與 XAML UI 執行緒無關的轉譯執行緒。 如需這些範本的詳細資訊，請參閱[從範本建立通用 Windows 平台和 DirectX 遊戲專案](user-interface.md)。

## <a name="additional-ways-to-reduce-input-latency"></a>降低輸入延遲的其他方式


### <a name="use-waitable-swap-chains"></a>使用可等候的交換鏈結

DirectX 遊戲會透過更新使用者在畫面上看到的內容，回應使用者輸入。 在 60 Hz 顯示器上，畫面每 16.7 毫秒 (1 秒/60 個畫面格) 會重新整理一次。 圖 1 顯示大約的生命週期以及對輸入事件的回應 (相對於每秒轉譯 60 個畫面格的應用程式 16.7 毫秒的重新整理訊號 (VBlank))：

圖 1

![圖 1 Directx 的輸入延遲 ](images/input-latency1.png)

在 Windows8.1，DXGI 導入交換鏈結，讓 app 輕鬆降低這個延遲，不需要實作啟發學習法以保持 Present 佇列空白**DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT**旗的標。 使用此旗標建立的交換鏈結稱為可等候的交換鏈結。 圖 2 顯示大約的生命週期，以及使用可等候的交換鏈結時對輸入事件的回應：

圖 2

![圖 2 Directx 可等候的輸入延遲](images/input-latency2.png)

我們從這些圖表中看到的內容就是遊戲可能可以透過兩個完整畫面降低的輸入延遲，但前提是這些遊戲可以在顯示器的重新整理頻率所定義的 16.7 毫秒預算內轉譯並呈現每個畫面。 拼圖範例透過呼叫下行來使用可等候的交換鏈結，並控制 Present 佇列的限制：` m_deviceResources->SetMaximumFrameLatency(1);`

 

 




