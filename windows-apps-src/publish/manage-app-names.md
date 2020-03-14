---
Description: 檢視已保留給您應用程式使用的名稱、保留其他名稱 (適用於其他語言或變更您的應用程式名稱)，以及刪除您不再需要使用的保留名稱。
title: 管理應用程式名稱
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10，uwp，應用程式名稱，變更應用程式名稱，更新應用程式名稱，遊戲名稱，產品名稱
ms.localizationpriority: medium
ms.openlocfilehash: 38cedf40d4ecf997f6fbced2186cd5b27c6d5e4f
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210354"
---
# <a name="manage-app-names"></a>管理應用程式名稱

[**管理應用程式名稱**] 可讓您查看您為應用程式保留的所有名稱、保留額外的名稱（適用于其他語言或變更您的應用程式名稱），以及刪除您不需要的名稱。 您可以在 [[合作夥伴中心](https://partner.microsoft.com/dashboard)] 中，展開您任何應用程式左側導覽功能表中的 [**應用程式管理**] 區段，即可找到此頁面。

> [!IMPORTANT]
> 您可以為應用程式保留其他名稱，而且您可以選擇在應用程式的已發佈版本中使用其中一個，而不是在您第一次在合作夥伴中心建立應用程式時所保留的名稱。 不過，請注意，您為產品保留的名字將會用於其中一些身分[識別詳細資料](view-app-identity-details.md)，例如**套件系列名稱（PFN）** 。 某些使用者可能會看到這些值，而且無法變更，因此請確定您先保留的名稱適用于此用途。


## <a name="reserve-additional-names-for-your-app"></a>保留您 app 的其他名稱

您可以保留多個 app 名稱以供同一個的 app 使用。 如果您要使用多種語言提供您的 app 且想要為不同語言使用不同名稱時，這個功能特別有用。 您也可以保留新名稱，以便變更應用程式的名稱，如下所述。

若要保留新的應用程式名稱，請在 [**管理應用程式名稱**] 頁面的 [**保留更多名稱**] 區段中尋找文字方塊。 輸入您要保留的名稱，然後按一下 **\[檢查可用性\]** 。 如果可以使用該名稱，請按一下 **\[保留產品名稱\]** 。 如有需要，您可以重複這些步驟來保留多個應用程式名稱。

> [!NOTE]
> 如需保留應用程式名稱的詳細資訊，以及無法使用某個名稱的原因，請參閱[透過保留名稱建立您的應用程式](create-your-app-by-reserving-a-name.md)。


## <a name="delete-app-names"></a>刪除 app 名稱

如果您不再使用先前所保留的名稱，可以在此處刪除該名稱並釋出。 這麼做之前，請務必確定您的決定，因為這表示該名稱將立即可供其他人保留並使用。

若要刪除其中一個您 app 的保留名稱，請找出您不再使用的名稱，然後按一下 **\[刪除\]** 。 在確認對話方塊中，再按一下 **\[刪除\]** 以確認。

請注意，您的應用程式至少必須有一個保留名稱。 若要從合作夥伴中心完全移除應用程式（併發行您保留給該應用程式的所有名稱），請從**應用程式**的 [總覽] 頁面按一下 [**刪除此應用程式**]。 如果您的 App 提交正在進行中，就必須先刪除這項提交。 請注意，如果您已將應用程式發佈至存放區，則無法從合作夥伴中心刪除它（不過，您可以使用 [**總覽**] 頁面上的 [**顯示/隱藏產品**] 功能來隱藏它）。 


## <a name="rename-an-app-that-has-already-been-published"></a>重新命名已發佈的 App

如果您的 app 已經在市集中，而且您想要重新命名，可以先保留一個新名稱 (依照上述步驟進行)，然後再重新提交該 app。 

您必須更新應用程式的套件，以將舊名稱取代為新名稱，並將更新的封裝上傳至您的提交。
- 首先，將 Package.storeassociation.xml 更新為使用新名稱，不論是手動或使用 Visual Studio （**Project > Store > 將應用程式與存放區相關聯**...）。如需詳細資訊，請參閱[使用 Visual Studio 封裝 UWP 應用程式](/windows/msix/package/packaging-uwp-apps)。
- 您也需要更新 app 資訊清單的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 元素，並更新包含 app 名稱的任何圖片或文字。 
  > [!IMPORTANT]
  > 請務必先更新 Package.StoreAssociation.xml 檔案之後，才變更應用程式資訊清單中的 **Package/Properties/DisplayName**，否則您可能會收到錯誤訊息。

若要更新商店清單，使其使用新名稱，請移至該語言的[商店清單頁面](create-app-store-listings.md)，然後從 [**產品名稱**] 下拉式清單中選取名稱。 如有任何提及的名稱，請務必檢查您的描述和其他部分，並視需要進行更新。

> [!NOTE]
> 如果您的應用程式有多種語言的套件和（或）存放區清單，您必須針對需要更新名稱的每一種語言更新套件和/或存放區清單。

當您的應用程式以新名稱發佈之後，您就可以刪除任何不再需要使用的舊名稱。

> [!TIP]
> 每個應用程式會使用您為其保留的名字，出現在合作夥伴中心。 如果您已遵循上述步驟來重新命名應用程式，而且想要將它顯示在合作夥伴中心使用新名稱，您必須刪除原始名稱（按一下 [**管理應用程式名稱**] 頁面上的 [**刪除**]）。 

 

 




