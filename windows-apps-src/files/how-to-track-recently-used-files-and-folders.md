---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: 追蹤最近使用的檔案和資料夾
description: 將使用者經常存取的檔案新增到您 app 的最近使用清單 (MRU) 中，以追蹤這些檔案。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29d140672e80304bb0adb7081ba616e75d8d8ecd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173202"
---
# <a name="track-recently-used-files-and-folders"></a>追蹤最近使用的檔案和資料夾

**重要 API**

- [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) \(英文\)
- [**FileOpenPicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) \(英文\)

將使用者經常存取的檔案新增到您 app 的最近使用清單 (MRU) 中，以追蹤這些檔案。 平台會根據項目上次存取的時間來排序項目，並在達到清單的 25 個項目數限制時移除最舊的項目，為您管理 MRU。 所有 app 都有自己的 MRU。

從靜態 [**StorageApplicationPermissions.MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) 屬性取得的 [**StorageItemMostRecentlyUsedList**](/uwp/api/Windows.Storage.AccessCache.StorageItemMostRecentlyUsedList) 類別，代表您的 app 的 MRU。 MRU 項目會儲存為 [**IStorageItem**](/uwp/api/Windows.Storage.IStorageItem) 物件，所以 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 物件 (代表檔案) 和 [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) 物件 (代表資料夾) 都可以新增到 MRU。

> [!NOTE]
> 如需完整範例，請參閱[檔案選擇器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker) \(英文\) 和[檔案存取範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) \(英文\)。

## <a name="prerequisites"></a>必要條件

-   **了解通用 Windows 平台 (UWP) 應用程式的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)。

-   **位置的存取權限**

    請參閱[檔案存取權限](file-access-permissions.md)。

-   [使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md)

    挑選的檔案經常是使用者會一再反覆使用的相同檔案。

 ## <a name="add-a-picked-file-to-the-mru"></a>將挑選的檔案新增到 MRU

-   使用者挑選的檔案經常是他們會一再反覆使用的檔案。 所以，請考慮儘快將已挑選的檔案新增到您的 app MRU。 方法如下。

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) \(英文\) 是多載。 在這個範例中，我們使用 [**Add(IStorageItem, String)** ](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add)，以便將中繼資料與檔案建立關聯。 設定中繼資料可讓您記錄項目的用途，例如「個人檔案圖片」。 您也可以藉由呼叫 [**Add(IStorageItem)** ](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add)，在沒有中繼資料的情況下，將檔案新增到 MRU 中。 當您將項目新增到 MRU 時，該方法會傳回唯一的識別字串 (稱為權杖)，可用來擷取該項目。

> [!TIP]
> 您需要此權杖才能從 MRU 擷取項目，因此請將它保存在別處。 如需 app 資料的詳細資訊，請參閱[管理應用程式資料](/previous-versions/windows/apps/hh465109(v=win.10))。

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>使用權杖從 MRU 擷取項目

使用最適合您要擷取之項目的擷取方法。

-   使用 [**GetFileAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfileasync) 將檔案擷取為 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)。
-   使用 [**GetFolderAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfolderasync) 將資料夾擷取為 [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder)。
-   使用 [**GetItemAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getitemasync) 擷取可代表檔案或資料夾的泛型 [**IStorageItem**](/uwp/api/Windows.Storage.IStorageItem)。

以下說明如何取回我們剛才新增的檔案。

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

以下說明如何逐一查看所有項目以取得權杖及項目。

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

[  **AccessListEntryView**](/uwp/api/Windows.Storage.AccessCache.AccessListEntryView) 可以讓您重複處理 MRU 中的項目。 這些項目 (entry) 是 [**AccessListEntry**](/uwp/api/Windows.Storage.AccessCache.AccessListEntry) 結構，其中包含某個項目 (item) 的權杖和中繼資料。

## <a name="removing-items-from-the-mru-when-its-full"></a>當 MRU 塞滿時從 MRU 移除項目

在達到 MRU 的 25 個項目數限制，而您嘗試新增項目時，自動會移除最久以前存取的項目。 因此，您永遠不需要在新增項目之前移除項目。

## <a name="future-access-list"></a>未來存取清單

如同 MRU 一樣，您的 app 也有一個未來存取清單。 您的使用者會挑選檔案和資料夾，以授權您的 app 存取原本可能無法存取的項目。 如果您將這些項目新增到未來存取清單，則可以保留該權限，讓您的 app 稍後再次存取這些項目。 從靜態 [**StorageApplicationPermissions.FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 屬性取得的 [**StorageItemAccessList**](/uwp/api/Windows.Storage.AccessCache.StorageItemAccessList) 類別，代表您 app 的未來存取清單。

當使用者挑選項目時，請考慮將此項目新增到您的未來存取清單及 MRU。

-   [  **FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 最多可以保留 1000 個項目。 請記住：它可以保留資料夾和檔案，所以會有許多資料夾。
-   平台永遠不會替您從 [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 移除項目。 當達到 1000 個項目的限制時，除非您使用 [**Remove**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.remove) 方法挪出空間，否則無法再新增其他項目。