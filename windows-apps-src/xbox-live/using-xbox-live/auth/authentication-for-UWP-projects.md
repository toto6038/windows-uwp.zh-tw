---
title: UWP 專案的驗證
author: aablackm
description: 了解如何在通用 Windows 平台 (UWP) 標題中的 Xbox Live 使用者登入。
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 驗證、 登入
ms.localizationpriority: medium
ms.openlocfilehash: 5473b7ede7731d7d07b7e5bfd72857fdb64f1c89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628383"
---
# <a name="authentication-for-uwp-projects"></a>UWP 專案的驗證

若要利用 Xbox Live 功能的遊戲中，使用者必須建立 Xbox Live 設定檔來識別自行在 Xbox Live 社群。  追蹤的 Xbox Live 服務相關的遊戲活動使用該 Xbox Live 設定檔，例如使用者的玩家代號和玩家的圖片，身為使用者的遊戲朋友，什麼遊戲使用者扮演了非常，哪些使用者已解除鎖定，其中使用者代表的成就特定的遊戲，等排行榜。

當使用者想要存取特定的裝置上的特定遊戲中的 Xbox Live 服務時，使用者就必須先驗證。  遊戲可以呼叫受 Xbox Live 的 Api 來起始驗證程序。  在某些情況下，使用者會看到的介面，以提供其他資訊，例如輸入使用者名稱和密碼，若要使用，Microsoft 帳戶的權限同意讓遊戲、 解析帳戶的問題、 接受新規定，等。

一旦通過驗證，直到明確登出 Xbox Live 從 Xbox 應用程式的使用者是該裝置相關聯。  只有一個播放程式可以一次 （適用於所有 Xbox Live 的遊戲）; 非主控台的裝置上進行驗證 對於非主控台的裝置上驗證新的播放器，現有的已驗證播放程式必須先登入。

## <a name="steps-to-sign-in"></a>登入的步驟

概括而言，您可以使用 Xbox Live Api 遵循下列步驟：

1. 建立 XboxLiveUser 物件來代表使用者
2. 登入以無訊息方式 Xbox Live 在啟動
3. 嘗試登入與 UX 如有必要
4. 建立 Xbox Live 內容以互動的使用者
5. 使用 Xbox Live 的內容來存取 Xbox Live 服務
6. 當在遊戲結束 」 或 「 使用者符號外、 發行 XboxLiveUser 物件和 XboxLiveContext 物件設定為 null

### <a name="creating-an-xboxliveuser-object"></a>建立 XboxLiveUser 物件

大部分的 Xbox Live 的活動會與 Xbox Live 使用者。  身為遊戲開發人員，您必須先建立 XboxLiveUser 物件來代表本機使用者。

C++：

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C + + /CX (WinRT):

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C#(WinRT):

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** windows 系統的使用者物件，用來與 xbox live 使用者產生關聯。 如果應用程式是單一使用者 application(SUA)，可能是 nullptr。
  * 如需有關單一使用者 Application(SUA) 和多重使用者 Application(MUA) 的詳細資訊，請參閱[多使用者應用程式簡介](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * 如需有關如何取得 Windows::System::User ^ 從 Windows，請檢查[擷取 UWP 上的 windows 系統使用者](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>登入以無訊息方式 Xbox Live 在啟動 ###

您的遊戲應該來驗證使用者到 Xbox Live 盡早啟動之後，即使之前呈現使用者介面，從 Xbox Live 服務的 預先提取資料時，才會啟動。

若要以無訊息模式驗證本機使用者，呼叫

C++：

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C + + /CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  執行緒發送器用來在執行緒之間的通訊。 無訊息登入 API 不會顯示任何 UI，雖然 XSAPI 仍然需要 UI 執行緒發送器取得 appx 的地區設定的相關資訊。 您可以取得靜態 UI 執行緒發送器，藉由呼叫 Windows::UI::Core::CoreWindow::GetForCurrentThread()]-> [Dispatcher 在 UI 執行緒中。 或者，如果您確定在 UI 執行緒上呼叫這個 API，您可以傳入 nullptr （例如在 JS UWA)。


有 3 個可能的結果，從無訊息登入嘗試

* **Success**

  如果裝置在線上時，這表示成功，驗證到 Xbox Live 使用者和我們能夠取得有效的語彙基元。

  如果裝置已離線，這表示使用者已之前驗證到 Xbox Live 成功，並沒有明確簽署外從這個項目。  請注意在此情況下沒有標題不保證具有存取權有效的語彙基元，它只保證使用者的身分識別已知，且已驗證。    透過 xbox 使用者識別碼 (xuid) 和玩家代號標題已知的使用者身分識別。

* **UserInteractionRequired**

  這表示執行階段無法將使用者登入以無訊息模式。  遊戲應該呼叫`xbox_live_user::sign_in`叫用來顯示必要的 UX 流程，讓使用者註冊/登入 Xbox 身分識別提供者。  常見的問題包括：

  * 使用者沒有 Microsoft 帳戶
  * 使用者未設定慣用的 Microsoft 帳戶，如遊戲
  * 選取的 Microsoft 帳戶沒有 Xbox Live 設定檔
  * 使用者必須接受 Microsoft 帳戶同意

* **其他錯誤**

  執行階段無法登入由其他原因所造成。  通常這些問題不會在遊戲或使用者的可採取動作。 當使用 c + + API，您必須藉由檢查 xbox_live_result <>.err();，請查看錯誤在 WinRT，您將需要攔截 platform:: exception ^。

### <a name="attempt-to-sign-in-with-ux-if-required"></a>嘗試登入與 UX 如有必要 ###

若要以無訊息登入不成功，且您已準備好顯示的使用者介面時才啟用的 UX 的 Xbox Live 使用者驗證您的遊戲。

若要驗證本機使用者與 UX，呼叫

C++：

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C + + /CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  執行緒發送器用來在執行緒之間的通訊。 登入 API 需要 UI 發送器，以便它可以顯示登入 UI，並取得您的 appx 地區設定的相關資訊。 您可以取得靜態 UI 執行緒發送器，藉由呼叫 Windows::UI::Core::CoreWindow::GetForCurrentThread()]-> [Dispatcher 在 UI 執行緒中。 或者，如果您確定在 UI 執行緒上呼叫這個 API，您可以傳入 nullptr （例如在 JS UWA)。

有 3 個可能的結果與 UX 登入嘗試：

* **Success**

  如果裝置在線上時，這表示成功，驗證到 Xbox Live 使用者和我們能夠取得有效的語彙基元。

  如果裝置已離線，這表示使用者已之前驗證到 Xbox Live 成功，並沒有明確簽署外從這個項目。  請注意在此情況下沒有標題不保證具有存取權有效的語彙基元，它只保證使用者的身分識別已知，且已驗證。    使用者的身分識別已知的標題 xbox 使用者識別碼 (xuid) 和玩家代號。

* **UserCancel**

  這表示使用者已取消登入作業之前完成。  當發生這種情況時，遊戲應該自動重試登入與 ux。  相反地，它應該會在遊戲 UX，可讓使用者重試登入作業。  （例如，登入按鈕）

* **其他錯誤**

  執行階段無法登入由其他原因所造成。  通常這些問題不會在遊戲或使用者的可採取動作。 當使用 c + + API，您必須藉由檢查 xbox_live_result <>.err();，請查看錯誤在 WinRT，您將需要攔截 platform:: exception ^。

## <a name="sign-in-code-examples"></a>登入程式碼範例

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>登出

### <a name="handling-user-sign-out-completed-event"></a>處理使用者登出完成的事件

如果發生下列其中一種情況，將標題從登出使用者：

1. 使用者已簽署外從 Xbox 應用程式 (Windows 10) 或主控台命令介面 (Xbox One)。 登出後，將會影響所有啟用的 Xbox Live 應用程式針對此使用者安裝。
2. 切換到不同的 Microsoft 帳戶使用者
3. 使用者登入相同的項目，從不同的裝置

在這些情況下，標題會收到的事件`xbox_live_user::add_sign_out_completed_handler`或`XboxLiveUser::SignOutCompleted`處理常式。  遊戲必須適當地處理已完成的事件登出：

1. 遊戲應該顯示給使用者，上表有簽署外從 Xbox Live 的清除視覺指示。
2. 遊戲無法呼叫任何 Xbox Live 服務 Api 中的事件處理常式，因為使用者有已簽署外，而且不沒有可用的任何授權權杖。

## <a name="sign-out-handler-code-samples"></a>登出 處理常式程式碼範例

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>判斷裝置是否為離線

登入 Api 仍會成功時離線如果一次，使用者登入，而且不會傳回最後一個登入帳戶。  

如果沒有任何使用者已登入之前，您也可以離線登入不會達成。

如果離線播放標題 （行銷活動等模式，） 標題可以允許使用者玩而且透過 WriteInGameEvent API 和連接的儲存體 API 的遊戲進度記錄，這兩者的正常運作時裝置已離線。

如果標題不能播放離線標題 （多人連線遊戲或伺服器為基礎的遊戲等） 應該呼叫 GetNetworkConnectivityLevel API 來了解，如果裝置處於離線狀態，並通知使用者有關的狀態和可能的解決方案 (例如，' 您必須連線至網際網路以繼續...')。

## <a name="online-status-code-samples"></a>線上狀態的程式碼範例

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```