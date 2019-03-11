---
title: 設定 Xbox Live 中 Unity
description: 了解如何使用 Xbox Live Unity 外掛程式來設定您的 Unity 遊戲中的 Xbox Live。
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，Unity 設定
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596723"
---
# <a name="configure-xbox-live-in-unity"></a>設定 Xbox Live 中 Unity

> [!NOTE]
> Xbox Live Unity 外掛程式只建議用於[Xbox Live 創作者計劃](../developer-program-overview.md)成員，因為目前沒有成就或多人遊戲支援。

具有[Xbox Live Unity 外掛程式](https://github.com/Microsoft/xbox-live-unity-plugin)新增 Unity 遊戲至 Xbox Live 的支援很簡單，讓您擁有更多時間專注於使用 Xbox Live 方式最符合您的標題。

本主題會瀏覽設定的 Xbox Live 的外掛程式在 Unity 中的程序。

## <a name="prerequisites"></a>必要條件

您可以使用 Xbox Live Unity 中，您需要下列：

1.  **[Xbox Live 帳戶](https://support.xbox.com/browse/my-account/manage-account/Create%20account)**。
1. 中的註冊**[合作夥伴中心開發人員計劃](https://developer.microsoft.com/store/register)**。
2. **[Windows 10 年度更新版](https://microsoft.com/windows)** 或更新版本
3. **[Unity](https://store.unity.com/)** 版本**5.5.4p5** （或更新版本） **2017.1p5** （或更新版本） 或**2017.2.0f3** （或更新版本） 與**[Microsoft Visual Studio Tools for Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** 並**指令碼後端的 Windows 市集.NET**。
4. **[Visual Studio 2015](https://www.visualstudio.com/)** 或是**[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** （或更新版本） 與**通用 Windows App 開發工具**。
5. **[Xbox Live 平台擴充功能 SDK](https://aka.ms/xblextsdk)**。


> [!NOTE]
> 如果您想要使用 Xbox Live IL2CPP 指令碼的後端，您將需要 Unity 2017.2.0p2 或更新版本和 Xbox Live Unity 外掛程式版本 「 1802年預覽發行 」 或更高版本。


## <a name="import-the-unity-plugin"></a>匯入 Unity 外掛程式

若要匯入新的或現有 Unity 專案的外掛程式，請遵循下列步驟：

1. 在瀏覽至 [Xbox Live Unity 外掛程式版本] 索引標籤[ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases)。
2. 下載**XboxLive.unitypackage**。
3. 在 Unity 中，按一下**資產** > **匯入封裝** > **自訂封裝**並瀏覽至**XboxLive.unitypackage**.

![成功匯入](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>（選擇性）設定外掛程式以處理在 Unity Editor （.NET 4.6 或 IL2CPP 只）

> [!NOTE]
> 支援變更指令碼的執行階段版本在 Unity 中需要的 Xbox Live Unity 外掛程式版本 「 1711年版本 」 或更高版本的.NET 4.6 及版本 「 1802年預覽發行 」 或更高的 IL2CPP。

有三個可以設定 Unity 以定義如何編譯您的程式碼中的設定：

1. **指令碼後端**是編譯器所使用。 Unity 針對通用 Windows 平台支援兩個不同的指令碼後端：.NET 和 IL2CPP。
2. **指令碼的執行階段版本**執行 Unity Editor 的指令碼執行階段版本。
3. **API 相容性層級**是您將建置您的遊戲針對 API 介面。

下表顯示目前的指令碼的支援矩陣 Xbox Live Unity 外掛程式：

| 指令碼的後端     | 指令碼的執行階段版本 | 支援     | 所需的最小的 Unity 版本 |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | .NET 3.5 對等       | 否            | 無                            |
| Il2CPP                | .NET 4.6 對等       | 是           | 2017.2.0p2                     |
| .NET                  | .NET 3.5 對等       | 是           | 相同的必要條件          |
| .NET                  | .NET 4.6 對等       | 是           | 相同的必要條件          |

我們已加入 Xbox Live Unity 外掛程式，從版本"1711年版本 」 的其他指令碼的執行階段支援。 根據預設，外掛程式會設定為在 Unity 編輯器中執行使用.NET 後端的指令碼和指令碼的.NET 3.5 執行階段版本。 如果您的專案使用.NET 4.6 的指令碼執行階段版本，您必須設定外掛程式以在編輯器中正常運作：

1. 在 Unity 專案總管 中，瀏覽至**Xbox Live\Libs\UnityEditor\NET46**並選取所有 dll 檔的資料夾中。
2. 在 [偵測器] 視窗中，檢查**編輯器**下方**包含平台**。
3. 在 Unity 專案總管 中，瀏覽至**Xbox Live\Libs\UnityEditor\NET35**並選取所有 dll 檔的資料夾中。
4. 在 [偵測器] 視窗中，取消核取**編輯器**下方**包含平台**。

![將指令碼的執行階段變更](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> 如果您在專案中變更的指令碼的執行階段版本回到 3.5 反轉需要這些步驟。

## <a name="set-visual-studio-as-the-ide-in-unity"></a>將 Visual Studio 設定為 Unity 中的 IDE

Visual Studio，才能建置[通用 Windows 平台 (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp)遊戲。 您也可以移至 Visual studio 的 Unity 中設定您的 IDE**編輯** > **喜好設定** > **外部工具**並設定**外部指令碼編輯器**Visual studio。

![設定與外部工具](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Unity 外掛程式檔案結構

Unity 外掛程式的檔案結構會分成下列部分：

* __Xbox Live__包含實際的外掛程式資產會包含在已發佈 **.unitypackage**。
    * __編輯器__包含指令碼，提供基本的 Unity 組態 UI，並在建置期間處理專案。
    * __範例__包含一組簡單的場景檔案示範如何使用各種 prefabs，並將它們連接在一起。
    * __映像__是一組小型 prefabs 所使用的映像。
    * __程式庫__儲存的 Xbox Live 的程式庫。
    * __Prefabs__包含各種[Unity prefab](https://docs.unity3d.com/Manual/Prefabs.html)實作 Xbox Live 功能的物件。
    * __指令碼__包含從 prefabs 呼叫 Xbox Live Api 的所有程式碼檔案。 這是非常好的起點位置尋找有關如何適當地呼叫 Xbox Live Api 的範例。
    * __Tools\AssociationWizard__包含 Xbox Live 關聯精靈，用來從應用程式設定為提取[合作夥伴中心](https://developer.microsoft.com/windows)Unity 內使用。

## <a name="enable-xbox-live"></a>啟用 Xbox Live

如您與 Xbox Live 互動的標題，您必須設定的 Xbox Live 的初始組態。 您可以輕鬆地在 Unity 內使用 Xbox Live 關聯精靈：

1. 在  **Xbox Live**功能表上，選取**組態**。
2. 在  **Xbox Live**視窗中，選取**執行 Xbox Live 關聯精靈**。
3. 在 **將您的標題與 Windows 市集產生關聯** 對話方塊中，按一下**下一步**，然後使用您的合作夥伴中心帳戶登入。
4. 選取您想要將此專案中，與建立關聯，然後按一下 應用程式**選取**。 如果您沒有看到它那里，請按一下**重新整理**。 或者，您可以建立新的應用程式保留名稱，然後按一下**保留**。
5. 系統會提示您啟用 Xbox Live 如果您尚未這麼做。 按一下 **啟用**標題中啟用 Xbox Live。
6. 按一下 **完成**來儲存您的組態。

若要呼叫 Xbox Live 服務，您的桌面必須是開發人員模式及設為相同的沙箱，因為您的標題中的 Xbox Live 的組態。 您可以確認兩者來看看**Xbox Live 設定**Unity 中的視窗：

1. **開發人員模式組態**應該**已啟用**。 如果那**已停用**，按一下**切換至 開發人員模式**。
2. **標題 Configuration** > **沙箱**應該具有相同的識別碼**開發人員模式組態** > **開發人員模式**。

![啟用 XBL](../images/unity/unity-xbl-enabled.png)

請參閱[Xbox Live 的沙箱](../xbox-live-sandboxes.md)沙箱的相關資訊。

## <a name="build-and-test-the-project"></a>建置和測試專案

當在編輯器中執行您的標題，您會看到假的資料，當您嘗試使用 Xbox Live 的功能。 比方說，如果您[新增登入功能](unity-prefabs-and-sign-in.md)至您的場景和嘗試登入，您會看到**假使用者**會顯示為您的設定檔名稱，以預留位置的圖示。 若要使用實際的設定檔登入，並測試您的標題中的 Xbox Live 功能，您要建置的 UWP 方案，並在 Visual Studio 中執行它。  您可以建置的 UWP 專案，在 Unity 中遵循下列步驟：

1. 開啟**Build Settings**視窗中的選取**檔案** > **組建設定**。
2. 新增所有您想要包含在組建中的運作原理**組建中的場景**一節。
3. 切換至**通用 Windows 平台**藉由選取**通用 Windows 平台**之下**平台**，然後按一下**切換平台**.
4. 設定**SDK**要**10.0.15063.0**或更新版本。
5. 若要啟用指令碼偵錯核取**UnityC#專案**。
6. 按一下 **建置**並指定專案的位置。

![組建設定](../images/unity/build_settings.JPG)

建置完成之後，Unity 會產生新的 UWP 方案檔在 Visual Studio 中執行時，將需要：

1. 在您所指定資料夾中，開啟 **&lt;ProjectName&gt;.sln** Visual Studio 中。
2. 在頂端工具列中，選取**x64**並部署至**本機**。

如果您已啟用**指令碼偵錯**當您從 Unity 建置的 UWP 方案，然後您的指令碼會位於下面**組件 CSharp (通用 Windows)** 專案。

![假的使用者：123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> 之前使用 Visual Studio 組建來測試您的遊戲與實際資料，請遵循[這份檢查清單](test-visual-studio-build.md)以協助確保您的標題都能夠存取 Xbox Live 服務。

> [!IMPORTANT]
> 可能的 2018 現在需要對 package.appxmanifest.xml 檔案進行更新，才能正確地在 Visual Studio 測試 UWP 標題。 請這樣做：
>
> 1. 搜尋方案總管，package.appxmanifest.xml 檔案
> 2. 以滑鼠右鍵按一下檔案，然後選擇 檢視程式碼。  
    如果無法使用 [檢視程式碼] 選項，或 package.appxmanifest 檔案並沒有延伸模組。 您必須開啟做為 xml 檔案，並繼續進行其餘步驟。
> 3. 底下`<Properties></Properties>`區段中，新增下面這一行： `<uap:SupportedUsers>multiple</uap:SupportedUsers>`。
> 4. 部署至您的 Xbox 的遊戲，藉由從 Visual Studio 中啟動遠端偵錯組建。 您可以找到指示來設定您在 Xbox 上的標題[設定您在 Xbox 開發環境上的 UWP](../../xbox-apps/development-environment-setup.md)文章。
>
> 組態已變更的部分可能看起來像它讓多人，但仍然必須在單一播放器案例中執行您的遊戲。

## <a name="try-out-the-examples"></a>嘗試的範例

您全都準備好要開始使用 Unity 專案中的 Xbox Live ！ 嘗試開啟中的場景**Xbox Live/Examples**資料夾，以查看此外掛程式的動作，以及如何使用您自己的功能的範例。 在編輯器中執行的範例可讓您假的資料，但如果您要建置 Visual Studio 中的專案和[您的 Xbox Live 帳戶相關聯沙箱](authorize-xbox-live-accounts.md)，您可以使用您的玩家代號登入。

請嘗試**SignInAndProfile**登入您的 Microsoft 帳戶的場景**排行榜**場景建立排行榜，和**UpdateStat**場景顯示和更新統計資料。

## <a name="see-also"></a>請參閱

* [在 Unity 中的登入 Xbox Live](unity-prefabs-and-sign-in.md)
* [授權 Xbox Live 帳戶](authorize-xbox-live-accounts.md)
