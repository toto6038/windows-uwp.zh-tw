---
author: QuinnRadich
title: 使用通用 Windows 平台建立應用程式
description: 這比您以為的建立適用於 Windows 10 的通用 Windows 平台 (UWP) app 還要簡單。
ms.author: quradic
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66536a3059ea6d9b17709c836f4149b1ec583165
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1692709"
---
# <a name="create-apps-for-windows-10"></a>建立適用於 Windows 10 的應用程式

![建置您的 App](images/build-your-app.png)

歡迎使用 [UWP 平台](universal-application-platform-guide.md)！ 無論您想要的是開始建立您的第一個 UWP app，還是使用更多的進階功能，下列教學課程將會讓您步入正軌。您將會了解如何：

-   在 Microsoft Visual Studio 中建立 UWP 專案。
-   將 UI 元素和程式碼新增至專案。
-   使用 XAML、資料繫結及其他基本 UWP 元素。
-   將獨特的 UWP 功能 (例如筆跡和 Dial) 納入您的應用程式。
-   使用協力廠商程式庫加入新的功能。
-   在本機電腦上建置並偵錯 App。

## <a name="ask-a-bot"></a>詢問 Bot！

如果您在尋找適當文件時受阻或是需要協助，請嘗試詢問下面實驗性的聊天 Bot。 例如，問說「哪裡可以下載 Visual Studio？」 或「告訴我關於 Fluent 設計的資訊」。 如果沒有得到有用的答案，請嘗試稍微改變您的查詢措辭。

<iframe src='https://webchat.botframework.com/embed/DocBot4?s=T2nP6qZUXC8.cwA.lvc.AR-ZBwtULpaITu6_dAhMwrmg4R2GSLNzIoiMNFL8M7M' height="400" width="400"></iframe>

## <a name="write-your-first-uwp-app-in-your-favorite-programming-language"></a>使用最喜愛的程式設計語言撰寫您的第一個 UWP app

如果您是新的開發人員，或是您熟悉 Windows 平台但不想要開始使用 UWP 時，請查看下列基本教學課程︰

* [使用 C#,、Visual C++ 或 JavaScript 建立您的第一個 UWP app](your-first-app.md)

您是 IOS 開發人員嗎？

* 請使用[適用於 iOS 的 Windows 橋接器](https://developer.microsoft.com/windows/bridges/ios)將您現有的程式碼轉換成 UWP app，然後繼續以 Objective-C 進行開發。

如果您仍在學習，或是需要溫故知新，請試著閱讀下列外部資源：

* [Windows 10 開發人員指南](https://go.microsoft.com/fwlink/?linkid=850804)
* [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/)

## <a name="customize-your-apps-layout-and-appearance-with-xaml"></a>使用 XAML 自訂應用程式版面配置及外觀

大部分 UWP app 會使用 XAML 標記語言建立其 UI。 了解您可以如何使用其核心功能來自訂您的 App 的視覺呈現，並瀏覽此指導方針以建立適合您的 App 的獨特外觀。

* [應用程式 UI 設計簡介](../design/basics/design-and-ui-intro.md)
* [教學課程：以 XAML 建立使用者介面](../design/basics/xaml-basics-ui.md)
* [UWP app 的版面配置](../design/layout/index.md)
* [適用於UWP app 的控制項和模式](../design/controls-and-patterns/index.md)

## <a name="use-features-unique-to-windows-10"></a>使用只有 Windows 10 才有的功能

Windows 10 有什麼特別之處？ 學習只使用它的一些獨特功能。

* [教學課程︰在 UWP app 中支援筆跡](../design/input/ink-walkthrough.md)
* [教學課程︰支援 Surface Dial](../design/input/radialcontroller-walkthrough.md)
* [探索最新版 Windows 中的新功能](../whats-new/windows-10-version-latest.md)

探索操作說明文章和適用於 Windows 10 開發的詳細文件：

* [開發 UWP app 的操作說明文章](https://developer.microsoft.com/windows/apps/develop)
* [UWP app 的 API 參照](https://docs.microsoft.com/en-us/uwp/)

## <a name="develop-javascript-and-web-apps"></a>開發 JavaScript 和 Web 應用程式

UWP 是非常有彈性的平台，可支援各種不同的語言版本及架構。 利用 JavaScript 建置 UWP app，並使用您的技能建置可在 Microsoft Store 中特別展售的託管 Web 應用程式。

* [利用您的 Web 技能，使用 HTML5、CSS3 和 JavaScript 來建置 App。](your-first-app.md#javascript-and-html)

對於有關建置 Web 應用程式的詳細資訊感興趣嗎？

* [Microsoft Edge 開發人員文件](https://docs.microsoft.com/microsoft-edge/)

## <a name="cross-platform-and-mobile-development"></a>跨平台和行動應用程式開發

* 需要以 Android 和 iOS 做為目標嗎？ 請查看 [Xamarin](https://www.xamarin.com)。

## <a name="see-also"></a>請參閱

* [發佈您的 UWP app](https://developer.microsoft.com/store/publish-apps)。
* [開發 UWP app 的操作說明文章](https://developer.microsoft.com/windows/apps/develop)
* [適用於 UWP 開發人員的程式碼範例](https://developer.microsoft.com/windows/samples)
* [什麼是 UWP app？](universal-application-platform-guide.md)
* [開始設定](get-set-up.md)
* [註冊 Windows 帳戶](sign-up.md)