---
author: jnHs
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: 建立平台專屬的 Store 清單
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 3/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 自訂, 清單, 描述, 舊版
ms.localizationpriority: medium
ms.openlocfilehash: 6f30a825cc7aec1b6f7dbf5cff0ea1c17c43ffd7
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3115177"
---
# <a name="create-platform-specific-store-listings"></a>建立平台專屬的 Store 清單


如果您的應用程式具有目標為不同作業系統的套件，您可以為舊版作業系統的客戶 (Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本) 自訂部分 Microsoft Store 清單。 

> [!IMPORTANT]
> Windows 10 的客戶一律會看到預設的 [Microsoft Store 清單](create-app-store-listings.md)。 您不會看到建立平台專屬 Microsoft Store 清單的選項，除非您已上傳支援一或多個舊版作業系統的套件。 

如果您想要提及只會出現在一個作業系統版本中的功能，或想要提供特定作業系統專屬的螢幕擷取畫面 (與裝置類型無關)，而不是讓所有客戶看到相同的市集清單，平台專屬的市集清單就非常有用。

> [!NOTE]
> 使用某一種語言建立平台專屬的 Microsoft Store 清單時，不會以您應用程式支援之其他語言建立平台專屬的 Microsoft Store 清單。 您必須分別為每個語言建立平台專屬的市集清單。 也請注意您無法匯入及匯出平台專屬清單的 Microsoft Store 清單資料。


## <a name="creating-a-platform-specific-store-listing"></a>建立平台專屬的市集清單

如果您的應用程式支援舊版作業系統 (Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)，在 **\[Microsoft Store 清單\]** 頁面最上方附近，您可以選取 **\[建立平台專屬的 App Microsoft Store 清單\]**。 

> [!TIP]
> 除非您已上傳套件，否則不會看見建立平台專屬 Microsoft Store 清單的選項。

選取此選項之後，系統會提示您從您的提交支援的目標作業系統版本中選擇。 若您已經為應用程式的所有目標作業系統版本建立平台專屬的 Microsoft Store 清單，將無法進行其他選擇。 (Windows 10 不會包含在選擇清單中，因為 Windows 10 的客戶一律會看到 App 的預設 Microsoft Store 清單)。

您可以使用預設的 Microsoft Store 清單做為起點，這將會顯示您已針對預設 Microsoft Store 清單輸入的適用文字和影像；接著您就能夠進行任何所需的變更，然後儲存。 如有需要，您也可以從完全空白的市集清單開始。

按一下 **\[繼續\]** 之後，您的 **\[Microsoft Store 清單\]** 頁面現在將會包含您剛建立的平台專屬 Microsoft Store 清單區段。 本區段將包含一組自己的欄位，其中有**介紹** (必要)、**此版本中的新功能**、**螢幕擷取畫面**、**應用程式磚圖示**、**應用程式功能**和**其他系統需求**。 請務必在您要在自訂市集清單中顯示資訊的每個欄位中輸入資訊，即使它與您的預設市集清單資訊相同，也必須輸入。 如果您讓任何欄位保留空白，則自訂市集清單中的該欄位中將不會出現任何資訊。


> [!IMPORTANT]
> Store 清單的[其他資訊](create-app-store-listings.md#additional-information)區段中欄位無法針對不同的作業系統版本而自訂。
> 
> 此外，由於預設 [Store 清單](create-app-store-listings.md)頁面中的部分欄位僅適用使用 Windows 10 的客戶，因此您在建立平台專屬 Store 清單時，不會看到所有相同選項。 例如，您將無法新增預告片至平台專屬 Microsoft Store 清單中，因為預告片只會對使用 Windows 10 版本 1607 或更新版本的客戶顯示。 


## <a name="removing-a-platform-specific-store-listing"></a>移除平台專屬的 Microsoft Store 清單

如果您建立平台專屬的 Microsoft Store 清單，並稍後決定您傾向在該作業系統上向客戶顯示您的預設 Microsoft Store 清單，請選取該清單旁的 **\[刪除\]** 連結。

在確認您想要為這些客戶顯示預設 Microsoft Store 清單之後，選取 **\[確定\]**。 將會移除平台專屬的 Microsoft Store 清單 (針對其適用的所有語言)，該作業系統版本的客戶現在將會看到您的預設 Microsoft Store 清單。 如果您改變心意，可以依照上述步驟，為該作業系統建立另一個平台專屬的 Microsoft Store 清單。

 

 




