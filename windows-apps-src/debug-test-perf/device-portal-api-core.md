---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Device Portal 核心 API 參考
description: 了解可用來存取資料並以程式設計方式控制裝置的 Windows Device Portal 核心 REST API。
---

# Device Portal 核心 API 參考

Windows Device Portal 中的所有項目都是以 REST API (可讓您用來存取資料並以程式設計方式控制裝置) 為基礎所建置。

## App 部署

---
### 安裝 App

**要求**

您可以使用下列要求格式來安裝 App。

方法      | 要求 URI
:------     | :-----
POST | /api/appx/packagemanager/package

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
package   | (**必要**) 要安裝之套件的檔案名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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
GET | /api/appx/packagemanager/state

**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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
DELETE | /api/appx/packagemanager/package


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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
GET | /api/appx/packagemanager/packages


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

回應會包含一份已安裝套件與其相關詳細資料的清單。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包括 JSON 結構，其中包含階層式裝置樹狀目錄。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含每個側載應用程式的損毀傾印清單。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
fileName   | (**必要**) 應該刪除之傾印檔案的名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。
fileName   | (**必要**) 您要下載之傾印檔案的名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含傾印檔案。 您可以使用 WinDbg 或 Visual Studio 檢查傾印檔案。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
packageFullname   | (**必要**) 側載 App 的完整套件名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含一份傾印檔案名稱與這些檔案大小的清單。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
filename   | (**必要**) 傾印檔案的檔案名稱。 您可以透過使用 API 來取得傾印清單，以找到此項目。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含傾印檔案。 您可以使用 WinDbg 來檢查此檔案。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含損毀控制設定。 如需有關 CrashControl 的詳細資訊，請參閱 [CrashControl](https://technet.microsoft.com/library/cc951703.aspx) 文章。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含完整的核心模式傾印。 您可以使用 WinDbg 來檢查此檔案。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
pid   | (**必要**) 您感興趣的處理程序唯一處理程序識別碼。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含處理程序傾印。 您可以使用 WinDbg 或 Visual Studio 來檢查此檔案。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
autoreboot   | (**選用**) True 或 False。 這指出系統失敗或遭鎖定之後，是否會自動重新啟動。
dumptype   | (**選用**) 傾印類型。 如需支援的值，請參閱 [CrashDumpType 列舉](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx)。
maxdumpcount   | (**選用**) 要儲存的傾印數目上限。
overwrite   | (**選用**) True 或 False。 這指出在達到 *maxdumpcount* 指定的傾印計數器限制時，是否會覆寫舊的傾印。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows 電腦
* IoT

---
## ETW
---
### 透過 WebSocket 建立即時 ETW 工作階段

**要求**

您可以透過使用下列要求格式，以建立即時 ETW 工作階段。 這將會透過 Websocket 管理。
 
方法      | 要求 URI
:------     | :-----
GET/WebSocket | /api/etw/session/realtime


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含來自已啟用之提供者的 ETW 事件。

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 列舉已登錄的 ETW 提供者

**要求**

您可以透過使用下列要求格式，以列舉已登錄提供者。
 
方法      | 要求 URI
:------     | :-----
GET | /api/etw/providers


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含 ETW 提供者的清單。 清單會包含每個提供者的易記名稱與 GUID。

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows Mobile
* Windows 電腦
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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含 IP 設定

**狀態碼**

下表顯示可當成此作業之結果傳回的其他狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 已順利完成作業
500 | 發生內部伺服器錯誤

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
name | (**必要**) 電腦的新名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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

您可以透過使用下列要求格式，以取得目前執行中的處理程序清單。
 
方法      | 要求 URI
:------     | :-----
GET | /api/resourcemanager/processes


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含一份處理程序與每個處理程序之詳細資料的清單。 該資訊使用 JSON 格式。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 回應會包含系統的效能統計資料，例如 CPU 與 GPU 使用量、記憶體存取以及網路存取。 此資訊使用 JSON 格式。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 取得使用中電源配置

**要求**

您可以透過使用下列要求格式，以取得使用中電源配置。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/activecfg


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
### 取得睡眠研究報告

**要求**

您可以透過使用下列要求格式，以取得睡眠研究報告。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/sleepstudy/reports


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
FileName | (**必要**) 您要下載之睡眠研究報告的檔案名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows 電腦
* IoT

---
### 設定使用中電源配置

**要求**

您可以透過使用下列要求格式，以設定使用中電源配置。
 
方法      | 要求 URI
:------     | :-----
POST | /api/power/activecfg


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
scheme | (**必要**) 您要為系統設定之使用中電源配置的配置 GUID。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
valueAC | (**必要**) 要用於 A/C 電源的值。
valueDC | (**必要**) 要用於電池電力的值。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows 電腦
* IoT

---
### 取得睡眠研究轉換

**要求**

您可以透過使用下列要求格式，以取得睡眠研究轉換。 此轉換是 XSLT，它可將睡眠研究報告轉換成人們可以讀取的 XML 格式。
 
方法      | 要求 URI
:------     | :-----
GET | /api/power/sleepstudy/reports


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
appid   | (**必要**) 您要啟動之 App 的 PRAID。 此值應該是 hex64 編碼。
package   | (**必要**) 您要啟動之應用程式套件的完整名稱。 此值應該是 hex64 編碼。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
package   | (**必要**) 您要停止之應用程式套件的完整名稱。 此值應該是 hex64 編碼。
forcestop   | (**選用**) **yes** 的值表示系統應該強制停止所有處理程序。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* Xbox
* HoloLens
* IoT

---
## WiFi
---
### 列舉無線網路介面

**要求**

您可以透過使用下列要求格式，以列舉可用的無線網路介面。
 
方法      | 要求 URI
:------     | :-----
GET | /api/wifi/interfaces


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 含有詳細資料的可用無線介面清單。 詳細資料將會包含例如 GUID、描述、易記名稱等項目。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
interface   | (**必要**) 可用來搜尋無線網路的網路介面 GUID。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 可在提供的 *interface* 上找到的無線網路清單。 這包括網路的詳細資料。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
interface   | (**必要**) 可用來連線到網路的網路介面 GUID。
op   | (**必要**) 指出要採取的動作。 可能的值是 connect 或 disconnect。
ssid   | (**必要，如果 *op* == connect**) 要連線的 SSID。
key   | (**必要，如果 *op* == connect**) 共用金鑰。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
interface   | (**必要**) 與要刪除之設定檔關聯的網路介面 GUID。
profile   | (**必要**) 要刪除的設定檔名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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
GET | /api/wer/reports/file


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
user   | (**必要**) 與報告關聯的使用者名稱。
type   | (**必要**) 報告的類型。 這可以是 **queried** 或 **archived**。
name   | (**必要**) 報告的名稱。
file   | (**必要**) 要從報告下載之檔案的名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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
GET | /api/wer/reports/files


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
user   | (**必要**) 與報告關聯的使用者。
type   | (**必要**) 報告的類型。 這可以是 **queried** 或 **archived**。
name   | (**必要**) 報告的名稱。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows 電腦
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### 使用自訂設定檔開始追蹤

**要求**

您可以透過使用下列要求格式，以上傳 WPR 設定檔，並使用該設定檔開始追蹤。
 
方法      | 要求 URI
:------     | :-----
POST | /api/wpr/customtrace


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 包含自訂 WPR 設定檔的多部分合格 http 主體。

**回應**

- 傳回 WPR 工作階段狀態。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
profile   | (**必要**) 啟動時一定要有此參數。 應啟動效能追蹤工作階段之設定檔的名稱。 可能的設定檔儲存在 perfprofiles/profiles.json。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 啟動時，此 API 會傳回 WPR 工作階段狀態。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT

---
### 啟動效能追蹤工作階段

**要求**

您可以透過使用下列要求格式，以啟動 WPR 追蹤工作階段。 這也稱為效能追蹤工作階段。
 
方法      | 要求 URI
:------     | :-----
POST | /api/wpr/trace


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數 | 描述
:---          | :---
profile   | (**必要**) 應啟動效能追蹤工作階段之設定檔的名稱。 可能的設定檔儲存在 perfprofiles/profiles.json。

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 啟動時，此 API 會傳回 WPR 工作階段狀態。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- 傳回追蹤 ETL 檔案。

**狀態碼**

- 標準狀態碼。

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


**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**

- WPR 追蹤工作階段的狀態。

**狀態碼**

- 標準狀態碼。

**可用裝置系列**

* Windows Mobile
* Windows 電腦
* HoloLens
* IoT


<!--HONumber=Mar16_HO5-->


