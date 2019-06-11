---
Description: 產品宣告有助於確保您的應用程式在 Microsoft Store 中適當地顯示，並提供客戶的權限集。
title: 產品宣告
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 47011a22353f26361a392690d857bde1fc180c03
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826112"
---
# <a name="product-declarations"></a>產品宣告

**產品宣告**一節[屬性](enter-app-properties.md)頁面[提交程序](app-submissions.md)有助於確保您的應用程式會適當地顯示，並且會提供給正確的設定客戶，並有助於使用者了解如何使用您的應用程式。

下列各節說明的宣告，和您需要判斷每個宣告是否適用於您的應用程式時，請考慮一些。 請注意，這些宣告的兩個會檢查預設 （如下所述。）根據您的產品類別目錄中，您可能也會看到額外的宣告。 請務必檢閱所有宣告，並確保它們會正確反映您的提交。

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>此應用程式可讓使用者以進行購買，但不會使用 Microsoft Store 商務系統。

幾乎每次提交中，您應該核取此方塊，因為這會提供機會來購買的應用程式已或可以取用或應用程式中使用的項目必須使用 Microsoft Store 應用程式內購買程序 API 來建立並提交的附加元件。 每個[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)，建立並在 2015 年 6 月 29 日之前提交應用程式無法繼續而不需購買功能，因此長時間使用 Microsoft 的商務引擎，提供應用程式內購買功能遵守[Microsoft Store 原則](store-policies.md#108-financial-transactions)。 如果這適用您的 app，您必須選取此方塊。 否則，請保留它未核取。

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>這個應用程式已經過測試，符合協助工具指導方針。

勾選此核取方塊，讓特地在市集中尋找無障礙應用程式的客戶能夠探索您的應用程式。

如果您已完成下列項目，則應該只勾選此方塊：

-   已設定 UI 元素的所有相關協助工具資訊，例如，無障礙的名稱。
-   已實作鍵盤瀏覽與操作、考慮 Tab 鍵順序、鍵盤啟動、方向鍵瀏覽、捷徑。
-   已透過納入像是 4.5:1 文字對比率，而不只是依賴色彩來向使用者傳達資訊，確保無障礙的視覺經驗。
-   已使用像是 Inspect 或 AccChecker 等協助工具測試工具來驗證您的應用程式，並解決那些工具偵測到的所有高優先順序錯誤。
-   已使用像是朗讀程式、放大鏡、螢幕小鍵盤、高對比及高 DPI 等設備與工具，全盤檢驗應用程式的重要情境。

當您將應用程式宣告為無障礙應用程式時，表示同意所有客戶 (包含殘障人士) 都能使用您的應用程式。 例如，這表示您已使用高對比模式和螢幕助讀程式測試過應用程式。 您也已經確認當使用鍵盤、放大鏡以及其他協助工具時，使用者介面都可以正常運作。

如需詳細資訊，請參閱 [協助工具](../design/accessibility/accessibility.md)、[協助工具測試](../design/accessibility/accessibility-testing.md)和[市集中的協助工具](../design/accessibility/accessibility-in-the-store.md)。

> [!IMPORTANT]
> 除非您特別設計並測試針對該用途不清單做為可存取您的應用程式。 如果將您的 app 宣告為無障礙 app，但實際上不支援協助工具，則有面臨從社群收到負面意見反應的風險。

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>客戶可將此 app 安裝至備用磁碟機或抽取式存放裝置。

根據預設，若要允許您的應用程式安裝到外部或卸除式媒體，例如 sd 記憶卡，或非系統磁碟區磁碟機，例如外接式磁碟機的儲存體的客戶，核取此方塊。

如果您想要防止您的應用程式安裝到其他磁碟機或抽取式存放裝置，並只允許安裝內部硬碟機，在其裝置上，取消核取此方塊。 (請注意，沒有任何選項可限制安裝，使應用程式可以*只*卸除式存放媒體安裝。)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows 可以在自動備份至 OneDrive 時包含此 app 的資料。

預設會勾選此方塊，以便在客戶選擇讓 Windows 自動備份至 OneDrive 時包含您應用程式的資料。

如果您想要防止 app 的資料包含於自動備份中，請取消選取此方塊。


## <a name="this-app-sends-kinect-data-to-external-services"></a>此應用程式會將 Kinect 資料傳送到外部服務。 

如果您的應用程式會使用 Kinect 資料並將它傳送給任何外部服務，您必須核取此方塊。



 

 

 




