---
Description: 遵循這些指導方針來準備要提交到 Microsoft Store 的應用程式套件。
title: 應用程式套件需求
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 套件需求, 套件, 套件格式, 支援的版本, 提交
ms.localizationpriority: medium
ms.openlocfilehash: 851aaa28a7c42d395a16ee78a49a7e8bc5712f62
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158102"
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

當您建立應用程式的 UWP 套件時，Visual Studio 可以建立 msix 或 appx 檔案，或 msixupload 或 .appxupload 檔案。 針對 UWP 應用程式，我們建議您一律將 msixupload 或 .appxupload 檔案上傳至 [ [套件](upload-app-packages.md) ] 頁面。 如需針對 Microsoft Store 封裝 UWP 應用程式的詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](/windows/msix/package/packaging-uwp-apps)。

您的 app 套件不一定要以根目錄在受信任之憑證授權單位的憑證簽署。


### <a name="app-bundles"></a>App 套件組合

針對 UWP 應用程式，Visual Studio 可以產生應用程式套件組合 (. msixbundle 或 .appxbundle) 以縮減使用者下載的應用程式大小。 如果您已經定義了語言特定的資產、各種大小影像的資產，或是套用到特定 Microsoft DirectX 版本的資源，這通常很有幫助。

> [!NOTE]
> 一個應用程式套件組合可以包含所有架構的套件。

有了 app 套件組合，使用者只需要下載相關的檔案，不需要下載所有可能的資源。 如需應用程式套件組合的詳細資訊，請參閱[封裝應用程式](../packaging/index.md)和[使用 Visual Studio 封裝 UWP app](/windows/msix/package/packaging-uwp-apps)。


## <a name="building-the-app-package-manually"></a>手動建置 app 套件

如果您沒有使用 Visual Studio 建立套件，必須[手動建立套件資訊清單](/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually)。

如需完整的資訊清單詳細資料和需求，請務必檢閱 [App 套件資訊清單](/uwp/schemas/appxpackage/appx-package-manifest)文件。 您的資訊清單必須遵循套件資訊清單結構描述才能通過認證。

您的資訊清單必須包含一些關於帳戶與 app 的特定資訊。 您可以在儀表板中，查看 app 概觀頁面 **應用程式管理** 區段中的[檢視應用程式身分識別詳細資料](view-app-identity-details.md)，來找到此資訊。

> [!NOTE]
> 資訊清單中的值區分大小寫。 空格與其他標點符號也必須相符。 請仔細輸入相關值，並檢查以確保正確無誤。


應用程式套件組合 ( msixbundle 或 .appxbundle) 使用不同的資訊清單。 如需應用程式套件組合資訊清單的詳細資料和需求，請檢閱[套件組合資訊清單](/uwp/schemas/bundlemanifestschema/bundle-manifest)文件。 請注意，在 msixbundle 或 .appxbundle 中，每個包含套件的資訊清單都必須使用相同的專案和屬性，但[Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素的**ProcessorArchitecture**屬性除外。

> [!TIP]
> 提交封裝之前，請務必先執行 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md) 。 這有助於協助您判斷資訊清單是否會造成認證或提交失敗的任何問題。


## <a name="package-format-requirements"></a>套件格式需求

您的應用程式套件必須符合下列需求。

| 應用程式套件屬性 | 需求                                                          |
|----------------------|----------------------------------------------------------------------|
| 封裝大小         | msixbundle 或 .appxbundle：每個套件組合最多 25 GB <br>msix 或 .appx 封裝的目標為每個套件的 Windows 10:25 GB 上限<br>以 Windows 8.1 為目標的 .appx 套件：每個套件上限為 8 GB <br> 以 Windows 8 為目標的 .appx 套件：每個套件上限為 2 GB <br> 以 Windows Phone 8.1 為目標的 .appx 套件：每個套件上限為 4 GB <br> .xap 套件：每個套件上限為 1 GB                                                                           |
| 區塊對應雜湊     | SHA2-256 演算法                                                   |

> [!IMPORTANT]
> 您無法再上傳使用 Windows Phone 8.x SDK (s) 建立的新 XAP 套件。 已存在於 XAP 套件中的應用程式將會繼續在 Windows 10 行動裝置版裝置上運作。 如需詳細資訊，請參閱這 [篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

## <a name="supported-versions"></a>支援的版本

UWP 應用程式的所有套件都必須以 Microsoft Store 所支援的 Windows 10 版本為目標。 您的套件支援的版本必須表示在應用程式資訊清單的[TargetDeviceFamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)元素的**MinVersion**和**MaxVersionTested**屬性中。

目前支援的版本範圍為： 
- 最小值：10.0.10240.0
- 最大值：10.0.17763。1


## <a name="storemanifest-xml-file"></a>StoreManifest XML 檔案

StoreManifest.xml 是選用的組態檔，可能包含在 app 套件中。 它的用途是啟用封裝資訊清單沒有涵蓋的功能，例如將您的 app 宣告為 Microsoft Store 裝置應用程式，或是宣告套件仰賴的需求適用於某裝置。 如果使用，則會使用應用程式套件提交 StoreManifest.xml，而且必須在應用程式主要專案的根資料夾中。 如需詳細資訊，請參閱 [StoreManifest 結構描述](/uwp/schemas/storemanifest/store-manifest-schema-portal)。

 

 