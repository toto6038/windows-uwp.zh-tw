---
author: Mtoepke
title: "Xbox One 上的 UWP"
description: "如何在 Xbox One 上建置通用 Windows 平台 (UWP) 的應用程式。"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.openlocfilehash: 82c8fd0945ed49f8accf5e101acfbea151caa3a1
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="uwp-on-xbox-one"></a>Xbox One 上的 UWP

在 Xbox One 上建置通用 Windows 平台 (UWP) App 的入門。

Xbox One 上的 UWP 支援開發 App 和遊戲。 要實驗、建立和測試 Xbox 上的遊戲或 Apps，並不需要是開發人員計畫的一份子。 當您準備好要在 Xbox One 上發行和銷售遊戲，或是在 Windows 10 上運用 Xbox Live 時，您必須加入 [Xbox Live Creators 計劃](https://developer.microsoft.com/games/xbox/xboxlive/creator) 或成為 [ID@Xbox](http://www.xbox.com/Developers/id) 開發人員。 如需詳細資訊，請參閱[開發人員計劃概觀](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html)。

本節包括設定步驟，透過驗證程序的指南、安裝所需版本的 Visual Studio 和 Windows 10 工具的相關資訊，以及建置、執行和偵錯您第一個簡單應用程式的步驟。 

| 主題      | 描述 |
|------------|-------------|
|[開始使用](getting-started.md)| Xbox One 上的 UWP 開發入門指南。 |
|[新功能](whats-new.md)| 針對 Xbox One 上的 UWP 新功能進行重點摘要。 |
|[Xbox 最佳做法](tailoring-for-xbox.md)| 如何關閉滑鼠模式、繪製到螢幕邊緣，以及停用縮放。 |
|[已知問題](known-issues.md)| Xbox One 上的 UWP 已知問題。 |
|[常見問題集](frequently-asked-questions.md)| 與 Xbox One 上的 UWP 相關的常見問題集。 |
|[啟用 Xbox One 開發人員模式](devkit-activation.md)| 說明如何在 Xbox One 上啟用開發人員模式。 |
|[工具](introduction-to-xbox-tools.md)| 說明 Xbox One 專屬的工具「開發人員首頁」__、如何使用 Windows Device Portal，和如何針對開發設定 Visual Studio。 本節也會引導新的開發人員完成第一個 Xbox UWP 應用程式範例，並說明 Fiddler 工具的使用方式以檢視網路流量。 |
|[在 Xbox 開發環境上設定 UWP](development-environment-setup.md)| 描述設定並測試 Xbox One 開發環境的步驟。 |
|[適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源](system-resource-allocation.md)| 描述您的應用程式在 Xbox One 上執行時可以使用的資源。 | 
|[針對 Xbox 和電視進行設計](..\input-and-devices\designing-for-tv.md)| 描述設計將在 TV 上檢視並使用控制器進行輸入之 App 的最佳做法。 |  
|[多使用者應用程式的簡介](multi-user-applications.md)| 描述 Xbox One 上的多使用者應用程式 (MUA)。 |
|[範例](samples.md)| github 位置指標 (TVHelpers)，您將在其中找到有用的 XAML 和 JavaScript 範例以開始針對 Xbox 進行開發。 範例包括完整的 XAML 媒體 App 範本，以及針對網路技術的自動控制器瀏覽、多媒體播放及搜尋。 |
|[將現有的遊戲移到 Xbox](development-lanes-landing.md)|根據您建置遊戲時所使用的技術，我們可以引導您使用逐步指示，以加快使用 UWP 將遊戲移到 Xbox 的程序。|
|[在 Xbox One 上停用開發人員模式](devkit-deactivation.md)| 說明如何在 Xbox One 上停用開發人員模式。 |
|[Xbox One 上尚未支援的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  描述 Xbox One 上尚未完全運作的 UWP 功能區域。|  

## <a name="see-also"></a>另請參閱
- [Xbox One 上的 UWP App 概觀](http://go.microsoft.com/fwlink/p/?LinkId=780786) 
- [自動化啟動 Windows 10 UWP App](automate-launching-uwp-apps.md)
- [CPUSets 遊戲開發](cpusets-games.md)
  
