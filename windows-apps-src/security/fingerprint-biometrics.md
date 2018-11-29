---
title: 指紋生物識別技術
description: 本文將說明如何將指紋生物識別技術新增到您的通用 Windows 平台 (UWP) 應用程式。
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 973091926ddff312b20002f7b535d34a3b7d2bc4
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193801"
---
# <a name="fingerprint-biometrics"></a>指紋生物識別技術




本文將說明如何將指紋生物識別技術新增到您的通用 Windows 平台 (UWP) 應用程式。 包括使用者必須同意特定動作時的指紋驗證要求，以增強 app 的安全性。 例如，您可以在授權 app 內購買之前或授與限制資源的存取權之前要求指紋驗證。 指紋驗證是使用 [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356) 命名空間中的 [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134) 類別所管理。

## <a name="check-the-device-for-a-fingerprint-reader"></a>檢查裝置是否有指紋辨識器


若要查明裝置是否具有指紋辨識器，請呼叫 [**UserConsentVerifier.CheckAvailabilityAsync**](https://msdn.microsoft.com/library/windows/apps/dn279138)。 即使裝置支援指紋驗證，您的 app 仍應在 [設定] 中為使用者提供啟用或停用指紋驗證的選項。

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>要求同意並傳回結果


若要要求使用者同意指紋掃描，請呼叫 [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139) 方法。 為了讓指紋驗證能運作，使用者必須先將指紋「簽章」加到指紋資料庫。

當您呼叫 [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139) 時，使用者會看到一個要求指紋掃描的強制回應對話方塊。 您可以提供一個訊息給 **UserConsentVerifier.RequestVerificationAsync** 方法，使用者會在強制回應對話方塊中看見該訊息，如下列影像所示。

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```