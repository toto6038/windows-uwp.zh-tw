---
title: 擷取在 UWP 上的 Windows 系統使用者
description: 了解如何擷取 Windows 系統使用者，在通用 Windows 平台 (UWP) 遊戲中。
ms.date: 06/07/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，系統使用者
ms.localizationpriority: medium
ms.openlocfilehash: c46f7e98c2dea3b23beb2cec80816067d4c4e341
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632753"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>擷取通用 Windows 平台 (UWP) 標題中的 Windows 系統使用者

## <a name="windowssystemuser"></a>Windows.System.User

A [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user)物件都代表本機 Windows 使用者。 在 Xbox 主控台中，它可讓多個 windows 使用者在登入的相同的時間，以單一的互動式工作階段中，如果您的應用程式則是多使用者應用程式中，將需要 Windows.System.User 物件建構 XboxLiveUser 才能存取即時服務。 在 PC 或 phone 等其他 windows 平台，它有可能是只允許一個 windows 使用者在一部裝置上或一個互動式工作階段，來建構 XboxLiveUser Windows.System.User 傳入不需要。

## <a name="ways-to-retrieve-windows-system-user"></a>如何擷取 Windows 系統使用者

* **使用中的靜態方法[Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user)類別。**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user)類別提供一組靜態方法，可協助擷取 Windows.System.User 物件。 舉個例說，您可能會呼叫 FindAllAsync 來取得所有作用中的 windows 使用者。

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)類別提供方法來啟動使用者選擇器 UI，並在多使用者實例中選取 windows 系統使用者。

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)為核心介面的所有控制器裝置 （遊戲台、 賽車滾輪、 飛行隨身碟等）。 您可能會收到 Windows.System.User 物件呼叫其使用者屬性，與遊戲控制器相關聯。  

  以下是一些實作由 windows 的遊戲控制器[ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick)， [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick)，[遊戲台](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad)， [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel)。

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  您可以呼叫靜態方法來尋找相關聯的使用者具有裝置識別碼的 FindUserFromDeviceId。您通常可以收到輸入的 windows 事件，從裝置識別碼，例如[Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs)， [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)
