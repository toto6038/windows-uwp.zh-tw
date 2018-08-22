---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: 產品宣告
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 959e056d5edf5e1fe7a1c51a2f855c9e11512cb0
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2799539"
---
# <a name="product-declarations"></a>產品宣告

[送出程序](app-submissions.md)中的 [[屬性](enter-app-properties.md)] 頁面上的**產品宣告**區段可協助確保您的應用程式會顯示適當地與提供給右邊的客戶，並協助其了解如何他們可以使用您的應用程式集。

下列各節說明宣告和您需要決定是否每個宣告適用於您的應用程式時所應考量的一些。 請注意這些宣告的兩個會檢查有預設 （如下所示）。視您的產品類別] 中，您也可能會看到其他宣告。 請務必檢閱所有宣告並確定他們正確地反映您提交。

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>此應用程式可讓使用者進行購買，但不會使用 Microsoft 存放區商務系統。

如幾乎送出、 應保留未核取此方塊，自提供機會購買的應用程式是或可耗用或您的應用程式中使用項目必須使用 Microsoft 存放區中應用程式購買功能 API 來建立並送出的附加元件。 每個[應用程式開發人員協議](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中，應用程式的已建立並送出前 2015 年 6 月 29 無法繼續了在應用程式購買功能提供而不需使用 Microsoft 的 commerce 引擎，因此只要購買功能遵守[Microsoft 存放區原則](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions)。 如果這適用您的 app，您必須選取此方塊。 否則，請保留它未核取。

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>這個應用程式已經過測試，符合協助工具指導方針。

勾選此核取方塊，讓特地在市集中尋找無障礙應用程式的客戶能夠探索您的應用程式。

如果您已完成下列項目，則應該只勾選此方塊：

-   已設定 UI 元素的所有相關協助工具資訊，例如，無障礙的名稱。
-   已實作鍵盤瀏覽與操作、考慮 Tab 鍵順序、鍵盤啟動、方向鍵瀏覽、捷徑。
-   已透過納入像是 4.5:1 文字對比率，而不只是依賴色彩來向使用者傳達資訊，確保無障礙的視覺經驗。
-   已使用像是 Inspect 或 AccChecker 等協助工具測試工具來驗證您的應用程式，並解決那些工具偵測到的所有高優先順序錯誤。
-   已使用像是朗讀程式、放大鏡、螢幕小鍵盤、高對比及高 DPI 等設備與工具，全盤檢驗應用程式的重要情境。

當您將應用程式宣告為無障礙應用程式時，表示同意所有客戶 (包含殘障人士) 都能使用您的應用程式。 例如，這表示您已使用高對比模式和螢幕助讀程式測試過應用程式。 您也已經確認當使用鍵盤、放大鏡以及其他協助工具時，使用者介面都可以正常運作。

如需詳細資訊，請參閱[協助工具](../design/accessibility/accessibility.md)、[協助工具測試](../design/accessibility/accessibility-testing.md)及[存放區中的協助工具](../design/accessibility/accessibility-in-the-store.md)。

> [!IMPORTANT]
> 除非您特別工程及測試的目的不清單為可存取您的應用程式。 如果將您的 app 宣告為無障礙 app，但實際上不支援協助工具，則有面臨從社群收到負面意見反應的風險。

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>客戶可將此 app 安裝至備用磁碟機或抽取式存放裝置。

根據預設，若要讓客戶能夠安裝您的應用程式至 SD 卡，例如或非系統磁碟區的媒體磁碟機等外部的磁碟機的外部或抽取式儲存裝置已核取此方塊。 （for Windows Phone 8.1，這是先前所述透過 StoreManifest.xml。）

若要防止您的應用程式正在安裝至替代磁碟機或抽取式儲存並只允許安裝，以在其裝置上的內部硬碟機、 取消核取此方塊。

請注意是沒有選項可以限制安裝，以便讓應用程式可以*只*安裝到抽取式儲存媒體。


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows 可以在自動備份至 OneDrive 時包含此 app 的資料。

預設會勾選此方塊，以便在客戶選擇讓 Windows 自動備份至 OneDrive 時包含您應用程式的資料。 （for Windows Phone 8.1，這是先前所述透過 StoreManifest.xml。）

如果您想要防止 app 的資料包含於自動備份中，請取消選取此方塊。


## <a name="this-app-sends-kinect-data-to-external-services"></a>此應用程式會將 Kinect 資料傳送至外部服務。 

如果您的應用程式使用 Kinect 資料並將它傳送至任何外部的服務，您必須核取此方塊。



 

 

 




