---
title: 部署使用 Project 留尼旺島的應用程式
description: 本文提供的指示可讓您部署使用 Project 留尼旺島的應用程式。
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32、windows 應用程式開發、專案留尼旺島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 967b38be2ee1485c28175b86c016e2bca1a8ce5b
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730779"
---
# <a name="deploy-apps-that-use-project-reunion"></a>部署使用 Project 留尼旺島的應用程式

若要將使用 Project 留尼旺島0.5 的應用程式部署到其他電腦，您必須使用 [MSIX](/windows/msix)來封裝應用程式。 Project 留尼旺島可支援在未來版本中部署未封裝的應用程式。 如需我們未來計畫的詳細資訊，請參閱我們的 [藍圖](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md)。

依預設，當您使用 Visual Studio 的 Project 留尼旺島延伸模組所提供的其中一個 [WinUI 專案範本](..\winui\winui3\winui-project-templates-in-visual-studio.md) 來建立專案時，您的專案會包含設定為將應用程式建立至 MSIX 封裝的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 。 如需設定此專案以為應用程式建立 MSIX 套件的詳細資訊，請參閱 [Visual Studio 中的封裝桌面或 UWP 應用程式](/windows/msix/package/packaging-uwp-apps)。

為您的應用程式建立 MSIX 套件之後，您有數個選項可將其部署到其他電腦。 如需詳細資訊，請參閱 [管理您的 MSIX 部署](/windows/msix/desktop/managing-your-msix-deployment-overview)。

> [!NOTE]
> 在生產環境中， (c #/.NET 5 或 c + +/Win32) 的 MSIX 封裝桌面應用程式中，可使用 Project 留尼旺島0.5。 使用 Project 留尼旺島0.5 的已封裝桌面應用程式可以發行至 Microsoft Store。

## <a name="dependencies-on-the-project-reunion-framework-package"></a>Project 留尼旺島 framework 套件的相依性

當您建立使用 Project 留尼旺島的應用程式時，您的應用程式會參考一組透過 *架構套件* 散發給終端使用者的 project 留尼旺島 runtime 元件。 架構封裝可讓封裝的應用程式透過使用者裝置上的單一共用來源來存取 Project 留尼旺島元件，而不是將它們組合到應用程式套件中。 架構套件也會攜帶自己的資源，例如 (COM 和 Windows 執行階段註冊) 的 Dll 和 API 定義。 這些資源會在您的應用程式內容中執行，因此它們會繼承您應用程式的功能和許可權，而且不會判斷提示本身的任何功能或許可權。

Project 留尼旺島 framework 套件是透過 Microsoft Store 部署給終端使用者的 MSIX 套件。 除了安全性和可靠性修正之外，它可以輕鬆且快速地更新為最新版本。 在電腦上使用 Project 留尼旺島的所有應用程式相依于架構套件的共用實例，如下圖所示。

![應用程式如何存取 Project 留尼旺島 framework 套件的圖表](images/framework.png)

設定相依性的方式，取決於您是在應用程式中使用專案留尼旺島的發行前版本或發行版本。 Project 留尼旺島將于發行前版本和發行版本中提供，這些版本將包含架構套件的發行前版本和發行版本。 應用程式必須確定它們參考的是正確的套件，以取得所需的功能。

## <a name="configure-dependencies-on-preview-versions-of-the-framework-package"></a>設定 framework 套件預覽版本的相依性

Project 留尼旺島的預覽版本適用于調查和新功能的意見反應。 這些授權不會在生產環境中使用，而且不應該發行至 Microsoft Store。

當您在開發電腦上安裝適用于 Visual Studio 或 Project 留尼旺島 NuGet 套件之專案留尼旺島延伸模組的預覽版本時，會在組建階段將架構套件的預覽版本部署為 NuGet 套件相依性。

## <a name="configure-dependencies-on-release-versions-of-the-framework-package"></a>設定架構套件發行版本的相依性

支援在生產環境中使用 Project 留尼旺島的發行版本。

當您在開發電腦上安裝 Project 留尼旺島延伸模組或 Project 留尼旺島 NuGet 套件的發行版本，並使用其中一個提供的 WinUI 3 專案範本建立專案時，產生的套件資訊清單會包含指定架構套件相依性的 [PackageDependency](/uwp/schemas/appxpackage/uapmanifestschema/element-packagedependency) 元素。

```xml
<Dependencies>
    <PackageDependency Name="Microsoft.ProjectReunion.0.5" MinVersion="0.52103.9000.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
</Dependencies>
```

但是，如果您手動建立應用程式套件，您必須自行將這個 **PackageDependency** 專案新增至您的套件資訊清單，以宣告 Project 留尼旺島 framework 套件的相依性。

## <a name="updates-and-versioning-of-the-framework-package"></a>架構套件的更新和版本控制

發行新版本的 Project 留尼旺島 framework 套件時，所有應用程式都會更新至新版本，而不需要重新發佈複本。 Windows 會在發行時更新為最新版本的架構，而應用程式會在重新開機期間自動參考最新的 framework 套件版本。 舊的 framework 套件版本將不會從系統中移除，直到系統上的應用程式不再執行或主動使用為止。

![應用程式如何取得 Project 留尼旺島 framework 套件更新的圖表](images/framework-update.png)

由於應用程式相容性對 Microsoft 以及相依于專案留尼旺島的應用程式很重要，因此 Project 留尼旺島 framework 套件會遵循 [語義版本控制 2.0.0](https://semver.org/) 規則。 這表示在發行專案的1.0 版時，Project 留尼旺島 framework 套件將保證次要和修補程式版本變更之間的相容性，而且只會在主要版本更新之間進行中斷性變更。

## <a name="related-topics"></a>相關主題

- [使用 Project 留尼旺島建立桌面 Windows 應用程式](index.md)
- [開始使用 Project 留尼旺島](get-started-with-project-reunion.md)
