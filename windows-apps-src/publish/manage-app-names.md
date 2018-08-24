---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: 管理 app 名稱
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 應用程式名稱變更應用程式名稱、 更新應用程式名稱、 遊戲名稱、 產品名稱
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2829904"
---
# <a name="manage-app-names"></a>管理 app 名稱

**管理應用程式名稱**屬性可讓您檢視所有已保留供您的應用程式的名稱保留其他名稱 （其他語言的或變更您的應用程式名稱），並刪除您不需要的名稱。 您可以找到[Windows 開發人員中心儀表板](https://partner.microsoft.com/dashboard)中的此頁面來展開 [**應用程式管理**] 區段中的左側的導覽功能表之任何您的應用程式。


## <a name="reserve-additional-names-for-your-app"></a>保留您 app 的其他名稱

您可以保留多個 app 名稱以供同一個的 app 使用。 如果您要使用多種語言提供您的 app 且想要為不同語言使用不同名稱時，這個功能特別有用。 您也可以保留的新名稱以變更應用程式的名稱如下所示。

若要保留新的應用程式名稱，尋找 [文字] 方塊中**保留多個名稱**] 區段中的 [**管理應用程式名稱**] 頁面。 輸入您要保留的名稱，然後按一下 **\[檢查可用性\]**。 如果可以使用該名稱，請按一下 **\[保留產品名稱\]**。 您可以視，重複這些步驟，來保留多個應用程式名稱。

> [!NOTE]
> 如需保留應用程式名稱的詳細資訊，以及無法使用某個名稱的原因，請參閱[透過保留名稱建立您的應用程式](create-your-app-by-reserving-a-name.md)。


## <a name="delete-app-names"></a>刪除 app 名稱

如果您不再使用先前所保留的名稱，可以在此處刪除該名稱並釋出。 這麼做之前，請務必確定您的決定，因為這表示該名稱將立即可供其他人保留並使用。

若要刪除其中一個您 app 的保留名稱，請找出您不再使用的名稱，然後按一下 **\[刪除\]**。 在確認對話方塊中，再按一下 **\[刪除\]** 以確認。

請注意您的應用程式必須至少一個保留的名稱。 完全移除從儀表板、 應用程式 （及釋出已保留該應用程式的所有名稱），按一下 [從 [**應用程式概觀**] 頁面上的 [**刪除此應用程式**]。 如果您的 App 提交正在進行中，就必須先刪除這項提交。 請注意是否您已經發佈應用程式存放區，您無法刪除它從儀表板 （雖然您可以使用**顯示/隱藏產品**功能**概觀**」 頁面上將它隱藏）。 


## <a name="rename-an-app-that-has-already-been-published"></a>重新命名已發佈的 App

如果您的 app 已經在市集中，而且您想要重新命名，可以先保留一個新名稱 (依照上述步驟進行)，然後再重新提交該 app。 

您必須更新的舊名稱取代為新的其中一個並將更新的套件上傳至您提交您的應用程式套件。
- 首先，更新 Package.StoreAssociation.xml 檔案，以使用新的名稱，以手動方式或使用 Visual Studio (**Project > 存放區 > 建立關聯的應用程式與存放區...**)。如需詳細資訊，請參閱[使用 Visual Studio UWP 應用程式套件](../packaging/packaging-uwp-apps.md)。
- 您也需要更新 app 資訊清單的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 元素，並更新包含 app 名稱的任何圖片或文字。 
  > [!IMPORTANT]
  > 請務必先更新 Package.StoreAssociation.xml 檔案之後，才變更應用程式資訊清單中的 **Package/Properties/DisplayName**，否則您可能會收到錯誤訊息。

若要更新存放區列出，使它會使用新的名稱，請移至[Store 列出頁面](create-app-store-listings.md)為該語言並選取 [**產品名稱**] 下拉式清單中的名稱。 請務必檢閱您的說明及其他組件的任何提及名稱的清單並在必要時進行更新。

> [!NOTE]
> 如果您的應用程式會有多種語言套件及 （或） 存放區清單，您需要更新套件及 （或） 儲存在其名稱必須更新每個語言清單。

一旦您的應用程式發佈新的名稱，您可以刪除您不再需要使用的任何舊名稱。

> [!TIP]
> 每個應用程式會顯示使用保留給它的名字在儀表板。 如果遵循上述步驟以重新命名應用程式，且您想要顯示在儀表板使用的新名稱，您必須刪除原始的名稱 （依序按一下 [**刪除**在 [**管理應用程式名稱**] 頁面)。 

 

 




