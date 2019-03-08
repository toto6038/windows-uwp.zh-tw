---
title: 適用於 UWP unity 中使用.NET 指令碼
description: 加入 Xbox Live 支援 Unity 適用於 UWP 的.NET 指令碼的後端ID@Xbox及管理合作夥伴
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox 其中 Unity
ms.localizationpriority: medium
ms.openlocfilehash: 8c4ca9d58f89e215563adcc7985b978641efdf07
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594083"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>加入 Xbox Live 支援 Unity 適用於 UWP 的.NET 指令碼的後端ID@Xbox及管理合作夥伴

**1） 安裝 Unity**

安裝 Unity 5.3 或更新版本，而且在 Unity 安裝程序，檢查 「 Windows 市集.NET 指令碼處理後端 」 元件。

![](../images/unity/unity1-install.png)

**2） 開啟新的或現有的 Unity 專案**

它可以是 3D 或 2D 的專案。 其中一種類型可使用 Xbox Live SDK。

**3） 匯入 Xbox Live 的 WinRT Unity 資產封裝，這可在最新的版本 https://github.com/Microsoft/xbox-live-api/releases**

**4） 新增並附加新的 C\# Unity 物件的指令碼。**

例如，"Main Camera"，例如 Unity 物件上按一下，然後按一下 [新增元件] \| 「 新的指令碼 」 \| C\#指令碼\|並將它命名為"XboxLiveScript 」。 會執行任何遊戲的物件。

**5） 來建置 Unity 專案。**

1.  移至檔案\|Build Settings]，按一下 [Windows 市集，請確定您按一下 「 切換平台 」

2.  按一下 「 加入開啟花絮 」 將目前的場景新增至組建

3.  在 SDK 下拉式方塊中，選擇 "通用 10"

4.  在 UWP 建置型別下拉式方塊中，選擇 「 D3D"，但如果想要的話，也會運作"XAML"。

5.  按一下 「 Unity C\#專案 」 核取方塊以產生組件 Csharp.dll 專案

6.  按一下 [建置]，for Unity，以產生 UWP 的 Visual Studio 專案的 UWP 應用程式中包裝您的 Unity 遊戲。 當系統提示的位置時，建立新的資料夾，以避免產生混淆，因為很多新的檔案將會建立。 建議您呼叫 「 建置 」 的資料夾，然後選取 資料夾

![](../images/unity/unity3-buildsettings.png)


**6） 在 Visual Studio 中開啟產生的 UWP 專案**

Unity 會在 [總管] 開啟 [輸出] 專案資料夾。  略過而非.sln 檔案。  改為瀏覽到您的組建資料夾，然後在 Visual Studio 中開啟產生的.sln。  

您會看到此方案中的 3 個專案。

1.  組件 CSharp。 這是您的 Xbox Live 的指令碼所在

2.  組件 Csharp firstpass。 基於本文的目的，您可以忽略此專案。

3.  您的專案名稱為基礎的 UWP 應用程式。 這是傳統的 UWP 應用程式裝載 Unity 引擎。 這是在其中您將會設定一些 Xbox Live 設定類似於傳統的 UWP 應用程式。


**7） 將 Xbox Live 的組態新增至 UWP 應用程式**

請遵循文件頁面呼叫[新增新的或現有的 UWP 專案 Xbox Live](get-started-with-visual-studio-and-uwp.md)

**8） 將 Xbox Live 的程式碼新增至您的指令碼**

複製/貼上此範例的 Xbox Live 的程式碼到您連接至遊戲物件的指令碼。 此指令碼會出現在 「 組件 CSharp"專案。 您可以變更所需的程式碼。

```csharp
#if NETFX_CORE

using UnityEngine;
using System;
using Microsoft.Xbox.Services.System;

public class XboxLiveScript : MonoBehaviour
{
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();
    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
    string debugText = "";

    void Start()
    {
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;

        UIDispatcher = cw.Dispatcher;
        SignIn();
    }

    void Update()
    {
    }

    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }

    async void SignIn()
    {
        SignInResult result = await m_user.SignInAsync(UIDispatcher);

        if (result.Status == SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }
    }
}

#endif
```

**9） 編譯並執行 UWP 應用程式從 Visual Studio**

這會啟動應用程式，例如一般的 UWP 應用程式，並允許 Xbox Live 的呼叫來操作，因為它們需要函式的 UWP 應用程式容器。

**10） 重建如果您變更 Unity 中的任何項目**  
如果您變更 Unity 中的任何項目，您必須重建 UWP 專案。

請注意 Unity 將會取代您的 pfx 檔案，當您重新編譯這樣將會造成 Xbox Live 登入失敗，因此您必須更新它在 Unity 專案，若要避免這個問題。

若要這樣做，請移至檔案\|組建設定，Windows 市集的播放程式上按一下 建置設定，然後按一下 PFX 按鈕，以取代您有上述其中一個的 PFX 檔案。 您也可以刪除 PFX 檔案每次重建在 Unity 內的專案。

## <a name="troubleshooting-common-issues"></a>針對常見問題進行疑難排解

**1)** 如果 Unity 有相關聯的指令碼可以不會載入，則請確定您並未步驟 3，以將 WinMD 拖曳至 Unity 專案的 [資產] 面板

**2)** 或如果應用程式當機遷移在啟動時嘗試執行這行程式碼：

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

請確定您 xboxservices.config 文字檔案加入至專案和其屬性、 設定 [建置動作] [內容]，以和"複製到輸出的目錄 」 設定 「 永遠複製 」。

> [!NOTE]
> Xboxservices.config 內的所有值都都區分大小寫的。

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

d) Unity 所提供的內建的.pfx 檔案將不會有正確的身分識別，因此是從磁碟中刪除，在.csproj 中參考它，移除行，或權限在 Visual Studio 中的專案上按一下，然後選擇 「 市集 」 \| 」 建立關聯的應用程式與市集建立關聯「 這會將放在下一個適當的.pfx 檔案。  請務必，然後移回 Unity，按一下 建置設定 在 Windows 市集播放程式，然後按一下 PFX 按鈕，以取代您從 Visual Studio 的 「 建立關聯的應用程式與市集建立關聯 動作取得一個.pfx 檔案。
