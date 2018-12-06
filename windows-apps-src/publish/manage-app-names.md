---
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: 管理應用程式名稱
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10，uwp，應用程式名稱，變更應用程式名稱、 更新應用程式名稱、 遊戲名稱、 產品名稱
ms.localizationpriority: medium
ms.openlocfilehash: a27955f64a36fadde9b0f1781337929ce6871a9c
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8752162"
---
# <a name="manage-app-names"></a>管理應用程式名稱

**管理應用程式名稱**可讓您檢視所有已保留給您的應用程式的名稱保留其他名稱 （適用於其他語言或變更您的應用程式名稱），並刪除您不需要的名稱。 您可以在[合作夥伴中心](https://partner.microsoft.com/dashboard)中找到此頁面，藉此拓展任何應用程式的左側的導覽功能表中的 [**應用程式管理**] 區段。

> [!IMPORTANT]
> 您可以保留其他名稱用於應用程式，以及您可能會選擇其中一個已發佈您的應用程式的版本中使用而不是您保留了您在第一次在合作夥伴中心建立您的應用程式時的一個。 不過，請注意您保留以供您的產品的名字，會使用您的 it 部分中的[身分識別詳細資料](view-app-identity-details.md)，例如**套件系列名稱 (PFN)**。 這些值可能會看到某些使用者，而且不能變更，因此請確定您在第一次保留的名稱是適用於這種使用。


## <a name="reserve-additional-names-for-your-app"></a>保留您 app 的其他名稱

您可以保留多個 app 名稱以供同一個的 app 使用。 如果您要使用多種語言提供您的 app 且想要為不同語言使用不同名稱時，這個功能特別有用。 您也可以保留新名稱才能變更的應用程式時，名稱如下所述。

若要保留一個新的應用程式名稱，尋找在文字方塊中**管理應用程式名稱**] 頁面的 [**保留更多名稱**] 區段中。 輸入您要保留的名稱，然後按一下 **\[檢查可用性\]**。 如果可以使用該名稱，請按一下 **\[保留產品名稱\]**。 如有需要，您可以藉由重複這些步驟中，保留多個應用程式名稱。

> [!NOTE]
> 如需保留應用程式名稱的詳細資訊，以及無法使用某個名稱的原因，請參閱[透過保留名稱建立您的應用程式](create-your-app-by-reserving-a-name.md)。


## <a name="delete-app-names"></a>刪除 app 名稱

如果您不再使用先前所保留的名稱，可以在此處刪除該名稱並釋出。 這麼做之前，請務必確定您的決定，因為這表示該名稱將立即可供其他人保留並使用。

若要刪除其中一個您 app 的保留名稱，請找出您不再使用的名稱，然後按一下 **\[刪除\]**。 在確認對話方塊中，再按一下 **\[刪除\]** 以確認。

請注意，您的應用程式必須擁有至少一個保留的名稱。 若要完全從合作夥伴中心移除 app （並釋出該應用程式保留的所有名稱），按一下 [**刪除此應用程式**從**應用程式概觀**頁面。 如果您的 App 提交正在進行中，就必須先刪除這項提交。 請注意，是否您已經發佈至 microsoft Store 應用程式，您無法刪除它從合作夥伴中心 （雖然您可以使用**概觀**頁面上**顯示/隱藏產品**功能來隱藏它）。 


## <a name="rename-an-app-that-has-already-been-published"></a>重新命名已發佈的 App

如果您的 app 已經在市集中，而且您想要重新命名，可以先保留一個新名稱 (依照上述步驟進行)，然後再重新提交該 app。 

您必須更新您的應用程式套件，以使用新套件取代舊的名稱，並將更新的套件上傳到您的提交。
- 首先，更新 Package.StoreAssociation.xml 檔案以使用新的名稱，以手動方式或使用 Visual Studio (**專案 > 市集 > 關聯 App 與市集...**)。如需詳細資訊，請參閱[套件與 Visual Studio 的 UWP 應用程式](../packaging/packaging-uwp-apps.md)。
- 您也需要更新 app 資訊清單的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 元素，並更新包含 app 名稱的任何圖片或文字。 
  > [!IMPORTANT]
  > 請務必先更新 Package.StoreAssociation.xml 檔案之後，才變更應用程式資訊清單中的 **Package/Properties/DisplayName**，否則您可能會收到錯誤訊息。

若要更新 Store 清單，因此，它使用新的名稱，移至[市集清單頁面](create-app-store-listings.md)該語言並從 [**產品名稱**] 下拉式清單中選取的名稱。 請務必檢閱您的描述和其他部分適用於任何提及的名稱清單，並在必要時進行更新。

> [!NOTE]
> 如果您的應用程式會有多個語言套件和/或市集清單，您將需要更新套件和/或儲存在其中名稱需要更新每種語言的清單。

一旦您的應用程式發佈與新的名稱，您可以刪除您不再需要使用任何舊名稱。

> [!TIP]
> 每個應用程式會出現在合作夥伴中心使用它們您保留給它的名字。 如果您已按照上述步驟，以重新命名應用程式，而您想要在合作夥伴中心中顯示使用新的名稱，您必須刪除的原始名稱 （藉由在 [**管理應用程式名稱**] 頁面上，按一下 [**刪除**)。 

 

 




