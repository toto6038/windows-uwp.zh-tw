---
author: jnHs
Description: You can create Store listings for your apps without using the Dev Center dashboard by exporting your listings in a .csv file, entering your info and assets, and then importing the updated file.
title: 匯入及匯出 Store 清單
ms.author: wdg-dev-content
ms.date: 03/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 匯入 Store 清單, 匯出 Store 清單, 匯入匯出, Store 清單 csv
ms.localizationpriority: medium
ms.openlocfilehash: 0e9b23f21f87bf6caeb2cbee97a854bc8202c0b3
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2799549"
---
# <a name="import-and-export-store-listings"></a>匯入及匯出 Store 清單

除了[直接在儀表板中輸入 Store 清單的資訊](create-app-store-listings.md)，您也可以選擇藉由將清單匯出為 .csv 檔案、輸入您的資訊和資產，然後匯入更新的檔案，來新增或更新資訊。 您可以使用此方法，從頭開始建立清單或更新您已經建立的清單。

如果您想為您的產品建立或更新多種語言的 Store 清單，此選項會很有用，因為您可將相同資訊複製/貼上至多個欄位，並輕鬆進行應套用至特定語言的任何變更。 不過，您無法使用此方法來針對應用程式建立或更新[平台特定 Store 清單](create-platform-specific-store-listings.md)。 

> [!TIP]
> 您也可以使用此功能，匯入及匯出附加元件的 Store 清單詳細資料。 對於附加元件，除了[只包含附加元件的相關欄位](#add-ons)以外，程序運作方式是相同的。

請記住，您永遠可在開發人員中心儀表板中直接建立或更新清單 (即使您先前已使用匯入/匯出方法)。 如果只是要進行一個簡單變更，在儀表板中直接更新可能比較容易，但您可以隨時使用任一個方法。

## <a name="export-listings"></a>匯出清單

在應用程式提交概觀頁面上，按一下 **\[匯出清單\]** (在 **\[Store 清單\]** 區段) 來建立 UTF-8 編碼的 .csv 檔案。 將此檔案儲存到您電腦上的位置。

您可以使用 Microsoft Excel 或其他編輯器來編輯這個檔案。 請注意，Office 365 版本的 Excel 會讓您將 .csv 檔案儲存為 **CSV UTF-8 (逗號分隔) (*.csv)**，但其他版本可能不支援此功能。 您可以在 [Excel 2016 新功能公告](https://support.office.com/en-us/article/What-s-new-in-Excel-2016-for-Windows-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73)找到哪些版本的 Excel 支援這項功能的詳細資料，並在[這裡](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16)找到使用各種不同編輯器編碼為 UTF-8 的詳細資訊。
      
如果您尚未建立產品的任何清單，匯出的 .csv 檔案不會包含任何自訂資料。 您會看到 **Field**、**ID**、**Type** 和 **default** 欄，以及對應至可能會出現在 Store 清單中每個項目的列。

如果您已經建立清單（或已上傳套件），您也會看到標示為語言-地區設定代碼的欄，該代碼對應到您所建立的（或我們在您的套件偵測到的）每個清單的語言，以及您先前已提供的任何清單資訊。
     
以下是匯出的 .csv 檔案中各欄內容的概觀：
- **Field** 欄包含 Store 清單中每個部分的相關名稱。 這些對應於您在儀表板中建立 Store 清單時可以提供的相同項目，雖然有些名稱稍微不同。 對於您可以輸入多個類型相同的項目，您會看到多列，直到可提供的列數上限。 例如，對於 **App features**，您會看到 **Feature1**、**Feature2** 等等，直到 **Feature20**（因為最多可提供 20 個功能）。
- **ID** 欄包含開發人員中心為每個欄位關聯的數字。 
- **Type** 欄提供應該為該欄位提供何種資訊類型的一般指引，例如 **Text** (文字) 或 **Relative path (or URL to file in Dev Center)** (相對路徑 (或開發人員中心中的檔案 URL))。 
- **default** 欄（及任何標示語言-地區設定代碼的欄）代表 Store 清單中每個部分的相關聯文字或資產。 您可以編輯這些欄中的欄位，對您的 Store 清單進行更新。

>[!IMPORTANT]
> 請勿變更 **Field**、**ID** 或 **Type** 欄的任何資訊。 這些欄中的資訊必須維持不變，以處理匯入檔案。

## <a name="update-listing-info"></a>更新清單資訊

已匯出清單並儲存 .csv 檔案之後，您就可以直接在 .csv 檔案中編輯清單資訊。 

除了 **default** 欄，每個您已經建立清單的語言也有自己的欄。 您對欄的變更會套用到該語言的描述。 您可以在頂端列中的下一個空白欄新增語言-地區設定代碼，建立新語言的清單。 如需有效語言-地區設定代碼的清單，請參閱[支援的語言](supported-languages.md)。

您可以使用 **default** 欄，輸入您想要在所有應用程式描述中共用的資訊。 如果特定語言的欄位保留空白，該語言將會使用 default 欄中的資訊。 您可以輸入特定語言的其他資訊，覆寫該語言的該欄位。

大部分的 Store 清單欄位都是選擇性的。 每個清單都需要 **Description** 和一個螢幕擷取畫面。對於沒有相關套件的語言，您也需要提供 **Title** 以指出您保留的 app 名稱應該用於該清單。 針對其他所有欄位，如果您不想要包含在清單中，您可以將欄位保留空白。 請記住，如果您將特定語言的欄位保留空白，我們會檢查 default 欄中的該欄位是否有資訊。 若是有，則會使用該資訊。 

例如，請考慮下列範例： 

![匯出的清單範例](images/listingimport.png)
     
- 文字 “Default description” 會用於 en-us 和 fr-fr 清單中的 **Description** 欄位。 不過，es-es 清單中的 **Description** 欄位會使用文字 “Spanish description”。 
- 對於 **ReleaseNotes** 欄位，文字 “English release notes” 會用於 en-us，而文字 “French release notes” 會用於 fr-fr。 不過，es-es 不會顯示版本資訊。

如果您不想對特定欄位進行任何編輯，您可以從試算表刪除整列，**但預告片和其相關縮圖和標題列除外**。 除了這些項目以外，刪除列不會影響清單中該欄位相關聯的資料。 這可讓您移除不想要編輯的任何列，因此您可以專注於要變更的欄位。

刪除一種語言的欄位資訊，而不移除整列，運作方式根據欄位而不同。 對於其 **Type** 是 **Text** 的欄位，刪除欄位中的資訊只會從該語言的清單移除該項目。  不過，刪除影像欄位中的資訊，例如螢幕擷取畫面或商標，沒有任何影響。除非您直接在開發人員中心編輯，將它移除，否則仍然可以使用先前的影像。 刪除預告片欄位資訊，實際上會從開發人員中心移除預告片，所以這麼做之前，請務必確定您有任何所需檔案的複本。

您匯出清單中的許多欄位需要文字項目，例如上面範例中的 **Description** 和 **ReleaseNotes** 欄位。 這些類型的欄位中，只要將適當的文字輸入每一種語言的欄位中。 請務必遵循每個欄位的長度與其他需求。 如需這些需求的詳細資訊，請參閱[建立應用程式 Store 清單](create-app-store-listings.md)。

為對應到影像和預告片等資產的欄位提供資訊，更加複雜一點。 這些資產的 **Type** 是 **Relative path (or URL to file in Dev Center)** (相對路徑 (或開發人員中心中的檔案 URL))，而非 **Text**。 
     
如果您已經為 Store 清單上傳資產，這些資產就會由 URL 表示。 這些 URL 可以重複使用在產品的多個描述中，或甚至在相同開發人員帳戶內不同產品的描述中，所以如果您要的話，可以複製這些 URL 以重複使用在不同的欄位中。

> [!TIP]
> 若要確認哪些資產對應至 URL，您可以在瀏覽器中輸入 URL 以檢視影像（或下載預告片影片）。  您必須登入開發人員中心帳戶，此 URL 才能正常運作。

如果您想要使用之前尚未加入開發人員中心的新資產，您可以匯入清單做為資料夾，而非 .csv 單一檔案。 您將需要建立包含 .csv 檔案的資料夾。 然後，將影像新增至該相同的資料夾 (可能位於根資料夾中或子資料夾中)。 您將需要在欄位中輸入完整路徑，包括根資料夾名稱。

> [!TIP]
> 以資料夾方式匯入清單時，若要得到最佳結果，請務必使用最新版的 Microsoft Edge、Chrome 或 Firefox。

例如，如果根資料夾名為 **my_folder**，而您想要對 **DesktopScreenshot1** 使用名為 **screenshot1.png** 的影像，您可以將 screenshot1.png 新增到該根資料夾，然後在 **DesktopScreenshot1** 欄位中輸入 **my_folder/screenshot1.png**。 如果您在根資料夾中建立 images 資料夾，然後在那裡放置 screenshot1.jpg，則輸入 **my_folder/images/screenshot1.png**。 請注意，使用資料夾匯入清單之後，下一次您匯出清單時，影像路徑將會轉換為開發人員中心中的檔案 URL。 您可以複製和貼上這些 URL，重新使用它們（例如，在數種清單語言中使用相同資產）。 

> [!IMPORTANT]
> 如果您匯出的清單包含預告片，請注意，從 .csv 檔案刪除預告片或其縮圖影像的 URL，會從儀表板完全移除刪除的檔案，而且您將無法在那裡存取刪除的檔案（除非在另一個清單中尚未刪除該檔案）。 

## <a name="import-listings"></a>匯入清單

一旦您將所有變更輸入到 .csv 檔案（並包含您想要上傳的任何資產），上傳之前需要儲存檔案。 如果您使用支援 UTF-8 編碼的 Microsoft Excel 版本，請務必選取 **\[另存新檔\]**，並使用 **CSV UTF-8 (逗號分隔) (*.csv)** 格式。 如果您使用不同的編輯器檢視和編輯 .csv 檔案，上傳之前請確認 .csv 檔案是 UTF-8 編碼。

當您準備上傳更新的 .csv 檔案與匯入清單資料，請選取提交概觀頁面上的 **\[匯入清單\]**。 如果您只匯入.csv 檔案，請選擇 **\[匯入 .csv\]**，瀏覽至您的檔案，然後按一下 **\[開啟\]**。 如果您要匯入具有影像檔案的資料夾，請選擇匯入資料夾，瀏覽至您的資料夾，按一下 **\[選取資料夾\]**。 請確定資料夾中只有一個 .csv 檔案，以及要上傳的任何資產。 

當我們處理匯入的 .csv 檔案，顯示的進度列會反映匯入和驗證狀態。 這可能需要花費一些時間，尤其是當您有許多清單及/或影像檔案。 

如果我們偵測到任何問題，您會看到注意事項，表示您需要執行任何需要的更新，並再試一次。 選取 **\[檢視錯誤\]** 連結，以查看哪些欄位無效和原因。 您將需要在 .csv 檔案中修正這些問題（或取代任何無效的資產），然後再次匯入清單。

> [!TIP]
> 您可以透過 **\[View errors for last import\]** (檢視上次匯入的錯誤) 連結，稍後再次存取此資訊。

.csv 檔案中的所有錯誤解決之前，檔案的資訊不會儲存到開發人員中心，甚至也不會儲存無錯誤的欄位。 您匯入無錯誤的 .csv 檔案之後，您所提供的清單資訊就會儲存在開發人員中心，並會用於該次提交。

您可以匯入另一個更新的 .csv 檔案，或是直接在開發人員中心中進行變更，繼續更新您的清單。

## <a name="add-ons"></a>附加元件

對於附加元件，匯入及匯出 Store 清單使用上述相同的程序，不過您只會看到[附加元件存放區清單](create-add-on-store-listings.md)的三個相關欄位：**Description**、**Title** 和 **StoreLogo300x300** (在開發人員中心的 Store 中清單頁面中稱為 **\[圖示\]**)。 **Title** 欄位是必要的，其他兩個欄位則是選擇性的。

請注意，您必須藉由瀏覽到附加元件提交概觀頁面，針對應用程式的每個附加元件分別匯入及匯出 Store 清單。


