---
title: 取得並了解磁條資料
description: 瞭解如何使用通用 Windows 平臺 (UWP) 服務點 (POS) Api，從磁性 stripe 讀取器取得和解讀資料。
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10、uwp、point of service、pos、磁性 stripe 讀取器
ms.localizationpriority: medium
ms.openlocfilehash: 2a405a66fbc243925c4c9bbd5b1ee499a9a7b9f8
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238273"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>取得並了解磁條資料

當您使用 [ [開始使用服務點](pos-basics.md)] 中所述的步驟，在應用程式中設定您的磁性 stripe 讀取器之後，您就可以開始從中取得資料。

## <a name="subscribe-to-datareceived-events"></a>訂閱 * DataReceived 事件

每當讀者辨識出撥動卡時，就會引發三個事件的其中一個：

* [AamvaCardDataReceived 事件](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)：當車輛車輛卡已撥動時，就會發生此事件。
* [BankCardDataReceived 事件](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)：撥動銀行卡片時發生。
* [VendorSpecificDataReceived 事件](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived)：撥動廠商專屬的卡片時，就會發生此事件。

您的應用程式只需要訂閱磁性 stripe 讀取器所支援的事件。 您可以看到 [MagneticStripeReader. SupportedCardTypes](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes)支援哪些類型的卡片。

下列程式碼示範如何訂閱三個 ***DataReceived** 事件：

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

系統會將 [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) 和自 *變數物件傳遞* 給事件處理常式，而該物件的類型會根據事件而有所不同：

* **AamvaCardDataReceived** 事件： [MagneticStripeReaderAamvaCardDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** 事件： [MagneticStripeReaderBankCardDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** 事件： [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>取得資料

針對 **AamvaCardDataReceived** 和 **BankCardDataReceived** 事件，您可以直接從 *args* 物件取得一些資料。 下列範例將示範如何取得一些屬性，並將它們指派給成員變數：

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

不過，某些資料（包括 **VendorSpecificDataReceived** 事件中的所有資料）必須透過 **報表** 物件（ *args* 參數的屬性）來抓取。 這是 [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)類型。

您可以使用 [CardType](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) 屬性來找出已撥動的卡片類型，然後使用該屬性來通知您如何解讀 [Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)、 [Track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)、 [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)和 [Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)中的資料。

每個追蹤的資料會以 [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) 物件表示。 從這個類別中，您可以取得下列資料類型：

* [Data](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data)：原始或已解碼的資料。
* [DiscretionaryData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata)：任意資料。 
* [A](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata)：加密的資料。

下列程式碼片段會取得報表和追蹤資料，然後檢查卡片類型：

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

## <a name="see-also"></a>另請參閱

* [磁條讀取器](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 類別](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)