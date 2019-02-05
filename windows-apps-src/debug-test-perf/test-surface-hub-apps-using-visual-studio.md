---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: 使用 Visual Studio 測試 Surface Hub app
description: Visual Studio 模擬器提供了可設計、開發、偵錯和測試 UWP 應用程式 (包括針對 Surface Hub 建置的應用程式) 的環境。
ms.date: 10/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: db481fac1bdcb9e79762f52aee48574e987c4cbb
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048881"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>使用 Visual Studio 測試 Surface Hub 應用程式
Visual Studio 模擬器提供了可讓您設計、開發、偵錯和測試通用 Windows 平台 (UWP) app (包括針對 Microsoft Surface Hub 建置的應用程式) 的環境。 模擬器不會使用相同的使用者介面為 Surface Hub，但很適合用來測試您的應用程式的外觀和行為與 Surface Hub 的螢幕大小與解析度。

如需詳細資訊，模擬器工具在一般情況下，請參閱[在模擬器中執行的 UWP 應用程式](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator)。

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>將 Surface Hub 解析度新增到模擬器
若要將 Surface Hub 解析度新增到模擬器：

1. 建立適用於 55 」 設定 Surface Hub，藉由將下列 XML 程式碼儲存到名為*Hardwareconfigurations-surfacehub55.xml 中*的檔案。  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. 建立適用於 84 」 設定 Surface Hub，藉由將下列 XML 程式碼儲存到名為*Hardwareconfigurations-surfacehub84.xml 中*的檔案。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. 將這兩個 XML 檔案複製到 *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;版本號碼&gt;\HardwareConfigurations* 中。

   > [!NOTE]
   > 系統管理員權限，才能將檔案儲存到這個資料夾。

4. 在 Visual Studio 模擬器中執行 App。 按一下調色盤上的 **\[變更解析度\]** 按鈕，然後從清單中選取 Surface Hub 設定。

    ![Visual Studio 模擬器解析度](images/vs-simulator-resolutions.png)

   > [!TIP]
   > 若要更好的[開啟平板電腦模式](https://windows.microsoft.com/windows-10/getstarted-like-a-tablet)模擬 Surface Hub 的體驗。

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>從 Visual Studio 將應用程式部署到 Surface Hub 裝置
手動部署到 Surface Hub 的 [應用程式是簡單的程序。

### <a name="enable-developer-mode"></a>啟用開發人員模式
根據預設，Surface Hub 只會從 Microsoft Store 安裝應用程式。 若要安裝其他來源所簽署的 App，您必須啟用開發人員模式。

> [!NOTE]
> 已啟用開發人員模式之後，您必須重設 Surface Hub，如果您想要再次將它停用。 重設裝置會移除所有本機使用者檔案和設定，然後重新安裝 Windows。

1. 從 Surface Hub 的 **\[開始\]** 功能表，開啟 [設定] 應用程式。

   > [!NOTE]
   > 系統管理員權限，才能存取 Surface Hub 上的 [設定] app。

2. 瀏覽至**更新 & 安全性 \> 適用於開發人員**。

3. 選擇 **\[開發人員模式\]** 並接受警告提示。

### <a name="deploy-your-app-from-visual-studio"></a>從 Visual Studio 部署您的應用程式。
如需詳細資訊，部署程序一般情況下，看到[部署和偵錯 UWP 應用程式](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)。

   > [!NOTE]
   > 此功能會需要 Visual Studio 2015 Update 1 或更新版本，但我們建議您先使用最新的最新版本的 Visual Studio。 最新的 Visual Studio 執行個體將會在您的所有最新的開發及安全性更新 gibe。

1. 瀏覽至 **\[開始偵錯\]** 按鈕旁的偵錯目標下拉式清單，並選取 **\[遠端電腦\]**。

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 偵錯目標下拉式清單](images/vs-debug-target.png)

2. 輸入 Surface Hub 的 IP 位址。 確定已選取 **\[通用\]** 驗證模式。

   > [!TIP] 
   > 啟用開發人員模式之後，您可以在歡迎畫面上找到 Surface Hub 的 IP 位址。

3. 選取 [**開始偵錯 (F5)** 部署和偵錯您的 Surface Hub 上的應用程式，或按 Ctrl + F5 只部署您的應用程式。

   > [!TIP]
   > 如果 Surface Hub 顯示歡迎畫面，選擇任何按鈕關閉它。
