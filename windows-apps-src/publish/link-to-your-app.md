---
Description: 您可以協助客戶探索您的應用程式連結至 Microsoft Store 中的應用程式的清單。
title: 應用程式的連結
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 連結, windows 市集通訊協定, 連結到應用程式, 應用程式的連結
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661533"
---
# <a name="link-to-your-app"></a>應用程式的連結


您可以協助客戶探索您的應用程式連結至 Microsoft Store 中的應用程式的清單。

## <a name="getting-the-link-to-your-apps-store-listing"></a>取得您的 app 市集清單的連結

若要取得您的應用程式儲存區清單的 URL，請瀏覽至 **\[應用程式管理\]** 區段中應用程式的[應用程式身分識別](view-app-identity-details.md) 頁面。 URL 的格式為 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

當客戶按一下此連結時，會開啟您 App 的網頁清單頁面。 在 Windows 裝置上，市集應用程式也將啟動並顯示您的應用程式清單。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>連結至您的應用程式存放區清單，與 Microsoft Store 標誌

您可以直接連結至您的應用程式使用自訂的徽章，以讓客戶知道您的應用程式是在 Microsoft Store 中的清單。

若要建立您的商標，請瀏覽[Microsoft Store 徽章](https://go.microsoft.com/fwlink/p/?LinkID=534236)頁面。 您需要有您 app 的 12 個字元的**市集識別碼**，才能產生徽章與連結。 您可以在 **\[應用程式管理\]** 區段的[應用程式身分識別](view-app-identity-details.md)頁面上找到 App 的**市集識別碼**。

> [!NOTE]
> 請參閱[應用程式行銷指導方針](app-marketing-guidelines.md)資訊和相關 Microsoft Store 徽章的使用需求。


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>直接連結到 Microsoft Store 中的應用程式

您可以建立一個連結，啟動 Microsoft Store，並會直接移至您的應用程式清單頁面而不需要開啟瀏覽器使用**ms windows 市集：** URI 配置。

如果您知道您的使用者是在 Windows 裝置上且您想要這些使用者可以直接進入市集中的清單頁面，這些連結會非常有用。 例如，在檢查過瀏覽器中的使用者代理字串以確認使用者的作業系統支援市集後，或您已經透過 UWP app 進行通訊，您可能會想要使用此連結。

若要使用此 URI 配置為直接連結至您的應用程式存放區清單，請將您的應用程式存放區識別碼附加至這個連結：

`ms-windows-store://pdp/?ProductId=`

如需有關使用 Microsoft Store 通訊協定的詳細資訊，請參閱[啟動 Microsoft 應用程式](../launch-resume/launch-store-app.md)。

 

 




