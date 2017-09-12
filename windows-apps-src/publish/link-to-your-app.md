---
author: jnHs
Description: "您可以將客戶連結到您 app 的市集清單頁面來協助客戶發現該 app。"
title: "應用程式的連結"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 連結, windows 市集通訊協定, 連結到應用程式, 應用程式的連結"
ms.openlocfilehash: 2d0750493926937a6326c5f72f568d4294b137c5
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2017
---
# <a name="link-to-your-app"></a>應用程式的連結


您可以將客戶連結到您 app 的市集清單頁面來協助客戶發現該 app。

## <a name="getting-the-link-to-your-apps-store-listing"></a>取得您的 app 市集清單的連結

若要取得您的應用程式儲存區清單的 URL，請瀏覽至 **\[應用程式管理\]** 區段中應用程式的[應用程式身分識別](view-app-identity-details.md) 頁面。 URL 的格式為 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

當客戶按一下此連結時，會開啟您 App 的網頁清單頁面。 在 Windows 裝置上，市集應用程式也將啟動並顯示您的應用程式清單。


## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>使用 Windows 市集徽章連結到您的 app 市集清單

您可以使用自訂的徽章直接連結到您的 app 清單，讓客戶知道您的 app 位於 Windows 市集中。

若要建立您的徽章，請瀏覽 [Windows 市集徽章](http://go.microsoft.com/fwlink/p/?LinkID=534236)頁面。 您需要有您 app 的 12 個字元的**市集識別碼**，才能產生徽章與連結。 您可以在 **\[應用程式管理\]** 區段的[應用程式身分識別](view-app-identity-details.md)頁面上找到 App 的**市集識別碼**。

> [!NOTE]
> 如需有關使用 Windows 市集徽章的相關資訊和需求，請參閱[應用程式行銷指導方針](app-marketing-guidelines.md)。


## <a name="linking-directly-to-your-app-in-the-windows-store"></a>直接連結到 Windows 市集中您的 app

您可以使用 **ms-windows-store:** URI 配置來建立連結，用來啟動 Windows 市集且不需要開啟瀏覽器就可以直接連到您的 app 清單頁面。

如果您知道您的使用者是在 Windows 裝置上且您想要這些使用者可以直接進入市集中的清單頁面，這些連結會非常有用。 例如，在檢查過瀏覽器中的使用者代理字串以確認使用者的作業系統支援市集後，或您已經透過 UWP app 進行通訊，您可能會想要使用此連結。

若要使用 Windows 市集通訊協定來直接連結至您 app 的市集清單，請將您 app 的市集識別碼附加到此連結：

`ms-windows-store://pdp/?ProductId=`

如需有關使用 Windows 市集通訊協定的詳細資訊，請參閱[啟動 Windows 市集 app](../launch-resume/launch-store-app.md)。

 

 




