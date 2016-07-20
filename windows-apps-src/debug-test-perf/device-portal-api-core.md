---
author: dbirtolo
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: "Device Portal 核心 API 參考資料"
description: "了解可用來存取資料並以程式設計方式控制裝置的 Windows Device Portal 核心 REST API。"
translationtype: Human Translation
ms.sourcegitcommit: 30aeffcf090c881f84331ced4f7199fd0092b676
ms.openlocfilehash: 0fa515d28431d4256b977ee3c3c41169661f129f

---

# Device Portal 核心 API 參考資料

Windows Device Portal 中的所有項目都是以 REST API (可讓您用來存取資料並以程式設計方式控制裝置) 為基礎所建置。

## 應用程式部署

---
### 安裝 App

**要求**

您可以使用下列要求格式來安裝 App。

方法      | 要求 URI
:------     | :-----
POST | /api/app/packagemanager/package
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
package   | (**必要**) 要安裝之套件的檔案名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 部署要求已受理並正在處理
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 取得 App 安裝狀態

**要求**

您可以透過使用下列要求格式，以取得目前正在進行的 App 安裝狀態。
 
方法      | 要求 URI
:------     | :-----
GET | /api/app/packagemanager/state
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 最後部署的結果
204 | 正在執行安裝
404 | 找不到安裝動作
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 解除安裝 App

**要求**

您可以使用下列要求格式將 App 解除安裝。
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/app/packagemanager/package
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 取得已安裝的應用程式

**要求**

您可以透過使用下列要求格式，以取得系統上已安裝 App 的清單。
 
方法      | 要求 URI
:------     | :-----
GET | /api/app/packagemanager/packages
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含一份已安裝套件與其相關詳細資料的清單。 適用於此回應的範本如下所示。
```
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## 裝置管理員
---
### 取得電腦上已安裝的裝置

**要求**

您可以透過使用下列要求格式，以取得電腦上已安裝裝置的清單。
 
方法      | 要求 URI
:------     | :-----
GET | /api/devicemanager/devices
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

此回應包含附加至裝置的裝置 JSON 陣列。
``` 
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* IoT

---
## 傾印集合
---
### 取得 App 的所有損毀傾印清單

**要求**

您可以透過使用下列要求格式，以取得所有側載 App 的所有可用損毀傾印清單。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/usermode/dumps
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含每個側載應用程式的損毀傾印清單。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 取得 App 的損毀傾印集合設定

**要求**

您可以透過使用下列要求格式，以取得側載 App 的損毀傾印集合設定。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應具有下列格式。
```
{"CrashDumpEnabled": bool}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 刪除側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以刪除側載 App 的損毀傾印。
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
fileName   | (**必要**) 應該刪除之傾印檔案的名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 停用側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以停用側載 App 的損毀傾印。
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol

<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 下載側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以下載側載 App 的損毀傾印。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/usermode/crashdump
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
fileName   | (**必要**) 您要下載之傾印檔案的名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含傾印檔案。 您可以使用 WinDbg 或 Visual Studio 檢查傾印檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 啟用側載 App 的損毀傾印

**要求**

您可以透過使用下列要求格式，以啟用側載 App 的損毀傾印。
 
方法      | 要求 URI
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 取得錯誤檢查檔案的清單

**要求**

您可以透過使用下列要求格式，以取得錯誤檢查小型傾印檔案的清單。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/kernel/dumplist
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含一份傾印檔案名稱與這些檔案大小的清單。 此清單必須使用下列格式。 第二個 *FileName* 參數為檔案大小。 這是已知的錯誤。
```
{"DumpFiles": [
    {
        "FileName": string,
        "FileName": string
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 下載錯誤檢查傾印檔案

**要求**

您可以透過使用下列要求格式，以下載錯誤檢查傾印檔案。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/kernel/dump
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
filename   | (**必要**) 傾印檔案的檔案名稱。 您可以透過使用 API 來取得傾印清單，以找到此項目。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含傾印檔案。 您可以使用 WinDbg 來檢查此檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得錯誤檢查損毀控制設定

**要求**

您可以透過使用下列要求格式，以取得錯誤檢查損毀控制設定。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol

<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含損毀控制設定。 如需有關 CrashControl 的詳細資訊，請參閱 [CrashControl](https://technet.microsoft.com/library/cc951703.aspx) 文章。 適用於此回應的範本如下所示。
```
{
    "autoreboot": int,
    "dumptype": int,
    "maxdumpcount": int,
    "overwrite": int
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得即時核心傾印

**要求**

您可以透過使用下列要求格式，以取得即時核心傾印。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/livekernel
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含完整的核心模式傾印。 您可以使用 WinDbg 來檢查此檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得即時使用者處理程序的傾印

**要求**

您可以透過使用下列要求格式，以取得即時使用者處理程序的傾印。
 
方法      | 要求 URI
:------     | :-----
GET | /api/debug/dump/usermode/live
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
pid   | (**必要**) 您感興趣的處理程序唯一處理程序識別碼。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含處理程序傾印。 您可以使用 WinDbg 或 Visual Studio 來檢查此檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 設定錯誤檢查損毀控制設定

**要求**

您可以透過使用下列要求格式，以設定要用於收集錯誤檢查資料的設定。
 
方法      | 要求 URI
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
autoreboot   | (**選用**) True 或 False。 這指出系統失敗或遭鎖定之後，是否會自動重新啟動。
dumptype   | (**選用**) 傾印類型。 如需支援的值，請參閱 [CrashDumpType 列舉](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx)。
maxdumpcount   | (**選用**) 要儲存的傾印數目上限。
overwrite   | (**選用**) True 或 False。 這指出在達到 *maxdumpcount* 指定的傾印計數器限制時，是否會覆寫舊的傾印。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
## ETW
---
### 透過 WebSocket 建立即時 ETW 工作階段

**要求**

您可以透過使用下列要求格式，以建立即時 ETW 工作階段。 這將會透過 Websocket 管理。  ETW 事件會在伺服器上進行批次處理，並每秒傳送至用戶端一次。 
 
方法      | 要求 URI
:------     | :-----
GET/WebSocket | /api/etw/session/realtime
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含來自已啟用之提供者的 ETW 事件。  請參閱下方的 ETW WebSocket 命令。 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

### ETW WebSocket 命令
這些命令會從用戶端傳送至伺服器。

命令 | 說明
:----- | :-----
provider *{guid}* enable *{level}* | 在指定層級啟用標示為 *{guid}* (不含括號) 的提供者。 *{level}* 是介於 1 (粗略) 至 5 (詳細) 之間的 **int**。
provider *{guid}* disable | 停用標示為 *{guid}* (不含括號) 的提供者。

此回應會從伺服器傳送至用戶端。 此回應會傳送為文字，而您可透過剖析 JSON 取得下列格式。
```
{
    "Events":[
        {
            "Timestamp": int,
            "Provider": string,
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
```
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

---
### 列舉已登錄的 ETW 提供者

**要求**

您可以透過使用下列要求格式，以列舉已登錄提供者。
 
方法      | 要求 URI
:------     | :-----
GET | /api/etw/providers
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含 ETW 提供者的清單。 清單會包含每個提供者的易記名稱與 GUID，且使用下列格式。
```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 列舉由平台公開的自訂 ETW 提供者。

**要求**

您可以透過使用下列要求格式，以列舉已登錄提供者。
 
方法      | 要求 URI
:------     | :-----
GET | /api/etw/customproviders
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

200 確定。 回應會包含 ETW 提供者的清單。 清單會包含每個提供者的易記名稱與 GUID。

```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**狀態碼**

- 標準狀態碼。
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
## OS 資訊
---
### 取得電腦名稱

**要求**

您可以透過使用下列要求格式，以取得電腦的名稱。
 
方法      | 要求 URI
:------     | :-----
GET | /api/os/machinename
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應包含下列格式的電腦名稱。 

```
{"ComputerName": string}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 取得作業系統資訊

**要求**

您可以透過使用下列要求格式，以取得電腦的 OS 資訊。
 
方法      | 要求 URI
:------     | :-----
GET | /api/os/info
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應包含下列格式的作業系統資訊。

```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 取得裝置系列 

**要求**

您可以使用下列要求格式取得裝置系列 (Xbox、手機、桌上型電腦等)。
 
方法      | 要求 URI
:------     | :-----
GET | /api/os/devicefamily
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應包含裝置系列 (SKU - Desktop、Xbox 等)。

```
{
   "DeviceType" : string
}
```

DeviceType 看起來會像 "Windows.Xbox"、"Windows.Desktop" 等。 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 設定電腦名稱

**要求**

您可以透過使用下列要求格式，以設定電腦的名稱。
 
方法      | 要求 URI
:------     | :-----
POST | /api/os/machinename
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
name | (**必要**) 電腦的新名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## 效能資料
---
### 取得執行中的處理程序清單

**要求**

您可以透過使用下列要求格式，以取得目前執行中的處理程序清單。  此清單亦可升級至 WebSocket 連線，且每秒會將相同的 JSON 資料推送至用戶端一次。 
 
方法      | 要求 URI
:------     | :-----
GET | /api/resourcemanager/processes
GET/WebSocket | /api/resourcemanager/processes
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含一份處理程序與每個處理程序之詳細資料的清單。 此資訊採用 JSON 格式，並具有下列範本。
```
{"Processes": [
    {
        "CPUUsage": int,
        "ImageName": string,
        "PageFileUsage": int,
        "PrivateWorkingSet": int,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": int,
        "WorkingSetSize": int
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 取得系統效能統計資料

**要求**

您可以透過使用下列要求格式，以取得系統效能統計資料。 此類資訊包含讀取和寫入週期以及已使用多少記憶體。
 
方法      | 要求 URI
:------     | :-----
GET | /api/resourcemanager/systemperf
GET/WebSocket | /api/resourcemanager/systemperf
<br />
此項目亦可升級至 WebSocket 連線。  其每秒會提供一次與下方相同的 JSON 資料。 

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含系統的效能統計資料，例如 CPU 與 GPU 使用量、記憶體存取以及網路存取。 此資訊採用 JSON 格式，並具有下列範本。
```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## 電源
---
### 取得目前的電池狀態

**要求**

您可以透過使用下列要求格式，以取得目前的電池狀態。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/battery
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

使用以下格式傳回目前的電池狀態資訊。
```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT
* 行動裝置版

---
### 取得使用中電源配置

**要求**

您可以透過使用下列要求格式，以取得使用中電源配置。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/activecfg
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

使用中電源配置具有下列格式。
```
{"ActivePowerScheme": string (guid of scheme)}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得電源配置的子值

**要求**

您可以透過使用下列要求格式，以取得電源配置的子值。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*
<br />
選項：
- SCHEME_CURRENT

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

根據每個應用程式與設定提供完整電源狀態清單，以標示諸如電池電力偏低或嚴重不足等各種電力狀態。 

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得系統的電源狀態

**要求**

您可以透過使用下列要求格式，以檢查系統的電源狀態。 這可讓您檢查是否處於低電源狀態。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/state
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

電源狀態資訊具有下列範本。
```
{"LowPowerStateAvailable": bool}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 設定使用中電源配置

**要求**

您可以透過使用下列要求格式，以設定使用中電源配置。
 
方法      | 要求 URI
:------     | :-----
POST | /api/power/activecfg
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
scheme | (**必要**) 您要為系統設定之使用中電源配置的配置 GUID。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 設定電源配置的子值

**要求**

您可以透過使用下列要求格式，以設定電源配置的子值。
 
方法      | 要求 URI
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
valueAC | (**必要**) 要用於 A/C 電源的值。
valueDC | (**必要**) 要用於電池電力的值。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得睡眠研究報告

**要求**

方法      | 要求 URI
:------     | :-----
GET | /api/power/sleepstudy/report
<br />
您可以透過使用下列要求格式，以取得睡眠研究報告。

**URI 參數**
URI 參數 | 描述
:---          | :---
FileName | (**必要**) 您要下載之檔案的完整名稱。 此值應該是 hex64 編碼。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應為包含休眠研究的檔案。 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 列舉可用的睡眠研究報告

**要求**

您可以透過使用下列要求格式，以列舉可用的睡眠研究報告。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/sleepstudy/reports
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

可用的報告清單具有下列範本。

```
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得睡眠研究轉換

**要求**

您可以透過使用下列要求格式，以取得睡眠研究轉換。 此轉換是 XSLT，它可將睡眠研究報告轉換成人們可以讀取的 XML 格式。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/sleepstudy/transform
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應包含休眠研究轉換。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* IoT

---
## 遠端控制
---
### 重新啟動目標電腦

**要求**

您可以透過使用下列要求格式，以重新啟動目標電腦。
 
方法      | 要求 URI
:------     | :-----
POST | /api/control/restart
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 將目標電腦關機

**要求**

您可以透過使用下列要求格式，以將目標電腦關機。
 
方法      | 要求 URI
:------     | :-----
POST | /api/control/shutdown
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## 工作管理員
---
### 啟動現代化 App

**要求**

您可以透過使用下列要求格式，以啟動現代化 App。
 
方法      | 要求 URI
:------     | :-----
POST | /api/taskmanager/app
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
appid   | (**必要**) 您要啟動之 App 的 PRAID。 此值應該是 hex64 編碼。
package   | (**必要**) 您要啟動之應用程式套件的完整名稱。 此值應該是 hex64 編碼。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 停止現代化 App

**要求**

您可以透過使用下列要求格式，以停止現代化 App。
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/taskmanager/app
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
package   | (**必要**) 您要停止之應用程式套件的完整名稱。 此值應該是 hex64 編碼。
forcestop   | (**選用**) **yes** 的值表示系統應該強制停止所有處理程序。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## 網路功能
---
### 取得目前的 IP 設定

**要求**

您可以透過使用下列要求格式，以取得目前的 IP 設定。
 
方法      | 要求 URI
:------     | :-----
GET | /api/networking/ipconfig
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應包含下列範本中的 IP 設定。

```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

--
### 列舉無線網路介面

**要求**

您可以透過使用下列要求格式，以列舉可用的無線網路介面。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wifi/interfaces
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

具有詳細資料的可用無線介面清單，使用下列格式。

``` 
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 列舉無線網路

**要求**

您可以透過使用下列要求格式，以列舉指定介面上的無線網路清單。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wifi/networks
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
interface   | (**必要**) 可用來搜尋無線網路的網路介面 GUID，不含括號。 
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

可在提供的 *interface* 上找到的無線網路清單。 這包括網路的詳細資料，使用下列格式。

```
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 連線到 Wi-Fi 網路和與它中斷連線。

**要求**

您可以透過使用下列要求格式，以連線到 Wi-Fi 網路或與它中斷連線。
 
方法      | 要求 URI
:------     | :-----
POST | /api/wifi/network
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
interface   | (**必要**) 可用來連線到網路的網路介面 GUID。
op   | (**必要**) 指出要採取的動作。 可能的值是 connect 或 disconnect。
ssid   | (****如果 op == connect** 則為必要) 要連線的 SSID。
索引鍵   | (****如果 op == connect 且網路需要驗證**則為必要) 共用金鑰。
createprofile | (**必要**) 在裝置上建立網路設定檔。  這會導致日後將裝置自動連線至網路。 此項目可為**是**或**否**。 

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 刪除 Wi-Fi 設定檔

**要求**

您可以透過使用下列要求格式，以刪除與特定介面上的網路關聯的設定檔。
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/wifi/network
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
interface   | (**必要**) 與要刪除之設定檔關聯的網路介面 GUID。
profile   | (**必要**) 要刪除的設定檔名稱。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## Windows 錯誤報告 (WER)
---
### 下載 Windows 錯誤報告 (WER) 檔案

**要求**

您可以透過使用下列要求格式，以下載 WER 檔案。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wer/report/file
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
user   | (**必要**) 與報告關聯的使用者名稱。
type   | (**必要**) 報告的類型。 這可以是 **queried** 或 **archived**。
name   | (**必要**) 報告的名稱。 此應為 base64 編碼。 
file   | (**必要**) 要從報告下載之檔案的名稱。 此應為 base64 編碼。 
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應包含所要求的檔案。 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 列舉 Windows 錯誤報告 (WER) 報告中的檔案

**要求**

您可以透過使用下列要求格式，以列舉 WER 報告中的檔案。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wer/report/files
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
user   | (**必要**) 與報告關聯的使用者。
type   | (**必要**) 報告的類型。 這可以是 **queried** 或 **archived**。
name   | (**必要**) 報告的名稱。 此應為 base64 編碼。 
<br />
**要求標頭**

- 無

**要求主體**

```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 列出 Windows 錯誤報告 (WER) 報告

**要求**

您可以使用下列要求格式來取得 WER 報告。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wer/reports
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

WER 報告使用下列格式。

```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### 使用自訂設定檔開始追蹤

**要求**

您可以透過使用下列要求格式，以上傳 WPR 設定檔，並使用該設定檔開始追蹤。  一次僅可執行一個追蹤。 此設定檔不會保留在裝置上。 
 
方法      | 要求 URI
:------     | :-----
POST | /api/wpr/customtrace
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 包含自訂 WPR 設定檔的多部分合格 http 主體。

**回應**

WPR 工作階段狀態使用下列格式。

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 啟動開機效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以啟動開機 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
方法      | 要求 URI
:------     | :-----
POST | /api/wpr/boottrace
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
profile   | (**必要**) 啟動時一定要有此參數。 應啟動效能追蹤工作階段之設定檔的名稱。 可能的設定檔儲存在 perfprofiles/profiles.json。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

啟動時，此 API 會傳回使用下列格式的 WPR 工作階段狀態。

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 停止開機效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以停止開機 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wpr/boottrace
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 啟動效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以啟動 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。  一次僅可執行一個追蹤。 
 
方法      | 要求 URI
:------     | :-----
POST | /api/wpr/trace
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
profile   | (**必要**) 應啟動效能追蹤工作階段之設定檔的名稱。 可能的設定檔儲存在 perfprofiles/profiles.json。
<br />
**要求標頭**

- 無

**要求主體**

- 無

**回應**

啟動時，此 API 會傳回使用下列格式的 WPR 工作階段狀態。

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 停止效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以停止 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wpr/trace
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無。  **注意︰**這是長時間執行的作業。  它在 ETL 完成寫入至磁碟後才會傳回。  

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 擷取追蹤工作階段的狀態

**要求**

您可以透過使用下列要求格式，以擷取目前的 WPR 工作階段狀態。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wpr/status
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

WPR 追蹤工作階段的狀態，使用下列格式。

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 列出已完成的追蹤工作階段 (ETL)。

**要求**

您可以使用下列要求格式取得裝置上 ETL 追蹤的清單。 

方法      | 要求 URI
:------     | :-----
GET | /api/wpr/tracefiles
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

已完成的追蹤工作階段清單將以下列格式提供。

```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 下載追蹤工作階段 (ETL)

**要求**

您可以使用下列要求格式下載 Tracefile (開機追蹤或使用者模式追蹤)。 

方法      | 要求 URI
:------     | :-----
GET | /api/wpr/tracefile
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
filename   | (**必要**) 要下載之 ETL 追蹤的名稱。  可以在 /api/wpr/tracefiles 找到

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 刪除追蹤工作階段 (ETL)

**要求**

您可以使用下列要求格式刪除 Tracefile (開機追蹤或使用者模式追蹤)。 

方法      | 要求 URI
:------     | :-----
DELETE | /api/wpr/tracefile
<br />

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
filename   | (**必要**) 要刪除之 ETL 追蹤的名稱。  可以在 /api/wpr/tracefiles 找到

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
## DNS-SD 標記 
---
### 檢視標記

**要求**

檢視裝置目前套用的標記。  這些標記會透過 T 索引鍵中的 DNS-SD TXT 公告。  
 
方法      | 要求 URI
:------     | :-----
GET | /api/dns-sd/tags
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應** 目前以下列格式套用的標記。 
```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
5XX | 伺服器錯誤 

<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 刪除標記

**要求**

刪除目前由 DNS-SD 公告的所有標記   
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/dns-sd/tags
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**
 - 無

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
5XX | 伺服器錯誤 

<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
### 刪除標記

**要求**

刪除目前由 DNS-SD 公告的標記   
 
方法      | 要求 URI
:------     | :-----
DELETE | /api/dns-sd/tag
<br />

**URI 參數**

URI 參數 | 描述
:------     | :-----
tagValue | (**必要**) 要移除的標記。

**要求標頭**

- 無

**要求主體**

- 無

**回應**
 - 無

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定

<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT
 
---
### 新增標記

**要求**

將標籤新增到 DNS-SD 廣告。   
 
方法      | 要求 URI
:------     | :-----
POST | /api/dns-sd/tag
<br />

**URI 參數**

URI 參數 | 描述
:------     | :-----
tagValue | (**必要**) 要新增的標記。

**要求標頭**

- 無

**要求主體**

- 無

**回應**
 - 無

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
401 | 標記空間溢位。  當建議標記對於產生的 DNS-SD 服務記錄過長時的結果。  

<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

## App 檔案總管

---
### 取得已知的資料夾

**要求**

取得可存取的最上層資料夾清單。

方法      | 要求 URI
:------     | :-----
GET | /api/filesystem/apps/knownfolders
<br />

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應** 以下列格式呈現的可用資料夾。 
```
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 部署要求已受理並正在處理
4XX | 錯誤碼
5XX | 錯誤碼
<br />

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

---
### 取得檔案

**要求**

取得資料夾中的檔案清單。

方法      | 要求 URI
:------     | :-----
GET | /api/filesystem/apps/files
<br />

**URI 參數**

URI 參數 | 描述
:------     | :-----
knownfolderid | (**必要**) 您希望取得檔案清單的最上層目錄。 使用 **LocalAppData** 存取側載 App。 
packagefullname | (****如果 knownfolderid == LocalAppData** 則為必要) 您感興趣之 App 的套件完整名稱。 
path | (**選用**) 資料夾內的子目錄或上方所指定的套件。 

**要求標頭**

- 無

**要求主體**

- 無

**回應** 以下列格式呈現的可用資料夾。 
```
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

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

---
### 下載檔案

**要求**

從已知的資料夾或 appLocalData 取得檔案。

方法      | 要求 URI
:------     | :-----
GET | /api/filesystem/apps/file

**URI 參數**

URI 參數 | 描述
:------     | :-----
knownfolderid | (**必要**) 您希望下載檔案的最上層目錄。 使用 **LocalAppData** 存取側載 App。 
filename | (**必要**) 下載的檔案名稱。 
packagefullname | (****如果 knownfolderid == LocalAppData** 則為必要) 您感興趣的套件完整名稱。 
path | (**選用**) 資料夾內的子目錄或上方所指定的套件。

**要求標頭**

- 無

**要求主體**

- 要求的檔案 (若有的話)

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求的檔案
404 | 找不到檔案
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

---
### 刪除檔案

**要求**

刪除資料夾中的檔案。

方法      | 要求 URI
:------     | :-----
DELETE | /api/filesystem/apps/file
<br />
**URI 參數**

URI 參數 | 描述
:------     | :-----
knownfolderid | (**必要**) 您希望刪除檔案的最上層目錄。 使用 **LocalAppData** 存取側載 App。 
filename | (**必要**) 刪除的檔案名稱。 
packagefullname | (****如果 knownfolderid == LocalAppData** 則為必要) 您感興趣之 App 的套件完整名稱。 
path | (**選用**) 資料夾內的子目錄或上方所指定的套件。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定。 檔案已刪除
404 | 找不到檔案
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT

---
### 上傳檔案

**要求**

上傳檔案至資料夾。  這將會使用相同名稱覆寫現有的檔案，但不會建立新的資料夾。 

方法      | 要求 URI
:------     | :-----
POST | /api/filesystem/apps/file
<br />
**URI 參數**

URI 參數 | 描述
:------     | :-----
knownfolderid | (**必要**) 您希望上傳檔案的最上層目錄。 使用 **LocalAppData** 存取側載 App。
packagefullname | (****如果 knownfolderid == LocalAppData** 則為必要) 您感興趣之 App 的套件完整名稱。 
path | (**選用**) 資料夾內的子目錄或上方所指定的套件。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 確定。 檔案已上傳
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* Xbox
* IoT



<!--HONumber=Jul16_HO2-->


