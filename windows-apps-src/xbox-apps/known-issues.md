---
author: Mtoepke
title: "Xbox One 開發人員預覽上的 UWP 已知問題"
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: 9a9180f8d6fcd51808310a7f8fbac986ca9c3817

---

# Xbox 開發人員預覽上的 UWP 已知問題

本主題說明 Xbox 開發人員預覽上的 UWP 已知問題。 如需此開發人員預覽的詳細資訊，請參閱 [Xbox 上的 UWP](index.md)。 

\[如果您是透過 API 參考主題中的連結來到這裡，並在尋找通用裝置系列 API 的資訊，請參閱[尚未在 Xbox 上支援的 UWP 功能 (英文)](http://go.microsoft.com/fwlink/?LinkID=760755)。\]

Xbox 開發人員預覽系統更新包括實驗性和早期發行前版本軟體。 這代表某些熱門的遊戲和應用程式將不會如預期般運作，而您可能偶爾會遇到當機和資料遺失。 如果您離開開發人員預覽，您的主機將會恢復出廠預設值，您必須重新安裝所有遊戲、App 及內容。

對開發人員來說，這表示不是所有開發人員工具和 API 都能如預期運作。 最終版本的所有功能並非都會包括在內，或都處於發行品質。 
**特別是此預覽中的系統效能並不會反映最終版本的系統效能。**

下列清單醒目提示這個版本中可能遇到的一些已知問題，但這並不是完整的清單。 


            **我們想要您的意見反應**，因此請在[開發通用 Windows App](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) 論壇上報告您發現的任何問題。 

如果您遇到困難，請閱讀本主題中的資訊、參閱[常見問題集](frequently-asked-questions.md)，並使用論壇以尋求協助。


<!--## Developing games-->

## 滑鼠模式支援

從此預覽版開始，「滑鼠模式」__針對XAML 和託管的 Web 應用程式預設為啟用。 所有未選擇不使用的應用程式將會看到滑鼠指標，類似於 Xbox Edge 瀏覽器中的指標。

**強烈建議開發人員應關閉滑鼠模式，並針對控制器 (X-Y) 瀏覽最佳化。**

若要關閉 XAML 中的滑鼠模式，請依照此範例：

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

若要關閉 HTML/Javascript app 中的滑鼠模式，請依照此範例：

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

> 
            **注意**
            &nbsp;&nbsp;在此開發人員預覽版中，當滑鼠模式開啟時，使用控制器上的右搖桿移動瀏覽可能造成主機當機。 如果您發生此問題，必須重新啟動主機。

如需滑鼠模式支援的資訊，請參閱[針對 Xbox 與電視設計](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode)主題。 本主題包含如何啟用與停用滑鼠模式的資訊，可讓您選擇適用於 app 的正確行為。

## 您必須已有使用者登入才能部署 app (錯誤 0x87e10008)

App 現在需要使用者登入才能啟動 (您必須已有使用者登入才能在 VS 2015「開始偵錯 (F5)」)。目前從 Visual Studio 收到的錯誤訊息並不直覺：
 
![無法啟用 Windows 市集應用程式](images/windows-store-app-activation-error.jpg)
 
若要解決此問題，請在部署 app 之前，先從 Xbox 殼層或開發人員首頁登入使用者。
 
## 尚未強制執行背景應用程式的記憶體限制
 
在背景執行之應用程式的 128 MB 限制，未在此預覽版中強制執行。 這表示若您的 app 在背景執行時超過 128 MB，它仍可配置記憶體。
 
目前此問題還沒有解決方法。 您應該據此來管理記憶體使用量；在未來的預覽版中，若您的 app 超過 128 MB 限制，將會收到記憶體配置失敗訊息。
 
## 家長監護開啟時，從 VS 部署失敗

若主機開啟 \[設定\] 中的 \[家長監護\] 功能，從 VS 啟用 app 將會失敗。

若要解決此問題，可以先暫時停用家長監護，或：
1. 將 app 部署到關閉家長監護的主機
2. 開啟家長監護
3. 從主機啟動 app
4. 輸入 PIN 或密碼來允許啟動 app
5. App 將會啟動
6. 關閉 app
7. 使用 F5 從 VS 啟動，app 將會以無提示方式啟動

此時，即使解除安裝並重新安裝 app，權限仍會「相黏」__直到您將使用者登出為止。
 
有另外一種豁免類型，僅適用於子女帳戶。 子女帳戶需要家長登入以授與權限，但當他們這樣做時，家者可以選擇 \[一律\] 選項，來允許子女啟動 app。 該豁免會儲存於雲端，且即使在子女登出再重新登入之後也會保留。   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## DirectX 12 支援

Xbox One 上的 UWP 支援 DirectX 11 功能層級 10。 此時不支援 DirectX 12。 Xbox One (與所有傳統遊戲主機類似) 是一個特殊的硬體，需要有特定 SDK 才能充分發揮其潛力。 如果您正在處理需要存取 Xbox One 硬體之最大潛力的遊戲，請向 [ID@XBOX](http://www.xbox.com/en-us/Developers/id) 計畫註冊來存取該 SDK (內含 DirectX 12 支援)。

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## 電視安全區域的已知問題

根據預設，Xbox 上 UWP App 的顯示區域應該要針對電視安全區域進行內凹處理。 不過，Xbox One 開發人員預覽包含的一個已知錯誤，會使電視安全區域從 [0, 0] 開始，而非從 [_offset_, _offset_] 開始。

> 
            **注意**
            &nbsp;&nbsp;這僅適用於使用 Javascript 的 UWP app。

解決這個問題最簡單的方式便是停用電視安全區域，如下列 JavaScript 範例所示。

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

如需有關電視安全區域的詳細資訊，請參閱[針對 Xbox 與電視設計](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv)。

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->


## 使用傳統通訊端的網路功能

在這個開發人員預覽中，無法使用來自使用傳統 TCP/UDP 通訊端 (WinSock、Windows.Networking.Sockets) 的主機的輸入和輸出網路存取。 開發人員仍然可以使用 HTTP 和 WebSocket。 


## UWP API 涵蓋範圍

並非所有的 UWP API 都在 Xbox 上受到支援。 請參閱[尚未在 Xbox 上支援的 UWP 功能 (英文)](http://go.microsoft.com/fwlink/p/?LinkId=760755) 以取得已知無法運作的 API 清單。 如果您發現其他 API 問題，請在論壇上報告它們。 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


## Windows Device Portal (WDP) 預覽

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

### WDP UI 未正確地顯示在 Internet Explorer 7 中 

根據預設，使用 Internet Explorer 7 時，WDP UI 未正確地顯示在瀏覽器中。 修正這個問題的方式是關閉 Internet Explorer 7 的 WDP 相容性檢視。

### 瀏覽到 WDP 會導致憑證警告

您將收到有關所提供憑證的警告 (與下列螢幕擷取畫面類似)，因為 Xbox One 主機所簽署的安全性憑證並不被視為已知的受信任發行者。 按一下 \[繼續瀏覽此網站\] 來存取 Windows Device Portal。

![網站安全性憑證警告](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## 另請參閱
- [常見問題集](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Jun16_HO4-->


