---
author: joannaleecy
title: DirectX 遊戲的最佳化與進階主題
description: DirectX 遊戲程式設計的最佳化與進階主題。
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 遊戲, directx, 最佳化, 多重取樣, 交換鏈結
ms.localizationpriority: medium
ms.openlocfilehash: e1a9b16dcf8c40c2b1db4af172d97009563e677a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5882184"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>DirectX 遊戲的最佳化與進階主題

本節提供有關最佳化 DirectX 遊戲效能和其他進階主題的資訊。

「遊戲的非同步程式設計」主題討論，當您使用非同步程式設計以平行處理某些元件，以及使用多執行緒以充分使用功能強大的 GPU 時，應考量的各個重點。

「處理 Direct3D 11 中的裝置已移除案例」這個主題使用逐步解說解釋使用 Direct3D 11 開發的遊戲如何偵測及回應圖形卡重設、移除或變更的案例。

「UWP app 中的多重取樣」主題概述如何使用多重取樣消除鋸齒，一種在以 Direct3D 建立的 UWP 遊戲中減少鋸齒邊緣外觀的圖形技術。

「最佳化輸入和轉譯迴圈」主題提供有關如何選擇輸入事件處理選項，以管理輸入延遲及最佳化轉譯迴圈的指引。

「透過 DXGI 1.3 交換鏈結減少延遲」主題說明如何藉由等候交換鏈結在適當時機發出訊號來開始轉譯新畫面，減少有效的畫面延遲。

「交換鏈結縮放和覆疊」主題說明如何藉由使用縮放交換鏈結，比起顯示器原生解析度，以較低解析度來轉譯即時遊戲內容，改善轉譯時間。 它也說明如何為具有硬體重疊功能的裝置，建立覆疊交換鏈結；這項技術可用來改善因使用交換鏈結縮放而縮小 UI 的問題。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">遊戲的非同步程式設計</a></p></td>
<td align="left"><p>了解如何使用 DirectX 進行非同步程式設計與執行緒處理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">處理 Direct3D 11 中的裝置已移除案例</a></p></td>
<td align="left"><p>在移除或重新初始化圖形卡之後，重建 Direct3D 與 DXGI 裝置介面鏈結。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">UWP app 中的多重取樣</a></p></td>
<td align="left"><p>在使用 Direct3D 建置的 UWP 遊戲中使用多重取樣。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">最佳化輸入和轉譯迴圈</a></p></td>
<td align="left"><p>降低輸入延遲和最佳化轉譯迴圈。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">透過 DXGI 1.3 交換鏈結減少延遲</a></p></td>
<td align="left"><p>使用 DXGI 1.3 減少有效的畫面延遲。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">交換鏈結縮放和覆疊</a></p></td>
<td align="left"><p>在行動裝置上建立縮放的交換鏈結以加快轉譯速度，以及使用覆疊交換鏈結來提高視覺品質。</p></td>
</tr>
</tbody>
</table>