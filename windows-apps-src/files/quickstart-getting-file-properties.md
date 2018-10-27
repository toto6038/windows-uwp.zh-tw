---
author: laurenhughes
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: 取得檔案屬性
description: 取得由 StorageFile 物件所表示檔案的屬性 &amp;\#8212;最上層、基本及延伸&amp;\#8212;。
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8fc44300376efb5b56f390457e516f35a3ec4202
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695919"
---
# <a name="get-file-properties"></a>取得檔案屬性



**重要 API**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

取得由 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件所表示檔案的屬性 (最上層、基本及延伸)。

> [!NOTE]
> 另請參閱[檔案存取範例](http://go.microsoft.com/fwlink/p/?linkid=619995)。

 


## <a name="prerequisites"></a>先決條件

-   **了解通用 Windows 平台 (UWP) App 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **位置的存取權限**

    例如，這些範例中的程式碼需要 **picturesLibrary** 功能，但是您的位置可能需要其他功能或完全不需要功能。 若要深入了解，請參閱[檔案存取權限](file-access-permissions.md)。

## <a name="getting-a-files-top-level-properties"></a>取得檔案的最上層屬性

許多最上層檔案屬性都可當成 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 類別的成員來存取。 這些屬性包含檔案屬性、內容類型、建立日期、顯示名稱及檔案類型等。

**注意：** 請記得宣告**picturesLibrary**功能。

 

此範例會列舉圖片庫中的所有檔案，存取每個檔案的一些最上層屬性。

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>取得檔案的基本屬性

許多基本檔案屬性都可藉由先呼叫 [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737) 方法來取得。 這個方法會傳回 [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) 物件，此物件會定義項目 (檔案或資料夾) 大小的屬性，以及上次修改項目的時間。

此範例會列舉圖片庫中的所有檔案，存取每個檔案的一些基本屬性。

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>取得檔案的延伸屬性

除了最上層和基本檔案屬性之外，還提供許多與檔案內容相關聯的屬性。 這些延伸屬性可藉由呼叫 [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) 方法來存取 ([**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) 物件可藉由呼叫 [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) 屬性來取得)。當最上層和基本檔案屬性可以分別當成類別屬性 ([**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 和 **BasicProperties**) 來存取時，您可以將 [String](http://go.microsoft.com/fwlink/p/?LinkID=325032) 物件 (代表要擷取之屬性的名稱) 的 [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) 集合傳送到 **BasicProperties.RetrievePropertiesAsync** 方法，來取得延伸屬性。 這個方法接著會傳回 [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) 集合。 然後，系統會從集合中依名稱或索引擷取每個延伸屬性。

此範例會列舉圖片媒體櫃中的所有檔案、指定 [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) 物件中所需屬性 (**DataAccessed** 和 **FileOwner**) 的名稱、將該 [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) 物件傳送到 [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) 以擷取這些屬性，然後從傳回的 [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) 物件中依名稱擷取這些屬性。

請查看 [Windows 核心屬性](https://msdn.microsoft.com/library/windows/desktop/mt805470) 以取得檔案延伸屬性的完整清單。

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
