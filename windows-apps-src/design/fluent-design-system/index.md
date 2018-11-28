---
description: 了解 Fluent Design 以及如何將其納入您的應用程式。
title: 適用於 Windows 的 fluent Design 系統
keywords: uwp app 配置、通用 Windows 平台、應用程式設計、Fluent Design 系統
ms.date: 3/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7c5d2c1b112b96dc86d1dfef3015f9b52f43cb83
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7993444"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Fluent 設計系統的 Windows 應用程式 creators

![Fluent 設計標頭](images/fluentdesign-app-header.jpg)

## <a name="introduction"></a>簡介

Fluent Design 系統是我們建立彈性、 共鳴和美觀的使用者介面的系統。

## <a name="principles"></a>原則

**調適型：Fluent 體驗在每個裝置上帶來自然的感覺**

Fluent 體驗適應環境。 Fluent 體驗以及上感覺舒適平板電腦、 桌上型電腦、 Xbox — 它甚至適用於混合實境頭戴式裝置。 且當您新增更多的硬體，例如您電腦的其他螢幕，Fluent 體驗可充分利用它。

**富有感情的：Fluent 體驗是直覺且強大的**

Fluent 體驗調整行為和意圖&mdash;它們了解並預期所需的項目。 它們連結人和的想法，不論是在地球的另一端或站在彼此身邊。

**效能良好：Fluent 體驗是生動且沈浸式**

藉由納入現實世界的元素，Fluent 體驗點選到基本項目。 它會使用光線、陰影、動作、深度和紋理，以直覺與本能的方式組織資訊。


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>套用 Fluent Design 到您的應用程式的 UWP

![Fluent 設計標誌](images/fluentdesign_header.png)

我們設計指導方針將說明如何將 Fluent 設計原則套用到應用程式。 什麼類型的應用程式中？ 雖然我們的指導方針的許多可以套用至任何平台，但我們會建立 UWP （通用 Windows 平台） 以支援 Fluent Design。

將 Fluent Design 功能建置到 UWP。 其中部分功能&mdash;例如有效像素和通用輸入系統&mdash;是自動的。 您不需要編寫任何額外程式碼，即可運用它們。 其他功能 (例如壓克力風格) 是選用的；您可撰寫程式碼來包含它們新增在您的應用程式中。

> 我們將 UWP 控制項放到桌面上，以便您可以 Fluent Design 功能提升外觀、感覺和現有 WPF 或 Windows 應用程式的功能。 若要深入了解，請參閱[WPF 及 Windows Forms 應用程式中的主機 UWP 控制項](/windows/uwp/xaml-platform/xaml-host-controls)。

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

除了設計指導方針，我們的 Fluent Design 文章也說明如何撰寫可讓您的設計會發生的代碼。 UWP 使用 XAML，標記為基礎的語言，可讓您更輕鬆地建立使用者介面。 以下是範例：

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="24,16"/>
</Grid>
```

![](images/xaml-example.png)


> 如果您是 UWP 開發的新手，請查看我們[開始使用 UWP 頁面](https://developer.microsoft.com/windows/apps/getstarted)。

## <a name="find-a-natural-fit"></a>尋找自然符合

要如何讓各種裝置上的應用程式感覺自然呢？ 藉由讓它感覺就像是考量每個特定裝置來設計它。 UI 配置適合不同螢幕大小&mdash;因此未浪費的一點空間 (但不是擠在一塊) &mdash;讓體驗感覺自然就像是專為該裝置設計的。

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>讓直覺化

當它的做出使用者期待的行為，體驗感覺是直覺。 利用以建立的控制項和模式以及充分利用協助工具與全球化的平台支援，您可以建立輕鬆的體驗，可協助使用者變更有效率。

示範同理是在正確的時間做正確的事。

Fluent 體驗以一致的方式使用控制項與模式，如此，使用者已學會預期它們行為的方式。 人可存取的 Fluent 體驗有廣泛的實體功能，且納入全球化功能，讓世界各地的人可以使用。

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>生動和沈浸式

Fluent Design 不是亮色效果。 它包含實體效果，其完全增強使用者體驗，因為它們模擬我們經過程式設計的大腦做為有效處理的經驗。

## <a name="use-light"></a>使用光線

光源總是能夠吸引大家的注意力。 它會創造氛圍與存在感，而且是照亮資訊的實用工具。

將光線新增到您的 UWP app：

:::row:::
    :::column:::
        ![fpo image](../style/images/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](../style/reveal.md) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](../style/images/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](../style/reveal-focus.md) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>建立深度感

我們活在立體世界中。 藉由特意將深度納入 UI，我們可以轉換扁平的 2-D 介面，透過建立視覺階層&mdash;有效率地呈現資訊。 此功能在有層次的實體環境中，重新塑造項目彼此之間的關聯

將深度新增到您的 UWP app：

:::row:::
    :::column:::
        ![fpo image](../motion/images/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](../motion/parallax.md) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>納入動作

可以將動作設計想成電影。 順暢的轉場能讓您專注於故事本身，讓體驗與生活結合。 我們可以將這些感覺納入我們的設計中，使用電影的輕鬆感帶領人們從一個工作移往下一項工作。

將動作新增到您的 UWP app：

:::row:::
    :::column:::
        ![continuity gif](images/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](../motion/connected-animation.md) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>使用正確的材質建置

現實世界中圍繞著我們的萬物充滿感官性和刺激。 它們會彎曲、伸展、反彈、粉碎和滑翔。 這些材質轉化為數位環境，讓人們想要伸手去碰觸我們的設計。

將材質新增到您的 UWP app：

:::row:::
    :::column:::
        ![fpo image](../style/images/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](../style/acrylic.md) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>設計工具組與程式碼範例

想要開始建立您自己的應用程式與 Fluent Design 嗎？ 我們的工具組 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，以及 Sketch 有助於快速建立您的設計，以及我們的範例有助於您更快速地設計程式。

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









