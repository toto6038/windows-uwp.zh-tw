---
author: Mtoepke
title: "Xbox 開發人員計畫上的 UWP 已知問題"
description: "列出 UWP 在 Xbox 開發人員計畫上的已知問題。"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.openlocfilehash: 203d1abede2607617e0175103f54bf3068d53ff4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 開發人員計畫上的 UWP 已知問題

本主題說明 Xbox One 開發人員計畫上的 UWP 已知問題。 如需此計畫的詳細資訊，請參閱 [Xbox 上的 UWP](index.md)。 

\[如果您是透過 API 參考主題中的連結來到這裡，並在尋找通用裝置系列 API 的資訊，請參閱[尚未在 Xbox 上支援的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755) (英文)。\]

下列清單將針對您可能遇到的一些已知問題進行重點提示，但這並不是完整的清單。 

**我們想要您的意見反應**，因此請在[開發通用 Windows 平台應用程式](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop) 論壇上報告您發現的任何問題。 

如果您遇到困難，請閱讀本主題中的資訊、參閱[常見問題集](frequently-asked-questions.md)，並使用論壇以尋求協助。


<!--## Developing games-->

## <a name="issue-when-leaving-dev-mode"></a>離開開發人員模式時的問題
此時，可能發生您無法使用 DevHome 或從 [開發人員設定] 離開開發人員模式的狀況。
對此狀況有兩種可能的解決方式： 
1. 離開開發人員模式時取消核取 **\[刪除側載遊戲和應用程式\]**
2. 移至 [我的遊戲和應用程式]，解除安裝您安裝在主控台的開發人員應用程式
 
<!--## Memory limits for background apps are partially enforced
 
The maximum memory footprint for apps running in the background is 128 megabytes. In the current version of UWP on Xbox One, your app will be suspended if it is above this limit when it is moved to the background. This limit is not currently enforced if your app exceeds the limit while it is already running in the background—this means that if your app exceeds 128 MB while running in the background, it will still be able to allocate memory.
 
There is currently no workaround for this issue. Apps should govern their memory usage accordingly and continue to stay under the 128 MB limit while running in the background.-->
 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>家長監護開啟時，從 VS 進行部署將會失敗

若主機開啟 [設定] 中的 [家長監護] 功能，從 VS 啟用應用程式將會失敗。

若要解決此問題，可以先暫時停用家長監護，或：
1. 將應用程式部署到已關閉家長監護的主機。
2. 開啟家長監護。
3. 從主機啟動應用程式。
4. 輸入 PIN 或密碼來允許啟動應用程式。
5. 應用程式將會啟動。
6. 關閉應用程式。
7. 使用 F5 從 VS 啟動，應用程式將會以無提示方式啟動。

此時，直到您將使用者登出為止，該權限將會「持續生效」__，即使您將應用程式解除安裝並重新安裝也一樣。
 
有另外一種免除類型，僅適用於子女帳戶。 子女帳戶需要家長登入以授與權限，但當他們這樣做時，家者可以選擇 **\[一律\]** 允許子女啟動該應用程式。 該免除項目儲存於雲端，即使子女登出後又重新登入，也會保留。

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>StorageFile.CopyAsync 無法將加密的檔案複製到未加密的目的地 

使用 StorageFile.CopyAsync 將已加密檔案複製到沒有加密的目的地時，這個呼叫會失敗並發生下列例外狀況：

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

這可能會影響想要將部署為應用程式套件一部分之檔案複製到其他位置的 Xbox 開發人員。 之所以如此的原因是，Xbox 加密套件內容時所用的是零售模式，而不是開發人員模式。 因此，App 或許在開發和測試期間看起來會依預期正常運作，但是發佈並安裝至零售 Xbox 後，就會失敗。

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

## <a name="directx-12-support"></a>DirectX 12 支援

Xbox One 上的 UWP 支援 DirectX 11 功能層級 10。 此時不支援 DirectX 12。 

Xbox One (與所有傳統遊戲主機類似) 是一個特殊的硬體，需要有特定 SDK 才能充分發揮其潛力。 如果您正在處理需要充分存取 Xbox One 硬體資源的遊戲，請向 [ID@XBOX](http://www.xbox.com/Developers/id) 計畫註冊，即可存取這個內含 DirectX 12 支援的 SDK。

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 

## <a name="blocked-networking-ports-on-xbox-one"></a>Xbox One 上的已封鎖網路連接埠

Xbox One 裝置上的通用 Windows 平台 (UWP) 應用程式具有無法繫結至 [57344, 65535] (包含此項) 範圍中之連接埠的限制。 雖然於執行階段期間繫結至這些連接埠時，看起來可能已成功，不過網路流量在抵達應用程式之前可能便已無訊息地被捨棄。 您的應用程式應該在可行的情況下繫結至連接埠 0，這將能允許系統選取本機連接埠。 如果您需要使用特定的連接埠，連接埠號碼必須位於 [1025, 49151] 的範圍內，而您也應該檢查並避免與 IANA 登錄發生衝突。 如需詳細資訊，請參閱[服務名稱與傳輸通訊協定連接埠號碼登錄 (英文)](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)。

## <a name="uwp-api-coverage"></a>UWP API 涵蓋範圍

並非所有的 UWP API 都在 Xbox 上受到支援。 如需取得已知無法運作的 API 清單，請參閱[尚未在 Xbox 上支援的 UWP 功能 (英文)](http://go.microsoft.com/fwlink/p/?LinkId=760755)。 如果您發現其他 API 問題，請在論壇上報告它們。 

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


<!--## Windows Device Portal (WDP) preview-->

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

## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>瀏覽到 WDP 會導致憑證警告

您將收到有關所提供憑證的警告 (與下列螢幕擷取畫面類似)，因為 Xbox One 主機所簽署的安全性憑證並不被視為已知的受信任發行者。 若要存取 Windows Device Portal，請按一下 **\[繼續瀏覽此網站\]**。

![網站安全性憑證警告](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>在 Xbox 上 KnownFolders.MediaServerDevices 附加說明

在桌面上，媒體伺服器與電腦「搭配」，而裝置關聯服務會不斷地追蹤哪些伺服器目前在線上，如此，初始檔案系統查詢便可立即傳回目前在線上的已配對伺服器清單。

在 Xbox 上，沒有新增或移除伺服器的 UI，因此初始檔案系統查詢將永遠傳回空白。 您必須建立查詢，並訂閱至 ContentsChanged 事件，每當收到通知便重新整理查詢。 伺服器會魚貫而至，大部分在 3 秒鐘內即會被發現。

簡單的範例程式碼︰

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>請參閱
- [常見問題集](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)
