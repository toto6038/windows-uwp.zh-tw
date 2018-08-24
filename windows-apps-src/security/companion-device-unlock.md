---
title: 使用 Windows Hello 隨附 (IoT) 裝置的 Windows 解除鎖定
description: Windows Hello 隨附裝置是可與您的 Windows 10 Desktop 搭配使用，以增強使用者驗證體驗的裝置。 透過 Windows Hello 隨附裝置架構，即使無法使用生物特徵辨識技術 (例如，如果 Windows 10 Desktop 缺少可進行臉部驗證的相機或指紋辨識器裝置)，隨附裝置還是可以提供豐富的 Windows Hello 體驗。
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 安全性
ms.assetid: 89f3d331-20cd-457b-83e8-1a22aaab2658
ms.localizationpriority: medium
ms.openlocfilehash: ac2e29d5219809058edee2d7a92c2e2475d9752e
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2830750"
---
# <a name="windows-unlock-with-windows-hello-companion-iot-devices"></a>使用 Windows Hello 隨附 (IoT) 裝置的 Windows 解除鎖定

Windows Hello 隨附裝置是可與您的 Windows 10 Desktop 搭配使用，以增強使用者驗證體驗的裝置。 透過 Windows Hello 隨附裝置架構，即使無法使用生物特徵辨識技術 (例如，如果 Windows 10 Desktop 缺少可進行臉部驗證的相機或指紋辨識器裝置)，隨附裝置還是可以提供豐富的 Windows Hello 體驗。

> **注意** Windows Hello 隨附裝置架構是無法供所有 app 開發人員使用的特殊功能。 若要使用這個架構，您的 app 必須由 Microsoft 特別佈建，並在其資訊清單中列出受限制的 *secondaryAuthenticationFactor* 功能。 若要取得核准，請連絡 [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com)。

## <a name="introduction"></a>簡介

> 如需視訊的概觀，請參閱 Channel 9 上 Build 2016 的[使用 IoT 裝置的 Windows 解除鎖定](https://channel9.msdn.com/Events/Build/2016/P491)會議。

> 如需程式碼範例，請參閱 [Windows Hello 隨附裝置架構 Github 存放庫](https://github.com/Microsoft/companion-device-framework)。

### <a name="use-cases"></a>使用案例

在為數眾多的方式中，有一個可以使用 Windows Hello 隨附裝置架構，利用隨附裝置來建置絕佳的 Windows 解除鎖定體驗。 例如，使用者可以︰

- 透過 USB 將隨附裝置連接到電腦、觸控隨附裝置上的按鈕，然後自動解除鎖定電腦。
- 隨身攜帶已經透過藍牙與電腦配對的手機。 按電腦上的空格鍵時，手機即會收到通知。 核准之後，電腦就會解除鎖定。
- 點選他們對於 NFC 讀取器的隨附裝置，快速解除鎖定電腦。
- 戴上已經驗證過穿戴者的健身手環。 在接近電腦時，藉由執行特殊的手勢 (例如拍手) 來解除鎖定電腦。

### <a name="biometric-enabled-windows-hello-companion-devices"></a>啟用生物特徵辨識技術的 Windows Hello 隨附裝置

如果隨附裝置支援生物特徵辨識技術，在某些情況下 [Windows 生物特徵辨識架構](https://msdn.microsoft.com/library/windows/hardware/mt608302(v=vs.85).aspx)可能是比 Windows Hello 隨附裝置架構更好的解決方案。 請連絡 [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com)，我們將協助您挑選正確的方法。

### <a name="components-of-the-solution"></a>解決方案的元件

下圖說明解決方案的元件以及負責建置它們的人員。

![架構概觀](images/companion-device-1.png)

Windows Hello 隨附裝置架構會實作為在 Windows 上執行的服務 (本文中稱為「隨附驗證服務」)。 此服務負責產生需要由儲存於 Windows Hello 隨附裝置上的 HMAC 金鑰提供保護的解除鎖定權杖。 這樣可以保證存取解除鎖定權杖需要 Windows Hello 隨附裝置存在。 針對每個 (電腦、Windows 使用者) Tuple，將會有唯一的解除鎖定權杖。

與 Windows Hello 隨附裝置架構整合需要︰

- 適用於隨附裝置的[通用 Windows 平台 (UWP)](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) Windows Hello 隨附裝置 app 可從 Windows app 市集下載。 
- 能夠在 Windows Hello 隨附裝置上建立兩個 256 位元的 HMAC 金鑰，並使用它 (使用 SHA-256) 來產生 HMAC。
- 正確設定 Windows 10 Desktop 上的安全性設定。 隨附驗證服務需要先設定這個 PIN，才能將任何 Windows Hello 隨附裝置插入它。 使用者必須透過 [設定] &gt; [帳戶] &gt; [登入選項] 來設定 PIN。

除了上述需求，Windows Hello 隨附裝置 app 還需負責︰

- 初始註冊的使用者體驗和商標，以及稍後取消註冊 Windows Hello 隨附裝置。
- 在背景中執行、探索 Windows Hello 隨附裝置、與 Windows Hello 隨附裝置以及隨附驗證服務通訊。
- 錯誤處理

通常，隨附裝置會隨附於 app 中以進行初始設定，就像第一次設定健身手環一樣。 本文件中所述的功能可以是該 app 的一部分，而且應該不需要個別的 app。  

### <a name="user-signals"></a>使用者訊號

每個 Windows Hello 隨附裝置應該與支援三個使用者訊號的 app 相結合。 這些訊號的形式可以是動作或手勢。

- **意圖訊號**︰可讓使用者顯示他要解除鎖定的意圖，例如，藉由按 Windows Hello 隨附裝置上的按鈕。 意圖訊號必須在 **Windows Hello 隨附裝置**端收集。
- **使用者存在的訊號**︰證明使用者存在。 例如，Windows Hello 隨附裝置可能需要 PIN，才能用來解除鎖定電腦 (請不要與電腦 PIN 混淆)，或者可能需要按下按鈕。
- **去除混淆訊號**︰若有多個選項可供 Windows Hello 隨附裝置使用，可去除混淆使用者想要解除鎖定的 Windows 10 Desktop。

您可以將任意數目的這類使用者訊號合併成一個。 每次使用時都需要使用者存在訊號與意圖訊號。

### <a name="registration-and-future-communication-between-a-pc-and-windows-hello-companion-devices"></a>在電腦與 Windows Hello 隨附裝置之間進行註冊與未來通訊

將 Windows Hello 隨附裝置插入 Windows Hello 隨附裝置架構之前，必須先向結構註冊。 Windows Hello 隨附裝置 app 完全擁有註冊的體驗。

Windows Hello 隨附裝置與 Windows 10 電腦 裝置間的關係可以是一對多的 (例如，一個隨附裝置可以供多個 Windows 10 電腦 裝置使用)。 不過，每個 Windows Hello 隨附裝置只能供每個 Windows 10 電腦 裝置上的一位使用者使用。   

在 Windows Hello 隨附裝置可以與電腦通訊之前，他們必須同意使用傳輸。 這類選擇會保留到 Windows Hello 隨附裝置 app；Windows Hello 隨附裝置架構不會在 Windows Hello 隨附裝置和 Windows 10 Desktop 裝置端上的 Windows Hello 隨附裝置 app 間使用的傳輸類型 (USB、NFC、WiFi、BT、BLE 等) 或通訊協定上加諸任何限制。 但是建議針對傳輸層使用某些安全性考量，如本文件的＜安全性需求＞一節中所概述。 裝置提供者必須負責提供這些需求。 架構不會為您提供它們。


## <a name="user-interaction-model"></a>使用者互動模型

### <a name="windows-hello-companion-device-app-discovery-installation-and-first-time-registration"></a>Windows Hello 隨附裝置 app 探索、安裝與第一次註冊

典型的使用者工作流程如下所示︰

- 使用者會在她想要使用該 Windows Hello 隨附裝置解除鎖定的每個目標 Windows 10 Desktop 裝置上設定 PIN。
- 使用者會在 Windows 10 Desktop 裝置上執行 Windows Hello 隨附裝置 app，以向 Windows 10 Desktop 註冊她的 Windows Hello 隨附裝置。

注意：

- 我們建議簡化和自動化 (如果可行) Windows Hello 隨附裝置 app 的探索、下載及啟動 (例如，可在 Windows 10 Desktop 裝置端點選 NFC 讀取器上的 Windows Hello 隨附裝置 app 來下載 app)。 不過，這是由 Windows Hello 隨附裝置和 Windows Hello 隨附裝置 app 負責執行。
- 在企業環境中，可透過 MDM 部署 Windows Hello 隨附裝置 app。
- Windows Hello 隨附裝置 app 負責向使用者顯示註冊過程中發生的任何錯誤訊息。

### <a name="registration-and-de-registration-protocol"></a>註冊和取消註冊通訊協定

下圖說明 Windows Hello 隨附裝置在註冊期間如何與隨附驗證服務互動。  

![註冊流程](images/companion-device-2.png)

我們的通訊協定中使用了兩個金鑰：

- 裝置金鑰 (**devicekey**)：用來保護電腦解除鎖定 Windows 所需的解除鎖定權杖。
- 驗證金鑰 (**authkey**)：用來相互驗證 Windows Hello 隨附裝置和隨附驗證服務。

裝置金鑰和驗證金鑰會在註冊期間，於 Windows Hello 隨附裝置 app 和隨附裝置之間交換。 因此，Windows Hello 隨附裝置 app 和 Windows Hello 隨附裝置必須使用安全傳輸來保護金鑰。

此外，請注意，雖然上圖顯示兩個在 Windows Hello 隨附裝置上產生的 HMAC 金鑰，但也可能針對 app 產生它們，並將其傳送到 Windows Hello 隨附裝置以便儲存。

### <a name="starting-authentication-flows"></a>開始驗證流程

使用者有兩種方式，可使用 Windows Hello 隨附裝置架構來開始 Windows 10 Desktop 的登入流程 (例如提供意圖訊號)：

- 開啟膝上型電腦的上蓋，或者按電腦上的空格鍵或向上撥動。
- 在 Windows Hello 隨附裝置端執行手勢或動作。

端看 Windows Hello 隨附裝置選擇要使用哪一個做為起點。 選擇選項一時，Windows Hello 隨附裝置架構將通知隨附裝置 app。 若選擇選項二，Windows Hello 隨附裝置 app 應會查詢隨附裝置，以查看是否已擷取該事件。 這可確保 Windows Hello 隨附裝置會在成功解除鎖定之前先收集意圖訊號。

### <a name="windows-hello-companion-device-credential-provider"></a>Windows Hello 隨附裝置認證提供者

Windows 10 提供新的認證提供者來處理所有 Windows Hello 隨附裝置。

Windows Hello 隨附裝置認證提供者負責透過啟用觸發程序來啟動隨附裝置背景工作。 第一次設定觸發程序是在電腦喚醒並顯示鎖定畫面時。 第二次則是當電腦正在進入登入 UI 時，而 Windows Hello 隨附裝置認證提供者就是所選取的磚。

Windows Hello 隨附裝置 app 的協助程式程式庫將會接聽鎖定畫面狀態變更，並傳送與 Windows Hello 隨附裝置背景工作相對應的事件。

如果有多個 Windows Hello 隨附裝置背景工作，第一個完成驗證程序的背景工作將會解除鎖定電腦。 隨附裝置驗證服務將會忽略任何剩餘的驗證呼叫。

Windows Hello 隨附裝置端上的體驗是由 Windows Hello 隨附裝置 app 所擁有和管理。 Windows Hello 隨附裝置架構無法控制這部分的使用者體驗。 更具體地說，隨附驗證提供者會通知 Windows Hello 隨附裝置 app (透過它的背景 app) 有關登入 UI 中的狀態變更 (例如，鎖定畫面剛剛呈現，或者使用者剛按下空格鍵來消除鎖定畫面)，而 Windows Hello 隨附裝置 app 會負責建置相關的體驗 (例如，在使用者按下空格鍵並消除解除鎖定畫面、透過 USB 開始查看裝置)。

Windows Hello 隨附裝置架構將提供一組 (當地語系化的) 文字和錯誤訊息，以供 Windows Hello 隨附裝置 app 選擇。 這些將會顯示在鎖定畫面上方 (或在登入 UI 中)。 如需詳細資訊，請參閱＜訊息和錯誤＞一節。

### <a name="authentication-protocol"></a>驗證通訊協定

一旦開始觸發與 Windows Hello 隨附裝置 app 相關聯的背景工作之後，它就會負責要求 Windows Hello 隨附裝置驗證由隨附驗證服務所計算出的 HMAC 值，並協助計算兩個 HMAC 值：
- 驗證服務 HMAC = HMAC (驗證金鑰、服務 nonce || 裝置 nonce || 工作階段 nonce)。
- 計算含有 nonce 的裝置金鑰 HMAC。
- 使用第一個 HMAC 值 (其會串聯隨附驗證服務所產生的 nonce) 來計算驗證金鑰的 HMAC。

服務會使用第二個計算出的值來驗證裝置，也會避免在傳輸通道中遭到重新執行攻擊。

![註冊流程](images/companion-device-3.png)

## <a name="lifecycle-management"></a>生命週期管理

### <a name="register-once-use-everywhere"></a>只需註冊一次，就能在任何地方使用

如果沒有後端伺服器，使用者就必須向每個 Windows 10 Desktop 裝置個別註冊他們的 Windows Hello 隨附裝置。

隨附裝置廠商或 OEM 可以實作 Web 服務，以便在使用者 Windows 10 Desktop 或行動裝置之間漫遊註冊狀態。 如需詳細資訊，請參閱＜漫遊、撤銷及篩選服務＞一節。

### <a name="pin-management"></a>PIN 管理

您必須先在 Windows 10 Desktop 裝置上設定 PIN，才能使用隨附裝置。 這可確保使用者在發生 Windows Hello 隨附裝置無法運作的情況下會有備份。 該 PIN 是 Windows 所管理且 app 絕對不會看見的項目。 若要變更它，使用者可瀏覽至 [設定] &gt; [帳戶] &gt; [登入選項]。

### <a name="management-and-policy"></a>管理和原則

使用者可以透過在 Windows 10 Desktop 裝置上執行 Windows Hello 隨附裝置 app，從該 Desktop 裝置移除 Windows Hello 隨附裝置。

企業有兩個選項可以控制 Windows Hello 隨附裝置架構：

- 開啟或關閉這個功能
- 定義使用 Windows AppLocker 允許的 Windows Hello 隨附裝置允許清單

Windows Hello 隨附裝置架構不支援任何集中式方法來持續清查可用的隨附裝置，或是允許進一步篩選 Windows Hello 隨附裝置類型執行個體的方法 (例如，只允許序號介於 X 和 Y 之間的隨附裝置)。 但是，App 開發人員可以建置服務來提供這類功能。 如需詳細資訊，請參閱＜漫遊、撤銷及篩選服務＞一節。

### <a name="revocation"></a>撤銷

Windows Hello 隨附裝置架構不支援從遠端移除特定 Windows 10 Desktop 裝置的隨附裝置。 使用者可以改為透過在 Windows 10 Desktop 上執行的 Windows Hello 隨附裝置 app 來移除 Windows Hello 隨附裝置。

不過，隨附裝置廠商可以建置服務來提供遠端撤銷功能。 如需詳細資訊，請參閱＜漫遊、撤銷及篩選服務＞一節。

### <a name="roaming-and-filter-services"></a>漫遊和篩選服務

隨附裝置廠商可以實作可用於下列案例的 Web 服務︰

- 適用於企業的篩選服務︰企業可以將一組可在其環境中運作的 Windows Hello 隨附裝置，限制為由特定廠商所提供的極少數項目。 例如，Contoso 公司可向廠商 X 訂購 10,000 個型號 Y 的隨附裝置，並確定只有這些裝置能在 Contoso 網域中運作 (由廠商 X 提供的所有其他裝置型號都不行)。
- 清查︰企業可以決定要在企業環境中使用的現有隨附裝置清單。
- 即時撤銷︰如果員工報告他的隨附裝置已遺失或遭竊時，可以使用 Web 服務來撤銷該裝置。
- 漫遊︰使用者只需註冊他的隨附裝置一次，就能在他的所有 Windows 10 Desktop 和行動裝置上運作。

實作這些功能會要求 Windows Hello 隨附裝置 app 在註冊和使用期間與 Web 服務聯繫。 Windows Hello 隨附裝置 app 可以針對快取登入案例最佳化，例如，一天只需要求與 Web 服務聯繫一次 (代價是最多可將撤銷時間延長為一天)。  

## <a name="windows-hello-companion-device-framework-api-model"></a>Windows Hello 隨附裝置架構 API 模型

### <a name="overview"></a>概觀

Windows Hello 隨附裝置 app 應該包含兩個元件︰具有 UI 的前景 app 負責註冊和取消註冊裝置，而背景工作會負責處理驗證。

整體的 API 流程如下所示︰

1. 註冊 Windows Hello 隨附裝置
    * 確定裝置在附近並查詢其功能 (如有必要)
    * 產生兩個 HMAC 金鑰 (在隨附裝置端或 app 端)
    * 呼叫 RequestStartRegisteringDeviceAsync
    * 呼叫 FinishRegisteringDeviceAsync
    * 確定 Windows Hello 隨附裝置 app 會儲存 HMAC 金鑰 (如果支援)，而且 Windows Hello 隨附裝置 app 會捨棄其複本
2. 註冊您的背景工作
3. 等待背景工作中產生正確的事件
    * WaitingForUserConfirmation：如果需要 Windows Hello 隨附裝置端上的使用者動作/手勢才能啟動驗證流程，請等待此事件
    * CollectingCredential：如果 Windows Hello 隨附裝置依賴電腦端上的使用者動作/手勢來啟動驗證流程 (例如按空格鍵)，請等待此事件
    * 其他觸發程序 (例如智慧卡)︰務必查詢目前的驗證狀態以呼叫正確的 API。
4. 讓使用者藉由呼叫 ShowNotificationMessageAsync 來得知錯誤訊息或所需的下一個步驟。 只有在收集到意圖訊號之後才呼叫這個 API
5. 解除鎖定
    * 確定已收集到意圖訊號和使用者存在訊號
    * 呼叫 StartAuthenticationAsync
    * 與隨附裝置通訊以執行所需的 HMAC 作業
    * 呼叫 FinishAuthenticationAsync
6. 在使用者提出要求時取消註冊 Windows Hello 隨附裝置 (例如，如果他們遺失了隨附裝置)
    * 透過 FindAllRegisteredDeviceInfoAsync，為登入的使用者列舉 Windows Hello 隨附裝置
    * 使用 UnregisterDeviceAsync 取消註冊隨附裝置

### <a name="registration-and-de-registration"></a>註冊和取消註冊

註冊需要對隨附驗證服務進行兩個 API 呼叫：RequestStartRegisteringDeviceAsync 和 FinishRegisteringDeviceAsync。

進行這任一個呼叫之前，Windows Hello 隨附裝置 app 必須確定有可用的 Windows Hello 隨附裝置。 如果 Windows Hello 隨附裝置負責產生 HMAC 金鑰 (驗證和裝置金鑰)，則 Windows Hello 隨附裝置 app 應該也會要求隨附裝置先產生它們，然後才會呼叫任一個上述這兩個呼叫。 如果 Windows Hello 裝置隨附 app 負責產生 HMAC 金鑰，則它應該在呼叫上述兩個呼叫之前先執行此動作。

此外，第一次進行 API 呼叫 (RequestStartRegisteringDeviceAsync) 時，Windows Hello 隨附裝置 app 必須決定裝置功能，並準備好將它當成 API 呼叫的一部分來傳送；例如，Windows Hello 隨附裝置是否支援安全儲存 HMAC 金鑰。 如果使用同一個 Windows Hello 隨附裝置 app 來管理多個版本的相同隨附裝置，而那些功能變更 (並要求裝置查詢來決定)，則建議您在進行第一次 API 呼叫之前，進行這個查詢。   

第一個 API (RequestStartRegisteringDeviceAsync) 會傳回第二個 API (FinishRegisteringDeviceAsync) 所使用的控制代碼。 對於註冊的第一個呼叫將啟動 PIN 提示，以確定使用者存在。 如果未設定 PIN，則這個呼叫會失敗。 Windows Hello 隨附裝置 app 也會查詢是否已透過 KeyCredentialManager.IsSupportedAsync 呼叫設定 PIN。 如果原則已停用 Windows Hello 隨附裝置的使用，則 RequestStartRegisteringDeviceAsync 呼叫也會失敗。

第一個呼叫的結果會透過 SecondaryAuthenticationFactorRegistrationStatus 列舉傳回︰

```C#
{
    Failed = 0,         // Something went wrong in the underlying components
    Started,            // First call succeeded
    CanceledByUser,     // User cancelled PIN prompt
    PinSetupRequired,   // PIN is not set up
    DisabledByPolicy,   // Companion device framework or this app is disabled
}
```

第二個呼叫 (FinishRegisteringDeviceAsync) 會完成註冊。 在註冊程序期間，Windows Hello 隨附裝置 app 可以使用隨附驗證服務來儲存隨附裝置設定資料。 此資料的大小限制為 4K。 此資料在驗證期間可供 Windows Hello 隨附裝置 app 使用。 舉例來說，此資料可用來連線到 Windows Hello 隨附裝置 (例如 MAC 位址)，或者，如果 Windows Hello 隨附裝置沒有存放裝置且隨附裝置想要使用電腦做為存放裝置，則可使用設定資料。 請注意，儲存為設定資料一部分的所有敏感資料必須使用只有 Windows Hello 隨附裝置知道的金鑰來加密。 此外，假定設定資料是透過 Windows 服務來儲存，就能供使用者設定檔上的 Windows Hello 隨附裝置 app 使用。

Windows Hello 隨附裝置 app 可以呼叫 AbortRegisteringDeviceAsync 來取消註冊，然後傳入錯誤碼。 隨附驗證服務會將錯誤記錄於遙測資料中。 這個呼叫的最佳範例就是當 Windows Hello 隨附裝置發生錯誤且無法完成註冊 (例如，它無法儲存 HMAC 金鑰或 BT 連線中斷) 時。

Windows Hello 隨附裝置 app 必須為使用者提供選項，從其 Windows 10 Desktop 中取消註冊他們的 Windows Hello 隨附裝置 (例如，如果他們遺失了隨附裝置或購買了新版本)。 當使用者選取該選項時，則 Windows Hello 隨附裝置 app 必須呼叫 UnregisterDeviceAsync。 這個由 Windows Hello 隨附裝置 app 所做的呼叫將會觸發隨附裝置驗證服務，來刪除對應到電腦端上的特定裝置識別碼及呼叫者 app 的 AppId 的所有資料 (包括 HMAC 金鑰)。 此 API 呼叫不會嘗試從 Windows Hello 隨附裝置 app 或隨附裝置端刪除 HMAC 金鑰。 其會保留來讓 Windows Hello 隨附裝置 app 能夠進行實作。

Windows Hello 隨附裝置 app 會負責顯示在註冊和取消註冊階段所產生的任何錯誤訊息。

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.UI.Popups;

namespace SecondaryAuthFactorSample
{
    public class DeviceRegistration
    {

        public void async OnRegisterButtonClick()
        {
            //
            // Pseudo function, the deviceId should be retrieved by the application from the device
            //
            string deviceId = await ReadSerialNumberFromDevice();

            IBuffer deviceKey = CryptographicBuffer.GenerateRandom(256/8);
            IBuffer mutualAuthenticationKey = CryptographicBuffer.GenerateRandom(256/8);

            SecondaryAuthenticationFactorRegistration registrationResult =
                await SecondaryAuthenticationFactorRegistration.RequestStartRegisteringDeviceAsync(
                    deviceId,  // deviceId: max 40 wide characters. For example, serial number of the device
                    SecondaryAuthenticationFactorDeviceCapabilities.SecureStorage |
                        SecondaryAuthenticationFactorDeviceCapabilities.HMacSha256 |
                        SecondaryAuthenticationFactorDeviceCapabilities.StoreKeys,
                    "My test device 1", // deviceFriendlyName: max 64 wide characters. For example: John's card
                    "SAMPLE-001", // deviceModelNumber: max 32 wide characters. The app should read the model number from device.
                    deviceKey,
                    mutualAuthenticationKey);

            switch(registerResult.Status)
            {
            case SecondaryAuthenticationFactorRegistrationStatus.Started:
                //
                // Pseudo function:
                // The app needs to retrieve the value from device and set into opaqueBlob
                //
                IBuffer deviceConfigData = ReadConfigurationDataFromDevice();

                if (deviceConfigData != null)
                {
                    await registrationResult.Registration.FinishRegisteringDeviceAsync(deviceConfigData); //config data limited to 4096 bytes
                    MessageDialog dialog = new MessageDialog("The device is registered correctly.");
                    await dialog.ShowAsync();
                }
                else
                {
                    await registrationResult.Registration.AbortRegisteringDeviceAsync("Failed to connect to the device");
                    MessageDialog dialog = new MessageDialog("Failed to connect to the device.");
                    await dialog.ShowAsync();
                }
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.CanceledByUser:
                MessageDialog dialog = new MessageDialog("You didn't enter your PIN.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.PinSetupRequired:
                MessageDialog dialog = new MessageDialog("Please setup PIN in settings.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.DisabledByPolicy:
                MessageDialog dialog = new MessageDialog("Your enterprise prevents using this device to sign in.");
                await dialog.ShowAsync();
                break;
            }
        }

        public void async UpdateDeviceList()
        {
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.User);

            if (deviceInfoList.Count > 0)
            {
                foreach (SecondaryAuthenticationFactorInfo deviceInfo in deviceInfoList)
                {
                    //
                    // Add deviceInfo.FriendlyName and deviceInfo.DeviceId into a combo box
                    //
                }
            }
        }

        public void async OnUnregisterButtonClick()
        {
            string deviceId;
            //
            // Read the deviceId from the selected item in the combo box
            //
            await SecondaryAuthenticationFactorRegistration.UnregisterDeviceAsync(deviceId);
        }
    }
}
```

### <a name="authentication"></a>驗證

驗證需要對隨附驗證服務進行兩個 API 呼叫︰StartAuthenticationAsync 和 FinishAuthencationAsync。

第一個初始化 API 將會傳回第二個 API 所使用的控制代碼。  除此之外，第一個呼叫還會傳回一個 nonce (會立即串聯其他項目)，其需要使用儲存於 Windows Hello 隨附裝置上的裝置金鑰來設定 HMAC。 第二個呼叫會使用裝置金鑰傳回 HMAC 的結果，而且可能會在成功驗證時結束 (也就是使用者將會看到他們的桌面)。

如果原則已在初始註冊之後停用該 Windows Hello 隨附裝置，第一個初始化 API (StartAuthenticationAsync) 就會失敗。 如果 API 呼叫是在 WaitingForUserConfirmation 或 CollectingCredential 狀態外部所進行，則該 API 也會失敗 (詳情請見本節後續內容)。 如果取消註冊的隨附裝置 app 會呼叫該 API，則它也會失敗。 SecondaryAuthenticationFactorAuthenticationStatus 列舉會摘要說明可能的結果︰

```C#
{
    Failed = 0,                     // Something went wrong in the underlying components
    Started,
    UnknownDevice,                  // Companion device app is not registered with framework
    DisabledByPolicy,               // Policy disabled this device after registration
    InvalidAuthenticationStage,     // Companion device framework is not currently accepting
                                    // incoming authentication requests
}
```

如果第一個呼叫中提供的 nonce 到期 (20 秒)，則第二個 API 呼叫 (FinishAuthencationAsync) 會失敗。 SecondaryAuthenticationFactorFinishAuthenticationStatus 列舉會擷取可能的結果。

```C#
{
    Failed = 0,     // Something went wrong in the underlying components
    Completed,      // Success
    NonceExpired,   // Nonce is expired
}
```

這兩個 API 呼叫 (StartAuthenticationAsync 和 FinishAuthencationAsync) 的時機需要配合 Windows Hello 隨附裝置收集意圖、使用者存在及去除混淆訊號的方式 (如需詳細資訊，請參閱＜使用者訊號＞)。 例如，必須先取得意圖訊號，才能提交第二個呼叫。 換句話說，如果使用者未表達解除鎖電腦的意圖，就不應將它解除鎖定。 更明確地說，假設您使用藍牙鄰近性來解除鎖定電腦，則必須收集明確的意圖訊號，否則，一旦使用者帶著他的電腦前往廚房途中，該電腦將會解除鎖定。 此外，從第一個呼叫傳回的 nonce 也會與時間繫結 (20 秒)，將會在特定期間之後到期。 因此，只有在 Windows Hello 隨附裝置 app 具有隨附裝置存在的良好指示時，才應進行第一個呼叫，例如，將隨附裝置插入 USB 連接埠或在 NFC 讀取器上點選它。 使用藍牙時，請小心謹慎，以避免影響電腦端的電池，或影響其他將在檢查 Windows Hello 隨附裝置是否存在的時點上發生的藍牙活動。 此外，如果需要提供使用者存在訊號 (例如，藉由輸入 PIN)，建議您只有在收集到該訊號之後，才能進行第一個驗證呼叫。

Windows Hello 隨附裝置架構可藉由提供使用者在驗證流程中的全貌，來協助 Windows Hello 隨附裝置 app 做出何時應進行上述兩個呼叫的通知決定。 Windows Hello 隨附裝置架構會藉由為 app 背景工作提供鎖定狀態變更通知來提供此功能。

![隨附裝置流程](images/companion-device-4.png)

這其中每一個狀態的詳細資料如下所示︰

| 狀態                         | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| WaitingForUserConfirmation    | 出現鎖定畫面時 (例如，使用者按下 Windows + L 鍵)，即會觸發此狀態變更通知事件。 由於很難找到處於此狀態的裝置，因此我們建議不要請求任何與之相關的錯誤訊息。 我們通常建議只在有意圖訊號可用時，才顯示訊息。 如果隨附裝置會收集意圖訊號 (例如，在 NFC 讀取器上點選、按隨附裝置上的按鈕，或像是拍手的特定手勢)，Windows Hello 隨附裝置 app 就應該進行第一個 API 呼叫以便在此狀態中進行驗證，而 Windows Hello 隨附裝置 app 背景工作會接收來自偵測到該意圖訊號之隨附裝置的指示。 否則，如果 Windows Hello 裝置隨附 app 依賴電腦來啟動驗證流程 (讓使用者向上撥動解除鎖定畫面，或者按空格鍵)，則 Windows Hello 隨附裝置 app 就需要等待下一個狀態 (CollectingCredential)。     |
| CollectingCredential          | 當使用者開啟膝上型電腦的上蓋、按鍵盤上的任何按鍵，或者向上撥動至解除鎖定畫面時，即會觸發此狀態變更通知事件。 如果 Windows Hello 隨附裝置依賴上述動作以開始收集意圖訊號，則 Windows Hello 隨附裝置 app 應該會開始收集它 (例如，透過隨附裝置上的快顯視窗，詢問使用者是否想要解除鎖定電腦)。 如果 Windows Hello 隨附裝置 app 需要使用者在隨附裝置上提供使用者存在訊號 (例如，在 Windows Hello 隨附裝置上輸入 PIN)，則這是提供錯誤案例的好時機。                                                                                                                                                                                                                                                                                                                                            |
| SuspendingAuthentication      | 當 Windows Hello 隨附裝置 app 收到這個狀態時，表示隨附驗證服務已停止接受驗證要求。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| CredentialCollected           | 這表示另一個 Windows Hello 隨附裝置 app 已經呼叫第二個 API，而隨附驗證服務正在驗證提交的項目。 此時，除非目前提交的項目未通過驗證，否則隨附驗證服務不會接受任何其他驗證要求。 在到達下一個狀態之前，Windows Hello 隨附裝置 app 應持續關注。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CredentialAuthenticated       | 這表示提交的認證是有效的。 CredentialAuthenticated 具備已成功之 Windows Hello 隨附裝置的裝置識別碼。 Windows Hello 隨附裝置 app 應該確定會在其上進行檢查，以查看與其相關聯的裝置是否會勝出。 如果沒有，則 Windows Hello 隨附裝置 app 應該避免顯示任何後續驗證流程 (例如，隨附裝置上的成功訊息，也可能是該裝置上的震動) 請注意，如果提交的認證無法運作，則狀態將變更為 CollectingCredential 狀態。                                                                                                                                                                                                                                                                                                                                                                                       |
| StoppingAuthentication        | 驗證成功且使用者看到桌面。 刪除背景工作的時機。 先結束背景工作，明確取消登錄 StageEvent 處理常式。 這將有助於快速結束背景工作。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |



Windows Hello 隨附裝置 app 只能在前兩個狀態中呼叫這兩個驗證 API。 Windows Hello 隨附裝置 app 應該檢查是哪一種案例觸發了這個事件。 有兩種可能性︰解除鎖定或後續解除鎖定。 目前只支援解除鎖定。 在未來的版本中，可能會支援後續解除鎖定的案例。 SecondaryAuthenticationFactorAuthenticationScenario 列舉會擷取這兩個選項︰

```C#
{
    SignIn = 0,         // Running under lock screen mode
    CredentialPrompt,   // Running post unlock
}
```

完整程式碼範例︰

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using System.Threading;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public sealed class AuthenticationTask : IBackgroundTask
    {
        private string _deviceId;
        private static AutoResetEvent _exitTaskEvent = new AutoResetEvent(false);
        private static IBackgroundTaskInstance _taskInstance;
        private BackgroundTaskDeferral _deferral;

        private void Authenticate()
        {
            int retryCount = 0;

            while (retryCount < 3)
            {
                //
                // Pseudo code, the svcAuthNonce should be passed to device or generated from device
                //
                IBuffer svcAuthNonce = CryptographicBuffer.GenerateRandom(256/8);

                SecondaryAuthenticationFactorAuthenticationResult authResult = await
                    SecondaryAuthenticationFactorAuthentication.StartAuthenticationAsync(
                        _deviceId,
                        svcAuthNonce);
                if (authResult.Status != SecondaryAuthenticationFactorAuthenticationStatus.Started)
                {
                    SecondaryAuthenticationFactorAuthenticationMessage message;
                    switch (authResult.Status)
                    {
                        case SecondaryAuthenticationFactorAuthenticationStatus.DisabledByPolicy:
                            message = SecondaryAuthenticationFactorAuthenticationMessage.DisabledByPolicy;
                            break;
                        case SecondaryAuthenticationFactorAuthenticationStatus.InvalidAuthenticationStage:
                            // The task might need to wait for a SecondaryAuthenticationFactorAuthenticationStageChangedEvent
                            break;
                        default:
                            return;
                    }

                    // Show error message. Limited to 512 characters wide
                    await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(null, message);
                    return;
                }

                //
                // Pseudo function:
                // The device calculates and returns sessionHmac and deviceHmac
                //
                await GetHmacsFromDevice(
                    authResult.Authentication.ServiceAuthenticationHmac,
                    authResult.Authentication.DeviceNonce,
                    authResult.Authentication.SessionNonce,
                    out deviceHmac,
                    out sessionHmac);
                if (sessionHmac == null ||
                    deviceHmac == null)
                {
                    await authResult.Authentication.AbortAuthenticationAsync(
                        "Failed to read data from device");
                    return;
                }

                SecondaryAuthenticationFactorFinishAuthenticationStatus status =
                    await authResult.Authentication.FinishAuthencationAsync(deviceHmac, sessionHmac);
                if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.NonceExpired)
                {
                    retryCount++;
                    continue;
                }
                else if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.Completed)
                {
                    // The credential data is collected and ready for unlock
                    return;
                }
            }
        }

        public void OnAuthenticationStageChanged(
            object sender,
            SecondaryAuthenticationFactorAuthenticationStageChangedEventArgs args)
        {
            // The application should check the args.StageInfo.Stage to determine what to do in next. Note that args.StageInfo.Scenario will have the scenario information (SignIn vs CredentialPrompt).

            switch(args.StageInfo.Stage)
            {
            case SecondaryAuthenticationFactorAuthenticationStage.WaitingForUserConfirmation:
                // Show welcome message
                await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(
                    null,
                    SecondaryAuthenticationFactorAuthenticationMessage.WelcomeMessageSwipeUp);
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CollectingCredential:
                // Authenticate device
                Authenticate();
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CredentialAuthenticated:
                if (args.StageInfo.DeviceId = _deviceId)
                {
                    // Show notification on device about PC unlock
                }
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.StoppingAuthentication:
                // Quit from background task
                _exitTaskEvent.Set();
                break;
            }

            Debug.WriteLine("Authentication Stage = " + args.StageInfo.AuthenticationStage.ToString());
        }

        //
        // The Run method is the entry point of a background task.
        //
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            _taskInstance = taskInstance;
            _deferral = taskInstance.GetDeferral();

            // Register canceled event for this task
            taskInstance.Canceled += TaskInstanceCanceled;

            // Find all device registred by this application
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.AllUsers);

            if (deviceInfoList.Count == 0)
            {
                // Quit the task silently
                return;
            }
            _deviceId = deviceInfoList[0].DeviceId;
            Debug.WriteLine("Use first device '" + _deviceId + "' in the list to signin");

            // Register AuthenticationStageChanged event
            SecondaryAuthenticationFactorRegistration.AuthenticationStageChanged += OnAuthenticationStageChanged;

            // Wait the task exit event
            _exitTaskEvent.WaitOne();

            _deferral.Complete();
        }

        void TaskInstanceCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _exitTaskEvent.Set();
        }
    }
}
```

### <a name="register-a-background-task"></a>註冊背景工作

當 Windows Hello 隨附裝置 app 註冊第一個隨附裝置時，也應該註冊它的背景工作元件，其將會在裝置和隨附裝置驗證服務之間傳遞驗證訊息。

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public class BackgroundTaskManager
    {
        // Register background task
        public static async Task<IBackgroundTaskRegistration> GetOrRegisterBackgroundTaskAsync(
            string bgTaskName,
            string taskEntryPoint)
        {
            // Check if there's an existing background task already registered
            var bgTask = (from t in BackgroundTaskRegistration.AllTasks
                          where t.Value.Name.Equals(bgTaskName)
                          select t.Value).SingleOrDefault();
            if (bgTask == null)
            {
                BackgroundAccessStatus status =
                    BackgroundExecutionManager.RequestAccessAsync().AsTask().GetAwaiter().GetResult();

                if (status == BackgroundAccessStatus.Denied)
                {
                    Debug.WriteLine("Background Execution is denied.");
                    return null;
                }

                var taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = bgTaskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger(new SecondaryAuthenticationFactorAuthenticationTrigger());
                bgTask = taskBuilder.Register();
                // Background task is registered
            }

            bgTask.Completed += BgTask_Completed;
            bgTask.Progress += BgTask_Progress;

            return bgTask;
        }
    }
}
```

### <a name="errors-and-messages"></a>錯誤和訊息

Windows Hello 隨附裝置架構會負責為使用者提供有關登入成功或失敗的回饋。 Windows Hello 隨附裝置架構將提供一組 (當地語系化的) 文字和錯誤訊息，以供 Windows Hello 隨附裝置 app 選擇。 這些將會顯示於登入 UI 中。

![隨附裝置錯誤](images/companion-device-5.png)

Windows Hello 隨附裝置 app 可以使用 ShowNotificationMessageAsync，為使用者顯示訊息以做為登入 UI 的一部分。 如果有意圖訊號可用，就呼叫這個 API。 請注意，意圖訊號一律必須在 Windows Hello 隨附裝置端收集。

有兩種類型的訊息：指引和錯誤。

指引訊息是設計來向使用者顯示如何開始解除鎖定程序。 這些訊息只會在第一次進行裝置註冊時於鎖定畫面向使用者顯示一次，並且永遠不會再次於該處顯示。 這些訊息會繼續顯示在鎖定畫面。

錯誤訊息一律會顯示，並且會在提供意圖訊號之後顯示。 假設在向使用者顯示訊息之前必須先收集意圖訊號，而使用者只會使用這其中一個 Windows Hello 隨附裝置來提供該意圖，則不能發生下列情況：有多個 Windows Hello 隨附裝置競相顯示錯誤訊息。 因此，Windows Hello 隨附裝置架構不會保留任何佇列。 當呼叫者要求錯誤訊息時，它將會顯示 5 秒，並捨棄在那 5 秒間所有顯示錯誤訊息的其他要求。 過了 5 秒之後，就有機會為其他呼叫者顯示錯誤訊息。 我們禁止任何呼叫者塞滿錯誤通道。

指引和錯誤訊息如下所示。 裝置名稱是隨附裝置 app 傳遞的參數，可做為 ShowNotificationMessageAsync 的一部分。

**指引**

- 「向上撥動或按空格鍵，使用*裝置名稱*來登入。」
- 「正在設定您的隨附裝置。 請稍候或使用另一個登入選項。」
- 「將*裝置名稱*與 NFC 讀取器輕觸以登入。」
- 「正在尋找*裝置名稱*...」
- 「將*裝置名稱*插入 USB 連接埠來登入。」

**錯誤**

- 「請參閱*裝置名稱*，以取得登入指示。」
- 「開啟藍牙以使用*裝置名稱*來登入。」
- 「開啟 NFC 以使用*裝置名稱*來登入。」
- 「連線到 Wi-Fi 網路，以使用*裝置名稱*來登入。」
- 「再次點選*裝置名稱*。」
- 「您的企業可以防止利用*裝置名稱*來登入。 使用另一個登入選項。」
- 「點選*裝置名稱*來登入。」
- 「將您的手指停留在*裝置名稱*上來登入。」
- 「使用您的手指在*裝置名稱*上撥動來登入。」
- 「無法使用*裝置名稱*來登入。 使用另一個登入選項。」
- 「發生問題。 使用另一個登入選項，然後重新設定*裝置名稱*。」
- 「再試一次。」
- 「對*裝置名稱*說出複雜密碼。」
- 「準備好使用*裝置名稱*來登入。」
- 「首先使用另一個登入選項，您就可以使用*裝置名稱*登入。」

### <a name="enumerating-registered-devices"></a>列舉已註冊的裝置

Windows Hello 隨附裝置 app 可以透過 FindAllRegisteredDeviceInfoAsync 呼叫，來列舉已註冊的隨附裝置清單。 這個 API 支援透過列舉 SecondaryAuthenticationFactorDeviceFindScope 定義的兩個查詢類型︰

```C#
{
    User = 0,
    AllUsers,
}
```

第一個範圍會傳回登入使用者的裝置清單。 第二個範圍會傳回該電腦上所有使用者的清單。 在取消註冊期間必須使用第一個範圍，以避免將其他使用者的 Windows Hello 隨附裝置取消註冊。 在驗證或註冊期間必須使用第二個範圍：在註冊期間，這個列舉可協助 app 避免嘗試註冊相同的 Windows Hello 隨附裝置兩次。

請注意，即使 app 不會執行這項檢查，但電腦還是會執行，並將拒絕多次註冊相同的 Windows Hello 隨附裝置。 在驗證期間，使用 AllUsers 範圍有助於 Windows Hello 隨附裝置 app 支援切換使用者流程：在使用者 B 登入時登入使用者 A (這會要求這兩位使用者必須安裝 Windows Hello 隨附裝置 app，而且使用者 A 必須已向電腦註冊他們的隨附裝置，且電腦正處於鎖定畫面 (或登入畫面))。

## <a name="security-requirements"></a>安全性需求

隨附驗證服務會提供下列安全性防護。

- Windows 10 Desktop 裝置上以中型使用者身分或 app 容器執行的惡意程式碼，無法使用 Windows Hello 隨附裝置自動存取電腦上的使用者認證金鑰 (已儲存為 Windows Hello 的一部分)。
- Windows 10 Desktop 裝置上的惡意使用者無法使用該 Windows 10 Desktop 裝置上屬於其他使用者的 Windows Hello 隨附裝置，以無訊息方式存取他的使用者認證金鑰 (在同一個 Windows 10 Desktop 裝置上)。
- Windows Hello 隨附裝置上的惡意程式碼無法以無訊息方式存取 Windows 10 Desktop 裝置上的使用者認證金鑰，包括利用特別針對 Windows Hello 隨附裝置架構開發的功能和程式碼。
- 惡意的使用者無法透過擷取 Windows Hello 隨附裝置與 Windows 10 Desktop 裝置之間的流量並於稍後重新執行，來解除鎖定 Windows 10 Desktop 裝置 在我們的通訊協定中使用 nonce、authkey 及 HMAC 保證能提供保護，免於遭到重新執行攻擊。
- Rogue 電腦上的惡意程式碼或惡意使用者無法使用 Windows Hello 隨附裝置來存取正當的使用者電腦。 這是透過隨附驗證服務和 Windows Hello 隨附裝置之間的相互驗證，藉由在我們的通訊協定中使用 authkey 和 HMAC 來達成。

如果想要達到上述列舉的安全性保護，關鍵是保護 HMAC 金鑰免於受到未經驗證的存取，同時也要確認使用者存在。 更具體來說，它必須符合下列需求︰

- 在複製 Windows Hello 隨附裝置期間提供保護
- 提供保護，以防止註冊期間將 HMAC 金鑰傳送到電腦時遭到竊聽
- 確認該使用者存在訊號可供使用
