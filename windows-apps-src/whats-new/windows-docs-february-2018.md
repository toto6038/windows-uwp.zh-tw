---
author: QuinnRadich
title: 2018 年 2 月 Windows 文件的最新動向 - 開發 UWP app
description: 新功能、影片及開發人員指引已加入 2018 年 2 月的 Windows 10 開發人員文件中
keywords: 最新動向, 更新, 功能, 開發人員指引, Windows 10, 2 月
ms.author: quradic
ms.date: 2/5/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6eb9b95ad7c5f7c63bca5b11fbbbf9018b75fcf
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6023637"
---
# <a name="whats-new-in-the-windows-developer-docs-in-february-2018"></a>2018 年 2 月 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 1 月份已有下列功能概觀、開發人員指引和影片可以使用，包含提供給 Windows 開發人員的全新及更新資訊。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。


## <a name="features"></a>功能

### <a name="adaptive-and-interactive-toast-notifications"></a>調適型和互動式快顯通知

使用調適型和互動式通知增強您的 App 開始使用我們[更新的快顯通知相關指引](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)，並探索有關影像大小限制、進度列以及新增輸入選項的新資訊。

使用[自訂時間戳記](../design/shell/tiles-and-notifications/custom-timestamps-on-toasts.md)、[自訂音效](../design/shell/tiles-and-notifications/custom-audio-on-toasts.md)或[進度列](../design/shell/tiles-and-notifications/toast-progress-bar.md)進一步自訂快顯通知

使用[通知鏡像](../design/shell/tiles-and-notifications/notification-mirroring.md)和[通用關閉](../design/shell/tiles-and-notifications/universal-dismiss.md)，在各種不同的使用者裝置上延伸通知。

使用[快顯通知標頭](../design/shell/tiles-and-notifications/toast-headers.md)和[擱置中更新啟用](../design/shell/tiles-and-notifications/toast-pending-update.md)來分組並整理您的通知。

![擱置中更新啟用的實際運作](../design/shell/tiles-and-notifications/images/toast-pendingupdate.gif)

### <a name="page-layouts-with-xaml"></a>使用 XAML 的頁面配置

我們已使用有關流暢版面配置及視覺狀態的新資訊來更新 [XAML 頁面配置](../design/layout/layouts-with-xaml.md)文件。 這些新功能允許進一步控制 App 中元素位置的回應方式，並配合可用視覺空間進行調整。

![XAML 頁面配置的邊界及邊框間距](../design/layout/images/xaml-layout-margins-padding.png)

### <a name="subscription-add-ons-are-now-available-to-all-developers"></a>訂閱附加元件現在可提供給所有開發人員

建立和發佈訂閱附加元件，透過自動化固定計費週期在您的 App 和遊戲中銷售數位產品 (例如應用程式功能或數位內容)。 如需詳細資訊，請參閱[啟用應用程式的訂閱附加元件](../monetize/enable-subscription-add-ons-for-your-app.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="design-basics"></a>設計基本知識

我們的 [UWP app 設計簡介](../design/basics/design-and-ui-intro.md)已使用大量新的視覺效果範例進行增強。 每個 UWP 中的通用設計功能概觀現在特別強調如何開始實作 Fluent Design 系統的功能。

我們已將一般頁面模式的展示工具新增至[內容設計基本知識](../design/basics/content-basics.md)，並提供 App 如何顯示不同類型內容的範例資源庫。

![中樞頁面模式圖例](../design/basics/images/hub.png)

### <a name="writing-style"></a>撰寫方式

我們已針對有關語音與音調的文章提升品質並擴充內容，並將其轉換成[撰寫方式指引](../design/style/writing-style.md)。 這項新資訊提供關於在 App 中建立有效文字的原則，並建議撰寫控制項 (例如錯誤訊息或對話方塊) 的最佳做法。

### <a name="getting-started-for-game-development"></a>遊戲開發入門

對開發 Windows 10 遊戲感興趣？ 新的[遊戲開發入門](../gaming/getting-started.md)頁面提供您自行完成設定、註冊，以及準備好提交應用程式與遊戲所需執行之動作的完整概觀。

## <a name="videos"></a>影片

### <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

Xbox Live 創作者計畫可讓開發人員將其 UWP 遊戲快速發佈至 Xbox One 和 Windows 10。[觀看影片](https://www.youtube.com/watch?v=zpFfHHBkVq4)了解計畫，然後[查看此頁面](https://www.xbox.com/developers/creators-program)來開始。

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>建立 Windows Mixed Reality 的 3D 應用程式啟動器

3D 啟動器提供獨特方式，讓使用者將您的 App 的真實體積測量表示法放置在混合實境主要環境中。 [觀看影片](https://www.youtube.com/watch?v=TxIslHsEXno)了解如何準備 3D 模型，並指派其做為您的 App 的啟動器，然後[閱讀開發人員文件](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers)和[查看我們的設計指引](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)以取得詳細資訊。

### <a name="motion-controller-tracking"></a>運動控制器追蹤

運動控制器在 Windows Mixed Reality 中代表使用者的雙手。 [觀看影片](https://www.youtube.com/watch?v=rkDpRllbLII)了解運動控制器處於混合實境頭戴式裝置視野範圍內外的運作方式，並[在這裡閱讀更多有關控制器追蹤的資訊](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)。