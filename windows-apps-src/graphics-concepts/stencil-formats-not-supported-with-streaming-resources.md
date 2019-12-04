---
title: 串流資源不支援的樣板格式
description: 串流資源不支援包含樣板的格式。
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- 串流資源不支援的樣板格式
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: db6476afe265ea4a2556a5f787a14daa2e212e61
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735133"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>串流資源不支援的樣板格式


串流資源不支援包含樣板的格式。

包含樣板的格式包括 DXGI\_格式\_D24\_UNORM\_S8\_UINT （和 R24G8 系列中的相關格式）和 DXGI\_格式\_D32\_FLOAT\_S8X24\_UINT （以及 R32G8X24 系列中的相關格式）。

某些實作將深度和樣板儲存在不同的配置中，其他則將它們儲存在一起。 兩個配置的磚管理會有不同，而且單一 API 無法抽象或合理化差異。 我們建議未來硬體支援獨立深度和樣板表面，每個獨立並排顯示。

32 位元深度會有 128x128 個磚，而 8 位元樣板會有 256x256 個磚。 因此，應用程式需要接受深度與樣板之間的動態磚形狀不對齊。 但在不同的轉譯目標表面格式，已經有相同的問題。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流處理資源跨進程和裝置共用](streaming-resource-cross-process-and-device-sharing.md)

 

 




