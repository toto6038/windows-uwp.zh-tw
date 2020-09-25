---
Description: 如果您提供了目標為不同作業系統的套件，就可以選擇為不同的目標作業系統自訂哪些市集清單的部分。
title: 建立平台專屬的市集清單
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 自訂, 清單, 描述, 舊版
ms.localizationpriority: medium
ms.openlocfilehash: b0008095ac027a1ef1d17b655610a3ad50a6596b
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220501"
---
# <a name="create-platform-specific-store-listings"></a>建立平台專屬的市集清單


如果您先前發佈的應用程式有以不同作業系統為目標的套件，您可以選擇針對舊版作業系統版本 (Windows 8. x 或更早版本及/或 Windows Phone 8.x 或更早版本的) 自訂 Store 清單的部分。 

Windows 10 (的客戶（包括 Xbox) ）一律會看到預設的 [Store 清單](create-app-store-listings.md)。 除非您已使用支援一或多個較早作業系統版本的套件發佈您的應用程式，否則您不會看到建立平臺特定存放區清單的選項。 

> [!IMPORTANT]
> 您無法再上傳使用 Windows Phone 8.x SDK (s) 建立的新 XAP 套件。 已存在於 XAP 套件中的應用程式將會繼續在 Windows 10 行動裝置版裝置上運作。 如需詳細資訊，請參閱這 [篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

如果您想要提及只出現在一個作業系統版本中的功能，或想要提供特定作業系統專用的螢幕擷取畫面， (獨立于裝置類型) 時，平臺特定的存放區清單會很有用。

> [!NOTE]
> 使用某一種語言建立平台專屬的 Microsoft Store 清單時，不會以您應用程式支援之其他語言建立平台專屬的 Microsoft Store 清單。 您必須分別為每個語言建立平台專屬的市集清單。 另請注意，您無法匯 [入和匯出](import-and-export-store-listings.md) 平臺特定清單的 Store 清單資料。


## <a name="creating-a-platform-specific-store-listing"></a>建立平台專屬的市集清單

在 **Store 清單** 頁面頂端附近，如果先前發佈的應用程式包含支援舊版 OS 版本的套件 ( # B1 Windows 8 x 或更早版本，以及（或） Windows Phone 8.x 或更早版本) ，則可以選取 [ **建立平臺特定應用程式] Store 清單**。 選取此選項之後，系統會提示您從您的提交支援的目標作業系統版本中選擇。 一旦您已為應用程式目標的所有舊版作業系統建立平臺特定的存放區清單之後，就無法再進行另一個選擇。

您可以使用預設的 (Windows 10) Store 清單做為起點，這會帶出您針對預設 Store 清單所輸入的適用文字和影像;然後您就可以在儲存之前，先進行任何您想要的變更。 如有需要，您也可以從完全空白的市集清單開始。

按一下 **\[繼續\]** 之後，您的 **\[Microsoft Store 清單\]** 頁面現在將會包含您剛建立的平台專屬 Microsoft Store 清單區段。 本區段將包含一組自己的欄位，其中有**介紹** (必要)、**此版本中的新功能**、**螢幕擷取畫面**、**應用程式磚圖示**、**應用程式功能**和**其他系統需求**。 請務必在您要在自訂市集清單中顯示資訊的每個欄位中輸入資訊，即使它與您的預設市集清單資訊相同，也必須輸入。 如果您讓任何欄位保留空白，則自訂市集清單中的該欄位中將不會出現任何資訊。

> [!IMPORTANT]
> Store 清單的[其他資訊](create-app-store-listings.md#additional-information)區段中欄位無法針對不同的作業系統版本而自訂。
> 
> 此外，因為預設 [Store 清單](create-app-store-listings.md) 頁面中的某些欄位僅適用于 Windows 10 上的客戶，所以在建立平臺特定 Store 清單時，您將不會看到所有相同的選項。 例如，您將無法新增預告片至平台專屬 Microsoft Store 清單中，因為預告片只會對使用 Windows 10 版本 1607 或更新版本的客戶顯示。 

您可以視需要繼續編輯平臺特定清單，以針對特定 OS 版本的客戶進行變更。


## <a name="removing-a-platform-specific-store-listing"></a>移除平台專屬的 Microsoft Store 清單

如果您建立平台專屬的 Microsoft Store 清單，並稍後決定您傾向在該作業系統上向客戶顯示您的預設 Microsoft Store 清單，請選取該清單旁的 **\[刪除\]** 連結。

在確認您想要為這些客戶顯示預設 Microsoft Store 清單之後，選取 **\[確定\]**。 將會移除平台專屬的 Microsoft Store 清單 (針對其適用的所有語言)，該作業系統版本的客戶現在將會看到您的預設 Microsoft Store 清單。 如果您改變心意，可以依照上述步驟，為該作業系統建立另一個平台專屬的 Microsoft Store 清單。
 

 




