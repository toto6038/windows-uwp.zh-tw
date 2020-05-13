---
Description: ''
title: 螢幕讀取器和硬體按鈕事件
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: windows 10，uwp，協助工具，朗讀程式，螢幕閱讀程式
ms.localizationpriority: medium
ms.openlocfilehash: 41c6ed531f21a922b407ff3ba5aae93afb8d917e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234931"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>螢幕讀取器和硬體系統按鈕

螢幕讀者（例如 [[朗讀](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)程式]）必須能夠辨識並處理硬體系統按鈕事件，並將其狀態傳達給使用者。 在某些情況下，螢幕閱讀程式可能需要以獨佔方式處理這些硬體按鈕事件，而不是讓它們向上反升到其他處理常式。

從 Windows 10 2004 版開始，UWP 應用程式可以使用與其他硬體按鈕相同的方式，接聽並處理**Fn**硬體系統按鈕事件。 先前，此系統按鈕只會做為其他硬體按鈕報告其事件和狀態的修飾詞。

> [!NOTE]
> Fn 按鈕支援是 OEM 特有的，而且可以包含功能，例如切換/鎖定開啟或關閉（vs. 按住按鍵組合），以及相對應的鎖定指示器光源（這對盲人或視力受損的使用者可能不會有説明）。

Fn 按鈕事件會透過[Windows. UI. Input](/uwp/api/windows.ui.input)命名空間中的新[SystemButtonEventController 類別](/uwp/api/windows.ui.input.systembuttoneventcontroller)來公開。 SystemButtonEventController 物件支援下列事件：

- [SystemFunctionButtonPressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> 如果 SystemButtonEventController 已經由較高優先順序的處理常式處理，就無法接收這些事件。

## <a name="examples"></a>範例

在下列範例中，我們會示範如何根據 DispatcherQueue 建立[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) ，並處理這個物件所支援的四個事件。

當按下 Fn 按鈕時，通常會引發多個支援的事件。 例如，按下介面鍵盤上的 Fn 按鈕時，會同時引發 SystemFunctionButtonPressed、SystemFunctionLockChanged 和 SystemFunctionLockIndicatorChanged。

1. 在第一個程式碼片段中，我們只是包含必要的命名空間，並指定一些全域物件，包括[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue)和[DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)物件來管理[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)執行緒。

   然後，我們會指定在註冊[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)事件處理委派時傳回的[事件權杖](/uwp/cpp-ref-for-winrt/event-token)。

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. 我們也會指定[SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)事件的事件權杖以及布林值，以指出應用程式是否處於「學習模式」（其中使用者只是嘗試流覽鍵盤，而不執行任何功能）。

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. 第三個程式碼片段包含[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)物件所支援之每個事件的對應事件處理常式委派。

   每個事件處理常式都會宣佈已發生的事件。 此外，FunctionLockIndicatorChanged 處理常式也會控制應用程式是否為「學習」模式（ `_isLearningMode` = true），這可防止事件反升至其他處理常式，並讓使用者流覽鍵盤功能而不實際執行動作。

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>請參閱

[SystemButtonEventController 類別](/uwp/api/windows.ui.input.systembuttoneventcontroller)
