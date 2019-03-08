---
title: UWP 適用的 Unity (含 IL2CPP 後端)
description: 加入 Xbox Live 支援 Unity 適用於 UWP 的 IL2CPP 指令碼的後端ID@Xbox及管理合作夥伴
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox 其中 Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622913"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>加入 Xbox Live 支援 Unity 適用於 UWP 的 IL2CPP 指令碼的後端ID@Xbox及管理合作夥伴

## <a name="overview"></a>概觀

IL2CPP Unity 中的 Windows 執行階段支援

Unity 5.6f3 隨著引擎包含了可讓開發人員直接在指令碼中使用 Windows 執行階段 (WinRT) 元件，它們直接包括遊戲專案中的新功能。 直到 5.6 的開發人員需要外掛程式或 dll 以從 UWP 中的遊戲指令碼支援 （包括 Xbox Live SDK） 的任何平台功能。 這個新的投影層無須外掛程式，並導入了新的和簡化的工作流程，只有支援選擇 IL2CPP 指令碼的後端的遊戲。

如需有關如何開始使用的詳細資訊，請參閱 Unity 文件： https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>步驟

**1） 安裝 Unity**

安裝 Unity 5.6 或更新版本，並確保您擁有**Windows 儲存 clr、mono、il2cpp 指令碼的後端**在安裝期間選取

**2） 安裝 Visual Studio Tools for Unity 3.1 版和以上適用於 IntelliSense 支援時使用 Winmd** For Visual Studio 2015，這位於 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity。  Visual Studio 2017，可以在 Visual Studio 2017 安裝程式加入元件。

**3） 中開啟新的或現有的 Unity 專案**

**4） 交換器以通用 Windows 平台 Unity 組建設定 功能表中的平台**

**5） 啟用 IL2CPP 輸入指令碼的後端在 Unity 播放器設定中，並設為.NET 4.6 的 API 相容性**

![](../images/unity/unity-il2cpp-1.png)

**6） 匯入最新版的 Xbox Live 的 WinRT Unity 資產封裝**這樣，請參閱 https://github.com/Microsoft/xbox-live-api/releases

**7） 新增並附加新的 C\# Unity 物件的指令碼。**

例如，"Main Camera"，例如 Unity 物件上按一下，然後按一下 [新增元件] \| 「 新的指令碼 」 \| C\#指令碼\|並將它命名為"XboxLiveScript 」。 會執行任何遊戲的物件。

**8） （與 VSTU 3.1 + 安裝) 的 Visual Studio 中開啟指令碼**

您會注意到兩個專案，開啟您的遊戲指令碼 XboxLiveTest.cs VSTU 所產生的"Player"專案中

![](../images/unity/unity-il2cpp-2.png)

這是適用於 UWP，產生的特殊專案，而且包含 winmd 檔案，您已置於您的資產的參考。
它也會定義 「 #if ENABLE_WINMD_SUPPORT 」 定義為您，讓 IntelliSense 和語法反白顯示會正常運作。

**9） XboxLiveTest.cs 原始程式檔中加入下列的 Xbox Live 程式碼**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**10） 請確定您有在播放程式設定中找到的發行設定中選取的 'InternetClient' 功能**

![](../images/unity/unity-il2cpp-3.png)

**11） 來建置 Unity 專案。**

1.  移至檔案\|建置的設定，請按一下**通用 Windows 平台**，並確定您按一下**切換平台**

2.  按一下 「 加入開啟花絮 」 將目前的場景新增至組建

3.  在 SDK 下拉式方塊中，選擇 "通用 10"

4.  在 UWP 建置型別下拉式方塊中，選擇 「 D3D"，但如果想要的話，也會運作"XAML"。

5.  按一下 [建置]，for Unity，以產生 UWP 的 Visual Studio 專案的 UWP 應用程式中包裝您的 Unity 遊戲。 當系統提示的位置時，建立新的資料夾，以避免產生混淆，因為很多新的檔案將會建立。 建議您呼叫 「 建置 」 的資料夾，然後選取 資料夾

**12） 將 Xbox Live 的組態新增至您的專案**

加入 xboxservices.config 檔案：

![](../images/unity/unity-il2cpp-4.png)

請遵循文件頁面呼叫[新增新的或現有的 UWP 專案 Xbox Live](get-started-with-visual-studio-and-uwp.md)

> [!NOTE]
> Xboxservices.config 內的所有值都都區分大小寫的。

**13） 編譯並執行 UWP 應用程式從 Visual Studio**

這會啟動應用程式，例如一般的 UWP 應用程式，並允許 Xbox Live 的呼叫來操作，因為它們需要函式的 UWP 應用程式容器。

**14） 重建如果您變更 Unity 中的任何項目**  
如果您變更 Unity 中的任何項目，您必須重建 UWP 專案。

請注意 Unity 將會取代您的 pfx 檔案，當您重新編譯這樣將會造成 Xbox Live 登入失敗，因此您必須更新它在 Unity 專案，若要避免這個問題。

若要這樣做，請移至檔案\|建置設定 上按一下 建置設定**通用 Windows 平台**播放程式，並按一下與您有上述的 PFX 按鈕，即可將 PFX 檔案。 您也可以刪除 PFX 檔案每次重建在 Unity 內的專案。

## <a name="troubleshooting-common-issues"></a>針對常見問題進行疑難排解

**1)** 如果 Unity 有相關聯的指令碼可以不會載入，則請確定您並未步驟 3，以將 WinMD 拖曳至 Unity 專案的 [資產] 面板

**2)** 如果應用程式可以在啟動時立即當機，或嘗試執行這行程式碼時：

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

請確定您 xboxservices.config 文字檔案加入至專案和其屬性、 設定 [建置動作] [內容]，以和"複製到輸出的目錄 」 設定 「 永遠複製 」。
也請確定它包含正確 JSON 的格式 TitleId 以小數格式，例如：

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** 若應用程式啟動時，但無法登入再檢查下列：

a） 您的電腦設定為您的開發人員沙箱。  使用 Xbox Live SDK 的 \Tools 資料夾中的 SwitchSandbox.cmd 指令碼，若要這樣做。

b） 您正登入可存取開發人員沙箱的 Xbox Live 帳戶。  一般的零售 Xbox Live 帳戶無權存取。  您可以使用 XDP 或合作夥伴中心建立測試帳戶。

c） 您 package.appxmanfiest UWP 應用程式中的設定為正確的身分識別。  您可以手動編輯這個檔案，但若要修正此問題最簡單方式是在 Visual Studio 中的專案上按一下滑鼠右鍵，然後選擇 Store \| 」 建立關聯的應用程式與市集建立關聯。

d) Unity 所提供的內建的.pfx 檔案將不會有正確的身分識別，因此是從磁碟中刪除，在.csproj 中參考它，移除行，或權限在 Visual Studio 中的專案上按一下，然後選擇 「 市集 」 \| 」 建立關聯的應用程式與市集建立關聯「 這會將放在下一個適當的.pfx 檔案。  請務必然後若要返回到 Unity 中，按一下 建置設定 上**通用 Windows 平台**播放程式，並按一下 PFX 按鈕，以取代您的.pfx 檔案已從 Visual Studio 的 「 建立關聯的應用程式與市集建立關聯 動作。
