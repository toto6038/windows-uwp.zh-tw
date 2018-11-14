---
author: Xansky
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: 每個 Microsoft Store 交易只要結果為產品購買成功，都可依選擇傳回交易收據。
title: 使用收據來驗證產品購買
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, uwp,app 內購買, IAPs, 收據, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: ed79a3ac50fb3a6cbe735e0ea713256845d39441
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2018
ms.locfileid: "6156378"
---
# <a name="use-receipts-to-verify-product-purchases"></a>使用收據來驗證產品購買

每個 Microsoft Store 交易只要結果為產品購買成功，都可依選擇傳回交易收據。 這個收據會為客戶提供所列產品和金錢花費的相關資訊。

如果您的 App 需要確認使用者已購買 App，或是已從 Microsoft Store 進行附加元件 (也稱為 App 內產品或 IAP) 購買，這項資訊的存取將可支援這些情況。 例如，想像有一個提供下載內容的遊戲。 如果購買遊戲內容的使用者想要在另一台裝置上玩遊戲，您就需要確認該使用者已經購買內容。 方法如下。

> [!IMPORTANT]
> 這篇文章說明如何使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空間的成員來取得和驗證收到 app 內購買。 如果您使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store) 命名空間來進行 App 內購買 (在 Windows 10 (版本 1607) 中引進，適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393))** 或更新版本的專案，此命名空間並未提供 API 來取得 App 內購買的購買收據。 不過，您可以在 Microsoft Store 收藏 API 中使用 REST 方法取得購買交易的資料。 如需詳細資訊，請參閱 [App 內購買的收據](in-app-purchases-and-trials.md#receipts)。

## <a name="requesting-a-receipt"></a>要求收據


**Windows.ApplicationModel.Store** 命名空間支援幾種方式取得收據︰

* 當您使用 [CurrentApp.RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 或 [CurrentApp.RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) (或此方法的其中一個其他多載) 進行購買，傳回的值會包含收據。
* 您可以呼叫 [CurrentApp.GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) 方法來擷取 App 和 App 中任何附加元件目前的收據資訊。

App 收據看起來如下。

> [!NOTE]
> 這個範例將格式設定成可讓 XML 可讀性變高。 實際應用程式收據的各元素之間不包含空格。

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

產品收據看起來如下。

> [!NOTE]
> 這個範例將格式設定成可讓 XML 可讀性變高。 實際產品收據的各元素之間不包含空格。

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

您可以使用這些收據範例的任一種來測試驗證碼。 如需收據內容的詳細資訊，請參閱[元素和屬性描述](#receipt-descriptions)。

## <a name="validating-a-receipt"></a>驗證收據

若要驗證收據的真確性，您需要後端系統 (Web 服務或類似服務) 使用公開憑證來檢查收據的簽章。 若要取得這個憑證，請使用 URL ```https://go.microsoft.com/fwlink/p/?linkid=246509&cid=CertificateId```，其中 ```CertificateId``` 是收據中的 **CertificateId** 值。

以下是該驗證程序的範例。 這個程式碼會在包含 **System.Security** 組件之參考的 .NET Framework 主控台應用程式中執行。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>收據的元素和屬性描述

本節說明收據中的元素和屬性。

### <a name="receipt-element"></a>Receipt 元素

此檔案的根元素是 **Receipt** 元素，其中包含 App 與 App 內購買的相關資訊。 此元素包含下列子項元素。

|  元素  |  必要  |  數量  |  描述   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    否        |  0 或 1  |  包含目前 App 的購買資訊。            |
|  [ProductReceipt](#productreceipt)  |     否       |  0 或以上    |   包含目前 App 之 App 內購買的相關資訊。     |
|  Signature  |      是      |  1   |   此元素是標準的 [XML-DSIG 建構](http://go.microsoft.com/fwlink/p/?linkid=251093)。 它包含一個 **SignatureValue** 元素，其中包含您可用來驗證收據的簽章，以及一個 **SignedInfo** 元素。      |

**Receipt** 具有下列屬性。

|  屬性  |  描述   |
|-------------|-------------------|
|  **Version**  |    收據的版本號碼。            |
|  **CertificateId**  |     用來簽署收據的憑證指紋。          |
|  **ReceiptDate**  |    簽署和下載收據的日期。           |  
|  **ReceiptDeviceId**  |   識別用來要求此收據的裝置。         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>AppReceipt 元素

此元素包含目前 App 的購買資訊。

**AppReceipt** 具有下列屬性。

|  屬性  |  描述   |
|-------------|-------------------|
|  **Id**  |    識別購買。           |
|  **AppId**  |     作業系統用於 App 的「套件系列名稱」值。           |
|  **LicenseType**  |    如果使用者已購買完整版的 App，為 **Full**。 如果使用者下載試用版的 App，為 **Trial**。           |  
|  **PurchaseDate**  |    取得 App 的日期。          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>ProductReceipt 元素

此元素包含目前 App 之 App 內購買的相關資訊。

**ProductReceipt** 具有下列屬性。

|  屬性  |  描述   |
|-------------|-------------------|
|  **Id**  |    識別購買。           |
|  **AppId**  |     識別使用者透過哪個 App 進行購買。           |
|  **ProductId**  |     識別購買的產品。           |
|  **ProductType**  |    決定產品類型。 目前僅支援 **Durable** 值。          |  
|  **PurchaseDate**  |    購買的日期。          |  |

 

 
