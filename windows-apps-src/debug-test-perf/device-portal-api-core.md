---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: 裝置入口網站核心 API 參考資料
description: 了解可用來存取資料並以程式設計方式控制裝置的 Windows 裝置入口網站核心 REST API。
ms.custom: 19H1
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10，uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 86724b084edb9350adfd2ed2623623d255302b70
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683456"
---
# <a name="device-portal-core-api-reference"></a>裝置入口網站核心 API 參考資料

所有裝置入口網站功能都是以 REST API 建置，開發人員可直接呼叫這些 API 來存取資源，並透過程式設計的方式控制其裝置。

## <a name="app-deployment"></a>應用程式部署

### <a name="install-an-app"></a>安裝 App

**要求**

您可以使用下列要求格式來安裝 App。

| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/app/packagemanager/package |

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| 套件   | (**必要**) 要安裝之套件的檔案名稱。 |

**要求標頭**

- 無

**要求本文**

- .appx 或 .appxbundle 檔案，以及 App 所需的任何相依性。 
- 用來簽署 App 的憑證 (如果裝置是 IoT 或 Windows Desktop)。 其他平台並不需要憑證。 

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
| 200 | 部署要求已受理並正在處理 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="install-a-related-set"></a>安裝相關集合

**要求**

您可以使用下列要求格式來安裝[相關集合](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)。

| 方法      | 要求 URI |
| :------     | :------ |
| POST | /api/app/packagemanager/package |

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| 套件   | (**必要**) 要安裝之套件的檔案名稱。 |

**要求標頭**

- 無

**要求本文** 
- 指定為參數時，新增 ".opt" 至選用的套件檔案名稱，像這樣："foo.appx.opt" 或 "bar.appxbundle.opt"。 
- .appx 或 .appxbundle 檔案，以及 App 所需的任何相依性。 
- 用來簽署 App 的憑證 (如果裝置是 IoT 或 Windows Desktop)。 其他平台並不需要憑證。 

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
| 200 | 部署要求已受理並正在處理 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-an-app-in-a-loose-folder"></a>登錄鬆散資料夾中的 App

**要求**

您可以使用下列要求格式登錄鬆散資料夾中的 App。

| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/app/packagemanager/networkapp |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    }
}
```

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
| 200 | 部署要求已受理並正在處理 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-a-related-set-in-loose-file-folders"></a>在鬆散檔案資料夾中登錄相關集合

**要求**

您可以使用下列要求格式在鬆散資料夾中登錄[相關集合](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)。

| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/app/packagemanager/networkapp |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    },
    "optionalpackages" :
    [
        {
            "networkshare" : "\\some\share\path2",
            "username" : "optional_username2",
            "password" : "optional_password2"
        },
        ...
    ]
}
```

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
| 200 | 部署要求已受理並正在處理 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-app-installation-status"></a>取得 App 安裝狀態

**要求**

您可以透過使用下列要求格式，以取得目前正在進行的 App 安裝狀態。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/app/packagemanager/state |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
| 200 | 最後部署的結果 |
| 204 | 正在執行安裝 |
| 404 | 找不到安裝動作 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="uninstall-an-app"></a>解除安裝 App

**要求**

您可以使用下列要求格式將 App 解除安裝。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/app/packagemanager/package |

**URI 參數**

| URI 參數 | 說明 |
| :------          | :------ |
| 套件   | (**必要**) 目標 App 的 PackageFullName (來自 GET /api/app/packagemanager/packages) |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-installed-apps"></a>取得已安裝的應用程式

**要求**

您可以透過使用下列要求格式，以取得系統上已安裝 App 的清單。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/app/packagemanager/packages |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含一份已安裝套件與其相關詳細資料的清單。 適用於此回應的範本如下所示。
```json
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

## <a name="bluetooth"></a>[藍牙]

<hr>

### <a name="get-the-bluetooth-radios-on-the-machine"></a>取得電腦上的藍牙無線電

**要求**

您可以透過使用下列要求格式，以取得電腦上已安裝藍牙無線電的清單。 這也可以使用相同的 JSON 資料升級至 WebSocket 連線。
 
| 方法        | 要求 URI |
| :------          | :------ |
| GET           | /api/bt/getradios |
| GET/WebSocket | /api/bt/getradios |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

此回應包含附加至裝置的藍牙無線電 JSON 陣列。
```json
{"BluetoothRadios" : [
    {
        "BluetoothAddress" : int64,
        "DisplayName" : string,
        "HasUnknownUsbDevice" : boolean,
        "HasProblem" : boolean,
        "ID" : string,
        "ProblemCode" : int,
        "State" : string
    },...
]}
```
**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼 | 說明 |
| :------             | :------ |
| 200              | [確定] |
| 4XX              | 錯誤碼 |
| 5XX              | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="turn-the-bluetooth-radio-on-or-off"></a>開啟或關閉藍牙無線電。

**要求**

設定特定藍牙無線電為開或關。
 
| 方法 | 要求 URI |
| :------   | :------ |
| POST   | /api/bt/setradio |

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| 識別碼            | (**需要**) 藍牙無線電的裝置 ID 而且必須是 Base-64 編碼。 |
| 州/省         | （**必要**）這可以是 `"On"` 或 `"Off"`。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼 | 說明 |
| :------             | :------ |
| 200              | [確定] |
| 4XX              | 錯誤碼 |
| 5XX              | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### <a name="get-a-list-of-paired-bluetooth-devices"></a>取得配對的藍牙裝置清單

**要求**

您可以使用下列要求格式來取得目前配對的藍牙裝置清單。 這可以升級至具有相同 JSON 資料的 WebSocket 連線。 在 WebSocket 連線的存留期內，裝置清單可能會變更。 每次有更新時，將會透過 WebSocket 連線傳送完整的裝置清單。

| 方法        | 要求 URI       |
| :---          | :---              |
| GET           | /api/bt/getpaired |
| GET/WebSocket | /api/bt/getpaired |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含目前配對的藍牙裝置的 JSON 陣列。
```json
{"PairedDevices": [
    {
        "Name" : string,
        "ID" : string,
        "AudioConnectionStatus" : string
    },...
]}
```
如果裝置可用於此系統上的音訊，則會出現 [ *AudioConnectionStatus* ] 欄位。 （原則和選用元件可能會影響此。）*AudioConnectionStatus*將會是「已連接」或「已中斷連線」。

---
### <a name="get-a-list-of-available-bluetooth-devices"></a>取得可用的藍牙裝置清單

**要求**

您可以使用下列要求格式，取得可供配對的藍牙裝置清單。 這可以升級至具有相同 JSON 資料的 WebSocket 連線。 在 WebSocket 連線的存留期內，裝置清單可能會變更。 每次有更新時，將會透過 WebSocket 連線傳送完整的裝置清單。

| 方法        | 要求 URI          |
| :---          | :---                 |
| GET           | /api/bt/getavailable |
| GET/WebSocket | /api/bt/getavailable |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含目前可用於配對的藍牙裝置的 JSON 陣列。
```json
{"AvailableDevices": [
    {
        "Name" : string,
        "ID" : string
    },...
]}
```

---
### <a name="connect-a-bluetooth-device"></a>連接藍牙裝置

**要求**

如果裝置可用於此系統上的音訊，將會連接到裝置。 （原則和選用元件可能會影響此。）

| 方法       | 要求 URI           |
| :---         | :---                  |
| POST         | /api/bt/connectdevice |

**URI 參數**

| URI 參數 | 說明 |
| :---          | :--- |
| 識別碼            | （**必要**）藍牙裝置的關聯端點識別碼，必須是 Base64 編碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼 | 說明 |
| :---             | :--- |
| 200              | [確定] |
| 4XX              | 錯誤碼 |
| 5XX              | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT


---
### <a name="disconnect-a-bluetooth-device"></a>中斷藍牙裝置的連線

**要求**

如果裝置可用於此系統上的音訊，將會中斷裝置的連線。 （原則和選用元件可能會影響此。）

| 方法       | 要求 URI              |
| :---         | :---                     |
| POST         | /api/bt/disconnectdevice |

**URI 參數**

| URI 參數 | 說明 |
| :---          | :--- |
| 識別碼            | （**必要**）藍牙裝置的關聯端點識別碼，必須是 Base64 編碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼 | 說明 |
| :---             | :--- |
| 200              | [確定] |
| 4XX              | 錯誤碼 |
| 5XX              | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
## <a name="device-manager"></a>[裝置管理員]
<hr>

### <a name="get-the-installed-devices-on-the-machine"></a>取得電腦上已安裝的裝置

**要求**

您可以透過使用下列要求格式，以取得電腦上已安裝裝置的清單。

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/devicemanager/devices |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

此回應包含附加至裝置的裝置 JSON 陣列。
```json
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* IoT

<hr>

### <a name="get-data-on-connected-usb-deviceshubs"></a>在連接的 USB 裝置/中樞上取得資料

**要求**

您可以透過使用下列要求格式，取得已連接 USB 裝置和中樞的 USB 描述項清單。

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /ext/devices/usbdevices |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應是 JSON，包括 USB 裝置的 DeviceID 以及中樞的 USB 描述項和連接埠資訊。
```json
{
    "DeviceList": [
        {
        "ID": string,
        "ParentID": string, // Will equal an "ID" within the list, or be blank
        "Description": string, // optional
        "Manufacturer": string, // optional
        "ProblemCode": int, // optional
        "StatusCode": int // optional
        },
        ...
    ]
}
```

**範例傳回資料**
```json
{
    "DeviceList": [{
        "ID": "System",
        "ParentID": ""
    }, {
        "Class": "USB",
        "Description": "Texas Instruments USB 3.0 xHCI Host Controller",
        "ID": "PCI\\VEN_104C&DEV_8241&SUBSYS_1589103C&REV_02\\4&37085792&0&00E7",
        "Manufacturer": "Texas Instruments",
        "ParentID": "System",
        "ProblemCode": 0,
        "StatusCode": 25174026
    }, {
        "Class": "USB",
        "Description": "USB Composite Device",
        "DeviceDriverKey": "{36fc9e60-c465-11cf-8056-444553540000}\\0016",
        "ID": "USB\\VID_045E&PID_00DB\\8&2994096B&0&1",
        "Manufacturer": "(Standard USB Host Controller)",
        "ParentID": "USB\\VID_0557&PID_8021\\7&2E9A8711&0&4",
        "ProblemCode": 0,
        "StatusCode": 25182218
    }]
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

## <a name="dump-collection"></a>傾印集合

<hr>

### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>取得應用程式的所有損毀傾印清單

**要求**

您可以透過使用下列要求格式，以取得所有側載 App 的所有可用損毀傾印清單。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/dumps |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含每個側載應用程式的損毀傾印清單。

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile (位於Windows 測試人員計畫中)
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>取得 App 的損毀傾印集合設定

**要求**

您可以透過使用下列要求格式，以取得側載 App 的損毀傾印集合設定。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashcontrol |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| packageFullname   | (**必要**) 側載 App 的完整套件名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應具有下列格式。
```json
{"CrashDumpEnabled": bool}
```

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile (位於Windows 測試人員計畫中)
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>刪除側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以刪除側載 App 的損毀傾印。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashdump |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| packageFullname   | (**必要**) 側載 App 的完整套件名稱。 |
| fileName   | (**必要**) 應該刪除之傾印檔案的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile (位於Windows 測試人員計畫中)
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>停用側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以停用側載 App 的損毀傾印。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashcontrol |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| packageFullname   | (**必要**) 側載 App 的完整套件名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile (位於Windows 測試人員計畫中)
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>下載側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以下載側載 App 的損毀傾印。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashdump |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| packageFullname   | (**必要**) 側載 App 的完整套件名稱。 |
| fileName   | (**必要**) 您要下載之傾印檔案的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含傾印檔案。 您可以使用 WinDbg 或 Visual Studio 檢查傾印檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile (位於Windows 測試人員計畫中)
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>啟用側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以啟用側載 App 的損毀傾印。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/debug/dump/usermode/crashcontrol |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| packageFullname   | (**必要**) 側載 App 的完整套件名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 

**可用的裝置系列**

* Windows Mobile (位於Windows 測試人員計畫中)
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="get-the-list-of-bugcheck-files"></a>取得錯誤檢查檔案的清單

**要求**

您可以透過使用下列要求格式，以取得錯誤檢查小型傾印檔案的清單。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dumplist |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含一份傾印檔案名稱與這些檔案大小的清單。 此清單將使用下列格式。 
```json
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="download-a-bugcheck-dump-file"></a>下載錯誤檢查傾印檔案

**要求**

您可以透過使用下列要求格式，以下載錯誤檢查傾印檔案。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dump |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| filename   | (**必要**) 傾印檔案的檔案名稱。 您可以透過使用 API 來取得傾印清單，以找到此項目。 |


**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含傾印檔案。 您可以使用 WinDbg 來檢查此檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-the-bugcheck-crash-control-settings"></a>取得錯誤檢查損毀控制設定

**要求**

您可以透過使用下列要求格式，以取得錯誤檢查損毀控制設定。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/crashcontrol |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含損毀控制設定。 如需有關 CrashControl 的詳細資訊，請參閱 [CrashControl](https://technet.microsoft.com/library/cc951703.aspx) 文章。 適用於回應的範本如下所示。
```json
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**傾印類型**

0：已停用

1：完整記憶體傾印 (收集所有的使用中記憶體)

2：核心記憶體傾印 (忽略使用者模式記憶體)

3︰有限的核心小型傾印

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-a-live-kernel-dump"></a>取得即時核心傾印

**要求**

您可以透過使用下列要求格式，以取得即時核心傾印。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/livekernel |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含完整的核心模式傾印。 您可以使用 WinDbg 來檢查此檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-a-dump-from-a-live-user-process"></a>取得即時使用者處理程序的傾印

**要求**

您可以透過使用下列要求格式，以取得即時使用者處理程序的傾印。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/live |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| pid   | (**必要**) 您感興趣的處理程序唯一處理程序識別碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含處理程序傾印。 您可以使用 WinDbg 或 Visual Studio 來檢查此檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="set-the-bugcheck-crash-control-settings"></a>設定錯誤檢查損毀控制設定

**要求**

您可以透過使用下列要求格式，以設定要用於收集錯誤檢查資料的設定。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/debug/dump/kernel/crashcontrol |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| autoreboot   | (**選用**) True 或 False。 這指出系統失敗或遭鎖定之後，是否會自動重新啟動。 |
| dumptype   | (**選用**) 傾印類型。 如需支援的值，請參閱 [CrashDumpType 列舉](https://docs.microsoft.com/previous-versions/azure/reference/dn802457(v=azure.100))。|
| maxdumpcount   | (**選用**) 要儲存的傾印數目上限。 |
| overwrite   | (**選用**) True 或 False。 這指出在達到 *maxdumpcount* 指定的傾印計數器限制時，是否會覆寫舊的傾印。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

## <a name="etw"></a>ETW

<hr>

### <a name="create-a-realtime-etw-session-over-a-websocket"></a>透過 WebSocket 建立即時 ETW 工作階段

**要求**

您可以透過使用下列要求格式，以建立即時 ETW 工作階段。 這將會透過 Websocket 管理。  ETW 事件會在伺服器上進行批次處理，並每秒傳送至用戶端一次。 
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET/WebSocket | /api/etw/session/realtime |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含來自已啟用之提供者的 ETW 事件。  請參閱下方的 ETW WebSocket 命令。 

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>ETW WebSocket 命令
這些命令會從用戶端傳送至伺服器。

| 命令 | 說明 |
| :----- | :----- |
| provider *{guid}* enable *{level}* | 在指定層級啟用標示為 *{guid}* (不含括號) 的提供者。 *{level}* 是介於 1 (粗略) 至 5 (詳細) 之間的 **int**。 |
| provider *{guid}* disable | 停用標示為 *{guid}* (不含括號) 的提供者。 |

此回應會從伺服器傳送至用戶端。 此回應會傳送為文字，而您可透過剖析 JSON 取得下列格式。
```json
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

承載物件為原始 ETW 事件中提供的額外機碼值組 (字串︰字串)。

範例：
```json
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

<hr>

### <a name="enumerate-the-registered-etw-providers"></a>列舉已登錄的 ETW 提供者

**要求**

您可以透過使用下列要求格式，以列舉已登錄提供者。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/etw/providers |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含 ETW 提供者的清單。 清單會包含每個提供者的易記名稱與 GUID，且使用下列格式。
```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
|  200 | [確定] | 

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>列舉由平台公開的自訂 ETW 提供者。

**要求**

您可以透過使用下列要求格式，以列舉已登錄提供者。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/etw/customproviders |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

200 確定。 回應會包含 ETW 提供者的清單。 清單會包含每個提供者的易記名稱與 GUID。

```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**狀態碼**

- 標準狀態碼。

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

## <a name="location"></a>位置

<hr>

### <a name="get-location-override-mode"></a>取得位置覆寫模式

**要求**

您可以使用下列要求格式，取得裝置的位置堆疊覆寫狀態。 開發人員模式必須已開啟，才能讓這個呼叫成功。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /ext/location/override |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含下列格式的裝置覆寫狀態。 

```json
{"Override" : bool}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
|  200 | [確定] | 
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

### <a name="set-location-override-mode"></a>設定位置覆寫模式

**要求**

您可以使用下列要求格式，設定裝置的位置堆疊覆寫狀態。 啟用時，位置堆疊允許位置插入。 開發人員模式必須已開啟，才能讓這個呼叫成功。

| 方法      | 要求 URI |
| :------     | :----- |
| PUT | /ext/location/override |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

```json
{"Override" : bool}
```

**回應**

回應包含已用下列格式將裝置設定成的覆寫狀態。 

```json
{"Override" : bool}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

### <a name="get-the-injected-position"></a>取得插入的位置

**要求**

您可以使用下列要求格式，取得裝置的插入 (偽裝) 位置。 必須設定插入的位置，否則將會擲回錯誤。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /ext/location/position |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含下列格式的目前插入緯度及經度值。 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

|  HTTP 狀態碼      | 說明 | 
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

### <a name="set-the-injected-position"></a>設定插入的位置

**要求**

您可以使用下列要求格式，設定裝置的插入 (偽裝) 位置。 必須先在裝置上啟用位置覆寫模式，而且設定的位置必須是有效的位置，否則會擲回錯誤。

| 方法      | 要求 URI |
| :------     | :----- |
| PUT | /ext/location/override |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**回應**

回應包含已設定為下列格式的位置。 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

## <a name="os-information"></a>OS 資訊

<hr>

### <a name="get-the-machine-name"></a>取得電腦名稱

**要求**

您可以透過使用下列要求格式，以取得電腦的名稱。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/os/machinename |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含下列格式的電腦名稱。 

```json
{"ComputerName": string}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-operating-system-information"></a>取得作業系統資訊

**要求**

您可以透過使用下列要求格式，以取得電腦的 OS 資訊。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/os/info |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含下列格式的作業系統資訊。

```json
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-device-family"></a>取得裝置系列 

**要求**

您可以使用下列要求格式取得裝置系列 (Xbox、手機、桌上型電腦等)。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/os/devicefamily |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含裝置系列 (SKU - Desktop、Xbox 等)。

```json
{
   "DeviceType" : string
}
```

DeviceType 看起來會像 "Windows.Xbox"、"Windows.Desktop" 等。 

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-the-machine-name"></a>設定電腦名稱

**要求**

您可以透過使用下列要求格式，以設定電腦的名稱。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/os/machinename |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| name | (**必要**) 電腦的新名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

## <a name="user-information"></a>使用者資訊

<hr>

### <a name="get-the-active-user"></a>取得使用中使用者

**要求**

您可以使用下列要求格式，取得裝置上使用中使用者的名稱。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/users/activeuser |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含下列格式的使用者資訊。 

成功時： 
```json
{
    "UserDisplayName" : string, 
    "UserSID" : string
}
```
失敗時：
```json
{
    "Code" : int, 
    "CodeText" : string, 
    "Reason" : string, 
    "Success" : bool
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

## <a name="performance-data"></a>效能資料

<hr>

### <a name="get-the-list-of-running-processes"></a>取得執行中的處理程序清單

**要求**

您可以透過使用下列要求格式，以取得目前執行中的處理程序清單。  此清單亦可升級至 WebSocket 連線，且每秒會將相同的 JSON 資料推送至用戶端一次。 
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/resourcemanager/processes |
| GET/WebSocket | /api/resourcemanager/processes |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含一份處理程序與每個處理程序之詳細資料的清單。 此資訊採用 JSON 格式，並具有下列範本。
```json
{"Processes": [
    {
        "CPUUsage": float,
        "ImageName": string,
        "PageFileUsage": long,
        "PrivateWorkingSet": long,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": long,
        "WorkingSetSize": long
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="get-the-system-performance-statistics"></a>取得系統效能統計資料

**要求**

您可以透過使用下列要求格式，以取得系統效能統計資料。 此類資訊包含讀取和寫入週期以及已使用多少記憶體。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/resourcemanager/systemperf |
| GET/WebSocket | /api/resourcemanager/systemperf |

此項目亦可升級至 WebSocket 連線。  其每秒會提供一次與下方相同的 JSON 資料。 

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應會包含系統的效能統計資料，例如 CPU 與 GPU 使用量、記憶體存取以及網路存取。 此資訊採用 JSON 格式，並具有下列範本。
```json
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

## <a name="power"></a>開啟/關閉

<hr>

### <a name="get-the-current-battery-state"></a>取得目前的電池狀態

**要求**

您可以透過使用下列要求格式，以取得目前的電池狀態。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/battery |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

使用以下格式傳回目前的電池狀態資訊。
```json
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="get-the-active-power-scheme"></a>取得使用中電源配置

**要求**

您可以透過使用下列要求格式，以取得使用中電源配置。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/activecfg |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

使用中電源配置具有下列格式。
```json
{"ActivePowerScheme": string (guid of scheme)}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-the-sub-value-for-a-power-scheme"></a>取得電源配置的子值

**要求**

您可以透過使用下列要求格式，以取得電源配置的子值。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/cfg/ *<power scheme path>* |

選項：
- SCHEME_CURRENT

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

根據每個應用程式與設定提供完整電源狀態清單，以標示諸如電池電力偏低或嚴重不足等各種電力狀態。 

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-the-power-state-of-the-system"></a>取得系統的電源狀態

**要求**

您可以透過使用下列要求格式，以檢查系統的電源狀態。 這可讓您檢查是否處於低電源狀態。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/state |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

電源狀態資訊具有下列範本。
```json
{"LowPowerState" : false, "LowPowerStateAvailable" : true }
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="set-the-active-power-scheme"></a>設定使用中電源配置

**要求**

您可以透過使用下列要求格式，以設定使用中電源配置。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/power/activecfg |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| scheme | (**必要**) 您要為系統設定之使用中電源配置的配置 GUID。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="set-the-sub-value-for-a-power-scheme"></a>設定電源配置的子值

**要求**

您可以透過使用下列要求格式，以設定電源配置的子值。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/power/cfg/ *<power scheme path>* |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| valueAC | (**必要**) 要用於 A/C 電源的值。 |
| valueDC | (**必要**) 要用於電池電力的值。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-a-sleep-study-report"></a>取得睡眠研究報告

**要求**

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/report |

您可以透過使用下列要求格式，以取得睡眠研究報告。

**URI 參數**
| URI 參數 | 說明 |
| :------          | :------ |
| FileName | (**必要**) 您要下載之檔案的完整名稱。 此值應該是 hex64 編碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應為包含休眠研究的檔案。 

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="enumerate-the-available-sleep-study-reports"></a>列舉可用的睡眠研究報告

**要求**

您可以透過使用下列要求格式，以列舉可用的睡眠研究報告。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/reports |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

可用的報告清單具有下列範本。

```json
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

### <a name="get-the-sleep-study-transform"></a>取得睡眠研究轉換

**要求**

您可以透過使用下列要求格式，以取得睡眠研究轉換。 此轉換是 XSLT，它可將睡眠研究報告轉換成人們可以讀取的 XML 格式。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/transform |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含休眠研究轉換。

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* IoT

<hr>

## <a name="remote-control"></a>遙控器

<hr>

### <a name="restart-the-target-computer"></a>重新啟動目標電腦

**要求**

您可以透過使用下列要求格式，以重新啟動目標電腦。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/control/restart |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="shut-down-the-target-computer"></a>將目標電腦關機

**要求**

您可以透過使用下列要求格式，以將目標電腦關機。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/control/shutdown |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

## <a name="task-manager"></a>工作管理員

<hr>

### <a name="start-a-modern-app"></a>啟動現代化應用程式

**要求**

您可以透過使用下列要求格式，以啟動現代化 App。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/taskmanager/app |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| appid   | (**必要**) 您要啟動之 App 的 PRAID。 此值應該是 hex64 編碼。 |
| 套件   | (**必要**) 您要啟動之應用程式套件的完整名稱。 此值應該是 hex64 編碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="stop-a-modern-app"></a>停止現代化 App

**要求**

您可以透過使用下列要求格式，以停止現代化 App。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/taskmanager/app |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 套件   | (**必要**) 您要停止之應用程式套件的完整名稱。 此值應該是 hex64 編碼。 |
| forcestop   | (**選用**) **yes** 的值表示系統應該強制停止所有處理程序。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="kill-process-by-pid"></a>依 PID 終止處理序

**要求**

您可以使用下列要求格式來終止處理序。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/taskmanager/process |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| pid   | (**必要**) 要停止的處理序唯一的處理序 ID。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

## <a name="networking"></a>網路

<hr>

### <a name="get-the-current-ip-configuration"></a>取得目前的 IP 設定

**要求**

您可以透過使用下列要求格式，以取得目前的 IP 設定。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/networking/ipconfig |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

回應包含下列範本中的 IP 設定。

```json
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-a-static-ip-address-ipv4-configuration"></a>設定靜態 IP 位址（IPV4 設定）

**要求**

設定具有靜態 IP 和 DNS 的 IPV4 設定。 如果未指定靜態 IP，則會啟用 DHCP。 如果指定了靜態 IP，則也必須指定 DNS。
 
| 方法      | 要求 URI |
| :------     | :----- |
| PUT | /api/networking/ipv4config |


**URI 參數**

| URI 參數 | 說明 |
| :---          | :--- |
| AdapterName | （**必要**）網路介面 GUID。 |
| IPAddress | 要設定的靜態 IP 位址。 |
| SubnetMask | （如果*IPAddress*不是 null，則為**必要**）靜態子網路遮罩。 |
| DefaultGateway | （如果*IPAddress*不是 null，則為**必要**）靜態預設閘道。 |
| PrimaryDNS | （如果*IPAddress*不是 null，則為**必要**）要設定的靜態主要 DNS。 |
| SecondayDNS | （如果*PrimaryDNS*不是 null，則為**必要**）要設定的靜態次要 DNS。 |

為了清楚起見，若要設定 DHCP 的介面，只會將網路上的 `AdapterName` 序列化：

```json
{
    "AdapterName":"{82F86C1B-2BAE-41E3-B08D-786CA44FEED7}"
}
```

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-network-interfaces"></a>列舉無線網路介面

**要求**

您可以透過使用下列要求格式，以列舉可用的無線網路介面。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wifi/interfaces |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

具有詳細資料的可用無線介面清單，使用下列格式。

```json 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-networks"></a>列舉無線網路

**要求**

您可以透過使用下列要求格式，以列舉指定介面上的無線網路清單。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wifi/networks |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| interface   | (**必要**) 可用來搜尋無線網路的網路介面 GUID，不含括號。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

可在提供的 *interface* 上找到的無線網路清單。 這包括網路的詳細資料，使用下列格式。

```json
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (for example, AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>連線到 Wi-Fi 網路和與它中斷連線。

**要求**

您可以透過使用下列要求格式，以連線到 Wi-Fi 網路或與它中斷連線。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/wifi/network |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| interface   | (**必要**) 可用來連線到網路的網路介面 GUID。 |
| op   | (**必要**) 指出要採取的動作。 可能的值是 connect 或 disconnect。|
| ssid   | (**如果 *op* == connect 則為必要**) 要連線的 SSID。 |
| key   | (**如果 *op* == connect 且網路需要驗證則為必要**) 共用金鑰。 |
| createprofile | (**必要**) 在裝置上建立網路設定檔。  這會導致日後將裝置自動連線至網路。 此項目可為**是**或**否**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-a-wi-fi-profile"></a>刪除 Wi-Fi 設定檔

**要求**

您可以透過使用下列要求格式，以刪除與特定介面上的網路關聯的設定檔。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/wifi/profile |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| interface   | (**必要**) 與要刪除之設定檔關聯的網路介面 GUID。 |
| profile   | (**必要**) 要刪除的設定檔名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

## <a name="windows-error-reporting-wer"></a>Windows 錯誤報告 (WER)

<hr>

### <a name="download-a-windows-error-reporting-wer-file"></a>下載 Windows 錯誤報告 (WER) 檔案

**要求**

您可以透過使用下列要求格式，以下載 WER 檔案。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wer/report/file |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| 使用者   | (**必要**) 與報告關聯的使用者名稱。 |
| 型別   | (**必要**) 報告的類型。 這可以是 **queried** 或 **archived**。 |
| name   | (**必要**) 報告的名稱。 此應為 base64 編碼。 |
| file   | (**必要**) 要從報告下載之檔案的名稱。 此應為 base64 編碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 回應包含所要求的檔案。 

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>列舉 Windows 錯誤報告 (WER) 報告中的檔案

**要求**

您可以透過使用下列要求格式，以列舉 WER 報告中的檔案。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wer/report/files |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| 使用者   | (**必要**) 與報告關聯的使用者。 |
| 型別   | (**必要**) 報告的類型。 這可以是 **queried** 或 **archived**。 |
| name   | (**必要**) 報告的名稱。 此應為 base64 編碼。 |

**要求標頭**

- 無

**要求本文**

```json
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="list-the-windows-error-reporting-wer-reports"></a>列出 Windows 錯誤報告 (WER) 報告

**要求**

您可以使用下列要求格式來取得 WER 報告。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wer/reports |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

WER 報告使用下列格式。

```json
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows 電腦
* HoloLens
* IoT

<hr>

## <a name="windows-performance-recorder-wpr"></a>Windows Performance Recorder (WPR) 

<hr>

### <a name="start-tracing-with-a-custom-profile"></a>使用自訂設定檔開始追蹤

**要求**

您可以透過使用下列要求格式，以上傳 WPR 設定檔，並使用該設定檔開始追蹤。  一次僅可執行一個追蹤。 此設定檔不會保留在裝置上。 
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/wpr/customtrace |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 包含自訂 WPR 設定檔的多部分合格 http 主體。

**回應**

WPR 工作階段狀態使用下列格式。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="start-a-boot-performance-tracing-session"></a>啟動開機效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以啟動開機 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/wpr/boottrace |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| profile   | (**必要**) 啟動時一定要有此參數。 應啟動效能追蹤工作階段之設定檔的名稱。 可能的設定檔儲存在 perfprofiles/profiles.json。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

啟動時，此 API 會傳回使用下列格式的 WPR 工作階段狀態。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="stop-a-boot-performance-tracing-session"></a>停止開機效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以停止開機 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wpr/boottrace |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

-  無。  **注意︰** 這是長時間執行的作業。  它在 ETL 完成寫入至磁碟後才會傳回。

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="start-a-performance-tracing-session"></a>啟動效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以啟動 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。  一次僅可執行一個追蹤。 
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/wpr/trace |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| profile   | (**必要**) 應啟動效能追蹤工作階段之設定檔的名稱。 可能的設定檔儲存在 perfprofiles/profiles.json。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

啟動時，此 API 會傳回使用下列格式的 WPR 工作階段狀態。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="stop-a-performance-tracing-session"></a>停止效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以停止 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wpr/trace |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無。  **注意︰** 這是長時間執行的作業。  它在 ETL 完成寫入至磁碟後才會傳回。  

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="retrieve-the-status-of-a-tracing-session"></a>擷取追蹤工作階段的狀態

**要求**

您可以透過使用下列要求格式，以擷取目前的 WPR 工作階段狀態。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wpr/status |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

WPR 追蹤工作階段的狀態，使用下列格式。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="list-completed-tracing-sessions-etls"></a>列出已完成的追蹤工作階段 (ETL)。

**要求**

您可以使用下列要求格式取得裝置上 ETL 追蹤的清單。 

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wpr/tracefiles |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

已完成的追蹤工作階段清單將以下列格式提供。

```json
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="download-a-tracing-session-etl"></a>下載追蹤工作階段 (ETL)

**要求**

您可以使用下列要求格式下載 Tracefile (開機追蹤或使用者模式追蹤)。 

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/wpr/tracefile |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| filename   | (**必要**) 要下載之 ETL 追蹤的名稱。  可以在 /api/wpr/tracefiles 找到 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

### <a name="delete-a-tracing-session-etl"></a>刪除追蹤工作階段 (ETL)

**要求**

您可以使用下列要求格式刪除 Tracefile (開機追蹤或使用者模式追蹤)。 

| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/wpr/tracefile |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :------          | :------ |
| filename   | (**必要**) 要刪除之 ETL 追蹤的名稱。  可以在 /api/wpr/tracefiles 找到 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

<hr>

## <a name="dns-sd-tags"></a>DNS-SD 標記 

<hr>

### <a name="view-tags"></a>檢視標記

**要求**

檢視裝置目前套用的標記。  這些標記會透過 T 索引鍵中的 DNS-SD TXT 公告。  
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/dns-sd/tags |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應** 目前以下列格式套用的標記。 
```json
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 5XX | 伺服器錯誤 |


**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tags"></a>刪除標記

**要求**

刪除目前由 DNS-SD 公告的所有標記   
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/dns-sd/tags |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**
 - 無

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 5XX | 伺服器錯誤 |


**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tag"></a>刪除標記

**要求**

刪除目前由 DNS-SD 公告的標記   
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/dns-sd/tag |


**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| tagValue | (**必要**) 要移除的標記。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**
 - 無

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |


**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT
 
<hr>

### <a name="add-a-tag"></a>新增標記

**要求**

將標籤新增到 DNS-SD 廣告。   
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/dns-sd/tag |


**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| tagValue | (**必要**) 要新增的標記。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**
 - 無

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 401 | 標記空間溢位。  當建議標記對於產生的 DNS-SD 服務記錄過長時的結果。 |


**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>App 檔案總管

<hr>

### <a name="get-known-folders"></a>取得已知的資料夾

**要求**

取得可存取的最上層資料夾清單。

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/filesystem/apps/knownfolders |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應** 以下列格式呈現的可用資料夾。 
```json
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | 部署要求已受理並正在處理 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |


**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

<hr>

### <a name="get-files"></a>取得檔案

**要求**

取得資料夾中的檔案清單。

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/filesystem/apps/files |


**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| knownfolderid | (**必要**) 您希望取得檔案清單的最上層目錄。 使用 **LocalAppData** 來存取側載 App。 |
| packagefullname | (**如果 *knownfolderid* == LocalAppData 則為必要**) 您感興趣之應用程式的套件完整名稱。 |
| path | (**選用**) 資料夾內的子目錄或上方所指定的套件。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應** 以下列格式呈現的可用資料夾。 
```json
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

<hr>

### <a name="download-a-file"></a>下載檔案

**要求**

從已知的資料夾或 appLocalData 取得檔案。

| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/filesystem/apps/file |

**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| knownfolderid | (**必要**) 您希望下載檔案的最上層目錄。 使用 **LocalAppData** 來存取側載 App。 |
| filename | (**必要**) 下載的檔案名稱。 |
| packagefullname | (**如果 *knownfolderid* == LocalAppData 則為必要**) 您感興趣的套件完整名稱。 |
| path | (**選用**) 資料夾內的子目錄或上方所指定的套件。 |

**要求標頭**

- 無

**要求本文**

- 要求的檔案 (若有的話)

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | 要求的檔案 |
| 404 | 找不到檔案 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

<hr>

### <a name="rename-a-file"></a>重新命名檔案

**要求**

重新命名資料夾中的檔案。

| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/filesystem/apps/rename |


**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| knownfolderid | (**必要**) 檔案所在的最上層目錄。 使用 **LocalAppData** 來存取側載 App。 |
| filename | (**必要**) 所要重新命名之檔案的原始名稱。 |
| newfilename | (**必要**) 檔案的新名稱。|
| packagefullname | (**如果 *knownfolderid* == LocalAppData 則為必要**) 您感興趣之應用程式的套件完整名稱。 |
| path | (**選用**) 資料夾內的子目錄或上方所指定的套件。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |。 檔案已重新命名
| 404 | 找不到檔案 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

<hr>

### <a name="delete-a-file"></a>刪除檔案

**要求**

刪除資料夾中的檔案。

| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/filesystem/apps/file |

**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| knownfolderid | (**必要**) 您希望刪除檔案的最上層目錄。 使用 **LocalAppData** 來存取側載 App。 |
| filename | (**必要**) 刪除的檔案名稱。 |
| packagefullname | (**如果 *knownfolderid* == LocalAppData 則為必要**) 您感興趣之應用程式的套件完整名稱。 |
| path | (**選用**) 資料夾內的子目錄或上方所指定的套件。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無 

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |。 檔案已刪除 |
| 404 | 找不到檔案 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

<hr>

### <a name="upload-a-file"></a>上傳檔案

**要求**

上傳檔案至資料夾。  這將會使用相同名稱覆寫現有的檔案，但不會建立新的資料夾。 

| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/filesystem/apps/file |

**URI 參數**

| URI 參數 | 說明 |
| :------     | :----- |
| knownfolderid | (**必要**) 您希望上傳檔案的最上層目錄。 使用 **LocalAppData** 來存取側載 App。 |
| packagefullname | (**如果 *knownfolderid* == LocalAppData 則為必要**) 您感興趣之應用程式的套件完整名稱。 |
| path | (**選用**) 資料夾內的子目錄或上方所指定的套件。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼      | 說明 |
| :------     | :----- |
| 200 | [確定] |。 檔案已上傳 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用的裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT
