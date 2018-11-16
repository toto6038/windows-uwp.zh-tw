---
author: laurenhughes
title: 使用應用程式安裝程式安裝 UWP app
description: 本節包含或連結至關於應用程式安裝程式，以及有關如何使用應用程式安裝程式功能的文章。
ms.author: lahugh
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, 應用程式安裝程式, AppInstaller, 側載, 相關集合, 選用套件
ms.localizationpriority: medium
ms.openlocfilehash: f3da1f4f393eea1362b6e59d2ad7e9a2a97bc33b
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6968352"
---
# <a name="install-uwp-apps-with-app-installer"></a>使用應用程式安裝程式安裝 UWP app

## <a name="purpose"></a>用途
本節包含或連結至關於應用程式安裝程式，以及有關如何使用應用程式安裝程式功能的文章。 

應用程式安裝程式可讓您以按兩下應用程式套件的方式來安裝 UWP app。 這表示使用者不需要使用 PowerShell 或其他開發人員工具來部署 UWP app。 應用程式安裝程式也可以從網頁、選用套件及相關集合中安裝應用程式。 若要了解如何使用應用程式安裝程式來安裝應用程式，請參閱表中的主題。

| 主題 | 描述 |
|-------|-------------|
| [使用 Visual studio 建立應用程式安裝程式檔案](create-appinstallerfile-vs.md)| 了解如何使用 Visual Studio，以便啟用透過 .appinstaller 檔案的自動更新。 |
| [從網頁安裝 UWP app](installing-UWP-apps-web.md) | 在本節中，我們將檢閱可讓使用者直接從網頁安裝應用程式所需採取的步驟。 |
| [使用應用程式安裝程式檔案安裝相關集合](install-related-set.md) | 在本節中，了解如何透過應用程式安裝程式安裝相關集合。 我們還會逐步完成建構定義相關集合之應用程式安裝程式檔案的步驟。 |
| [疑難排解應用程式安裝程式檔案的安裝問題](troubleshoot-appinstaller-issues.md) | 使用應用程式安裝程式檔案側載應用程式時的常見問題和解決方案。 |
| [應用程式安裝程式檔案 (.appinstaller) 參考資料](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file) | 檢視應用程式安裝程式檔案的完整 XML 結構描述。 |

## <a name="tutorials"></a>教學課程 

請遵循這些教學課程，了解如何提供主機服務，並從各種發佈平台安裝 UWP app。 這些教學課程對於不想要或不需要發佈應用程式至 Microsoft Store ，但仍然想要利用 Windows 10 封裝與部署平台的企業與開發人員而言，是很實用的。

| 教學課程 | 說明 |
|----------|-------------|
| [從 Azure Web 應用程式安裝 UWP 應用程式](web-install-azure.md) | 建立 Azure Web 應用程式，並使用它來提供主機服務，並散發您的 UWP 應用程式套件。 |
| [從 IIS 伺服器上安裝 UWP app](web-install-IIS.md) | 設定 IIS 伺服器，確認您的 Web 應用程式可以裝載應用程式套件，並有效使用應用程式安裝程式。 |
| [在 AWS 上裝載 UWP 應用程式套件以進行 Web 安裝](web-install-aws.md) | 了解如何設定 Amazon Simple Storage Service，從網站裝載您的 UWP 應用程式套件。 |

