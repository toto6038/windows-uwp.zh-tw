---
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: 安裝 Microsoft Store Engagement and Monetization SDK 之後，請依照本主題中的指示，在您的 app 中使用廣告流量分配者控制項。
title: 新增和使用廣告流量分配者控制項
---

# 新增和使用廣告流量分配者控制項


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[安裝 Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) 之後，請依照本主題中的指示，在您的 app 中使用廣告流量分配者控制項。 如需廣告流量分配目前支援的廣告網路和專案類型清單，請參閱[選取和管理廣告網路](select-and-manage-your-ad-networks.md)。

## 將廣告流量分配者控制項新增到專案


若要將廣告流量分配者控制項的執行個體新增到專案：

1.  在 Visual Studio 中，開啟您的專案。
2.  如果您要將廣告流量分配新增到已經在使用任何受廣告流量分配支援之[廣告網路](select-and-manage-your-ad-networks.md)來獲利的 app，請先移除現有的廣告實作及其所有參考，再繼續進行。
3.  在 [方案總管]**** 中，於 App 內尋找想要顯示廣告的頁面，然後按兩下該頁面，在設計工具中加以開啟。
4.  將新的 **AdMediatorControl** 從 [工具箱]**** 拖曳到設計工具中 (務必將控制項拖曳到設計工具中，而不是拖曳到 XAML 程式碼中)。 將控制項放置在您想要顯示廣告的位置。 如果您想要在 app 的多個區域中顯示廣告，可新增多個控制項。

    **AdMediatorControl** 位於下列 [工具箱]**** 位置中：

    -   在通用 Windows 平台 (UWP) 專案中，使用 [AdMediator 通用]**** 區段下方的 **AdMediatorControl**。
    -   在使用 C# 或 Visual Basic 與 XAML 的 Windows 8.1 或 Windows Phone 8.1 專案中，使用 [AdMediator]**** 區段下方的 **AdMediatorControl**。
    -   在 Windows Phone Silverlight 專案中，使用 [所有 Windows Phone 控制項]**** 區段下方的 **AdMediatorControl**。

    **注意** 當您在使用 C# 或 Visual Basic 與 XAML 的 UWP、Windows 8.1 或 Windows Phone 8.1 專案中首次將 **AdMediatorControl** 控制項拖曳到設計工具時，Visual Studio 會將必要的廣告流量分配者組件參考新增到您的專案，但還不會將該控制項新增到設計工具。 若要新增控制項，請按一下 Visual Studio 所顯示之訊息中的 [確定]，接著等候設計工具重新整理，然後將控制項再次拖曳回設計工具。 如果您仍然無法順利將控制項新增到設計工具，請確定您的專案目標為應用程式適用的處理器架構 (例如，**x86**)，而不是**任何 CPU**。 如果專案建置平台的目標是「任何 CPU」****，則控制項無法新增到設計工具。

5.  Visual Studio 會將廣告流量分配者組件參考新增到您的專案，並將適用於廣告流量分配者控制項的 XAML 插入目前頁面，包括該控制項的唯一識別碼與名稱。 組件參考與 XAML 會根據您的目標平台而不同。 例如，如果是通用 Windows 平台 (UWP) app，組件名稱就是 **Microsoft.AdMediator.Universal**，而且會產生類似下列範例的 XAML。

    ```xml
    // Code that gets added to the XAML page header
        xmlns:Universal="using:Microsoft.AdMediator.Universal"

        // Code that gets added for the ad mediator control
        <Universal:AdMediatorControl x:Name="AdMediator_3D4884"
         Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250"
         Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0"
         VerticalAlignment="Top" Width="300"/>
    ```

    **Name** 元素可在您設定廣告流量分配時，協助識別 app 中的特定控制項。 您可以將此元素變更成您想要的任何值，但是請勿變更或複製 **Id** 元素。 這個 **Id** 對您 app 內的每個控制項必須是唯一的。

6.  視需要調整控制項的大小與位置。 如需詳細資訊，請參閱[調整大小和位置](#adjust-size-and-position)。

## 設定廣告網路

當您新增想要的所有控制項之後，就已經準備好透過「已連接服務」來設定廣告網路。

**重要** 如果您稍後新增額外的 AdMediatorControl，就需要再次透過「已連接服務」來設定它。 否則，新控制項將無法使用廣告流量分配。

設定廣告網路：

1.  在 [方案總管] 中專案的名稱上按一下滑鼠右鍵，然後按一下 [加入]**** > [已連接服務...]****， 來啟動 [加入已連接服務]**** 視窗 (Visual Studio 2015) 或 [服務管理員]**** 視窗 (Visual Studio 2013)。
2.  如果您使用的是 Visual Studio 2015，請按一下 [Ad Mediator]****，然後按一下 [設定]**** 以開啟 [Ad Mediator]**** 視窗。 如果您使用的是 Visual Studio 2013，只要按一下 [服務管理員]**** 左窗格中的 [Ad Mediator]**** 即可。

    AdMediator.config 檔案就會新增到您的專案中。 這個檔案是您專案中初始廣告網路組態設定在本機的儲存位置。

3.  在 [Ad Mediator]**** (Visual Studio 2015) 或 [服務管理員]**** (Visual Studio 2013) 視窗中，按一下 [選取廣告網路]****，選取您想要使用的廣告網路，然後按一下 [選取廣告網路]**** 視窗中的 [確定]****。

    **祕訣** 新增所有您擁有帳戶的網路是相當好的做法，即使您並不打算立即在您的 app 中使用所有這些網路。 發行 app 之後，您將可以在「開發人員中心」中設定每個網路的使用頻率 (或開始使用您之前未使用過的網路)，而不需要進行程式碼變更並重新送出 app。

    Visual Studio 會為所選取的廣告網路擷取必要的組件，並將這些組件參考新增到您的專案。 此程序完成後，按一下 [擷取狀態]**** 對話方塊中的 [確定]****。

4.  在 [Ad Mediator]**** (Visual Studio 2015) 或 [服務管理員]**** (Visual Studio 2013) 視窗中，可以選擇性地選取每個網路，並按 [設定]**** 來輸入測試 app 時要使用之每個網路的設定資訊。 此資訊會儲存至您專案中的 AdMediator.config 檔案。 您將可以在 Windows 開發人員中心儀表板設定廣告網路行為時修改此資訊。 如需詳細資訊，請參閱[提交您的 app 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)。
    **注意** 如果您在此步驟中沒有輸入設定資訊，當您在開發電腦 (針對 UWP和 Windows 8.1 XAML app) 上或在模擬器或裝置 (針對 Windows Phone app) 上執行 app 時，廣告流量分配將自動使用測試設定值。

5.  在 [Ad Mediator]**** (Visual Studio 2015) 或 [服務管理員]**** (Visual Studio 2013) 視窗中，確認您已選取的每個廣告網路都有顯示 [已擷取]****。 按一下 [確定]****，送出您對專案所做的變更。

**注意** 如果您稍後升級到較新版的 Microsoft Store Engagement and Monetization SDK，就需要再次啟動 [已連接服務]****，以確保所有自動擷取的廣告網路 DLL 都已正確更新。

### 宣告所需的功能

每個廣告網路可能都需要特定的 app 功能。 每個提供者會在 [Ad Mediator]**** (適用於 Visual Studio 2015) 或 [服務管理員]**** (適用於 Visual Studio 2013) 視窗中顯示這些功能。 請務必在 app 的資訊清單中宣告全部所需的功能，讓廣告能夠正確顯示。

下列螢幕擷取畫面顯示 Windows 8.1 或 Windows Phone 8.1 XAML app 中數個廣告網路所需的功能。

![顯示所有參考皆已擷取的 [服務管理員]](images/ad-med-8.jpg)

下列螢幕擷取畫面顯示 Windows Phone 8.1 Silverlight app 中數個廣告網路所需的功能。

![顯示所有參考皆已擷取的 [服務管理員]](images/ad-med-6.jpg)
### 手動擷取廣告網路 DLL

在某些情況下，您可能會看到某些 DLL 沒有被擷取。 在此情況下，您將需要手動新增它們。 如需可下載個別組件的連結，請參閱[選取和管理廣告網路](select-and-manage-your-ad-networks.md)。

**注意** 手動新增 DLL 時，您可能會收到錯誤訊息，指出「無法將較高版本或不相容組件的參考加入專案。」 若要解決這個錯誤，請在 [檔案總管] 中的 DLL 上按一下滑鼠右鍵，然後選取 [屬性]****。 在 [安全性] 區段中，按一下 [解除封鎖]****。

![用於解決錯誤訊息的 [解除封鎖] 按鈕](images/ad-med-4.png)
## 調整大小和位置

您可以在設計工具或 XAML 程式碼中設定廣告流量分配者控制項的大小和位置。 請確定此大小足夠容納要從廣告網路顯示的所有廣告。 有些廣告網路在偵測到畫布大小不足以讓廣告完全顯示時，可能就不會提供廣告。 如果您的任何廣告單位的大小將會超出預設大小，您可以調整畫布大小以容納您最大的廣告。

當您將控制項拖曳到設計工具時，預設的控制項大小是：

-   UWP 和 Windows 8.1 XAML：寬 300 x 高 250。
-   Windows Phone 8.1 XAML：寬 400 x 高 67。
-   Windows Phone 8 和 Windows Phone 8.1 Silverlight：寬 480 x 高 80。

您可以使用選擇性參數 **Width** 和 **Height** 來覆寫預設大小，如下所示。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

您也可以指定每個控制項的放置位置，以根據您的 app 需求，配合各種大小和佈置方式。 針對某些廣告網路，您可以使用選擇性參數來進行調整。 例如，若要讓來自 Microsoft Advertising 的廣告靠左下角對齊：

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Microsoft Advertising 支援的廣告大小

Microsoft Advertising 只支援 Interactive Advertising Bureau (IAB) 針對在下列平台中執行的 app 所建議之下列標準大小的廣告。

-   Windows 10 和 Windows 8.1：
    -   160 x 600
    -   250 x 250 (請注意，很少有廣告會使用這個大小。 我們建議使用其他大小，以便將填滿速率與 eCPM 最大化。)
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 行動裝置版、Windows Phone 8.1 及 Windows Phone 8：
    -   300 x 50
    -   320 x 50
    -   480 x 80
    -   640 x 100

您可能想要指定與 Microsoft Advertising 支援的其中一個廣告大小不符的廣告流量分配者控制項大小 (例如，如果其他大小更適合您 app 的 UI，或者您也以支援其他廣告大小的廣告網路為目標)。 若要這樣做，請在設計工具或 XAML 程式碼中確切指定所需的控制項大小，然後為 Microsoft Advertising 的 **Width** 和 **Height** 選擇性參數指派不會超出控制項界限範圍且最接近所支援大小的尺寸。 控制項會完全依照您在設計工具中指定的大小顯示，但 Microsoft Advertising 提供之廣告的大小會符合您使用 **Width** 和 **Height** 選擇性參數指定的大小。

例如，假設您有一個 UWP App 且想要讓廣告流量分配者控制項以 300 x 300 顯示，則可在設計工具或 XAML 程式碼中將控制項設為 300 x 300。 然後，將 Microsoft Advertising 的 **Width** 選擇性參數指派為 300，以及將 **Height** 選擇性參數指派為 250，如以下程式碼所示。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

若要確認您的大小設定與 Microsoft Advertising 相容，請[測試您的廣告流量分配實作](test-your-ad-mediation-implementation.md)，並確認有顯示 Microsoft Advertising 的測試廣告。

## 暫停、繼續及停用廣告流量分配

如果您想要在 app 執行階段期間，將廣告流量分配依任何指定的時間長度暫停，請使用 AdMediatorControl.Pause() 方法。 請注意，當您這樣做時，最近一次的廣告將會繼續顯示，直到您透過呼叫 AdMediatorControl.Resume() 方法來繼續流量分配為止。

若要完全停用廣告流量分配者，請使用 AdMediatorControl.Disable() 方法。 這會移除任何正在顯示的廣告，並將流量分配者的磁碟使用量降到最低。 您可以呼叫 AdMediatorControl.Resume() 來繼續流量分配，但請注意，在已停用廣告流量分配之後，啟動時間會比平常久。

## 設定逾時

您可以指定讓廣告流量分配在向該廣告網路要求廣告之後，應該要等候幾秒 (從 2 到 60) 才放棄該要求，再改向另一個網路提出要求。 所有廣告網路的預設逾時為 15 秒。

以下程式碼示範如何指定 Microsoft Advertising 的逾時持續時間。 您可以視需要修改持續時間和網路。

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10);
```

**注意** 您也可以在開發人員中心儀表板上的 [利用廣告營利]**** 頁面中設定逾時值。 如果您在程式碼和儀表板中設定逾時，則您在程式碼中設定的值會覆寫儀表板的值。

## 事件處理

新增用以記錄事件及擷取廣告流量分配錯誤的程式碼將有助於進行疑難排解。 以下程式碼範例會為來自控制項的特定事件新增事件處理常式。

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## 處理來自廣告網路的未處理例外狀況

**注意** 我們已經在測試中識別出一些來自特定廣告網路的未處理例外狀況，必須在 app 內處理這些例外狀況，以避免 app 因此當機。 強烈建議您複製下方的程式碼範例，並貼到 App.xaml.cs 檔案中。

用於使用 C# 和 XAML 之 UWP、Windows 8.1、Windows Phone app 的程式碼

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException &amp;&amp; exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

用於 Windows Phone Silverlight app 的程式碼

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) &amp;&amp; exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException &amp;&amp; exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) &amp;&amp; exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) &amp;&amp; exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }

   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## 相關主題

* [選取和管理廣告網路](select-and-manage-your-ad-networks.md)
* [測試您的廣告流量分配實作](test-your-ad-mediation-implementation.md)
* [送出您的 App 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)
* [疑難排解廣告流量分配](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


