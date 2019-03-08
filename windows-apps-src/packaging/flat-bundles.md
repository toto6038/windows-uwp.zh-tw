---
title: 一般套件組合應用程式套件
description: 描述如何建立一般套件組合，使用應用程式套件的參照將應用程式的 .appx 套件檔案組合起來。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, 封裝, 套件設定, 一般套件組合
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638483"
---
# <a name="flat-bundle-app-packages"></a>一般套件組合應用程式套件 

> [!IMPORTANT]
> 如果您想要提交應用程式至 Microsoft Store，您需要連絡[Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得使用一般套件組合的核准。

一般的套件組合都更好的方式來封裝您的應用程式套件檔案。 應用程式套件組合的檔案會使用應用程式套件檔案要包含在套件組合的多層級封裝結構的一般 Windows，一般的套組移除這項需求只參考應用程式套件檔案，以進行外部應用程式套件組合。 應用程式套件檔案都不會再包含組合中，因為它們可以平行處理，這會導致減少上傳時間，更快發行 （因為在此同時，可以處理每個應用程式套件檔案），且最終更快速開發反覆項目。

![一般套件組合圖表](images/bundle-combined.png)

一般套件組合的另一個優點是建立較少的套件。 只會參考應用程式封裝的檔案，因為兩個版本的應用程式可以參考相同的封裝檔案，如果套件未在兩個版本之間變更。 為應用程式的下一個版本建置套件時，這可讓您只建立已變更的應用程式套件。
根據預設，一般的套件組合會參考與本身相同的資料夾內的應用程式套件檔案。 不過，這個參考可以變更為其他路徑（相對路徑、網路共用資料夾和 http 位置）。 若要這樣做，您必須在一般套件組合建立期間直接提供**BundleManifest**。 

## <a name="how-to-create-a-flat-bundle"></a>如何建立一般套件組合

可透過使用 MakeAppx.exe 工具，或透過使用封裝配置來定義套件組合結構，建立一般套件組合。

### <a name="using-makeappxexe"></a>使用 MakeAppx.exe
若要建立使用 MakeAppx.exe 一般套件組合，使用"MakeAppx.exe 配套 命令如往常一樣，但使用 /fb 參數產生一般的應用程式配套檔案 （這會是極小，因為它只會參考應用程式套件檔案，並不包含任何實際的承載). 

以下是命令語法的範例：

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

如需有關使用 MakeAppx.exe 的詳細資訊，請參閱[使用 MakeAppx.exe 工具建立應用程式套件](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

### <a name="using-packaging-layout"></a>使用封裝配置
或者，您可以使用封裝配置來建立一般套件組合。 若要這樣做，請將應用程式套件組合資訊清單的**PackageFamily**元素中的**FlatBundle**屬性設定為**true**。 若要深入了解封裝配置，請查看[使用封裝配置的套件建立](packaging-layout.md)。

## <a name="how-to-deploy-a-flat-bundle"></a>如何部署一般套件組合 
在部署一般套件組合之前，（除了應用程式套件組合）每個應用程式套件也需要使用相同的憑證來簽署。 這是因為所有的應用程式套件檔案 (.appx/.msix) 現在是獨立的檔案，而且不包含在應用程式套件組合 (.appxbundle/.msixbundle) 檔案不再。 一旦已簽署封裝，使用[Add-appxpackage cmdlet](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) PowerShell 指向應用程式套件組合的檔案和部署應用程式 （假設應用程式套件其中應用程式套件組合會預期要） 中。 