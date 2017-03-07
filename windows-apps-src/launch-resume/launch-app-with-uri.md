---
author: PatrickFarley
title: "使用 URI 啟動 App"
description: "本節說明如何使用統一資源識別項 (URI)，從一個應用程式中啟動另一個應用程式。"
ms.assetid: a40c4ce2-4f41-4a55-aeb3-1beb3e84e839
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6dceda60b60b29f772ec4fab8b142b45cc387e75
ms.lasthandoff: 02/07/2017

---

# <a name="launch-an-app-with-a-uri"></a>使用 URI 啟動應用程式

本節說明如何使用統一資源識別項 (URI)，從一個應用程式 中啟動另一個應用程式，啟用實用的應用程式對應用程式案例。

| 主題 | 說明 |
|-------|-------------|
| [啟動 URI 的預設 App](launch-default-app.md) | 了解如何啟動統一資源識別項 (URI) 的預設 app。 URI 可讓您啟動另一個應用程式來執行特定工作。 本主題也提供許多內建於 Windows 之 URI 配置的概觀。 |
| [處理 URI 啟用](handle-uri-activation.md) | 了解如何登錄應用程式，以使它成為統一資源識別項 (URI) 配置名稱的預設處理常式。 |
| [啟動應用程式以取得結果](how-to-launch-an-app-for-results.md) | 了解如何從某個應用程式啟動另一個應用程式，以及在這兩者間交換資料的方式。 這稱為「啟動應用程式以取得結果」。 |
| [使用 ms-tonepicker URI 配置來選擇及儲存音調](launch-ringtone-picker.md) | 本主題說明 ms-tonepicker URI 配置，以及如何使用它來顯示音調選擇器以選取音調、儲存音調，以及取得音調的易記名稱。 |
| [啟動 Windows 設定應用程式](launch-settings-app.md) | 了解如何從您的應用程式啟動 Windows 設定應用程式。 本主題描述 ms-settings URI 配置。 使用此 URI 配置，可將 Windows 設定應用程式啟動到特定的設定頁面。 |
| [啟動 Windows 市集應用程式](launch-store-app.md) | 本主題描述 ms-windows-store URI 配置。 您的應用程式可以使用此 URI 配置，將 Windows 市集應用程式啟動到市集中的特定頁面。 |
| [啟動 Windows 地圖應用程式](launch-maps-app.md) | 了解如何從您的應用程式啟動 Windows 地圖應用程式。 |
| [啟動連絡人應用程式](launch-people-apps.md) | 本主題描述 ms-people URI 配置。 您的應用程式可以使用此 URI 配置來啟動連絡人應用程式，以執行特定動作。 |
| [透過應用程式 URI 處理常式支援網站至應用程式連結](web-to-app-linking.md) | 使用 app URI 處理常式，讓使用者持續使用您的 app。 |

## <a name="related-topics"></a>相關主題
* [啟動遠端裝置上的 App](launch-a-remote-app.md)
