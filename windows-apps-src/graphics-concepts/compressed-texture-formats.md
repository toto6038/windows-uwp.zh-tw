---
title: 壓縮紋理格式
description: 本章節包含了壓縮紋理格式內部組織的相關資訊。
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- 壓縮紋理格式
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3171eba376911157a6ad2687fe3879df751615ac
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8468822"
---
# <a name="compressed-texture-formats"></a>壓縮紋理格式


本章節包含了壓縮紋理格式內部組織的相關資訊。 您不需要這些詳細資料即可使用壓縮紋理，因為您可以使用 Direct3D 功能對壓縮格式進行轉換。 但是，若您想要對壓縮表面資料進行直接的操作，這項資訊對您將會非常有幫助。

Direct3D 使用壓縮格式，將材質貼圖分為 4 x 4 的材質區塊。 如果紋理沒有設定任何透明度 (不透明)，或是其透明度由一個 1 位元的 Alpha 值所指定，一個 8 位元組的區塊將會代表材質貼圖區塊。 如果材質貼圖中包含了透明的材質，一個利用 Alpha 色板的 16 位元區塊將會代表它。

任何單一紋理都必須指定由 16 個材質組成的每個群組，資料將會以 64 位元或是 128 位元的方式儲存。 如果 64 位元區塊 (即 BC1 格式) 被指定用於紋理，在同樣的紋理之中，便可以每個區塊作為基礎來混合不透明物與 1 位元的 Alpha 格式。 換句話說，針對每個 16 個材質組成的區塊，color\_0 與 color\_1 之間不帶正負號的整數大小比較將會以唯一的方式進行。

使用 128 位元區塊時，整個紋理的 Alpha 色板必須指定為明確模式 (BC2 格式) 或插入模式 (BC3 格式)。 至於色彩，在選取插入模式時，您可逐區塊地選擇使用 8 個 Alpha 值插入模式或 6 個 Alpha 值插入模式。 同樣地，alpha\_0 與 alpha\_1 之間大小的比較也會逐區塊地以唯一的方式進行。

BCn 格式的間距單位以位元組計算 (而非區塊數)。 例如：如果寬度為 16，則間距將會是四個區塊。(BC1 為 4\*8，BC2 或 BC3 為 4\*16。)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[壓縮的紋理資源](compressed-texture-resources.md)

 

 




