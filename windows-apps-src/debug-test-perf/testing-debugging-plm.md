---
author: PatrickFarley
description: 用來偵錯和測試您 App 如何使用處理程序生命週期管理的工具及技術。
title: 處理程序生命週期管理 (PLM) 的測試與偵錯工具
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 8ac6d127-3475-4512-896d-80d1e1d66ccd
ms.localizationpriority: medium
ms.openlocfilehash: 92d03ce30443f6efe8b19f4938b35d4040d7ea70
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5884423"
---
# <a name="testing-and-debugging-tools-for-process-lifetime-management-plm"></a>處理程序生命週期管理 (PLM) 的測試與偵錯工具

UWP app 與傳統型桌面應用程式的其中一個主要差異是，UWP 產品位於應用程式容器中，受到處理程序生命週期管理 (PLM) 限制。 Runtime Broker 服務可以在所有平台上暫停、繼續，或終止 UWP app，而且在您測試或偵錯處理這些動作的程式碼時，有專屬的工具可供您用來強制執行這些轉換。

## <a name="features-in-visual-studio-2015"></a>Visual Studio 2015 中的功能

Visual Studio 2015 中內建的偵錯工具可協助您調查使用 UWP 專屬功能時的潛在問題。 您可以使用 **\[週期事件\]** 工具列 (這在執行和偵錯您的產品時會出現)，強制應用程式進入不同的 PLM 狀態。

![週期事件工具列](images/gs-debug-uwp-apps-001.png)

## <a name="the-plmdebug-tool"></a>PLMDebug 工具

PLMDebug.exe 是一項隨附於 Windows SDK 中的命令列工具，可讓您控制應用程式套件的 PLM 狀態。 工具安裝之後，預設會位於 *C:\Program Files (x86)\Windows Kits\10\Debuggers\x64*。 

PLMDebug 也允許您停用任何已安裝之應用程式套件的 PLM，這對於某些偵錯程式來說是必要的。 停用 PLM 會防止 Runtime Broker 服務在您可以偵錯前，終止您的 App。 若要停用 PLM，請使用 **/enableDebug** 切換參數，後面加上您 UWP app 的完整套件名稱** (簡短名稱、套件系列名稱或套件的 AUMID 將無法運作)：

```
plmdebug /enableDebug [PackageFullName]
```

從 Visual Studio 部署 UWP app 之後，完整套件名稱將會顯示在輸出視窗中。 或者，您也可以在 PowerShell 主控台中執行 **Get-AppxPackage** 來擷取完整套件名稱。

![執行 Get-AppxPackage](images/gs-debug-uwp-apps-003.png)

另外，您可以指定應用套件啟動時，會自動啟動之偵錯工具的絕對路徑。 如果您希望使用 Visual Studio 來這樣做，您必須指定 VSJITDebugger.exe 做為偵錯工具。 不過，VSJITDebugger.exe 需要您指定 “-p” 切換參數，以及 UWP app 的處理序識別碼 (PID)。 因為無法事先知道您的 UWP app 的 PID，這個案例無法直接使用。

您可以撰寫指令碼或工具，識別您遊戲的處理序，然後在殼層中執行 VSJITDebugger.exe，傳入 UWP app 的 PID 來解決這個限制。 下列 C# 程式碼範例說明完成這項作業的簡單方法。

```
using System.Diagnostics;

namespace VSJITLauncher
{
    class Program
    {
        static void Main(string[] args)
        {
            // Name of UWP process, which can be retrieved via Task Manager.
            Process[] processes = Process.GetProcessesByName(args[0]);

            // Get PID of most recent instance
            // Note the highest PID is arbitrary. Windows may recycle or wrap the PID at any time.
            int highestId = 0;
            foreach (Process detectedProcess in processes)
            {
                if (detectedProcess.Id > highestId)
                    highestId = detectedProcess.Id;
            }

            // Launch VSJITDebugger.exe, which resides in C:\Windows\System32
            ProcessStartInfo startInfo = new ProcessStartInfo("vsjitdebugger.exe", "-p " + highestId);
            startInfo.UseShellExecute = true;

            Process process = new Process();
            process.StartInfo = startInfo;
            process.Start();
        }
    }
}
```

這個範例用法搭配 PLMDebug：

```
plmdebug /enableDebug 279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg "\"C:\VSJITLauncher.exe\" Game"
```
其中 `Game` 是處理程序名稱，`279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg` 是範例 UWP app 套件的完整套件名稱。

請注意，每次呼叫 **/enableDebug** 都必須在稍後使用 **/disableDebug** 切換參數結合另一個 PLMDebug 呼叫。 此外，偵錯工具路徑必須是絕對路徑 (不支援相對路徑)。

## <a name="related-topics"></a>相關主題
- [部署和偵錯 UWP app](deploying-and-debugging-uwp-apps.md)
- [偵錯、測試及效能](index.md)
