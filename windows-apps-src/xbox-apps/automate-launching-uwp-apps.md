---
title: 自動化啟動 Windows 10 通用 Windows 平台 (UWP) 應用程式
description: 開發人員可以使用通訊協定啟用和啟動啟用來自動化啟動他們的 UWP app 或遊戲，以自動進行測試。
ms.topic: article
ms.localizationpriority: medium
ms.date: 02/08/2017
ms.openlocfilehash: fb68b4bbd1b751591e9f336efe5dad3c22b3bf92
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618303"
---
# <a name="automate-launching-windows-10-uwp-apps"></a>自動化啟動 Windows 10 UWP App

## <a name="introduction"></a>簡介

開發人員有達成自動化啟動 Universal Windows 平台 (UWP) 應用程式的數個選項。 在本文中，我們將探索使用通訊協定啟用和啟動啟用來啟動 app 的各種方法。

*通訊協定啟用*可讓 app 自行登錄為特定通訊協定的處理常式。 

*啟動啟用*是正常啟動 app，例如從應用程式磚啟動。

利用每個啟用方法，您就可以選擇使用命令列或啟動程式應用程式。 針對所有的啟用方法，如果 app 目前正在執行，啟用會將 app 帶至前景 (這會重新啟動它) 並提供新的啟用引數。 這可讓使用啟用命令更有彈性，以提供 app 的新訊息。 請務必注意專案需要針對啟用方法進行編譯與部署以執行新更新的 app。 

## <a name="protocol-activation"></a>通訊協定啟用

請依照下列步驟來設定 app 的通訊協定啟用： 

1. 在 Visual Studio 中開啟 **Package.appxmanifest** 檔案。
2. 選取 [宣告] 索引標籤。
3. 在 [可用的宣告] 下拉式清單下，選取 [通訊協定]，然後選取 [新增]。
4. 在 [名稱] 欄位中的 [屬性] 下，輸入要啟動的 app 的唯一名稱。 

    ![通訊協定啟用](images/automate-uwp-apps-1.png)

5. 儲存檔案並部署專案。 
6. 部署專案之後，通訊協定啟用就應該已經設定完畢。 
7. 移至 [控制台\所有控制台項目\預設程式] 並選取 [建立檔案類型或通訊協定與特定程式之間的關聯]。 捲動到 [通訊協定] 區段，以查看是否列出通訊協定。 

這樣就已設定通訊協定啟用，您就有使用通訊協定啟動 app 的兩個選項 (命令列或啟動程式應用程式)。 

### <a name="command-line"></a>命令列

App 就可以透過使用包含 start 命令，後面接著之前設定的通訊協定名稱、一個冒號 (":") 及任何參數的命令列，以透過通訊協定啟動的方式啟動。 參數可以是任意字串；不過，若要利用統一資源識別元 (URI) 功能，建議您遵循標準的 URI 格式： 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

URI 物件都有剖析此格式之 URI 字串的方法。 如需詳細資訊，請參閱 [URI 類別 (MSDN)](https://msdn.microsoft.com/library/windows/apps/windows.foundation.uri.aspx)。 

範例：

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

通訊協定命令列啟用支援在原始 URI 上 Unicode 字元最多 2038 個字元的限制。 

### <a name="launcher-application"></a>啟動程式應用程式

針對啟動，請建立支援 WinRT API 的個別應用程式。 下列範例中顯示利用啟動程式中的通訊協定進行啟動的 C++ 程式碼，其中 **PackageURI** 是包含任何引數之應用程式的 URI；例如 `myapplication:` 或 `myapplication:protocol activation arguments`。

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
使用啟動程式應用程式的通訊協定啟用具有與使用命令列之通訊協定啟用相同的引數限制。 兩者在原始 URI 上都支援最多 2038 個字元的 Unicode 字元限制。 

## <a name="launch-activation"></a>啟動啟用

您也可以使用啟動啟用來啟動 app。 不需要設定，但是需要 UWP app 的應用程式使用者模型識別碼 (AUMID)。 AUMID 是加上驚嘆號和應用程式識別碼的套件系列名稱。 

取得套件系列名稱最好的方式就是完成下列步驟：

1. 開啟 **Package.appxmanifest** 檔案。
2. 在 [封裝] 索引標籤中，輸入**套件名稱**。

    ![啟動啟用](images/automate-uwp-apps-2.png)

3. 如果未列出 [套件系列名稱]，請開啟 PowerShell 並執行 `>get-appxpackage MyPackageName` 以尋找 **PackageFamilyName**。

應用程式識別碼可以在 `<Applications>` 元素底下的 **Package.appxmanifest** 檔案 (在 XML 檢視中開啟) 中找到。

### <a name="command-line"></a>命令列

執行 UWP App 啟動啟用的工具，會與 Windows 10 SDK 一起安裝。 它可以從命令列執行，它需要 app 的 AUMID 做為引數啟動。

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

它看起來就像這樣：

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

此選項不支援命令列引數。 

### <a name="launcher-application"></a>啟動程式應用程式

您可以建立支援使用 COM 來進行啟動的個別應用程式。 下列範例顯示利用啟動程式中的啟動啟用進行啟動的 C++ 程式碼。 您可以使用這個程式碼建立 **ApplicationActivationManager** 物件並在先前發現的 AUMID 和任何引數中呼叫 **ActivateApplication** 傳遞。 如需其他參數的詳細資訊，請參閱 [IApplicationActivationManager::ActivateApplication 方法 (MSDN)](https://msdn.microsoft.com/library/windows/desktop/hh706903(v=vs.85).aspx)。

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

值得一提的是此方法不支援傳入的引數，這不像進行啟動的前一個方法 (也就是，使用命令列)。

## <a name="accepting-arguments"></a>接受引數

若要接受在啟動 UWP app 時傳入的引數，您必須新增某些程式碼至 app。 若要判斷是否發生通訊協定啟用或啟動啟用，請覆寫 **OnActivated** 事件並檢查引數類型，然後取得原始的字串或 URI 物件的預先剖析的值。 

這個範例示範如何取得原始的字串。

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
            Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
}
```

## <a name="summary"></a>摘要
在摘要中，您可以使用各種方法來啟動 UWP app。 根據需求和使用情況，某些方法可能會比其他方法更適合。 

## <a name="see-also"></a>請參閱
- [在 Xbox One UWP](index.md)

