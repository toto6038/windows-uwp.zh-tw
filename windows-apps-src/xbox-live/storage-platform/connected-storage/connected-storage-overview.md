---
title: 連接的儲存體概觀
description: 了解如何使用連接儲存體來儲存及載入遊戲資料，在裝置上。
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 40ad13e46e074154d72d7aad236747c3374110ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639053"
---
# <a name="connected-storage"></a>連接的儲存空間
連接的儲存體可讓您儲存遊戲資料和其他相關的狀態資料，應該在裝置之間漫遊的標題。 連接儲存體 API 可讓 Xbox One 和通用 Windows Platform(UWP) 來儲存、 載入和刪除項目資料，是儲存在本機和 Xbox One 或 UWP 標題連接到網際網路時，也同步至雲端的標題。 已儲存的資料將可執行您的標題，進行同步處理之後的其他任何裝置上。 開發人員都能夠離開主目錄的最佳玩體驗盡量精確地儲存項目狀態。 連接的儲存體是可讓您的遊戲在家裡、 辦公室中進行，然後挑選您遊戲的權限離開的地方繼續支援相同遊戲的其他任何裝置上。

## <a name="connected-storage-features"></a>連接的儲存體功能

連接儲存體 API 提供下列功能：

- 應用程式可以快速將儲存最多 16 MB 的資料一次到記憶體緩衝區，然後在本機快取的 HDD 上，系統並上傳至雲端。
- 適用於受管理的合作夥伴和ID@Xbox開發人員：
    - 256 MB 每個使用者/應用程式的雲端儲存體。
- 適用於 Xbox Live 創作者計劃開發人員：
    - 64 MB 每個使用者/應用程式的雲端儲存體。
- 電源故障強固的回應，應用程式不一定要處理與儲存的部分資料。
- 資料自動上傳至雲端，即使應用程式未執行。
- 資料可跨 Xbox One 或 UWP 的裝置連接到 Xbox Live。
- Xbox Live 處理跨裝置同步處理和衝突管理而不需要應用程式的參與程度。

連接的儲存體系統可確保所有儲存都進行完整或不完全。 這表示，身為開發人員您將永遠不必擔心部分儲存的資料會影響您的遊戲狀態，電源故障或突然關閉應用程式的使用者以手動方式或藉由開啟另一個應用程式/遊戲主控台上。 這也可確保更順暢的遊戲播放您的標題的使用者體驗，做為連接儲存體可讓使用者在遊戲之間切換很重要的一部分，應用程式快速及隨意但仍再回到原始遊戲就保持狀態。 若要實作您自己的標題中的這些功能，您必須先了解連接的儲存體 api。

## <a name="connected-storage-structure"></a>連接的儲存體結構

連接的儲存體系統允許將資料儲存成一或多個 blob 容器中的應用程式。 概括而言，連接的儲存體系統中的所有資料都都會與使用者相關聯，或使用者或電腦在 Xbox 開發套件的情況下開發標題。 特定使用者的應用程式所儲存的所有資料，或機器會儲存在連接的儲存空間。 您的應用程式的每個使用者取得連接的儲存空間限制為 64 或 256 MB 總儲存體。 請務必請注意，此儲存體專屬於您單獨的應用程式，它將不會共用與其他應用程式。

### <a name="containers-and-blobs"></a>容器和 blob

連接的儲存體容器或簡稱，容器是儲存體的基本單位。 每個連接的儲存空間可以包含多個容器，如下圖所示。

連接的儲存空間 （每個標題/電腦或每個標題/使用者）

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 資料會儲存在容器中，為呼叫 blob 的一或多個緩衝區。 下圖說明在磁碟上的容器的內部系統表示法。 針對每個容器中，沒有容器檔案，其中包含每個 blob 容器中的資料檔案的參考。

容器的圖表

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

若要將資料儲存在容器中，呼叫適當的 Api 容器方法 SubmitUpdatesAsync，提供名稱和 blob （緩衝區物件） 的對應。 以不可分割方式套用 SubmitUpdatesAsync 呼叫中所述的所有變更，也就是所有 blob 會都更新要求，或整個作業會中止，且容器會保留在其之前呼叫的狀態。

個別儲存作業使用 SubmitUpdatesAsync 限於 16 MB 的資料一次。

## <a name="connected-storage-api"></a>已連接的儲存體 API

連接的存放裝置具有不同的 Api，XDK 及 UWP 應用程式。 幸運的是，這些 Api 類似於彼此非常密切。 主要命名空間和類別名稱中，不同的兩個 Api。 所需的函式[儲存](connected-storage-saving.md)，[載入](connected-storage-loading.md)，並[刪除](connected-storage-deleting.md)相同名為使用 API 的資料。

兩個連接儲存體 Api 的進一步差異所述的 [連接的儲存體] 區段[移植 Xbox Live 的程式碼 XDK UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)。

您可以找到 XDK 連接儲存體 Api 記載於路徑下的 XDK.chm 檔案：**Xbox 一個 XDK >> API 參考 >> 平台 API 參考 >> 系統 API 參考 >> Windows.Xbox.Storage**。
XDK Api 也會記載於[developer.microsoft.com 網站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)。
XDK Api 的連結，您需要已啟用 Xbox 開發人員 Kit(XDK) 存取 Microsoft 帳戶 （msa）。
Windows.Xbox.Storage 是 Xbox One 主控台連接的儲存體命名空間的名稱。

您可以找到 UWP 連接儲存體中記載的 Api 路徑之下的 Xbox Live SDK.chm 檔案：**Xbox Live Api >> Xbox Live 平台延伸模組 SDK API 參考 >> Windows.Gaming.XboxLive.Storage**。
UWP 連接儲存體 Api 也在線上記錄[Windows.Gaming.XboxLive.Storage 命名空間參考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)。
Windows.Gaming.XboxLive.Storage 是 UWP 應用程式連接的儲存體命名空間的名稱。

若要開始使用您要取得連接的儲存體的連接儲存體*空間*。 連接的儲存空間相關聯的使用者或電腦，並保留所有連接的儲存體相關聯的資料與該使用者或電腦的形式*容器*並*blob*。 取得電腦或使用者連接的儲存空間可讓您存取讀取和寫入至該儲存實體資料。 若要取得連接的儲存空間，XDK 及 UWP 的標題會呼叫`GetForUserAsync`方法，XDK 標題可能也會呼叫`GetForMachineAsync`方法，UWP 標題將會無法呼叫`GetForMachineAsync`。 `GetForUserAsync` 並`GetForMachineAsync`中所包含`ConnectedStorageSpace`XDK 中的類別。 `GetForUserAsync` 包含在`GameSaveProvider`UWP API 中的類別。 特別是如果使用者有一個裝置上儲存資料，並正在繼續遊戲，第一次在另一個裝置時可能長時間執行的作業，這些方法。 `GetForUserAsync` 擷取連接儲存體空間，您可以使用它來建立、 存取和刪除容器的使用者。

若要建立的容器，或存取先前建立的容器，呼叫`CreateContainer`函式的您`ConnectedStorageSpace`或是`GameSaveProvider`類別，這會讓您存取的命名容器的使用者或電腦相關聯`ConnectedStorageSpace`或`GameSaveProvider`用來建立容器。 `ConnectedStorageSpace`並`GameSaveProvider`類別也包括`DeleteContainerAsync`函式可讓您刪除容器會提供您提供要刪除之容器的名稱。

若要更新您的容器呼叫中的 blob`SubmitUpdatesAsync`中`ConnectedStorageContainer`類別的 XDK 或`GameSaveContainer`UWP API 的類別。 `SubmitUpdatesAsync` 可讓您提供的名稱和緩衝區清單做為資料来寫入至 blob，一份要刪除的 blob 的名稱和呼叫的顯示名稱儲存容器。 這是您最終要呼叫來更新資料，在您連接的儲存空間的函式。

若要查看連接的儲存體 api 使用讀取下列已連接的儲存體文件中的範例：[將資料儲存](connected-storage-saving.md)
[載入資料](connected-storage-loading.md)
[刪除資料](connected-storage-deleting.md)

> [!NOTE]
> 安全性注意事項：
>
> 在電腦上執行的通用 Windows 平台 (UWP) 應用程式會將本機資料儲存在可存取本機使用者，且原本就未受到保護防止使用者的位置。
>您應該考慮套用您自己的加密和有效性檢查遊戲的儲存資料以協助防止使用者修改外部遊戲遊戲的儲存。
>因此您應該決定是否要允許 PC 和 Xbox 遊戲來共用儲存，或區分兩者的版本。

## <a name="managing-local-storage"></a>管理本機儲存體

在測試連接的儲存體在 Xbox 開發套件或 UWP 應用程式時，您可能需要處理儲存在本機開發裝置上的資料。

工具 xbstorage 隨附 XDK，是一種命令列工具，可讓您操作開發主控台上的本機儲存體。
適用於 UWP 開發人員沒有呼叫 gamesaveutil.exe 隨附於 Windows 10 SDK Fall Creators Update(10.0.16299.91) 和更新版本的電腦的相同工具。

這兩種工具可讓您操作中使用下列命令在裝置上的本機儲存體：

|命令  |描述  |
|---------|---------|
|重設    | 執行原廠重設連接的儲存體上。 |
|匯入   | 從指定的 XML 檔案匯入資料，來連接的儲存空間。 |
|export   | 從連接的儲存空間的資料匯出至指定的 XML 檔案中。 |
|[刪除]   | 從連接的儲存空間刪除資料。 |
|產生 | 會產生空的資料，並將儲存至指定的 XML 檔案。 |
|模擬 | 模擬儲存體空間不足的情況。 |

若要深入了解適用於 xbstorage 工具和 gamesaveutils.exe 讀取函式[管理的本機連接的儲存體](connected-storage-xb-storage.md)。

## <a name="technical-overview"></a>技術概觀

若要進行更深入探討連接的儲存體上讀取 XDK 的運作方式[連接儲存體技術概觀](connected-storage-technical-overview.md)。 技術概觀特別針對 XDK 開發人員所撰寫，但內含的概念適用於連接的儲存體一般。