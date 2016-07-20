---
author: mcleblanc
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: "使用 Visual Studio 測試 Surface Hub app"
description: "Visual Studio 模擬器提供了可設計、開發、偵錯和測試 UWP 應用程式 (包括針對 Surface Hub 建置的應用程式) 的環境。"
translationtype: Human Translation
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: 655dffb5f1948724c894de291e119be1322f6e77

---

# 使用 Visual Studio 測試 Surface Hub app
Visual Studio 模擬器提供了可讓您設計、開發、偵錯和測試通用 Windows 平台 (UWP) app (包括針對 Microsoft Surface Hub 建置的應用程式) 的環境。 此模擬器不會使用與 Surface Hub 相同的使用者介面，但很適合以 Surface Hub 的畫面大小與解析度測試您應用程式的外觀和行為。

如需詳細資訊，請參閱[在模擬器中執行 Windows 市集應用程式](https://msdn.microsoft.com/library/hh441475.aspx)。

## 將 Surface Hub 解析度新增到模擬器
若要將 Surface Hub 解析度新增到模擬器：

1. 將下列 XML 儲存到名為 **HardwareConfigurations-SurfaceHub55.xml** 的檔案中，以建立適用於 55 吋 Surface Hub 的設定。  

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

2. 將下列 XML 儲存到名為 **HardwareConfigurations-SurfaceHub84.xml** 的檔案中，以建立適用於 84 吋 Surface Hub 的設定。

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

3. 將這兩個 XML 檔案複製到 **C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;版本號碼&gt;\HardwareConfigurations** 中。

   > **注意** &nbsp;&nbsp;需要有系統管理權限，才能將檔案儲存到這個資料夾。

4. 在 Visual Studio 模擬器中執行 app。 按一下調色盤上的 [變更解析度]**** 按鈕，然後從清單中選取 Surface Hub 設定。

    ![Visual Studio 模擬器解析度](images/vs-simulator-resolutions.png)

   > **提示** &nbsp;&nbsp; [開啟平板電腦模式](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet)可模擬更貼近 Surface Hub 上的體驗。

## 從 Visual Studio 將應用程式部署到 Surface Hub
以手動方式部署應用程式是很簡單的程序。

### 啟用開發人員模式
根據預設，Surface Hub 只會從 Windows 市集安裝應用程式。 若要安裝其他來源所簽署的應用程式，您必須啟用開發人員模式。

> **注意** &nbsp;&nbsp;啟用開發人員模式之後，您必須重設 Surface Hub 才能再次將它停用。 重設裝置會移除所有本機使用者檔案和設定，然後重新安裝 Windows。

1. 從 Surface Hub 的 [開始]**** 功能表，開啟 [設定] 應用程式。

   >  **注意** &nbsp;&nbsp;需要有系統管理權限，才能存取 [設定] 應用程式。

2. 瀏覽至 [更新與安全性] &gt; [適用於開發人員]****。

3. 選擇 [開發人員模式]**** 並接受警告提示。

### 從 Visual Studio 部署您的應用程式。
如需詳細資訊，請參閱[部署和偵錯通用 Windows 平台 (UWP) 應用程式](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)。

   > **注意** &nbsp;&nbsp;此功能至少需要 **Visual Studio 2015 Update 1**。

1. 請瀏覽到 [開始偵錯]**** 按鈕旁的偵錯目標下拉式清單，並選取 [遠端電腦]****。

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 偵錯目標下拉式清單](images/vs-debug-target.png)

2. 輸入 Surface Hub 的 IP 位址。 確定已選取 [通用]**** 驗證模式。

   > **提示** &nbsp;&nbsp;啟用開發人員模式之後，即可在歡迎畫面上找到 Surface Hub 的 IP 位址。

3. 選擇 [開始偵錯 (F5)]**** 可在 Surface Hub 上部署及偵錯您的應用程式，或按 Ctrl+F5 只部署您的應用程式。

   > **提示** &nbsp;&nbsp;如果 Surface Hub 在歡迎畫面上，選擇任何按鈕即可將它關閉。



<!--HONumber=Jun16_HO4-->


