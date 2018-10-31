---
author: Karl-Bridge-Microsoft
Description: Just as people use a combination of voice and gesture when communicating with each other, multiple types and modes of input can also be useful when interacting with an app.
title: 多重輸入設計指導方針
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 670caf5c519fcf1b7ff57b47781541d98358aa50
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5825879"
---
# <a name="multiple-inputs"></a>多重輸入


就如同人們彼此聯繫時會使用語音和手勢的組合一樣，多種類型與模式的輸入在與 app 互動時相當有用。


為了儘可能配合多個使用者和裝置，我們建議將 app 設計為可以使用各種輸入類型 (手勢、語音、觸控、觸控板、滑鼠與鍵盤)。 這樣會最大化彈性、可用性及可存取性。

若要開始，請考慮您的 app 處理輸入的各種案例。 嘗試在您的 app 保持一致，並記住平台控制項提供多個輸入類型的內建支援。

-   使用者可以透過多種輸入裝置與應用程式互動嗎？
-   所有輸入法是否隨時受到支援？ 使用特定控制項？ 在特定時間或情況？
-   某個輸入法的優先順序是否較高？

## <a name="single-or-exclusive-mode-interactions"></a>單一 (或專屬) 模式互動


使用單一模式互動，支援多個輸入類型，但是只有一個可以用於每個動作。 例如，命令的語音辨識及進行瀏覽的手勢；或根據鄰近性使用觸控或手勢的文字輸入。

## <a name="multimodal-interactions"></a>多樣式互動

使用多樣式互動，會使用序列中的多個輸入法完成一個動作。

語音 + 手勢  
使用者指向產品，然後說出「新增到購物車」。

語音 + 觸控  
使用者以長按的方式選取相片，然後說「傳送相片」。



