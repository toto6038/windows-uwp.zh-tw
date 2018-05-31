---
description: 了解 Fluent Design 以及如何將其納入您的應用程式。
title: 適用於 UWP 應用程式的 Fluent Design 系統
author: mijacobs
keywords: uwp app 配置、通用 Windows 平台、應用程式設計、Fluent Design 系統
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5b57dc2ddae4c6e260df663097db5649866b96aa
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638786"
---
# <a name="the-fluent-design-system-for-uwp-apps"></a>適用於 UWP 應用程式的 Fluent Design 系統

## <a name="introduction"></a>簡介

<img src="images/fluentdesign-app-header.jpg" alt=" " />

改善使用者介面中。 擴展到包含全新的維度和介面，從 2D 到 3D 以及更新的介面，從鍵盤和滑鼠到注視、手寫筆，以及觸控。  

Fluent Design 系統是一組與最佳做法結合的創新 UWP 功能，用於建立在所有執行 Windows 裝置類型上都能展現絕佳效能的應用程式。

它是我們的系統，用於建立調適型、富有感情的，且效能良好的使用者介面。 

**調適型：Fluent 體驗在每個裝置上帶來自然的感覺**

Fluent 體驗適應環境。 Fluent 體驗在桌上型電腦、平板電腦，以及 Xbox 上感覺舒適&mdash;甚至適用於混合實境頭戴式裝置。 且當您新增更多的硬體，例如您電腦的其他螢幕，Fluent 體驗可充分利用它。 

**富有感情的：Fluent 體驗是直覺且強大的**

Fluent 體驗調整行為和意圖&mdash;它們了解並預期所需的項目。 它們連結人和的想法，不論是在地球的另一端或站在彼此身邊。 

示範同理是在正確的時間做正確的事。 

Fluent 體驗以一致的方式使用控制項與模式，如此，使用者已學會預期它們行為的方式。 人可存取的 Fluent 體驗有廣泛的實體功能，且納入全球化功能，讓世界各地的人可以使用。 

**效能良好：Fluent 體驗是生動且沈浸式** 

藉由納入現實世界的元素，Fluent 體驗點選到基本項目。 它會使用光線、陰影、動作、深度和紋理，以直覺與本能的方式組織資訊。 

Fluent Design 不是亮色效果。 它包含實體效果，其完全增強使用者體驗，因為它們模擬我們經過程式設計的大腦做為有效處理的經驗。 

## <a name="applying-fluent-design-to-your-app"></a>套用 Fluent Design 到您的應用程式

將 Fluent Design 功能建置到 UWP。 其中部分功能&mdash;例如有效像素和通用輸入系統&mdash;是自動的。 您不需要編寫任何額外程式碼，即可運用它們。 其他功能 (例如壓克力風格) 是選用的；您可撰寫程式碼來包含它們新增在您的應用程式中。 

> 若要深入了解每個 UWP app 自動包含的基本功能，請參閱 [UWP app 設計簡介文章](../basics/design-and-ui-intro.md)。 如果您是新手開發 UWP，您可能想先參閱我們的 [開始使用 UWP 頁面](https://developer.microsoft.com/windows/apps/getstarted)。 

若要深入了解可協助您將 Fluent Design 納入應用程式的新功能，請繼續閱讀。

## <a name="find-a-natural-fit"></a>尋找自然符合

要如何讓各種裝置上的應用程式感覺自然呢？ 藉由讓它感覺就像是考量每個特定裝置來設計它。 UI 配置適合不同螢幕大小&mdash;因此未浪費的一點空間 (但不是擠在一塊) &mdash;讓體驗感覺自然就像是專為該裝置設計的。 

*  **正確中斷點的設計**

    將焦點放在幾個重要的寬度 (也稱做「中斷點」) 可大幅簡化您的設計與程式碼，同時仍然讓您的應用程式從小螢幕到大螢幕上看起來都很棒，而不是針對每一個別螢幕大小設計。

    [深入了解螢幕大小與中斷點](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)

*  **建立回應式配置**

    針對感覺自然的應用程式，需要填滿可顯示的空間，而不會太擁擠。 UWP 提供在格線、堆疊，與流程中排列好內容的面板，且您可以在彼此間巢狀化它們。

    [深入了解 UWP 配置面板](/windows/uwp/design/layout/layout-panels)

* **適用於裝置頻譜的設計**

    UWP 應用程式可以在各種不同執行 Windows 的裝置上執行。 這對了解有哪些裝置可用、它們用於什麼地方，以及使用者如何與它們互動，很有幫助。

    [深入了解 UWP 裝置](/windows/uwp/design/devices/)

* **適用於正確輸入的最佳化**

    UWP 應用程式會自動支援常見的滑鼠、鍵盤、手寫筆及觸控式互動&mdash;您不需要執行額外的動作。 但是，您可以使用適用於特定輸入的最佳化支援來增強應用程式，例如手寫筆和 Surface Dial。

    [深入了解輸入與互動](/windows/uwp/design/input/input-primer)


## <a name="make-it-intuitive-and-powerful"></a>讓它是直覺且強大的

當它做出使用者期待的行為，體驗感覺是直覺的。 利用以建立的控制項和模式以及充分利用協助工具與全球化的平台支援，您可以建立輕鬆的體驗，可協助使用者變更有效率。 

* **使用適用的控制項工作。**

    控制項是使用者介面的建置組塊；使用正確的控制項可協助您建立的使用者介面，做出使用者期待的行為。  UWP 提供超過 45 種控制項，範圍從簡單按鈕到強大資料控制。 

    [深入了解 UWP 控制項](/windows/uwp/design/controls-and-patterns/)

* **全人的** 

    妥善設計的應用程式可讓身心障礙人士存取。 加入一些額外的程式設計，您可以與世界各地的人分享您的應用程式。

    [深入了解可用性](/windows/uwp/design/usability/)


## <a name="be-engaging-and-immersive"></a>生動和沈浸式 

透過納入實體元素，例如光線和動作，讓您的應用程式更生動。 

## <a name="use-light"></a>使用光線

光源總是能夠吸引大家的注意力。 它會創造氛圍與存在感，而且是照亮資訊的實用工具。
        
將光線新增到您的 UWP app：
        
* [顯色醒目提示](../style/reveal.md)使用光線讓互動元素脫穎而出。光線照亮可與使用者互動的元素，顯示隱藏的邊框。 在部分控制項上已自動啟用顯示，例如清單檢視及格線檢視。 您可以藉由套用我們預先定義的顯色顯目提示樣式，在其他控制項上啟用它。 

* [顯色焦點](../style/reveal-focus.md)使用光線提醒您注意目前已輸入焦點的元素。  

## <a name="create-a-sense-of-depth"></a>建立深度感

我們活在立體世界中。 藉由特意將深度納入 UI，我們可以轉換扁平的 2-D 介面，透過建立視覺階層&mdash;有效率地呈現資訊。 此功能在有層次的實體環境中，重新塑造項目彼此之間的關聯

將深度新增到您的 UWP app：

* [壓克力](../style/acrylic.md)為透明材質，可讓使用者看到多層內容，建立 UI 元素階層。

* [視差](../motion/parallax.md)透過讓前景中的項目看起來移動速度比背景中的項目快，創造深度幻覺。

## <a name="incorporate-motion"></a>納入動作

可以將動作設計想成電影。 順暢的轉場能讓您專注於故事本身，讓體驗與生活結合。 我們可以將這些感覺納入我們的設計中，使用電影的輕鬆感帶領人們從一個工作移往下一項工作。

將動作新增到您的 UWP app：

* [連接動畫](../motion/connected-animation.md)可讓使用者在場景之間建立無縫轉場，維持脈絡。 

## <a name="build-it-with-the-right-material"></a>使用正確的材質建置

現實世界中圍繞著我們的萬物充滿感官性和刺激。 它們會彎曲、伸展、反彈、粉碎和滑翔。 這些材質轉化為數位環境，讓人們想要伸手去碰觸我們的設計。

將材質新增到您的 UWP app： 
        
* [壓克力](../style/acrylic.md)為透明材質，可讓使用者看到多層內容，建立 UI 元素階層。 

## <a name="design-toolkits-and-code-samples"></a>設計工具組與程式碼範例

想要開始建立您自己的應用程式與 Fluent Design 嗎？ 我們的工具組 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，以及 Sketch 有助於快速建立您的設計，以及我們的範例有助於您更快速地設計程式。

* 請查看我們的[設計工具組與範例頁面](/windows/uwp/design/downloads/)

<img src="images/fluentdesign_header.png" alt=" " />








