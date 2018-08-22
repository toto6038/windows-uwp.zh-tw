---
author: jnHs
Description: Follow these guidelines to prepare your app's packages for submission to the Microsoft Store.
title: 應用程式套件需求
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 05/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 套件需求, 套件, 套件格式, 支援的版本, 提交
ms.localizationpriority: medium
ms.openlocfilehash: d7d748f36dafd93066928f01f9aa42414f2ffc1f
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2791373"
---
# <a name="app-package-requirements"></a>應用程式套件需求

遵循這些指導方針來準備要提交到 Microsoft Store 的應用程式套件。

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>在建置 Microsoft Store 的應用程式套件之前

請確定[使用 Windows 應用程式認證套件測試您的 app](../debug-test-perf/windows-app-certification-kit.md)。 我們也建議您在不同類型的硬體上測試您的 app。 請注意，除非您的 app 經過我們的認證，並在 Microsoft Store 發行，否則只能在有開發人員授權的電腦上安裝和執行 app。

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>使用 Microsoft Visual Studio 建置應用程式套件

如果您使用 Microsoft Visual Studio 做為開發環境，則您已經擁有內建的工具可以快速並輕易地建立應用程式套件。 如需詳細資訊，請參閱[封裝 app](../packaging/index.md)。

> [!NOTE]
> 務必讓您的所有檔案名稱都使用 ANSI。 

在 Visual Studio 中建立套件時，請確定您已經使用與開發人員帳戶相關聯的相同帳戶登入。 套件資訊清單的某些部分具有與您帳戶相關的特定詳細資料。 系統會自動偵測並新增此資訊。 若未新增其他資訊至資訊清單，您可能會遇到套件上傳失敗。 

當您建置 app 套件時，Visual Studio 可以建立 .appx 檔案或 .appxupload 檔案 (或適用於 Windows Phone 8.1 及較舊版本的 .xap 檔案)。 對於目標為 Windows 10 的 app，請一律在 [套件](upload-app-packages.md) 頁面上傳 .appxupload 檔案。 如需針對 Microsoft Store 封裝 UWP 應用程式的詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](../packaging/packaging-uwp-apps.md)。

您的 app 套件不一定要以根目錄在受信任之憑證授權單位的憑證簽署。


### <a name="app-bundles"></a>App 套件組合

Windows 10、 Windows 8.1 和/或 Windows Phone 8.1 為目標的應用程式、 Visual Studio 可以產生應用程式組合 (.appxbundle) 以減少使用者下載的應用程式的大小。 如果您已經定義了語言特定的資產、各種大小影像的資產，或是套用到特定 Microsoft DirectX 版本的資源，這通常很有幫助。

> [!NOTE]
> 一個應用程式套件組合可以包含所有架構的套件。 只能針對每個目標 OS 提交一個套件組合。

有了 app 套件組合，使用者只需要下載相關的檔案，不需要下載所有可能的資源。 如需應用程式套件組合的詳細資訊，請參閱[封裝應用程式](../packaging/index.md)和[使用 Visual Studio 封裝 UWP app](../packaging/packaging-uwp-apps.md)。


## <a name="building-the-app-package-manually"></a>手動建置 app 套件

如果您沒有使用 Visual Studio 建立套件，必須[手動建立套件資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually)。

如需完整的資訊清單詳細資料和需求，請務必檢閱 [App 套件資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)文件。 您的資訊清單必須遵循套件資訊清單結構描述才能通過認證。

您的資訊清單必須包含一些關於帳戶與 app 的特定資訊。 您可以在儀表板中，查看 app 概觀頁面 **應用程式管理** 區段中的[檢視應用程式身分識別詳細資料](view-app-identity-details.md)，來找到此資訊。

> [!NOTE]
> 資訊清單中的值區分大小寫。 空格與其他標點符號也必須相符。 請仔細輸入相關值，並檢查以確保正確無誤。


應用程式可 (.appxbundle) 使用不同的資訊清單。 如需應用程式套件組合資訊清單的詳細資料和需求，請檢閱[套件組合資訊清單](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest)文件。 附註中.appxbundle，每個.appxmanifest 包含套件必須使用相同的元素和屬性，但不包括**ProcessorArchitecture**元素之屬性的[身分識別](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)。

> [!TIP]
> 在您提交套件之前，請務必執行 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)。 這有助於協助您判斷資訊清單是否會造成認證或提交失敗的任何問題。


## <a name="package-format-requirements"></a>套件格式需求

您的應用程式套件必須符合下列需求。

| 應用程式套件屬性 | 需求                                                          |
|----------------------|----------------------------------------------------------------------|
| 套件大小         | .appxbundle：每個套件組合上限為 25 GB <br>以 Windows 10 為目標的 .appx 套件：每個套件上限為 25 GB<br>以 Windows 8.1 為目標的 .appx 套件：每個套件上限為 8 GB <br> 以 Windows 8 為目標的 .appx 套件：每個套件上限為 2 GB <br> 以 Windows Phone 8.1 為目標的 .appx 套件：每個套件上限為 4 GB <br> .xap 套件：每個套件上限為 1 GB                                                                           |
| 區塊對應雜湊     | SHA2-256 演算法                                                   |


## <a name="supported-versions"></a>支援版本

UWP 應用程式的所有套件都必須以 Microsoft Store 所支援的 Windows 10 版本為目標。 您的套件支援的版本必須表示在應用程式資訊清單的[TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)元素的**MinVersion**和**MaxVersionTested**屬性中。

目前支援的版本範圍為： 
- 最小值：10.0.10240.0
- 最大值：10.0.17134.0


## <a name="storemanifest-xml-file"></a>StoreManifest XML 檔案

StoreManifest.xml 是選用的組態檔，可能包含在 app 套件中。 它的用途是啟用封裝資訊清單沒有涵蓋的功能，例如將您的 app 宣告為 Microsoft Store 裝置應用程式，或是宣告套件仰賴的需求適用於某裝置。 如果使用 StoreManifest.xml 送出與應用程式套件和必須在您的應用程式的主要專案的根資料夾。 如需詳細資訊，請參閱 [StoreManifest 結構描述](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal)。

 

 




