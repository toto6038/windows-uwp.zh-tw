---
Description: 如果您提供了目標為不同作業系統的套件，就可以選擇為不同的目標作業系統自訂哪些市集清單的部分。
title: 建立平台專屬的市集清單
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 自訂, 清單, 描述, 舊版
ms.localizationpriority: medium
ms.openlocfilehash: 50c14ed3fa7f7f5f7d1b4c80bb8165b3f27e522a
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63777702"
---
# <a name="create-platform-specific-store-listings"></a>建立平台專屬的市集清單


如果您先前已發行的應用程式會有不同的作業系統為目標的套件，您可以自訂您的存放區清單，客戶在較早的作業系統版本的組件 (Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)。 

在 Windows 10 （包括 Xbox） 上的客戶一律會看到預設[存放區清單](create-app-store-listings.md)。 您不會看到建立平台專屬存放區清單，除非您已發佈您的應用程式，以支援一或多個較早的作業系統版本的套件的選項。 

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新建立的產品不能包含封裝目標 Windows 8.x/Windows Phone 8.x 或更早版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

如果您想要提及一個作業系統版本，只會出現的功能，或想要提供專屬於特定的作業系統 （而不是裝置型別） 的螢幕擷取畫面，平台專屬存放區清單相當實用。

> [!NOTE]
> 建立特定平台清單中一種語言的存放區不會建立平台專屬存放區清單中其他應用程式支援的語言。 您必須分別為每個語言建立平台專屬的市集清單。 另外請注意，您將無法[匯入和匯出列出資料存放區](import-and-export-store-listings.md)針對特定平台清單。


## <a name="creating-a-platform-specific-store-listing"></a>建立平台專屬的市集清單

頂端附近您**存放區清單**頁面上，如果您先前已發行的應用程式包含支援舊版的作業系統版本的套件 ((Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)，您可以選取**建立平台專屬的應用程式存放區清單**。 選取此選項之後，系統會提示您從您的提交支援的目標作業系統版本中選擇。 一旦您已經建立平台專屬存放區清單，較早的作業系統版本的所有應用程式目標，您無法選擇其他選項。

您可以使用您的預設值為 (Windows 10) 存放區清單，做為起點，這樣將會顯示適用於文字和映像，您已輸入您的預設存放區的清單;您接著可以進行您想要在儲存之前的任何變更。 如有需要，您也可以從完全空白的市集清單開始。

按一下 **\[繼續\]** 之後，您的 **\[Microsoft Store 清單\]** 頁面現在將會包含您剛建立的平台專屬 Microsoft Store 清單區段。 本區段將包含一組自己的欄位，其中有**介紹** (必要)、**此版本中的新功能**、**螢幕擷取畫面**、**應用程式磚圖示**、**應用程式功能**和**其他系統需求**。 請務必在您要在自訂市集清單中顯示資訊的每個欄位中輸入資訊，即使它與您的預設市集清單資訊相同，也必須輸入。 如果您讓任何欄位保留空白，則自訂市集清單中的該欄位中將不會出現任何資訊。

> [!IMPORTANT]
> Store 清單的[其他資訊](create-app-store-listings.md#additional-information)區段中欄位無法針對不同的作業系統版本而自訂。
> 
> 此外，由於預設 [Store 清單](create-app-store-listings.md)頁面中的部分欄位僅適用使用 Windows 10 的客戶，因此您在建立平台專屬 Store 清單時，不會看到所有相同選項。 例如，您將無法新增預告片至平台專屬 Microsoft Store 清單中，因為預告片只會對使用 Windows 10 版本 1607 或更新版本的客戶顯示。 

您可以繼續進行變更的特定作業系統版本的客戶所視需要編輯特定平台清單。


## <a name="removing-a-platform-specific-store-listing"></a>移除平台專屬的市集清單

如果您建立平台專屬的 Microsoft Store 清單，並稍後決定您傾向在該作業系統上向客戶顯示您的預設 Microsoft Store 清單，請選取該清單旁的 **\[刪除\]** 連結。

在確認您想要為這些客戶顯示預設 Microsoft Store 清單之後，選取 **\[確定\]**。 將會移除平台專屬的 Microsoft Store 清單 (針對其適用的所有語言)，該作業系統版本的客戶現在將會看到您的預設 Microsoft Store 清單。 如果您改變心意，可以依照上述步驟，為該作業系統建立另一個平台專屬的 Microsoft Store 清單。
 

 




