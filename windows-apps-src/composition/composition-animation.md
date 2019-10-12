---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 組合動畫
description: 許多組合物件和效果屬性都可藉由使用主要畫面格和運算式動畫來製作動畫效果，這些動畫可允許隨時間或根據計算來變更 UI 元素的屬性。
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281830"
---
# <a name="composition-animations"></a>組合動畫

Windows.UI.Composition API 可讓您在整合 API 層中針對撰寫器物件，執行建立、產生動畫效果、轉換以及操控等作業。 組合動畫提供了功能強大且具效率的方式，可讓您在應用程式 UI 中執行動畫。 其經過徹底從頭開始的全新設計，確保讓動畫無論 UI 執行緒為何皆能以 60 FPS 運作，此外並提供優異操作彈性以協助您打造令人驚艷的使用體驗，您不僅可運用時間，更可運用輸入和其他屬性來驅動動畫。

## <a name="motion-in-windows"></a>Windows 中的動作

可以將動作設計想成電影。 順暢的轉場能讓您專注於故事本身，讓體驗與生活結合。 我們可以將這種感覺納入我們的設計中，使用電影的輕鬆感帶領人們從一個工作移往下一項工作。 動作通常是使用者介面與使用者體驗之間的區別因素。

CompositionAnimations 是 Windows UI 平臺的基本建立組塊，提供強大且有效率的方式，在應用程式的 UI 中建立動作體驗。 動畫引擎已從頭開始設計，以確保您的動作會在 60 FPS 執行，與 UI 執行緒無關。 這些動畫的設計目的是為了根據時間、輸入和其他屬性，提供彈性來建立創新的行動體驗。

### <a name="examples-of-motion"></a>動作範例

以下為一些應用程式中的動作範例。

在此，應用程式使用連接動畫讓項目有動畫效果，因為它會「持續」成為下一頁標題的一部分。 此效果有助於維持整個轉換過程的使用者內容。

![連接的動畫範例](images/animation/connected-animation-example.gif)

在此，視覺視差效果會在 UI 捲動或平移時以不同的速率移動不同的物件，以產生深度、透視和移動的感覺。

![一個帶有背景影像和清單的視差範例](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>使用 CompositionAnimations 建立動作

若要在 UI 中產生動作，開發人員可以存取 XAML 或視覺效果層中的動畫。 視覺階層式動畫為開發人員提供一系列的優點：

- 效能– Windows UI 平臺上的動畫不是傳統的 UI 執行緒系結動畫，而是在獨立的執行緒上操作 60 FPS，以實現順暢的運動體驗。
- 範本化模型– Windows UI 層中的動畫是範本，這表示可以在多個物件上使用單一動畫，並調整屬性或參數，而不需要擔心先前的用法。
- 自訂– Windows UI 層不僅可讓您輕鬆地製作美觀的 UI，還能使用完整的動畫類型，透過自訂漸層來建立全新且驚人的體驗

身為開發人員在 Windows UI 層建立經驗，您可以存取各種動畫概念，讓您的設計變得更生動。 您可以使用任何一種概念，以動畫顯示任何 CompositionObject 的屬性或 subchannel 元件（如果適用）。

> [!NOTE]
> 並非 CompositionObject 的所有屬性都是 animatable。 請參閱個別 CompositionObject 的檔，以判斷屬性是否為 animatable。

> [!NOTE]
> 「詞彙」（ _subchannel_ ）一詞指的是屬性（property）的元件形式。 例如，Vector3 Offset 屬性的 X 或 XY subchannel。

| 動畫概念 | 描述 |
| ----------------- | ----------- |
| [以時間為基礎的 KeyFrameAnimations 動作](time-animations.md)  | KeyFrameAnimations 是用來直接控制一段時間內的整個動作體驗。 程式開發人員以傳統的 keyframed 方式來描述動作的開始、結束、插補，以及持續時間。 |
| [ExpressionAnimations 的相對運動](relation-animations.md)  | ExpressionAnimations 是用來描述某個物件屬性的動作如何相對於另一個物件的屬性來驅動。 開發人員會定義數學方程式，以定義以參考為基礎的關聯性。 |
| ImplicitAnimations | 這些動畫是以觸發程式為基礎，並與核心應用程式邏輯分開定義。 ImplicitAnimations 是用來描述動畫發生的方式和時機，做為直接屬性變更的回應。 |
| [具有輸入動畫的輸入導向動作](input-driven-animations.md)  | 輸入動畫涵蓋一組案例，可讓開發人員透過觸控或其他輸入形式來描述以操作為基礎的動作。 這些動畫是根據作用中的使用者輸入或手勢來驅動。 |
| [以物理為基礎的動作與 NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations 是用來根據實際的強制導向動作來描述自然且熟悉的動作體驗。 開發人員不會定義時間，而是定義動作的特性（例如，彈簧的阻尼比例） |