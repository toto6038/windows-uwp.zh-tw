---
author: mcleanbyron
description: 本文說明常見的存放區作業的應用程式和附加元件，包括在應用程式購買、 授權、 及自行安裝應用程式更新的錯誤碼。
title: Microsoft Store 作業的錯誤碼
ms.author: mcleans
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 在應用程式購買、 IAPs、 附加元件、 錯誤碼
ms.localizationpriority: medium
ms.openlocfilehash: 0931397e24eaba44cdf04092f5f367c43a91dd82
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "959375"
---
# <a name="error-codes-for-store-operations"></a>Microsoft Store 作業的錯誤碼

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

本文說明您可能會發生時要開發或測試您的應用程式中的儲存區相關作業的一般錯誤碼。

## <a name="in-app-purchase-error-codes"></a>在 [應用程式購買功能錯誤碼

在應用程式購買功能作業相關的下列的錯誤代碼。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F6100   | 因為小子仍無法完成在應用程式購買功能。 若要完成購買，登入您的 Microsoft 帳戶裝置並執行的應用程式。               |
| 0x803F6101   | 找不到指定的 App。 應用程式可能不再是可在 「 儲存 」、 或您可能會有提供了錯誤的存放區 ID 應用程式。     |
| 0x803F6102   | 找不到指定的附加元件。 附加元件可能不再是可在 「 儲存 」、 或您可能有提供了錯誤的儲存區識別碼的附加元件。                                               |
| 0x803F6103   | 找不到指定的產品。 產品不再中可能會提供 「 儲存 」、 或您可能會有提供了錯誤的存放區 ID 的產品。                                          |
| 0x803F6104   | 因為您正在執行試用版的應用程式無法完成在應用程式購買功能。 若要完成的應用程式購買，安裝完整版的應用程式。               |
| 0x803F6105   | 因為您沒有登入您的 Microsoft 帳戶無法完成在應用程式購買功能。                                              |
| 0x803F6107   | 未預期的某個項目發生時處理目前的作業。                                             |
| 0x803F6108   | 因為應用程式授權遺漏資訊無法完成在應用程式購買功能。 當您端載入您的應用程式時可能發生此錯誤。 若要解決此問題，請解除安裝應用程式並再重新整理應用程式授權的存放區從重新安裝。                                          |
| 0x803F6109   | 因為指定的數量超過剩餘平衡無法完成消耗附加元件履行。        |
| 0x803F610A   | 不支援的存放區的使用者帳戶的指定提供者類型。                                            |
| 0x803F610B   | 不支援指定的儲存作業。                                             |
| 0x803F610C   | 應用程式不支援指定的背景工作合約。                                             |
| 0x80040001   | 提供的清單的附加元件產品識別碼無效。                        |
| 0x80040002   | 關鍵字提供的清單無效。                   |
| 0x80040003.   | 履行目標無效。                       |

## <a name="licensing-error-codes"></a>授權的錯誤碼

下列的錯誤代碼相關授權的應用程式或附加元件的作業。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F700C   | 裝置目前已離線。 若要使用此應用程式之裝置處於離線時，開啟存放區設定，並切換**離線權限**] 設定。            |
| 0x803F8001   | 您沒有權益產品。 您可能會使用不同於購買產品所用的 Microsoft 帳戶。           |
| 0x803F8002   | 您的產品權益已過期。           |
| 0x803F8003   | 您的產品權益處於無效狀態禁止授權所建立。   |
| 0x803F8009<br/>0x803F800A   | 應用程式的試用期已經過期。   |
| 0x803F8190   |  授權不允許以供目前的國家或地區的裝置的產品。  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  您已達到可搭配遊樂場和存放區的應用程式的裝置數目上限。 若要在目前的裝置上使用此遊戲或應用程式，先移除從您的帳戶的其他裝置。  |
| 0x803F9000<br/>0x803F9001    |  授權已過期或損毀。 為了協助解決此錯誤，請嘗試執行[的 Windows 應用程式的疑難排解員](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps)重設的存放區快取。     |
| 0x803F9006    |  因為此產品有資格的使用者無法登入 Microsoft 帳戶的裝置無法完成作業。            |
| 0x803F9008<br/>0x803F9009    |  您的裝置已離線。 您的裝置必須能夠線上使用此產品。            |
| 0x803F900A    |  訂閱已過期。            |


## <a name="self-install-update-error-codes"></a>自我安裝更新錯誤碼

下列的錯誤代碼相關[自我安裝套件更新](../packaging/self-install-package-updates.md)。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F6200   | 使用者同意才能下載套件更新。               |
| 0x803F6201   | 使用者同意才下載並安裝套件更新。                                                  |
| 0x803F6203   | 使用者同意才可安裝套件更新。                                         |
| 0x803F6204   | 使用者同意才能下載套件更新因為下載就會發生在 metered 的網路連線。                                             |
| 0x803F6206   | 使用者同意才下載並安裝套件更新因為下載就會發生在 metered 的網路連線。     |


## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [啟用應用程式的訂閱附加元件](enable-subscription-add-ons-for-your-app.md)
* [實作您應用程式的試用版](implement-a-trial-version-of-your-app.md)
