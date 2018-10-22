---
title: Windows Hello
description: 本文章說明隨附在 Windows 10 作業系統中的新 Windows Hello 技術，然後討論開發人員可如何使用這項技術，來保護自己的通用 Windows 平台 (UWP) 應用程式和後端服務。 文章強調該技術的幾個特定功能，以協助您減少因使用傳統認證所帶來的威脅；它還提供指南來引導您設計及部署該技術，來做為您 Windows 10 首度發行的一部分。
ms.assetid: 0B907160-B344-4237-AF82-F9D47BCEE646
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: b81bf3c55b493d8e36f186ec56b68367c6245cc7
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5400931"
---
# <a name="windows-hello"></a>Windows Hello




本文章說明隨附在 Windows 10 作業系統中的新 Windows Hello 技術，然後討論開發人員可如何使用這項技術，來保護自己的通用 Windows 平台 (UWP) 應用程式和後端服務。 文章強調該技術的幾個特定功能，以協助您減少因使用傳統認證所帶來的威脅；它還提供指南來引導您設計及部署該技術，來做為您 Windows 10 首度發行的一部分。

請注意，本文著重在應用程式開發上。 如需 Windows Hello 的結構與實作的詳細資訊，請參閱 [TechNet 上的 Windows Hello 指南](https://technet.microsoft.com/library/mt589441.aspx)。

如需完整的程式碼範例，請參閱 [GitHub 上的 Windows Hello 程式碼範例](http://go.microsoft.com/fwlink/?LinkID=717812)。

如需如何使用 Windows Hello 及支援驗證服務來建立 UWP app 的逐步解說，請參閱 [Windows Hello 登入應用程式](microsoft-passport-login.md)及 [Windows Hello 登入服務](microsoft-passport-login-auth-service.md)這兩篇文章。

## <a name="1-introduction"></a>1 簡介


對於資訊安全的基本假設，就是系統可以識別正在使用它的人員。 識別使用者的功能，可讓系統判斷是否已適當地識別使用者的身分 (此程序稱為驗證)，然後決定已適當地經過驗證的使用者應該能夠執行哪些動作 (授權)。 世界各地絕大多數的電腦系統，都依賴使用者認證來做出驗證及授權的決定，這代表這些系統在安全性方面，依賴由使用者自行建立，且可重複使用的密碼。 有句經常被引用的名言，指出驗證牽涉到「您知道的事情、您擁有的東西，或是您扮演的角色」，這巧妙地點出下列問題：可重複使用的密碼是本身就是驗證因素，因此任何知道密碼的人，都能冒充擁有該密碼的使用者。

## <a name="11-problems-with-traditional-credentials"></a>1.1 傳統認證的問題


自從 1960 年代中期，Fernando Corbató 和他在麻省理工學院的團隊積極引進密碼之後，使用者和系統管理員就必須處理使用密碼來進行使用者驗證和授權的工作。 隨著時間過去，先進的密碼儲存及使用方式已經有所進步 (例如安全雜湊、Salt)，但我們仍面臨了兩個問題。 要複製密碼很容易，要竊取密碼也很容易。 此外，實作錯誤可能會使它們變得不安全，且使用者會很難在便利性與安全性之間取得平衡。

## <a name="111-credential-theft"></a>1.1.1 認證竊取


使用密碼最大的風險很簡單，就是攻擊者能輕鬆地竊取密碼。 每個輸入、處理或儲存密碼的位置都很容易受到攻擊。 舉例來說，攻擊者可以從驗證伺服器竊取一組密碼或雜湊，方法有竊聽傳送至應用程式伺服器的網路傳輸、在應用程式或裝置中置入惡意程式碼、記錄使用者在裝置上的按鍵動作，或是觀察使用者輸入了哪些字元。 這些都還只是最常見的攻擊方法而已。

此外，另一個相關的風險就認證重新執行，也就是攻擊者藉由竊聽不安全的網路來取得有效的認證，然後稍後重新執行該認證來模仿有效的使用者。 大部分的驗證通訊協定 (包括 Kerberos 和 OAuth) 都會在認證交換過程中納入時間戳記來提供保護，以防止重新執行的攻擊，但這種方法只保護由驗證系統發出的權杖，而非使用者一開始為取得票證所提供的密碼。

## <a name="112-credential-reuse"></a>1.1.2 認證重複使用




把電子郵件地址當做使用者名稱的常見作法，只會讓問題雪上加霜。 如果攻擊者成功從遭破解的系統中取得使用者名稱/密碼組，接著就能在其他系統上嘗試使用該資料。 令人驚訝的是，這種方法通常能讓攻擊者從遭破解的系統跳到其他的系統上。 而把電子郵件地址當做使用者名稱，會導致其他問題 (我們稍後將在本指南探討)。

## <a name="12-solving-credential-problems"></a>1.2 解決認證問題


要解決密碼造成的問題，需要一些技巧。 強化密碼使用原則並無法解決問題，因為使用者可能就只是重複使用、共用，或寫下密碼。 雖然驗證安全性相關的使用者教育是非常重要的，但僅僅只是教育，並無法消除這個問題。

Windows Hello 以增強式雙因素驗證 (2FA) 取代密碼，方法是驗證現有的認證，以及建立以生物識別或 PIN 式使用者手勢所保護的裝置特定認證。 


## <a name="2-what-is-windows-hello"></a>2 什麼是 Windows Hello？


Windows Hello 是 Microsoft 提供給 Windows10 內建的新生物特徵辨識登入系統的名稱。 因為此功能是直接內建於作業系統，所以 Windows Hello 讓使用者能夠使用臉部或指紋辨識來解除鎖定使用者的裝置。 當使用者提供了自己的唯一生物特徵辨識識別碼來存取裝置特定的認證時即會進行驗證，這表示除非竊取裝置的攻擊者具備 PIN，否則該攻擊者無法登入裝置。 Windows 安全的認證存放區會保護裝置上的生物特徵辨識資料。 使用 Windows Hello 解除鎖定裝置之後，授權的使用者就能取得自己的所有 Windows 體驗、App、資料、網站及服務的存取權。

Windows Hello 的驗證器被稱為 Hello。 Hello 對於單一裝置與特定使用者的組合來說是唯一的， 它不會在裝置之間漫遊、不會與伺服器或呼叫中的應用程式分享，也無法輕易從裝置中取出。 如果有多位使用者共用一個裝置，每位使用者都需要設定自己的帳戶， 而每個帳戶都會取得該裝置專有的唯一 Hello。 您可以把 Hello 想像成可用來解除鎖定 (或釋放) 已儲存認證的權杖。 Hello 本身不會對應用程式或服務驗證您的身分，但它釋放認證可以這麼做。 換句話說，Hello 不是使用者認證，但它是驗證程序可使用的第二因素。

## <a name="21-windows-hello-authentication"></a>2.1 Windows Hello 驗證


Windows Hello 可為裝置提供健全的方式來辨識個別使用者，這也就是使用者與所要求服務或資料項目之間路徑的第一個部分。 裝置在成功辨識使用者之後，仍然需要驗證使用者的身分，才能決定是否要授與所要求資源的存取權。 Windows Hello 提供與 Windows 完全整合的增強式 2FA，利用特定裝置和生物特徵辨識技術或 PIN 的組合，來取代可重複使用的密碼。

不過，Windows Hello 不只是用來取代傳統的 2FA 系統。 它在概念上與智慧卡類似：使用密碼編譯基本類型來執行驗證，而非使用字串比較，且使用者的金鑰內容安全地存放無法竄改的硬體中。 Windows Hello 不需要智慧卡部署所需的額外基礎結構元件。 特別的是，您並不需要公開金鑰基礎結構 (PKI) 來管理憑證 (如果您目前沒有 PKI)。 Windows Hello 結合智慧卡的主要優點，有虛擬智慧卡的部署彈性及實體智慧卡的強大安全性，卻沒有兩者的任何缺點。

## <a name="22-how-windows-hello-works"></a>2.2 Windows Hello 的運作方式


當使用者在自己的電腦上設定 Windows Hello 之後，它會在裝置上產生新的公開/私密金鑰組。 [信賴平台模組](https://technet.microsoft.com/itpro/windows/keep-secure/trusted-platform-module-overview) (TPM) 會產生並保護這個私密金鑰。 如果裝置沒有 TPM 晶片，私密金鑰則由軟體來加密並保護。 此外，已啟用 TPM 的裝置會產生可用來證明金鑰已繫結至 TPM 的資料區塊。 舉例來說，此證明資訊可以用在您的解決方案中，來決定是否要授與使用者不同的授權層級。

如要啟用裝置上的 Windows Hello，使用者在 Windows 設定中必須已與 Azure Active Directory 帳戶連接，或是已與 Microsoft 帳戶連接。

## <a name="221-how-keys-are-protected"></a>2.2.1 保護金鑰的方式


在任何時間產生金鑰內容時，都必須提供保護來防止攻擊。 而保護金鑰最好的方式，就是透過專用的硬體。 人類在使用硬體安全性模組 (HSM) 來產生、儲存及處理適用於安全性關鍵應用程式的金鑰方面，有很長的歷史。 智慧卡是一種特殊類型的 HSM，它們都是符合信賴運算群組 TPM 標準規範的裝置。 如果可行，Windows Hello 實作會充分利用內建的 TPM 硬體來產生、儲存及處理金鑰。 不過，Windows Hello 和 Windows Hello 工作不需要將 TPM 上線。

但 Microsoft 會在任何合適的時機建議您使用 TPM 硬體。 TPM 會提供保護來對抗各種已知及潛在的攻擊，包括 PIN 暴力密碼破解攻擊。 TPM 在帳戶鎖定之後，也會提供額外的保護。 當 TPM 鎖定金鑰內容時，使用者必須重設 PIN。 重設 PIN 表示將移除所有使用舊的金鑰內容加密的金鑰與憑證。

## <a name="222-authentication"></a>2.2.2 驗證


當使用者想要存取受保護的金鑰內容時，驗證程序首先會要求使用者輸入 PIN 或生物特徵辨識手勢來解除裝置的鎖定，這個程序有時也稱為「釋出金鑰」。

應用程式永遠無法使用另一個應用程式的金鑰，而使用者也永遠無法使用另一位使用者的金鑰。 當有尋求特定資源存取權的要求傳送到識別提供者 (或 IDP) 時，這些金鑰可用來簽署該要求。 應用程式可以使用特定的 API，來要求需要適用於特定行動之金鑰內容的操作。 透過這些 API 要求的存取權確實需要透過使用者手勢進行明確的驗證，且不會向提出要求的應用程式公開金鑰內容。 更確切地說，應用程式會要求進行特定的動作 (例如簽署資料片段)，而 Windows Hello 層級會處理實際的工作，並傳回結果。

## <a name="23-getting-ready-to-implement-windows-hello"></a>2.3 準備實作 Windows Hello


現在我們對 Windows Hello 的運作方式已經有基本的了解，那就來看看如何在自己的應用程式中使用它們。

我們有幾個可使用 Windows Hello 來實作的不同案例。 例如，就只是登入您裝置上的應用程式而已。 另一個常見的案例，就是向服務進行驗證。 您將不會使用登入名稱和密碼，而是使用 Windows Hello。 在下列章節中，我們將討論幾個不同案例的實作，包括如何使用 Windows Hello 向您的服務進行驗證，以及如何將現有的登入名稱和密碼系統，轉換成使用 Windows Hello 的系統。

最後，請注意 Windows Hello API 需要使用的 Windows 10 SDK，必須符合將來執行應用程式的作業系統。 換句話說，針對將會部署到 Windows 10 的應用程式，您必須使用 10.0.10240 Windows SDK；而針對將會部署到 Windows 10 版本 1511 的應用程式，您必須使用 10.0.10586 Windows SDK。

## <a name="3-implementing-windows-hello"></a>3 實作 Windows Hello


在此章節中，我們將從沒有現存驗證系統的嶄新案例開始，然後我們將說明如何實作 Windows Hello。

下一個章節的內容是如何從現有的使用者名稱/密碼系統移轉到新的系統。 不過，即使您對那個章節比較有興趣，建議您還是看看這個章節，以便對處理程序及所需程式碼有基本的了解。

## <a name="31-enrolling-new-users"></a>3.1 為新的使用者註冊


我們要從將會使用 Windows Hello 的全新服務開始，然後有個假想的新使用者準備要在新的裝置上註冊。

第一個步驟，就是確認該使用者能夠使用 Windows Hello。 應用程式會確認使用者的設定及電腦的功能，以確保它能夠建立使用者識別碼索引鍵。 如果應用程式確定使用者尚未啟用 Windows Hello，它就會提示使用者先安裝好，再使用該應用程式。

如要啟用 Windows Hello，使用者只需要在 Windows 設定中設定 PIN 即可，除非使用者已在全新體驗 (OOBE) 中設定完成。

下列幾行程式碼是查看使用者是否已設定 Windows Hello 的簡單方式。

```cs
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
if (!keyCredentialAvailable)
{
   // User didn't set up PIN yet
   return;
}
```

下一個步驟，則是要求使用者提供資訊，以便在您的服務中註冊。 您可以選擇要求使用者提供名字、姓氏、電子郵件地址，以及唯一的使用者名稱。 您也可以把電子郵件地址當做唯一識別碼，一切由您來決定。

在這個案例中，我們把電子郵件地址是當做該使用者的唯一識別碼。 當使用者註冊成功時，您應該要考慮傳送驗證電子郵件，以確認該電子郵件地址是有效的。 這個機制還能讓您在必要時重設帳戶。

如果使用者已經設定自己的 PIN，應用程式就能建立使用者的 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029)。 應用程式也會獲得選用的金鑰證明資訊，以便取得金鑰是在 TPM 上產生的密碼編譯證明。 產生的公開金鑰，以及選用的證明會傳送到後端伺服器，來為使用者所用的裝置註冊。 在每個裝置上所產生的金鑰組都是唯一的。

以下是建立 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029) 的程式碼範例：

```cs
var keyCreationResult = await KeyCredentialManager
    .RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

[**RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) 是建立公開金鑰和私密金鑰的部分。 如果裝置有正確的 TPM 晶片，API 會要求 TPM 晶片建立私密和公開金鑰金鑰並儲存結果；如果使用沒有 TPM 晶片，作業系統會在程式碼中建立金鑰組。 沒有方法能讓應用程式直接存取建立的私密金鑰。 建立金鑰組的過程也會產生證明資訊。 (如需證明的詳細資訊，請參閱下一節。)

當金鑰組和證明資訊在裝置上建立之後，公開金鑰、選用的證明資訊，以及唯一識別碼 (例如電子郵件地址) 必須傳送至後端註冊服務，並儲存在後端。

如要讓使用者能夠在多個裝置上存取應用程式，後端服務就必須能夠為同一位使用者儲存多個金鑰。 因為每個金鑰對每個裝置而言都是唯一的，我們會儲存與同一位使用者相關的所有金鑰。 在驗證使用者時，裝置識別碼的使用有助於讓伺服器端的驗證過程最佳化。 我們將在下一章深入討論這一點。

在後端儲存此資訊的範例資料庫結構描述看起來可能像這樣：

![Windows Hello 範例資料庫結構描述](images/passport-db.png)

註冊邏輯看起來可能會像這樣：

![Windows Hello 註冊邏輯](images/passport-registration.png)

當然，您所收集註冊資訊中的身分辨識資訊，可能比我們在這簡單案例中包含的多出許多。 舉例來說，如果您的應用程式存取某個受保護的服務 (例如銀行服務)，您必須要求身分識別證明及其他項目，做為註冊程序的一部分。 當所有條件都符合之後，這位使用者的公開金鑰將會儲存在後端，讓該使用者下次使用服務時可用來進行驗證。

```cs
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Credentials;

static async void RegisterUser(string AccountId)
{
    var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
    if (!keyCredentialAvailable)
    {
        // The user didn't set up a PIN yet
        return;
    }

    var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
    if (keyCreationResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = keyCreationResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttestationIncluded = false;
        bool keyAttestationCanBeRetrievedLater = false;

        keyAttestationResult = await userKey.GetAttestationAsync();
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLater = true;
        }
    }
    else if (keyCreationResult.Status == KeyCredentialStatus.UserCanceled ||
        keyCreationResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        // Show error message to the user to get confirmation that user
        // does not want to enroll.
    }
}
```

## <a name="311-attestation"></a>3.1.1 證明


在建立金鑰組時，還可以選擇要求由 TPM 晶片產生的證明資訊。 這個選用的資訊可以傳送到伺服器，做為註冊程序的一部分。 TPM 金鑰證明是能以密碼編譯方式證明金鑰與 TPM 繫結的通訊協定。 這種類型的證明可用來保證，某些密碼編譯作業確實曾發生在特定電腦的 TPM 中。

當伺服器收到產生的 RSA 金鑰、證明聲明，以及 AIK 憑證時，它需要確認下列條件：

-   AIK 憑證簽章是有效的。
-   AIK 憑證鏈結至受信任的根。
-   AIK 憑證和其鏈結啟用「EKU OID 2.23.133.8.3」(易記名稱為「證明識別金鑰憑證」)。
-   AIK 憑證是有效的時限。
-   鏈結中所有簽發的 CA 憑證都是有效的時限而且未被撤銷。
-   證明聲明的格式正確。
-   [**KeyAttestation**](https://msdn.microsoft.com/library/windows/apps/dn298288) blob 上的簽章使用 AIK 公開金鑰。
-   [**KeyAttestation**](https://msdn.microsoft.com/library/windows/apps/dn298288) blob 中的公開金鑰，符合用戶端連同證明聲明一起傳送的公開 RSA 金鑰。

根據以上這些條件，您的應用程式可能會把不同的授權層級指派給使用者。 舉例來說，如果這些檢查項目中有一個檢查失敗，應用程式可能不會讓使用者註冊，或是可能會限制使用者能執行的功能。

## <a name="32-logging-on-with-windows-hello"></a>3.2 使用 Windows Hello 登入


使用者在您的系統中註冊之後，就能夠使用應用程式。 視情況而定，您可以要求使用者必須先經過驗證才能開始使用應用程式，或是只要求使用者在開始使用您的後端服務時才進行驗證。

## <a name="33-force-the-user-to-sign-in-again"></a>3.3 強迫使用者再次登入


在某些情況下，您可能會想要求使用者在存取應用程式之前，或是有時在您的應用程式中進行特定動作之前，證明自己的確就是目前登入的那個人。 舉例來說，在銀行應用程式傳送轉帳命令給伺服器之前，您想要確定這的確是正確的使用者，而不是某個找到已登入裝置，且嘗試要執行交易的人。 您可以使用 [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134) 來強迫使用者再次登入您的應用程式。 下列程式碼會強迫使用者輸入自己的認證。

下列程式碼會強迫使用者輸入自己的認證。

```cs
UserConsentVerificationResult consentResult = await UserConsentVerifier.RequestVerificationAsync("userMessage");
if (consentResult.Equals(UserConsentVerificationResult.Verified))
{
   // continue
}
```

當然，您也可以從伺服器使用查問回應機制，要求使用者輸入自己的 PIN 碼或生物特徵辨識認證。 這完全根據身為開發人員的您需要實作的案例而定。 下一節將會說明這種機制。

## <a name="34-authentication-at-the-backend"></a>3.4 在後端進行驗證


當應用程式想要存取受保護的後端服務時，服務會傳送查問給應用程式。 應用程式會利用使用者的私密金鑰來簽署查問，然後將查問傳送回伺服器。 因為伺服器已儲存使用者的公開金鑰，它會使用標準的密碼編譯 API，來確定訊息確實是以正確的私密金鑰簽署的。 在用戶端上，簽署是由 Windows Hello API 來完成的；開發人員永遠沒有任何使用者私密金鑰的存取權。

除了檢查金鑰之外，服務也會檢查金鑰證明，並判斷是否有因為金鑰在裝置上的儲存方式所產生的任何限制。 舉例來說，當裝置使用 TPM 來保護金鑰時，會比不用 TPM 儲存金鑰的裝置更加安全。 後端邏輯可以決定，舉例來說，當裝置沒有使用 TPM，使用者有轉帳金額的限制，以便降低風險。

只有擁有 TPM 晶片版本 2.0 或更新版本的裝置才有證明。 因此，您必須考量並非每個裝置都有這項資訊。

下圖為用戶端工作流程的範例：

![Windows Hello 用戶端的工作流程](images/passport-client-workflow.png)

當 app 呼叫在後端的服務時，伺服器會傳送查問。 查問可以用下列程式碼來簽署：

```cs
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

if (openKeyResult.Status == KeyCredentialStatus.Success)
{
    var userKey = openKeyResult.Credential;
    var publicKey = userKey.RetrievePublicKey();
    var signResult = await userKey.RequestSignAsync(message);
    
    if (signResult.Status == KeyCredentialStatus.Success)
    {
        return signResult.Result;
    }
    else if (signResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        
    }
}
```

第一行的 [**KeyCredentialManager.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn973046) 會要求作業系統開啟金鑰控制代碼。 如果成功完成，您就能利用 [**KeyCredential.RequestSignAsync**](https://msdn.microsoft.com/library/windows/apps/dn973058) 方法來簽署查問訊息，這會讓作業系統透過 Windows Hello 來要求使用者提供 PIN 或生物特徵辨識資料。 開發人員永遠沒有使用者私密金鑰的存取權。 這整個過程會透過 API 來保持資料的安全。

API 會要求作業系統利用私密金鑰來簽署查問。 然後系統會要求使用者提供 PIN 碼，或已設定的生物特徵辨識登入資料。 當使用者輸入正確的資訊時，系統可以要求 TPM 晶片執行密碼編譯函式，並簽署查問 (或如果沒有 TPM 可用，則使用後援軟體解決方案)。 用戶端必須將已簽署的查問傳送回伺服器。

下列圖表為基本的查問/回應流程：

![Windows Hello 挑戰回應](images/passport-challenge-response.png)

接下來，伺服器必須驗證簽章。 當您要求公開金鑰並傳送至伺服器以供未來驗證之用時，則是 ASN.1 編碼的 publicKeyInfo Blob。如果您查看 [GitHub 上的 Windows Hello 程式碼範例](http://go.microsoft.com/fwlink/?LinkID=717812)，您會看到有用來包裝 Crypt32 函式的協助程式類別，以將 ASN.1 編碼的 Blob 轉譯成較常使用的 CNG Blob。 該 Blob 包含公開金鑰演算法，也就是 RSA 和 RSA 公開金鑰。

當您擁有 CNG Blob 之後，就必須根據該使用者的公開金鑰，來驗證已簽署的查問。 由於每個人都使用自己的系統或後端技術，因此沒有任何常用來實作該邏輯的方法。 我們使用 SHA256 做為雜湊演算法，並針對 SignaturePadding 使用 Pkcs1，因此當您驗證來自用戶端的已簽署回應時，請確定那就是您所使用的項目。 同樣地，請參考範例以尋找在 .NET 4.6 中於您的伺服器上執行的方式，但通常來說會類似：

```cs
using (RSACng pubKey = new RSACng(publicKey))
{
   retval = pubKey.VerifyData(originalChallenge, responseSignature,  HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1); 
}
```

我們讀取已儲存的公開金鑰，而它是 RSA 金鑰。 我們利用公開金鑰來驗證已簽署的查問訊息；如果驗證成功，我們就會授權給使用者。 如果使用者通過驗證，應用程式就能照平常的方式呼叫後端服務。

以下為完整的程式碼範例：

```cs
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Security.Credentials;

static async Task<IBuffer> GetAuthenticationMessageAsync(IBuffer message, String AccountId)
{
    var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

    if (openKeyResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = openKeyResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var signResult = await userKey.RequestSignAsync(message);
        if (signResult.Status == KeyCredentialStatus.Success)
        {
            return signResult.Result;
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // Launch app-specific flow to handle the scenario 
            return null;
        }
    }
    else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
    {
        // PIN reset has occurred somewhere else and key is lost.
        // Repeat key registration
        return null;
    }
    else
    {
        // Show custom UI because unknown error has happened.
        return null;
    }
}
```

實作正確的查問/回應機制不在本文的討論範圍，但為了要建立安全的機制來防止重新執行或攔截式攻擊等狀況，這個主題是需要多加留意的。

## <a name="35-enrolling-another-device"></a>3.5 為另一個裝置註冊


使用者擁有多個裝置，但都安裝了相同的應用程式，在今日是很普遍的現象。 搭配多個裝置來使用 Windows Hello 時，會如何運作呢？

當您使用 Windows Hello 時，每個裝置都會建立唯一的私密和公開金鑰組。 這代表如果您想讓使用者能夠使用多個裝置，您的後端必須能夠為該使用者儲存多個公開金鑰。 如需資料表結構的範例，請參閱 2.1 節的資料庫圖表。

為另一個裝置註冊的方法，幾乎就跟使用者第一次註冊時的方法相同。 但您仍然必須確認為新裝置註冊的使用者，真的就是該使用者所宣稱的那個人。 您可以使用目前任何一種雙因素驗證機制來確認這件事。 要安全地完成這項作業的方式有好幾種， 這完全取決於您的情況。

舉例來說，如果您仍然使用登入名稱和密碼，您可以使用該資料來驗證使用者的身分，並要求對方使用其中一種驗證方法，例如簡訊或電子郵件。 如果您沒有登入名稱和密碼，您也可以利用某個已註冊的裝置，並將通知傳送給該裝置上的應用程式。 MSA 驗證器應用程式就是使用這種方法的例子。 簡單地說，您應該使用常見的 2FA 機制，來為該使用者額外的裝置註冊。

註冊新裝置所用的程式碼，就跟 (從應用程式內) 第一次為使用者註冊所用的程式碼完全相同。

```cs
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

為了讓使用者能更輕易地辨識哪個裝置已經註冊，您可以選擇在註冊過程中傳送裝置名稱或另一個識別碼。 這也是很有用的方法；舉例來說，如果您想在後端實作可以讓使用者在遺失裝置時能取消註冊裝置的服務，這方法就很有用。

## <a name="36-using-multiple-accounts-in-your-app"></a>3.6 在您的應用程式中使用多個帳戶


除了支援在多個裝置上擁有單一帳戶之外，支援在單一應用程式中使用多個帳戶的情況也是很常見的。 舉例來說，或許您在應用程式中連線到多個 Twitter 帳戶。 只要使用 Windows Hello，您就能建立多個金鑰組，以及支援在您的應用程式中使用多個帳戶。

而方法之一，就是把我們在上一章中說明的使用者名稱或唯一識別碼，儲存在隔離的儲存體中。 因此，當您每次建立新帳戶時，就會把帳戶識別碼儲存在隔離的儲存體中。

在應用程式的使用者介面中，您可以讓使用者選擇使用某個先前建立的帳戶，或是註冊新的帳戶。 建立新帳戶的流程，跟我們之前說明的相同。 而選擇某個帳戶，就是在螢幕上列出儲存的帳戶清單。 當使用者選取帳戶之後，請使用帳戶識別碼讓該使用者登入您的應用程式：

```cs
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);
```

流程剩下的部分，跟我們之前說明的相同。 明確地說，所有這些帳戶都會被相同的 PIN 或生物特徵辨識手勢保護，因為在這個案例中，這些帳號全都搭配相同的 Windows 帳戶在單一裝置上使用。

## <a name="4-migrating-an-existing-system-to-windows-hello"></a>4 將現有的系統移轉到 Windows Hello


在簡短的這一節中，我們會提到某個現有的通用 Windows 平台應用程式，以及使用資料庫來儲存使用者名稱及雜湊密碼的後端系統。 當應用程式啟動時，這些應用程式就會收集使用者的認證，並在後端系統傳回驗證查問時使用這些認證。

我們將在這裡說明，需要變更或取代哪些部分，才能讓 Windows Hello 運作。

我們已經在先前的章節中說明大多數的技術。 將 Windows Hello 新增到您現有的系統，會牽涉在您程式碼的註冊和驗證部分，新增幾個不同的流程。

有個方法是讓使用者選擇何時升級。 當使用者登入應用程式，且您偵測到該應用程式及作業系統都能夠支援 Windows Hello 之後，您可以詢問使用者是否想要升級認證，來使用這個更現代化、更安全的系統。 您可以使用下列程式碼，來查看使用者是否能夠使用 Windows Hello。

```cs
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
```

UI 看起來如下：

![Windows Hello UI](images/passport-ui.png)

如果使用者選擇開始使用 Windows Hello，您就要建立之前所述的 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029)。 後端的註冊伺服器會把公開金鑰和選用的證明聲明新增到資料庫中。 因為使用者已經利用使用者名稱及密碼驗證自己的身分，伺服器可以讓新的認證與資料庫中目前的使用者資訊建立連結。 資料庫模型可能會與先前所述的範例相同。

如果應用程式能夠建立使用者的 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029)，它會把使用者識別碼儲存在隔離儲存體，讓使用者在應用程式再次啟動時，能夠從清單中選擇這個帳戶。 從這一點開始，流程就跟之前章節中所述的範例完全一樣。

移轉至完全使用 Windows Hello 的最後一個步驟，就是停用應用程式中登入名稱和密碼的選項，並移除儲存在資料庫中的雜湊密碼。

## <a name="5-summary"></a>5 總結


Windows 10 引進較高的安全性層級，實行方法也很簡單。 Windows Hello 提供新的生物特徵識別登入系統來辨識使用者，以及主動防禦規避適當身分識別程序的攻擊。 然後會提供多層按鍵及不可以洩露或在信賴平台模組外部使用的憑證。 此外，您可以選擇使用證明識別金鑰和憑證，來進一步提高安全性。

身為開發人員的您，可以使用這個指導方針來設計和部署這些技術，讓您能輕鬆地為您的 Windows 10 首度發行新增安全驗證，來保護應用程式與後端服務。 您需要用到的程式碼很少，而且容易了解。 繁重的工作就讓 Windows 10 來做吧。

有彈性的實作選項，能讓 Windows Hello 取代或配合您現有的驗證系統。 部署體驗是輕鬆且經濟的。 部署 Windows 10 安全性不需要任何額外的基礎結構。 內建 Microsoft Hello 的 Windows 10 作業系統，為當代開發人員所面臨的驗證問題，提供了最安全的解決方案。

任務完成了！ 您讓網際網路成為更安全的地方！

## <a name="6-resources"></a>6 資源


### <a name="61-articles-and-sample-code"></a>6.1 文章與範例程式碼

-   [Windows Hello 概觀](http://windows.microsoft.com/windows-10/getstarted-what-is-hello)
-   [適用於 Windows Hello 的實作詳細資料](https://msdn.microsoft.com/library/mt589441)
-   [GitHub 上的 Windows Hello 程式碼樣本](http://go.microsoft.com/fwlink/?LinkID=717812)

### <a name="62-terminology"></a>6.2 詞彙

|                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AIK                 | 證明識別金鑰是用來提供密碼編譯證明 (TPM 金鑰證明)，方法是簽署不可移轉金鑰的屬性，然後將屬性和簽章提供給信賴憑證者來進行驗證。 所產生的簽章稱為「證明聲明」。 因為簽章是使用 AIK 私密金鑰 (只能用在建立它的 TPM 中) 所建立的，信賴憑證者可以信任證明金鑰是真的無法移轉，且無法在該 TPM 以外的地方使用。 |
| AIK 憑證     | AIK 憑證用來證明 AIK 存在 TPM 內。 它也用來證明經過 AIK 認證的其他金鑰源自於該特定 TPM。                                                                                                                                                                                                                                                                                                                                              |
| IDP                 | IDP 是識別提供者。 其中一個例子，就是 Microsoft 為 Microsoft 帳戶所建立的 IDP。 應用程式每次需要利用 MSA 來驗證時，可以呼叫 MSA IDP。                                                                                                                                                                                                                                                                                                                                        |
| PKI                 | 公開金鑰基礎結構，通常用來指向組織本身裝載的環境，且負責建立金鑰和撤銷金鑰等。                                                                                                                                                                                                                                                                                                                                                           |
| TPM                 | 信賴平台模組 (TPM) 可用來建立密碼編譯的公開/私密金鑰組，讓私密金鑰無法在 TPM 以外的地方顯示或使用 (亦即金鑰是不可移轉的)。                                                                                                                                                                                                                                                                                                               |
| TPM 金鑰證明 | 以密碼編譯的方式證明金鑰與 TPM 繫結的通訊協定。 這種類型的證明可用來保證，某些密碼編譯作業確實曾發生在特定電腦的 TPM 中                                                                                                                                                                                                                                                                                                                       |

 

## <a name="related-topics"></a>相關主題

* [Windows Hello 登入應用程式](microsoft-passport-login.md)
* [Windows Hello 登入服務](microsoft-passport-login-auth-service.md)