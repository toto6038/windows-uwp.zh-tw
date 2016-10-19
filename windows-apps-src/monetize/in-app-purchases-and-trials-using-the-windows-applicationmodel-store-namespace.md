---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "了解如何在目標為 Windows 10 版本 1607 之前版本的 UWP app 中啟用 App 內購買和試用版。"
title: "使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 649d082cddcf301fe602a5ab99637ad7bea67d49

---

# 使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版

Windows SDK 提供 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間中的成員，讓您可用來將 App 內購買和試用版功能新增到通用 Windows 平台 (UWP) App，以協助您的 App 獲利並加入新功能。 這些 API 也會提供您 App 授權資訊的存取。

>**注意**&nbsp;&nbsp;如果您 App 的目標為 Windows 10 版本 1607 或更新版本，則我們建議您使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間的成員，而不是使用 **Windows.ApplicationModel.Store** 命名空間。 **Windows.Services.Store** 命名空間支援最新的附加元件類型 (例如市集管理的消費性附加元件)，而設計目的是與 Windows 開發人員中心和市集所支援的未來產品與功能類型相容。 **Windows.Services.Store** 命名空間的設計也具有較佳的效能。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

本節中的文章提供深入的指引與程式碼範例，其中將針對數個常見的案例，使用 **Windows.ApplicationModel.Store** 命名空間中的成員。 如需 UWP app 中 App 內購買相關概念的概觀，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

如需示範如何使用 **Windows.ApplicationModel.Store** 命名空間實作試用版和 App 內購買的完整範例，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)。

## 本節內容


| 主題                                                                                                       | 描述                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [啟用應用程式內產品購買](enable-in-app-product-purchases.md)      |  無論您的 app 是否免費，都可以直接從 app 內銷售內容、其他 app 或新的 app 功能 (例如解除鎖定遊戲的下一個關卡)。 以下示範如何在 App 中啟用這些產品。  |
| [啟用消費性應用程式內產品購買](enable-consumable-in-app-product-purchases.md)      | 您可以透過市集商業平台提供消費性的應用程式內產品 (亦即可購買、使用，然後再次購買的項目)，為客戶提供既健全又可靠的購買體驗。 這對於像遊戲內貨幣 (金幣、錢幣等) 這種可在買來後用來購買特定火力升級配備的東西，特別有用。 |
| [在試用版本中排除或限制某些功能](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 如果您讓客戶在試用期間免費使用 App，您可以在試用期間排除或限制某些功能，吸引客戶升級成完整版的 App。 |
| [管理大型的 App 內產品型錄](manage-a-large-catalog-of-in-app-products.md)      |   如果您的 App 提供大型的 App 內產品型錄，您可以選擇性地依照本主題中描述的程序來協助管理型錄。    |
| [使用收據來驗證產品購買](use-receipts-to-verify-product-purchases.md)      |   成功購買產品時所產生的每個 Windows 市集交易可以選擇性地傳回交易收據，以便提供所列出產品的相關資訊與客戶的貨幣成本。 如果您的 App 需要確認使用者已購買 App，或是已從 Windows 市集進行 App 內產品購買，這項資訊的存取將可支援這些情況。 |



<!--HONumber=Aug16_HO5-->


