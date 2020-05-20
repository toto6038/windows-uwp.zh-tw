---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: 使用 Visual Studio 測試 Surface Hub應用程式
description: Visual Studio 模擬器提供了可設計、開發、偵錯和測試 UWP 應用程式 (包括針對 Surface Hub 建置的應用程式) 的環境。
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37c7f9edbaee008b6e16ef2ca202ff5cbcf39ca2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "67317500"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>使用 Visual Studio 測試 Surface Hub應用程式
Visual Studio 模擬器提供了可讓您設計、開發、偵錯和測試通用 Windows 平台 (UWP) app (包括針對 Microsoft Surface Hub 建置的應用程式) 的環境。 此模擬器不會使用與 Surface Hub 相同的使用者介面，但很適合以 Surface Hub 的畫面大小與解析度測試您應用程式的外觀和行為。

如需模擬器工具的一般詳細資訊，請參閱[在模擬器中執行 UWP 應用程式](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator)。

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>將 Surface Hub 解析度新增到模擬器
若要將 Surface Hub 解析度新增到模擬器：

1. 將下列 XML 程式碼儲存到名為 *HardwareConfigurations-SurfaceHub55.xml* 的檔案中，以建立適用於 55 吋 Surface Hub 的設定。  

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

2. 將下列 XML 程式碼儲存到名為 *HardwareConfigurations-SurfaceHub84.xml* 的檔案中，以建立適用於 84 吋 Surface Hub 的設定。

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
   > 必須要有系統管理權限，才能將檔案儲存到這個資料夾。

4. 在 Visual Studio 模擬器中執行 App。 按一下調色盤上的 [變更解析度]  按鈕，然後從清單中選取 Surface Hub 設定。

    ![Visual Studio 模擬器解析度](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [開啟平板電腦模式](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet)可以模擬更貼近 Surface Hub 上的體驗。

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>從 Visual Studio 將應用程式部署到 Surface Hub
將應用程式手動部署到 Surface Hub 是簡單的流程。

### <a name="enable-developer-mode"></a>啟用開發人員模式
根據預設，Surface Hub 只會從 Microsoft Store 安裝應用程式。 若要安裝其他來源所簽署的 App，您必須啟用開發人員模式。

> [!NOTE]
> 啟用開發人員模式之後，如果要停用此模式，必須重設 Surface Hub 才能停用它。 重設裝置會移除所有本機使用者檔案和設定，然後重新安裝 Windows。

1. 從 Surface Hub 的 [開始]  功能表，開啟 [設定] 應用程式。

   > [!NOTE]
   > 必須要有系統管理權限，才能在 Surface Hub 上存取 [設定] 應用程式。

2. 瀏覽至 [更新與安全性] \> [適用於開發人員]。

3. 選擇 [開發人員模式]  並接受警告提示。

### <a name="deploy-your-app-from-visual-studio"></a>從 Visual Studio 部署您的應用程式。
如需有關一般部署程序的詳細資訊，請參閱[部署和偵測 UWP 應用程式](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)。

   > [!NOTE]
   > 這項功能需要 Visual Studio 2015 Update 1 或更新版本，但我們建議您最好使用最新版本的 Visual Studio。 Visual Studio 執行個體的最新版本會有所有最新的開發和安全性更新。

1. 瀏覽至 [開始偵錯]  按鈕旁的偵錯目標下拉式清單，並選取 [遠端電腦]  。

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 偵錯目標下拉式清單](images/vs-debug-target.png)

2. 輸入 Surface Hub 的 IP 位址。 確定已選取 [通用]  驗證模式。

   > [!TIP] 
   > 啟用開發人員模式之後，即可在歡迎畫面上找到 Surface Hub 的 IP 位址。

3. 選取 [開始偵錯 (F5)]  以在 Surface Hub 上進行應用程式的部署和偵錯，或按 Ctrl+F5 只部署您的應用程式。

   > [!TIP]
   > 如果 Surface Hub 顯示歡迎畫面，選擇任何按鈕即可將它關閉。
