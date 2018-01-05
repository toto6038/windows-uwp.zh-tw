---
title: "串流資源功能層級"
description: "Direct3D 在三個功能層級中支援串流資源。"
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords: "串流資源功能層級"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 167165d060df098125586292ef00dee8d1c48122
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="streaming-resources-features-tiers"></a>串流資源功能層級


Direct3D 在三個功能層級中支援串流資源。

第 1 層提供串流資源的基本功能。

第 2 層新增第 1 層以外的功能，例如當大小至少是一個標準磚形狀時保證非壓縮紋理 mipmap；鉗制詳細等級 (LOD) 以及取得著色器作業狀態的相關著色器指示；此外，讀取 NULL 對應磚，將取樣值視為零。

第 3 層新增 Texture3D 功能，在第 2 層以外。

Direct3D 各版本提供查詢功能，以驗證串流資源的硬體及驅動程式支援，以及在哪些層級。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


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
<td align="left"><p>[第 1 層](tier-1.md)</p></td>
<td align="left"><p>本節描述第 1 層支援。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[第 2 層](tier-2.md)</p></td>
<td align="left"><p>串流資源的第 2 層支援新增第 1 層以外的功能，例如當大小至少是一個標準磚形狀時保證非壓縮紋理 mipmap；鉗制詳細等級 (LOD) 以及取得著色器作業狀態的相關著色器指示；此外，讀取 NULL 對應磚，將取樣值視為零。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[第 3 層](tier-3.md)</p></td>
<td align="left"><p>除了[第 2 層](tier-2.md)功能，第 3 層還新增串流資源的 Texture3D 支援。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源](streaming-resources.md)

 

 




