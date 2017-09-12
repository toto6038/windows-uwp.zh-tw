---
author: TylerMSFT
title: "啟動 Windows 設定 app"
description: "了解如何從您的應用程式啟動 Windows 設定應用程式。 本主題描述 ms-settings URI 配置。 使用此 URI 配置，可將 Windows 設定應用程式啟動到特定的設定頁面。"
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 06/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 2a30e14f7c275c48f52253157fc9c67bf05d259e
ms.sourcegitcommit: 00c3f5a1208bd0125f5b275f972cf2a82d8eb9b6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2017
---
# <a name="launch-the-windows-settings-app"></a>啟動 Windows 設定應用程式

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何從您的 app 啟動 Windows 設定 app。 本主題描述 **ms-settings:** URI 配置。 使用此 URI 配置可將 Windows 設定 app 啟動到特定的設定頁面。

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

或者，您的 app 也可以呼叫 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以從程式碼啟動 [**設定**] app。 這個範例示範如何使用 `ms-settings:privacy-webcam` URI 來啟動進入相機的隱私權設定頁面。

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

上述程式碼會啟動相機的隱私權設定頁面：

![相機隱私權設定。](images/privacyawarenesssettingsapp.png)

如需啟動 URI 的詳細資訊，請參閱[啟動 URI 的預設 app](launch-default-app.md)。

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI 配置參考

使用下列 URI 可開啟 [設定] app 的各個頁面。

> 請注意，設定頁面是否可用因 Windows SKU 而異。 Windows 10 桌面版上所有的設定頁面不一定皆在 Windows 10 行動裝置版上，反之亦然。 附註欄也會擷取必須符合頁面才可供使用的其他需求。

<table border="1">
 <tr>
  <th>類別</th>
  <th>設定頁面</th>
  <th>URI</th>
  <th>附註</th>
 </tr>
 <tr>
  <td rowspan="6">帳戶</td>
  <td>存取工作或學校帳戶</td>
  <td>ms-settings:workplace</td>
  <td></td>
 </tr>
 <tr>
  <td>電子郵件與應用程式帳戶</td>
  <td>ms-settings:emailandaccounts</td>
  <td></td>
 </tr>
 <tr>
  <td>家人與其他使用者</td>
  <td>ms-settings:otherusers</td>
  <td></td>
 </tr>
 <tr>
  <td>登入選項</td>
  <td>ms-settings:signinoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>同步您的設定</td>
  <td>ms-settings:sync</td>
  <td></td>
 </tr>
 <tr>
  <td>您的資訊</td>
  <td>ms-settings:yourinfo</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="4">應用程式</td>
  <td>應用程式與功能</td>
  <td>ms-settings:appsfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td>網站的應用程式</td>
  <td>ms-settings:appsforwebsites</td>
  <td></td>
 </tr>
 <tr>
  <td>預設應用程式</td>
  <td>ms-settings:defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>應用程式與功能</td>
  <td>ms-settings:optionalfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="12">裝置</td>
  <td>USB</td>
  <td>ms-settings:usb</td>
  <td></td>
 </tr>
 <tr>
  <td>音訊與語音</td>
  <td>ms-settings:holographic-audio</td>
  <td>僅適用於混合實境入口網站應用程式已安裝時 (可在 Windows 市集中取得)</td>
 </tr>
 <tr>
  <td>AutoPlay</td>
  <td>ms-settings:autoplay</td>
  <td></td>
 </tr>
 <tr>
  <td>觸控板</td>
  <td>ms-settings:devices-touchpad</td>
  <td>僅適用於觸控板硬體存在時</td>
 </tr>
 <tr>
  <td>手寫筆與 Windows Ink</td>
  <td>ms-settings:pen</td>
  <td></td>
 </tr>
 <tr>
  <td>印表機與掃描器</td>
  <td>ms-settings:printers</td>
  <td></td>
 </tr>
 <tr>
  <td>文字輸入</td>
  <td>ms-settings:typing</td>
  <td></td>
 </tr>
 <tr>
  <td>轉盤</td>
  <td>ms-settings:wheel</td>
  <td>僅適用於撥號已配對時</td>
 </tr>
 <tr>
  <td>預設相機</td>
  <td>ms-settings:camera</td>
  <td></td>
 </tr>
 <tr>
  <td>藍牙</td>
  <td>ms-settings:bluetooth</td>
  <td></td>
 </tr>
 <tr>
  <td>連線的裝置</td>
  <td>ms-settings:connecteddevices</td>
  <td></td>
 </tr>
 <tr>
  <td>滑鼠與觸控板</td>
  <td>ms-settings:mousetouchpad</td>
  <td>觸控板設定僅適用於具有觸控板的裝置上</td>
 </tr>
 <tr>
  <td rowspan="7">輕鬆存取</td>
  <td>朗讀程式</td>
  <td>ms-settings:easeofaccess-narrator</td>
  <td></td>
 </tr>
 <tr>
  <td>放大鏡</td>
  <td>ms-settings:easeofaccess-magnifier</td>
  <td></td>
 </tr>
 <tr>
  <td>高對比</td>
  <td>ms-settings:easeofaccess-highcontrast</td>
  <td></td>
 </tr>
 <tr>
  <td>隱藏式輔助字幕</td>
  <td>ms-settings:easeofaccess-closedcaptioning</td>
  <td></td>
 </tr>
 <tr>
  <td>鍵盤</td>
  <td>ms-settings:easeofaccess-keyboard</td>
  <td></td>
 </tr>
 <tr>
  <td>滑鼠</td>
  <td>ms-settings:easeofaccess-mouse</td>
  <td></td>
 </tr>
 <tr>
  <td>其他選項</td>
  <td>ms-settings:easeofaccess-otheroptions</td>
 </tr>
 <tr>
  <td>其他</td>
  <td>其他</td>
  <td>ms-settings:extras</td>
  <td>僅適用於「設定應用程式」已安裝時 (例如透過第三方)</td>
 </tr>
 <tr>
  <td rowspan="4">遊戲</td>
  <td>廣播</td>
  <td>ms-settings:gaming-broadcasting</td>
  <td></td>
 </tr>
 <tr>
  <td>遊戲列</td>
  <td>ms-settings:gaming-gamebar</td>
  <td></td>
 </tr>
 <tr>
  <td>遊戲 DVR</td>
  <td>ms-settings:gaming-gamedvr</td>
  <td></td>
 </tr>
 <tr>
  <td>遊戲模式</td>
  <td>ms-settings:gaming-gamemode</td>
  <td></td>
 </tr>
 <tr>
  <td>首頁</td>
  <td>\[設定\] 的登陸頁面</td>
  <td>ms-settings:</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="10">網路和網際網路</td>
  <td>Ethernet</td>
  <td>ms-settings:network-ethernet</td>
  <td></td>
 </tr>
 <tr>
  <td>VPN</td>
  <td>ms-settings:network-vpn</td>
  <td></td>
 </tr>
 <tr>
  <td>撥號</td>
  <td>ms-settings:network-dialup</td>
  <td></td>
 </tr>
 <tr>
  <td>DirectAccess</td>
  <td>ms-settings:network-directaccess</td>
  <td>僅適用於 DirectAccess 已啟用時</td>
 </tr>
 <tr>
  <td>Wi-Fi 通話</td>
  <td>ms-settings:network-wificalling</td>
  <td>僅適用於 Wi-Fi 通話已啟用時</td>
 </tr>
 <tr>
  <td>數據使用量</td>
  <td>ms-settings:datausage</td>
  <td></td>
 </tr>
 <tr>
  <td>行動數據與 SIM 卡</td>
  <td>ms-settings:network-cellular</td>
  <td></td>
 </tr>
 <tr>
  <td>行動熱點</td>
  <td>ms-settings:network-mobilehotspot</td>
  <td></td>
 </tr>
 <tr>
  <td>Proxy</td>
  <td>ms-settings:network-proxy</td>
  <td></td>
 </tr>
 <tr>
  <td>狀態</td>
  <td>ms-settings:network-status</td>
  <td></td>
 </tr>
 <tr>
  <td>管理已知的網路</td>
  <td>ms-settings:network-wifisettings</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="3">網路與無線網路</td>
  <td>NFC</td>
  <td>ms-settings:nfctransactions</td>
  <td></td>
 </tr>
 <tr>
  <td>Wi-Fi</td>
  <td>ms-settings:network-wifi</td>
  <td>僅適用於裝置有 Wi-Fi 介面卡</td>
 </tr>
 <tr>
  <td>飛航模式</td>
  <td>ms-settings:network-airplanemode</td>
  <td>在 Windows 8.x 上使用 ms-settings:proximity</td>
 </tr>
 <tr>
  <td rowspan="10">個人化</td>
  <td>開始</td>
  <td>ms-settings:personalization-start</td>
  <td></td>
 </tr>
 <tr>
  <td>佈景主題</td>
  <td>ms-settings:themes</td>
  <td></td>
 </tr>
 <tr>
  <td>瀏覽</td>
  <td>ms-settings:personalization-glance</td>
  <td></td>
 </tr>
 <tr>
  <td>瀏覽列</td>
  <td>ms-settings:personalization-navbar</td>
  <td></td>
 </tr>
 <tr>
  <td>個人化 (類別)</td>
   <td>ms-settings:personalization</td>
   <td></td>
 </tr>
 <tr>
  <td>背景</td>
   <td>ms-settings:personalization-background</td>
   <td></td>
 </tr>
 <tr>
  <td>色彩</td>
   <td>ms-settings:personalization-colors</td>
   <td></td>
 </tr>
 <tr>
  <td>音效</td>
   <td>ms-settings:sounds</td>
   <td></td>
 </tr>
 <tr>
  <td>鎖定畫面</td>
   <td>ms-settings:lockscreen</td>
   <td></td>
 </tr>
 <tr>
  <td>工作列</td>
   <td>ms-settings:taskbar</td>
   <td></td>
 </tr>
 <tr>
  <td rowspan="22">隱私權</td>
  <td>應用程式診斷</td>
   <td>ms-settings:privacy-appdiagnostics</td>
   <td></td>
 </tr>
 <tr>
  <td>通知</td>
   <td>ms-settings:privacy-notifications</td>
   <td></td>
 </tr>
 <tr>
  <td>工作</td>
   <td>ms-settings:privacy-tasks</td>
   <td></td>
 </tr>
 <tr>
  <td>一般</td>
   <td>ms-settings:privacy-general</td>
   <td></td>
 </tr>
 <tr>
  <td>配件專屬應用程式</td>
   <td>ms-settings:privacy-accessoryapps</td>
   <td></td>
 </tr>
 <tr>
  <td>廣告識別碼</td>
   <td>ms-settings:privacy-advertisingid</td>
   <td></td>
 </tr>
 <tr>
  <td>通話</td>
   <td>ms-settings:privacy-phonecall</td>
   <td></td>
 </tr>
 <tr>
  <td>位置</td>
   <td>ms-settings:privacy-location</td>
   <td></td>
 </tr>
 <tr>
  <td>相機</td>
   <td>ms-settings:privacy-webcam</td>
   <td></td>
 </tr>
 <tr>
  <td>麥克風</td>
   <td>ms-settings:privacy-microphone</td>
   <td></td>
 </tr>
 <tr>
  <td>動作</td>
   <td>ms-settings:privacy-motion</td>
   <td></td>
 </tr>
 <tr>
  <td>語音、筆跡與輸入</td>
   <td>ms-settings:privacy-speechtyping</td>
   <td></td>
 </tr>
 <tr>
  <td>帳戶資訊</td>
   <td>ms-settings:privacy-accountinfo</td>
   <td></td>
 </tr>
 <tr>
  <td>連絡人</td>
   <td>ms-settings:privacy-contacts</td>
   <td></td>
 </tr>
 <tr>
  <td>行事曆</td>
   <td>ms-settings:privacy-calendar</td>
   <td></td>
 </tr>
 <tr>
  <td>通訊記錄</td>
   <td>ms-settings:privacy-callhistory</td>
   <td></td>
 </tr>
 <tr>
  <td>電子郵件</td>
  <td>ms-settings:privacy-email</td>
  <td></td>
 </tr>
 <tr>
  <td>訊息中心</td>
    <td>ms-settings:privacy-messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>無線通訊</td>
    <td>ms-settings:privacy-radios</td>
  <td></td>
 </tr>
 <tr>
  <td>背景應用程式</td>
    <td>ms-settings:privacy-backgroundapps</td>
  <td></td>
 </tr>
 <tr>
  <td>其他裝置</td>
    <td>ms-settings:privacy-customdevices</td>
  <td></td>
 </tr>
 <tr>
  <td>意見反應與診斷</td>
    <td>ms-settings:privacy-feedback</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Surface Hub</td>
  <td>帳戶</td>
    <td>ms-settings:surfacehub-accounts</td>
      <td></td>
  </tr>
  <tr>
    <td>小組會議</td>
      <td>ms-settings:surfacehub-calling</td>
      <td></td>
  </tr>
  <tr>
    <td>小組裝置管理</td>
      <td>ms-settings:surfacehub-devicemanagenent</td>
      <td></td>
  </tr>
  <tr>
    <td>工作階段清理</td>
      <td>ms-settings:surfacehub-sessioncleanup</td>
      <td></td>
  </tr>
  <tr>
    <td>歡迎畫面</td>
      <td>ms-settings:surfacehub-welcome</td>
      <td></td>
  </tr>
    <td rowspan="19">系統</td>
    <td>共用體驗</td>
      <td>ms-settings:crossdevice</td>
    <td></td>
 </tr>
 <tr>
  <td>顯示</td>
    <td>ms-settings:display</td>
  <td></td>
 </tr>
 <tr>
  <td>多工</td>
    <td>ms-settings:multitasking</td>
  <td></td>
 </tr>
 <tr>
  <td>投影到此電腦</td>
    <td>ms-settings:project</td>
  <td></td>
 </tr>
 <tr>
  <td>平板電腦模式</td>
    <td>ms-settings:tabletmode</td>
  <td></td>
 </tr>
 <tr>
  <td>工作列</td>
    <td>ms-settings:taskbar</td>
  <td></td>
 </tr>
 <tr>
  <td>電話</td>
    <td>ms-settings:phone-defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>顯示</td>
    <td>ms-settings:screenrotation</td>
  <td></td>
 </tr>
 <tr>
  <td>通知與動作</td>
    <td>ms-settings:notifications</td>
  <td></td>
 </tr>
 <tr>
  <td>電話</td>
    <td>ms-settings:phone</td>
  <td></td>
 </tr>
 <tr>
  <td>訊息中心</td>
    <td>ms-settings:messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>省電模式</td>
  <td>ms-settings:batterysaver</td>
  <td>僅適用於具有電池的裝置上，例如平板電腦</td>
 </tr>
 <tr>
  <td>電池使用情況</td>
  <td>ms-settings:batterysaver-usagedetails</td>
  <td>僅適用於具有電池的裝置上，例如平板電腦</td>
 </tr>
 <tr>
  <td>電源與睡眠</td>
  <td>ms-settings:powersleep</td>
  <td></td>
 </tr>
 <tr>
  <td>關於</td>
    <td>ms-settings:about</td>
  <td></td>
 </tr>
 <tr>
  <td>儲存空間</td>
    <td>ms-settings:storagesense</td>
  <td></td>
 </tr>
 <tr>
  <td>儲存空間感知器</td>
    <td>ms-settings:storagepolicies</td>
  <td></td>
 </tr>
 <tr>
  <td>加密</td>
    <td>ms-settings:deviceencryption</td>
  <td></td>
 </tr>
 <tr>
  <td>離線地圖</td>
    <td>ms-settings:maps</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">時間與語言</td>
  <td>日期和時間</td>
    <td>ms-settings:dateandtime</td>
  <td></td>
 </tr>
 <tr>
  <td>地區及語言</td>
    <td>ms-settings:regionlanguage</td>
  <td></td>
 </tr>
 <tr>
     <td>語音功能的語言</td>
     <td>ms-settings:speech</td>
     <td></td>
 </tr>
 <tr>
     <td>拼音鍵盤</td>
     <td>ms-settings:regionlanguage-chsime-pinyin</td>
     <td>適用於 Microsoft 拼音輸入法編輯器有安裝時</td>
 </tr>
 <tr>
     <td>五筆輸入模式</td>
     <td>ms-settings:regionlanguage-chsime-wubi</td>
     <td>適用於 Microsoft 五筆輸入法編輯器有安裝時</td>
 </tr>
 <tr>
  <td rowspan="13">更新與安全性</td>
  <td>Windows 說明設定</td>
    <td>ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment</td>
  </tr>
  <tr>
    <td>備份</td>
      <td>ms-settings:backup</td>
    <td></td>
 </tr>
 <tr>
  <td>尋找我的裝置</td>
    <td>ms-settings:findmydevice</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 測試人員計畫</td>
  <td>ms-settings:windowsinsider</td>
  <td>僅適用於使用者已在 WIP 中註冊時</td>
 </tr>
 <tr>
  <td>Windows Update</td>
  <td>ms-settings:windowsupdate</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-history</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-options</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-restartoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-action</td>
  <td></td>
 </tr>
 <tr>
  <td>啟用</td>
    <td>ms-settings:activation</td>
  <td></td>
 </tr>
 <tr>
  <td>修復</td>
    <td>ms-settings:recovery</td>
  <td></td>
 </tr>
 <tr>
  <td>疑難排解</td>
    <td>ms-settings:troubleshoot</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Defender</td>
    <td>ms-settings:windowsdefender</td>
  <td></td>
 </tr>
 <tr>
  <td>適用於開發人員</td>
    <td>ms-settings:developers</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="2">使用者帳戶</td>
  <td>Windows Anywhere</td>
  <td>ms-settings:windowsanywhere</td>
  <td>裝置必須具有 Windows Anywhere 功能</td>
 </tr>
 <tr>
  <td>佈建</td>
  <td>ms-settings:workplace-provisioning</td>
  <td>僅適用於企業已部署佈建套件時</td>
 </tr>
</table><br/>  
