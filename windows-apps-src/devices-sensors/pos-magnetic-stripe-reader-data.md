---
author: eliotcowley
title: 取得並了解磁條資料
description: 了解如何取得並解譯從磁條讀取資料。
ms.author: elcowle
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10，uwp，點的服務、 pos、 磁條讀取器
ms.localizationpriority: medium
ms.openlocfilehash: a130243fb5a77277e4e8a326316cd30bab2cd96e
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5760632"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>取得並了解磁條資料

一旦您設定好您的應用程式使用中[開始使用服務點](pos-basics.md)所述的步驟中您磁條讀取器，就可以開始從其取得資料。

## <a name="subscribe-to-datareceived-events"></a>訂閱 * DataReceived 事件

每當讀卡機辨識對這個撥動的卡，才會引發其中三個事件：

* [AamvaCardDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)： 汽車卡撥動時，會發生。
* [BankCardDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)： 是銀行卡撥動時，會發生。
* [VendorSpecificDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived)： 廠商特定卡撥動時，會發生。

您的應用程式只需要訂閱磁條讀取器所支援的事件。 您可以看到哪些類型的卡片支援的[MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
)。

下列程式碼示範三種訂閱 ***DataReceived**事件：

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

[ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)和*引數*物件，其類型將會有所不同，事件，將會被傳遞的事件處理常式：

* **AamvaCardDataReceived**事件： [MagneticStripeReaderAamvaCardDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived**事件： [MagneticStripeReaderBankCardDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived**事件： [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>取得資料

**AamvaCardDataReceived**和**BankCardDataReceived**事件，您可以直接從*引數*物件取得的某些資料。 下列範例示範取得幾個屬性，並將它們指派給成員變數：

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

不過，部分資料，包括從**VendorSpecificDataReceived**事件時，所有資料必須擷取透過**報告**物件，這是*引數*參數的屬性。 這是類型[MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)。

您可以使用[CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype)屬性，以找出什麼類型的卡片有撥動，並再使用該資料來通知您如何解譯從[Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)、 [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)、 [Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)，以及[Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)資料。

每個播放軌中的資料會表示為[MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)物件。 從這個類別中，您可以取得下列類型的資料：

* [資料](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data)： 原始或解碼資料。
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata)： 判別資料。 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata)： 加密的資料。

下列程式碼片段取得報告和追蹤資料，並接著會檢查卡類型：

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>請參閱

* [磁條讀取器](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)