---
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: 企業共用存放裝置
description: 企業共用存放裝置定義本機資料位置，讓企業營運應用程式共用資料。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 006507d4665f5578310b8d3e31fb8f7fba4117a2
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8798303"
---
# <a name="enterprise-shared-storage"></a>企業共用存放裝置

企業共用存放裝置包含兩個位置，其中，具有受限制功能 **enterpriseDeviceLockdown** 和一個企業憑證的應用程式具有完整讀取與寫入權。 請注意，**enterpriseDeviceLockdown** 功能讓應用程式能夠使用裝置鎖定 API，以及存取企業共用存放裝置資料夾。 如需 API 的詳細資訊，請參閱 [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331) 命名空間。  

這些位置設定於本機磁碟機︰
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>案例

企業共用存放裝置支援下列案例。

- 您可以在應用程式執行個體內、相同應用程式的執行個體之間或甚至在假設它們具有適當功能和憑證的應用程式之間共用資料。
- 您可以將資料儲存至本機硬碟的 \Data\SharedData\Enterprise\Persistent 資料夾，而且即使重設裝置之後仍會保存資料。
- 操作檔案，包括透過行動裝置管理 (MDM) 服務讀取、寫入和刪除裝置上的檔案。 如需如何透過 MDM 服務使用企業共用存放裝置的詳細資訊，請參閱 [EnterpriseExtFileSystem CSP](http://go.microsoft.com/fwlink/?LinkId=699333)。

## <a name="access-enterprise-shared-storage"></a>存取企業共用存放裝置

下列範例示範如何宣告在套件資訊清單中存取企業共用存放裝置的功能，以及如何使用 Windows.Storage.StorageFolder 類別來存取共用存放裝置資料夾。

您必須在應用程式套件資訊清單中包括下列功能：

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

若要存取共用資料位置，您的應用程式會使用下列程式碼。

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```

