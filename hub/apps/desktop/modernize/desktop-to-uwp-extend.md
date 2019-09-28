---
Description: 使用 Windows UI 和元件擴充您的傳統型應用程式
title: 使用 Windows UI 和元件擴充您的傳統型應用程式
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 7359d28d968a2948e9f4049e2acc3c655edcfcb3
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339208"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>使用現代化的 UWP 元件擴充您的桌面應用程式

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 如果您想要新增這些體驗，請使用 UWP 專案和 Windows 執行階段元件來擴充您的桌面應用程式。

在許多情況下，您可以直接從您的桌面應用程式呼叫 Windows 執行階段 Api，因此在閱讀本指南之前，請參閱[Windows 10 的增強](desktop-to-uwp-enhance.md)功能。

> [!NOTE]
> 本文中所述的功能需要您為桌面應用程式建立 Windows 應用程式套件。 如果您還沒有這麼做，請參閱[封裝桌面應用程式](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。

當您準備好時，我們就可以開始進行操作。

<a id="setup" />

## <a name="first-setup-your-solution"></a>首先，設定您的解決方案

新增一或多個 UWP 專案和執行階段元件至您的解決方案。

從包含 **Windows 應用程式封裝專案**的解決方案開始，其中包含傳統型應用程式的參考。

下圖顯示範例解決方案。

![延伸起始專案](images/desktop-to-uwp/extend-start-project.png)

如果您的解決方案不包含封裝專案，請參閱[使用 Visual Studio 封裝您的桌面應用程式](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。

### <a name="configure-the-desktop-application"></a>設定桌面應用程式

請確定您的桌面應用程式具有呼叫 Windows 執行階段 Api 所需的檔案參考。

若要這麼做，請參閱[設定您的專案](desktop-to-uwp-enhance.md#set-up-your-project)一節。

### <a name="add-a-uwp-project"></a>新增 UWP 專案

新增 **\[空白應用程式 (通用 Windows)\]** 到您的解決方案。

您會在此組建現代化 XAML UI 或使用只在 UWP 處理程序中執行的 API。

![UWP 專案](images/desktop-to-uwp/add-uwp-project-to-solution.png)

在封裝專案的 **\[應用程式\]** 節點上按一下滑鼠右鍵，然後按一下 **\[加入參考\]** 。

![參考 UWP 專案](images/desktop-to-uwp/add-uwp-project-reference.png)

然後，新增 UWP 專案的參考。

![參考 UWP 專案](images/desktop-to-uwp/choose-uwp-project.png)

您的解決方案看起來會像這樣：

![解決方案與 UWP 專案](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>選擇性新增 Windows 執行階段元件

若要完成某些案例，您必須將程式碼新增至 Windows 執行階段元件。

![執行階段元件應用程式服務](images/desktop-to-uwp/add-runtime-component.png)

然後，從您的 UWP 專案，新增執行階段元件的參考。 您的解決方案看起來會像這樣：

![執行階段參考](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>建立您的解決方案

建立您的解決方案，以確保不會出現任何錯誤。 如果您收到錯誤, 請開啟**Configuration Manager** , 並確定您的專案以相同的平臺為目標。

![Config manager](images/desktop-to-uwp/config-manager.png)

讓我們看看您可以使用 UWP 專案和執行階段元件做幾件事。

## <a name="show-a-modern-xaml-ui"></a>顯示現代化 XAML UI

做為應用程式流程的一部分，您可以將現代化 XAML 型使用者介面納入您的傳統型應用程式。 這些使用者介面會自然調整至不同的螢幕大小和解析度，並支援現代化互動模型，例如觸控和筆跡。

例如，只需少量的 XAML 標記，您就可以為使用者提供強大的地圖相關視覺效果功能。

此圖顯示一個 Windows Forms 應用程式，開啟包含地圖控制項的 XAML 型現代化 UI。

![調適型設計](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>這個範例會藉由將 UWP 專案新增至方案來顯示 XAML UI。 這是在桌面應用程式中顯示 XAML Ui 的穩定支援方法。 這種方法的替代方式是使用 XAML 島，將 UWP XAML 控制項直接新增至您的桌面應用程式。 XAML 島目前以開發人員預覽的形式提供。 雖然我們鼓勵您現在在自己的原型程式碼中試用它們，但我們不建議您此時在實際程式碼中使用它們。 在未來的 Windows 版本中，這些 Api 和控制項將會繼續成熟且穩定。 若要深入瞭解 XAML Islands，請參閱[桌面應用程式中的 UWP 控制項](xaml-islands.md)

### <a name="the-design-pattern"></a>設計模式

若要顯示 XAML 型 UI，請執行下列動作：

:one:[設定您的解決方案](#solution-setup)

:two:[建立 XAML UI](#xaml-UI)

:three:[將通訊協定擴充功能新增至 UWP 專案](#add-a-protocol-extension)

:four:[從您的桌面應用程式啟動 UWP 應用程式](#start)

:five:[在 UWP 專案中，顯示您想要的頁面](#parse)

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

在**方案總管**中，開啟方案中封裝專案的**package.appxmanifest.xml**檔案，並新增此延伸模組。

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

首先，從您的傳統型應用程式建立 [Uri](https://docs.microsoft.com/dotnet/api/system.uri)，包括通訊協定名稱以及您想要傳遞到 UWP 應用程式的任何參數。 然後，呼叫 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法。

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

在 XAML 的後置程式字碼頁面中，覆寫 ``OnNavigatedTo`` 方法，以使用傳入頁面的參數。 在這種情形下，我們會使用傳遞到此頁面的緯度和經度，以在地圖中顯示位置。

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

例如，使用者可以選擇您的應用程式，從 Microsoft Edge （相片應用程式）共用圖片。 以下是具有該功能的 WPF 範例應用程式。

![分享目標](images/desktop-to-uwp/share-target.png).

請參閱[這裡](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)的完整範例

### <a name="the-design-pattern"></a>設計模式

若要讓您的應用程式成為分享目標，請進行下列項目：

:one:[新增共用目標延伸模組](#share-extension)

:two:[覆寫 OnShareTargetActivated 事件處理常式](#override)

:three:[將桌面擴充功能新增至 UWP 專案](#desktop-extensions)

:four:[新增完全信任進程延伸模組](#full-trust)

:five:[修改桌面應用程式以取得共用檔案](#modify-desktop)

<a id="share-extension" />

下列步驟  

### <a name="add-a-share-target-extension"></a>新增分享目標擴充功能

在**方案總管**中，開啟方案中封裝專案的**package.appxmanifest.xml**檔案，並新增共用目標延伸模組。

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

提供由 UWP 專案產生的可執行檔名稱，以及進入點類別的名稱。 此標記假設 UWP 應用程式的可執行檔名稱為 `ShareTarget.exe`。

您也需要指定您的應用程式可以分享哪些檔案類型。 在此範例中，我們會將[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)桌面應用程式設為點陣圖影像的共用目標，以便為支援的檔案類型指定 `Bitmap`。

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>覆寫 OnShareTargetActivated 事件處理常式

覆寫 UWP 專案之**App**類別中的**OnShareTargetActivated**事件處理常式。

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

在此程式碼中，我們會將使用者所共用的影像儲存到應用程式的本機儲存體資料夾。 稍後，我們將修改桌面應用程式，以從相同的資料夾提取影像。 桌面應用程式可以這麼做，因為它包含在與 UWP 應用程式相同的套件中。

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>將桌面擴充功能新增至 UWP 專案

將 UWP 擴充功能的**Windows 桌面延伸**模組新增至 uwp 應用程式專案。

![桌面延伸模組](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>新增完全信任進程延伸模組

在**方案總管**中，開啟方案中封裝專案的**package.appxmanifest.xml**檔案，然後在您稍早加入此檔案的共用目標延伸模組旁新增完全信任進程延伸模組。

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

此延伸模組可讓 UWP 應用程式啟動您想要共用檔案的桌面應用程式。 例如，我們指的是[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) desktop 應用程式的可執行檔。

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>修改桌面應用程式以取得共用檔案

修改您的桌面應用程式，以尋找及處理共用檔案。 在此範例中，UWP 應用程式會將共用檔案儲存在本機應用程式的 data 資料夾中。 因此，我們會修改[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) desktop 應用程式，以從該資料夾提取相片。

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

對於已由使用者開啟的桌面應用程式實例，我們也可能會處理[FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher)事件，並傳入檔案位置的路徑。 如此一來，任何開啟的桌面應用程式實例都會顯示共用相片。

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

請參閱[這裡](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)的完整範例。

### <a name="the-design-pattern"></a>設計模式

若要建立背景服務，請執行下列動作：

:one:[執行背景工作](#implement-task)

:two:[設定背景工作](#configure-background-task)

:three:[註冊背景工作](#register-background-task)

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

在 [資訊清單設計工具] 中，開啟方案中封裝專案的**package.appxmanifest.xml**檔案。

在 **\[宣告\]** 索引標籤，新增 **\[背景工作\]** 宣告。

![背景工作選項](images/desktop-to-uwp/background-task-option.png)

然後，選擇您想要的屬性。 我們的範例使用 **\[計時器\]** 屬性。

![計時器屬性](images/desktop-to-uwp/timer-property.png)

在執行背景工作的 Windows 執行階段元件中，提供類別的完整名稱。

![計時器屬性](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>登錄背景工作

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

**尋找您問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
