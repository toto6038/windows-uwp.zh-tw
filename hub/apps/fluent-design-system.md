---
description: 了解 Fluent Design 以及如何將其納入您的應用程式。
title: 適用於 Windows 的 Fluent Design 系統
keywords: uwp 應用程式配置, 通用 Windows 平台, 應用程式設計, Fluent Design 系統
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 7d340b3db12aef964a65860855acc0f0722cda64
ms.sourcegitcommit: 6cdba316bdbd85a2429259ebfb59ff94440e234a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85882932"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>適用於 Windows 的 Fluent Design 系統應用程式建立者

![Fluent Design 標頭](images/fluent/fluentdesign-app-header.png)

## <a name="introduction"></a>簡介

Fluent Design 系統是我們的系統，用於建立調適型、富有感情的，且效能良好的使用者介面。

## <a name="principles"></a>原則

**調適型：Fluent 體驗在每個裝置上帶來自然的感覺**

Fluent 體驗適應環境。 Fluent 體驗在桌上型電腦、平板電腦，以及 Xbox 上感覺舒適—甚至適用於混合實境頭戴式裝置。 且當您新增更多的硬體，例如您電腦的其他螢幕，Fluent 體驗可充分利用它。

**富有感情的：Fluent 體驗是直覺且強大的**

Fluent 體驗調整行為和意圖&mdash;它們了解並預期所需的項目。 它們連結人和的想法，不論是在地球的另一端或站在彼此身邊。

**效能良好：Fluent 體驗是生動且沈浸式**

藉由納入現實世界的元素，Fluent 體驗點選到基本項目。 它會使用光線、陰影、動作、深度和紋理，以直覺與本能的方式組織資訊。


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>透過 UWP 將 Fluent Design 套用到您的應用程式

![Fluent Design 標誌](images/fluent/fluentdesign_header.png)

我們的設計指導方針說明如何將 Fluent Design 原則套用至應用程式。 何種應用程式？ 雖然我們有許多指導方針可套用到任何平台，但我們建立了 UWP (通用 Windows 平台) 來支援 Fluent Design。

將 Fluent Design 功能建置到 UWP。 其中部分功能&mdash;例如有效像素和通用輸入系統&mdash;是自動的。 您不需要編寫任何額外程式碼，即可運用它們。 其他功能 (例如壓克力風格) 是選用的；您可撰寫程式碼來將它們新增在您的應用程式中。

> 我們將 UWP 控制項放到桌面上，以便您可以 Fluent Design 功能提升外觀、感覺和現有 WPF 或 Windows 應用程式的功能。 若要深入了解，請參閱[在 WPF 和 Windows Form 應用程式中裝載 UWP 控制項](/windows/uwp/xaml-platform/xaml-host-controls)。

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

除了設計指引，我們的 Fluent Design 文章也會說明如何撰寫程式碼，讓您的設計付諸實踐。 UWP 會使用 XAML，這是以標記為基礎的語言，可讓您更輕鬆地建立使用者介面。 以下是範例：

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![XAML 範例](images/fluent/xaml-example.png)


> 如果您是 UWP 開發的新手，請參閱我們的[開始使用 UWP 頁面](/windows/uwp/get-started)。

## <a name="find-a-natural-fit"></a>尋找自然符合

要如何讓各種裝置上的應用程式感覺自然呢？ 藉由讓它感覺就像是考量每個特定裝置來設計它。 UI 配置適合不同螢幕大小因此未浪費的一點空間 (但不是擠在一塊) 讓體驗感覺自然就像是專為該裝置設計的。

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**正確中斷點的設計**

將焦點放在幾個重要的寬度 (也稱做「中斷點」) 可大幅簡化您的設計與程式碼，同時仍然讓您的應用程式從小螢幕到大螢幕上看起來都很棒，而不是針對每一個別螢幕大小設計。

[了解螢幕大小與中斷點](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**建立回應式配置**

若要讓應用程式感覺自然，應將其配置調整為不同的螢幕大小和裝置。 您可以使用自動調整大小、配置面板、視覺狀態，甚至將 XAML 中的 UI 定義分開來建立回應式 UI。

[了解回應式設計](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**適用於裝置頻譜的設計**

UWP 應用程式可以在各種不同執行 Windows 的裝置上執行。 這對了解有哪些裝置可用、它們用於什麼地方，以及使用者如何與它們互動，很有幫助。

[了解 UWP 裝置](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**適用於正確輸入的最佳化**

UWP 應用程式會自動支援常見的滑鼠、鍵盤、手寫筆及觸控式互動。 您不需要執行額外的動作。 但是，如果想要，您可以使用適用於特定輸入的最佳化支援來增強應用程式，例如手寫筆和 Surface Dial。

[了解輸入與互動](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>讓它是直覺的

當它做出使用者期待的行為，體驗感覺是直覺的。 利用以建立的控制項和模式以及充分利用協助工具與全球化的平台支援，您可以建立輕鬆的體驗，可協助使用者變更有效率。

示範同理是在正確的時間做正確的事。

Fluent 體驗以一致的方式使用控制項與模式，如此，使用者已學會預期它們行為的方式。 人可存取的 Fluent 體驗有廣泛的實體功能，且納入全球化功能，讓世界各地的人可以使用。

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**提供適當的瀏覽**

使用正確的應用程式結構和瀏覽元件來建立輕鬆的體驗。

[了解瀏覽](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**互動式**

按鈕、命令列、鍵盤快速鍵和內容功能表可讓使用者與您的應用程式互動；這些工具會將靜態體驗變更為動態的內容。

[了解命令](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**對工作使用正確的控制項**

控制項是使用者介面的建置組塊；使用正確的控制項可協助您建立的使用者介面，做出使用者期待的行為。 UWP 提供超過 45 種控制項，範圍從簡單按鈕到強大資料控制。

[了解 UWP 控制項](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![內含影像](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**內含**妥善設計的應用程式可讓身心障礙人士存取。 加入一些額外的程式設計，您可以與世界各地的人分享您的應用程式。

[了解可用性](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>生動和沈浸式

Fluent Design 不是亮色效果。 它包含實體效果，其完全增強使用者體驗，因為它們模擬我們經過程式設計的大腦做為有效處理的經驗。

## <a name="use-light"></a>使用光線

光源總是能夠吸引大家的注意力。 它會創造氛圍與存在感，而且是照亮資訊的實用工具。

將光線新增到您的 UWP 應用程式：

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**顯示顯目提示**

[顯示醒目提示](/windows/uwp/design/style/reveal)使用光線讓互動式元素顯得與眾不同。光線會照亮使用者可以與之互動的元素，並顯示隱藏的框線。 在部分控制項上已自動啟用顯示，例如清單檢視及格線檢視。 您可以藉由套用我們預先定義的顯示醒目提示樣式，在其他控制項上啟用它。
:::row-end:::

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**顯示焦點**

[顯示焦點](/windows/uwp/design/style/reveal-focus)使用光線提醒您注意目前已輸入焦點的元素。
:::row-end:::

## <a name="create-a-sense-of-depth"></a>建立深度感

我們活在立體世界中。 藉由特意將深度納入 UI，我們可以轉換扁平的 2-D 介面，透過建立視覺階層&mdash;有效率地呈現資訊。 此功能在有層次的實體環境中，重新塑造項目彼此之間的關聯

將深度新增到您的 UWP 應用程式：

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**視差**

[視差](/windows/uwp/design/motion/parallax)透過讓前景中的項目看起來移動速度比背景中的項目快，創造深度幻覺。
:::row-end:::

## <a name="incorporate-motion"></a>納入動作

可以將動作設計想成電影。 順暢的轉場能讓您專注於故事本身，讓體驗與生活結合。 我們可以將這些感覺納入我們的設計中，使用電影的輕鬆感帶領人們從一個工作移往下一項工作。

將動作新增到您的 UWP 應用程式：

:::row:::
    :::column:::
        ![持續性 gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**連接動畫**

[連接動畫](/windows/uwp/design/motion/connected-animation)可讓使用者在場景之間建立無縫轉場，維持脈絡。
:::row-end:::

## <a name="build-it-with-the-right-material"></a>使用正確的材質建置

現實世界中圍繞著我們的萬物充滿感官性和刺激。 它們會彎曲、伸展、反彈、粉碎和滑翔。 這些材質轉化為數位環境，讓人們想要伸手去碰觸我們的設計。

將材質新增到您的 UWP 應用程式：

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**壓克力**

[壓克力](/windows/uwp/design/style/acrylic)為透明材質，可讓使用者看到多層內容，建立 UI 元素階層。
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>設計工具組與程式碼範例

想要開始建立您自己的應用程式與 Fluent Design 嗎？ 我們的工具組 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，以及 Sketch 有助於快速建立您的設計，以及我們的範例有助於您更快速地設計程式。

:::row:::
    :::column:::
        ![fpo 影像](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**設計工具組與範例頁面**

請查看我們的[設計工具組與範例頁面](/windows/uwp/design/downloads/)
:::row-end:::









