---
description: 本文說明 Microsoft Store 中應用程式和附加元件相關作業 (包括 App 內購買、授權及自行安裝應用程式更新) 的常見錯誤碼。
title: Microsoft Store 作業的錯誤碼
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10, uwp, App 內購買, IAP, 附加元件, 錯誤碼
ms.localizationpriority: medium
ms.openlocfilehash: ba505b30076c356a39ae195e1d187cbc49d8a66a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662873"
---
# <a name="error-codes-for-store-operations"></a>Microsoft Store 作業的錯誤碼

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

本文說明當您在應用程式中開發或測試 Microsoft Store 相關作業時，可能會遇到的常見錯誤碼。

## <a name="in-app-purchase-error-codes"></a>App 內購買錯誤碼

下列錯誤碼與 App 內購買作業相關。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F6100   | 由於兒童專區使用中，因此無法完成 App 內購買。 若要完成購買，請使用您的 Microsoft 帳戶登入裝置並再次執行應用程式。               |
| 0x803F6101   | 找不到指定的 App。 應用程式可能不再於 Microsoft Store 中提供，或者您可能提供了應用程式的錯誤 Microsoft Store 識別碼。     |
| 0x803F6102   | 找不到指定的附加元件。 附加元件可能不再於 Microsoft Store 中提供，或者您可能提供了附加元件的錯誤 Microsoft Store 識別碼。                                               |
| 0x803F6103   | 找不到指定的產品。 產品可能不再於 Microsoft Store 中提供，或者您可能提供了產品的錯誤 Microsoft Store 識別碼。                                          |
| 0x803F6104   | 因為您正在執行應用程式的試用版，因此無法完成 App 內購買。 若要完成 App 內購買，請安裝應用程式的完整版。               |
| 0x803F6105   | 因為您未登入您的 Microsoft 帳戶，因此無法完成 App 內購買。                                              |
| 0x803F6107   | 處理目前的作業時發生意外的狀況。                                             |
| 0x803F6108   | 因為應用程式授權遺失資訊，因此無法完成 App 內購買。 當您側載應用程式，就會發生這個錯誤。 若要解決此問題，請解除安裝應用程式，再從 Microsoft Store 重新安裝以重新整理應用程式授權。                                          |
| 0x803F6109   | 由於指定的數量超過剩餘的餘額，因此消費性附加元件履行無法完成。        |
| 0x803F610A   | 對於 Microsoft Store 使用者帳戶，不支援指定的提供者類型。                                            |
| 0x803F610B   | 不支援指定的 Microsoft Store 作業。                                             |
| 0x803F610C   | 應用程式不支援指定的背景工作合約。                                             |
| 0x80040001   | 提供的附加元件產品識別碼清單無效。                        |
| 0x80040002   | 提供的關鍵字清單無效。                   |
| 0x80040003   | 履行目標無效。                       |

## <a name="licensing-error-codes"></a>授權錯誤碼

下列錯誤碼與應用程式或附加元件的授權作業相關。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F700C   | 裝置目前離線。 若要在離線裝置時使用此應用程式，請開啟 Microsoft Store 設定並切換 **\[離線權限\]** 設定。            |
| 0x803F8001   | 您沒有產品的權利。 您使用的 Microsoft 帳戶可能不同於用來購買產品的帳戶。           |
| 0x803F8002   | 您的產品權利已過期。           |
| 0x803F8003   | 您的產品權利處於無效狀態，因此無法建立授權。   |
| 0x803F8009<br/>0x803F800A   | 應用程式的試用期間已到期。   |
| 0x803F8190   |  授權不允許在您裝置目前的國家/地區使用產品。  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  您已經達到可使用 Microsoft Store 中遊戲與應用程式的裝置數目上限。 若要在目前裝置上使用此遊戲或應用程式，請先從您的帳戶移除其他裝置。  |
| 0x803F9000<br/>0x803F9001    |  授權已過期或損壞。 若要解決這個錯誤，請嘗試在執行[針對 Windows 應用程式進行疑難排解](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps)重設存放區快取。     |
| 0x803F9006    |  由於有權使用此產品的使用者未使用其 Microsoft 帳戶登入裝置，因此無法完成作業。            |
| 0x803F9008<br/>0x803F9009    |  您的裝置已離線。 您的裝置需要連線才能使用此產品。            |
| 0x803F900A    |  訂閱已到期。            |


## <a name="self-install-update-error-codes"></a>自行安裝更新錯誤碼

下列錯誤碼與[自行安裝套件更新](../packaging/self-install-package-updates.md)相關。

|  錯誤碼  |  描述  |
|--------------|---------------|
| 0x803F6200   | 使用者必須同意，才能下載套件更新。               |
| 0x803F6201   | 使用者必須同意，才能下載並安裝套件更新。                                                  |
| 0x803F6203   | 使用者必須同意，才能安裝套件更新。                                         |
| 0x803F6204   | 使用者必須同意，才能下載套件更新，因為下載將會在計量付費連線中進行。                                             |
| 0x803F6206   | 使用者必須同意，才能下載並安裝套件更新，因為下載將會在計量付費連線中進行。     |


## <a name="related-topics"></a>相關主題

* [在應用程式內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得應用程式和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用應用程式內購買的應用程式和附加元件](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用可取用的附加元件購買的項目](enable-consumable-add-on-purchases.md)
* [啟用您的應用程式的訂用帳戶附加元件](enable-subscription-add-ons-for-your-app.md)
* [實作您的應用程式的試用版](implement-a-trial-version-of-your-app.md)
