---
author: msatranjr
Description: This topic describes performance guidelines for apps that require access to a user's location.
title: 定位感知應用程式的指導方針
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 位置, 地圖, 地理位置
ms.localizationpriority: medium
ms.openlocfilehash: 903a7b308c78e4ab9826ea4c46c642cb3361b462
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5439983"
---
# <a name="guidelines-for-location-aware-apps"></a>定位感知應用程式的指導方針




**重要 API**

-   [**地理位置**](https://msdn.microsoft.com/library/windows/apps/br225603)
-   [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)

這個主題說明需要存取使用者位置之 app 的效能指導方針。

## <a name="recommendations"></a>建議事項


-   只在應用程式要求位置資料時開始使用位置物件。

    請在存取使用者的位置之前，先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)。 此時，您的 app 必須在前景，且 **RequestAccessAsync** 必須是從 UI 執行緒呼叫。 在使用者授與您的 app 存取其位置的權限之前，您的 app 將無法存取位置資料。

-   如果定位功能對您的應用程式來說並非必要，除非使用者嘗試完成需要此功能的工作，否則請不要存取它。 舉例來說，如果社交網路應用程式有一個 [用我的位置登入] 按鈕，除非使用者按一下該按鈕，否則應用程式不應該存取位置。 如果您應用程式的主要功能需要，可以立即存取位置。

-   第一次使用 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 物件，必須是在前景 app 的主要 UI 執行緒上，以便向使用者觸發同意提示。 第一次使用 **Geolocator** 可以是第一次呼叫 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)，也可以是 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件的第一次處理常式登錄。

-   將您會如何使用位置資料的資訊告訴使用者。
-   提供可讓使用者手動重新整理他們位置的 UI。
-   等候取得位置資料時，顯示進度列或進度環。 <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   在停用或無法取得定位服務時顯示適當的錯誤訊息或對話方塊。

    如果位置設定不允許您的應用程式存取使用者的位置，建議您提供一個可連到 [**設定**] app 中 [**位置隱私權設定**] 的便利連結。 例如，您可以使用超連結控制項或呼叫 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，使用 `ms-settings:privacy-location` URI 從程式碼啟動 [**設定**] app。 如需詳細資訊，請參閱[啟動 Windows 設定 app](https://msdn.microsoft.com/library/windows/apps/mt228342)。

-   在使用者停用位置資訊存取功能時，清除快取的位置資料並釋放 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)。

    如果使用者透過 [設定] 關閉位置資訊的存取，則釋放 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 物件。 然後，針對任何定位 API 呼叫，app 將會收到 **ACCESS\_DENIED** 結果。 如果您的 app 會儲存或快取位置資料，請在使用者撤銷存取位置資訊時清除所有快取資料。 在無法透過定位服務使用位置資訊時，提供手動輸入位置的替代方法。

-   提供用來重新啟用定位服務的 UI。 例如，提供重新整理] 按鈕，reinstantiates [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)物件，並嘗試再次取得位置資訊。

    讓您的 app 提供可重新啟用定位服務的 UI—

    -   如果使用者在停用位置存取之後重新啟用，app 將不會收到通知。 [**status**](https://msdn.microsoft.com/library/windows/apps/br225601) 屬性不會變更，也不會有 [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件。 您的應用程式應該建立新的 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 物件並呼叫 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 以嘗試取得更新的位置資料，或是再次訂閱 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件。 然後，如果狀態指出已重新啟用定位，請清除您的 app 先前通知使用者定位服務已停用的所有 UI，然後適當回應新狀態。
    -   您的應用程式也應該在啟用時、使用者明確嘗試使用需要位置資訊的功能時，或在任何適用情況下，重新嘗試取得位置資料。

**效能**

-   如果您的應用程式不需要接收位置更新，請使用單次定位要求。 例如，為相片加上位置標記的應用程式不需要接收位置更新事件。 它應該改用 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 要求定位，如[取得目前位置](https://msdn.microsoft.com/library/windows/apps/mt219698)中所述。

    當您發出單次定位要求時，應該設定下列的值。

    -   設定 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 或 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271)，指定應用程式要求的準確度。 如需使用這些參數的建議，請參閱下文
    -   設定 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 的最大使用期限參數，指定多久之前取得位置，對於您的應用程式才有幫助。 如果您的應用程式能夠使用已存在數秒或數分鐘的位置，則可以立即接收位置，這有助於節省裝置電源。
    -   設定 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 的逾時參數。 這是您的應用程式等待定位的時間，超過這個時間就會傳回錯誤。 您必須在回應使用者的速度與應用程式所需的準確度之間做出取捨。
-   如需頻繁更新位置，請使用持續定位工作階段。 使用 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 和 [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件偵測超過特定閾值的移動，或在事件發生時持續進行位置更新。

    要求位置更新時，您可以設定 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 或 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271)，指定應用程式要求的準確度。 您也應該使用 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 或 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)，在需要位置更新的項目設定頻率。

    -   指定移動閾值。 有些應用程式僅在使用者移動很大的距離時才需要位置更新。 例如，提供當地新聞或天氣更新的應用程式，除非使用者定位變更為不同城市時，才需要位置更新。 在這種情況下，您要設定 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 屬性，為位置更新事件調整必要移動最小值。 這會產生篩選掉 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件的效果。 只在位置變更超過移動閾值時，才會引發這些事件。

    -   使用符合您應用程式體驗且盡可能減少使用系統資源的 [**reportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)。 例如，天氣應用程式可能只需要每 15 分鐘更新一次資料。 大部分的應用程式 (即時導航應用程式除外) 不需要高度精確且持續串流的位置更新。 如果您的應用程式不需要最精確的可用資料串流，或者不常需要更新，請設定 **ReportInterval** 屬性來指示應用程式所需的最小位置更新頻率。 如此一來，位置來源就可以只在需要時才計算位置，以便節省電力。

        需要即時資料的應用程式應該將 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541) 設定為 0，表示不指定最小間隔。 預設報告間隔是 1 秒或硬體可支援的最短頻率 – 以兩者中較短者為主。

        提供位置資料的裝置可以追蹤不同應用程式要求的報告間隔，並以最小要求間隔提供資料報告。 這樣最需要精確度的應用程式就會收到符合需求的資料。 因此，如果另一個 app 要求更高頻率的更新時，定位提供者可能會以比您 app 所要求之頻率還要高的頻率產生更新。

        **注意**  位置來源不保證接受指定報告間隔的要求。 並非所有的定位提供者裝置都會追蹤報告間隔，但是您仍然應該為會追蹤的裝置提供此間隔。

    -   為了幫助節省電源，請設定 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 屬性，向定位平台指出您的應用程式是否需要高精確度的資料。 如果沒有 app 需要高精確度的資料，系統可以藉由不開啟 GPS 提供者以便節省電源。

        -   將 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 設定為 **HIGH** 以使 GPS 取得資料。
        -   如果您的 app 僅針對廣告對象用途使用位置資訊，請將 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 設定為 **Default**，且只使用單次呼叫模式，以將耗電量降至最低。

        如果您的 app 有特定的準確度需求，您可以考慮使用 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271) 屬性，而不是使用 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535)。 這在 Windows Phone 上特別實用，通常能夠根據行動電話指標、Wi-Fi 指標及衛星來取得位置。 挑選較具體的準確度值，有助於系統在提供位置時使用電源成本最低的適當技術。

        例如：

        -   如果您的應用程式為了廣告調整、天氣、新聞等而取得位置，5000 公尺的準確度通常足以勝任。
        -   如果您的應用程式會顯示附近街區的鄰近，300 公尺的準確度很通常足以提供結果。
        -   如果使用者在尋找鄰近的推薦餐廳，我們希望能夠取得同個街區內的位置，因此 100 公尺的準確度即足夠。
        -   如果使用者嘗試分享其位置，應用程式應該要求大約 10 公尺的準確度。
    -   如果您的 app 有特定的準確度需求，請使用 [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526) 屬性。 例如，導航 app 應該使用 **Geocoordinate.accuracy** 屬性來判斷可用的位置資料是否符合 app 的需求。

-   請考慮啟動延遲。 應用程式第一次要求定位資料時，在定位提供者啟動時可能會有短暫延遲 (1 到 2 秒)。 在設計您應用程式的 UI 時，應該考量這一點。 例如，您可能想要避免封鎖讓 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 呼叫擱置無法完成的其他工作。

-   考量背景行為。 如果應用程式沒有焦點，則在背景暫停時就不會接收位置更新事件。 如果您的應用程式透過記錄位置更新的方式來進行追蹤，請注意這一點。 當應用程式重新取得焦點時，它僅會接收新的事件。 它不會取得在未使用時所發生的任何更新。

-   有效使用原始感應器與融合感應器。 感應器有兩種：[原始]** 與 [融合]**。

    -   原始感應器包括加速計、陀螺儀及磁力儀。
    -   融合感應器包括方向、傾角計及指南針。 融合感應器可從原始感應器組合取得資料。

    Windows 執行階段 API 可存取上述所有感應器 (除了磁力儀之外)。 融合感應器比原始感應器更精確且更穩定，但較為耗電。 您應該根據用途，使用適當的感應器。 如需詳細資訊，請參閱[感應器](https://msdn.microsoft.com/library/windows/apps/mt187358)。

**連線待命**
- 當電腦處於連線待命狀態時，一律可以具現化 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 物件。 不過，**Geolocator** 物件將不會找到任何感應器來彙總，因此針對 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 的呼叫會在 7 秒後會逾時、永遠不會呼叫 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件接聽器，而 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件接聽器將會搭配 **NoData** 被呼叫一次。

## <a name="additional-usage-guidance"></a>其他用法指導方針


### <a name="detecting-changes-in-location-settings"></a>偵測位置設定中的變更

使用者可以使用 **\[設定\]** 應用程式中的 **\[位置隱私權設定\]** 來關閉定位功能。

-   若要偵測使用者何時停用或重新啟用定位服務：
    -   處理 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件。 如果使用者關閉定位服務，**StatusChanged** 事件之引數的 [**Status**](https://msdn.microsoft.com/library/windows/apps/br225601) 屬性值就會是 **Disabled**。
    -   檢查 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 傳回的錯誤碼。 如果使用者已停用定位服務，針對 **GetGeopositionAsync** 的呼叫會失敗並出現 **ACCESS\_DENIED** 錯誤，且 [**LocationStatus**](https://msdn.microsoft.com/library/windows/apps/br225538) 屬性值會是 **Disabled**。
-   如果您有位置資料為必要資料的 app (例如地圖 app)，請確實執行下列動作：
    -   處理 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件以便在使用者位置變更時取得更新。
    -   如前述方式處理 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件，偵測位置設定中的變更。

請注意，定位服務會在資料變成可用時傳回該資料。 它可能會先以較大的錯誤半徑傳回一個位置，然後在較準確的資訊變成可用時，使用該資訊來更新位置。 顯示使用者位置的應用程式通常會希望在較準確的資訊變成可用時更新位置。

### <a name="graphical-representations-of-location"></a>位置圖形表示法

讓您的 app 使用 [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526)，來在地圖上清楚表示使用者目前的位置。 準確度有三個主要波段 - 約 10 公尺的錯誤半徑、約 100 公尺的錯誤半徑，以及大於 1 公里的錯誤半徑。 藉由使用準確度資訊，您可以確保 app 能根據可用的資料內容，準確地顯示位置。 如需有關使用地圖控制項的一般資訊，請參閱[顯示地圖的 2D、3D 和 Streetside 檢視](https://msdn.microsoft.com/library/windows/apps/mt219695)。

-   針對約等於 10 公尺 (GPS 解析) 的準確度，可以在地圖上以點或釘子來表示位置。 在這個準確度下，也可顯示經緯度座標與街道地址。

    ![以約 10 公尺的 GPS 準確度所顯示的地圖範例。](images/10metererrorradius.png)

-   針對介於 10 到 500 公尺 (約 100 公尺) 的準確度，通常是透過 Wi-Fi 解析來接收位置。 從行動電話取得的位置準確度大約為 300 公尺。 在這類狀況下，建議您的應用程式顯示錯誤半徑。 針對可顯示方向，並需要中心點的 app，可以在中心點周圍顯示錯誤半徑。

    ![以約 100 公尺的 Wi-Fi 準確度所顯示的地圖範例。](images/100metererrorradius.png)

-   如果傳回的準確度大於 1 公里，您可能是透過 IP 層級解析接收位置資訊。 這個準確度層級通常太低，無法在地圖上指出特定的地點。 您的 app 應將地圖放大至城市層級，或根據錯誤半徑 (例如地區層級) 來放大至適當區域。

    ![以約 1 公里的 Wi-Fi 準確度所顯示的地圖範例。](images/1000metererrorradius.png)

當定位準確度從某一級區的準確度切換到另一級區時，請在不同的圖形表示法之間提供自然流暢的轉換方式。 可以下列方式進行：

-   製作順暢的轉換動畫，並讓轉換過程快速且流暢。
-   讓使用者等候幾個連續的報告，以確認準確度的變更，這有助於防止使用者執行不想要且太頻繁的縮放。

### <a name="textual-representations-of-location"></a>位置文字表示法

某些類型的 app (例如，氣象 app 或當地資訊 app) 需要在不同準確度級區中使用文字來表示位置的方法。 請確定位置能清楚顯示，並且只顯示到資料中提供的準確度層級。

-   若準確度約等於 10 公尺 (GPS 解析)，接收到的位置資料是相當準確且可用來顯示達到街區名稱層級的資訊。 也可使用城市名稱、州或省名稱與國家/地區名稱。
-   若準確度約等於 100 公尺 (Wi-Fi 解析)，接收到的位置資料為適度準確的資料，因此我們建議您顯示達到城市名稱層級的資訊。 避免使用街區名稱。
-   若準確度大於 1 公里 (IP 解析)，僅顯示州或省，或國家/地區名稱。

### <a name="privacy-considerations"></a>隱私權考量

使用者的地理位置是個人識別資訊 (PII)。 下列網站提供保護使用者隱私權的指導。

-   [Microsoft 隱私權]( http://go.microsoft.com/fwlink/p/?LinkId=259692)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>相關主題

* [設定地理柵欄](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [取得目前的位置](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [顯示 2D、3D 和 Streetside 檢視的地圖](https://msdn.microsoft.com/library/windows/apps/mt219695)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [UWP 位置範例 (地理位置)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
