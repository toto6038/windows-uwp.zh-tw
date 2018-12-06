---
title: 遺失裝置
description: Direct3D 裝置可能在操作狀態或遺失狀態。
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- 遺失裝置
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f0b42a10c2cdd61aef84e08d6bd4f6408a978c3
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8754147"
---
# <a name="lost-devices"></a>遺失裝置


Direct3D 裝置可能在操作狀態或遺失狀態。 *操作*狀態是一般裝置狀態，裝置如預期般執行和呈現所有轉譯。 該裝置會轉換成*遺失*狀態，例如在全螢幕應用程式中遺失鍵盤焦點，會導致不可能轉譯。 遺失狀態的特性所有轉譯操作無聲息地失敗，這表示轉譯方法可傳回成功碼，即使所有的轉譯作業失敗。

經過設計，未指定可能會導致裝置遺失的整套案例。 一些常見的範例包括焦點，例如當使用者按下 ALT + TAB 或當初始化系統對話方塊時。 裝置也可能因電源管理事件而遺失，或在另一個應用程式繼續全螢幕作業時遺失。 此外，重設裝置的失敗致使裝置進入遺失狀態。

所有衍生自[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)的方法，保證在裝置遺失之後運作。 遺失裝置之後，每項功能通常具有以下三個選項︰

-   因「裝置遺失」錯誤而失敗 - 這表示應用程式需要辨識該裝置已經遺失，以便應用程式識別未如預期的運作。
-   以無訊息模式失效、傳回 S\_OK 或任何其他傳回碼 - 如果函式以無訊息模式失敗時，應用程式通常不區分「成功」和「以無訊息模式失敗」的結果。
-   傳回一個傳回碼。

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>回應遺失裝置


遺失的裝置必須在重設後重新建立資源 (包括視訊記憶體資源)。 如果遺失裝置，應用程式會查詢裝置，查看是否可以還原到運作狀態。 若否，應用程式會等到裝置還原。

如果可以還原裝置，應用程式會藉由破壞所有影片記憶體資源和任何交換鏈來準備裝置。 重設為遺失裝置時，唯一會有作用的程序，而且應用程式只能用此方式變更裝置狀態，從遺失改為可操作。 重設將會失敗，除非應用程式發佈所有已分配的資源，包括轉譯目標和深度樣板表面。

大部分的 Direct3D 高頻呼叫不會傳回是否裝置已遺失的任何資訊。 應用程式可以繼續呼叫轉譯方法，而不會收到遺失裝置的通知。 內部將會捨棄這些作業，直到裝置重設為操作狀態。

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>鎖定作業


在內部 Direct3D 運作以確保遺失裝置之後成功鎖定作業。 不過，它並不保證鎖定作業期間影片記憶體資源的資料是準確的。 它保證不傳回錯誤碼。 這可讓應用程式寫入，不需擔心鎖定作業期間遺失裝置。

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>資源


資源可能會消耗視訊記憶體。 遺失的裝置與介面卡擁有的視訊記憶體中斷連線，所以不保證遺失裝置時的視訊記憶體配置。 如此一來，雖成功實作所有資源建立方法，但實際上僅配置空的系統記憶體。 必須先破壞任何視訊記憶體資源，再重新調整裝置大小，如此就不會有過度配置視訊記憶體的問題。 這些空的表面允許鎖定並複製作業以能正常運作，直到應用程式發現該裝置已經遺失。

必須發佈所有的視訊記憶體，才能將裝置從遺失狀態重設為可操作狀態。 在轉換到操作狀態時，其他狀態資料會自動損壞。

您可以使用單一程式碼路徑開發應用程式，以回應裝置遺失。 此程式碼路徑如果和初始啟動裝置的程式碼路徑不相同，也會很類似。

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>擷取的資料


Direct3D 可讓應用程式驗證紋理和硬體單一行程轉譯的轉譯狀態。

Direct3D 也可讓應用程式複製已產生或先前從視訊記憶體資源寫入影像至非揮發性系統記憶體資源。 這類傳輸的來源影像可能隨時會遺失，因此 Direct3D 允許在裝置遺失讓此類複製作業失敗。

遺失裝置時，因沒有主要表面，所以複製作業會失敗。 當遺失裝置時，因無法建立背景緩衝區，建立交換鏈也會失敗。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[裝置](devices.md)

 

 




