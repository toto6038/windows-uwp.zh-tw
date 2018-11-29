---
description: 本文章說明市集作業的應用程式和附加元件，包括應用程式內購買、 授權和自我安裝應用程式更新的常見錯誤的代碼。
title: Microsoft Store 作業的錯誤碼
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10，uwp，應用程式內購買，Iap，附加元件，錯誤碼
ms.localizationpriority: medium
ms.openlocfilehash: ba505b30076c356a39ae195e1d187cbc49d8a66a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8192948"
---
# <a name="error-codes-for-store-operations"></a>Microsoft Store 作業的錯誤碼

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

本文章說明您正在開發或測試您的應用程式中的市集相關作業時，可能會遇到的常見錯誤代碼。

## <a name="in-app-purchase-error-codes"></a>應用程式內購買錯誤碼

下列錯誤碼相關應用程式內購買的作業。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F6100   | 無法完成應用程式內購買，因為兒童專區作用中。 若要完成購買，將裝置與您的 Microsoft 帳戶登入並執行的應用程式。               |
| 0x803F6101   | 找不到指定的 App。 應用程式可能不再可用在市集中，或是您可能會有應用程式提供了錯誤的市集識別碼。     |
| 0x803F6102   | 找不到指定的附加元件。 附加元件可能不再位於可用的存放區中或您可能還提供了錯誤的市集識別碼的附加元件。                                               |
| 0x803F6103   | 找不到指定的產品。 產品可能不再可用在市集中，或是您可能會有產品所提供了錯誤的市集識別碼。                                          |
| 0x803F6104   | 無法完成應用程式內購買，因為您正在執行之應用程式的試用版。 若要完成應用程式內購買，安裝應用程式的完整版本。               |
| 0x803F6105   | 無法完成應用程式內購買，因為您無法登入您的 Microsoft 帳戶。                                              |
| 0x803F6107   | 非預期發生在處理目前的作業。                                             |
| 0x803F6108   | 無法完成應用程式內購買，因為應用程式授權遺失的資訊。 當您側載您的應用程式時，會發生這個錯誤。 若要解決此問題，解除安裝應用程式，然後從 「 市集 」 來重新整理應用程式授權重新安裝。                                          |
| 0x803F6109   | 因為指定的數量超過一個剩下的餘額，所以無法完成消費性附加元件履行。        |
| 0x803F610A   | 指定的提供者的市集中的使用者帳戶不是支援的類型。                                            |
| 0x803F610B   | 指定的市集作業不支援。                                             |
| 0x803F610C   | 應用程式不支援指定的背景工作合約。                                             |
| 0x80040001   | 提供的清單的附加元件的產品識別碼無效。                        |
| 0x80040002   | 無效的關鍵字提供的清單。                   |
| 0x80040003.   | 履行目標無效。                       |

## <a name="licensing-error-codes"></a>授權的錯誤碼

下列錯誤碼相關授權的應用程式或附加元件的作業。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F700C   | 裝置目前已離線。 若要裝置離線時，請使用此應用程式，開啟您的市集設定並切換**離線的權限**設定。            |
| 0x803F8001   | 您沒有產品的權利。 您可能會使用與用來購買產品的一個不同的 Microsoft 帳戶。           |
| 0x803F8002   | 您產品的權利已到期。           |
| 0x803F8003   | 您產品的權利處於無效狀態，防止授權建立。   |
| 0x803F8009<br/>0x803F800A   | 應用程式在試用期間已過期。   |
| 0x803F8190   |  授權不允許在目前的國家或地區，您的裝置中使用的產品。  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  您已經達到可與遊戲和應用程式從 microsoft Store 的裝置的數目上限。 若要在目前的裝置上使用此遊戲或應用程式，請先移除另一部裝置從您的帳戶。  |
| 0x803F9000<br/>0x803F9001    |  授權已過期或損毀。 為了協助解決這個錯誤，請嘗試執行[的 Windows 應用程式的疑難排解員](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps)來重設的市集快取。     |
| 0x803F9006    |  因為有資格此產品的使用者未登入時使用其 Microsoft 帳戶在裝置無法完成作業。            |
| 0x803F9008<br/>0x803F9009    |  您的裝置處於離線狀態。 您的裝置必須是線上使用此產品。            |
| 0x803F900A    |  訂閱已到期。            |


## <a name="self-install-update-error-codes"></a>自我安裝更新的錯誤碼

下列的錯誤代碼與有關[自我安裝套件更新](../packaging/self-install-package-updates.md)。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F6200   | 取得使用者同意，才能下載的套件更新。               |
| 0x803F6201   | 取得使用者同意，才能下載並安裝套件更新。                                                  |
| 0x803F6203   | 取得使用者同意，才能安裝套件更新。                                         |
| 0x803F6204   | 取得使用者同意，才能下載的套件更新，因為下載會發生在計量付費的網路連線。                                             |
| 0x803F6206   | 取得使用者同意，才能下載並安裝套件更新，因為下載會發生在計量付費的網路連線。     |


## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [啟用應用程式的訂閱附加元件](enable-subscription-add-ons-for-your-app.md)
* [實作您應用程式的試用版](implement-a-trial-version-of-your-app.md)
