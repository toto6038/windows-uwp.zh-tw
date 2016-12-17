---
title: "安全性"
description: "本節包含與建置適用於 Windows 10 的安全通用 Windows 平台 (UWP) 應用程式相關的文章。"
ms.assetid: 41E2EEFB-E8A9-4592-814C-72B703CD952C
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 82f6e2decde2d332bd08b0b9798350b973860f21
ms.openlocfilehash: b30492c3c74b19d5ce306829302be17ff303723f

---

# <a name="security"></a>安全性


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節包含與建置適用於 Windows 10 的安全通用 Windows 平台 (UWP) 應用程式相關的文章。

## <a name="introduction"></a>簡介 

如果您是 Windows 或 UWP 開發的新手，請先參閱[開發安全的 Windows 應用程式簡介](intro-to-secure-windows-app-development.md)。 這篇簡介等級的文章提供您應用程式的安全性考量，以及 Windows 10 中各種可用功能的概觀。

## <a name="authentication-and-user-identity"></a>驗證和使用者識別

[驗證和使用者識別](authentication-and-user-identity.md)一節包含與使用者登入和身分識別相關之案例的逐步解說。 App 有數個使用者驗證選項，涵蓋範圍從使用 [Web 驗證代理人](web-authentication-broker.md)的簡單單一登入 (SSO) 到高度安全的雙因素驗證。

<table>
<tr><th>主題</th><th>描述</th></tr>
<tr><td>[認證保險箱](credential-locker.md)</td><td>本文說明 App 如何使用認證保險箱來安全地儲存和擷取使用者認證，並透過使用者的 Microsoft 帳戶在裝置之間進行漫遊。</td></tr>

<tr><td>[指紋生物識別技術](fingerprint-biometrics.md) </td><td>本文將說明如何在您的應用程式中新增指紋生物識別技術。 包括使用者在進行特定動作時所必須同意的指紋驗證要求，以提高應用程式的安全性。 例如，您可以在授權 app 內購買之前或授與限制資源的存取權之前要求指紋驗證。 指紋驗證是使用 [Windows.Security.Credentials.UI](https://msdn.microsoft.com/library/windows/apps/hh701356) 命名空間中的 [UserConsentVerifier](https://msdn.microsoft.com/library/windows/apps/dn279134) 類別所管理。</td></tr>
<tr><td>[Microsoft Passport 及 Windows Hello](microsoft-passport.md)</td><td>這篇文章說明新的 Windows 10 Microsoft Passport 技術，並討論開發人員如何實作這項技術來保護自己的應用程式及後端服務。 文章強調該技術的幾個特定功能，以協助您減少傳統認證所帶來的威脅；它還提供指南來引導您設計及部署該技術，來做為您 Windows 10 首度發行的一部分。 </td></tr>
<tr><td>[建立 Microsoft Passport 登入應用程式](microsoft-passport-login.md)</td><td>如何建立使用 Microsoft Passport 取代傳統使用者名稱及密碼驗證系統的 Windows 10 UWP(通用 Windows 平台) app 之完整逐步解說的第 1 部分。</td></tr>
<tr><td>[建立 Microsoft Passport 登入服務](microsoft-passport-login-auth-service.md)</td><td>在 Windows 10 UWP (通用 Windows 平台) app 中使用 Microsoft Passport 取代傳統的使用者名稱及密碼驗證系統之完整逐步解說的第 2 部分。</td></tr>
<tr><td>[智慧卡](smart-cards.md)</td><td>此主題說明應用程式如何使用智慧卡將使用者連接到安全的網路服務，包括如何存取實體智慧卡讀卡機、建立虛擬智慧卡、與智慧卡通訊、驗證使用者、重設使用者 PIN 和移除或中斷智慧卡的連線。</td></tr>
<tr><td>[在應用程式之間共用憑證](share-certificates.md)</td><td>針對需要比使用者識別碼和密碼組合更安全之驗證方式的 UWP app，即可使用憑證驗證。 憑證驗證可在驗證使用者時提供高階的信任層級。 在某些情況下，會有一組服務想驗證多個 app 的某位使用者。 本文說明如何使用相同的憑證來驗證多個 App，以及如何提供便利的程式碼，讓使用者匯入用來存取受保護 Web 服務的憑證。</td></tr>
<tr><td>[使用隨附 IoT 裝置的 Windows 解除鎖定](companion-device-unlock.md)</td><td>隨附裝置是可與您的 Windows 10 Desktop 搭配使用，以增強使用者驗證體驗的裝置。 透過隨附裝置架構，即使在 Windows Hello 無法使用時 (例如，如果 Windows 10 Desktop 缺少可進行臉部驗證的相機或指紋辨識器裝置)，隨附裝置還是可以提供豐富的 Microsoft Passport 體驗。</td></tr>
<tr><td>[Web 帳戶管理員](web-account-manager.md)</td><td>本文章說明如何使用新的 Windows 10 Web 帳戶管理員 API 顯示 AccountsSettingsPane，並將您的通用 Windows 平台 (UWP) App 連接到外部身份識別提供者 (例如 Microsoft 或 Facebook)。 您將了解如何要求使用者的權限以使用其 Microsoft 帳戶，取得存取權杖，並利用它來執行基本操作 (例如取得個人檔案資料，或上傳檔案到他們的 OneDrive)。 </td></tr>
<tr><td>[Web 驗證代理人](web-authentication-broker.md)</td><td>本文說明如何將您的 app 連接到使用授權通訊協定 (如 OpenID 或 OAuth) 的線上身分識別提供者，例如 Facebook、Twitter、Flickr、Instagram 等。 [AuthenticateAsync](https://msdn.microsoft.com/library/windows/apps/br212066) 方法會將要求傳送到線上身分識別提供者，然後取得描述 App 存取之提供者資源的存取權杖。</td></tr>
</table>

## <a name="cryptography"></a>密碼編譯 

密碼編譯一節包含更複雜的密碼編譯相關主題資訊。 

| 主題                                                                         | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [憑證簡介](certificates.md)                                      | 本文討論應用程式中的憑證使用方式。 數位憑證用於公開金鑰密碼編譯，將公開金鑰繫結至個人、電腦或組織。 這種繫結身分常被用來在實體之間互相驗證。 例如，憑證通常是用來向使用者驗證網頁伺服器，或是向網頁伺服器驗證使用者。 您可以建立憑證要求並安裝或匯入已發出的憑證。 您也可以在憑證階層中註冊憑證。 |
| [密碼編譯金鑰](cryptographic-keys.md)                                   | 本文說明如何使用標準金鑰衍生函式來衍生金鑰，以及如何使用對稱和非對稱金鑰來加密內容。                                                                                                                                                                                                                                                                                                                                                                         |
| [資料保護](data-protection.md)                                         | 本文說明如何使用 [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空間中的 [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) 類別，來加密和解密 UWP app 中的數位資料。                                                                                                                                                                                                              |
| [MAC、雜湊以及簽章](macs-hashes-and-signatures.md)               | 本文討論如何在應用程式中使用訊息驗證碼 (MAC)、雜湊及簽章來偵測訊息是否遭竄改。                                                                                                                                                                                                                                                                                                                                                                                |
| [密碼編譯的出口限制](export-restrictions-on-cryptography.md) | 使用這項資訊判斷您的應用程式使用密碼編譯的方式，是否會阻止其列在 Windows 市集中。                                                                                                                                                                                                                                                                                                                                                                                                     |
| [常見的密碼編譯工作](common-cryptography-tasks.md)                     | 下列文章提供常見的密碼編譯工作範例程式碼，例如建立隨機數字、比較緩衝區、在字串與二進位資料間轉換、複製到位元組陣列和從位元組陣列中複製，以及編碼和解碼資料。                                                                                                                                                                                                                                                                                    |



<!--HONumber=Dec16_HO1-->


