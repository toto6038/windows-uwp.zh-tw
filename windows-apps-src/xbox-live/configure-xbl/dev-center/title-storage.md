---
title: 標題在合作夥伴中心內的儲存體設定
description: 了解如何在合作夥伴中心設定標題的儲存體
ms.date: 04/24/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 遊戲、 uwp、 windows 10，其中一個 Xbox、 標題儲存體、 合作夥伴中心
ms.openlocfilehash: d32f9f2f4e003db50ad560acc513511a850d43b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612093"
---
# <a name="configure-storage-for-you-title-in-partner-center"></a>在合作夥伴中心內設定儲存體，您的標題

Xbox Live，可讓您儲存您的遊戲中的標題儲存體服務透過雲端相關聯的資料。 標題的儲存體組態 頁面可讓您判斷哪些類型的雲端儲存體服務您的遊戲會允許，以及上傳要用於全域儲存體的檔案。

您可以找到 [Xbox Live 的標題儲存體組態] 頁面，方法是前往[夥伴](https://partner.microsoft.com/dashboard)，選擇您的應用程式，從**概觀**或是**產品**，開啟**服務**卸除，並選取**Xbox Live**。 創作者計劃中的開發人員就必須按一下**顯示的選項**中**雲端節省和儲存體**區段及其組態 頁面，來查看標題儲存體組態選項。 具有完整的 Xbox Live 功能集就必須找出**標題儲存體**瀏覽至 [標題儲存體組態] 頁面的連結。

標題儲存體組態有兩個主要區段。 標題的儲存體的設定 區段中，全域儲存體檔案管理 區段。

## <a name="section-1-title-storage-settings"></a>第 1 節：標題儲存體設定

![標題儲存體設定螢幕擷取畫面](../../images/dev-center/title-storage/title-storage-settings.JPG)

在本節中您可以讓核取方塊，四個儲存體類型的任何**Active**資料行。 選擇您標題的儲存體類型之後您可以選擇是否讀取資料，將限制為擁有，依序按一下 儲存體類型資料列的玩家**擁有者**只選項按鈕，或公開共用，按一下**Everyone**選項按鈕。 如果您選取**只有擁有者**的給定**儲存體類型**則該類型的標題儲存體資料只會讀取該資料產生的玩家。 如果您選取**Everyone**的給定**儲存體類型**則會到所有播放程式可讀取該類型的標題儲存體資料。 撰寫或修改儲存的資料才可在所有情況下產生它的使用者。

## <a name="storage-types"></a>儲存體類型

有四種儲存體類型可以在標題儲存體組態 頁面中啟用。 停留在每一個儲存體類型的名稱旁邊的資訊圖示，或閱讀下表，您可以找到每個儲存體類型的描述。

|存放類型 |描述 |使用方式範例  |
|---------|---------|---------|
|全域             |資料上傳到合作夥伴中心的可讀取的任何裝置，且每個使用者進行存取。 可以只寫入至合作夥伴中心的開發人員上傳。 | 通告所有的使用者，透過在遊戲新聞摘要的更新。     |
|連接的儲存空間  |可讓背景同步處理的 XboxOne 和 Windows 10 遊戲的遊戲相關資料。 強固的錯誤容錯遊戲儲存服務。 可讀取的任何裝置、 可以寫入至 Xbox One 和 Windows 10 裝置    | 儲存為個別的使用者，以便在不同的主控台上播放的檔案。         |
|通用          |網路存取的 blob 儲存體，可讓讀取/寫入存取權並不是 Xbox 360 或 Windows Phone 的任何裝置。 可以讀取 Android 和 IOS 裝置。      | 儲存播放或可從多個 Windows 裝置的其他統計資料。        |
|受信任            |只可寫入的 Xbox One、 Xbox 360 與 Windows Phone 網路存取的 blob 儲存體。 可以讀取任何裝置。 可以讀取由 Android 和 IOS。     | 儲存多人遊戲玩家的排名。        |

## <a name="section-2-global-storage-file-management"></a>第 2 節：通用的儲存體檔案管理

![全域儲存體檔案管理螢幕擷取畫面](../../images/dev-center/title-storage/global-storage-file-management.JPG)

若要查看完整的全域儲存體檔案管理選項，您必須按一下**顯示選項**下拉式清單。 在本節中，您可以加入檔案和可存取的資料夾如果**Global**儲存體類型設為作用中的標題儲存體設定。 您的遊戲需要發行用來測試以將檔案新增這一節。 如果測試未適當地發行您的遊戲，您可能看到的標題儲存體組態 頁面頂端的警告。

## <a name="manage-global-storage-files"></a>管理全域儲存體檔案

全域儲存體檔案管理可讓您上傳和下載檔案，可針對通用的儲存體。 擁有您的標題，任何人都可存取這些檔案，並打算在玩這個遊戲的所有球員之間共用。 若要查看全域儲存體檔案選項，您必須按一下**顯示選項**區段標題旁邊下拉式清單。 若要將您的第一個檔案按一下 **+ 新增...** 連結。 您接著將提供通用的儲存體檔案中加入新的檔案或資料夾的選項。

### <a name="new-folders"></a>新的資料夾

當加入新的資料夾，全域儲存體檔案，只需要將資料夾命名，然後按一下**建立** 按鈕。 您的新資料夾會出現在檔案總管 中的資料表。

![新增資料夾 對話方塊](../../images/dev-center/title-storage/add-folder-global-storage-filled.JPG)

若要將檔案新增至您的資料夾您必須上傳至資料夾直接藉由推送資料夾**動作**按鈕:"**...**"，然後選取**將檔案上傳**。 您無法藉由拖放到資料夾，在檔案總管 資料表中的檔案。 使用**建立的資料夾**中的動作**動作**功能表的資料夾將建立的子資料夾。 選擇**刪除**中的動作**動作**功能表的資料夾將會刪除該資料夾及其所有內容。

### <a name="new-files"></a>新的檔案

將新檔案新增至全域儲存體時您將會提示您上傳您的電腦檔案總管 中的檔案，然後從選擇其中的三個檔案類型、 二進位、 Config 和 JSON 要求。 除了能夠上傳的檔案 **+ 新增...** 按鈕，您可以拖放檔案從您的電腦檔案總管 中的資料表。

> [!WARNING]
> 您無法將資料夾拖曳至 [檔案總管] 中的資料表，嘗試這個動作會被視為檔案的資料夾，並將無法如預期般運作。

檔案管理動作：![檔案管理 gif](../../images/dev-center/title-storage/global-storage-management.gif)

#### <a name="file-types"></a>檔案類型

* 二進位-二進位型別應該用於影像、 音效和自訂資料。 此資料必須是易記的 HTTP。
* 設定-設定檔保存您的遊戲的相關資訊，並可以有動態查詢會傳回根據某些輸入的值。
* JSON-.json 檔案。 這些檔案包含有關遊戲，類似於組態檔的一些資訊。

## <a name="further-reading"></a>進一步閱讀

若要深入了解 Xbox Live 的儲存體平台，請閱讀[標題儲存體文件](../../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)這可提供更多有關 Universal、 信任、 全域儲存體和儲存體的檔案類型，而[連接的儲存體文件](../../storage-platform/connected-storage/connected-storage-overview.md)，這會讓您詳細資訊，讓您可以繼續您的遊戲裝置之間，在雲端中儲存遊戲的進度。