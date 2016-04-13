---
title: 啟動 Windows 設定 app
description: 了解如何從您的 app 啟動 Windows 設定 app。 本主題描述 ms-settings URI 配置。 使用此 URI 配置可將 Windows 設定 app 啟動到特定的設定頁面。
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
---

# 啟動 Windows 設定 app


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何從您的 app 啟動 Windows 設定 app。 本主題描述 **ms-settings:** URI 配置。 使用此 URI 配置可將 Windows 設定 app 啟動到特定的設定頁面。

啟動設定 app 是撰寫隱私權感知 app 的重要部分。 如果您的 app 無法存取敏感資源，建議讓使用者能夠方便地連結到該資源的隱私權設定。 如需詳細資訊，請參閱[隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh768223)。

## 如何啟動設定 app


如果隱私權設定不允許您的 app 存取敏感資源，建議讓使用者能夠方便地連結到**設定** app 中的隱私權設定。 這會讓使用者更容易變更其設定。

若要直接啟動到**設定** app，請使用 `ms-settings:` URI 配置，如下列範例所示。

在這個範例中，會使用超連結 XAML 控制項與 `ms-settings:privacy-microphone` URI 來啟動麥克風的隱私權設定頁面。

```xaml
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

或者，您的 app 也可以呼叫 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以從程式碼啟動 [**設定**] app。

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

這個範例示範如何使用 `ms-settings:privacy-webcam` URI 啟動相機的隱私權設定頁面。

![相機隱私權設定。](images/privacyawarenesssettingsapp.png)

如需啟動 URI 的詳細資訊，請參閱[啟動 URI 的預設 app](launch-default-app.md)。

## ms-settings: URI 配置參考


使用下列 URI 可開啟 [設定] app 的各個頁面。 請注意，[支援的 SKU] 欄會指出適用於傳統型版本 (家用版、專業版、企業版及教育版) 的 Windows 10、「Windows 10 行動裝置版」或兩者中是否有設定頁面。

| 類別           | 設定頁面                          | 支援的 SKU | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| 首頁          | [設定] 的登陸頁面              | 兩者           | ms-settings:                              |
| 系統             | 顯示器                                | 兩者           | ms-settings:screenrotation                |
|                    | 通知與動作                | 兩者           | ms-settings:notifications                 |
|                    | 手機                                  | 僅限行動裝置版    | ms-settings:phone                         |
|                    | 訊息中心                              | 僅限行動裝置版    | ms-settings:messaging                     |
|                    | 省電模式                          | 兩者           | ms-settings:batterysaver                  |
|                    | 省電模式 / 省電模式設定 | 兩者           | ms-settings:batterysaver-settings         |
|                    | 省電模式 / 電池使用情況            | 兩者           | ms-settings:batterysaver-usagedetails     |
|                    | 電源與睡眠                          | 僅限傳統型版本   | ms-settings:powersleep                    |
|                    | 傳統型版本：關於                         | 兩者           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | 行動裝置版：裝置加密              |                |                                           |
|                    | 離線地圖                           | 兩者           | ms-settings:maps                          |
|                    | 關於                                  | 兩者           | ms-settings:about                         |
| 裝置            | 預設相機                         | 僅限行動裝置版    | ms-settings:camera                        |
|                    | 藍牙                              | 僅限傳統型版本   | ms-settings:bluetooth                     |
|                    | 滑鼠與觸控板                       | 兩者           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | 兩者           | ms-settings:nfctransactions               |
| 網路與無線網路 | Wi-Fi                                  | 兩者           | ms-settings:network-wifi                  |
|                    | 飛航模式                          | 兩者           | ms-settings:network-airplanemode          |
| 網路和網際網路 | 數據使用量                             | 兩者           | ms-settings:datausage                     |
|                    | 行動數據與 SIM 卡                         | 兩者           | ms-settings:network-cellular              |
|                    | 行動熱點                         | 兩者           | ms-settings:network-mobilehotspot         |
|                    | Proxy                                  | 兩者           | ms-settings:network-proxy                 |
| 個人化    | 個人化 (類別)             | 兩者           | ms-settings:personalization               |
|                    | 背景                             | 僅限傳統型版本   | ms-settings:personalization-background    |
|                    | 色彩                                 | 兩者           | ms-settings:personalization-colors        |
|                    | 音效                                 | 僅限行動裝置版    | ms-settings:sounds                        |
|                    | 鎖定畫面                            | 兩者           | ms-settings:lockscreen                    |
| 帳戶           | 您的電子郵件與帳戶                | 兩者           | ms-settings:emailandaccounts              |
|                    | 公司存取                            | 兩者           | ms-settings:workplace                     |
|                    | 同步您的設定                     | 兩者           | ms-settings:sync                          |
| 時間與語言  | 日期和時間                            | 兩者           | ms-settings:dateandtime                   |
|                    | 地區及語言                      | 僅限傳統型版本   | ms-settings:regionlanguage                |
| 輕鬆存取     | 朗讀程式                               | 兩者           | ms-settings:easeofaccess-narrator         |
|                    | 放大鏡                              | 兩者           | ms-settings:easeofaccess-magnifier        |
|                    | 高對比                          | 兩者           | ms-settings:easeofaccess-highcontrast     |
|                    | 隱藏式輔助字幕                        | 兩者           | ms-settings:easeofaccess-closedcaptioning |
|                    | 鍵盤                               | 兩者           | ms-settings:easeofaccess-keyboard         |
|                    | 滑鼠                                  | 兩者           | ms-settings:easeofaccess-mouse            |
|                    | 其他選項                          | 兩者           | ms-settings:easeofaccess-otheroptions     |
| 隱私權            | 位置                               | 兩者           | ms-settings:privacy-location              |
|                    | 相機                                 | 兩者           | ms-settings:privacy-webcam                |
|                    | 麥克風                             | 兩者           | ms-settings:privacy-microphone            |
|                    | 動作                                 | 兩者           | ms-settings:privacy-motion                |
|                    | 語音、筆跡與輸入                | 兩者           | ms-settings:privacy-speechtyping          |
|                    | 帳戶資訊                           | 兩者           | ms-settings:privacy-accountinfo           |
|                    | 連絡人                               | 兩者           | ms-settings:privacy-contacts              |
|                    | 行事曆                               | 兩者           | ms-settings:privacy-calendar              |
|                    | 通訊記錄                           | 兩者           | ms-settings:privacy-callhistory           |
|                    | 電子郵件                                  | 兩者           | ms-settings:privacy-email                 |
|                    | 訊息中心                              | 兩者           | ms-settings:privacy-messaging             |
|                    | 無線通訊                                 | 兩者           | ms-settings:privacy-radios                |
|                    | 背景應用程式                        | 兩者           | ms-settings:privacy-backgroundapps        |
|                    | 其他裝置                          | 兩者           | ms-settings:privacy-customdevices         |
|                    | 意見反應與診斷                 | 兩者           | ms-settings:privacy-feedback              |
| 更新與安全性  | 適用於開發人員                         | 兩者           | ms-settings:developers                    |
 





<!--HONumber=Mar16_HO1-->


