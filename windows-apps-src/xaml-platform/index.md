---
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: 本節所包含的主題說明通用 Windows 平台 (UWP) 應用程式的 XAML 架構。
title: XAML 平台
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84427f0cec6aa0921f903fff9b5e74b4da5a58a9
ms.sourcegitcommit: f8c354def02d5c82d195e4f629e6470110268223
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68623390"
---
# <a name="xaml-platform"></a>XAML 平台

本節包含說明程式設計概念的主題，這些概念通常適用於使用 C#、Visual Basic 或 Visual C++ 元件延伸 (C++/CX) 所撰寫，且使用 XAML 定義 UI 的所有應用程式。 程式設計概念包含如何使用屬性和事件，以及如何將它們套用到通用 Windows平台 (UWP) 應用程序的程式設計。 通用 Windows 平台會藉由新增相依性屬性系統，來延伸屬性及其值的 C#、Visual Basic 或 C++/CX 概念。 本節中的主題會在 UWP 使用 XAML 語言時記錄該語言，並記錄如何使用 XAML 為 UWP 應用程式定義 UI 的基本到進階方案。

| 主題 | 描述 |
|-------|-------------|
| [XAML 概觀](xaml-overview.md) | 向 Windows 執行階段應用程式開發人員介紹 XAML 語言與概念，並描述在 XAML 中宣告物件與設定屬性的不同方法，因為 XAML 可以用來建立 Windows 執行階段應用程式。 |
| [相依性屬性概觀](dependency-properties-overview.md) | 說明當您使用 C++、C# 或 Visual Basic 搭配 UI 的 XAML 定義來撰寫 Windows 執行階段應用程式時，可供使用的相依性屬性系統。 |
| [自訂相依性屬性](custom-dependency-properties.md) | 說明如何針對使用 C++、C# 或 Visual Basic 的 Windows 執行階段 app 定義及實作自訂相依性屬性。 |
| [附加屬性概觀](attached-properties-overview.md) | 說明 XAML 中附加屬性的概念並提供一些範例。 |
| [自訂附加屬性](custom-attached-properties.md) | 說明如何將 XAML 附加屬性當作相依性屬性來實作，以及如何定義讓附加屬性可以在 XAML 中使用所需的存取子慣例。 |
| [事件與路由事件概觀](events-and-routed-events-overview.md) | 描述使用 C#、Visual Basic 或 C++/CX 做為程式設計語言並使用 XAML 定義 UI 時，Windows 執行階段應用程式中事件的程式設計概念。 您可以在 XAML 中指派事件的處理常式，以做為 UI 元素宣告的一部分，或是在程式碼中新增處理常式。 Windows 執行階段支援「路由事件」  ：某些輸入事件與資料事件，可以由引發事件之物件以外的物件來處理。 當您定義控制項範本或是使用頁面或配置容器時，路由事件非常實用。 |
|[傳統型應用程式中的 UWP 控制項 (XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)| 說明如何使用 UWP XAML 控制項來增強 Windows Forms、WPF 或 Win32 傳統型應用程式的 UI。|
