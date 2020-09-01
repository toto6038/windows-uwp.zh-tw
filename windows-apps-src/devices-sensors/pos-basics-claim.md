---
title: PointOfService 裝置宣告和啟用模型
description: 使用服務裝置宣告和啟用 Api 來宣告裝置，並啟用它們以進行 i/o 作業。
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 391acd1f4acf0620a87e77f3c540c3dd0faa6def
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165402"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>服務裝置宣告和啟用模型的點

## <a name="claiming-for-exclusive-use"></a>索取專屬用途

在您成功建立 PointOfService 裝置物件之後，您必須先使用適合裝置類型的宣告方法來宣告它，您才能使用裝置來進行輸入或輸出。  宣告授與應用程式專屬存取許多裝置的功能，以確保某個應用程式不會干擾其他應用程式使用裝置。  一次只有一個應用程式可以宣告 PointOfService 裝置專屬使用。 

> [!Note]
> 宣告動作會建立裝置的獨佔鎖定，但不會使其進入操作狀態。  如需詳細資訊，請參閱 [啟用裝置的 i/o 作業](#enable-device-for-io-operations) 。

### <a name="apis-used-to-claim--release"></a>用來宣告/發行的 Api

|裝置|宣告 | 版本 | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner. ClaimScannerAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner。關閉](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer。關閉](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay。關閉](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader。關閉](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter。關閉](/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>啟用裝置以進行 i/o 作業

宣告動作只會建立裝置的獨佔許可權，但不會使其進入操作狀態。  為了接收事件或執行任何輸出作業，您必須使用 **EnableAsync**來啟用裝置。  相反地，您可以呼叫 **DisableAsync** 停止接聽裝置的事件，或執行輸出。  您也可以使用 **IsEnabled** 來判斷裝置的狀態。

### <a name="apis-used-enable--disable"></a>使用的 Api 啟用/停用

| 裝置 | 啟用 | 停用 | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | 不適用¹¹ | 不適用¹¹ | 不適用¹¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹行顯示不需要您明確啟用裝置的 i/o 作業。  啟用會由執行 i/o 的 PointOfService LineDisplay Api 自動執行。

## <a name="code-sample-claim-and-enable"></a>程式碼範例：宣告和啟用

這個範例顯示如何在您成功建立條碼掃描器物件之後，宣告條碼掃描器裝置。

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> 宣告可能在以下情況遺失：
> 1. 其他應用程式已要求宣告相同裝置，而您的應用程式並未發出 **RetainDevice** 以回應 **ReleaseDeviceRequested** 事件。  (如需詳細資訊，請參閱以下[宣告交涉](#claim-negotiation)。)
> 2. 您的應用程式已暫停，它會導致裝置物件關閉，因此宣告不再有效。 (如需詳細資訊，請參閱[裝置物件週期](pos-basics-deviceobject.md#device-object-lifecycle)。


## <a name="claim-negotiation"></a>宣告交涉

由於 Windows 是一個多工環境，它可讓相同電腦上的多個應用程式以合作方式要求存取周邊。  PointOfService API 提供交涉模型，允許多個應用程式共用連接到電腦的周邊。

當相同的電腦上的第二個應用程式要求宣告 PointOfService 周邊，而其他應用程式已經宣告該周邊，則會發佈 **ReleaseDeviceRequested** 事件通知。 如果應用程式目前使用裝置以避免遺失宣告，使用具主動式宣告的應用程式必須透過呼叫 **RetainDevice** 來回應事件通知。 

如果具主動式宣告的應用程式未立即回應 **RetainDevice**，則可能應用程式已暫停，或者不需要裝置，而且宣告撤銷，並提供給新的應用程式。 

第一個步驟是建立事件處理常式，以使用**RetainDevice**回應**ReleaseDeviceRequested**事件。  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

然後註冊事件處理常式與您宣告的裝置關聯

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
```



### <a name="apis-used-for-claim-negotiation"></a>用於宣告交涉的 API

|已宣告的裝置|發行通知| 保留裝置 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|