---
author: msatranjr
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: "app 功能宣告"
description: "功能必須在您的通用 Windows 平台 (UWP) app 的套件資訊清單中進行宣告，才能存取特定 API、資源 (例如圖片或音樂) 或裝置 (例如相機或麥克風)。"
translationtype: Human Translation
ms.sourcegitcommit: a86efd3e50be6a5cbe0271c1024d3b405fc0d160
ms.openlocfilehash: 100d1b94dec44cbf00d4a3cacd6aa74f94c20d9c

---
# app 功能宣告

\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

功能必須在您的通用 Windows 平台 (UWP) app 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/BR211474)中進行宣告，才能存取特定 API、資源 (例如圖片或音樂) 或裝置 (例如相機或麥克風)。

您可以藉由在 app 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/BR211474)中宣告功能，來要求存取特定資源或 API。 您可以使用 Microsoft Visual Studio 中的[資訊清單設計工具](https://msdn.microsoft.com/library/windows/apps/xaml/br230259.aspx)來宣告一般的功能，也可以手動新增。 如需詳細資訊，請參閱[如何在套件資訊清單中指定功能](https://msdn.microsoft.com/library/windows/apps/BR211477)。 請務必了解，當客戶從市集取得您的 app 時，會得知該 app 宣告的所有功能。 因此，請避免宣告您的 app 不需要的功能。

部分功能提供 app 存取*敏感資源*的權限。 這些資源會視為敏感資源是因為它們可以存取使用者的個人資料或使用者必須付費才能使用。 受設定 app 管理的隱私權設定可讓使用者動態控制敏感資源的存取權。 因此，您的 app 不會假設敏感資源可隨時使用這點非常重要。 如需存取敏感資源的詳細資訊，請參閱[隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/Hh768223)。 提供 app *敏感資源*存取權的功能，其功能案例旁邊會加註星號 (\*)。

本文探討下面所述的四種功能類別。

-   適用於大部分常見 app 案例的一般用途功能。

-   可以讓您的應用程式存取周邊和內部裝置的裝置功能。

-   需要特殊公司帳戶以送出到「市集」來使用它們的特殊用途功能。 如需有關公司帳戶的詳細資訊，請參閱[帳戶類型、位置和費用](https://msdn.microsoft.com/library/windows/apps/JJ863494)。

-   只有 Microsoft 和其合作夥伴可以使用的受限制的功能。

## 一般用途功能

一般用途功能適用於大部分常見的應用程式案例。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **音樂**\* | **musicLibrary** 功能提供以程式設計方式存取使用者的 [音樂]，讓 app 不需要使用者互動，就能列舉和存取媒體櫃中的所有檔案。 這個功能通常用於點唱機 app，這類 app 需要存取整個 [音樂] 媒體櫃。<br /><br />[檔案選擇器](https://msdn.microsoft.com/library/windows/apps/BR207847)****提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器****來實現時，才需宣告 **musicLibrary** 功能。<br /><br /> 在您的 app 套件資訊清單中宣告 **musicLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。 <table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="musicLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **圖片**\* | **picturesLibrary** 功能提供以程式設計方式存取使用者 [圖片] 的功能，讓 app 不需要使用者互動，就能列舉和存取媒體櫃中的所有檔案。 這個功能通常用於相片 app，這類 app 需要存取整個 [圖片] 媒體櫃。<br /><br />[檔案選擇器](https://msdn.microsoft.com/library/windows/apps/BR207847)****提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器****來實現時，才需宣告 **picturesLibrary** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **picturesLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="picturesLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **影片**\* | **videosLibrary** 功能提供以程式設計方式存取使用者的 [影片]，允許 app 在沒有使用者互動的情況下，列舉和存取媒體櫃中的所有檔案。 這個功能通常用於電影播放 app，這類 app 需要存取整個 [影片] 媒體櫃。<br /><br />[檔案選擇器](https://msdn.microsoft.com/library/windows/apps/BR207847)****提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器****來實現時，才需宣告 **videosLibrary** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **videosLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="videosLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **抽取式存放裝置** | **removableStorage** 功能提供以程式設計方式存取抽取式存放裝置 \(例如 USB 快閃磁碟機和外接式硬碟\) 中的檔案 \(已篩選出套件資訊清單中宣告的檔案類型關聯\) 的功能。 例如，如果文件閱讀程式 app 宣告 .doc 檔案類型關聯，則可以開啟抽取式存放裝置上的 .doc 檔案，但不能開啟其他類型的檔案。 宣告這個功能時請小心，因為使用者可能會在他們的抽取式存放裝置上包含各種不同資訊，而且會預期您的 app 會針對以程式設計方式存取抽取式存放裝置上所有宣告的檔案類型提供有效的調整。<br /><br />使用者會預期您的 app 會處理宣告的所有檔案關聯。 因此，請勿宣告您的 app 無法負責處理的檔案關聯。 [檔案選擇器](https://msdn.microsoft.com/library/windows/apps/BR207847)****提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。<br /><br />只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用[檔案選擇器](https://msdn.microsoft.com/library/windows/apps/BR207847)****來實現時，才需宣告 **removableStorage** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **removableStorage** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="removableStorage"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **網際網路和公用網路**\* | 有兩個功能可以提供不同等級的網際網路和公用網路存取權。<br /><br />**internetClient** 功能指出 app 可接收從網際網路傳入的資料。 無法做為伺服器。 不具區域網路存取權。<br />**internetClientServer** 功能指出 app 可接收從網際網路傳入的資料。 可以做為伺服器。 不具區域網路存取權。<br /><br />大部分含有 Web 服務元件的 app 都將使用 **internetClient**。 啟用點對點 (P2P) 案例的 app (此 app 需要接聽連入的網路連線) 應該使用 **internetClientServer**。 **internetClientServer** 功能包含 **internetClient** 功能提供的存取權，因此指定 **internetClientServer** 就不需要指定 **internetClient**。
| **家用與工作場所網路**\* | **privateNetworkClientServer** 功能可以透過防火牆，提供家用與工作場所網路的對內和對外存取。 這個功能通常用於透過區域網路 (LAN) 通訊的遊戲，或在各種本機裝置上共用資料的 app。 如果 app 指定 **musicLibrary**、**picturesLibrary** 或 **videosLibrary**，則您不需使用這個功能來存取家用群組中對應的媒體櫃。 在 Windows 上，這個功能不提供網際網路的存取權。
| **約會** | **appointments** 功能可提供存取使用者的約會存放區。 這個功能提供從已同步網路帳戶取得之約會和其他寫入約會存放區之 app 的讀取權限。 有了這個功能，您的 app 就可以建立新的行事曆，以及將約會寫入建立的行事曆中。<br /><br />在您的 app 套件資訊清單中宣告 **appointments** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="appointments"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **連絡人**\* | **contacts** 功能可存取來自各個連絡人存放區的連絡人彙總檢視。 這個功能提供 app 有限的存取權 (套用網路許可規則)，讓 app 存取與各種網路同步的連絡人和本機連絡人存放區。<br /><br />在您的 app 套件資訊清單中宣告 **contacts** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="contacts"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **程式碼產生** | **codeGeneration** 功能讓 App 能夠存取下列函式，以提供 JIT 功能給 App。<br /><br />[**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)<br />[**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)<br />[**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)<br />[**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
| **AllJoyn** | **allJoyn** 功能讓網路上具備 AllJoyn 功能的 app 和裝置能夠進行探索且與彼此互動。<br /><br />存取 [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) 命名空間中之 API 的所有 app 都必須使用這個功能。
| **通話** | **phoneCall** 功能讓 app 能夠存取裝置上的所有電話線路，並執行下列功能。<br /><br />在不提示使用者的情況下，透過該電話線路撥打電話，並顯示系統撥號程式。<br />存取與線路相關的中繼資料。<br />存取與線路相關的觸發程序。<br />讓使用者選取的垃圾電話篩選 app 能夠設定和檢查封鎖清單及通話來源資訊。<br /><br />在您的 app 套件資訊清單中宣告 **phoneCall** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="phoneCall"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>**phoneCallHistoryPublic** 功能讓 app 能夠讀取行動電話和該裝置上的一些 VOIP 通話歷程記錄資訊。 這個功能也可讓 app 寫入 VOIP 通話歷程記錄項目。 要存取 [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) 類別的所有成員，必須要有這個功能。
| **錄製的通話資料夾**\* | **recordedCallsFolder** 裝置功能讓 app 能夠存取錄製的通話資料夾。<br /><br />在您的 app 套件資訊清單中宣告 **recordedCallsFolder** 功能時，它必須包含 **mobile** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;mobile:Capability Name="recordedCallsFolder"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **使用者帳戶資訊**\* | **userAccountInformation** 功能讓 app 能夠存取使用者的名稱和圖片。<br /><br />您需要具備這個功能，才能存取 [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.System.UserProfile) 命名空間中的某些 API。<br /><br />在您的 app 套件資訊清單中宣告 **userAccountInformation** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="userAccountInformation"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **VOIP 通話** | **voipCall** 功能讓 app 能夠存取 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空間中的 VOIP 通話 API。<br /><br />在您的 app 套件資訊清單中宣告 **voipCall** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="voipCall"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **3D 物件** | **objects3D** 功能讓 app 能以程式設計的方式存取 3D 物件檔案。 這項功能通常用於必須存取整個 3D 物件庫的 3D 應用程式和遊戲。<br /><br />需要具備這個功能，才能使用 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 命名空間中的 API 存取包含 3D 物件的資料夾。<br /><br />在您的 app 套件資訊清單中宣告 **objects3D** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="objects3d"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **讀取已封鎖訊息**\* | **blockedChatMessages** 功能讓 app 能夠讀取已由垃圾郵件篩選 app 封鎖的簡訊和多媒體簡訊訊息。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API 存取已封鎖的訊息。<br /><br />在您的 app 套件資訊清單中宣告 **blockedChatMessages** 功能時，它必須包含 **uap** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="chat"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **IoT 低階匯流排硬體** | **lowLevelDevices** 功能讓在 IoT 裝置上執行的 app 能夠存取低階匯流排硬體，例如 GPIO、I2C、SPI、ADC 和 PWM。<br /><br />需要具備此功能才能存取 [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178) 命名空間中的某些 API。<br /><br />在您的 app 套件資訊清單中宣告 **lowLevelDevices** 功能時，它必須包含 **iot** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;iot:Capability Name="lowLevelDevices"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **IoT 系統管理** | **systemManagement** 功能讓 app 具備基本的系統管理權限，例如關機或重新啟動、地區設定和時區。<br /><br />需要具備這個功能，才能存取 [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814) 命名空間中的某些 API。<br /><br />在您的 app 套件資訊清單中宣告 **systemManagement** 功能時，它必須包含 **iot** 命名空間，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;iot:Capability Name="systemManagement"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **背景媒體播放** | **backgroundMediaPlayback** 功能會變更媒體特定 API (例如 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) 和 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/windows.media.audio.audiograph.aspx) 類別) 的行為，以便在您的 app 位於背景時啟用媒體播放。 所有作用中的音訊資料流將不再設為靜音，但在 app 轉換到背景時仍可繼續聽到聲音。 此外，進行播放時，將會自動延伸應用程式存留期。
| **遠端系統** | **remoteSystem** 功能讓 app 能夠存取與使用者 Microsoft 帳戶相關聯的裝置清單。 需要存取裝置清單，才能執行任何保留在裝置上的操作。 需要具備這個功能，才能存取下列各項的所有成員。<br /><br />Windows.System.RemoteSystems 命名空間<br />Windows.System.RemoteLauncher 命名空間<br />AppServiceConnection.OpenRemoteAsync 方法

## 裝置功能

裝置功能可以讓您的 app 存取周邊和內部裝置。 裝置功能是使用 app 套件資訊清單中的 **DeviceCapability** 元素來指定。 這個元素可能需要其他子元素，而且您必須手動將一些裝置功能新增到套件資訊清單。 如需詳細資訊，請參閱[如何在套件資訊清單中指定裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)和 [**DeviceCapability 結構描述參考**](https://msdn.microsoft.com/library/windows/apps/BR211430)。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **位置**\* | **location** 功能提供存取位置功能，您可以透過專用硬體 (例如電腦中的 GPS 感應器) 來擷取這類功能，或是從可用的網路資訊加以衍生。 當使用者從設定****常用鍵停用定位服務時，app 必須能夠處理這種狀況。 |
| **麥克風** | **microphone** 功能提供存取麥克風的音訊饋送，讓 app 能夠從連接的麥克風錄音。 當使用者從設定****常用鍵停用麥克風時，app 必須能夠處理這種狀況。 |
| **近接** | **proximity** 功能可以讓多個近接的裝置彼此通訊。 這個功能通常用於輕鬆的多人遊戲，以及交換資訊的 app 中。 裝置會試著使用提供最佳連線的通訊技術，例如藍牙、Wi-Fi 以及網際網路。 這個功能只能用於起始裝置間的通訊。 |
| **網路攝影機** | **webcam** 功能提供存取內建相機或外接式網路攝影機的視訊饋送，讓 app 能夠擷取相片和影片。 在 Windows 上，當使用者從設定****常用鍵停用相機時，app 必須能夠處理這種狀況。<br/>**webcam** 功能只授與對視訊串流的存取權。 若要同時授與對音訊串流的存取權，必須新增 **microphone** 功能。 |
| **USB** | **usb** 裝置功能可讓您存取[針對 USB 裝置更新應用程式資訊清單套件](http://go.microsoft.com/fwlink/p/?LinkId=302259)中所述的 API。 |
| **人性化介面裝置 (HID)** | **humaninterfacedevice** 裝置功能可讓您存取[如何指定 HID 的裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263091)中的 API。 |
| **服務點 (POS)** | **pointOfService** 裝置功能可讓您存取 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 命名空間中的 API。 這個命名空間可以讓您的 app 存取服務指標 (POS) 條碼掃描器和磁條讀取器。 這個命名空間提供一個廠商中性介面，可讓您從 Windows 市集應用程式存取來自各個製造商的 POS 裝置。 |
| **藍牙** | **bluetooth** 裝置功能讓 app 能夠與兩個都透過 Generic Attribute (GATT) 或 Classic Basic Rate (RFCOMM) 通訊協定且配對好的藍牙裝置通訊。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413) 命名空間中的某些 API。 |
| **Wi-Fi 網路功能** | **wiFiControl** 裝置功能讓 app 能夠掃描並連線到 Wi-Fi 網路。<br/>需要具備這個功能，才能使用 [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224) 命名空間中的某些 API。 |
| **無線電狀態** | **radios** 裝置功能讓 app 能夠在 Wi-Fi 和藍牙無線電之間切換。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447) 命名空間中的 API。  |
| **光碟片** | **optical** 裝置功能讓 app 能夠存取光碟機 (例如 CD、DVD 及藍光) 上的功能。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667) 命名空間中的某些 API。 |
| **動作活動** | **activity** 裝置功能讓 app 能夠偵測裝置目前的動作。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) 命名空間中的某些 API。 |

## 特殊和受限制的功能

這類功能在某些情況下是必要且適當的，例如具備雙因素驗證的銀行系統，使用者需提供含數位憑證的智慧卡來確認身分識別。 其他應用程式主要可能是針對企業客戶所設計，而且可能需要存取公司資源，若使用者沒有網域認證，便無法存取這類公司資源。

您必須使用公司帳戶，才能將套用特殊用途功能的應用程式提交到市集。 相反地，受限制的功能不需要市集的特殊公司帳戶，開發人員無法使用它們。 只有 Microsoft 和其合作夥伴開發的 app 才能使用受限制的功能。 如需有關公司帳戶的詳細資訊，請參閱[帳戶類型、位置和費用](https://msdn.microsoft.com/library/windows/apps/JJ863494)。

和其他功能不同的是，在您的 app 套件資訊清單中宣告受限制的功能時，所有受限制的功能必須包含 **rescap** 命名空間。 下列範例顯示如何宣告 **appCaptureSettings** 功能。

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

您也必須在 Package.appxmanifest 檔案的頂端新增 **xmlns:rescap** 命名空間宣告，如下所示。

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```
**重要**  
特殊和受限制的功能適用於非常特殊的情況。 這些功能的用法受到高度限制，而且受其他市集上架原則和審查規定的規範。 將您的 app 提交到市集之前，請依照下列步驟來要求存取受限制的功能。
1. 查看下表，以判斷您是否有資格能夠利用特定的受限制功能，將 app 提交到市集。 如果您不符合資格，則您所做的任何要求將會遭到拒絕。
2. 如果您符合資格，請瀏覽[提交應用程式](https://go.microsoft.com/fwlink/p/?LinkId=331509)支援頁面。 
3. 將問題類型設定為**應用程式提交和認證**，並將類別類型設定為**使用受限制的功能提交應用程式**。
4. 包含您要求存取的功能，並包含您提出要求的原因。 如果您未提供所有必要資訊，您的要求將會遭到拒絕。 您可能還需要提供更多資訊。

**注意** 我們將個別檢閱要求，因此可能需要 5 個工作天或較長的時間來回應。 我們強烈建議您能夠在將 app 提交到市集之前適當地提交您的要求。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **企業版** | Windows 網域認證可以讓使用者使用自己的認證登入遠端資源，如同使用者提供自己的使用者名稱和密碼一樣。 **enterpriseAuthentication** 特殊功能通常用於企業中與伺服器連線的商務營運用程式。 <br /><br />您不需要針對網際網路上的一般通訊使用此功能。<br /><br />**enterpriseAuthentication** 特殊功能是用來支援常見的商務營運用程式。 請勿在不需要存取公司資源的 app 中宣告此功能。 [檔案選擇器](https://msdn.microsoft.com/library/windows/apps/BR207847)****提供健全的 UI 機制，能夠讓使用者開啟網路共用上要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器****來實現時，才需宣告 **enterpriseAuthentication** 特殊功能。<br /><br />在您的 app 套件資訊清單中宣告 **enterpriseAuthentication** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br /><div class="code"><span codelanguage="XML"></span><table><colgroup><col width="100%" /></colgroup><thead><tr class="header"><th align="left">XML</th></tr></thead><tbody><tr class="odd"><td align="left"><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="enterpriseAuthentication"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table></div>**enterpriseDataPolicy** 功能可讓 app 定義和使用裝置適用的企業特定原則。 若要使用下列類別的所有成員，必須要有這個功能。<ul><li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li><li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li><li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li></ul><br /><br />任何人都可能要求存取這個功能以進行市集提交。</td></tr>
| **共用使用者憑證** | **sharedUserCertificates** 特殊功能可讓 app 在「共用使用者」存放區中新增和存取軟體和硬體型憑證，例如儲存在智慧卡的憑證。 這個功能通常用於需要使用智慧卡進行身分驗證的金融或企業 app。<br /><br />在您的 app 套件資訊清單中宣告 **sharedUserCertificates** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br /><div class="code"><span codelanguage="XML"></span><table><colgroup><col width="100%" /></colgroup><thead><tr class="header"><th align="left">XML</th></tr></thead><tbody><tr class="odd"><td align="left"><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="sharedUserCertificates"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table></div><br /><br />沒有人能夠要求存取這個功能以進行市集提交。 
|**文件**\* | **documentsLibrary** 功能提供以程式設計方式存取使用者的 \[文件\] \(已篩選出套件資訊清單中宣告的檔案類型關聯\) 的功能，以支援對 OneDrive 進行離線存取。 例如，如果 DOC 閱讀程式 app 宣告 .doc 檔案類型關聯，則它可以開啟 \[文件\] 中的 .doc 檔案，但無法開啟其他類型的檔案。 <br /><br />宣告 **documentsLibrary** 特殊功能的 app 無法存取家用群組電腦上的 [文件]。 [檔案選擇器](https://msdn.microsoft.com/library/windows/apps/Hh465174)提供健全的 UI 機制，能夠讓使用者開啟要供 app 使用的檔案。 只有在您無法使用檔案選擇器時，才宣告 **documentsLibrary** 特殊功能。<br /><br />若要使用 **documentsLibrary** 特殊功能，app 必須：<ul><li>使用有效的 OneDrive URL 或資源識別碼，協助對特定的 OneDrive 內容進行跨平台離線存取。</li><li>在離線時自動將開啟的檔案儲存到使用者的 OneDrive</li></ul>針對這兩個目的而使用 **documentsLibrary** 特殊功能的 app，也可選擇使用此功能開啟另一份文件內的內嵌內容。 只接受上述使用 **documentsLibrary** 特殊功能的方式。<ul><li>您的 app 無法存取手機內部儲存空間中的文件庫。 不過，如果另一個 app 在選用的 SD 記憶卡上建立 \[文件\] 資料夾，您的 app 能夠看到該資料夾。</li></ul>在您的 app 套件資訊清單中宣告 **documentsLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。<div class="code"><span codelanguage="XML"></span><table><colgroup><col width="100%" /></colgroup><thead><tr class="header"><th align="left">XML</th></tr></thead><tbody><tr class="odd"><td align="left"><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="documentsLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table></div><br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **遊戲 DVR 設定** | **appCaptureSettings** 受限制的功能讓 app 能夠控制「遊戲 DVR」的使用者設定。<br /><br />需要具備這個功能，才能使用 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738) 命名空間中的某些 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **行動數據** | **cellularDeviceControl** 受限制的功能讓 app 能夠控制行動電話通訊裝置。<br /><br />**cellularDeviceIdentity** 功能讓 app 能夠存取行動電話通訊識別資料。<br /><br />**cellularMessaging** 功能讓 app 能夠使用簡訊和 RCS。<br /><br />需要具備這些功能，才能使用 [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567) 命名空間中的某些 API。<br /><br />從 Windows 10 開始，app 會呼叫 [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996))。 <br /><br />任何人都可能要求存取這些功能以進行市集提交。
| **裝置解除鎖定** | **deviceUnlock** 受限制的功能讓 app 能夠解除鎖定裝置，以供開發人員和企業側載案例使用。<br /><br /> 沒有人能夠要求存取這個功能以進行市集提交。
| **雙 SIM 卡磚** | **dualSimTiles** 受限制的功能讓 app 能夠在配備多張 SIM 卡的裝置上建立其他 app 清單項目。<br /><br />需要具備這個功能，才能使用 [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235) 命名空間中的某些 API。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **企業共用存放裝置** | **enterpriseDeviceLockdown** 受限制的功能讓 app 使用裝置鎖定 API，以及存取企業共用存放裝置資料夾。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **系統輸入導入** | **inputInjection** 受限制的功能讓 app 能夠以程式設計方式，將各種不同形式的輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠) 導入系統中。 這個功能通常用於可控制系統的共同作業應用程式。<br /><br /><div class="alert">**注意** 針對電腦，從具備此功能的 app 導入的輸入源，只能透過位於相同應用程式容器的處理程序來接收。</div><br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **觀察輸入**\* | **inputObservation** 受限制的功能讓 app 能夠觀察系統所接收到的各種不同形式的原始輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠)，而不論其最終目的地為何。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **隱藏輸入** | **inputSuppression** 受限制的功能讓 app 能夠隱藏系統所接收到的各種不同形式的原始輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠)。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **VPN 應用程式** | **networkingVpnProvider** 受限制的功能讓 app 能夠具備 VPN 功能的完整存取權，包括管理連線的能力，以及提供 VPN 外掛程式功能。<br /><br />需要具備這個功能，才能使用 [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040) 命名空間中的某些 API。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **其他 App 管理** | **packageManagement** 受限制的功能讓 app 能夠直接管理其他 app。<br /><br />**packageQuery** 裝置功能讓 app 能夠收集其他 app 的相關資訊。<br /><br />需要具備這個功能，才能使用存取 [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960) 類別中的某些方法和屬性。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **畫面投影** | **screenDuplication** 受限制的功能讓 app 能將畫面投影到其他裝置。<br /><br />需要具備這個功能，才能使用 DirectX 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **使用者主體名稱** | **userPrincipalName** 受限制的功能讓 app 能夠修改和存取相片的縮圖快取。<br /><br />需要具備這個功能，才能呼叫 [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435) 函式。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **電子錢包** | **walletSystem** 受限制的功能讓 app 能完整存取儲存式電子錢包卡。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **位置歷程記錄** | **locationHistory** 受限制的功能讓 app 能夠存取裝置的位置歷程記錄。<br /><br />需要具備這個功能，才能使用 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603) 命名空間中的 API。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **App 關閉確認** | **confirmAppClose** 受限制的功能讓 app 能夠關閉本身和它們自己的視窗，並延遲關閉它們的 app。<br /><br />需要具備這個功能，才能使用 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **通訊記錄**\* | **phoneCallHistory** 受限制的功能讓 app 能夠讀取通訊記錄並刪除記錄中的項目。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **系統層級的約會存取** | **appointmentsSystem** 受限制的功能讓 app 能夠讀取和修改使用者行事曆上的所有約會。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **系統層級的聊天訊息存取**\* | **chatSystem** 受限制的功能讓 app 能夠讀取和寫入所有簡訊和多媒體簡訊訊息。<br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **系統層級的連絡人存取** | **contactsSystem** 受限制的功能讓 app 能夠讀取已指定為受限制或敏感的連絡人資訊，並修改現有的連絡人資訊。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **電子郵件存取*** | **email** 受限制的功能讓 app 能夠讀取、分級，以及傳送電子郵件給使用者。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **系統層級的電子郵件存取**| **emailSystem** 受限制的功能讓 app 能夠讀取、分級，以及傳送受限制或敏感的電子郵件給使用者。 <br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **系統層級的通話記錄存取** | **phoneCallHistorySystem** 受限制的功能讓 app 能夠透過變更現有項目及撰寫新項目來完整修改通話記錄。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空間中的 API。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **傳送簡訊**\* | **smsSend** 受限制的功能讓 app 能夠傳送簡訊和多媒體簡訊訊息。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **所有使用者資料的系統層級存取** | **userDataSystem** 受限制的功能讓 app 能夠存取使用者資料系統資料存放區。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **市集預覽功能** | **previewStore** 受限制的功能讓 app 能夠擷取並購買 app 內產品的 SKU。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546) 命名空間中的特定 API。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **第一次登入設定** | **firstSignInSettings** 受限制的功能讓 app 能夠存取使用者首次登入其裝置時所設定的使用者設定。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **Windows 小組體驗** | **teamEditionExperience** 受限制的功能讓 app 能夠存取控制 Windows 小組工作階段各個體驗層面的內部 API。 Windows 小組工作階段極有可能會在小組裝置 (如 Microsoft Surface Hub) 上執行。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **遠端解除鎖定** | **remotePassportAuthentication** 受限制的功能讓 app 能夠存取可以用來解除遠端電腦鎖定的認證。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **預覽組合** | **previewUiComposition** 受限制的功能讓 app 能夠預覽其使用者介面的 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) 命名空間，以便他們可以在完成 API 之前提供意見反應。 如需詳細資訊，請連絡 wincomposition@microsoft.com。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **安全性評定鎖定** | **secureAssessment** 受限制的功能讓 app 能夠將 Windows 鎖定為單一 app 模式以進行安全性評定。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **連線管理員佈建** | **networkConnectionManagerProvisioning** 受限制的功能讓 app 能夠定義可將裝置連線到 WWAN 和 WLAN 介面的原則。 電信業者會建立使用這個功能的 app，以便管理連線至其行動網路的裝置。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **行動數據方案佈建** | **networkDataPlanProvisioning** 受限制的功能讓 app 能夠收集裝置上行動數據方案的相關資訊，並讀取網路使用量。 電信業者會建立使用這個功能的 app，以便將其客戶的實際數據使用量整合到 OS 數據使用量設定。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **軟體授權** | **slapiQueryLicenseValue** 受限制的功能讓 app 能夠查詢軟體授權原則。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **延伸執行** | **extendedExecutionBackgroundAudio** 受限制的功能讓 app 能夠在背景執行時也能播放音訊。<br /><br />**extendedExecutionCritical** 受限制的功能讓 app 能夠開始嚴重延伸執行工作階段。<br /><br />**extendedExecutionUnconstrained** 受限制的功能讓 app 能夠開始不受限制的延伸執行工作階段。 <br /><br />沒有人能夠要求存取這些功能以進行市集提交。
| **行動裝置管理** | **deviceManagementDmAccount** 受限制的功能讓 app 能夠佈建和設定 [電信業者開放行動裝置聯盟 - 裝置管理 (MO OMA-DM)] 帳戶。<br /><br />**deviceManagementFoundation** 受限制的功能讓 app 能夠基本存取裝置上的行動裝置管理 (MDM) 設定服務提供者 (CSP) 基礎結構。 請注意，需有其他功能才能存取特定 CSP。<br /><br />**deviceManagementWapSecurityPolicies** 受限制的功能讓 app 能夠設定無線應用通訊協定 (WAP) 服務，例如 MMs、服務指示/服務載入 (SI/SL) 及開放行動裝置聯盟 - 用戶端佈建 (OMA-CP)。<br /><br />**deviceManagementEmailAccount** 受限制的功能讓 app 能夠由電信業者建立，以便在他們佈建給使用者的裝置上新增和管理電子郵件帳戶。<br /><br />任何人都可能要求存取這些功能以進行市集提交。
| **套件原則控制項** | **packagePolicySystem** 受限制的功能讓 app 能夠掌控裝置上所安裝之 app 的相關系統原則。<br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **遊戲清單** | **gameList** 受限制的功能讓 app 能夠取得一份系統上已安裝的知名遊戲清單。<br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **Xbox 配件** | **xboxAccessoryManagement** 受限制的功能讓 app 能夠直接管理符合 Xbox 硬體規格的 Xbox 裝置。<br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **附屬應用程式的語音辨識** | **cortanaSpeechAccessory** 受限制的功能讓 app 能夠叫用並傳遞命令到 Cortana。<br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **配件管理** | **accessoryManager** 受限制的功能讓 app 能夠註冊為配件專屬 App，並選擇加入特定 app 通知，以便將它們轉送到配件並向使用者顯示。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **驅動程式存取** | **interopServices** 受限制的功能讓 app 能夠直接與驅動程式互動。 <br /><br />只有 Microsoft 合作夥伴能夠要求存取這個功能以進行市集提交。
| **前景觀察** | **inputForegroundObservation** 受限制的功能讓前景的 app 能夠攔截鍵盤輸入，並繞過所有非 app 鍵盤輸入的處理。 此功能無法攔截 SAS 組合。 需要具備這個功能，才能存取 [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395) 類別的成員。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **OEM 和 MO 合作夥伴 app** | **oemDeployment** 受限制的功能讓 Microsoft 合作夥伴建立的 app 可以在裝置上安裝新的 app 和查詢目前安裝的 app。 任何人都可能要求存取這個功能以進行市集提交。<br /><br />**oemPublicDirectory** 受限制的功能讓 Microsoft 合作夥伴建立的 app 可以存取共用 app 資料夾。 <br /><br />只有 Microsoft 合作夥伴能夠要求存取這個功能以進行市集提交。
| **應用程式授權** | **appLicensing** 受限制的功能可允許 app 在不需要授權的情況下執行。 如果您在資訊清單中宣告此功能，您就無法將 app 提交至市集。 <br /><br />任何要求存取此功能以進行市集提交的要求將會一律被拒絕。
| **位置系統**| **locationSystem** 受限制的功能可允許 app 執行特定需要特殊權限的位置設定，例如設定裝置的預設位置。 如果您在資訊清單中宣告此功能，您就無法將 app 提交至市集。 任何要求存取此功能以進行市集提交的要求將會一律遭到拒絕。 <br /><br />沒有人能夠要求存取這個功能以進行市集提交。
| **使用者資料帳戶提供者**| **userDataAccountsProvider** 受限制的功能讓 app 能夠完全管理電子郵件、行事曆和連絡人帳戶。 <br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **手寫筆工作區** | **previewPenWorkspace** 功能讓 app 能夠存取 Windows.ApplicationModel.Preview.Notes 命名空間，此命名空間要裝載於手寫筆工作區內部，以做為記住動作處理常式。<br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **第二個驗證因素** | **secondaryAuthenticationFactor** 功能讓 app 能夠透過傳送儲存於鄰近隨附驗證裝置上的密碼來解除鎖定電腦。 例如，隨附的健身手環可以用來解除鎖定電腦。 需要具備這個功能，才能存取 Windows.Security.Authentication.Identity.Provider 命名空間中的 API。<br /><br />只有 Microsoft 合作夥伴以及與裝置廠商合作的人員能夠要求存取這個功能以進行市集提交。
| **市集授權管理**| **storeLicenseManagement** 功能讓 Microsoft 合作夥伴中樞應用程式能夠管理裝置上的市集授權。 需要具備這個功能，才能存取 Windows.ApplicationModel.Store.LicenseManagement 命名空間中的 API。<br /><br />任何人都可能要求存取這個功能以進行市集提交。
| **使用者系統識別碼**| **userSystemId** 功能讓 app 能夠取得使用者特定的系統識別碼。 這個識別碼可唯一識別特定系統上目前的使用者，也可以用來使應用程式間的資訊互相關聯。 需要具備這個功能，才能存取 Windows.System.Profile.SystemIdentification 類別中的 GetUserSpecificSystemId API。<br /><br />任何人都可能要求存取這個功能以進行市集提交。



**注意**  
此文章適用於撰寫 UWP app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

* [資訊清單設計工具](https://msdn.microsoft.com/library/windows/apps/xaml/br230259.aspx)
* [隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [如何在套件資訊清單中指定功能](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [如何在套件資訊清單中指定裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 



<!--HONumber=Aug16_HO3-->


