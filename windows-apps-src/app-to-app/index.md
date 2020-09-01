---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: 本節說明如何在通用 Windows 平台 (UWP) 應用程式之間分享資料，包括如何使用分享協定、複製並貼上，以及拖放。
title: 應用程式間通訊
ms.date: 02/12/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db6cf7937e0d757d946154a4c87e7e4da3fde2cb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175752"
---
# <a name="app-to-app-communication"></a>應用程式間通訊

本節說明如何在通用 Windows 平台 (UWP) 應用程式之間共用資料，包括如何使用分享協定、複製並貼上、拖放以及應用程式服務。

分享協定是一種使用者可以在 App 之間快速交換資料的方法。 舉例來說，使用者可能會想要使用社交網路 app 與朋友分享網頁，或是將連結儲存在筆記本 app 以供日後參考。 如果您 App 接收的內容是使用者可以在另一個 App 環境中快速完成的內容，請考慮使用分享協定。

App 可以用兩種方式支援分享功能。 第一種，App 可以是[來源 app，提供使用者要分享的內容](share-data.md)。 第二種，App 可以是[目標 app，使用者選取做為分享內容的目的地](receive-data.md)。 App 可以既是來源 App 也是目標 App。 如果您希望將 App 當作來源 App 來分享內容，您需要決定 App 可以提供的資料格式。

除了分享協定之外，App 也可以整合傳統的資料來傳輸技術，例如[拖放](../design/input/drag-and-drop.md)或[複製並貼上](copy-and-paste.md)。 除了 UWP app 之間的通訊之外，這些方法也支援分享至傳統型應用程式及從傳統型應用程式分享。

UWP 應用程式也可以建立[應用程式服務](../launch-resume/how-to-create-and-consume-an-app-service.md)，以提供其他 UWP 應用程式的功能。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。

## <a name="in-this-section"></a>本節內容

| 主題 | 描述 |
|-------|-------------|
| [共用資料](share-data.md) | 本文說明如何在 UWP app 中支援分享協定。 分享協定是一種在 app 之間快速分享資料 (例如文字、連結，照片和影片) 的簡單方法。 舉例來說，使用者可能會想要使用社交網路 app 與朋友分享網頁，或是將連結儲存在筆記本 app 以供日後參考。 |
| [接收資料](receive-data.md) | 本文說明如何使用分享協定在您的 UWP app 中接收從另一個 app 分享的內容。 分享協定可以在使用者叫用分享時，讓您的應用程式成為一個選項。 |
| [複製和貼上](copy-and-paste.md) | 本文說明如何使用剪貼簿在 UWP app 中支援複製和貼上。 複製和貼上是在 App 間 (或是 App 內) 交換資料的傳統方式，而且幾乎每個 App 在某種程度上都能支援剪貼簿作業。 |
| [拖放功能](../design/input/drag-and-drop.md) | 本文說明如何在您的 UWP app 中新增拖放功能。 拖放是一種與影像和檔案之類的內容進行互動的傳統、原始方式。 實作之後，拖放不論以哪一個方向都能順暢運作，包括應用程式間、應用程式到傳統型應用程式，以及傳統型應用程式到應用程式。 |
| [建立和使用應用程式服務](../launch-resume/how-to-create-and-consume-an-app-service.md) | 本文說明如何在 UWP 應用程式中建立應用程式服務，以提供服務給其他 UWP 應用程式。  |

## <a name="see-also"></a>另請參閱

- [開發 UWP 應用程式](../develop/index.md)