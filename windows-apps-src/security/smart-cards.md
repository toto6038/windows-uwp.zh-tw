---
title: 智慧卡
description: 本主題說明通用 Windows 平台 (UWP) 應用程式如何使用智慧卡將使用者連接到安全的網路服務，包括如何存取實體智慧卡讀卡機、建立虛擬智慧卡、與智慧卡通訊、驗證使用者、重設使用者 PIN 和移除或中斷智慧卡的連線。
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10 uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: ff17facbb22e4e336b302415b0b64d0dffda93e0
ms.sourcegitcommit: 68051d2adcf3facc036c9f4c76d82bb4d342a75f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2019
ms.locfileid: "67498890"
---
# <a name="smart-cards"></a>智慧卡




本主題說明通用 Windows 平台 (UWP) 應用程式如何使用智慧卡將使用者連接到安全的網路服務，包括如何存取實體智慧卡讀卡機、建立虛擬智慧卡、與智慧卡通訊、驗證使用者、重設使用者 PIN 和移除或中斷智慧卡的連線。 

## <a name="configure-the-app-manifest"></a>設定 app 資訊清單


您必須先在專案的 Package.appxmanifest 檔案中設定「共用使用者憑證」  功能，您的 app 才能驗證使用智慧卡或虛擬智慧卡的使用者。

## <a name="access-connected-card-readers-and-smart-cards"></a>存取連線的讀卡機與智慧卡


您可以透過將裝置識別碼 (在 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 中指定) 傳送至 [**SmartCardReader.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync) 方法，以查詢讀卡機和連接的智慧卡。 若要存取目前連接至傳回的讀卡機裝置的智慧卡，請呼叫 [**SmartCardReader.FindAllCardsAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync)。

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

您也應該透過實作一個方法來處理智慧卡插入時的 app 行為，以讓您的 app 觀察 [**CardAdded**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardadded) 事件。

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

接著，您可以將每個傳回的 [**SmartCard**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCard) 物件傳送至 [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning)，以存取讓您的應用程式存取和自訂其設定的方法。

## <a name="create-a-virtual-smart-card"></a>建立虛擬智慧卡


若要使用 [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 建立虛擬智慧卡，您的 app 首先需要提供一個易記的名稱、一個管理金鑰以及一個 [**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy)。 易記名稱通常會提供給 app，但您的 app 仍然需要提供管理金鑰並產生目前 **SmartCardPinPolicy** 的執行個體，才能將這三個值全部傳送至 [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync)。

1.  建立新的 [**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy) 執行個體
2.  在服務或管理工具提供的管理金鑰值上呼叫 [**CryptographicBuffer.GenerateRandom**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom)，以產生管理金鑰值。
3.  將這些值連同 *FriendlyNameText* 字串傳送至 [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync)。

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

在 [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) 傳回關聯的 [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 物件後，虛擬智慧卡便已佈建完成並可供使用。

>[!NOTE]
>若要建立虛擬智慧卡使用 UWP 應用程式，執行應用程式的使用者必須是 administrators 群組的成員。 如果使用者不是 administrators 群組的成員，虛擬智慧卡建立會失敗。

## <a name="handle-authentication-challenges"></a>因應驗證挑戰


若要使用智慧卡或虛擬智慧卡驗證，您的應用程式必須提供能在下列兩者之間完成挑戰的行為：智慧卡上儲存的管理金鑰資料，還有由驗證伺服器或管理工具維護的管理金鑰資料。

下列程式碼說明如何支援服務的智慧卡驗證或實體或虛擬智慧卡詳細資訊的修改。 如果在智慧卡上使用管理金鑰產生的資料 (所謂的「挑戰」) 與由伺服器或管理工具提供的管理金鑰資料 (「管理金鑰」) 相同，驗證就算成功。

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

您會在本主題的其餘部分看到此程式碼，因為我們會複習如何完成驗證動作，以及如何套用變更至智慧卡和虛擬智慧卡資訊。

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>驗證智慧卡或虛擬智慧卡驗證回應


我們在定義完驗證挑戰的邏輯之後，便可和讀卡機通訊以存取智慧卡，或是改為存取虛擬智慧卡，以進行驗證。

1.  若要開始挑戰，請從與智慧卡關聯的 [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 物件呼叫 [**GetChallengeContextAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync)。 這樣會產生 [**SmartCardChallengeContext**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext) 的執行個體，其包含智慧卡的 [**Challenge**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge) 值。

2.  接著，將智慧卡的挑戰值與服務或管理工具提供的管理金鑰，傳送至在上一個範例中定義的 **ChallengeResponseAlgorithm**。

3.  [**VerifyResponseAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync)會傳回 **，則為 true**如果驗證成功。

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>變更或重設使用者 PIN


變更與智慧卡關聯的 PIN：

1.  存取智慧卡並產生關聯的 [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 物件。
2.  呼叫 [**RequestPinChangeAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) 以向使用者顯示 UI 來完成此作業。
3.  如果 PIN 順利變更，呼叫將傳回 **true**。

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

要求重設 PIN：

1.  呼叫 [**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) 以起始作業。 此呼叫包含一個代表智慧卡和 PIN 重設要求的 [**SmartCardPinResetHandler**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler) 方法。
2.  [**SmartCardPinResetHandler** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler)提供的資訊，我們**ChallengeResponseAlgorithm**、 在包裝[ **SmartCardPinResetDeferral** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral)呼叫，用來比較卡片的挑戰值 」 和 「 服務 」 或 「 管理工具來驗證要求所提供的系統管理金鑰。

3.  如果挑戰成功，便會完成 [**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) 呼叫，如果 PIN 順利重設，則會傳回 **true**。

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>移除智慧卡或虛擬智慧卡


當實體智慧卡移除後，刪除智慧卡時便會觸發 [**CardRemoved**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved) 事件。

使用在智慧卡或讀卡機移除時將您的應用程式行為定義為事件處理常式的方法，將此事件的觸發與讀卡機建立關聯。 這個行為可以是簡單的行為，例如提供通知給智慧卡被移除的使用者。

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

虛擬智慧卡的移除是以程式設計方式來處理，先擷取智慧卡，再從 [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 傳回的物件呼叫 [**RequestVirtualSmartCardDeletionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync)。

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```
