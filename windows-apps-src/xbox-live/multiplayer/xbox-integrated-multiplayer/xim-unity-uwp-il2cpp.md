---
title: 使用 XIM (Unity 含 IL2CPP)
description: 使用 Unity 使用整合式多人 Xbox 遊戲適用於 UWP 和 IL2CPP 指令碼的後端
ms.date: 04/03/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox 其中 Unity 整合多人 Xbox 遊戲
ms.localizationpriority: medium
ms.openlocfilehash: a600fd253efae1daca34241b105a69514561e01d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646443"
---
# <a name="use-xim-unity-with-il2cpp"></a>使用 XIM (Unity 含 IL2CPP)

## <a name="overview"></a>概觀

IL2CPP Unity 中的 Windows 執行階段支援

Unity 5.6f3 隨著引擎包含了可讓開發人員直接在指令碼中使用 Windows 執行階段 (WinRT) 元件，它們直接包括遊戲專案中的新功能。 直到 5.6 的開發人員需要外掛程式或 dll 以從 UWP 中的遊戲指令碼支援 （包括 Xbox 整合式多人遊戲） 的任何平台功能。 這個新的投影層無須外掛程式，並導入了新的和簡化的工作流程，只有支援選擇 IL2CPP 指令碼的後端的遊戲。

- 如需有關如何開始使用 WinRT 和 Unity 的詳細資訊，請參閱 < [Unity 文件](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)。
- 如需有關如何將 Xbox Live 的支援新增至使用 IL2CPP Unity 的詳細資訊，請參閱[Xbox Live 的文件](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp)在主題上。

## <a name="using-the-xim-unity-asset-package"></a>使用 XIM Unity 資產套件

### <a name="1-install-unity"></a>1.Instalovat Unity

安裝 Unity 5.6 或更新版本，並確保您擁有**Windows 儲存 clr、mono、il2cpp 指令碼的後端**在安裝期間選取

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2.安裝 Visual Studio Tools for Unity 3.1 版和以上適用於 IntelliSense 支援時使用 Winmd

Visual Studio 2015，這篇[Visual Studio marketplace](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)。 Visual Studio 2017，可以在 Visual Studio 2017 安裝程式加入元件。

### <a name="3-open-a-new-or-existing-unity-project"></a>3.開啟新的或現有的 Unity 專案

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4.Unity 組建設定 功能表中，切換至通用 Windows 平台的平台

![通用 Windows 平台的 Unity 組建設定 功能表建置選取的設定](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5.啟用在 Unity 播放器設定中，IL2CPP 指令碼的後端，並設為.NET 4.6 的 API 相容性

![「 Api 相容性 」 設定設為 [Unity 播放程式設定] 功能表的組態節 「.NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6.匯入整合式多人遊戲 WinRT Unity Xbox 資產套件的最新版本

這可從 https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7.您現在可以在您的指令碼中使用 XIM

如需如何使用與 XIM 詳細指引C#，請參閱[使用 XIM (C#)](using-xim-cs.md)。

下列程式碼片段顯示如何 XIM 可能會整合您的程式碼：

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

如需詳細資訊`ENABLE_WINMD_SUPPORT`#define 指示詞，請參閱 Unity 文件[Windows 執行階段支援](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)

### <a name="8-required-capability-content"></a>8.必要的功能內容

應用程式原本就使用 XIM 需要連接到並接受來自網路資源，同時透過網際網路和區域網路連線。 它也會需要存取麥克風的裝置，來支援語音聊天。 如此一來，應用程式應該宣告 「 InternetClientServer"和"PrivateNetworkClientServer 」 功能，以及在播放程式設定中找到的發行設定中的 「 麥克風 」 裝置功能。

![「 InternetClientServer"、"PrivateNetworkClientServer 」 與選取的 「 麥克風 」 功能的 unity 的功能 功能表](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9.建置在 Unity 專案。

1. 移至檔案\|建置的設定，請按一下**通用 Windows 平台**，並確定您按一下**切換平台**

2. 按一下 「 加入開啟花絮 」 將目前的場景新增至組建

3. 在 SDK 下拉式方塊中，選擇 "通用 10"

4. 在 UWP 建置型別下拉式方塊中，選擇 「 D3D"，但如果想要的話，也會運作"XAML"。

5. 按一下 [建置]，for Unity，以產生 UWP 的 Visual Studio 專案的 UWP 應用程式中包裝您的 Unity 遊戲。

    當系統提示的位置時，建立新的資料夾，以避免產生混淆，因為很多新的檔案將會建立。 建議您呼叫 「 建置 」 的資料夾，然後選取 該資料夾。

6. 專案中加入 XIM 的網路資訊清單

    加入 networkmanifest.xml 檔案：

    ![Visual Studio 的 networkmanifest.xml 屬性](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    請參閱[XIM 專案組態](xim-manifest.md)如需有關網路資訊清單和其內容。

7. 編譯並執行 UWP 應用程式從 Visual Studio

這會啟動應用程式，例如一般的 UWP 應用程式，並允許 Xbox Live 的呼叫來操作，因為它們需要函式的 UWP 應用程式容器。

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10.如果您變更任何項目在 Unity 中，重建

如果您變更 Unity 中的任何項目，然後您必須重建 UWP 專案
