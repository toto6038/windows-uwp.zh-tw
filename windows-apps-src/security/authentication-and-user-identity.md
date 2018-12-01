---
title: 驗證和使用者識別
description: 通用 Windows 平台 (UWP) app 有數個使用者驗證選項，涵蓋範圍從使用 Web 驗證代理人的簡單單一登入 (SSO) 到高度安全的雙因素驗證。
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 3d23f54a371a883de8b56d03ddd153ab2d91c230
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8337314"
---
# <a name="authentication-and-user-identity"></a>驗證和使用者識別



通用 Windows 平台 (UWP) app 有數個使用者驗證選項，涵蓋範圍從使用 [Web 驗證代理人](web-authentication-broker.md)的簡單單一登入 (SSO) 到高度安全的雙因素驗證。

對於連線到第三方識別提供者服務 (例如 Facebook、Twitter 及 Flickr 等) 的一般 app 連線，請使用 [Web 驗證代理人](web-authentication-broker.md)。 如果您使用[認證保險箱](credential-locker.md)來儲存使用者的登入資訊，以及讓這些資訊漫遊，就能得到額外的便利性。

使用 windows 10 的企業應強烈考慮使用[Microsoft Passport 及 Windows Hello](microsoft-passport.md)，可讓高度安全的雙因素驗證。 如果您無法使用 Microsoft Passport，[智慧卡](smart-cards.md)和[指紋生物特徵辨識技術](fingerprint-biometrics.md)也能提供額外的安全性。

<table>
<tr><th>主題</th><th>描述</th></tr>
<tr><td><a href="credential-locker.md">認證保險箱</a></td><td>本文說明 App 如何使用認證保險箱來安全地儲存和擷取使用者認證，並透過使用者的 Microsoft 帳戶在裝置之間進行漫遊。</td></tr>

<tr><td><a href="fingerprint-biometrics.md">指紋生物識別技術</a> </td><td>本文將說明如何在您的應用程式中新增指紋生物識別技術。 包括使用者在進行特定動作時所必須同意的指紋驗證要求，以提高應用程式的安全性。 例如，您可以在授權 app 內購買之前或授與限制資源的存取權之前要求指紋驗證。 指紋驗證是使用 <a href="https://msdn.microsoft.com/library/windows/apps/hh701356">Windows.Security.Credentials.UI</a> 命名空間中的 <a href="https://msdn.microsoft.com/library/windows/apps/dn279134">UserConsentVerifier</a> 類別所管理。</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport 及 Windows Hello</a></td><td>這篇文章說明新的 Windows 10 Microsoft Passport 技術，並討論開發人員如何實作這項技術來保護自己的應用程式及後端服務。 文章強調該技術的幾個特定功能，以協助您減少傳統認證所帶來的威脅；它還提供指南來引導您設計及部署該技術，來做為您 Windows 10 首度發行的一部分。 </td></tr>
<tr><td><a href="microsoft-passport-login.md">建立 Microsoft Passport 登入應用程式</a></td><td>如何建立使用 Microsoft Passport 取代傳統使用者名稱及密碼驗證系統的 Windows 10 UWP(通用 Windows 平台) app 之完整逐步解說的第 1 部分。</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">建立 Microsoft Passport 登入服務</a></td><td>在 Windows 10 UWP (通用 Windows 平台) app 中使用 Microsoft Passport 取代傳統的使用者名稱及密碼驗證系統之完整逐步解說的第 2 部分。</td></tr>
<tr><td><a href="smart-cards.md">智慧卡</a></td><td>此主題說明應用程式如何使用智慧卡將使用者連接到安全的網路服務，包括如何存取實體智慧卡讀卡機、建立虛擬智慧卡、與智慧卡通訊、驗證使用者、重設使用者 PIN 和移除或中斷智慧卡的連線。</td></tr>
<tr><td><a href="share-certificates.md">在應用程式之間共用憑證</a></td><td>針對需要比使用者識別碼和密碼組合更安全之驗證方式的 UWP app，即可使用憑證驗證。 憑證驗證可在驗證使用者時提供高階的信任層級。 在某些情況下，會有一組服務想驗證多個 app 的某位使用者。 本文說明如何使用相同的憑證來驗證多個 App，以及如何提供便利的程式碼，讓使用者匯入用來存取受保護 Web 服務的憑證。</td></tr>
<tr><td><a href="companion-device-unlock.md">使用隨附 IoT 裝置的 Windows 解除鎖定</a></td><td>隨附裝置是可與您的 Windows 10 Desktop 搭配使用，以增強使用者驗證體驗的裝置。 透過隨附裝置架構，即使在 Windows Hello 無法使用時 (例如，如果 Windows 10 Desktop 缺少可進行臉部驗證的相機或指紋辨識器裝置)，隨附裝置還是可以提供豐富的 Microsoft Passport 體驗。</td></tr>
<tr><td><a href="web-account-manager.md">Web 帳戶管理員</a></td><td>本文章說明如何使用新的 Windows 10 Web 帳戶管理員 API 顯示 AccountsSettingsPane，並將您的通用 Windows 平台 (UWP) App 連接到外部身份識別提供者 (例如 Microsoft 或 Facebook)。 您將了解如何要求使用者的權限以使用其 Microsoft 帳戶，取得存取權杖，並利用它來執行基本操作 (例如取得個人檔案資料，或上傳檔案到他們的 OneDrive)。 </td></tr>
<tr><td><a href="web-authentication-broker.md">Web 驗證代理人</a></td><td>本文說明如何將您的 app 連接到使用授權通訊協定 (如 OpenID 或 OAuth) 的線上身分識別提供者，例如 Facebook、Twitter、Flickr、Instagram 等。 <a href="https://msdn.microsoft.com/library/windows/apps/br212066">AuthenticateAsync</a> 方法會將要求傳送到線上身分識別提供者，然後取得描述 app 存取之提供者資源的存取權杖。</td></tr>
</table>

