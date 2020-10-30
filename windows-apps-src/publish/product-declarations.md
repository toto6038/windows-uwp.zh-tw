---
description: 產品聲明可協助確保您的應用程式在 Microsoft Store 中正確顯示，並提供給正確的客戶集合。
title: 產品宣告
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3469992a73a4e90e25ce34883319343ad6986d75
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034611"
---
# <a name="product-declarations"></a>產品宣告

[提交](app-submissions.md)程式之 [ [屬性](enter-app-properties.md)] 頁面的 [ **產品** 宣告] 區段有助於確保您的應用程式能適當地顯示並提供給正確的客戶集合，並協助他們瞭解他們如何使用您的應用程式。

下列各節說明一些宣告，以及您在判斷每個宣告是否適用于您的應用程式時所需考慮的事項。 請注意，根據預設 (會檢查其中的兩個宣告，如下所述 ) 。根據您的產品類別，您可能也會看到其他宣告。 請務必檢查所有的宣告，並確保它們能準確地反映您的提交內容。

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>此應用程式可讓使用者進行購買，但不會使用 Microsoft Store commerce 系統。

對於幾乎每次提交，您應該將此方塊保留為未核取，因為供應商機的應用程式必須使用 Microsoft Store 的應用程式內購買 API，才能建立及提交附加元件。 根據 [應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)，在2015年6月29日前建立和提交的應用程式，只要購買功能符合 [Microsoft Store 原則](store-policies.md#108-financial-transactions)，就可以繼續提供應用程式內的購買功能，而不需要使用 Microsoft 的商務引擎。 如果這適用您的 app，您必須選取此方塊。 否則，請保留它未核取。

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
> 除非您已特別針對該目的進行設計和測試，否則請勿將應用程式列為可存取。 如果將您的 app 宣告為無障礙 app，但實際上不支援協助工具，則有面臨從社群收到負面意見反應的風險。

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>客戶可將此 app 安裝至備用磁碟機或抽取式存放裝置。

預設會核取此方塊，以允許客戶將您的應用程式安裝到外部或卸除式存放裝置媒體（例如 SD 記憶卡）或非系統磁碟區磁片磁碟機（例如外部磁片磁碟機）。

如果您想要防止應用程式安裝到替代磁片磁碟機或卸除式存放裝置，並且只允許安裝到其裝置上的內部硬碟，請取消核取此方塊。  (請注意，沒有限制安裝的選項，因此應用 *程式只能安裝* 至卸除式存放裝置媒體。 ) 


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows 可以在自動備份至 OneDrive 時包含此 app 的資料。

預設會勾選此方塊，以便在客戶選擇讓 Windows 自動備份至 OneDrive 時包含您應用程式的資料。

如果您想要防止 app 的資料包含於自動備份中，請取消選取此方塊。


## <a name="this-app-sends-kinect-data-to-external-services"></a>此應用程式會將 Kinect 資料傳送到外部服務。 

如果您的應用程式會使用 Kinect 資料並將它傳送給任何外部服務，您必須核取此方塊。



 

 

 
