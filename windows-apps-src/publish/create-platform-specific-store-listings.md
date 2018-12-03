---
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: 建立平台專屬的 Store 清單
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 自訂, 清單, 描述, 舊版
ms.localizationpriority: medium
ms.openlocfilehash: bfb21d56df357640734e9e5026783cc398468f0a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8460898"
---
# <a name="create-platform-specific-store-listings"></a>建立平台專屬的 Store 清單


如果您先前發佈的應用程式具有目標為不同作業系統的套件，您可以自訂您的舊版作業系統的客戶的市集清單的部分選項 (Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)。 

Windows 10 （包括 Xbox） 的客戶一律會看到的預設[市集清單](create-app-store-listings.md)。 您將不會看到建立平台專屬的市集清單，除非您已經發佈您的應用程式以支援一或多個舊版作業系統的套件的選項。 

> [!IMPORTANT]
> 截至 2018 年 10 月 31 剛建立的產品不能包含目標為 Windows 8.x/Windows 套件 Phone 8.x 或更舊版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

如果您想要提及只會出現在一個作業系統版本的功能，或想要提供 （與裝置類型無關） 的特定作業系統專屬的螢幕擷取畫面，平台專屬的市集清單就非常有用。

> [!NOTE]
> 使用某一種語言建立平台專屬的 Microsoft Store 清單時，不會以您應用程式支援之其他語言建立平台專屬的 Microsoft Store 清單。 您必須分別為每個語言建立平台專屬的市集清單。 也請注意您無法[匯入及匯出市集清單資料](import-and-export-store-listings.md)平台專屬的清單。


## <a name="creating-a-platform-specific-store-listing"></a>建立平台專屬的市集清單

您的**市集清單**頁面最上方附近，如果您先前發佈的應用程式內容包含支援舊版作業系統的套件 ((Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)，您可以選取**建立平台專屬的 app 的市集清單**. 選取此選項之後，系統會提示您從您的提交支援的目標作業系統版本中選擇。 一旦您已經建立平台專屬的市集清單的較舊版本的作業系統版本的所有 app 的目標，您將無法進行其他選擇。

您可以使用預設 (Windows 10) 的市集清單做為起點，這將會顯示的適用文字和影像您輸入您的預設市集清單;然後您將無法進行任何您想要儲存之前的變更。 如有需要，您也可以從完全空白的市集清單開始。

按一下 **\[繼續\]** 之後，您的 **\[Microsoft Store 清單\]** 頁面現在將會包含您剛建立的平台專屬 Microsoft Store 清單區段。 本區段將包含一組自己的欄位，其中有**介紹** (必要)、**此版本中的新功能**、**螢幕擷取畫面**、**應用程式磚圖示**、**應用程式功能**和**其他系統需求**。 請務必在您要在自訂市集清單中顯示資訊的每個欄位中輸入資訊，即使它與您的預設市集清單資訊相同，也必須輸入。 如果您讓任何欄位保留空白，則自訂市集清單中的該欄位中將不會出現任何資訊。

> [!IMPORTANT]
> Store 清單的[其他資訊](create-app-store-listings.md#additional-information)區段中欄位無法針對不同的作業系統版本而自訂。
> 
> 此外，由於預設 [Store 清單](create-app-store-listings.md)頁面中的部分欄位僅適用使用 Windows 10 的客戶，因此您在建立平台專屬 Store 清單時，不會看到所有相同選項。 例如，您將無法新增預告片至平台專屬 Microsoft Store 清單中，因為預告片只會對使用 Windows 10 版本 1607 或更新版本的客戶顯示。 

您可以繼續編輯平台專屬的清單，視需要進行變更的特定作業系統版本上的客戶。


## <a name="removing-a-platform-specific-store-listing"></a>移除平台專屬的 Microsoft Store 清單

如果您建立平台專屬的 Microsoft Store 清單，並稍後決定您傾向在該作業系統上向客戶顯示您的預設 Microsoft Store 清單，請選取該清單旁的 **\[刪除\]** 連結。

在確認您想要為這些客戶顯示預設 Microsoft Store 清單之後，選取 **\[確定\]**。 將會移除平台專屬的 Microsoft Store 清單 (針對其適用的所有語言)，該作業系統版本的客戶現在將會看到您的預設 Microsoft Store 清單。 如果您改變心意，可以依照上述步驟，為該作業系統建立另一個平台專屬的 Microsoft Store 清單。
 

 




