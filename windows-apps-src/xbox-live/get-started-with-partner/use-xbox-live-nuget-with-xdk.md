---
title: 使用 XDK 的 Xbox Live 的 NuGet 封裝
description: 了解如何使用 Xbox Live API NuGet 套件來開發 XDK 標題。
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 其中一個，NuGet xbox
ms.localizationpriority: medium
ms.openlocfilehash: c955ca42c09075e5125683588c335cfa47443f00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655033"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>使用 Xbox Live API NuGet 套件來開發 XDK 標題

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.請確定您已安裝最新 NuGet 套件管理員
1.  檢查目前的版本：
    - 在功能表列選取 工具-> 擴充功能和更新。
    - 在 [已安裝] 索引標籤中，尋找 `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  若要更新您目前的版本：
    - 在功能表列選取 工具-> 擴充功能和更新。
    - 在 [更新]-> [Visual Studio 組件庫] 索引標籤下選取 `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.將參考加入至專案
1.  將參考加入至專案
    1.  以滑鼠右鍵按一下您專案的方案，然後選取 [管理 NuGet 封裝]
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  搜尋`Xbox Live`並選取適當的套件，然後按一下`Install`。
  - UWP 和 XDK，以及 c + + 和 WinRT 類別有 Xbox 服務 API。  
  - 選擇`Microsoft.Xbox.Live.SDK.*.UWP`和`Microsoft.Xbox.Live.SDK.*.XboxOneXDK`。  `XboxOneXDK` 適用於ID@Xbox和 Managed 開發人員使用 Xbox One XDK。  `UWP` 適用於 UWP 的奧運賽可以在 PC、 Xbox One 或 Windows Phone 上執行。  您可以深入了解 Xbox One 在上執行 UWP [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - 選擇`Microsoft.Xbox.Live.SDK.Cpp.*`和`Microsoft.Xbox.Live.SDK.WinRT.*`。 `Cpp` 適用於 c + + 的遊戲引擎使用 Xbox Live Api。  `WinRT` 使用 c + + 撰寫的遊戲引擎做為C#，或使用 Xbox Live Api 的 Javascript。  使用 c + + 引擎 WinRT 時，您會使用 C + + /CX 使用 hats (^)。  `Cpp` 是建議的 API，可用於 c + + 的遊戲引擎。    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. 接受授權 TOS 之後等候，直到已成功新增封裝。  您應該會看到此記錄檔封裝管理員的 [輸出] 視窗中：

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.選擇性地包含標頭
* 針對`Microsoft.Xbox.Live.SDK.Cpp.*`型專案`#include <xsapi\services.h>`中專案的來源。
* 針對`Microsoft.Xbox.Live.SDK.WinRT.*`架構專案，不需要包含任何標頭。   
