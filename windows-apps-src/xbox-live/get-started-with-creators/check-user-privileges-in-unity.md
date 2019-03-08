---
title: 檢查 Unity 中的使用者權限設定
description: 了解如何檢查已簽署的權限設定中的 Xbox Live 帳戶。
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 帳戶、 測試帳戶、 家長監護、 使用者權限、 強制禁令、 追加銷售
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620403"
---
# <a name="check-user-privilege-settings-in-unity"></a>檢查 Unity 中的使用者權限設定
在 Xbox Live，每個已驗證的使用者帳戶相關聯的權限。 權限控制哪些功能的 Xbox Live 使用者可以存取在指定的時間點的時間。 這些權限。 有些系統控制功能，而其他人可能會與特定遊戲或延伸模組的訂用帳戶相關聯。 此外，家長監護和 ban 公布的 Xbox Live 強制執行小組可以限制使用者權限。 這些權限涵蓋的常見案例，包括多人遊戲，存取的使用者，產生的內容，聊天，或到串流影片的數字。 遊戲使用這項資訊來決定存取控制及個人化。

## <a name="prerequisites"></a>必要條件
若要判斷使用者權限設定，您必須有使用 Xbox Live 設定您的遊戲進行驗證和成功登入的使用者。

>[!IMPORTANT]
> 如果您要測試您的遊戲在 Unity 編輯器中，您的遊戲未連線到的 Xbox Live 服務，並使用假的資料以模擬連線。 這會導致 null 值，當您檢查有權限。 若要測試的實際資料，執行您的 Unity 遊戲的通用 Windows 平台組建，並在 Visual Studio 中開啟產生的專案檔。

下列文章說明您可以採取的步驟：

* [登入 Xbox Live 中 Unity （建置和測試登入）](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [在 Visual Studio 中測試您的 Unity 遊戲組建](test-visual-studio-build.md)

## <a name="determine-privileges"></a>判斷權限
使用者的權限包含在驗證時所收到的權杖。 在 Unity 中，您可以存取使用者的權限的清單`XboxLiveUser`類別之後使用者已成功登入。 權限會儲存成單一字串，以空格分隔。 例如，您可能會看到具有下列權限的使用者：

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

如果您想要尋找特定的權限，您可以檢查看`Privileges`屬性包含相關聯的程式碼：

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>權限代碼
以下是可能的權限代碼可能會傳回的清單。

| 程式碼  | 權限  | 描述   |
|------ |-----------------------------  |-------------------    |
| 190   | 廣播             | 可以廣播即時遊戲。     |
| 197   | view_friends_list     | 可以檢視其他使用者的好友名單。   |
| 198   | game_dvr              | 可以上傳記錄至雲端的遊戲中影片。      |
| 199   | share_kinect_content          | Kinect 可以上傳至雲端的使用者和所做的任何人都可以存取記錄的內容。 |
| 203   | multiplayer_parties           | 可以加入合作對象工作階段。     |
| 205   | communication_voice_ingame    | 可以參與語音聊天合作對象和多人連線遊戲工作階段期間。    |
| 206   | communication_voice_skype     | 可以使用 Skype Xbox One 上使用語音通訊。   |
| 207   | cloud_gaming_manage_session   | 可以配置，並裝載遊戲工作階段管理的雲端計算叢集。    |
| 208   | cloud_gaming_join_session     | 可以加入雲端計算工作階段。     |
| 209   | cloud_saved_games     | 可以在雲端標題儲存體儲存的遊戲。    |
| 211   | share_content     | 可以與其他人共用內容。    |
| 214   | premium_content   | 購買、 下載並啟動適用於 Xbox Live 金會員的訂用帳戶的優質內容。     |
| 219   | subscription_content  | 可以購買並下載 premium 訂用帳戶的內容和使用 premium 訂用帳戶功能。     |
| 220   | social_network_sharing    | 可以共用社交網路上的進度資訊。    |
| 224   | premium_video     | 可以存取進階視訊服務。    |
| 235   | purchase_content  | 可購買的內容。     |
| 247   | user_created_content  | 可以下載並檢視線上內容所建立的使用者。    |
| 249   | profile_viewing   | 可以檢視其他使用者的設定檔。   |
| 252   | 通訊    | 可以使用非同步傳訊與任何人的文字。    |
| 254   | multiplayer_sessions  | 可以加入多人連線的工作階段，遊戲。   |
| 255   | add_friend    | 可以遵循其他 Xbox Live 的使用者，並加入 Xbox Live 好友。   |
