---
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: HoloLens 的 Device Portal API 參考
description: 了解 HoloLens 的 Windows Device Portal REST API，您可以用來存取資料，並以程式設計方式控制您的裝置。
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 3aeb068908adf6d6c40a50cee3aececba1861ee8
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "63801387"
---
# <a name="device-portal-api-reference-for-hololens"></a>HoloLens 的 Device Portal API 參考

Windows Device Portal 中的所有項目都是以 REST API (可讓您用來存取資料並以程式設計方式控制裝置) 為基礎所建置。

## <a name="holographic-os"></a>全像攝影版 OS

### <a name="get-https-requirements-for-the-device-portal"></a>取得 Device Portal 的 HTTPS 需求

**要求**

您可以透過使用下列要求格式，以取得 Device Portal 的 HTTPS 需求。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/os/webmanagement/settings/https |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-stored-interpupillary-distance-ipd"></a>取得儲存的瞳孔距離 (IPD)

**要求**

您可以透過使用下列要求格式，以取得儲存的 IPD 值。 傳回的值以公釐表示。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/os/settings/ipd |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-a-list-of-hololens-specific-etw-providers"></a>取得 HoloLens 特定 ETW 提供者的清單

**要求**

您可以透過使用下列要求格式，以取得一份沒有在系統中登錄之 HoloLens 特定 ETW 提供者的清單。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/os/etw/customproviders |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。


### <a name="return-the-state-for-all-active-services"></a>傳回所有作用中服務的狀態

**要求**

您可以透過使用下列要求格式，以取得目前執行中之所有服務的狀態。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/os/services |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。


### <a name="set-the-https-requirement-for-the-device-portal"></a>設定 Device Portal 的 HTTPS 需求。

**要求**

您可以透過使用下列要求格式，以設定 Device Portal 的 HTTPS 需求。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/management/settings/https |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 必要   | (**required**) 決定 Device Portal 是否需要 HTTPS。 可能的值為 **yes**、**no** 與 **default**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。


### <a name="set-the-interpupillary-distance-ipd"></a>設定瞳孔距離 (IPD)

**要求**

您可以透過使用下列要求格式，以設定儲存的 IPD。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/os/settings/ipd |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| ipd   | (**required**) 要儲存的新 IPD 值。 此值應該以公釐表示。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。


## <a name="holographic-perception"></a>全息影像感知

### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>接受 WebSocket 升級並執行傳送更新的 Mirage 用戶端

**要求**

您可以透過使用下列要求格式，以接受 WebSocket 升級，並執行以 30 fps 傳送更新的 Mirage 用戶端。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET/WebSocket | /api/holographic/perception/client |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| clientmode   | (**必要**) 決定追蹤模式。 **active** 的值會在無法被動地建立視覺追蹤模式時強制建立它。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。


## <a name="holographic-thermal"></a>全息影像熱溫

### <a name="get-the-thermal-stage-of-the-device"></a>取得裝置的熱溫階段

**要求**

您可以透過使用下列要求格式，以取得裝置的熱溫階段。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/ |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

下表指明可能的值。

| 值 | 說明 |
| --- | --- |
| 1 | 標準 |
| 2 | 暖 |
| 3 | 重大 |

**狀態碼**

- 標準狀態碼。

## <a name="hsimulation-control"></a>HSimulation 控制項
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>建立控制項的串流，或將資料張貼到已建立的串流

**要求**

您可以透過使用下列要求格式，以建立控制項串流或將資料張貼到已建立的串流。 張貼的資料應該是 **application/octet-stream** 類型。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/control/stream |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 優先順序   | (**建立控制項串流時為必要**) 指出串流的優先順序。 |
| streamid   | (**張貼到已建立的串流時為必要**) 要張貼到之串流的識別碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="delete-a-control-stream"></a>刪除控制項串流

**要求**

您可以使用下列要求格式刪除控制項串流。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/control/stream |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-a-control-stream"></a>取得控制項串流

**要求**

您可以透過使用下列要求格式，為控制項串流開啟網路通訊端連線。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET/WebSocket | /api/holographic/simulation/control/stream |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-simulation-mode"></a>取得模擬模式

**要求**

您可以使用下列要求格式取得模擬模式。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/control/mode |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="set-the-simulation-mode"></a>設定模擬模式

**要求**

您可以使用下列要求格式設定模擬模式。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simluation/control/mode |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| mode   | (**必要**) 指出模擬模式。 可能的值包括 **default**、**simulation**、**remote** 與 **legacy**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

## <a name="hsimulation-playback"></a>HSimulation 播放

### <a name="delete-a-recording"></a>刪除錄製

**要求**

您可以使用下列要求格式刪除錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/playback/file |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要刪除之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-all-recordings"></a>取得所有錄製

**要求**

您可以使用下列要求格式取得所有可用的錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/files |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-types-of-data-in-a-loaded-recording"></a>取得已載入之錄製中的資料類型

**要求**

您可以使用下列要求格式取得已載入之錄製中的資料類型。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/types |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要取得之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-all-the-loaded-recordings"></a>取得所有已載入的錄製

**要求**

您可以使用下列要求格式取得所有已載入的錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/files |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-current-playback-state-of-a-recording"></a>取得錄製的目前播放狀態 

**要求**

您可以使用下列要求格式取得錄製的目前播放狀態。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要取得之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="load-a-recording"></a>載入錄製

**要求**

您可以使用下列要求格式載入錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/file |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要載入之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="pause-a-recording"></a>暫停錄製

**要求**

您可以使用下列要求格式暫停錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/pause |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要暫停之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="play-a-recording"></a>播放錄製

**要求**

您可以使用下列要求格式播放錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/play |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要播放之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="stop-a-recording"></a>停止錄製

**要求**

您可以使用下列要求格停止錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/stop |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要停止之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="unload-a-recording"></a>卸載錄製

**要求**

您可以使用下列要求格式卸載錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/playback/session/file |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| 記錄   | (**必要**) 要卸載之錄製的名稱。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="upload-a-recording"></a>上傳錄製

**要求**

您可以使用下列要求格式上傳錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/file |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

## <a name="hsimulation-recording"></a>HSimulation 錄製

### <a name="get-the-recording-state"></a>取得錄製狀態

**要求**

您可以使用下列要求格式取得目前的錄製狀態
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/recording/status |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="start-a-recording"></a>開始錄製

**要求**

您可以使用下列要求格式開始錄製。 一次只能有一個使用中的錄製。 
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/recording/start |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| head   | (**請參閱下面的內容**) 將此值設定為 1 來指出系統應該記錄頭部資料。 |
| hands   | (**請參閱下面的內容**) 將此值設定為 1 來指出系統應該記錄手勢資料。 |
| spatialMapping   | (**請參閱下面的內容**) 將此值設定為 1 來指出系統應該記錄空間對應資料。 |
| environment   | (**請參閱下面的內容**) 將此值設定為 1 來指出系統應該記錄環境資料。 |
| name   | (**必要**) 錄製的名稱。 |
| singleSpatialMappingFrame   | (**選用**) 將此值設定為 1 來指出應該只記錄單一空間對應框架。 |

對於這些參數，下列其中一個參數必須設定為 1：*head*、*hands*、*spatialMapping* 或 *environment*。

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="stop-the-current-recording"></a>停止目前的錄製

**要求**

您可以使用下列要求格式停止目前的錄製。 錄製會以檔案的形式傳回。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/recording/stop |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

## <a name="mixed-reality-capture"></a>混合實境擷取

### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>從裝置刪除混合實境擷取 (MRC) 錄製

**要求**

您可以使用下列要求格式刪除 MRC 錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
DELETE | /api/holographic/mrc/file |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| filename   | (**必要**) 要刪除的影片檔名稱。 名稱應該是 hex64 編碼。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="download-a-mixed-reality-capture-mrc-file"></a>下載混合實境擷取 (MRC) 檔案

**要求**

您可以透過使用下列要求格式，以從裝置下載 MRC 檔案。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/file |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| filename   | (**必要**) 要取得的影片檔名稱。 名稱應該是 hex64 編碼。 |
| op   | (**選用**) 若要下載串流，請將此值設定為 **stream**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-mixed-reality-capture-mrc-settings"></a>取得混合實境擷取 (MRC) 設定

**要求**

您可以使用下列要求格式取得 MRC 設定。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/settings |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>取得混合實境擷取 (MRC) 錄製的狀態

**要求**

您可以使用下列要求格式取得 MRC 錄製狀態。 可能的值包括 **running** 與 **stopped**。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/status |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>取得混合實境擷取 (MRC) 檔案的清單

**要求**

您可以使用下列要求格式取得儲存在裝置上的 MRC 檔案。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/files |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="set-the-mixed-reality-capture-mrc-settings"></a>設定混合實境擷取 (MRC) 設定

**要求**

您可以使用下列要求格式設定 MRC 設定。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/mrc/settings |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>開始混合實境擷取 (MRC) 錄製

**要求**

您可以使用下列要求格式開始 MRC 錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/mrc/video/control/start |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>停止目前的混合實境擷取 (MRC) 錄製

**要求**

您可以使用下列要求格式停止目前的 MRC 錄製。
 
| 方法      | 要求 URI |
| :------     | :----- |
| POST | /api/holographic/mrc/video/control/stop |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="take-a-mixed-reality-capture-mrc-photo"></a>拍攝混合實境擷取 (MRC) 相片

**要求**

您可以使用下列要求格式拍攝 MRC 相片。 會傳回 JPEG 格式的相片。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/photo |


**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

## <a name="mixed-reality-streaming"></a>混合實境串流

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>起始分段下載 mp4 片段

**要求**

您可以使用下列要求格式來起始分段下載 mp4 片段。 此 API 會使用預設品質。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live.mp4 |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| pv   | (**選用**) 指出是否擷取 PV 相機。 應該是 **true** 或 **false**。 |
| holo   | (**選用**) 指出是否擷取全像投影。 應該是 **true** 或 **false**。 |
| mic   | (**選用**) 指出是否擷取麥克風。 應該是 **true** 或 **false**。 |
| loopback   | (**選用**) 指出是否擷取應用程式音訊。 應該是 **true** 或 **false**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>起始分段下載 mp4 片段

**要求**

您可以使用下列要求格式來起始分段下載 mp4 片段。 此 API 會使用高品質。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_high.mp4 |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| pv   | (**選用**) 指出是否擷取 PV 相機。 應該是 **true** 或 **false**。 |
| holo   | (**選用**) 指出是否擷取全像投影。 應該是 **true** 或 **false**。 |
| mic   | (**選用**) 指出是否擷取麥克風。 應該是 **true** 或 **false**。 |
| loopback   | (**選用**) 指出是否擷取應用程式音訊。 應該是 **true** 或 **false**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>起始分段下載 mp4 片段

**要求**

您可以使用下列要求格式來起始分段下載 mp4 片段。 此 API 會使用低品質。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_low.mp4 |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| pv   | (**選用**) 指出是否擷取 PV 相機。 應該是 **true** 或 **false**。 |
| holo   | (**選用**) 指出是否擷取全像投影。 應該是 **true** 或 **false**。 |
| mic   | (**選用**) 指出是否擷取麥克風。 應該是 **true** 或 **false**。 |
| loopback   | (**選用**) 指出是否擷取應用程式音訊。 應該是 **true** 或 **false**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>起始分段下載 mp4 片段

**要求**

您可以使用下列要求格式來起始分段下載 mp4 片段。 此 API 會使用中品質。
 
| 方法      | 要求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_med.mp4 |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數 | 說明 |
| :---          | :--- |
| pv   | (**選用**) 指出是否擷取 PV 相機。 應該是 **true** 或 **false**。 |
| holo   | (**選用**) 指出是否擷取全像投影。 應該是 **true** 或 **false**。 |
| mic   | (**選用**) 指出是否擷取麥克風。 應該是 **true** 或 **false**。 |
| loopback   | (**選用**) 指出是否擷取應用程式音訊。 應該是 **true** 或 **false**。 |

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

- 標準狀態碼。
