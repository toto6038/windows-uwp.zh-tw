---
title: 在 Unity Prefabs 和登入 XBL
description: 涵蓋的社交 prefabs 和社會服務在 Xbox Live 的指令碼範例
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660033"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Unity Prefabs 和指令碼式登入

這篇文章會引導您完成加入 Xbox Live 登入您的 Unity 專案。 有兩種方式，如果您已下載，您可以達到登入時[Xbox Live Unity 外掛程式](https://github.com/Microsoft/xbox-live-unity-plugin)。 您可以使用包含在外掛程式內 prefabs 或 Xbox Live 登入到您自己自訂的 Gameobject 編寫指令碼中使用的指令碼和包含的程式庫。

> [!IMPORTANT]
> 這篇文章適用於在 2018 年 （1804年發行版本） 5 中所做的更新之前的外掛程式版本。 如果您安裝 Xbox Live 外掛程式之後，或尚未下載您可能有顯著的差異如何執行登入的較新版本。 此外，您會發現，此外掛程式中的螢幕擷取畫面不相符的最新版本。 請改為參閱[更新登入 prefab 的發行項](playerauthentication-prefab-sign-in.md)，以及[詳述登入指令碼的更新的方法的文件](sign-in-manager.md)。

## <a name="before-you-begin"></a>開始之前

在您開始將 Xbox Live 社會服務新增至您的 Unity 遊戲之前有幾個步驟，您必須在開始討論之前完成。 首先，請確定您已下載，並整合[Xbox Live Unity 外掛程式](https://github.com/Microsoft/xbox-live-unity-plugin)。 第二，您會想要已保留，並透過發佈您項目的[Microsoft 開發中心](https://developer.microsoft.com/en-us/games/uwp)。 讀取[建立新的 Xbox Live 創作者計劃標題](../get-started-with-creators/create-and-test-a-new-creators-title.md)如需有關發行您的標題。
最後，閱讀[設定 Xbox Live 中 Unity](../get-started-with-creators/configure-xbox-live-in-unity.md)正確設定您的 Unity 環境，並設定您要使用 Xbox Live 服務的標題。 一旦您的 Unity 專案已正確設定，就可以開始了解您可以使用 Xbox Live 啟用標題中的工具，以及您可以實作在 Unity 中的 「 Xbox Live 的兩種主要方式： prefabs 和指令碼。

## <a name="prefabs"></a>Prefabs

Unity 有可讓您儲存完整的元件和屬性 GameObject prefab 的資產類型。 Prefab 做為您可以從中建立新的物件執行個體在 Unity 場景中的範本。
[深入了解從 Unity 網站 prefabs](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage)。

Xbox Live Unity 外掛程式提供您可以使用您的專案中使用的幾個 prefabs Xbox Live 的功能。 這篇文章中所述 prefabs 可讓您登入，[新增多使用者支援](../get-started-with-creators/add-multi-user-support.md)給您的標題或顯示的 登入 Xbox Live 好友名單設定檔。 您可以找到這些和其他在 prefabs**專案**依照路徑 索引標籤：**資產 > Xbox Live > Prefabs**。

### <a name="the-userprofile-prefab"></a>UserProfile prefab

第一個也是最重要的社交 prefab **UserProfile** prefab。 **UserProfile** prefab 已允許 Xbox Live 登入所需的所有項目。 這是非常重要，因為您必須登入使用者在使用 Xbox Live 服務之前。 Prefab 包含登入 按鈕和 GameObject 來代表其玩家代號、 Gamerpic，和玩家分數的播放程式中的記錄。

> [!NOTE]
> 若要使用任何其他 Xbox Live 的 prefabs，您必須包含**UserProfile** prefab 或以手動方式叫用的登入 API。

![在 資產與階層的 UserProfile prefab](../images/unity/unity-userprofile-views.png)

如果您展開**UserProfile** prefab 中**專案** 面板或**階層**已加入至場景之後，您會看到**UserProfile** prefab 包含在其內部的兩個 Gameobject。 第一個物件**SignInPanel**其中包含登入 按鈕的體驗。 第二個物件是否**ProfileInfo**一旦他們登入，其中會包含使用者的相關資訊。 **UserProfile** prefab 是您將使用來表示任何 Xbox Live 的使用者登入的本機程式標題的資訊。

### <a name="the-xboxliveuser-prefab"></a>XboxLiveUser prefab

**UserProfile** prefab 會使用其呼叫的程式碼中的第二個社交 prefab **XboxLiveUser**。 此 prefab 的使用不會立即顯現的因為它不需要加入至場景階層，因為它可能只會具現化程式碼中。 **XboxLiveUser**沒有視覺表示，它只會包含屬於 Xbox Live 使用者的詳細資料。 您需要的執行個體**XboxLiveUser**每個執行個體**UserProfile**。 這點很重要的時機[新增多使用者支援](../get-started-with-creators/add-multi-user-support.md)到您的標題。 除了保留使用者的相關資訊，登入之後此 prefab 也是用來登入 Xbox Live 的使用者程式碼的包裝函式。

## <a name="sign-in-with-the-userprofile-prefab"></a>使用 UserProfile prefab 登入

Xbox Live Unity 外掛程式 prefabs 存在，才能讓特定開發工作更輕鬆。 若要讓 Xbox Live 登入您只需要使用 Unity 專案**UserProfile**並**XboxLiveServices** prefabs 連同 Unity **EventSystem**。

第一次拖曳**UserProfile** prefab 場景。 在理想情況下**UserProfile**應置於您專案的初始功能表畫面。

![將使用者設定檔拖曳至階層](../images/unity/drag-userprofile.gif)

除了**UserProfile**您也需要確定的 prefab **XboxLiveServices** prefab 是存在於至少您專案的第一個場景。
**XboxLiveServices** prefab 可讓您切換是否特定 prefabs 會記錄進行偵錯的資訊。 這可用於檢查 prefab 的行為。

![檢查 prefab xboxliveservices](../images/unity/check-for-xboxliveservices.gif)

最後， **UserProfile**也需要**EventSystem**才能正常執行。 這可以加入登入畫面上按一下滑鼠右鍵，然後按一下，即可透過**GameObject--> UI--> EventSystem**。

![新增事件系統](../images/unity/add_event_system.gif)

如果您輸入的播放模式，則服務會自動登入使用者。 在 Unity 中，Xbox Live SDK 會模擬 Xbox Live 服務的呼叫，並將傳送回假的資料，讓您使用。 若要檢視即時資料，您必須建置為 UWP 應用程式專案，並從 Visual Studio 中執行。 請參閱[Unity 中設定 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md)如需詳細資訊。 當您輸入在 Unity 播放模式，prefab 會填入假的資料，它會模擬的玩家代號、 Gamerpic 和玩家分數的播放器等資訊。 這是應該所顯示的資訊**UserProfile** prefab。

成功登入將會看起來如下：![成功 userprofile play](../images/unity/correct-user-profile-play.gif)

如果您尚未設定您的專案，以連線到 Xbox Live 正確輸入 [播放] 模式將會停用的登入按鈕及顯示錯誤訊息。

以下是範例中的失敗登入因為不正確的 Xbox Live 應用程式組態。
![失敗 userprofile play](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>登入指令碼

既然您已經知道如何使用**UserProfile** prefab 它應該先查看基礎指令碼，其可控制 prefab 的功能。 如果您看一下**UserProfile**中**Inspector**您會看到它有**UserProfile.cs**指令碼附加到它。 此指令碼會有您需要讓使用者登入，並載入您想要顯示在登入的設定檔資訊的所有項目。 不過，而不查看整個 prefab （這可能會更新一段時間） 我們要看看幾個範例的程式碼來了解所需的 Xbox Live 使用者登入。

### <a name="the-xboxliveuser-class"></a>XboxLiveUser 類別

登入使用者所需的呼叫會包裝在`XboxLiveUserInfo`類別。 在  **UserProfile.cs**指令碼的執行個體，您會看到`XboxLiveUserInfo`類別，稱為`XboxLiveUser`。 在我們的範例程式碼中，我們將使用相同的變數名稱。 `XboxLiveUserInfo`類別包含的執行個體`XboxLiveUser`類別，稱為`User`做為其成員變數的其中一個。 `XboxLiveUser`類別包含需要登入的登入 」 函式`XboxLiveUser`。 您將使用的執行個體`XboxLiveUser`類別`User`登入使用者並取得描述這些功能的資訊，以及像是他們的玩家代號、 Gamerpic 和玩家分數。 若要這樣做，您必須初始化的執行個體`XboxLiveUserInfo`類別，並使用產生`XboxLiveUserInfo.User`為呼叫登入。

### <a name="initialize-the-xboxliveuser"></a>初始化 XboxLiveUser

初始化 Xbox Live 使用者實際上將它們設為登入 Xbox Live 之前是第一個步驟。 這是很簡單地在程式碼使用`XboxLiveUserInfo.Initialize()`函式。
在我們的範例程式碼，我們使用`XboxLiveUser`成員變數，做為我們`XboxLiveUserInfo`執行個體，並初始化，用來登入。

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

看看此範例程式碼將會看到`XboxLiveUserInfo.Initialize()`函式會呼叫以回應按下按鈕。 完整**UserProfile.cs** prefab 的指令碼具有程式碼，讓自動登入其中`XboxLiveUserInfo.Initialize()`呼叫而不需要互動。
`XboxLiveUserInfo.Initialize()`函式會建立新`XboxLiveUserInfo.User`讓我們呼叫中包含的登入函數`XboxLiveUserInfo.User`類別。

### <a name="call-sign-in"></a>呼叫登入

已初始化 XboxLiveUser 之後，便可為呼叫登入。 在  **UserProfile.cs**登入中稱為`SignInAsync()`UserProfile.cs 函式。 在先前的範例程式碼中我們只需等待`XboxLiveUser`呼叫之前先初始化`SignInAsync()`函式。

> [!NOTE]
> 必須等到`XboxLiveUser`初始化之前呼叫登入，因為`XboxLiveUser`包含`XboxLiveUser.User`屬性用為呼叫登入。

在  **UserProfile.cs** `SignInAsync()`函式包含兩個登入的函式可用來將使用者登入。 `XboxLiveUser.User.SignInSilentlyAsync()` 和`XboxLiveUser.User.SignInAsync()`這些是讓使用者登入的函式。 `SignInAsync()`函式也很清楚如何適當使用這些函式。 下列程式碼範例顯示一個適當的方法，呼叫兩個的登入函式：

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

在此範例中的登入呼叫的結果會儲存在變數`signInStatus`。 這可讓我們來檢查已登入成功，並據此採取行動。 在此範例中的函式會先嘗試登入以無訊息模式，然後如果無訊息登入失敗函式會呼叫一般登函式中。 一旦您有在成功呼叫其中一個登入函式會在登入使用者。 您現在可以使用`XboxLiveUser.User`取得並顯示已登入使用者的相關詳細資料。 看看`LoadProfileInfo()`函式中**UserProfile.cs**如需如何使用的範例`XboxLiveUser.User`顯示登入使用者的相關資訊。

## <a name="build-and-test-sign-in"></a>建置和測試登入

當在編輯器中執行您的標題，您會看到假的資料，當您嘗試使用 Xbox Live 的功能。 若要使用實際的設定檔登入，並測試您的標題中的 Xbox Live 功能，您要建置的 UWP 方案，並在 Visual Studio 中執行它。  您可以建置的 UWP 專案，在 Unity 中遵循下列步驟：

1. 開啟**Build Settings**視窗中的選取**檔案** > **組建設定**。
2. 新增所有您想要包含在組建中的運作原理**組建中的場景**一節。
3. 切換至**通用 Windows 平台**藉由選取**通用 Windows 平台**之下**平台**，然後按一下**切換平台**.
4. 設定**SDK**要**10.0.15063.0**或更新版本。
5. 若要啟用指令碼偵錯核取**UnityC#專案**。
6. 按一下 **建置**並指定專案的位置。

建置完成之後，Unity 會產生新的 UWP 方案檔在 Visual Studio 中執行時，將需要：

1. 在您所指定資料夾中，開啟 **&lt;ProjectName&gt;.sln** Visual Studio 中。
2. 在頂端工具列中，選取**x64**並部署至**本機**。

如果您已啟用**指令碼偵錯**當您從 Unity 建置的 UWP 方案，然後您的指令碼會位於下面**組件 CSharp (通用 Windows)** 專案。

> [!NOTE]
> 之前使用 Visual Studio 組建來測試您的遊戲與實際資料，請遵循[這份檢查清單](test-visual-studio-build.md)以協助確保您的標題都能夠存取 Xbox Live 服務。

## <a name="troubleshooting"></a>疑難排解

如果您有登入 Xbox Live 的問題。 請試著讀取[疑難排解 Xbox Live 登入](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md)。
