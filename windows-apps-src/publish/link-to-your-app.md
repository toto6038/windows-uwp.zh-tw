---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: 應用程式的連結
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 連結, windows 市集通訊協定, 連結到應用程式, 應用程式的連結
ms.localizationpriority: medium
ms.openlocfilehash: 0025321aa73a66cc0a976bd347e613de3c3c4765
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2836502"
---
# <a name="link-to-your-app"></a>應用程式的連結


您可以幫助探索您的應用程式連結至您的應用程式資訊清單 Microsoft 存放區中的客戶。

## <a name="getting-the-link-to-your-apps-store-listing"></a>取得您的 app 市集清單的連結

若要取得您的應用程式儲存區清單的 URL，請瀏覽至 **\[應用程式管理\]** 區段中應用程式的[應用程式身分識別](view-app-identity-details.md) 頁面。 URL 的格式為 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

當客戶按一下此連結時，會開啟您 App 的網頁清單頁面。 在 Windows 裝置上，市集應用程式也將啟動並顯示您的應用程式清單。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>連結至您的應用程式存放區清單與 Microsoft 存放區名牌

您可以直接至您的應用程式資訊清單具有讓客戶知道您的應用程式是在 Microsoft 存放區中的自訂名牌連結。

若要建立您名牌，請造訪[Microsoft 存放區徽章](http://go.microsoft.com/fwlink/p/?LinkID=534236)頁面。 您需要有您 app 的 12 個字元的**市集識別碼**，才能產生徽章與連結。 您可以在 **\[應用程式管理\]** 區段的[應用程式身分識別](view-app-identity-details.md)頁面上找到 App 的**市集識別碼**。

> [!NOTE]
> 請參閱[App 行銷指導方針](app-marketing-guidelines.md)資訊與相關的 Microsoft 存放區名牌使用的需求。


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>直接連結您在 Microsoft 存放區中的應用程式

您可以建立的啟動 Microsoft 存放區並將直接移至您的應用程式清單] 頁面上沒有開啟瀏覽器使用的連結**毫秒 windows 市集：** URI 配置。

如果您知道您的使用者是在 Windows 裝置上且您想要這些使用者可以直接進入市集中的清單頁面，這些連結會非常有用。 例如，在檢查過瀏覽器中的使用者代理字串以確認使用者的作業系統支援市集後，或您已經透過 UWP app 進行通訊，您可能會想要使用此連結。

若要使用此 URI 配置直接連結至您的應用程式存放區列出，附加您的應用程式存放區 ID 此連結：

`ms-windows-store://pdp/?ProductId=`

如需關於使用 Microsoft 存放區通訊協定的詳細資訊，請參閱[啟動 Microsoft 應用程式](../launch-resume/launch-store-app.md)。

 

 




