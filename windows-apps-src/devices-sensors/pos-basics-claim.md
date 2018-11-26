---
title: PointOfService 裝置宣告和啟用模型
description: 深入了解 PointOfService 宣告並啟用模型
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, point of service, pos, 服務點
ms.localizationpriority: medium
ms.openlocfilehash: 7169848084b587793ba1537ea3d6ad78d31892d5
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7698962"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>服務點裝置宣告和啟用模型

## <a name="claiming-for-exclusive-use"></a>宣告專屬使用

在您成功建立 PointOfService 裝置物件之後，您必須先使用適合裝置類型的宣告方法來宣告它，您才能使用裝置來進行輸入或輸出。  宣告授與應用程式專屬存取許多裝置的功能，以確保某個應用程式不會干擾其他應用程式使用裝置。  一次只有一個應用程式可以宣告 PointOfService 裝置專屬使用。 

> [!Note]
> 宣告動作建立獨佔到裝置，鎖定，但不會未將它放諸操作的狀態。  如需詳細資訊，請參閱[啟用裝置以用於 I/O 作業](#Enable-device-for-I/O-operations)。

### <a name="apis-used-to-claim--release"></a>Api，可用來宣告 / 發行

|裝置|宣告 | 版本 | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>啟用裝置以用於 I/O 作業

宣告動作只是建立在裝置專屬的權限，但不會未將它放諸操作的狀態。  為了接收事件，或執行任何輸出作業，您必須啟用使用**EnableAsync**的裝置。  相反地，您可以呼叫**DisableAsync**停止從裝置或執行輸出聆聽的事件。  您也可以使用**IsEnabled**來判斷您的裝置狀態。

### <a name="apis-used-enable--disable"></a>使用 Api 啟用/停用

| 裝置 | 啟用 | 停用 | IsEnabled？ |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | 不 Applicable¹ | 不 Applicable¹ | 不 Applicable¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasyc) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ 行顯示不需要明確啟用裝置以用於 I/O 作業。  啟用自動執行的可執行 I/O PointOfService LineDisplay Api。

## <a name="code-sample-claim-and-enable"></a>程式碼範例： 宣告和啟用

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
> 1. 其他應用程式已要求宣告相同裝置，而您的應用程式並未發出 **RetainDevice** 以回應 **ReleaseDeviceRequested** 事件。  (如需詳細資訊，請參閱以下[宣告交涉](#Claim-negotiation)。)
> 2. 您的應用程式已暫停，它會導致裝置物件關閉，因此宣告不再有效。 (如需詳細資訊，請參閱[裝置物件週期](pos-basics-deviceobject.md#device-object-lifecycle)。


## <a name="claim-negotiation"></a>宣告交涉

由於 Windows 是一個多工環境，它可讓相同電腦上的多個應用程式以合作方式要求存取周邊。  PointOfService API 提供交涉模型，允許多個應用程式共用連接到電腦的周邊。

當相同的電腦上的第二個應用程式要求宣告 PointOfService 周邊，而其他應用程式已經宣告該周邊，則會發佈 **ReleaseDeviceRequested** 事件通知。 如果應用程式目前使用裝置以避免遺失宣告，使用具主動式宣告的應用程式必須透過呼叫 **RetainDevice** 來回應事件通知。 

如果具主動式宣告的應用程式未立即回應 **RetainDevice**，則可能應用程式已暫停，或者不需要裝置，而且宣告撤銷，並提供給新的應用程式。 

第一個步驟是建立事件處理常式會與**RetainDevice** **ReleaseDeviceRequested**事件來回應。  

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

然後註冊與您已宣告的裝置相關聯的事件處理常式

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
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
