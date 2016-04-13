---
遵循這些指導方針來準備要提交到 Windows 市集的 app 套件。
應用程式套件需求
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
---

# 應用程式套件需求

遵循這些指導方針來準備要提交至 Windows 市集的 app 套件。

## 在您建置 Windows 市集的 app 套件之前

請確定[使用 Windows 應用程式認證套件測試您的 app](https://msdn.microsoft.com/library/windows/apps/mt186449)。 我們也建議您在不同類型的硬體上測試您的 app。 請注意，除非您的 app 經過我們的認證，並在 Windows 市集發行，否則只能在有開發人員授權的電腦上安裝和執行 app。

## 使用 Microsoft Visual Studio 建置應用程式套件

如果您使用 Microsoft Visual Studio 做為開發環境，則您已經擁有內建的工具可以快速並輕易地建立應用程式套件。 如需詳細資訊，請參閱[封裝 app](https://msdn.microsoft.com/library/windows/apps/mt270969)。

> **注意：**務必讓您的所有檔案名稱都使用 ANSI。 


在 Visual Studio 中建立套件時，請確定您已經使用與開發人員帳戶相關聯的相同 Microsoft 帳戶登入。 套件資訊清單的某些部分具有與您帳戶相關的特定詳細資料。 系統會自動偵測並新增此資訊。

當您建置 app 套件時，Visual Studio 可以建立 .appx 檔案或 .appxupload 檔案 (或適用於 Windows Phone 8.1 及較舊版本的 .xap 檔案)。 對於目標為 Windows 10 的 app，請一律在 [套件][](upload-app-packages.md) 頁面上傳 .appxupload 檔案。 如需針對市集封裝 UWP app 詳細資訊，請參閱[封裝適用於 Windows 10 的通用 Windows app](http://go.microsoft.com/fwlink/p/?LinkId=620193 )。

您的 app 套件不一定要以根目錄在受信任之憑證授權單位的憑證簽署。

### App 套件組合

針對 Windows 8.1、Windows Phone 8.1 及更新版本設計的 app 可以產生 app 套件組合 (.appxbundle)，以便縮減供使用者下載的 app 大小。 如果您已經定義了語言特定的資產、各種大小影像的資產，或是套用到特定 Microsoft DirectX 版本的資源，這通常很有幫助。

> **注意：**一個應用程式套件組合可以包含所有架構的套件。 只能針對每個目標 OS 提交一個套件組合。


有了 app 套件組合，使用者只需要下載相關的檔案，不需要下載所有可能的資源。 如需 app 套件組合的詳細資訊，請參閱[封裝 app](https://msdn.microsoft.com/library/windows/apps/mt270969) 和[封裝適用於 Windows 10 的通用 Windows app](http://go.microsoft.com/fwlink/p/?LinkId=620193 )。

## 手動建置 app 套件

如果您沒有使用 Visual Studio 建立套件，必須[手動建立套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211476)。

如需完整的資訊清單詳細資料和需求，請務必檢閱 [App 套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474)文件。 您的資訊清單必須遵循套件資訊清單結構描述才能通過認證。

您的資訊清單必須包含一些關於帳戶與 app 的特定資訊。 您可以在儀表板中，查看 app 概觀頁面 [應用程式管理]**** 區段中的[檢視應用程式身分識別詳細資料](view-app-identity-details.md)，來找到此資訊。

> **注意：**資訊清單中的值區分大小寫。 空格與其他標點符號也必須相符。 請仔細輸入相關值，並檢查以確保正確無誤。


App 套件組合使用不同的資訊清單。 如需應用程式套件組合資訊清單的詳細資料和需求，請檢閱[套件組合資訊清單](https://msdn.microsoft.com/library/windows/apps/dn263089)文件。

> **提示：**在您提交套件之前，請務必執行 [Windows 應用程式認證套件](https://msdn.microsoft.com/library/windows/apps/mt186449)。 這有助於協助您判斷資訊清單是否會造成認證或提交失敗的任何問題。


如果您的 app 具有多個套件，每個套件中的 app 資訊清單元素必須是相同的 (針對每個目標 OS)：

-   [**套件/功能**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**套件/相依性**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**套件/資源**](https://msdn.microsoft.com/library/windows/apps/br211462)

## 套件格式需求

您的應用程式套件必須符合下列需求。

| 應用程式套件屬性 | 需求                                                          |
|----------------------|----------------------------------------------------------------------|
| 套件大小         | .appxbundle：每個套件組合上限為 25 GB <br>以 Windows 8.1 為目標的 .appx 套件：每個套件上限為 8 GB <br> 以 Windows 8 為目標的 .appx 套件：每個套件上限為 2 GB <br> 以 Windows Phone 8.1 為目標的 .appx 套件：每個套件上限為 4 GB <br> .xap 套件：每個套件上限為 1 GB                                                                           |
| 區塊對應雜湊     | SHA2-256 演算法                                                   |
 

## StoreManifest XML 檔案

StoreManifest.xml 是選用的組態檔，可能包含在 app 套件中。 它的用途是啟用封裝資訊清單沒有涵蓋的功能，例如將您的 app 宣告為 Windows 市集裝置 app，或是宣告套件仰賴的需求適用於某裝置。 StoreManifest.xml 會與 app 套件一起提交，且必須位於您 app 主要專案的根資料夾中。 如需詳細資訊，請參閱 [StoreManifest 結構描述](https://msdn.microsoft.com/library/windows/apps/mt617325)。

 

 






<!--HONumber=Mar16_HO1-->


