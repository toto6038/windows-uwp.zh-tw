---
author: JnHs
Description: Learn how to create known user groups to use for package flighting and more.
title: 建立已知的使用者群組
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 目標群組, 客戶, 正式發行前小眾測試版群組, 使用者群組, 已知的使用者
ms.localizationpriority: medium
ms.openlocfilehash: 44f0ea17d1034ae617b4b8c96cf61cc4829a6704
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6969282"
---
# <a name="create-known-user-groups"></a>建立已知的使用者群組

已知的使用者群組可讓您使用與其 Microsoft 帳戶相關聯的電子郵件地址，將特定的人員新增到群組中。 這些已知的使用者群組最常用於搭配[套件正式發行前小眾測試](package-flights.md)散發特定套件給選定的人員群組，或用於發佈提交給[私人對象](choose-visibility-options.md#audience)。 他們也可用於互動廣告活動，例如傳送[目標式通知](send-push-notifications-to-your-apps-customers.md)或[針對性優惠](use-targeted-offers-to-maximize-engagement-and-conversions.md)給特定的客戶。

若要計算為群組成員，每個人都必須使用與您所提供之電子郵件地址相關聯的 Microsoft 帳戶於市集中通過驗證。 若要下載於套件正式發行前小眾測試的應用程式，群組成員必須使用支援套件正式發行前小眾測試的的 Windows 10 版本 (Windows.Desktop 組建 10586 或更新版本；Windows.Mobile 建置 10586.63 或更新版本；或 Xbox One)。 使用私人對象提交，群組成員必須使用 Windows 10 版本 1607 或更高版本 (包括 Xbox One)。

## <a name="to-create-a-known-user-group"></a>若要建立已知的使用者群組

1. 在[合作夥伴中心](https://partner.microsoft.com/dashboard)，展開左側瀏覽功能表中的**互動**，然後選取**客戶群組**。 
2. 在 **\[我的客戶群組\]** 區段，選取 **\[建立新群組\]**。
3. 在下一頁的 **\[群組名稱\]** 方塊中，輸入群組的名稱。
4. 請務必選取 **\[已知的使用者群組\]** 選項按鈕。
5. 輸入您想要加入該群組的人員的電子郵件地址。 您必須至少包含一個電子郵件地址，最多 10,000 個。 您可以直接將電子郵件地址輸入欄位中 (以空格、逗號、分號或換行字元區隔)，或者您可以按一下 **\[匯入 .csv\]** 連結，來從 .csv 檔的電子郵件地址清單建立正式發行前小眾測試版群組。
6. 選取 **\[儲存\]**。

群組現在可供您使用。

您也可以從[套件正式發行前小眾測試版](package-flights.md)建立頁面選取 **\[建立正式發行前小眾測試群組\]** 來建立已知的使用者群組。 請注意，如果這樣做，您將需要重新輸入您已經在套件正式發行前小眾測試版建立頁面中所提供的任何資訊。

> [!IMPORTANT]
> 將已知的使用者群組用於套件正式發行前小眾測試時，必須確定您已經取得該人員的同意，才能將其新增至群組，而且他們了解他們將取得和您非正式發行前小眾測試版提交不同的套件。 

## <a name="to-edit-a-known-user-group"></a>若要編輯已知的使用者群組

您無法從合作夥伴中心移除已知的使用者群組 （或變更其名稱） 已建立，但您可以隨時編輯成員資格之後。

若要檢閱和編輯已知的使用者群組，請展開左側瀏覽功能表的 **\[互動\]**，然後選取 **\[客戶群組\]**。 在 **\[我的客戶群組\]**，選取您想要編輯的群組名稱。 您也可以從套件正式發行前小眾測試版建立頁面編輯已知的使用者群組，方法是在建立新的正式發行前小眾測試版時，選取 **\[檢視和管理現有的群組\]**，或從套件正式發行前小眾測試版的概觀頁面選取群組名稱。 

選取您想要編輯的群組之後，您可以直接在欄位中新增或移除電子郵件地址。

若要執行較大幅度的變更，選取 **\[匯出 .csv\]** 將群組成員資格資訊儲存到 .csv 檔案。 在這個檔案中變更，然後按一下 **\[匯入 .csv\]** 來使用新版本更新群組成員資格。

請注意，這可能需要最多 30 分鐘來實作成員資格變更。 您不需要發佈新的提交以便讓新群組成員可以透過套件正式發行前小眾測試版或私人對象來存取您的提交；只要實作所做的變更，他們就可以存取。 






