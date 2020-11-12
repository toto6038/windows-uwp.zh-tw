---
title: 建立封裝資訊清單
description: 如果您想要將軟體封裝提交至 Windows 封裝管理員存放庫，請從建立封裝資訊清單開始。
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c0c01d87dc7e02b356a4fba20b01519e3361b28a
ms.sourcegitcommit: 36ae65013da22930c8e838fc65a63564a0bafee2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2020
ms.locfileid: "94377794"
---
# <a name="create-your-package-manifest"></a>建立封裝資訊清單

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

如果您想要將軟體封裝提交至 [Windows 封裝管理員存放庫](repository.md)，請從建立封裝資訊清單開始。 資訊清單是 YAML 檔案，用於描述要安裝的應用程式。

本文說明 Windows 封裝管理員的封裝資訊清單內容。

## <a name="yaml-basics"></a>YAML 基本概念

YAML 是專為封裝資訊清單所選擇的格式，因為相對於其他 Microsoft 開發工具，此格式較容易閱讀且一致性高。 如果您不熟悉 YAML 語法，您可以在 [Y 分鐘速成 YAML (Learn YAML in Y Minutes)](https://learnxinyminutes.com/docs/yaml/) 中了解基本概念。

> [!NOTE]
> Windows 封裝管理員的資訊清單目前未支援所有 YAML 功能。 不支援的 YAML 功能包括錨點、複雜索引鍵和集合。

## <a name="conventions"></a>慣例

本文會使用以下慣例：

* `:` 的左邊是用於資訊清單定義的常值關鍵字。
* `:` 的右邊是資料類型。 資料類型可以是基本類型 (例如 **字串** ) 或本文中其他位置所定義的豐富結構參考。
* `[` *datatype* `]` 標記法表示所述資料類型的陣列。 例如，`[ string ]` 是字串陣列。
* `{` *datatype* `:` *datatype* `}` 標記法表示一種資料類型對應到另一種資料類型。 例如，`{ string: string }` 是字串與字串的對應。

## <a name="manifest-contents"></a>資訊清單內容

封裝資訊清單必須包含一組必要項目，也可以進一步包含選擇性項目，以協助改善客戶安裝軟體的體驗。 本節將提供必要資訊清單結構描述和完整資訊清單結構描述的簡短摘要，以及每個結構描述的範例。

資訊清單檔案中的每個欄位都必須依照 Pascal 命名法的大小寫慣例，而且不能重複。

如需資訊清單中項目的完整清單和描述，請參閱 [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli) 存放庫中的[資訊清單規格](https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md)。

### <a name="minimal-required-schema"></a>最基本的必要結構描述

#### <a name="minimal-required-schema"></a>[最基本的必要結構描述](#tab/minschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
Version: string # Version numbering format.
License: string # The open source license or copyright.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Installers:
  - Arch: string # Enumeration of supported architectures.
    Url: string # Path to download installation file.
    Sha256: string # SHA256 calculated from installer.
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[範例](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>完整結構描述

#### <a name="complete-schema"></a>[完整結構描述](#tab/compschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
AppMoniker: string # The common name someone may use to search for the package.
Version: string # Version numbering format for package version.
Channel: string # A string representing the flight ring.
License: string # The open source license or copyright.
LicenseUrl: string # Valid secure URL to license.
MinOSVersion: string # Version numbering format for minimum version of Windows supported.
Description: string # Description of the package.
Homepage: string # Valid secure URL for the package.
Tags: list # Additional strings a user would use to search for the package.
FileExtensions: list # List of file extensions the package could support.
Protocols: list # List of protocols the package provides a handler for.
Commands: list # List of commands or aliases the user would use to run the package.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Switches: # These can be used to change the install behavior if supported by the InstallerType.
  Custom: string # Custom switches passed to the installer.
  Silent: string # Switches passed to the installer for silent installation.
  SilentWithProgress: string # Switches passed to the installer for non-interactive install.
  Interactive: string # Experimental.
  Language: string # Experimental.
  Log: string # Specifies log redirection switches and path.
  InstallLocation: string # Specifies alternate location to install package.
Installers: # Nested map of keys for specific installer.
  - Arch: string # Enumeration of supported architectures.
    Url: string # Path to download installation file.
    Sha256: string # SHA256 calculated from installer.
    SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file.
    Switches: # Collection of entries to override root keys. The primary supported values are: Custom, Silent, SilentWithProgress, Interactive. For a complete list see the specification at https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md.
    Scope: string # Experimental.
    SystemAppId: string # Experimental.
Localization: # Nested map of keys for localization.
  - Language: string # Locale for display fields and localized URLs.
ManifestVersion: string # Version number format for manifest version.
```

#### <a name="good-example"></a>[良好範例](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[更好的範例](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

> [!NOTE]
> 如果您的安裝程式是 .exe，且其為使用 Nullsoft 或 Inno 所建立，則可以改為指定這些值。 當指定 Nullsoft 或 Inno 時，用戶端會自動為安裝程式設定「無訊息」和「無訊息處理程序」的安裝行為。

## <a name="installer-switches"></a>安裝程式參數

您通常可藉由從命令列將 `-?` 傳遞至安裝程式，找出安裝程式可使用的無訊息 `Switches`。 以下是一些可用於不同安裝程式類型的常見無訊息 `Switches`。

| 安裝程式 | 命令  | 文件 |  
| :--- | :-- | :--- |  
| MSI | `/q` | [MSI 錄製進行](/windows/win32/msi/command-line-options) |
| InstallShield | `/s`  | [InstallShield 命令列參數](https://docs.flexera.com/installshield19helplib/helplibrary/IHelpSetup_EXECmdLine.htm) |
| Inno 設定 | `/SILENT or /VERYSILENT` | [Inno 安裝文件](https://jrsoftware.org/ishelp/) |
| Nullsoft | `/S` | [Nullsoft 無訊息安裝程式/解除安裝程式](https://nsis.sourceforge.io/Docs/Chapter4.html#silent) |

## <a name="tips-and-best-practices"></a>訣竅和最佳做法

* 若要在尋找及安裝軟體時獲得最佳的客戶體驗，建議您盡可能納入必要結構描述以外的多個選用項目。 例如，`AppMoniker` 欄位是選擇性的。 不過，如果您包含此欄位，在執行 [search](../winget/search.md) 命令時，客戶會看到與 `AppMoniker` 值相關聯的結果 (例如， **vscode** 代表 **Visual Studio Code** )。 如果只有一個應用程式具有指定的 `AppMoniker` 值，則客戶可以藉由指定別名來安裝應用程式，而不是指定完整識別碼。
* `Id` 必須是唯一的。 您不能以相同封裝識別碼進行多次提交。 請避免使用空格，因為在使用 [winget](../index.md) 用戶端時，這會要求使用者在 `Id` 前後加上引號。
* 請避免建立多個發行者資料夾。 例如，如果已經有 "Contoso" 資料夾，請勿再建立 "Contoso Ltd"。 建立資料夾時也請避免使用空格。
* 如果可能的話，所有封裝都應該以無訊息安裝的方式提交。 如果您的可執行檔不支援無訊息安裝，則會降低使用者體驗的品質。
* 在資訊清單中，分行符號之前的字串長度應限制為 100 個字元。
* 如果指定版本的封裝有一個以上的安裝程式類型，則 `InstallerType` 的執行個體可以放在每個 `Installers` 底下。
