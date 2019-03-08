---
title: XDK 適用的 Unity (含 IL2CPP 後端)
description: 加入 Xbox Live 支援 Unity 的 XDK IL2CPP 指令碼的後端使用ID@Xbox及管理合作夥伴
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox 其中 Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608463"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>加入 Xbox Live 支援 Unity 的 XDK IL2CPP 指令碼的後端使用ID@Xbox及管理合作夥伴

## <a name="overview"></a>概觀

IL2CPP Unity 中的 Windows 執行階段支援

Unity 5.6f3 隨著引擎包含了可讓開發人員直接在指令碼中使用 Windows 執行階段 (WinRT) 元件，它們直接包括遊戲專案中的新功能。 直到 5.6 的開發人員需要外掛程式或 dll 以從遊戲指令碼支援 （包括 Xbox Live SDK） 的任何平台功能。 這個新的投影層無須外掛程式，並導入了新的和簡化的工作流程，只有支援選擇 IL2CPP 指令碼的後端的遊戲。

如需有關如何開始使用的詳細資訊，請參閱 Unity 文件： https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>步驟

**1） 安裝 Unity**

安裝 Unity 5.6 或更新版本，並請確定您已安裝的 Xbox One 編輯器延伸模組。

**2） 安裝 Visual Studio Tools for Unity 3.1 版和以上適用於 IntelliSense 支援時使用 Winmd** For Visual Studio 2015，這位於 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity。  Visual Studio 2017，可以在 Visual Studio 2017 安裝程式加入元件。

**3） 中開啟新的或現有的 Unity 專案**

**4） 切換到 Xbox One Unity 組建設定 功能表中的平台**

**5） 啟用 IL2CPP 輸入指令碼的後端在 Unity 播放器設定中，並設為.NET 4.6 的 API 相容性**

![](../images/unity/unity-il2cpp-1.png)

**6） 切換至 Roslyn，指令碼編譯器**

**7） 的 Xbox One 的適當系統程式庫將所有會自動新增至您的專案，並包含平台二進位檔所需的任何額外的步驟。**

**8） 新增並附加新的 C\# Unity 物件的指令碼。**

例如，"Main Camera"，例如 Unity 物件上按一下，然後按一下 [新增元件] \| 「 新的指令碼 」 \| C\#指令碼\|並將它命名為"XboxLiveScript 」。 會執行任何遊戲的物件。

**9） （與 VSTU 3.1 + 安裝) 的 Visual Studio 中開啟指令碼**

您會注意到兩個專案，開啟您的遊戲指令碼 XboxLiveTest.cs VSTU 所產生的"Player"專案中

![](../images/unity/unity-il2cpp-2.png)

這是針對 XDK，產生的特殊專案，而且包含 winmd 檔案，您已置於您的資產的參考。
它也會定義 「 #if ENABLE_WINMD_SUPPORT 」 定義為您，讓 IntelliSense 和語法反白顯示會正常運作。

**10） 將下列的 Xbox Live 程式碼新增至 XboxLiveTest.cs 原始程式檔**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**11） 請確定您有在播放程式設定中找到的發行設定中選取的 'InternetClient' 功能**

![](../images/unity/unity-il2cpp-3.png)

**12） 來建置 Unity 專案。**
