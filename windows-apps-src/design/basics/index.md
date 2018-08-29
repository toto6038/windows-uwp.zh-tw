---
description: 了解如何設計在各種裝置與螢幕尺寸上都很容易瀏覽且看起來很棒的 UWP 應用程式並撰寫應用程式程式碼。
title: 設計基本知識
author: mijacobs
keywords: uwp app layout, universal windows platform, app design, interface, uwp app 配置, 通用 Windows 平台,應用程式設計, 介面
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d092a3fe1120dc5763b5c30ed834c1902a1f8752
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842375"
---
# <a name="design-basics-for-uwp-apps"></a>UWP app 設計基本知識

![主角圖像](images/header-design-basics.svg)

通用 Windows 平台 (UWP) 設計指導方針可以協助您設計和建立美觀、優雅的應用程式。 這不是一份詳盡規則的清單，這份文件會隨著不斷演進的 Fluent Design 系統以及我們 app 建置社群的需求改變。 

<!-- This introduction provides an overview of the Fluent Design System, UWP app design basics, and the XAML platform, helping you build user interfaces (UI) that scale beautifully across a range of devices. -->

## <a name="overview"></a>概觀

[**UWP app 設計簡介**](design-and-ui-intro.md)

與最佳做法結合的 UWP 功能簡介，其用於建立在所有執行 Windows 裝置類型上都能展現絕佳效能的 App。

[**Fluent Design 系統**](../fluent-design-system/index.md)

Fluent Design 系統展現我們建立彈性、共鳴和美觀的使用者介面的目標和原則。

## <a name="basics"></a>基本知識

[**瀏覽基本知識**](navigation-basics.md)

UWP 應用程式中的瀏覽，以瀏覽結構、瀏覽元素和系統層級功能的彈性模型為基礎。 本文會為您介紹這些元件，並示範如何搭配使用來建立最佳的瀏覽體驗。

[**命令基本知識**](commanding-basics.md)

命令元素是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。 此文件說明命令元素，例如按鈕和核取方塊、它們支援的互動，以及裝載它們的命令表面 (例如命令列和操作功能表)。

[**內容基本知識**](content-basics.md)

任何一種 app 的主要用途都是讓使用者存取內容：在相片編輯 app 中，內容是相片；在旅遊 app 中，內容是地圖與旅遊地點的相關資訊；以此類推。 本文提供三種內容案例的內容設計建議：取用、建立與互動。

## <a name="tutorials"></a>教學課程

了解如何使用 XAML 和 C# 建立基本的相片編輯應用程式。
<!-- <img src="images/landing-page/photolab-50.png" style="{height: 339px}" alt=" " /> -->

[**1. 建立基本 UI**](xaml-basics-ui.md)

使用 XAML 建立基本使用者介面。

[**2. 建立調適型配置**](xaml-basics-adaptive-layout.md)

為相片編輯應用程式設定調適型配置。

[**3. 建立自訂樣式**](xaml-basics-style.md)

建立自訂樣式，讓 UWP 控制項擁有自己的外觀和操作。