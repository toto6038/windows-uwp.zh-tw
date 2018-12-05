---
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: 效能
description: 使用者會期望其應用程式保持回應性，並可自在地使用，而不會耗盡電池。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b65a62c2a6182e3b120f8ae8cb6b5fe3a0bf45aa
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8732510"
---
# <a name="performance"></a>效能


使用者會期望其 app 保持回應性，並可自在地使用，而不會耗盡電池。 在技術上來說，效能是非功能的需求，但是將效能視為功能可協助您滿足使用者的期望。 指定目標和測量是主要因素。 決定您的效能關鍵案例有哪些；定義良好效能所代表的意義。 然後在整個專案週期中及早並經常進行測量，以確保您能夠達成目標。 本節說明如何組織您的效能工作流程、修正動畫問題和畫面播放速率問題，以及微調您的啟動時間、頁面瀏覽時間和記憶體使用量。

如果您還沒有這麼做，在步驟，我們已經看過能夠大幅提升效能的結果是只是您將 app 移植到目標 windows 10。 只在 windows 10 應用程式中提供數個 XAML 最佳化 (例如， [{X:bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783))。 請參閱[移植到 windows 10 的應用程式](https://msdn.microsoft.com/library/windows/apps/Mt238321)和 //build/ 工作階段[移動到通用 Windows 平台](http://channel9.msdn.com/Events/Build/2015/3-741)。

| 主題 | 說明 |
|-------|-------------|
| [規劃效能](planning-and-measuring-performance.md) | 使用者會期望其 app 保持回應性，並可自在地使用，而不會耗盡電池。 在技術上來說，效能是非功能的需求，但是將效能視為功能可協助您滿足使用者的期望。 指定目標和測量是主要因素。 決定您的效能關鍵案例是什麼；定義良好效能代表什麼意義。 然後在整個專案週期中及早並經常進行測量，以確保您能夠達成目標。 |
| [最佳化背景活動](optimize-background-activity.md) | 建立 UWP app，與系統一同以省電方式來使用背景工作。 |
| [ListView 與 GridView UI 最佳化](optimize-gridview-and-listview.md) | 透過 UI 虛擬化、減少元素以及漸進式更新項目，改善 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) 效能和啟動時間。 |
| [ListView 和 GridView 資料虛擬化](listview-and-gridview-data-optimization.md) | 透過資料虛擬化改善 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) 效能和啟動時間。 |
| [改善記憶體回收效能](improve-garbage-collection-performance.md) | 使用 C# 和 Visual Basic 撰寫的通用 Windows 平台 (UWP) app 會從 .NET 記憶體回收行程自動管理記憶體。 本節摘要說明 UWP app 中的 .NET 記憶體回收行程的行為和效能最佳做法。 |
| [讓 UI 執行緒保持回應](keep-the-ui-thread-responsive.md) | 不論使用何種電腦，使用者都希望 app 在進行計算時仍然能夠回應。 這對於不同的 app 有不同的意義。 對某些 app 而言，這表示提供更實際的物理性質、更快地從磁碟或網路載入資料、快速地呈現複雜的場景和在頁面之間瀏覽、立即找到方向，或是快速處理資料。 不論執行何種計算，使用者都會希望 app 可以回應輸入，不希望有看似在「思考」&quot;&quot;而停止回應的情況發生。 |
| [最佳化您的 XAML 標記](optimize-xaml-loading.md) | 剖析 XAML 標記以在記憶體建構物件，對複雜 UI 而言很耗費時間。 以下是一些您可以執行的動作，以針對您的 app 改善 XAML 標記剖析和載入時間及記憶體效率。 | 
| [最佳化您的 XAML 版面配置](optimize-your-xaml-layout.md) | 在 CPU 使用量與記憶體負荷方面，版面配置可說是 XAML app 中高度耗費資源的一部分。 您可以採取下列一些簡單步驟來提升 XAML app 的版面配置效能。 | 
| [MVVM 和語言效能祕訣](mvvm-performance-tips.md) | 本主題討論與您的軟體設計模式及程式設計語言選擇相關的一些效能考量。 |
| [App 啟動效能的最佳做法](best-practices-for-your-app-s-startup-performance.md) | 透過改善處理啟動和啟用的方式，建立具有最佳啟動時間的 UWP app。 |
| [最佳化動畫、媒體及影像](optimize-animations-and-media.md) | 建立具有流暢的動畫、高畫面播放速率和高效能媒體擷取和播放功能的通用 Windows 平台 (UWP) app。 |
| [最佳化暫停/繼續](optimize-suspend-resume.md) | 建立可更有效利用處理程序生命週期系統，以便在暫停或終止之後有效率地繼續執行的 UWP app。 |
| [最佳化檔案存取](optimize-file-access.md) | 建立可有效存取檔案系統的 UWP app，避免因為磁碟延遲和記憶體/CPU 週期而發生效能問題。 |
| [Windows 執行階段元件和最佳化互通性](windows-runtime-components-and-optimizing-interop.md) | 建立使用 UWP 元件和原生與 Managed 類型之間的互通性，同時可避免互通性效能問題的 UWP app。 |
| [分析和效能的工具](tools-for-profiling-and-performance.md) | Microsoft 提供數種可協助您改善 UWP app 效能的工具。|

