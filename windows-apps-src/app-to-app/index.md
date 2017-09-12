---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: "本節說明如何在通用 Windows 平台 (UWP) 應用程式之間分享資料，包括如何使用分享協定、複製並貼上，以及拖放。"
title: "應用程式間通訊"
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 938c2d20067dc99a84939c8501971a06fa702515
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/19/2017
---
# <a name="app-to-app-communication"></a>應用程式間通訊

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節說明如何在通用 Windows 平台 (UWP) 應用程式之間分享資料，包括如何使用分享協定、複製並貼上，以及拖放。

分享協定是一種使用者可以在 App 之間快速交換資料的方法。 舉例來說，使用者可能會想要使用社交網路 app 與朋友分享網頁，或是將連結儲存在筆記本 app 以供日後參考。 如果您 App 接收的內容是使用者可以在另一個 App 環境中快速完成的內容，請考慮使用分享協定。

App 可以用兩種方式支援分享功能。 第一種，app 可以是「來源 app」，提供使用者要分享的內容。 第二種，app 可以是「目標 app」，使用者選取做為分享內容的目的地 App 可以既是來源 App 也是目標 App。 如果您希望將 App 當作來源 App 來分享內容，您需要決定 App 可以提供的資料格式。

除了分享協定之外，App 也可以整合傳統的資料傳輸技術，例如拖放或複製並貼上。 除了 UWP app 之間的通訊之外，這些方法也支援分享至傳統型應用程式及從傳統型應用程式分享。



## <a name="in-this-section"></a>本節內容

| 主題 | 描述 |
|-------|-------------|
| [分享資料](share-data.md) | 本文說明如何在 UWP app 中支援分享協定。 分享協定是一種在 app 之間快速分享資料 (例如文字、連結，照片和影片) 的簡單方法。 舉例來說，使用者可能會想要使用社交網路 app 與朋友分享網頁，或是將連結儲存在筆記本 app 以供日後參考。 |
| [接收資料](receive-data.md) | 本文說明如何使用分享協定在您的 UWP app 中接收從另一個 app 分享的內容。 分享協定可以在使用者叫用分享時，讓您的 app 成為一個選項。 |
| [複製和貼上](copy-and-paste.md) | 本文說明如何使用剪貼簿在 UWP app 中支援複製和貼上。 複製和貼上是在 App 間 (或是 App 內) 交換資料的傳統方式，而且幾乎每個 App 在某種程度上都能支援剪貼簿作業。 |
| [拖放](drag-and-drop.md) | 本文說明如何在您的 UWP app 中新增拖放功能。 拖放是一種與影像和檔案之類的內容進行互動的傳統、原始方式。 實作之後，不論向哪一個方向拖放都能順暢運作，包括應用程式之間、應用程式到傳統型應用程式，以及傳統型應用程式到應用程式。 |

## <a name="see-also"></a>另請參閱
- [開發 UWP app](https://developer.microsoft.com/windows/develop)
