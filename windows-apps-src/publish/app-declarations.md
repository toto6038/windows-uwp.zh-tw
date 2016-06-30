---
author: jnHs
Description: "您可以在提交程序中，於 \\[應用程式內容\\] 頁面的 \\[應用程式宣告\\] 區段中，提供關於 app 的其他資訊。"
title: "應用程式宣告"
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee2e54494a868fbec8c4a8bd7bbb9bdcd6cbf9ae

---

# 應用程式宣告

您可以在提交程序中，於 \[應用程式內容\] 頁面的 \[應用程式宣告\] 區段中，提供關於 app 的其他資訊。 這些宣告可協助確認您的 app 已適當顯示並提供給正確的客戶群使用，或者可以指出客戶能夠如何使用您的 app。

下列各節說明每個宣告，以及您在決定每個宣告是否適合 app 時應考量的事項。

## 這個應用程式允許使用者進行購買，但不是使用 Windows 市集商務系統。

大部分 app 應該讓此方塊保留未選取，因為提供機會進行 App 內購買的 app 通常使用 Microsoft app 內購買 API 來建立和[提交 IAP](iap-submissions.md)。 根據 [app 開發人員合約](https://msdn.microsoft.com/library/windows/apps/hh694058)，在 2015 年 6 月 29 日之前建立並送出的 app 得以繼續提供 app 內購買功能，而不需使用 Microsoft 的商務引擎，只要購買功能符合 [Windows 市集原則](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_8)。 如果這適用您的 app，您必須選取此方塊。 否則，請保留它未核取。

## 這個應用程式已經過測試，符合協助工具指導方針。

勾選此核取方塊，讓特地在市集中尋找無障礙應用程式的客戶能夠探索您的應用程式。

如果您已完成下列項目，則應該只勾選此方塊：

-   已設定 UI 元素的所有相關協助工具資訊，例如，無障礙的名稱。
-   已實作鍵盤瀏覽與操作、考慮 Tab 鍵順序、鍵盤啟動、方向鍵瀏覽、捷徑。
-   已透過納入像是 4.5:1 文字對比率，而不只是依賴色彩來向使用者傳達資訊，確保無障礙的視覺經驗。
-   已使用像是 Inspect 或 AccChecker 等協助工具測試工具來驗證您的應用程式，並解決那些工具偵測到的所有高優先順序錯誤。
-   已使用像是朗讀程式、放大鏡、螢幕小鍵盤、高對比及高 DPI 等設備與工具，全盤檢驗應用程式的重要情境。

當您將應用程式宣告為無障礙應用程式時，表示同意所有客戶 (包含殘障人士) 都能使用您的應用程式。 例如，這表示您已使用高對比模式和螢幕助讀程式測試過應用程式。 您也已經確認當使用鍵盤、放大鏡以及其他協助工具時，使用者介面都可以正常運作。

如需詳細資訊，請參閱 [Windows 執行階段 app 的協助工具](https://msdn.microsoft.com/library/windows/apps/dn263101)、[協助工具測試](https://msdn.microsoft.com/library/windows/apps/mt297664)和[市集中的協助工具](https://msdn.microsoft.com/library/windows/apps/mt297663)。

> **重要：**請勿將您的 app 列示為無障礙 app，除非您已經特別針對該用途為 app 進行了工程設計及測試。 如果將您的 app 宣告為無障礙 app，但實際上不支援協助工具，則有面臨從社群收到負面意見反應的風險。

## 客戶可將此 app 安裝至備用磁碟機或抽取式存放裝置。

預設會勾選此方塊，讓客戶能夠將您的應用程式安裝至抽取式存放裝置媒體 (例如 SD 記憶卡)，或是安裝至非系統磁碟區磁碟機 (例如外部磁碟機)。

如果您想防止 app 安裝至備用磁碟機或抽取式存放裝置，請取消選取此方塊。

請注意，沒有選項可用來限制 app 只能安裝到抽取式存放裝置媒體。

> **注意：**針對 Windows Phone 8.1，已透過 StoreManifest.xml 來指定此選項。

## Windows 可以在自動備份至 OneDrive 時包含此 app 的資料。

預設會勾選此方塊，以便在客戶選擇讓 Windows 自動備份至 OneDrive 時包含您應用程式的資料。

如果您想要防止 app 的資料包含於自動備份中，請取消選取此方塊。

> **注意：**針對 Windows Phone 8.1，已透過 StoreManifest.xml 來指定此選項。

 

 

 







<!--HONumber=Jun16_HO4-->


