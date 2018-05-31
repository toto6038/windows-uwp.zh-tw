---
author: laurenhughes
title: 一般套件組合應用程式套件
description: 描述如何建立一般套件組合，使用應用程式套件的參照將應用程式的 .appx 套件檔案組合起來。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 封裝, 套件設定, 一般套件組合
ms.localizationpriority: medium
ms.openlocfilehash: 757f95a5f46bad6dbe650b4b552f3de486d84e1b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818279"
---
# <a name="flat-bundle-app-packages"></a>一般套件組合應用程式套件 

> [!IMPORTANT]
> 如果您想要提交應用程式至 Microsoft Store，您需要連絡[Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得使用一般套件組合的核准。

一般套件組合是將應用程式的 .appx 套件檔案組合起來的改進方式。 一般 .appxbundle 檔案使用多層次封裝結構，其中 .appx 套件檔案需要包含在套件組合中，一般套件組合透過只參考 .appx 套件檔案移除此需要，因此可在應用程式套件組合以外。 因為 .appx 套件檔案不再包含在套件組合中，所以可以平行處理，進而減少上傳時間、更快速地發佈（因為可以同時處理每個 .appx 套件檔案），以及最終更快速的開發反覆運算。

![一般套件組合圖表](images/bundle-combined.png)

一般套件組合的另一個優點是建立較少的套件。 因為只參考 .appx 套件檔案，如果套件在兩個版本之間未變更，兩個版本的應用程式可以參考相同套件檔案。 為應用程式的下一個版本建置套件時，這可讓您只建立已變更的應用程式套件。
根據預設，一般套件組合會參考和本身相同的資料夾中的 .appx 套件檔案。 不過，這個參考可以變更為其他路徑（相對路徑、網路共用資料夾和 http 位置）。 若要這樣做，您必須在一般套件組合建立期間直接提供**BundleManifest**。 

## <a name="how-to-create-a-flat-bundle"></a>如何建立一般套件組合

可透過使用 MakeAppx.exe 工具，或透過使用封裝配置來定義套件組合結構，建立一般套件組合。

### <a name="using-makeappxexe"></a>使用 MakeAppx.exe
若要使用 MakeAppx.exe 建立一般套件組合，請像往常一樣使用 “MakeAppx.exe bundle” 命令，但搭配 /fb 切換參數以產生一般 .appxbundle 檔案（因為它僅參考 .appx 套件，不含任何實際承載，所以非常小）。 

以下是命令語法的範例：

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

如需有關使用 MakeAppx.exe 的詳細資訊，請參閱[使用 MakeAppx.exe 工具建立應用程式套件](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

### <a name="using-packaging-layout"></a>使用封裝配置
或者，您可以使用封裝配置來建立一般套件組合。 若要這樣做，請將應用程式套件組合資訊清單的**PackageFamily**元素中的**FlatBundle**屬性設定為**true**。 若要深入了解封裝配置，請查看[使用封裝配置的套件建立](packaging-layout.md)。

## <a name="how-to-deploy-a-flat-bundle"></a>如何部署一般套件組合 
在部署一般套件組合之前，（除了應用程式套件組合）每個應用程式套件也需要使用相同的憑證來簽署。 這是因為所有應用程式套件檔案 (.appx) 現在都是獨立的檔案，不再包含在應用程式套件組合 (.appxbundle) 檔案中。 簽署套件之後，使用 PowerShell 中的[Add-AppxPackage cmdlet](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 指向 .appxbundle 檔案和部署應用程式（假設應用程式套件是在應用程式套件組合所預期的位置）。 