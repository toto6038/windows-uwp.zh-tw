---
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: 本節所包含的主題說明通用 Windows 平台 (UWP) app 的 XAML 架構。
title: XAML 平台
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b725a823f31309c2419bcdc5095a78994d1929c0
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7696809"
---
# <a name="xaml-platform"></a>XAML 平台


本節包含的主題說明程式設計概念通用於任何應用程式會在寫入時，如果您使用 C#、 Microsoft Visual Basic 中，或 VisualC + + 元件延伸 (C + + /CX) 做為程式設計語言並使用您的 UI 的 XAML定義。 這包含基本的程式設計概念，例如使用屬性和事件，以及如何將它們套用到通用 Windows 平台 (UWP) 應用程式的程式設計。 通用 Windows 平台 (UWP) 會藉由新增相依性屬性系統，來延伸屬性及其值的 C#、Visual Basic 或 C++/CX 概念。 本節中的主題也會在 UWP 使用 XAML 語言時記錄該語言，並且涵蓋基本案例和進階主題，以說明如何使用 XAML 來定義適用於 UWP app 的 UI。

| 主題 | 說明 |
|-------|-------------|
| [XAML 概觀](xaml-overview.md) | 我們將向 Windows 執行階段 app 開發人員介紹 XAML 語言與 XAML 概念，並描述在 XAML 中宣告物件與設定屬性的不同方法，因為 XAML 可以用來建立 Windows 執行階段 app。 |
| [相依性屬性概觀](dependency-properties-overview.md) | 這個主題說明當您使用 C++、C# 或 Visual Basic 搭配 UI 的 XAML 定義來撰寫 Windows 執行階段應用程式時，可供使用的相依性屬性系統。 |
| [自訂相依性屬性](custom-dependency-properties.md) | 說明如何針對使用 C++、C# 或 Visual Basic 的 Windows 執行階段 app 定義及實作自訂相依性屬性。 |
| [附加屬性概觀](attached-properties-overview.md) | 說明 XAML 中附加屬性的概念並提供一些範例。 |
| [自訂附加屬性](custom-attached-properties.md) | 說明如何將 XAML 附加屬性當作相依性屬性來實作，以及如何定義讓附加屬性可以在 XAML 中使用所需的存取子慣例。 |
| [事件與路由事件概觀](events-and-routed-events-overview.md) | 我們將描述使用 C#、Visual Basic 或 C++/CX 做為程式設計語言並使用 XAML 定義 UI 時，Windows 執行階段 app 中事件的程式設計概念。 您可以在 XAML 中指派事件的處理常式，以做為 UI 元素宣告的一部分，或是在程式碼中新增處理常式。 Windows 執行階段支援**路由事件**：某些輸入事件與資料事件，可以由引發事件之物件以外的物件來處理。 當您定義控制項範本或是使用頁面或配置容器時，路由事件非常實用。 |
|[在 WPF 和 Windows Form 應用程式中裝載 UWP 控制項](xaml-host-controls.md)| 說明如何使用 UWP XAML 控制項來增強 Windows Forms 或 WPF 傳統型應用程式的 UI。|
