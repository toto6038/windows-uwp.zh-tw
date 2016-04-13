---
您可以將客戶連結到您 app 的市集清單頁面來協助客戶發現該 app。
連結至您的 app
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
---

# 連結至您的 app


您可以將客戶連結到您 app 的市集清單頁面來協助客戶發現該 app。

## 取得您的 app 市集清單的連結


您可以在儀表板中每個 app 的 [**應用程式管理**] 區段中 [[應用程式身分識別](view-app-identity-details.md)] 頁面上找到連至您 app 市集清單的連結。

這個連結的格式為 **`https://www.microsoft.com/store/apps/<your app's product ID>`**。

當客戶按一下此連結時，會開啟您 app 的網頁清單頁面。 如果您的 app 適用於客戶的裝置，市集 app 也會啟動並顯示您的 app 清單。

> **注意** 根據您的目標作業系統版本而定，您可能會看到一個以上的連結。 所有 app 都會顯示 Windows 10 的 URL，對於任何作業系統都一樣。 您可能會看到適用於 Windows 8.1 和更早版本和/或 Windows Phone 8.1 和舊版的其他連結，這僅適用於指定的作業系統版本上。

 

## 使用 Windows 市集徽章連結到您的 app 市集清單


您可以使用自訂的徽章直接連結到您的 app 清單，讓客戶知道您的 app 位於 Windows 市集中。

若要建立您的徽章，請瀏覽 [Windows 市集徽章](http://go.microsoft.com/fwlink/p/?LinkID=534236)頁面。 若要使用此表格，您需要有您 app 的產品識別碼，才能產生徽章與連結。 此識別碼是 [**應用程式管理**] 區段中 [[應用程式身分識別](view-app-identity-details.md)] 頁面上顯示的 **Windows 10 的 URL** 的最後 12 個字元。

> **注意** 如需使用 Windows 市集徽章的詳細資訊，請參閱 [app 行銷指導方針](app-marketing-guidelines.md)。

 

## 直接連結到 Windows 市集中您的 app


您可以使用 **ms-windows-store:** URI 配置來建立連結，用來啟動 Windows 市集且不需要開啟瀏覽器就可以直接連到您的 app 清單頁面。

如果您知道您的使用者是在 Windows 裝置上且您想要這些使用者可以直接進入市集中的清單頁面，這些連結會非常有用；例如，在檢查過瀏覽器中的使用者代理字串以確認使用者的作業系統後，或您已經透過 UWP app 進行通訊，您可能會想要套用此通訊協定。

若要使用 Windows 市集通訊協定來直接連結至您 app 的市集清單，請將您 app 的產品識別碼附加到此連結：

`ms-windows-store://pdp/?ProductId=`

如需有關使用 Windows 市集通訊協定的詳細資訊，請參閱[啟動 Windows 市集 app](https://msdn.microsoft.com/library/windows/apps/mt228343)。

 

 






<!--HONumber=Mar16_HO1-->


