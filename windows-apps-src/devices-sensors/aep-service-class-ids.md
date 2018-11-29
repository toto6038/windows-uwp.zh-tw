---
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: AEP 服務類別識別碼
description: 關聯端點 (AEP) 服務提供特定通訊協定上裝置支援服務的程式設計協定。 其中幾個服務已建立參考他們時應使用的識別碼。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: faa9e3f8936e8650af905678ce7434c4b9967be0
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7976485"
---
# <a name="aep-service-class-ids"></a>AEP 服務類別識別碼



**重要 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

關聯端點 (AEP) 服務提供特定通訊協定上裝置支援服務的程式設計協定。 其中幾個服務已建立參考他們時應使用的識別碼。 這些協定可使用 **System.Devices.AepService.ServiceClassId** 屬性進行識別。 這個主題列出數個已知的 AEP 服務類別識別碼。 AEP 服務類別識別碼也適用於使用自訂類別識別碼的通訊協定。

app 開發人員應該根據類別識別碼使用進階的查詢語法 (AQS) 篩選器，來將查詢限制在他們想要使用的 AEP 服務內。 這同時會將查詢結果限制在相關的服務中，這將會大幅提高裝置的效能、電池使用時間，以及服務品質。 例如，應用程式可以使用這些服務類別識別碼，來使用如 Miracast 同步或 DLNA 數位媒體轉譯器 (DMR) 的裝置。 如需裝置和服務如何彼此互動的詳細資訊，請參閱[**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)。

## <a name="bluetooth-and-bluetooth-le-services"></a>藍芽和藍牙 LE 服務

藍芽服務會使用下列其中一個通訊協定：藍牙通訊協定或藍牙 LE 通訊協定。 這些通訊協定的識別碼是：

-   藍牙通訊協定識別碼：{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
-   藍牙 LE 通訊協定識別碼：{bb7bb05e-5972-42b5-94fc-76eaa7084d49}

藍牙通訊協定支援全都遵循相同基本格式的多個服務。 GUID 的前四個數字會因服務而有所不同，但所有的藍芽 GUID 都會以 **0000-0000-1000-8000-00805F9B34FB** 結尾。 例如，RFCOMM 服務都有 0x0003 的前導碼，因此完整的識別碼會是 **00030000-0000-1000-8000-00805F9B34FB**。 下表列出一些常見的藍芽服務。

| 服務名稱                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT - 警示通知服務    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT - 自動化 IO                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT - 電池服務               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT - 血壓計                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT - 內容組合              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT - 證券管理               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT - 連續血糖監視 | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT - 目前的時間服務          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT - 輪轉動力                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT - 輪轉速度和頻率     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT - 裝置資訊            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT - 環境感應器         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT - 標準存取                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT - 標準屬性             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT - 葡萄糖                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT - 健康體溫計            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT - 心率                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT - 人性化介面裝置        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT - 即時警示               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT - 室內定位            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT - 網際網路通訊協定支援     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT - 連結中斷                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT - 位置與瀏覽       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT - 下一個 DST 變更服務       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT - 電話警示狀態服務    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT - 脈搏血氧儀                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT - 參照時間更新服務 | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT - 跑步速度和頻率     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT - 掃描參數               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT - Tx 電量                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT - 使用者資料                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT - 體重計                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

如需可用藍芽服務的更完整清單，請參閱藍芽的通訊協定和服務頁面[這裡](http://go.microsoft.com/fwlink/p/?LinkID=619586)和[這裡](http://go.microsoft.com/fwlink/p/?LinkID=619587)。 您也可以使用 [**GattServiceUuids**](https://msdn.microsoft.com/library/windows/apps/Dn297571)API 來取得一些常見的 GATT 服務。

## <a name="custom-bluetooth-le-services"></a>自訂藍芽 LE 服務

自訂藍牙 LE 服務會使用下列通訊協定識別碼：{bb7bb05e-5972-42b5-94fc76eaa7084d49}

自訂設定檔會使用自己定義的 GUID 進行定義。 這個自訂 GUID 應該用於**System.Devices.AepService.ServiceClassId**。

## <a name="upnp-services"></a>UPnP 服務

UPnP 服務會使用下列通訊協定識別碼：{0e261de4-12f0-46e6-91ba-428607ccef64}

一般而言，所有 UPnP 服務都會使用 RFC 4122 中定義的演算法，將其名稱雜湊為 GUID。 下表列出一些在 Windows 中定義的常見 UPnP 服務。

| 服務名稱                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| 連線管理員                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| AV 傳輸                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| 轉譯控制                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| 第三層轉送                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| WAN 通用介面設定 | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| WAP IP 連線                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| WFA WLAN 設定             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| 功能加強的印表機                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| 印表機基本                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| 媒體接收器登錄器           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| 內容目錄                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| 撥號                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

 

## <a name="wsd-services"></a>WSD 服務

WSD 服務會使用下列通訊協定識別碼：{782232aa-a2f9-4993-971b-aedc551346b0}

一般而言，所有 WSD 服務都會使用 RFC 4122 中定義的演算法，將其名稱雜湊為 GUID。 下表列出一些在 Windows 中定義的常見 WSD 服務。

| 服務名稱 | GUID                                     |
|--------------|------------------------------------------|
| 印表機      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| 掃描器      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

 

## <a name="aqs-sample"></a>AQS 範例

這個 AQS 將會篩選所有支援撥號的 UPnP **AssociationEndpointService** 物件。 在此情況下，[**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 會設定為 **AsssociationEndpointService**。

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```

 

 
