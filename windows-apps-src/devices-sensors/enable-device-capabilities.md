---
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: 啟用裝置功能
description: 本教學課程描述如何在 Microsoft Visual Studio 中宣告裝置功能。 這可以讓您的 app 使用相機、麥克風、定位感應器及其他裝置。
---
# 啟用裝置功能

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本教學課程描述如何在 Microsoft Visual Studio 中宣告裝置功能。 這可以讓您的 app 使用相機、麥克風、定位感應器及其他裝置。

## 指定您 app 將要使用的裝置功能


當您使用特定類型的裝置時，Windows app 需要您在 app 套件資訊清單中指定。 在 Visual Studio 中，您可以使用[資訊清單設計工具](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)宣告大多數的功能，或是如同[如何在套件資訊清單中指定裝置功能 (手動)](https://msdn.microsoft.com/library/windows/apps/Dn263092) 中所述的方式，手動新增它們。 本教學課程假設您使用的是資訊清單設計工具。

**注意**  
有些裝置類型 (例如印表機、掃描器和感應器) 不需要在 app 套件資訊清單中宣告。

-   在 Visual Studio [方案總管] 中，按兩下套件資訊清單檔案 **Package.appxmanifest**。
-   開啟 [功能]**** 索引標籤。
-   選取您的應用程式使用的裝置功能。 如果沒有在資訊清單設計工具中看到您要尋找的功能，可手動新增。 如需詳細資訊，請參閱[如何在套件資訊清單中指定裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)。

| 裝置功能 | 資訊清單設計工具 | 說明 |
|-------------------|-------------------|-------------|    
| 全部 Joyn | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓網路上具備 AllJoyn 功能的 app 和裝置能夠進行探索且與彼此互動。 存取 [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) 命名空間中之 API 的所有 app 都必須使用這個功能。 |
| 封鎖的聊天訊息 | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓 app 能夠讀取已由垃圾郵件篩選 app 封鎖的 SMS 和 MMS 訊息。 |
| 輸入聊天存取 | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓 app 能夠讀取及刪除文字訊息。 它也能讓 app 將聊天訊息儲存於系統資料存放區中。 |
| 程式碼產生 | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓 app 能夠動態產生程式碼。 |
| 企業驗證 | ![資訊清單設計工具中提供](images/ap-tools.png) | 這個功能會受限於 Windows 市集原則。 它提供可連線至需要網域認證之企業內部網路資源的功能。 大部分的 app 通常不太需要用到這個功能。 | 
| 網際網路 (用戶端) | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供公共場所的網際網路與網路的對外存取，例如機場與咖啡廳。 例如，使用者指定網路為公用的內部網路位置。 大部分需要網際網路存取的 app 都應使用此功能。 |
| 網際網路 (用戶端與伺服器) | ![資訊清單設計工具中提供](images/ap-tools.png) | 在公共場所 (例如機場與咖啡廳) 提供網際網路與網路的對內及對外存取。 這個功能是 **Internet (Client)** 的超集。 如果已啟用此功能，則 **Internet (Client)** 不需要經過啟用。 一律封鎖對重要連接埠的對內存取。 |
| 位置| ![資訊清單設計工具中提供](images/ap-tools.png) | 提供目前位置的存取。 這是透過專用硬體 (如電腦中的 GPS 感應器) 取得或由可用的網路資訊衍生的位置。 | 
| 麥克風 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供麥克風音訊摘要的存取。 這樣可讓 app 從連接的麥克風錄音。 | 
| 音樂媒體櫃 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供在本機電腦和 **HomeGroup** 電腦中，新增、變更或刪除**音樂媒體櫃**中檔案的功能。 | 
| 物件 3D | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓使用者以程式設計方式存取他們的 [**立體物件**]，允許 app 在沒有使用者互動的情況下，列舉和存取媒體櫃中的所有檔案。 這項功能通常用於必須存取整個 [**立體物件**] 庫的 3D app 和遊戲。 | 
| 通話 | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓 app 能夠存取所有裝置上的電話線路，並執行下列功能：在無需提示使用者的情況下，在手機上撥打電話並顯示系統撥號程式、存取與線路相關的中繼資料、存取與線路相關的觸發程序。 讓使用者選取的垃圾電話篩選 app 能夠設定和檢查封鎖清單及通話來源資訊。 | 
| 圖片媒體櫃 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供在本機電腦和 **HomeGroup** 電腦中，新增、變更或刪除**圖片媒體櫃**中檔案的功能。 | 
| 私人網路 (用戶端和伺服器) | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供對內和對外存取權限給具有已驗證網域控制站或使用者已指定為家用或工作場所網路的內部網路。 一律封鎖對重要連接埠的對內存取。 | 
| 近接 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供透過近距離無線通訊 (NFC) 將鄰近裝置連線到電腦的能力。 近距離鄰近性可用來傳送檔案或與鄰近裝置上的 app 通訊。 | 
| 卸除式存放裝置 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供新增、變更或刪除卸除式存放裝置上檔案的功能。 app 在卸除式存放裝置上可以存取的檔案類型，只有在資訊清單中使用**檔案類型關聯**宣告所定義的檔案類型。 app 無法存取**家用群組**電腦上的卸除式存放裝置。 | 
| 共用使用者憑證 | ![資訊清單設計工具中提供](images/ap-tools.png) | 這個功能會受限於 Windows 市集原則。 它提供存取軟體和硬體憑證 (例如智慧卡憑證)，進行使用者身分識別驗證的功能。 系統會在執行階段中叫用相關的 API，使用者必須採取動作 (插入卡、選取憑證等)。 如果您的 app 包含透過 **Certificates** 宣告的私密憑證，則不需要這個功能。 | 
| 使用者帳戶資訊 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供 app 存取使用者名稱和圖片的能力。 您需要具備這個功能，才能存取 [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) 命名空間中的某些 API。 | 
| 視訊庫 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供在本機電腦和 **HomeGroup** 電腦中，新增、變更或刪除**視訊庫**中檔案的功能。 | 
| VOIP 通話 | ![資訊清單設計工具中提供](images/ap-tools.png) | 讓 app 能夠在 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空間中存取 VOIP 通話 API。 | 
| 網路攝影機 | ![資訊清單設計工具中提供](images/ap-tools.png) | 提供存取內建相機或連接的網路攝影機的視訊輸出。 這可讓應用程式擷取快照和影片。 | 
| USB | | 提供自訂 USB 裝置的存取。 此功能需要子元素。 Windows Phone 不支援此功能。 | 
| 人性化介面裝置 (HID) | | 提供人性化介面裝置 (HID) 的存取。 此功能需要子元素。 如需詳細資訊，請參閱[如何指定 HID 的裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263091)。 | 
| 藍牙 GATT | | 透過主要服務的集合 (包括服務、特性和描述元)，提供藍牙 LE 裝置的存取。 此功能需要子元素。 如需詳細資訊，請參閱[如何指定藍牙的裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263090)。 | 
| 藍牙 RFCOMM |  | 提供可支援基本速率/延伸資料速率 (BR/EDR) 傳輸之 API 的存取，也讓您的 Windows 市集應用程式存取可實作序列埠設定檔 (SPP) 的裝置。 此功能需要子元素。 如需詳細資訊，請參閱[如何指定藍牙的裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263090)。 |
| pointOfService |  | 提供服務指標 (POS) 條碼掃描器和磁條讀取器的存取。 Windows Phone 不支援此功能。 | 

## 使用 Windows 執行階段 API 與您的裝置進行通訊

下表會將部份功能連線到 Windows 執行階段 API。

| 裝置功能        | API             | 
|--------------------------|-----------------|
| 全部 Joyn                 | [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) | 
| 封鎖的聊天訊息    | [**Windows.ApplicationModel.CommunicationBlocking**](https://msdn.microsoft.com/library/windows/apps/Dn974207) | 
| 位置                 | 如需詳細資訊，請參閱[地圖與位置概觀](https://msdn.microsoft.com/library/windows/apps/Mt219699)。 | 
| 通話               | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| 使用者帳戶資訊 | [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) | 
| VOIP 通話             | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| USB                      | [**Windows.Devices.Usb**](https://msdn.microsoft.com/library/windows/apps/Dn278466) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://msdn.microsoft.com/library/windows/apps/Dn264174) | 
| 藍牙 GATT           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) | 
| 藍牙 RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) | 
| 服務指標 (POS)   | [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) |



<!--HONumber=Mar16_HO1-->


