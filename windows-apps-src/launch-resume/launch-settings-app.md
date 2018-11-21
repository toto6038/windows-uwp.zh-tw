---
author: TylerMSFT
title: 啟動 Windows 設定 app
description: 了解如何從您的應用程式啟動 Windows 設定應用程式。 本主題描述 ms-settings URI 配置。 使用此 URI 配置，可將 Windows 設定應用程式啟動到特定的設定頁面。
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 03/20/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b86298ca671282dea201e3088bc60845231fe007
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7442790"
---
# <a name="launch-the-windows-settings-app"></a>啟動 Windows 設定應用程式


**重要 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何啟動 Windows 設定應用程式。 本主題描述 **ms-settings:** URI 配置。 使用此 URI 配置可將 Windows 設定 app 啟動到特定的設定頁面。

啟動設定 app 是撰寫隱私權感知 app 的重要部分。 如果您的 app 無法存取敏感資源，建議讓使用者能夠方便地連結到該資源的隱私權設定。 如需詳細資訊，請參閱[隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh768223)。

## <a name="how-to-launch-the-settings-app"></a>如何啟動設定 App

若要啟動 **\[設定\]** App，請使用 `ms-settings:` URI 配置，如下列範例所示。

在這個範例中，會使用「超連結 XAML」控制項與 `ms-settings:privacy-microphone` URI 來啟動麥克風的隱私權設定頁面。

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

或者，您的應用程式可以呼叫 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以啟動**設定**應用程式。 這個範例示範如何使用 `ms-settings:privacy-webcam` URI 來啟動進入相機的隱私權設定頁面。

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

上述程式碼會啟動相機的隱私權設定頁面：

![相機隱私權設定。](images/privacyawarenesssettingsapp.png)

如需啟動 URI 的詳細資訊，請參閱[啟動 URI 的預設 app](launch-default-app.md)。

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI 配置參考

使用下列 URI 可開啟 [設定] app 的各個頁面。

> 請注意，設定頁面是否可用因 Windows SKU 而異。 Windows 10 桌面版上所有的設定頁面不一定皆在 Windows 10 行動裝置版上，反之亦然。 附註欄也會擷取必須符合頁面才可供使用的其他需求。

## <a name="accounts"></a>帳戶

|設定頁面| URI |
|-------------|-----|
| 存取工作或學校帳戶 | ms-settings:workplace |
| 電子郵件與應用程式帳戶  | ms-settings:emailandaccounts |
| 家人與其他使用者 | ms-settings:otherusers |
| 登入選項 | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| 同步您的設定 | ms-settings:sync |
| Windows Hello 設定 | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| 您的資訊 | ms-settings:yourinfo |

## <a name="apps"></a>應用程式

|設定頁面| URI |
|-------------|-----|
| 應用程式與功能 | ms-settings:appsfeatures |
| 應用程式功能 | ms-settings:appsfeatures-app (重設、管理應用程式的附加元件與可下載內容等)|
| 網站的應用程式 | ms-settings:appsforwebsites |
| 預設應用程式 | ms-settings:defaultapps |
| 管理選用功能 | ms-settings:optionalfeatures |
| 離線地圖 | ms-settings:maps |
| 啟動應用程式 | ms-settings:startupapps |
| 視訊播放 | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|設定頁面| URI |
|-------------|-----|
| 權限與歷程記錄 | ms-settings:cortana-permissions |
| 更多詳細資料 | ms-settings:cortana-moredetails |
| 我的裝置上的 Cortana | ms-settings:cortana-notifications |
| 向 Cortana 講話 | ms-settings:cortana-language |

> [!NOTE] 
> 在桌面上此設定 \] 區段時將會呼叫搜尋電腦設定為其中沒有目前可用的 Cortana，或已停用 Cortana 的區域。 在此情況下不會列 (跨我的裝置，Cortana) 和 Cortana 與交談，Cortana 特定頁面。 

## <a name="devices"></a>裝置

|設定頁面| URI |
|-------------|-----|
| 音訊與語音 | ms-settings:holographic-audio (僅適用於混合實境入口網站應用程式已安裝時--可在 Microsoft Store 中取得) |
| 自動播放 | ms-settings:autoplay |
| 藍牙 | ms-settings:bluetooth |
| 連線的裝置 | ms-settings:connecteddevices |
| 預設相機 | ms-settings:camera |
| 滑鼠與觸控板 | ms-settings:mousetouchpad (觸控板設定僅適用於具有觸控板的裝置上) |
| 手寫筆與 Windows Ink | ms-settings:pen |
| 印表機與掃描器 | ms-settings:printers |
| 觸控板 | ms-settings:devices-touchpad (僅適用於有觸控板硬體存在時) |
| 文字輸入 | ms-settings:typing |
| USB | ms-settings:usb |
| 轉盤 | ms-settings:wheel (僅適用於撥號已配對時) |
| 您的手機 | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>輕鬆存取

|設定頁面| URI |
|-------------|-----|
| 音效 | ms-settings:easeofaccess-audio |
| 隱藏式輔助字幕 | ms-settings:easeofaccess-closedcaptioning |
| 色彩篩選器 | ms-設定： easeofaccess-colorfilter |
| 顯示器 | ms-settings:easeofaccess-display |
| 眼球控制 | ms-settings:easeofaccess-eyecontrol |
| 字型 | ms-settings:fonts |
| 高對比 | ms-settings:easeofaccess-highcontrast |
| 全像攝影頭戴式裝置 | ms-settings:holographic-headset (需要全像攝影硬體) |
| 鍵盤 | ms-settings:easeofaccess-keyboard |
| 放大鏡 | ms-settings:easeofaccess-magnifier |
| 滑鼠 | ms-settings:easeofaccess-mouse |
| 朗讀程式 | ms-settings:easeofaccess-narrator |
| 其他選項 | ms-settings:easeofaccess-otheroptions |
| 語音 | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>其他

|設定頁面| URI |
|-------------|-----|
| 其他 | ms-settings:extras (僅適用於「設定應用程式」已安裝時，例如透過第三方) |

## <a name="gaming"></a>遊戲

|設定頁面| URI |
|-------------|-----|
| 廣播 | ms-settings:gaming-broadcasting |
| 遊戲列 | ms-settings:gaming-gamebar |
| 遊戲 DVR | ms-settings:gaming-gamedvr |
| 遊戲模式 | ms-settings:gaming-gamemode |
| 玩遊戲全螢幕 | ms-settings:quietmomentsgame |
| TruePlay | ms-settings:gaming-trueplay |
| Xbox 網路 | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>首頁

|設定頁面| URI |
|-------------|-----|
| 設定首頁 | ms-settings: |


## <a name="network--internet"></a>網路和網際網路

|設定頁面| URI |
|-------------|-----|
| 飛航模式 | ms-settings:network-airplanemode (在 Windows 8.x 上使用 ms-settings:proximity) |
| 行動數據與 SIM 卡 | ms-settings:network-cellular |
| 數據使用量 | ms-settings:datausage |
| 撥號 | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess (僅適用於 DirectAccess 已啟用時) |
| 乙太網路 | ms-settings:network-ethernet |
| 管理已知的網路 | ms-settings:network-wifisettings |
| 行動熱點 | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| 狀態 | ms-settings:network-status |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (僅適用於裝置有 Wi-Fi 介面卡時) |
| Wi-Fi 通話 | ms-settings:network-wificalling (僅適用於 Wi-Fi 通話已啟用時) |

## <a name="personalization"></a>個人化

|設定頁面| URI |
|-------------|-----|
| 背景 | ms-settings:personalization-background |
| 選擇要顯示在 \[開始\] 上的資料夾 | ms-settings:personalization-start-places |
| 色彩 | ms-settings:personalization-colors |
| 瀏覽 | ms-settings:personalization-glance |
| 鎖定畫面 | ms-settings:lockscreen |
| 瀏覽列 | ms-settings:personalization-navbar |
| 個人化 (類別) | ms-settings:personalization |
| 開始 | ms-settings:personalization-start |
| 工作列 | ms-settings:taskbar |
| 佈景主題 | ms-settings:themes |

## <a name="phone"></a>Phone

|設定頁面| URI |
|-------------|-----|
| 您的手機 | ms-settings:mobile-devices  |

## <a name="privacy"></a>隱私權

|設定頁面| URI |
|-------------|-----|
| 配件專屬應用程式 | ms-settings:privacy-accessoryapps |
| 帳戶資訊 | ms-settings:privacy-accountinfo |
| 活動歷程記錄 | ms-settings:privacy-activityhistory |
| 廣告識別碼 | ms-settings:privacy-advertisingid |
| 應用程式診斷 | ms-settings:privacy-appdiagnostics |
| 自動檔案下載 | ms-settings:privacy-automaticfiledownloads |
| 背景應用程式 | ms-settings:privacy-backgroundapps |
| 行事曆 | ms-settings:privacy-calendar |
| 通訊記錄 | ms-settings:privacy-callhistory |
| 相機 | ms-settings:privacy-webcam |
| 連絡人 | ms-settings:privacy-contacts |
| 文件 | ms-settings:privacy-documents |
| 電子郵件 | ms-settings:privacy-email |
| 眼球追蹤器 | ms-settings:privacy-eyetracker (需要眼球追蹤器硬體) |
| 意見反應與診斷 | ms-settings:privacy-feedback |
| 檔案系統 | ms-settings:privacy-broadfilesystemaccess |
| 一般 | ms-settings:privacy-general |
| 位置 | ms-settings:privacy-location |
| 訊息中心 | ms-settings:privacy-messaging |
| 麥克風 | ms-settings:privacy-microphone |
| 動作 | ms-settings:privacy-motion |
| 通知 | ms-settings:privacy-notifications |
| 其他裝置 | ms-settings:privacy-customdevices |
| 圖片 | ms-settings:privacy-pictures |
| 通話 | ms-settings:privacy-phonecall |
| 無線通訊 | ms-settings:privacy-radios |
| 語音、筆跡與輸入 |ms-settings:privacy-speechtyping |
| 工作 | ms-settings:privacy-tasks |
| 影片 | ms-settings:privacy-videos |

## <a name="surface-hub"></a>Surface Hub

|設定頁面| URI |
|-------------|-----|
| 帳戶 | ms-settings:surfacehub-accounts |
| 工作階段清理 | ms-settings:surfacehub-sessioncleanup |
| 小組會議 | ms-settings:surfacehub-calling |
| 小組裝置管理 | ms-settings:surfacehub-devicemanagenent |
| 歡迎畫面 | ms-settings:surfacehub-welcome |

## <a name="system"></a>系統

|設定頁面| URI |
|-------------|-----|
| 關於 | ms-settings:about |
| 進階顯示設定 | ms-settings:display-advanced (僅適用於支援進階顯示選項的裝置) |
| 省電模式 | ms-settings:batterysaver (僅適用於具有電池的裝置上，例如平板電腦) |
| 省電模式設定 | ms-settings:batterysaver-settings (僅適用於具有電池的裝置上，例如平板電腦) |
| 電池使用情況 | ms-settings:batterysaver-usagedetails (僅適用於具有電池的裝置上，例如平板電腦) |
| 顯示器 | ms-settings:display |
| 預設儲存位置 | ms-settings:savelocations |
| 顯示器 | ms-settings:screenrotation |
| 複製我的顯示器 | ms-settings:quietmomentspresentation |
| 在下列時間 | ms-settings:quietmomentsscheduled |
| 加密 | ms-settings:deviceencryption |
| 專注輔助 | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| 圖形設定 | ms-settings:display-advancedgraphics (僅適用於支援進階圖形選項的裝置) |
| 訊息中心 | ms-settings:messaging |
| 多工 | ms-settings:multitasking |
| 夜間光線設定 | ms-settings:nightlight |
| 電話 | ms-settings:phone-defaultapps |
| 投影到此電腦 | ms-settings:project |
| 共用體驗 | ms-settings:crossdevice |
| 平板電腦模式 | ms-settings:tabletmode |
| 工作列 | ms-settings:taskbar |
| 通知與動作 | ms-settings:notifications |
| 遠端桌面 | ms-settings:remotedesktop |
| 電話 | ms-settings:phone |
| 電源與睡眠 | ms-settings:powersleep |
| 音效 | ms-settings:sounds |
| 儲存空間 | ms-settings:storagesense |
| 儲存空間感知器 | ms-settings:storagepolicies |

## <a name="time-and-language"></a>時間與語言

|設定頁面| URI |
|-------------|-----|
| 日期和時間 | ms-settings:dateandtime |
| 日文 IME 設定 | ms-settings:regionlanguage-jpnime (適用於 Microsoft 日文輸入法編輯器有安裝時) |
| 拼音輸入法設定 | ms-settings:regionlanguage-chsime-pinyin (適用於 Microsoft 拼音輸入法編輯器有安裝時) |
| 地區及語言 | ms-settings:regionlanguage |
| 語音功能的語言 | ms-settings:speech |
| 五筆輸入法設定  | ms-settings:regionlanguage-chsime-wubi (適用於 Microsoft 五筆輸入法編輯器有安裝時) |

## <a name="update--security"></a>更新與安全性

|設定頁面| URI |
|-------------|-----|
| 啟用 | ms-settings:activation |
| 備份 | ms-settings:backup |
| 傳遞最佳化 | ms-settings:delivery-optimization |
| 尋找我的裝置 | ms-settings:findmydevice |
| 適用於開發人員 | ms-settings:developers |
| 修復 | ms-settings:recovery |
| 疑難排解 | ms-settings:troubleshoot |
| Windows 資訊安全中心 | ms-settings:windowsdefender |
| Windows 測試人員計畫 | ms-settings:windowsinsider (僅適用於使用者已在 WIP 中註冊時) |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows Update-進階選項 | ms-settings:windowsupdate-options |
| Windows Update-重新啟動選項 | ms-settings:windowsupdate-restartoptions |
| Windows Update-檢視更新記錄 | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>使用者帳戶

|設定頁面| URI |
|-------------|-----|
| 正在佈建 | ms-settings:workplace-provisioning (僅適用於企業已部署佈建套件時) |
| 正在佈建 | ms-settings:provisioning (僅適用於行動裝置上及企業已部署佈建套件時) |
| Windows Anywhere | ms-settings:windowsanywhere (裝置必須具有 Windows Anywhere 功能) |
