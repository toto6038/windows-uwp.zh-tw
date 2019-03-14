---
title: Xbox Live 程式碼移植從 XDK 到 UWP
description: 了解如何在 Xbox Live 程式碼移植從 Xbox Development Kit (XDK) 平台通用 Windows 平台 (UWP)。
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 xdk，移植
ms.localizationpriority: medium
ms.openlocfilehash: c6e8a6ebe716f1e062940066184e9f734441371b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590813"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Xbox Live 將程式碼移植 Xbox Developer Kit (XDK) 從通用 Windows 平台 (UWP)

## <a name="introduction"></a>簡介

本文旨在協助開發人員已使用 Xbox One XDK，若要開始將其 Xbox Live 的程式碼移轉到 Windows 10 通用 Windows 平台 (UWP)。

此移轉一部分包括從 XSAPI 1.0 (Xbox Live 服務 API，包含在 2015 年 8 月 Xbox One XDK) 切換 XSAPI 2.0 （包含在從 2015 年 11 月起，Xbox One XDK 或也可以在 Xbox Live SDK 取得。 這些 Api 的功能幾乎完全相同，但有一些重要的實作差異。

這篇文章所涵蓋的其他主題包括準備您的 Windows 開發電腦和安裝的其他 Api 通常需要時使用 Xbox Live 的服務，例如安全通訊端 API，以及連接的儲存體 API 來管理雲端備份將儲存的遊戲。

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-partner-center-and-xdp"></a>安裝及設定您的專案在合作夥伴中心和 XDP

使用 Xbox Live 的 UWP 標題服務必須在設定[合作夥伴中心](https://partner.microsoft.com/dashboard)。 如需最新資訊，請參閱[加入新的或現有的 UWP 專案 Xbox Live](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) Xbox Live 程式設計指南 》 中隨附[Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)。

在該頁面上的主題包括使用 Xbox Live 服務在您的標題中的步驟執行：

-   在合作夥伴中心建立 UWP 應用程式專案。

-   若要設定您的 Xbox Live 的使用方式的專案使用 XDP。

-   連結您的合作夥伴中心產品 XDP 產品。

-   在 XDP （必要時您的沙箱中執行您的 Xbox Live 標題） 的開發人員帳戶。

如果您的標題支援多人連線遊戲時，可能需要一些額外的設定，多人連線的工作階段範本中。 所有 Windows 10 項目使用 Xbox Live 多人遊戲，並寫入 MPSD （多人連線的工作階段文件） 都需要清單中的 「 功能 」 在您的工作階段範本中找到這個新欄位： ```userAuthorizationStyle: true```。

### <a name="enabling-cross-play"></a>啟用跨 play

如果您將會支援 「 跨-播放 」 （共用 Xbox Live 設定之間 Xbox One 和 PC 遊戲，允許跨裝置的多人遊戲），您也必須將這項功能新增至您的工作階段範本： **crossPlay: true**。

如需在 XDP 支援跨-婧矔菛 及其組態需求的詳細資訊，請參閱"內嵌 XDK 及 UWP 跨 Play 標題中 XDP"Xbox Live 程式設計指南中。

此外，某些程式設計考量，請參閱稍後的章節[支援 Xbox One 和 PC 之間的多人遊戲跨 play](#_Supporting_multiplayer_cross-play)。

## <a name="setting-up-your-windows-development-environment"></a>設定您的 Windows 開發環境

1.  [下載最新**Xbox Live SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)並在本機解壓縮。

2.  [安裝**Xbox Live 平台擴充功能 SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)如果您需要適用於 UWP 的安全通訊端 API 及/或遊戲儲存 API （也稱為連接儲存體）。

3.  為您在 Visual Studio 中的通用 Windows 應用程式專案加入 Xbox Live 的支援。 您可以加入完整來源，或藉由安裝到您的 Visual Studio 專案的 NuGet 套件參考二進位檔。 封裝可供 c + + 和 WinRT。 如需詳細資訊，請參閱[新增新的或現有的 UWP 專案 Xbox Live](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  設定開發電腦以使用您的沙箱。 Xbox Live SDK，您可以從系統管理員命令提示字元使用的 Tools 目錄中沒有命令列指令碼 (例如：SwitchSandbox.cmd XDKS.1)。

  **請注意**若要切換回零售沙箱，您可以刪除登錄機碼，以修改指令碼，或您可以切換到呼叫零售的沙箱。

1.  開發人員將帳戶新增至您的開發電腦。 XDP 中建立的開發人員帳戶，才能在您指派的沙箱中開發或執行範例時，與在執行階段的 Xbox Live 服務互動。 若要加入 Windows 中的一個或多個帳戶：

    1.  開啟**設定**(快速鍵：Windows 鍵 + I）。

    2.  開啟**帳戶**。

    3.  在  **Your Account**索引標籤上，按一下**新增 Microsoft 帳戶**。

    4.  輸入開發人員帳戶的電子郵件和密碼。

### <a name="appxmanifest-changes"></a>AppxManifest 變更

Appxmanifest.xml 檔案 Xbox 和 UWP 版本之間的最常見的變更如下：

1. 即使在開發期間，UWP 中, 很重要的套件識別。 在身分識別名稱 」 和 「 發行者*必須符合*的 UWP 應用程式定義在合作夥伴中心。

1. 需要的套件相依性區段。 例如：

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  如有特定需求的應用程式資訊清單中的其他章節，請參閱範例 UWP 應用程式資訊清單 （例如，一個 Xbox Live SDK 隨附的 UWP 範例的 Visual Studio 中建立的預設值的通用 Windows 應用程式專案）UWP，例如```<VisualElements>```。

1.  標題和 SCID xboxservices.config 檔案中定義 (請參閱[下一節](#_Define_your_title)) 而不是"xbox.live 」 延伸模組類別目錄中。

1.  不需要 「 xbox.system.resources 」 延伸模組類別目錄。

1.  Networkmanifest.xml 檔案中所定義的安全通訊端 (請參閱[安全通訊端](#_Secure_sockets)) 而不是"windows.xbox.networking 」 類別目錄中。

1.  必須定義 「 windows.protocol 」 延伸模組類別目錄，才能接收 UWP 標題中的 Xbox Live 的邀請 (請參閱[傳送和接收邀請](#_Sending_and_receiving))。

1.  如果您使用 GameChat API 時，您要新增 麥克風的裝置功能，內部```<Capabilities>```項目。 例如：

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>Xbox Live SDK 的組態檔中定義您的標題和 SCID

Xbox Live SDK，就必須知道您識別碼和 SCID，不會再納入 UWP 項目的 appxmanifest.xml 的標題。 相反地，您會建立名為文字檔**xboxservices.config**在您的專案根目錄，並加入下列欄位，這些值替換為您的標題資訊：

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Xboxservices.config 內的所有值都都區分大小寫的。

包含此組態檔做為您的專案中的內容，以便可在組建輸出。

**請注意**這些值會使用下列 API 以程式設計方式在您的標題：

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>API 命名空間對應

表 1. XDK 從命名空間對應至 UWP。

<table>
  <tr>
    <td></td>
    <td><b>Xbox One XDK</b></td><td><b>UWP</b></td>
    <td><b>API 是適用於...</b></td>
  </tr>
  <tr>
    <td>Xbox 服務 API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services (<i>沒有變更</i>)</td>
    <td>Xbox Live SDK （使用 NuGet 二進位或來源）</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*沒有變更*) Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK （使用 NuGet 二進位） </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Xbox Live 平台擴充功能 SDK</td>
  </tr>
  <tr>
    <td>連接的儲存空間</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Xbox Live 平台擴充功能 SDK</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>多人遊戲的訂用帳戶和事件處理

從 XSAPI 1.0 的重大變更為 XSAPI 2.0，最多人遊戲的標題將會遇到的其中一個是移動的幾個方法和事件**RealTimeActivityService**到**MultiplayerService**.

例如：

-   **EnableMultiplayerSubscriptions()\*** 方法

-   **DisableMultiplayerSubscriptions()** 方法

-   **MultiplayerSessionChanged**事件

-   **MultiplayerSubscriptionLost**事件

-   **MultiplayerSubscriptionsEnabled**屬性

**重要實作附註**即使您可能不會明確地使用任何其他項目**RealTimeActivityService**之後將這些事件和方法移至**MultiplayerService**，您仍然必須呼叫**xblContext-&gt;RealTimeActivityService-&gt;Activate()** 再呼叫**EnableMultiplayerSubscriptions()** 因為多人遊戲的訂用帳戶需要 RTA 服務。

## <a name="whats-handled-differently-in-uwp"></a>什麼是 UWP 中以不同方式處理

以下是非常高層級可能會有差異，XDK 及 UWP 的程式碼區段的清單，在新發現[NetRumble 範例](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)（包括 XDK 和 UWP 版本）：

-   存取標題的識別碼和 SCID 資訊

-   （新增適用於 UWP） 的預先啟動啟用

-   暫止/繼續 PLM 處理

-   （新增適用於 UWP） 的延伸的執行

-   Xbox**使用者**物件和使用者處理差異

    -   登入和登出的處理

    -   控制器配對 （僅限處理在 Xbox 上）

    -   遊戲台處理

-   檢查多人的權限

-   支援的 Xbox One 和 PC 之間的多人遊戲跨 play

-   傳送遊戲的邀請

    -   能夠開啟合作對象應用程式，從遊戲中-在 UWP 上的 n/a

    -   列舉的遊戲中-在 UWP 上的 n/a 的合作對象成員能力

-   顯示玩家的設定檔

-   安全通訊端 API 介面也會變更

-   QoS 測量啟始和結果處理

-   撰寫的遊戲事件

-   GameChat： 事件、 設定和 ChatUser 物件

-   連接儲存體 API 介面也會變更

-   PIX 事件 （只在 Xbox; 上未涵蓋在本白皮書）

-   部分呈現差異

下列各節會進入上許多這些差異的進一步詳細資料。

### <a name="accessing-title-id-and-scid-info"></a>存取標題的識別碼和 SCID 資訊

在 UWP 中您標題識別碼] 及 [服務組態識別碼會透過 AppConfig 屬性的執行個體上存取**XboxLiveContext**。

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**附註**在 XDK，您可以取得這些識別碼使用這些新的屬性或舊的靜態屬性，在**Windows::Xbox::Services::XboxLiveConfiguration**。

### <a name="prelaunch-activation"></a>預先啟動啟用

當使用者登入時，可能會預先啟動常用的 Windows 10 中的項目。 若要處理這種情況，您的標題應該檢查的啟動引數的程式碼**PreLaunchActivated**。 比方說，您可能不想要在這種啟動期間載入所有資源。 如需詳細資訊，請參閱 MSDN 文章[控制代碼的預先啟動應用程式](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx)。

### <a name="suspendresume-plm-handling"></a>暫止/繼續 PLM 處理

暫停和繼續與 PLM 一般而言，運作方式相似的方式在 Xbox One; 上的通用 Windows 應用程式中不過，有幾個重要的差異，要牢記在心：

-   沒有任何**Constrained**狀態的電腦上 — 這是一個 Xbox One 專有的概念。

-   標題降到最低; 時，會立即暫停開始解決這個問題的方式，請參閱下節[擴充執行](#_Extended_execution)。

-   現在參加正是不同： 您有 5 秒，而不是 1 秒，在主控台上的電腦上暫止。

如果您使用已連接儲存體的另一個重要考量是新**ContainersChangedSinceLastSync** UWP 版本，此 api 中的屬性。 處理時繼續事件，您可以檢查這個屬性，以查看任何容器是否已變更在雲端時暫止您的標題。 如果播放程式在一部電腦上暫停遊戲，播放其他位置，則傳回第一個電腦，也可能會發生。 如果有暫止之前，必須從這些容器讀取資料到記憶體中，您可能會想要讀取一次，以查看變更並據以處理所做的變更。

如需在 Windows 10 上的 UWP 應用程式中處理 PLM 的詳細資訊，請參閱 MSDN 文章[Launching，resuming，和背景工作](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx)。

您也可能會發現[Xbox one PLM](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) GDN 很有用，因為其已寫入的遊戲納入考量，而且大部分處理應用程式生命週期的概念的仍然適用的電腦上的白皮書。

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>延伸的執行

最小化的 UWP 應用程式的電腦上通常會導致它立即開始暫止。 藉由使用擴充的執行，您可以延遲此程序。 範例實作：

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

在後**ExtensionRevokedHandler**已呼叫，新的延伸模組需要針對未來可能暫止要求。 **ExtensionRevokedHandler**系統中沒有記憶體不足的壓力，過了 10 分鐘的時間，或使用者在遊戲為最小化時，要切換回遊戲時呼叫。 因此**RequestExtension()** 可能應該呼叫在這些時間：

-   在啟動。

-   在  **ExtensionRevokedHandler**時引數-&gt;原因 = = Resumed （索引標籤式時 10 分鐘計時器到期之前，已最小化遊戲的使用者）。

-   在  **OnResuming**處理常式 （如果標題由於記憶體不足的壓力或 10 分鐘的計時器已暫止）。

### <a name="handling-users-and-controllers"></a>處理使用者和控制站

在 Windows 中，您使用一個登入的使用者一次。 在 Xbox Live SDK 中，您先建立**XboxLiveUser**物件，他們登入 Xbox Live，然後建立**XboxLiveContext**於此使用者的物件。

之前，在 Xbox One XDK 上：

1.  取得使用者 （從遊戲台互動）。
2.  建立**XboxLiveContext**來自該使用者：
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  處理**SignOut**事件：
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  處理遊戲台/控制器配對使用：
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

現在，UWP/xbox Live SDK:

1.  建立**XboxLiveUser**:

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  請嘗試將他們使用它們也用 ui 唆的最後一個 Microsoft 帳戶登入：

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  如果您收到**SignInResult::Success**此非同步作業的結果，在建立**XboxLiveContext**，然後您可以完成：

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  如果您就會取得**SignInResult::UserInteractionRequired**，您要呼叫的互動式登入方法，會顯示系統 UI:

  ```
  xblUser->SignInAsync();
  ```

1.  從這裡，您可能會得到**SignInResult::UserCancel**，在此情況下您不需要登入的使用者，而且您應該考慮提供讓他們再試一次登入的功能表選項。

  **請注意**時提供的功能表選項，它是個不錯的主意，以便切換至不同的 Microsoft 帳戶的選項：

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  登入的使用者之後，您可能想要將連結**XboxLiveUser::SignOutCompleted**事件，讓您可以回應登出使用者：

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  沒有任何配對，以處理 Windows 10 中的控制器。

這是簡化的範例 c + + / WinRT。 如需更詳細的範例，請參閱 「 Xbox 即時驗證 Windows 10 中 「 Xbox Live 程式設計指南中。 您也可能會在"新增至新的 UWP 專案的 Xbox Live"中找到更廣泛的範例很有幫助。

### <a name="checking-multiplayer-privileges"></a>檢查多人的權限

相當於**CheckPrivilegeAsync()** 尚無法在 Xbox Live SDK 中使用。 現在，您必須搜尋所傳回的字串清單中所需的權限**權限**屬性**XboxLiveUser**。 例如，若要檢查多人的權限，尋找權限 」 254。 」 使用 XDK 文件，您可以找到一份所有 Xbox Live 中的權限**Windows::Xbox::ApplicationModel::Store::KnownPrivileges**列舉型別。

如需本主題的討論，請參閱論壇文章[xsapi 和使用者的權限](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html)。

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>支援的 Xbox One 和 PC UWP 之間的多人遊戲跨 play

除了新的工作階段範本需求中 XDP (請參閱[安裝及設定您的專案在合作夥伴中心和 XDP](#_Setting_up_and))，跨 play 隨附新的限制，工作階段聯結功能。 您無法再使用 [無] 當做工作階段加入限制。 您必須使用 「 追蹤 」 或 「 本機 」 （預設限制會是 「 區域 」）。

此外，聯結和讀取的限制預設為 「 本機 」 因為所需**userAuthorizationStyle** Windows 10 的多人的功能。

此論壇文章中，[是否可以建立公用的多人遊戲工作階段](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html)，包含額外的深入解析。

在更新多人遊戲的開發人員的流程圖，跨 play 啟用多人遊戲範例 NetRumble，或從您開發人員帳戶管理員 （補西牆），可以找到進一步的資訊和範例。

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>傳送和接收邀請

若要啟動 傳送邀請的使用者介面 API 現**Microsoft:: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()**。 您傳入工作階段-&gt; **SessionReference**從您活動的工作階段 （通常您大廳） 的物件。 您可以選擇性地傳入，參考自訂邀請 XDP 中的服務組態中已定義的字串識別碼的第二個參數。 您那里定義的字串會出現在傳送到受邀的播放程式的快顯通知。 請注意，您要傳遞中這個方法的參數是識別碼，以及它的服務必須正確格式化。 例如，字串識別碼為"1"必須傳遞為"/ / 1 」。

如果您想要使用的多人遊戲服務來直接傳送邀請 （亦即，而不顯示任何 UI），您仍然可以使用其他邀請方法**Microsoft:: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** 從使用者的**XboxLiveContext**。

若要允許邀請傳送至 Windows 通訊協定啟用您的標題，您需要新增這個擴充功能**&lt;應用程式&gt;** 中，appxmanifest 項目：

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

如同您先前在 Xbox One 上，您可以接著處理邀請時您**CoreApplication**取得**Activated**事件和種類是啟用**ActivationKind::Protocol**.

### <a name="showing-the-gamer-profile-card"></a>顯示玩家的設定檔卡片

若要顯示玩家在 UWP 上的設定檔卡片，使用**Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()**，並傳入的目標使用者 XUID。

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>安全通訊端

安全通訊端 API 會包含在不同[Xbox Live 平台擴充功能 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)。

此論壇張貼 API 使用方式，請參閱：[跨平台設定為 SecureDeviceAssociation](https://forums.xboxlive.com/answers/45722/view.html)。

**附註**適用於 UWP **SocketDescriptions**一節已移出，appxmanifest 並進入自己[networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt)。 內部格式&lt;SocketDescriptions&gt;項目是幾乎完全相同，但**mx:** 前置詞。

Xbox 和 Windows 10 之間的跨播放，是*確定*的所有項目定義*相同*兩種不同的資訊清單 (Package.appxmanifest Xbox One 和 networkmanifest.xml 之間適用於 Windows 10)。 必須符合的通訊端的名稱、 通訊協定等*完全*。

也針對跨播放，您必須定義內的下列四個 SDA 用法```<AllowedUsages>```中的項目*兩者*Xbox 一個 Package.appxmanifest 和 Windows 10 networkmanifest.xml:

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>多人遊戲 QoS 度量

除了 「 安全通訊端 API 的命名空間變更，某些物件名稱和值已變更，太。 下表中找到的對應通常用於測量狀態。

表 2. 通常用來測量狀態對應。

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | TimedOut                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| 成功                            | 成功                                  |

所需的步驟*測量*QoS （服務品質） 和*處理結果*原則相同時處於比較的 XDK 和 UWP 版本的 api。 不過，由於名稱變更和一些設計變更，產生的程式碼看起來不同中的某些位置。

若要測量的 QoS **XDK**，您建立安全的裝置位址的集合和計量的集合，並傳遞到這些**MeasureQualityOfServiceAsync()** 方法。

若要測量的 QoS **UWP**，您建立新**XboxLiveQualityOfServiceMeasurement()** 物件，請呼叫**append （)** 至其**計量**並**DeviceAddresses**內容，然後再呼叫物件的**MeasureAsync()** 方法。

例如：

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

如需其他範例，請參閱 < **MatchmakingSession::MeasureQualityOfService()** 並**MatchmakingSession::ProcessQosMeasurements()** NetRumble 範例中的函式。

### <a name="writing-game-events"></a>撰寫的遊戲事件

設定項目的服務組態中的遊戲事件傳送 UWP 中有不同的 API。 使用 Xbox Live SDK **EventsService**和屬性包模型。

例如：

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

如需詳細資訊，請參閱 Xbox Live SDK 文件。

**祕訣**您可以使用**xcetool.exe**轉換將您下載從 XDP.h 標頭檔的 events.man 檔案隨附的 Xbox Live SDK （位於的 Tools 目錄）。 使用 '-x' 選項，以使用新 v2 屬性包的結構描述產生此 c + + 標頭。 此標頭包含所有您已設定的事件;，您可以呼叫的 c + + 函式例如， **EventWriteMultiplayerRoundStart()**。 如果您想要使用 WinRT 介面，您仍然可以參考此標頭檔，以查看如何建構針對每個事件的屬性和度量。

### <a name="game-chat"></a>遊戲的對談

GameChat UWP 中的為隨附於 Xbox Live SDK NuGet 套件二進位。 請參閱 < 如何將此 NuGet 套件新增至您的專案在 Xbox Live 程式設計手冊中的指示。

基本的用法是 XDK 和 UWP 版本之間幾乎完全相同。 在 API 中的一些差異包括：

1.  **User::AudioDeviceAdded**事件並不需要將其連結依 UWP 標題。 基礎的聊天程式庫會處理裝置加入及移除。

2.  **ChatUser**現在稱為**GameChatUser**。

3.  **Microsoft::Xbox::GameChat**命名空間保持不變，但**Windows::Xbox::Chat**命名空間已經成為**Microsoft::Xbox::ChatAudio**。

4.  **AddLocalUserToChatChannelAsync()** a XUID 或 a 會採用**ChatAudio::IChatUser ^** 而非**XboxUser**。

5.  **RemoveLocalUserFromChatChannelAsync()** 需要**ChatAudio::IChatUser ^** 而非**XboxUser**。 您可以取得**IChatUser**從**GameChatUser**-&gt;**使用者**。

### <a name="connected-storage"></a>已連接儲存體

連接儲存體 API 所提供的個別[Xbox Live 平台擴充功能 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)。 Xbox Live SDK 文件中包含文件。

整體流程是上的相同 Xbox One，加上**ContainersChangedSinceLastSync** UWP 版本中的屬性。 應該檢查這個屬性，當您的標題之後呼叫處理繼續事件， **GetForUserAsync()** 同樣地，查看容器變更您的職稱時在雲端中已暫止。 如果您將資料載入其中一個變更容器的記憶體中，您可能會想要讀取資料，以查看變更並據以處理所做的變更。

在 UWP 版本中其他值得注意的差異包括：

1.  從命名空間變更**Windows::Xbox::Storage**要**Windows::Gaming::XboxLive::Storage**。

2.  **ConnectedStorageSpace**已重新命名**GameSaveProvider**。

3.  **Windows::System::User**用於**GetForUserAsync()** 而非**XboxUser**，而 SCID 現在是必要。

4.  沒有本機的 「 電腦 」 儲存體 (亦即**GetForMachineAsync()** 已移除)。 請考慮使用**Windows::Storage::ApplicationData**改為您非漫遊，本機儲存資料。

5.  非同步結果會傳回例外狀況免費\*結果型別物件 (例如**GameSaveProviderGetResult**); 您可以檢查這個**狀態**屬性，而且如果沒有發生錯誤，讀取從傳回的物件**值**屬性。

6.  **ConnectedStorageErrorStatus 列舉**已重新命名**GameSaveErrorStatus**而且會傳回在**狀態**結果屬性。 所有的舊值存在，並已新增少數新的：

-   中止

-   ObjectExpired

-   確定

-   UserHasNoXboxLiveInfo

請參閱 GameSave 範例或 NetRumble 範例，例如使用方式。

**請注意**Gamesaveutil.exe 相當於 xbstorage.exe （開發人員命令列公用程式隨附的 XDK）。 安裝之後的 Xbox Live 平台擴充功能 SDK，此公用程式都可以在這裡找到：C:\\程式檔案 (x86)\\Windows 套件\\10\\延伸模組 Sdk\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>摘要

API 變更和新這份白皮書中所述的需求是，您可能會發生移植現有遊戲的程式碼從 Xbox One XDK 至新的 UWP 時。 特別是已經提供給應用程式和環境設定，以及 Xbox Live 的服務，例如多人遊戲，而且已連線的儲存體相關的功能區。 如需詳細資訊，請依照下列連結提供這整篇文章，並在下列的參考，並務必瀏覽的 「 Windows 10 」 一節[開發人員論壇](https://forums.xboxlive.com)如需詳細的說明、 解答和新聞。

## <a name="references"></a>參考

-   [從 Xbox One 移植到 Windows 10](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Xbox One 技術白皮書](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [範例](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
