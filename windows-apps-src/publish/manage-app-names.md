---
Description: 檢視已保留給您應用程式使用的名稱、保留其他名稱 (適用於其他語言或變更您的應用程式名稱)，以及刪除您不再需要使用的保留名稱。
title: 管理應用程式名稱
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10 uwp 應用程式名稱，變更應用程式名稱、 更新應用程式名稱、 遊戲名稱、 產品名稱
ms.localizationpriority: medium
ms.openlocfilehash: 786bfd7cbb4129274a2ff19bc33084824d296bcd
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63827514"
---
# <a name="manage-app-names"></a>管理應用程式名稱

**管理的應用程式名稱**保留其他名稱 （針對其他語言或變更您的應用程式名稱） 可讓您檢視所有您已保留供您的應用程式的名稱，並刪除您不需要的名稱。 您可以找到在此頁面[合作夥伴中心](https://partner.microsoft.com/dashboard)藉由展開**應用程式管理**任何應用程式的左側的導覽功能表中的一節。

> [!IMPORTANT]
> 您可以額外應用程式保留名稱，以及您可以選擇使用您的應用程式的已發行版本中的其中之一而不是被保留，當您第一次在合作夥伴中心建立您的應用程式。 不過，請注意，您為您的產品保留第一個名稱將用於一些您的 it 人員的[身分識別詳細資料](view-app-identity-details.md)，例如**套件系列名稱 (PFN)**。 這些值可能會顯示給某些使用者，且無法變更，因此請確定您在第一次保留的名稱是適用於此用途。


## <a name="reserve-additional-names-for-your-app"></a>保留您 app 的其他名稱

您可以保留多個 app 名稱以供同一個的 app 使用。 如果您要使用多種語言提供您的 app 且想要為不同語言使用不同名稱時，這個功能特別有用。 您也可以保留新的名稱以變更應用程式的名稱，如下所述。

若要保留新的應用程式名稱，尋找 [文字] 方塊中的**保留多個名稱**一節**管理應用程式名稱**頁面。 輸入您要保留的名稱，然後按一下 [檢查可用性]。 如果可以使用該名稱，請按一下 **\[保留產品名稱\]**。 如有需要，您可以藉由重複這些步驟中，保留多個應用程式名稱。

> [!NOTE]
> 如需保留應用程式名稱的詳細資訊，以及無法使用某個名稱的原因，請參閱[透過保留名稱建立您的應用程式](create-your-app-by-reserving-a-name.md)。


## <a name="delete-app-names"></a>刪除 app 名稱

如果您不再使用先前所保留的名稱，可以在此處刪除該名稱並釋出。 這麼做之前，請務必確定您的決定，因為這表示該名稱將立即可供其他人保留並使用。

若要刪除其中一個您 app 的保留名稱，請找出您不再使用的名稱，然後按一下 [刪除]。 在確認對話方塊中，再按一下 [刪除] 以確認。

請注意，您的應用程式必須至少一個保留的名稱。 若要完全移除應用程式從合作夥伴中心 （並發行該應用程式保留您的所有名稱），請按一下**刪除此應用程式**從**應用程式概觀**頁面。 如果您的 App 提交正在進行中，就必須先刪除這項提交。 請注意，如果您已經發佈應用程式存放區，您無法刪除它從合作夥伴中心 (雖然您可以使用**顯示/隱藏產品**功能，在您**概觀**頁面，即可隱藏它)。 


## <a name="rename-an-app-that-has-already-been-published"></a>重新命名已發佈的 app

如果您的 app 已經在市集中，而且您想要重新命名，可以先保留一個新名稱 (依照上述步驟進行)，然後再重新提交該 app。 

您必須更新您的應用程式套件，將舊的名稱取代為新，並將更新的套件上傳到您的提交。
- 首先，以手動方式或使用 Visual Studio (**\[專案 > 市集 > 將 App 與市集關聯\]**)，更新 Package.StoreAssociation.xml 檔案以使用新名稱。如需詳細資訊，請參閱 <<c0> [ 封裝 UWP 應用程式與 Visual Studio](../packaging/packaging-uwp-apps.md)。
- 您也需要更新 app 資訊清單的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 元素，並更新包含 app 名稱的任何圖片或文字。 
  > [!IMPORTANT]
  > 請務必先更新 Package.StoreAssociation.xml 檔案之後，才變更應用程式資訊清單中的 **Package/Properties/DisplayName**，否則您可能會收到錯誤訊息。

若要更新存放區清單，使其使用新的名稱，請前往[存放區清單頁面](create-app-store-listings.md)該語言，然後選取從名稱**產品名稱**下拉式清單。 請務必檢閱您的描述及其他部分的清單中任何提及名稱，並在必要時進行更新。

> [!NOTE]
> 如果您的應用程式有多種語言套件及/或存放區清單，表示您要更新的套件，及/或儲存在其名稱必須更新每一種語言的清單。

一旦您的應用程式已發行新的名稱，您可以刪除您不再需要使用任何較舊名稱。

> [!TIP]
> 每個應用程式會出現在合作夥伴中心使用您為其保留第一個名稱。 如果您已遵循上述步驟，以重新命名應用程式，而您想要它出現在合作夥伴中心內使用新的名稱，您必須刪除原始名稱 (依序按一下**刪除**上**管理應用程式名稱**頁面)。 

 

 




