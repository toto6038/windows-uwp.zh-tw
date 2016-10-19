---
author: jnHs
Description: "在 Windows 開發人員中心儀表板中使用 app 時，您可以檢視由 Windows 市集指派給 app 的唯一身分識別相關詳細資料，以及取得 app 市集清單的連結。"
title: "檢視 app 身分識別詳細資料"
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
translationtype: Human Translation
ms.sourcegitcommit: a25d87556bb85718f818af5b586f54e6985aaaa4
ms.openlocfilehash: dc61971865a05e1de17cdcf55ab495fee4917b74

---

# 檢視 app 身分識別詳細資料


在 Windows 開發人員中心儀表板中使用 app 時，您可以檢視由 Windows 市集指派給 app 的唯一身分識別相關詳細資料，以及取得 app 市集清單的連結。

若要尋找這項資訊，請瀏覽到其中一個 app，然後在左方導覽功能表中展開 [應用程式管理]****。 按一下 [應用程式身分識別]**** 可檢視這些詳細資料。

> **注意** 您必須有 app 的[保留名稱](create-your-app-by-reserving-a-name.md)，才能查看大多數身分識別詳細資料。

## appx 資訊清單中包含的值


appx 資訊清單中必須包含下列值。 如果您使用 Microsoft Visual Studio 建立套件，並且使用與您開發人員帳戶關聯的相同 Microsoft 帳戶進行簽署，這些詳細資料就會自動包含在內。 如果您手動建立套件，您就必須加入下列這些項目。

-   **套件/身分識別/名稱**
-   **套件/身分識別/發佈者**

如需詳細資訊，請參閱[套件資訊清單結構描述參考](https://msdn.microsoft.com/library/windows/apps/br211473)中的 [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441)。

這些元素共同宣告您的 app 身分識別，建立所有其套件所屬的「套件系列」。 個別套件會有額外的詳細資料，例如架構和版本。

## 套件系列的其他值


下列值為參考您 app 套件系列的其他值，但是沒有包含在資訊清單中。

-   **套件系列名稱 (PFN)**：使用特定的 Windows API 時會使用這個值。
-   **套件 SID**：您需要此值以傳送 WNS 通知到您的 app。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](https://msdn.microsoft.com/library/windows/apps/mt187203)。

## App 清單連結

您可以分享連至您 app 頁面的連結，以協助您的客戶在市集中找到該 app。 這個連結的格式為 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

> **注意** 此 URL 適用於任何可使用您 App 的作業系統版本。 您可能也會看到適用於 Windows 8.1 和更早版本和/或 Windows Phone 8.1 和舊版的其他連結，這僅適用於指定的作業系統版本上。

當客戶按一下此連結時，會開啟您 App 的網頁清單頁面。 如果您的 App 適用於客戶的 Windows 裝置，市集 App 也會啟動並顯示您的 App 清單。

您 App 的**市集識別碼**也會在此區段中顯示。 這個市集識別碼可用來[產生市集徽章](http://go.microsoft.com/fwlink/p/?LinkId=534236)，或識別您的 App。

 

 







<!--HONumber=Aug16_HO3-->


