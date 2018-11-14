---
title: 索引緩衝
description: 索引緩衝是包含索引資料的記憶體緩衝區，是置入頂點緩衝區的整數位移，用於呈現基本類型。
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- 索引緩衝
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0df56ebeefdbdabe5904547d77e90077549422c2
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266703"
---
# <a name="index-buffers"></a>索引緩衝


*索引緩衝*是包含索引資料的記憶體緩衝區，是置入頂點緩衝區的整數位移，用於呈現基本類型。

索引緩衝為包含索引資料的記憶體緩衝區。 索引資料或指數，是置入頂點緩衝區的整數位移，可用來呈現基本類型。

頂點緩衝包含頂點，因此，您可以使用或不使用索引的基本類型來繪製頂點緩衝。 不過，因為索引緩衝包含索引，您無法使用無對應頂點緩衝的索引緩衝。

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>索引緩衝描述


索引緩衝是依照其能力來描述，例如存在於記憶體中的哪裡、是否支援讀取和寫入，以及可包含的索引類型以數目。

索引緩衝描述會告訴您的應用程式現有緩衝區是如何建立的。 您為系統提供空描述結構，以填入先前建立的索引緩衝功能。

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>索引處理需求


索引處理作業指數的效能，大幅仰賴索引緩衝存在於記憶體中的位置，以及正在使用何種轉譯裝置。 應用程式控制索引緩衝在建立時的記憶體配置。

應用程式可以直接將索引寫入驅動程式最佳化記憶體中配置的索引緩衝。 這項技術會防止稍後的冗餘複製作業。 如果您的應用程式從索引緩衝讀回資料，這項技術無法正常運作，因為主機從驅動程式最佳化記憶體完成讀取作業的速度非常緩慢。 因此，如果應用程式在不規律地處理或將資料寫入緩衝區期間需要讀取，系統記憶體索引緩衝則是較理想的選擇。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[頂點和索引緩衝](vertex-and-index-buffers.md)

 

 




