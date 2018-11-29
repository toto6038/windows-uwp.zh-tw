---
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: 使用 Windows UI 和元件擴充您的傳統型應用程式
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76e4b60e1cd25a205d6a304f12a0b04f5db693b5
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8215307"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>使用現代化 UWP 元件擴充您的傳統型應用程式

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 如果您想要新增這些體驗，請使用 UWP 專案和 Windows 執行階段元件擴充您的傳統型應用程式。

在許多情況下，您可以直接從傳統型應用程式呼叫 Windows 執行階段 Api，因此之前檢閱本指南中，請參閱[增強 Windows 10](desktop-to-uwp-enhance.md)。

>[!NOTE]
>本指南假設您已經為您傳統型應用程式建立 Windows 應用程式套件。 如果您還沒有這樣做，請參閱[封裝傳統型應用程式](desktop-to-uwp-root.md)。

當您準備好時，我們就可以開始進行操作。

<a id="setup" />

## <a name="first-setup-your-solution"></a>首先，設定您的解決方案

新增一或多個 UWP 專案和執行階段元件至您的解決方案。

從包含 **Windows 應用程式封裝專案**的解決方案開始，其中包含傳統型應用程式的參考。

下圖顯示範例解決方案。

![延伸起始專案](images/desktop-to-uwp/extend-start-project.png)

如果您的解決方案不包含封裝專案，請參閱[您使用 Visual Studio 的傳統型應用程式套件](desktop-to-uwp-packaging-dot-net.md)。

### <a name="configure-the-desktop-application"></a>傳統型應用程式設定

請確定您的傳統型應用程式的檔案，您需要呼叫 Windows 執行階段 Api 的參考。

若要這樣做，請參閱主題[增強適用於 Windows 10 傳統型應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project)的 [[首先，設定您的專案](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project)] 區段。

### <a name="add-a-uwp-project"></a>新增 UWP 專案

新增 **\[空白應用程式 (通用 Windows)\]** 到您的解決方案。

您會在此組建現代化 XAML UI 或使用只在 UWP 處理程序中執行的 API。

![UWP 專案](images/desktop-to-uwp/add-uwp-project-to-solution.png)

在封裝專案的 **\[應用程式\]** 節點上按一下滑鼠右鍵，然後按一下 **\[加入參考\]**。

![參考 UWP 專案](images/desktop-to-uwp/add-uwp-project-reference.png)

然後，新增 UWP 專案的參考。

![參考 UWP 專案](images/desktop-to-uwp/choose-uwp-project.png)

您的解決方案看起來會像這樣：

![解決方案與 UWP 專案](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(選用) 新增 Windows 執行階段元件

若要完成一些案例，您必須將程式碼新增到 Windows 執行階段元件。

![執行階段元件應用程式服務](images/desktop-to-uwp/add-runtime-component.png)

然後，從您的 UWP 專案，新增執行階段元件的參考。 您的解決方案看起來會像這樣：

![執行階段參考](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>建置您的方案

建置您的解決方案，以確保未出現任何錯誤。 如果您收到錯誤，請開啟**Configuration Manager** ，並確保您的專案目標相同的平台。

![組態管理員](images/desktop-to-uwp/config-manager.png)

讓我們看看您可以使用 UWP 專案和執行階段元件做幾件事。

## <a name="show-a-modern-xaml-ui"></a>顯示現代化 XAML UI

做為應用程式流程的一部分，您可以將現代化 XAML 型使用者介面納入您的傳統型應用程式。 這些使用者介面會自然調整至不同的螢幕大小和解析度，並支援現代化互動模型，例如觸控和筆跡。

例如，只需少量的 XAML 標記，您就可以為使用者提供強大的地圖相關視覺效果功能。

此圖顯示一個 Windows Forms 應用程式，開啟包含地圖控制項的 XAML 型現代化 UI。

![調適型設計](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>這個範例會顯示在 XAML UI 藉由將 UWP 專案新增至方案。 這是傳統型應用程式中顯示 XAML Ui 穩定的支援的方法。 這個方法的替代方法是以 UWP XAML 控制項直接新增至您的傳統型應用程式使用 XAML 島。 目前提供做為開發人員預覽 XAML 群島。 雖然我們鼓勵您嘗試它們在您自己的原型程式碼現在，我們不建議您使用它們在實際執行程式碼中這一次。 這些 Api 和控制項將會繼續成熟和穩定在未來的 Windows 版本。 若要深入了解 XAML 群島，請參閱[傳統型應用程式中的 UWP 控制項](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="the-design-pattern"></a>設計模式

若要顯示 XAML 型 UI，請執行下列動作：

:one: [設定您的解決方案](#solution-setup)

:two: [建立 XAML UI](#xaml-UI)

:three: [新增通訊協定延伸模組至 UWP 專案](#protocol)

:four: [從您的傳統型應用程式啟動 UWP app](#start)

:five: [在 UWP 專案中，顯示您要的頁面](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>設定您的解決方案

如需如何設定您的解決方案的一般指導方針，請參閱本指開頭南的[首先，設定您的解決方案](#setup)一節。

您的解決方案看起來會像這樣：

![XAML UI 解決方案](images/desktop-to-uwp/xaml-ui-solution.png)

在此範例中，Windows Forms 專案名為**地標**和包含 XAML U 的 UWP 專案名為 **MapUI** XAML UI。

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>建立 XAML UI

新增 XAML UI 至您的 UWP 專案 以下是 XAML 的基本對應。

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"
                     HorizontalAlignment="Left"
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>新增通訊協定延伸模組

在 [**方案總管**] 中，開啟封裝專案的**package.appxmanifest**檔案在您的方案，並新增此延伸模組。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

為通訊協定命名，提供由 UWP 專案產生的可執行檔名稱，以及進入點類別的名稱。

您也可以在設計工具中開啟 **package.appxmanifest**、選擇 **\[宣告\]** 索引標籤，然後在那裡新增延伸模組。

![宣告索引標籤](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> 地圖控制項會從網際網路下載資料，所以如果您使用地圖控制項，則也必須將「網際網路用戶端」功能新增至您的資訊清單。

<a id="start" />

### <a name="start-the-uwp-app"></a>啟動 UWP 應用程式。

首先，從您的傳統型應用程式建立 [Uri](https://msdn.microsoft.com/library/system.uri.aspx)，包括通訊協定名稱以及您想要傳遞到 UWP 應用程式的任何參數。 然後，呼叫 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法。

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>剖析參數並顯示頁面

在 UWP 專案的 **App** 類別中，覆寫 **OnActivated** 事件處理常式。 如果應用程式是由您的通訊協定啟動，請剖析參數然後開啟您想要的頁面。

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

在程式碼後置 XAML 頁面中，覆寫``OnNavigatedTo``方法，使用參數傳遞到頁面。 在這種情形下，我們會使用傳遞到此頁面的緯度和經度，以在地圖中顯示位置。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

## <a name="making-your-desktop-application-a-share-target"></a>讓您的傳統型應用程式成為分享目標

您可以讓您的傳統型應用程式成為分享目標，讓使用者可以輕鬆地共用資料，例如來自支援共用的其他應用程式的圖片。

例如，使用者可以選擇您的應用程式，來從 Microsoft Edge、 [相片] app 分享圖片。 以下是具有該功能的 WPF 範例應用程式。

![分享目標](images/desktop-to-uwp/share-target.png).

請參閱完整的範例[以下](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>設計模式

若要讓您的應用程式成為分享目標，請進行下列項目：

:one: [新增分享目標擴充功能](#share-extension)

: two：[覆寫 OnShareTargetActivated 事件處理常式](#override)

: three：[新增桌面延伸到 UWP 專案](#desktop-extensions)

： 四：[新增完全信任的處理程序擴充功能](#full-trust)

： 五：[修改傳統型應用程式，以取得共用的檔案](#modify-desktop)

<a id="share-extension" />

下列步驟  

### <a name="add-a-share-target-extension"></a>新增分享目標擴充功能

在 [**方案總管]** 中，在您的方案中開啟封裝專案的**package.appxmanifest**檔案，並新增分享目標擴充功能。

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

提供由 UWP 專案產生的可執行檔名稱，以及進入點類別的名稱。 此標記會假設您的 UWP 應用程式的可執行檔的名稱是`ShareTarget.exe`。

您也需要指定您的應用程式可以分享哪些檔案類型。 在此範例中，我們正在進行的[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)傳統型應用程式成為分享目標的點陣圖影像，因此我們指定`Bitmap`做為支援的檔案類型。

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>覆寫 OnShareTargetActivated 事件處理常式

覆寫您的 UWP 專案的**應用程式**類別中的**OnShareTargetActivated**事件處理常式。

當使用者選擇您的應用程式來共用檔案時，就會呼叫這個事件處理常式。

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

在這個程式碼中，我們將儲存已被使用者分享到應用程式本機存放裝置資料夾的影像。 稍後，我們將會修改到提取映像的傳統型應用程式從該相同的資料夾。 傳統型應用程式可以這麼做，因為它包含做為 UWP 應用程式在相同套件中。

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>將桌面延伸新增到 UWP 專案

將**適用於 UWP 的 Windows 桌面延伸**延伸模組新增到 UWP app 專案。

![桌面延伸模組](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>新增完全信任的處理程序擴充功能

在 [**方案總管]** 中，開啟封裝專案的**package.appxmanifest**檔案，在您的方案，並再新增完全信任程序延伸模組旁邊您稍早新增此檔案分享目標擴充功能。

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

此延伸模組可讓 UWP 應用程式啟動到您想要共用檔案的傳統型應用程式。 在範例中，我們會參考的[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)傳統型應用程式的可執行檔。

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>修改傳統型應用程式，以取得共用的檔案

修改您傳統型應用程式來尋找並處理共用的檔案。 在此範例中，UWP 應用程式會儲存在本機應用程式資料資料夾中的共用的檔案。 因此，我們會修改[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)傳統型應用程式以提取相片從該資料夾。

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

使用者開啟的傳統型應用程式已經的執行個體，我們也可能會處理[FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher?view=netframework-4.7.2)事件，並將路徑中傳遞至檔案的位置。 如此一來任何開啟的執行個體的傳統型應用程式將會顯示共用的相片。

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>建立背景工作

新增背景工作，即使 app 已暫停時執行程式碼。 背景工作非常適合不需要使用者互動的小工作。 例如，您的工作可以下載電子郵件、顯示收到聊天訊息的快顯通知，或回應系統條件的變更。

以下是註冊背景工作的 WPF 範例應用程式。

![背景工作](images/desktop-to-uwp/sample-background-task.png)

工作提供 http 要求並測量傳回要求回應所需的時間。 您的工作可能更有趣，但此範例非常適合學習背景工作的入門技巧。

請參閱完整的範例[如下](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)。

### <a name="the-design-pattern"></a>設計模式

若要建立背景服務，請執行下列動作：

:one: [實作背景工作](#implement-task)

:two: [設定背景工作](#configure-background-task)

:three: [註冊背景工作](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>實作背景工作

將程式碼新增至加入 Windows 執行階段元件專案，實作背景工作。

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>設定背景工作

在資訊清單設計工具中，開啟解決方案中的封裝專案的**package.appxmanifest**檔案。

在**\[宣告\]** 索引標籤，新增**\[背景工作\]** 宣告。

![背景工作選項](images/desktop-to-uwp/background-task-option.png)

然後，選擇您想要的屬性。 我們的範例使用**\[計時器\]** 屬性。

![計時器屬性](images/desktop-to-uwp/timer-property.png)

在實作背景工作的 Windows 執行階段元件中提供類別的完整名稱。

![計時器屬性](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>註冊背景工作

在註冊背景工作的傳統型應用程式專案中加入程式碼。

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```

## <a name="support-and-feedback"></a>支援與意見反應

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
