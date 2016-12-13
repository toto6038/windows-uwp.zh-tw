---
author: TylerMSFT
title: "啟動 Windows 設定 app"
description: "了解如何從您的 app 啟動 Windows 設定 app。 本主題描述 ms-settings URI 配置。 使用此 URI 配置可將 Windows 設定 app 啟動到特定的設定頁面。"
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
translationtype: Human Translation
ms.sourcegitcommit: 1135feec72510e6cbe955161ac169158a71097b9
ms.openlocfilehash: f762d7eb70a0e9119f32350a815691109f994c75

---

# <a name="launch-the-windows-settings-app"></a>啟動 Windows 設定 app

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何從您的 app 啟動 Windows 設定 app。 本主題描述 **ms-settings:** URI 配置。 使用此 URI 配置可將 Windows 設定 app 啟動到特定的設定頁面。

啟動設定 app 是撰寫隱私權感知 app 的重要部分。 如果您的 app 無法存取敏感資源，建議讓使用者能夠方便地連結到該資源的隱私權設定。 如需詳細資訊，請參閱[隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh768223)。

## <a name="how-to-launch-the-settings-app"></a>如何啟動設定 App

若要啟動 [設定] App，請使用 `ms-settings:` URI 配置，如下列範例所示。

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

使用下列 URI 可開啟 [設定] app 的各個頁面。 請注意，[支援的 SKU] 欄會指出適用於傳統型版本 (家用版、專業版、企業版及教育版) 的 Windows 10、「Windows 10 行動裝置版」或兩者中是否有設定頁面。

<table border="1">
    <tr>
        <th>類別</th>
        <th>設定頁面</th>
        <th>支援的 SKU</th>
        <th>URI</th>
    </tr>
    <tr>
        <td>首頁</td>
        <td>\[設定\] 的登陸頁面</td>
        <td>兩者</td>
        <td>ms-settings:</td>
    </tr>
    <tr>
        <td rowspan="10">系統</td>
        <td>顯示器</td>
        <td>兩者</td>
        <td>ms-settings:screenrotation</td>
    </tr>
    <tr>
        <td>通知與動作</td>
        <td>兩者</td>
        <td>ms-settings:notifications</td>
    </tr>
    <tr>
        <td>手機</td>
        <td>僅限行動裝置版</td>
        <td>ms-settings:phone</td>
    </tr>
    <tr>
        <td>訊息中心</td>
        <td>僅限行動裝置版</td>
        <td>ms-settings:messaging</td>
    </tr>
    <tr>
        <td>省電模式</td>
        <td>兩者<br>僅適用於具有電池的裝置上，例如平板電腦</td>
        <td>ms-settings:batterysaver</td>
    </tr>
    <tr>
        <td>電池使用情況</td>
        <td>兩者<br>僅適用於具有電池的裝置上，例如平板電腦</td>
        <td>ms-settings:batterysaver-usagedetails</td>
    </tr>
    <tr>
        <td>電源與睡眠</td>
        <td>僅限傳統型版本</td>
        <td>ms-settings:powersleep</td>
    </tr>
    <tr>
        <td>關於</td>
        <td>兩者</td>
        <td>ms-settings:about</td>
    </tr>
    <tr>
        <td>加密</td>
        <td>兩者</td>
        <td>ms-settings:deviceencryption</td>
    </tr>
    <tr>
        <td>離線地圖</td>
        <td>兩者</td>
        <td>ms-settings:maps</td>
    </tr>
    <tr>
        <td rowspan="4">裝置</td>
        <td>預設相機</td>
        <td>僅限行動裝置版</td>
        <td>ms-settings:camera</td>
    </tr>
    <tr>
        <td>藍牙</td>
        <td>僅限傳統型版本</td>
        <td>ms-settings:bluetooth</td>
    </tr>
    <tr>
        <td>連線的裝置</td>
        <td>僅限電腦</td>
        <td>ms-settings:connecteddevices</td>
    </tr>
    <tr>
        <td>滑鼠與觸控板</td>
        <td>兩者<br>觸控板設定僅適用於具有觸控板的裝置上</td>
        <td>ms-settings:mousetouchpad</td>
    </tr>
    <tr>
        <td rowspan="3">網路與無線網路</td>
        <td>NFC</td>
        <td>兩者</td>
        <td>ms-settings:nfctransactions</td>
    </tr>
    <tr>
        <td>Wi-Fi</td>
        <td>兩者</td>
        <td>ms-settings:network-wifi</td>
    </tr>
    <tr>
        <td>飛航模式</td>
        <td>兩者</td>
        <td>ms-settings:network-airplanemode</td>
    </tr>
    <tr>
        <td rowspan="5">網路和網際網路</td>
        <td>數據使用量</td>
        <td>兩者</td>
        <td>ms-settings:datausage</td>
    </tr>
    <tr>
        <td>行動數據與 SIM 卡</td>
        <td>兩者</td>
        <td>ms-settings:network-cellular</td>
    </tr>
    <tr>
        <td>行動熱點</td>
        <td>兩者</td>
        <td>ms-settings:network-mobilehotspot</td>
    </tr>
    <tr>
        <td>Proxy</td>
        <td>僅限電腦</td>
        <td>ms-settings:network-proxy</td>
    </tr>
    <tr>
        <td>狀態</td>
        <td>僅限傳統型版本</td>
        <td>ms-settings:network-status</td>
    </tr>
    <tr>
        <td rowspan="5">個人化</td>
        <td>個人化 (類別)</td>
        <td>兩者</td>
        <td>ms-settings:personalization</td>
    </tr>
    <tr>
        <td>背景</td>
        <td>僅限傳統型版本</td>
        <td>ms-settings:personalization-background</td>
    </tr>
    <tr>
        <td>色彩</td>
        <td>兩者</td>
        <td>ms-settings:personalization-colors</td>
    </tr>
    <tr>
        <td>音效</td>
        <td>僅限行動裝置版 </td>
        <td>ms-settings:sounds</td>
    </tr>
    <tr>
        <td>鎖定畫面</td>
        <td>兩者</td>
        <td>ms-settings:lockscreen</td>
    </tr>
    <tr>
        <td rowspan="7">帳戶</td>
        <td>存取公司或學校資源</td>
        <td>兩者</td>
        <td>ms-settings:workplace</td>
    </tr>
    <tr>
        <td>電子郵件與 App 帳戶</td>
        <td>兩者</td>
        <td>ms-settings:emailandaccounts</td>
    </tr>
    <tr>
        <td>家人與其他使用者</td>
        <td>兩者</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>登入選項</td>
        <td>兩者</td>
        <td>ms-settings:signinoptions</td>
    </tr>
    <tr>
        <td>同步您的設定</td>
        <td>兩者</td>
        <td>ms-settings:sync</td>
    </tr>
    <tr>
        <td>其他使用者</td>
        <td>兩者</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>您的資訊</td>
        <td>兩者</td>
        <td>ms-settings:yourinfo</td>
    </tr>
    <tr>
        <td rowspan="2">時間與語言</td>
        <td>日期和時間</td>
        <td>兩者</td>
        <td>ms-settings:dateandtime</td>
    </tr>
    <tr>
        <td>地區及語言</td>
        <td>僅限傳統型版本</td>
        <td>ms-settings:regionlanguage</td>
    </tr>
    <tr>
        <td rowspan="7">輕鬆存取</td>
        <td>朗讀程式</td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-narrator</td>
    </tr>
    <tr>
        <td>放大鏡</td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-magnifier</td>
    </tr>
    <tr>
        <td>高對比 </td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-highcontrast</td>
    </tr>
    <tr>
        <td>隱藏式輔助字幕</td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-closedcaptioning</td>
    </tr>
    <tr>
        <td>鍵盤</td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-keyboard</td>
    </tr>
    <tr>
        <td>滑鼠</td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-mouse</td>
    </tr>
    <tr>
        <td>其他選項</td>
        <td>兩者</td>
        <td>ms-settings:easeofaccess-otheroptions</td>
    </tr>
    <tr>
        <td rowspan="15">隱私權</td>
        <td>位置</td>
        <td>兩者</td>
        <td>ms-settings:privacy-location</td>
    </tr>
    <tr>
        <td>相機</td>
        <td>兩者</td>
        <td>ms-settings:privacy-webcam</td>
    </tr>
    <tr>
        <td>麥克風</td>
        <td>兩者</td>
        <td>ms-settings:privacy-microphone</td>
    </tr>
    <tr>
        <td>動作</td>
        <td>兩者</td>
        <td>ms-settings:privacy-motion</td>
    </tr>
    <tr>
        <td>語音、筆跡與輸入</td>
        <td>兩者</td>
        <td>ms-settings:privacy-speechtyping</td>
    </tr>
    <tr>
        <td>帳戶資訊</td>
        <td>兩者</td>
        <td>ms-settings:privacy-accountinfo</td>
    </tr>
    <tr>
        <td>連絡人</td>
        <td>兩者</td>
        <td>ms-settings:privacy-contacts</td>
    </tr>
    <tr>
        <td>行事曆</td>
        <td>兩者</td>
        <td>ms-settings:privacy-calendar</td>
    </tr>
    <tr>
        <td>通訊記錄</td>
        <td>兩者</td>
        <td>ms-settings:privacy-callhistory</td>
    </tr>
    <tr>
        <td>電子郵件</td>
        <td>兩者</td>
        <td>ms-settings:privacy-email</td>
    </tr>
    <tr>
        <td>訊息中心</td>
        <td>兩者</td>
        <td>ms-settings:privacy-messaging</td>
    </tr>
    <tr>
        <td>無線通訊</td>
        <td>兩者</td>
        <td>ms-settings:privacy-radios</td>
    </tr>
    <tr>
        <td>背景應用程式</td>
        <td>兩者</td>
        <td>ms-settings:privacy-backgroundapps</td>
    </tr>
    <tr>
        <td>其他裝置</td>
        <td>兩者</td>
        <td>ms-settings:privacy-customdevices</td>
    </tr>
    <tr>
        <td>意見反應與診斷</td>
        <td>兩者</td>
        <td>ms-settings:privacy-feedback</td>
    </tr>
    <tr>
        <td>更新與安全性</td>
        <td>適用於開發人員</td>
        <td>兩者</td>
        <td>ms-settings:developers</td>
    </tr>
</table><br/>



<!--HONumber=Dec16_HO1-->


