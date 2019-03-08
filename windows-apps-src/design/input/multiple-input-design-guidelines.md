---
Description: 就如同人們彼此聯繫時會使用語音和手勢的組合一樣，多種類型與模式的輸入在與應用程式互動時相當有用。
title: 多重輸入設計指導方針
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c67430680854e7940d12af15ecd3c07dcd976802
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601293"
---
# <a name="multiple-inputs"></a>多重輸入


就如同人們彼此聯繫時會使用語音和手勢的組合一樣，多種類型與模式的輸入在與應用程式互動時相當有用。


為了儘可能配合較多的使用者和裝置，建議您將應用程式設計成可以與越多的輸入類型 (手勢、語音、觸控、觸控板、滑鼠及鍵盤) 搭配使用越好。 這樣會將彈性、可用性及可存取性最大化。

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



