---
author: TylerMSFT
title: 建立通用 Windows 平台主控台應用程式
description: 本主題說明如何撰寫在主控台視窗中執行的 UWP 應用程式。
keywords: 主控台 uwp
ms.author: twhitney
ms.date: 08/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4c1b1df8ad29635f38ae5b373685d3504a4eb60
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2018
ms.locfileid: "3851398"
---
# <a name="create-a-universal-windows-platform-console-app"></a>建立通用 Windows 平台主控台應用程式

本主題說明如何建立[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)或 C + + /CX 通用 Windows 平台 (UWP) 主控台應用程式。

從 Windows 10，版本 1803 起，您可以撰寫 C + + /winrt 或 C + + /CX UWP 主控台應用程式執行於主控台視窗中，例如 DOS 或 PowerShell 主控台視窗。 主控台應用程式使用主控台視窗輸入與輸出，並可以使用[通用 C 執行階段](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference)功能，例如**printf**以及**getchar**。 UWP 主控台應用程式可發佈至 Microsoft Store。 這些應用程式均列於應用程式清單中，且皆有可釘選到 \[開始\] 功能表的主要磚。 UWP 主控台應用程式都可從 [開始] 功能表中，啟動，雖然您一般會啟動它們從命令列。

若要查看作用中的其中一個，以下是 「 creating a UWP Console App。

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>使用 UWP 主控台應用程式範本 

若要建立 UWP 應用程式主控台，請先安裝**主控台應用程式 (通用) 專案範本** (可從 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal) 取得)。 已安裝的範本，便可使用**新的專案**底下 > **已安裝** > **其他語言** > **Visual c + +** > **Windows 通用**為**主控台應用程式的 C + /winrt (通用 Windows)** 和**主機應用程式 C + + /CX (通用 Windows)**。

## <a name="add-your-code-to-main"></a>將您的程式碼新增至 main()

範本會新增 **Program.cpp**，其中包含 `main()` 函數。 這是在 UWP 主控台應用程式中執行開始的位置。 存取具有 `__argc` 與 `__argv` 參數的命令列引數。 當控制項從 `main()` 傳回時，會有 UWP 主控台應用程式存取。

下列**Program.cpp**的範例會藉由新增**主控台應用程式的 C + /winrt**範本：

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>UWP 主控台應用程式行為

UWP 主控台應用程式可從其執行目錄中或之下存取檔案系統。 這可能是因為範本將 [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 延伸模組新增至您應用程式的 Package.appxmanifest 檔案。 此延伸模組也讓使用者能夠從主控台視窗輸入別名以啟動應用程式。 應用程式不必在系統路徑中就能啟動。

您可以額外將檔案系統的廣泛存取權授與您的 UWP 主控台應用程式，做法是新增受限制的功能 `broadFileSystemAccess`，如[檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)中所述。 此功能在 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 命名空間中搭配 API 使用。

UWP 主控台應用程式同一時間可以有多個執行個體執行，因為範本將 [SupportsMultipleInstances](multi-instance-uwp.md) 功能新增至您應用程式的 Package.appxmanifest 檔案。

範本也會將 `Subsystem="console"` 功能新增至 Package.appxmanifest 檔案，這表示此 UWP 應用程式是主控台應用程式。 注意 `desktop4` 與 iot2 `namespace` 前置詞。 只有桌面與物聯網 (IoT) 專案上才支援 UWP 主控台應用程式。

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>UWP 主控台應用程式的其他考量事項

- 只有 C + + /winrt 與 C + + /CX UWP 應用程式可以是主控台應用程式。
- UWP 主控台應用程式必須以桌面或 IoT 專案類型為目標。
- UWP 主控台應用程式不可以建立視窗。 例如，系統會提示使用者同意，它們無法使用 messagebox （） 或 Location() 或任何其他 API，可能會因任何原因而建立視窗。
- UWP 主控台應用程式不可以取用背景工作，也不能作為背景工作。
- 除了[命令列啟用](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)外，UWP 主控台應用程式不支援啟用合約，包括檔案關聯、通訊協定關聯等等。
- UWP 主控台應用程式雖然支援多重執行個體，但不支援[多重執行個體重新導向](multi-instance-uwp.md)
- 如需可供 UWP 應用程式使用的 Win32 API 清單，請參閱 [UWP 應用程式適用的 Win32 與 COM API](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) (英文)