---
description: 深入瞭解可協助您撰寫支援 Windows、iOS 和 Android 的應用程式的工具和技術。
title: 選取 iOS 和 UWP 應用程式開發的方式
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7a8920b13b7e28947138a000fd46165abcb58b7
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104679"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>選取 iOS 和 UWP 應用程式開發的方式


開發跨平台 app 時有哪些選擇？

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>若要同時支援 iOS 與 Windows 最佳的方式為何？

Windows 與 iOS 似乎是非常不同的機制，但如果您要撰寫支援兩種平台 (也包括 Android) 的 app，有越來越多的工具和技術可大大地協助您。 最好的解決方案取決於您撰寫的 app 類型，以及您是從頭開始或移植現有的專案。

## <a name="writing-a-new-app"></a>撰寫新的 app

您可以使用全新的平板電腦，其中有許多選項供您任意使用，包括：

-   [Xamarin](https://xamarin.com/)

    有了 Xamarin，您可以使用 C# 撰寫應用程式，讓它在 Windows 上執行，也可以建立原生的 iOS 應用程式。 Visual Studio 內建對 Xamarin 的支援；只要選取正確的專案類型即可。

-   [Apache Cordova](https://www.microsoft.com/?ref=go)

    如果您比較熟悉 Javascript 和 HTML，Apache Cordova (也稱為 PhoneGap) 將協助您建立適用於 iOS、Windows 和 Android 的跨平台應用程式。 Visual Studio 也內建這個專案類型。

-   遊戲引擎

    有了 [Unity3D](https://www.unity3d.com/) 和 [Unreal Engine](https://www.unrealengine.com/en-US/) 這類供您任意使用的工具，就可以撰寫 AAA 品質的遊戲，供 Windows 和其他許多平台 (包括 iOS) 使用。 Unity 支援 C# 指令碼，Unreal 使用 C++。

-   [MonoGame](http://www.monogame.net/)

    XNA 的精神繼任者。 現在這是一個開放原始碼的跨平台架構，表示您可以使用 C# 為許多支援物理引擎以及 2D 和 3D 圖形的平台撰寫應用程式。

## <a name="adapting-an-existing-app"></a>調整現有的應用程式

如果是針對現有的 iOS app，您的選擇就會稍微受到限制。 不過可以確定的是，絕對不會有任何遺失。

-   [適用於 iOS 的 Windows 橋接器](https://github.com/Microsoft/WinObjC)

    這是一項仍在開發中的工具，可將 Xcode 專案直接匯入 Visual Studio。 可從 Visual Studio 內建置並偵錯 Objective-C 程式碼。 如果您的專案針對圖形使用 Cocos 這類程式庫，您可能會發現這是一個很好用的方式快速移植 App。

-   重新調整您的 C++ 程式碼。

    如果您的核心商務邏輯是以 C++ 撰寫，而不是 Objective-C 或 Swift，在專案中只要做一點點變更，就可以使用這個程式碼。 然後您可以使用 XAML 來定義您的 UI，就像用於其他 Windows 應用程式一樣，然後必要時再呼叫 C++ 程式碼。

-   [使用 ANGLE 在 Windows 上執行 OpenGL ES](https://github.com/microsoft/angle/wiki)

    移植 OpenGL ES 2.0 專案的中間步驟是使用 ANGLE。 ANGLE 可讓您透過將 OpenGL ES API 呼叫轉譯為 DirectX 11 API 呼叫，在 Windows 上執行 OpenGL ES 內容。

## <a name="other-cross-platform-authoring-tools"></a>其他跨平台編寫工具

-   [GameSalad](https://gamesalad.com/)

    遊戲編寫環境。

-   [Construct 2]( https://www.scirra.com/)

    遊戲編寫環境。

-   [Titanium Studio](https://www.appcelerator.com/platform/titanium-studio/)

    跨平台編寫環境。

-   [Cocos2D-x](https://www.cocos2d-x.org/)

    適用於子畫面處理與物理模型化的跨平台程式碼程式庫。

-   [Impact.js](https://impactjs.com/)

    以 HTML 為基礎的遊戲庫。

-   [Marmalade](http://madewithmarmalade.com/)

    跨平台 SDK。

-   [OpenFL](https://www.openfl.org/)

    跨平台開發工具。

-   [GameMaker](https://www.yoyogames.com/gamemaker/studio)

    專門針對遊戲的編寫環境。

-   [PlayCanvas](https://playcanvas.com/)

    以 HTML 為基礎的遊戲開發工具。

