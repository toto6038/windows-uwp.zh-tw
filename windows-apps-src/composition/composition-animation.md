---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 組合動畫
description: 許多組合物件和效果屬性都可藉由使用主要畫面格和運算式動畫來製作動畫效果，這些動畫可允許隨時間或根據計算來變更 UI 元素的屬性。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 38f9d0daf230007d1d32a7d2187d54baa90986e5
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7293291"
---
# <a name="composition-animations"></a>組合動畫

Windows.UI.Composition API 可讓您在整合 API 層中針對撰寫器物件，執行建立、產生動畫效果、轉換以及操控等作業。 組合動畫提供了功能強大且具效率的方式，可讓您在應用程式 UI 中執行動畫。 其經過徹底從頭開始的全新設計，確保讓動畫無論 UI 執行緒為何皆能以 60 FPS 運作，此外並提供優異操作彈性以協助您打造令人驚艷的使用體驗，您不僅可運用時間，更可運用輸入和其他屬性來驅動動畫。

## <a name="motion-in-windows"></a>在 Windows 中的動作

可以將動作設計想成電影。 順暢的轉場能讓您專注於故事本身，讓體驗與生活結合。 我們可以邀請該感覺納入我們的設計，使用電影到下前置人們從一個工作。 動作通常是使用者介面和使用者體驗之間的區別因素。

Windows UI 平台的基礎建置組塊，CompositionAnimations 提供強大且有效率的方式，來建立您的應用程式 UI 中的動作體驗。 動畫引擎已被從頭開始設計向上以確保在 60 FPS 的 UI 執行緒獨立執行您的動作。 這些動畫是設計用來提供要建置創新的動作體驗，根據時間、 輸入和其他屬性的彈性。

### <a name="examples-of-motion"></a>動作範例

以下為一些應用程式中的動作範例。

在此，應用程式使用連接動畫讓項目有動畫效果，因為它會「持續」成為下一頁標題的一部分。 此效果可幫助使用者在轉換之間維持脈絡。

![連接動畫範例](images/animation/connected-animation-example.gif)

在此，視覺視差效果會在 UI 捲動或平移時以不同的速率移動不同的物件，以產生深度、透視和移動的感覺。

![一個帶有背景影像和清單的視差範例](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>使用 CompositionAnimations，以建立動作

若要產生動作在 UI 中，開發人員可以存取 XAML （連結到 storyboard （腳本） 以下） 或視覺層中的動畫。 在視覺層的動畫提供開發人員使用一系列的優點：

- 在啟用順暢的動作體驗 60 FPS 的獨立執行緒上運作效能 – 而不是傳統 UI 執行緒繫結動畫，在 Windows UI 平台上的動畫。
- 範本化模型 – Windows UI 層中的動畫是範本、 意義可以在多個物件上使用單一動畫並調整屬性或參數，而不用妨礙先前使用。
- 自訂 – Windows UI 層不只可讓您輕鬆進行美觀的 UI，但搭配動畫類型的完整範圍，來建立新的和酷炫的可能使用的自訂項目漸層的體驗

身為開發人員，建立在 Windows UI 層的體驗，您可以存取各種不同的動畫概念，讓您的設計得活靈活現。 您可以使用任何這些概念的屬性產生動畫效果，或 subchannel 任何 CompositionObject 的元件 （當適用的話）。

> [!NOTE]
> 並非所有在 CompositionObject 的屬性都可產生動畫效果。 請參閱個別的 CompositionObject 以判斷是否為可產生動畫效果屬性的文件。

> [!NOTE]
> 詞彙_subchannel_指的是屬性的元件格式。 例如，X 或 XY subchannel Vector3 Offset 屬性。

| 動畫概念 | 描述 |
| ----------------- | ----------- |
| [使用 KeyFrameAnimations 時間為基礎的動作](time-animations.md)  | KeyFrameAnimations 用來直接控制的一段時間的動作體驗全部內容。 描述動作的開始、 結束、 在之間內插補點和傳統 keyframed 方式 duration 開發人員。 |
| [使用 ExpressionAnimations 相對的動作](relation-animations.md)  | ExpressionAnimations 用來描述物件的屬性的動作應該如何導向相對於另一個物件的屬性。 開發人員定義的數學方程式，定義參考為基礎的關係。 |
| ImplicitAnimations | 這些動畫會觸發程序為基礎，並從核心應用程式邏輯分別定義。 ImplicitAnimations 用來描述動畫如何及何時發生為直接存取的屬性變更的回應。 |
| [輸入導向的動作，使用輸入動畫](input-driven-animations.md)  | 輸入的動畫涵蓋了一組可以讓開發人員描述操作為基礎的動作，透過觸控或其他輸入的形式的案例。 這些動畫都驅動根據作用中使用者輸入或手勢。 |
| [使用 NaturalMotionAnimations 物理原理的動作](natural-animations.md)  | NaturalMotionAnimations 用來描述自然且熟悉的動作，根據真實世界的體驗強制導向的動作。 而不是定義的時間，開發人員定義特性 (例如彈簧的 damping ratio) 的動作 |