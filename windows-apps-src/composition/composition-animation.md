---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 組合動畫
description: 許多組合物件和效果屬性都可藉由使用主要畫面格和運算式動畫來製作動畫效果，這些動畫可允許隨時間或根據計算來變更 UI 元素的屬性。
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b94f14b32c5dd74e0aefb9b9a99f64bbd905a05d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616703"
---
# <a name="composition-animations"></a>組合動畫

Windows.UI.Composition API 可讓您在整合 API 層中針對撰寫器物件，執行建立、產生動畫效果、轉換以及操控等作業。 組合動畫提供了功能強大且具效率的方式，可讓您在應用程式 UI 中執行動畫。 其經過徹底從頭開始的全新設計，確保讓動畫無論 UI 執行緒為何皆能以 60 FPS 運作，此外並提供優異操作彈性以協助您打造令人驚艷的使用體驗，您不僅可運用時間，更可運用輸入和其他屬性來驅動動畫。

## <a name="motion-in-windows"></a>在 Windows 中的動作

可以將動作設計想成電影。 順暢的轉場能讓您專注於故事本身，讓體驗與生活結合。 我們可以將這種感覺納入我們的設計中，使用電影的輕鬆感帶領人們從一個工作移往下一項工作。 動作通常是使用者介面和使用者體驗之間的區別係數。

Windows UI 平台的基本建置組塊，CompositionAnimations 提供功能強大且有效率的方式，來建立您的應用程式 UI 中的移動體驗。 動畫引擎具有已全新設計以確保在 60 FPS，UI 執行緒的獨立執行您的影片。 這些動畫旨在提供建置創新的移動體驗時間、 輸入和其他屬性為基礎的彈性。

### <a name="examples-of-motion"></a>動作範例

以下為一些應用程式中的動作範例。

在此，應用程式使用連接動畫讓項目有動畫效果，因為它會「持續」成為下一頁標題的一部分。 此效果有助於維持整個轉換過程的使用者內容。

![舉例說明連線的動畫](images/animation/connected-animation-example.gif)

在此，視覺視差效果會在 UI 捲動或平移時以不同的速率移動不同的物件，以產生深度、透視和移動的感覺。

![一個帶有背景影像和清單的視差範例](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>使用 CompositionAnimations 建立動作

若要在 UI 中產生動作，開發人員可以存取 XAML （連結到分鏡腳本以下） 或視覺分層中的動畫。 在視覺分層的動畫會提供開發人員提供一連串優點：

- 效能 – 而不是傳統的 UI 執行緒繫結動畫的 Windows UI 平台上的動畫會對在獨立執行緒在 60 FPS、 啟用體驗平順的動作。
- 範本化模型 – Windows UI 層中的動畫是範本，可以使用多個物件上的單一動畫意義，並將其調整屬性或參數，而不用擔心的阻礙先前使用。
- 自訂 – Windows UI 層不只會使它輕鬆製作美觀的使用者介面，但在使用完整的動畫類型，來創造嶄新又令人讚嘆的可能使用自訂的漸層

開發人員建立體驗 Windows UI 層中，您可以存取各種不同的動畫的概念就可將您的設計融入生活。 您可以使用任何這些概念的動畫屬性，或 subchannel 任何 CompositionObject 元件 （如果適用）。

> [!NOTE]
> 並非所有的 CompositionObject 屬性都可顯示動畫。 請參閱個別 CompositionObject，以判斷是否可用來建立動畫屬性的文件。

> [!NOTE]
> 詞彙_subchannel_參考屬性的元件形式。 例如，X 或 XY subchannel 的 Vector3 位移的屬性。

| 動畫概念 | 描述 |
| ----------------- | ----------- |
| [以時間為基礎的移動與 KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations 用來直接控制一段時間的移動體驗的全部內容。 描述動作的開始、 結束、 在之間插補和傳統 keyframed 方式的持續時間的開發人員。 |
| [相對的移動與 ExpressionAnimations](relation-animations.md)  | ExpressionAnimations 用來描述一個物件的屬性的動作應該如何驅動相對於另一個物件的屬性。 開發人員定義定義參考為基礎的關聯性的數學方程式。 |
| ImplicitAnimations | 這些動畫是觸發程序為基礎，並從核心應用程式邏輯分開定義。 ImplicitAnimations 用來描述動畫如何及何時發生當做回應直接屬性變更。 |
| [輸入驅動的動作，以輸入動畫](input-driven-animations.md)  | 輸入的動畫涵蓋一組可讓開發人員描述操作為基礎的動作，透過觸控或其他輸入的型態的案例。 這些動畫是驅使根據作用中的使用者輸入或筆勢。 |
| [使用 NaturalMotionAnimations 物理架構的影片](natural-animations.md)  | NaturalMotionAnimations 用來描述自然且熟悉的動作，根據真實世界體驗強制導向的動作。 而不是定義的時間，開發人員定義特性 （例如禁止的 Springs 的比率） 的動作 |