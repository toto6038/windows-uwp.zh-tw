---
title: 使用 Visual studio 建立應用程式安裝程式檔案
description: 了解如何使用 Visual Studio，以便啟用透過 .appinstaller 檔案的自動更新。
ms.date: 5/2/2018
ms.topic: article
keywords: windows 10, uwp, 應用程式安裝程式, AppInstaller, 側載
ms.localizationpriority: medium
ms.openlocfilehash: 5c7055748eb8905341d9f90c47e6141c9c9c599e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7978005"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>使用 Visual studio 建立應用程式安裝程式檔案

從 Windows 10 版本 1804 與 Visual Studio 2017 更新 15.7 開始，側載應用程式可以設定為使用`.appinstaller`檔案自動接收更新。 Visual Studio 支援啟用這些更新。

## <a name="app-installer-file-location"></a>應用程式安裝程式檔案位置
`.appinstaller`檔案可以裝載於共用位置 (例如 HTTP 端點或 UNC 共用資料夾)，並包含了應用程式套件的安裝路徑。 使用者從共用位置安裝應用程式以及啟用新更新的定期檢查。 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>將專案設定為以正確的 Windows 版本為目標

您可以在建立專案時設定`TargetPlatformMinVersion`屬性，或稍後從專案屬性變更屬性。 

>[!IMPORTANT]
> 只有在`TargetPlatformMinVersion`是 Windows 10 版本 1804 或更高版本時，才會產生應用程式安裝程式檔案。


### <a name="create-packages"></a>建立套件

若要透過側載應用程式發佈，您必須建立應用程式套件 (.appx/.msix) 或應用程式套件組合 (.appxbundle/.msixbundle)，並將它發佈至共用位置。

若要這樣做，請依照下列步驟使用 Visual Studio 中的**\[建立應用程式套件\]** 精靈。

1. 在專案上按一下滑鼠右鍵，然後選擇 **\[Microsoft Store\]** -> **\[建立應用程式套件\]**。  

![導覽至 [建立應用程式套件] 的操作功能表](images/packaging-screen2.jpg)   

**\[建立應用程式套件\]** 精靈便會出現。

2. 選取 **\[我想要建立側載套件\]** 和 **\[啟用自動更新\]**。  

![顯示的 [建立您的套件] 對話方塊視窗](images/select-sideloading.png)  

只有在讓專案的 `TargetPlatformMinVersion` 設定為正確版本的 Windows 10，才會啟用 **\[啟用自動更新\]**。

3. **\[選取並設定套件\]** 對話方塊可讓您選取支援的架構設定。 如果您選擇套件組合，它將會產生單一安裝程式，但是如果您不想要套件組合並想要每個架構一個套件的話，也可以為每個架構取得一個安裝程式檔案。  如果您不確定選擇哪些架構，或想要深入了解各種裝置所使用的架構，請查看[應用程式套件架構](device-architecture.md)。

4. 設定任何其他詳細資料，例如版本編號或套件輸出位置。

![顯示套件設定的 [建立應用程式套件] 視窗](images/packaging-screen5.jpg)  

5. 當您在步驟 2 選取 **\[啟用自動更新\]**，**\[設定更新設定\]** 對話方塊會出現。 在這裡，您可以指定**\[安裝 URL\]** 和更新檢查的頻率。

![顯示發行位置設定的 [設定更新設定] 視窗](images/sideloading-screen.png)  

6. 已成功封裝應用程式時，對話方塊會顯示包含應用程式套件輸出資料夾的位置。 輸出資料夾包含側載應用程式所需的所有檔案，包括可用來促銷應用程式的 HTML 頁面。

### <a name="publish-packages"></a>發佈套件

若要讓應用程式可用，產生的檔案必須發行至指定的位置：

#### <a name="publish-to-shared-folders-unc"></a>發佈至共用資料夾 (UNC)

如果您想要透過通用命名規格 (UNC) 共用資料夾發佈套件，請將應用程式套件輸出資料夾和安裝 URL（如需詳細資訊請參閱步驟 6）設定為相同的路徑。 精靈會在正確的位置產生檔案，而且使用者會從相同的路徑取得應用程式與未來的更新。

#### <a name="publish-to-a-web-location-http"></a>發佈到網站位置 (HTTP)

發佈至網站位置需要存取權以便將內容發佈至網頁伺服器，請確認最終 URL 符合精靈中定義的安裝 URL（如需詳細資訊請參閱步驟 6）。 通常檔案傳輸通訊協定 (FTP) 或 SSH 檔案傳輸通訊協定 (SFTP) 會用來上傳檔案，但根據您的網站提供者，還有類似 MSDeploy、SSH 或 Blob 儲存等其他發佈方法。

若要設定網頁伺服器，您必須驗證用於使用中的檔案類型的 MIME 類型。 此範例中為適用於 Internet Information Services (IIS) 的 `web.config`：

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















