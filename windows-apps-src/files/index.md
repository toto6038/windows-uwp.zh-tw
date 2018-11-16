---
author: laurenhughes
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: 檔案、資料夾和媒體櫃
description: 了解如何讀取和寫入應用程式設定、檔案和資料夾選擇器，以及特殊的沙箱式位置，例如影片/音樂媒體櫃。
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80c6292c12568d7f1017ca67707689ce9539c804
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980365"
---
 # <a name="files-folders-and-libraries"></a>檔案、資料夾和媒體櫃


您可以使用 [Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346)、[Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) 和 [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928) 命名空間中的 API，以在檔案中讀取與寫入文字和其他資料格式，以及管理檔案和資料夾。 在本節中，您也將了解讀取和寫入 app 設定、檔案和資料夾選擇器，並了解特殊的沙箱式位置，例如影片/音樂媒體櫃。

| 主題 | 描述  |
|-------|--------------|
| [列舉和查詢檔案和資料夾](quickstart-listing-files-and-folders.md) | 存取位於資料夾、媒體櫃、裝置或網路位置中的檔案和資料夾。 您也可以建構檔案和資料夾查詢來查詢位置中的檔案和資料夾。 |
| [建立、寫入和讀取檔案](quickstart-reading-and-writing-files.md) | 使用 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 物件讀取和寫入檔案。 |
| [取得檔案屬性](quickstart-getting-file-properties.md) | 取得由 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 物件所表示檔案的屬性 (最上層、基本及延伸)。 |
| [使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md) | 讓使用者與選擇器互動以存取檔案和資料夾。 您可以使用 [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881) 存取資料夾。 |
| [使用選擇器儲存檔案](quickstart-save-a-file-with-a-picker.md) | 使用 [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者指定讓您的 app 儲存檔案的名稱和位置。 |
| [存取 HomeGroup 內容](quickstart-accessing-homegroup-content.md) | 存取儲存在使用者 HomeGroup 資料夾中的內容，包括圖片、音樂及視訊。 |
| [判斷 Microsoft OneDrive 檔案的可用性](quickstart-determining-availability-of-microsoft-onedrive-files.md) | 使用 [StorageFile.IsAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) 屬性判斷 Microsoft OneDrive 檔案是否可供使用。 |
| [音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | 將現有的音樂、圖片或視訊資料夾新增到對應的媒體櫃中。 您也可以從媒體櫃中移除資料夾、取得媒體櫃中的資料夾清單，以及尋找已儲存的相片、音樂和影片。 |
| [追蹤最近使用的檔案和資料夾](how-to-track-recently-used-files-and-folders.md) | 將使用者經常存取的檔案新增到您 app 的最近使用清單中 (MRU)，以追蹤這些檔案。 平台會根據項目上次存取的時間來排序項目，並在達到清單的 25 個項目數限制時移除最舊的項目，為您管理 MRU。 所有 app 都有自己的 MRU。 |
| [存取 SD 記憶卡](access-the-sd-card.md) | 您可以在選用的 microSD 記憶卡上儲存和存取非必要的資料，尤其是內部儲存空間有限的低價行動裝置。 |
| [檔案存取權限](file-access-permissions.md) | App 預設可以存取特定的檔案系統位置。 應用程式也可以透過檔案選擇器或宣告功能存取其他位置。 |
| [快速存取 UWP 中的檔案屬性](fast-file-properties.md) | 有效收集程式庫的檔案和其屬性清單以便用於 UWP app。 |

## <a name="related-samples"></a>相關範例
[資料夾列舉範例](http://go.microsoft.com/fwlink/p/?linkid=619993)

[檔案存取範例](http://go.microsoft.com/fwlink/p/?linkid=619995)

[檔案選擇器範例](http://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
