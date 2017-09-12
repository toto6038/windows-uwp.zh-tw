---
author: jnHs
Description: "檢視已保留給您應用程式使用的名稱、保留其他名稱 (適用於其他語言或變更您的應用程式名稱)，以及刪除您不再需要使用的保留名稱。"
title: "管理應用程式名稱"
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 539ccc671ae3f66c6c17a077ac1c09d989d86fdc
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2017
---
# <a name="manage-app-names"></a>管理應用程式名稱


您可以檢視所有已保留給您應用程式使用的名稱、保留其他名稱 (適用於其他語言或變更您的應用程式名稱)，以及刪除您不需要的名稱。 若要執行此動作，您可以在 Windows 開發人員中心儀表板上 **\[管理應用程式名稱\]** 頁面的 **\[應用程式管理\]** 區段中，取得您的所有 app。

## <a name="reserve-additional-names-for-your-app"></a>保留您 app 的其他名稱

您可以保留多個 app 名稱以供同一個的 app 使用。 如果您要使用多種語言提供您的 app 且想要為不同語言使用不同名稱時，這個功能特別有用。 您也可以保留新名稱，以便變更 app 的名稱。

在 **\[管理應用程式名稱\]** 頁面的 **\[保留更多名稱\]** 區段中，您將會看到一個文字方塊。 輸入您要保留的名稱，然後按一下 **\[檢查可用性\]**。 如果可以使用該名稱，請按一下 **\[保留產品名稱\]**。

> [!NOTE]
> 如需保留應用程式名稱的詳細資訊，以及無法使用某個名稱的原因，請參閱[透過保留名稱建立您的應用程式](create-your-app-by-reserving-a-name.md)。

如果需要，您可以在這裡繼續保留其他 app 名稱。

## <a name="delete-app-names"></a>刪除 app 名稱

如果您不再使用先前所保留的名稱，可以在此處刪除該名稱並釋出。 這麼做之前，請務必確定您的決定，因為這表示該名稱將立即可供其他人保留並使用。

若要刪除其中一個您 app 的保留名稱，請找出您不再使用的名稱，然後按一下 **\[刪除\]**。 在確認對話方塊中，再按一下 **\[刪除\]** 以確認。

請注意，您的 App 必須至少有一個保留的名稱。 若要從儀表板完全移除 App (並釋出該 App 保留的所有名稱)，從 **\[應用程式概觀\]** 頁面按一下 **\[刪除這個應用程式\]**。 如果您的 App 提交正在進行中，就必須先刪除這項提交。 如果已經將 App 發佈至市集，則無法從儀表板刪除它 (雖然您可以使用 **\[概觀\]**頁面的 **\[顯示/隱藏產品\]** 功能來隱藏它)。 

## <a name="rename-an-app-that-has-already-been-published"></a>重新命名已發佈的 App

如果您的 app 已經在市集中，而且您想要重新命名，可以先保留一個新名稱 (依照上述步驟進行)，然後再重新提交該 app。

您必須更新 App 套件以包含新的名稱，讓市集可以使用新的名稱顯示它。 首先，以手動方式或使用 Visual Studio (**\[專案 > 市集 > 將 App 與市集關聯\]**)，更新 Package.StoreAssociation.xml 檔案以使用新名稱。 如需詳細資訊，請參閱[封裝 UWP app](../packaging/packaging-uwp-apps.md)。

您也需要更新 app 資訊清單的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-displayname) 元素，並更新包含 app 名稱的任何圖片或文字。 

> [!IMPORTANT]
> 請務必先更新 Package.StoreAssociation.xml 檔案之後，才變更應用程式資訊清單中的 **Package/Properties/DisplayName**，否則您可能會收到錯誤訊息。

您也可以檢閱您 app 的市集清單，並在該處提及 app 時變更名稱。 一旦您的 app 使用新的名稱發佈之後，就可以刪除您不再需要使用的舊名稱。

 

 




