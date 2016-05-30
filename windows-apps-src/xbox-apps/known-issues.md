---
author: Mtoepke
title: Xbox One 開發人員預覽上的 UWP 已知問題
description: 
area: Xbox
---

# Xbox 開發人員預覽上的 UWP 已知問題

本主題說明 Xbox 開發人員預覽上的 UWP 已知問題。 
如需此開發人員預覽的詳細資訊，請參閱 [Xbox 上的 UWP](index.md)。 

\[如果您是透過 API 參考主題中的連結來到這裡，並在尋找通用裝置系列 API 的資訊，請參閱[尚未在 Xbox 上支援的 UWP 功能 (英文)](http://go.microsoft.com/fwlink/?LinkID=760755)。\]

Xbox 開發人員預覽系統更新包括實驗性和早期發行前版本軟體。 
這代表某些熱門的遊戲和應用程式將不會如預期般運作，而您可能偶爾會遇到當機和資料遺失。 
如果您離開開發人員預覽，您的主機將會恢復出廠預設值，您必須重新安裝所有遊戲、App 及內容。

對開發人員來說，這表示不是所有開發人員工具和 API 都能如預期運作。 
最終版本的所有功能並非都會包括在內，或都處於發行品質。 
**特別是此預覽中的系統效能並不會反映最終版本的系統效能。**

下列清單醒目提示這個版本中可能遇到的一些已知問題，但這並不是完整的清單。 

**我們想要您的意見反應**，因此請在[開發通用 Windows App](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) 論壇上報告您發現的任何問題。 

如果您遇到困難，請閱讀本主題中的資訊、參閱[常見問題集](frequently-asked-questions.md)，並使用論壇以尋求協助。


## 開發遊戲

### x86 與 x64

在今年我們稍後發行之前，對 x86 和 x64 有不錯的支援，而且在此預覽中確實支援 x86。 
不過，x64 到目前為止已經進行過很多測試 (Xbox 殼層以及現在主機上執行的所有應用程式都是 x64)，因此建議針對您的專案使用 x64。 
這特別適用於遊戲。

如果您決定使用 x86，請在論壇上報告您看到的任何問題。

另請參閱本頁面後面的[切換組建類別可能會導致部署失敗](known-issues.md#switching-build-flavors-can-cause-deployment-failures)。

### 遊戲引擎

我們只測試過一些熱門的遊戲引擎，而非所有遊戲引擎，因此我們對此預覽的測試涵蓋範圍並不完整。 
您的情況可能會不同。 

下列遊戲引擎已確認可以運作：
* [Construct 2](https://www.scirra.com/)

也很有可能有其他可以運作的引擎。 我們很希望您針對自己的發現提供意見反應。 
請使用論壇來報告您看到的任何問題。

### DirectX 12 支援

Xbox One 上的 UWP 支援 DirectX 11 功能層級 10。 
此時不支援 DirectX 12。 
Xbox One (與所有傳統遊戲主機類似) 是一個特殊的硬體，需要有特定 SDK 才能充分發揮其潛力。 
如果您正在處理需要存取 Xbox One 硬體之最大潛力的遊戲，請向 [ID@XBOX](http://www.xbox.com/en-us/Developers/id) 計畫註冊來存取該 SDK (內含 DirectX 12 支援)。

### Xbox One 開發人員預覽會停用對 Windows 10 的遊戲串流功能。

在您的主機上啟用 Xbox One 開發人員預覽會阻止您將遊戲從 Xbox One 串流到 Windows 10 上的 Xbox App，就算您的主機已經設定為零售模式也一樣。 若要還原遊戲串流功能，您必須退出開發人員預覽。

### 電視安全區域的已知問題

根據預設，Xbox 上 UWP App 的顯示區域應該要針對電視安全區域進行內凹處理。 不過，Xbox One 開發人員預覽包含的一個已知錯誤，會使電視安全區域從 [0, 0] 開始，而非從 [_offset_, _offset_] 開始。

如需電視安全區域的詳細資訊，請參閱 [https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv)。 

解決這個問題最簡單的方式便是停用電視安全區域，如下列 JavaScript 範例所示。

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

### 尚未支援滑鼠模式

於 [https://msdn.microsoft.com/zh-tw/windows/uwp/input-and-devices/designing-for-tv] (https://msdn.microsoft.com/zh-tw/windows/uwp/input-and-devices/designing-for-tv?f=255&amp;MSPPError=-2147217396#mouse-mode) 主題中所述的「滑鼠模式」__ 功能尚未在 Xbox One 開發人員預覽中支援。

## 適用於 Xbox One 上 UWP App 和遊戲的系統資源

在 Xbox One 上執行的 UWP 應用程式和遊戲會與系統和其他應用程式共用資源，因此系統會控管任何一個遊戲或應用程式可以使用的資源。 
如果您是發生記憶體或效能問題，則這可能是原因。 
如需詳細資料，請參閱[適用於 Xbox One 上 UWP App 和遊戲的系統資源](system-resource-allocation.md)。


## 使用傳統通訊端的網路功能

在這個開發人員預覽中，無法使用來自使用傳統 TCP/UDP 通訊端 (WinSock、Windows.Networking.Sockets) 的主機的輸入和輸出網路存取。 
開發人員仍然可以使用 HTTP 和 WebSocket。 


## UWP API 涵蓋範圍

在此預覽中，並非所有 UWP API 在 Xbox 上都會如預期運作。 
請參閱[尚未在 Xbox 上支援的 UWP 功能 (英文)](http://go.microsoft.com/fwlink/p/?LinkId=760755) 以取得已知無法運作的 API 清單。 
如果您發現其他 API 問題，請在論壇上報告它們。 

## XAML 控制項看起來不像或行為不像 Xbox One 殼層中的控制項

在這個開發人員預覽中，XAML 控制項不是其最終形式。 特別的是：
* 所有控制項的遊戲台 X-Y 瀏覽都無法可靠地運作。
* 控制項看起來不像 Xbox 殼層中的控制項。 這包括控制項焦點矩形。
* 在控制項之間瀏覽並不會自動設定「瀏覽音效」

在未來的開發人員預覽中，將會解決這些問題。

## Visual Studio 和部署問題

### 切換組建類別可能會導致部署失敗

在偵錯與發行組建、x86 與 x64 或是受管理與 .Net 原生組建之間切換，可能會導致部署失敗。 

避免這個預覽發生這些問題的最簡單方式是僅使用偵錯組建和單一架構。 

如果您遇到此問題，在 Xbox One 的收藏應用程式中解除安裝您的 App 通常就可以解決問題。

> **注意** &nbsp;&nbsp;從 Windows Device Portal (WDP) 解除安裝您的 App 將不會解決問題。

如果您的問題持續發生，請在收藏應用程式中解除安裝您的 App 或遊戲，離開開發人員模式，重新啟動到零售模式，然後切換回開發人員模式。

如需詳細資訊，請參閱[常見問題集](frequently-asked-questions.md)中的＜修正部署失敗＞小節。

### 在 Visual Studio 中對應用程式進行偵錯的同時解除安裝應用程式，會讓它以無訊息方式失敗

嘗試透過 WDP「已安裝的應用程式」工具解除安裝在偵錯工具下執行的應用程式，會讓它以無訊息方式失敗。 
因應措施是在嘗試透過 WDP 移除應用程式之前，於 Visual Studio 中停止偵錯應用程式。

### Visual Studio/Xbox PIN 配對失敗

可能會進入下列狀態：Visual Studio 與 Xbox One 之間的 PIN 配對不同步。 
如果 PIN 配對失敗，請使用開發人員首頁中的 [移除所有配對] 按鈕、重新啟動 Xbox One、重新啟動您的開發電腦，然後再試一次。 


## Windows Device Portal (WDP) 預覽

### 從開發人員首頁啟動 WDP 會讓開發人員首頁當機

當您在開發人員首頁中啟動 WDP 時，會在輸入使用者名稱和密碼並選取 [儲存]**** 之後，導致開發人員首頁當機。 
系統可儲存認證，但 WDP 不會啟動。 
重新啟動 Xbox One，即可啟動 WDP。 

### 在開發人員首頁中停用 WDP 無法運作

如果您在開發人員首頁中停用 WDP，則會關閉 WDP。 
不過，當您重新啟動您的 Xbox One 時，將再次啟動 WDP。 
使用 [重設並保留我的遊戲和應用程式]**** 刪除 Xbox One 上任何儲存的狀態，即可解決這個問題。 
移至 [設定] &gt; [系統] &gt; [主機資訊與更新] &gt; [重設主機]，然後選取 [重設並保留我的遊戲和應用程式]**** 按鈕。

> **注意** &nbsp;&nbsp;這樣做會刪除 Xbox One 上所有儲存的設定，包括無線設定、使用者帳戶以及任何未儲存到雲端儲存體的遊戲進度。

> **注意** &nbsp;&nbsp;請不要選取 [重設並移除所有項目]**** 按鈕。
這樣會刪除所有遊戲、App、設定和內容、停用開發人員模式，以及從開發人員預覽群組中移除您的主機。

### 未以可預測的方式更新 [執行應用程式] 資料表中的資料欄。 

有時解決此問題的方式就是排序資料表上的資料欄。

### WDP UI 未正確地顯示在 Internet Explorer 11 中 

根據預設，使用 Internet Explorer 11 時，WDP UI 未正確地顯示在瀏覽器中。 
修正這個問題的方式是關閉 Internet Explorer 11 的 WDP 相容性檢視。

### 瀏覽到 WDP 會導致憑證警告

您將收到有關所提供憑證的警告 (與下列螢幕擷取畫面類似)，因為 Xbox One 主機所簽署的安全性憑證並不被視為已知的受信任發行者。 
按一下 [繼續瀏覽此網站] 來存取 Windows Device Portal。

![網站安全性憑證警告](images/security_cert_warning.jpg)

## 開發人員首頁

偶而，在開發人員首頁中選取 [管理 Windows Device Portal] 選項，會導致開發人員首頁以無訊息方式退出到 [首頁] 畫面。 
這是主機的 WDP 基礎結構中的失敗所造成，而且重新啟動主機即可解決。

## 另請參閱
- [常見問題集](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)


<!--HONumber=May16_HO2-->


