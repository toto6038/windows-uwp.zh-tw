---
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: 列舉網路上的裝置
description: 除了探索本機連線的裝置，您也可以使用 Windows.Devices.Enumeration API 來列舉無線及網路通訊協定上的裝置。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e80d16b3338291c756b543018812e9db1370a4ac
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8743517"
---
# <a name="enumerate-devices-over-a-network"></a>列舉網路上的裝置



**重要 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

除了探索本機連線的裝置，您也可以使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 來列舉無線及網路通訊協定上的裝置。

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>列舉網路及無線通訊協定上的裝置

有時您需要列舉未在本機連接且只能透過無線或網路通訊協定進行探索的裝置。 針對這種需求，[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 提供三種不同的裝置物件：**AssociationEndpoint** (AEP)、**AssociationEndpointContainer** (AEP 容器) 及 **AssociationEndpointService** (AEP 服務)。 上述物件被統稱為 AEP 或 AEP 物件。

有些裝置 API 提供選取器字串，讓您可用來列舉可用的 AEP 物件。 這可能同時包含與系統配對及未配對的裝置。 某些裝置可能不需要配對。 如果在與裝置互動之前需要先進行配對，則這些裝置 API 可能會嘗試配對該裝置。 Wi-Fi Direct 就是遵循這個模式的 API 範例。 如果這些裝置 API 不會自動配對裝置，您可以使用可從 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960) 取得的 [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396) 物件進行配對。

不過，可能會有一些情況是您想要自己手動探索裝置，而不使用預先定義的選取器字串。 例如，您可能只需收集 AEP 裝置的相關資訊，而不需與它們互動，或者您可能想要尋找更多可以使用預先定義的選取器字串來探索的 AEP 物件。 在此情況下，您將建置自己的選取器字串，並遵循[建置裝置選取器](build-a-device-selector.md)下方的指示來使用它。

當您建置自己的選取器時，強烈建議您將列舉範圍限制在您感興趣的通訊協定。 例如，如果您對 UPnP 裝置特別感興趣，就不會想讓 Wi-Fi 廣播電台搜尋 Wi-Fi Direct 裝置。 Windows 已針對您可以用來設定列舉範圍的每個通訊協定定義身分識別。 下表列出通訊協定類型和識別碼。

| 通訊協定或網路裝置類型              | Id                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP (包括 DIAL 和 DLNA)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| 裝置上的 Web 服務 (WSD)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-Fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| DNS 服務探索 (DNS-SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| 服務點                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| 網路印表機 (Active Directory 印表機) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Windows Connect Now (WNC)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| WiGig 擴充座                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| HP 印表機的 Wi-Fi 佈建           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| 藍牙                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| 藍牙 LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## <a name="aqs-examples"></a>AQS 範例

每個 AEP 類型都有您可用來將列舉限制在特定通訊協定的屬性。 請記住，您可以在 AQS 篩選器中使用 OR 運算子來結合多個通訊協定。 以下是幾個 AQS 篩選字串的範例，可說明如何查詢 AEP 裝置。

這個 AQS 會在將 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 設定為 **AsssociationEndpoint** 時查詢所有的 UPnP **AssociationEndpoint** 物件。

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

這個 AQS 會在將 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 設定為 **AsssociationEndpoint** 時查詢所有的 UPnP 和 WSD **AssociationEndpoint** 物件。

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

這個 AQS 會在將 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 設定為 **AsssociationEndpointService** 時查詢所有的 UPnP **AssociationEndpointService** 物件。

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

這個 AQS 會在將 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 設定為 **AssociationEndpointContainer** 時查詢 **AssociationEndpointContainer** 物件，但只會透過列舉 UPnP 通訊協定來尋找它們。 一般而言，列舉只來自一個通訊協定的容器並不會有幫助。 不過，將篩選限制在您知道可探索到您裝置的通訊協定可能會有幫助。

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 
