---
title: 使用自動播放功能來自動啟動
description: 當使用者將裝置連接至電腦時，您可以使用「自動播放」，讓您的 app 成為一個選項。 這些裝置包含非磁碟區型裝置 (例如相機或媒體播放裝置) 或磁碟區型裝置 (例如 USB 隨身碟、SD 記憶卡或 DVD)。
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 714da78a8860eec92bce9389185f52a58e45b44e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7974169"
---
# <a name="span-iddevlaunchresumeauto-launchingwithautoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>使用自動播放功能來自動啟動

當使用者將裝置連接至電腦時，您可以使用**自動播放**，讓您的應用程式成為一個選項。 這些裝置包含非磁碟區型裝置 (例如相機或媒體播放裝置) 或磁碟區型裝置 (例如 USB 隨身碟、SD 記憶卡或 DVD)。 當使用者使用透過近接 (輕觸) 方式在兩部電腦之間分享檔案時，您也可以使用 **「自動播放」**，讓您的 app 成為一個選項。

> **注意：** 如果您是裝置製造商，而且您想要將您的[Microsoft Store 裝置應用程式](http://go.microsoft.com/fwlink/p/?LinkID=301381)做為您的裝置的 **「 自動播放 」** 處理常式產生關聯，您可以找出該應用程式在裝置中繼資料中的。 如需詳細資訊，請參閱 [Microsoft Store 裝置應用程式的自動播放](http://go.microsoft.com/fwlink/p/?LinkId=306684)。

## <a name="register-for-autoplay-content"></a>登錄自動播放內容

您可以登錄 app 做為 **「自動播放」** 內容事件的選項。 將磁碟區裝置 (例如，相機記憶卡、隨身碟或 DVD) 插入電腦時，就會引發 **「自動播放」** 內容事件。 我們在此處示範如何讓您的 app 在插入相機的磁碟區裝置時成為 **「自動播放」** 選項。

在這個教學課程中，您建立了一個可顯示影像檔或將影像檔複製到 [圖片] 中的應用程式。 您針對「自動播放」**ShowPicturesOnArrival** 內容事件登錄 app。

「自動播放」也會在使用近接感測 (輕觸) 的電腦間，針對分享的內容引發內容事件。 您可以使用這個小節中的步驟及程式碼，處理使用鄰近性功能的電腦間所分享的檔案。 下表列出可利用近接感測來分享內容的自動播放內容事件。

| 動作         | 自動播放內容事件  |
|----------------|-------------------------|
| 分享音樂  | PlayMusicFilesOnArrival |
| 分享視訊 | PlayVideoFilesOnArrival |

 
使用近接感測分享檔案時，**FileActivatedEventArgs** 物件的 **Files** 屬性會包含根資料夾的參照，這個參照包含所有已分享的檔案。

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>步驟 1：建立新專案以及新增自動播放宣告

1.  開啟 Microsoft Visual Studio，然後選取 **\[檔案\]** 功能表的 **\[新增專案\]**。 在 **Visual C#** 區段中，在 **Windows** 下，選取 **\[空白的 App (通用 Windows)\]**。 將 app 命名為 **AutoPlayDisplayOrCopyImages**，然後按一下 **\[確定\]**。
2.  開啟 Package.appxmanifest 檔案，然後選取 \[功能\]**** 索引標籤。選取 \[抽取式存放裝置\]**** 與 [圖片媒體庫\]**** 功能。 這樣 app 就可以存取相機記憶體的抽取式存放裝置，以及存取本機圖片。
3.  在資訊清單檔案中，選取 \[宣告\]**** 索引標籤。在 \[可用宣告\]**** 下拉式清單中選取 \[檔案類型關聯\]****，然後按一下 \[自動播放內容\]****。 選取已經新增至 **\[支援的宣告\]** 清單中的新 **\[自動播放內容\]** 項目。
4.  當自動播放引發內容事件時，**\[自動播放內容\]** 宣告可以將您的 app 識別為選項。 事件是以磁碟區裝置 (如 DVD 或隨身碟) 的內容為基礎。 自動播放會檢查磁碟區裝置的內容，然後決定要引發的內容事件。 如果磁碟區根目錄包含 DCIM、AVCHD 或 PRIVATE\\ACHD 資料夾，或如果使用者已在 \[自動播放\] 控制台中啟用 **\[選擇要對每種媒體類型執行的動作\]** 而且系統在磁碟區的根目錄中找到圖片，則自動播放就會引發 **ShowPicturesOnArrival** 事件。 在 **\[啟動動作\]** 區段中，為第一個啟動動作輸入來自下面表 1 的值。
5.  在 **\[自動播放內容\]** 項目的 **\[啟動動作\]** 區段中，按一下 **\[加入新的\]**，以加入第二個啟動動作。 為第二個啟動動作輸入下面表 2 的值。
6.  在 **\[可用宣告\]** 下拉式清單中，選取 **\[檔案類型關聯\]**，然後按一下 **\[加入\]**。 在新 **\[檔案類型關聯\]** 宣告的 \[屬性\] 中，將 **\[顯示名稱\]** 欄位設定成 **AutoPlay Copy or Show Images**，並將 **\[名稱\]** 欄位設定成 **image\_association1**。 在 **\[支援的檔案類型\]** 區段中，按一下 **\[加入新的\]**。 將 **\[檔案類型\]** 欄位設定成 **.jpg**。 在 **\[支援的檔案類型\]** 區段中，將新檔案關聯的 **\[檔案類型\]** 欄位設定成 **.png**。 對於內容事件，自動播放會篩選掉未明確地與您 app 關聯的所有檔案類型。
7.  儲存並關閉資訊清單檔案。

**表 1**

| 設定             | 值                 |
|---------------------|-----------------------|
| 動詞                | 顯示                  |
| 動作顯示名稱 | 顯示圖片         |
| 內容事件       | ShowPicturesOnArrival |

**\[動作顯示名稱\]** 設定會識別「自動播放」為您的 app 所顯示的字串。 **\[動詞\]** 設定會識別針對選取的選項而傳遞至您 app 的值。 您可以為自動播放事件指定多個啟動動作，並使用 **\[動詞\]** 設定判斷使用者為您 app 選取的選項。 您可以檢查傳遞至 app 啟動事件引數的 **verb** 屬性，以判斷使用者選取的選項。 您可以在 **\[動詞\]** 設定使用保留字 **open** 以外的任何值。

**表 2**  

| 設定             | 值                      |
|--------------------:|----------------------------|
| 動詞                | 複製                       |
| 動作顯示名稱 | 複製圖片到媒體櫃 |
| 內容事件       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>步驟 2：新增 XAML UI

開啟 MainPage.xaml 檔案，將下列 XAML 新增至預設的 &lt;Grid&gt; 區段。

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>步驟 3：新增初始化程式碼

這個步驟中的程式碼會檢查 **Verb** 屬性中的動詞值，這是發生 **OnFileActivated** 事件時傳遞至 app 的其中一個啟動引數。 程式碼接著會呼叫與使用者選取之選項相關的方法。 對於相機記憶體事件，自動播放會將相機存放裝置的根資料夾傳遞至 app。 您可以從 **Files** 屬性的第一個元素擷取這個資料夾。

開啟 App.xaml.cs 檔案，然後將下列程式碼新增到 **App** 類別。

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **注意：**`DisplayImages`和`CopyImages`方法會在下列步驟新增。

### <a name="step-4-add-code-to-display-images"></a>步驟 4：新增程式碼以顯示影像

在 MainPage.xaml.cs 檔案中，將下列程式碼加到 **MainPage** 類別。

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### <a name="step-5-add-code-to-copy-images"></a>步驟 5：新增程式碼以複製影像

在 MainPage.xaml.cs 檔案中，將下列程式碼加到 **MainPage** 類別。

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### <a name="step-6-build-and-run-the-app"></a>步驟 6：建置並執行 app

1.  按 F5 以建置和部署 app (偵錯模式)。
2.  若要執行 app，請將相機中的相機記憶卡或其他存放裝置插入電腦。 然後從自動播放選項清單中選取您在 package.appxmanifest 檔案中指定的其中一個內容事件選項。 這個範例程式碼只會顯示或複製相機記憶卡之 [DCIM] 資料夾中的相片。 如果您的相機記憶卡將相片儲存在 [AVCHD] 或 [PRIVATE\\ACHD] 資料夾中，您必須隨之更新程式碼。
    **注意：** 如果您沒有相機記憶卡，您可以使用快閃磁碟機是否有一個名為根目錄中的 **[dcim]** 資料夾，如果 [dcim] 資料夾的子資料夾，其中包含映像。

## <a name="register-for-an-autoplay-device"></a>登錄自動播放裝置


您可以登錄 app 做為 **「自動播放」** 裝置事件的選項。 將裝置連接到電腦時，就會引發 **「自動播放」** 裝置事件。

我們將在此處示範如何在將相機連接到電腦時，將您的 app 識別為 **\[自動播放\]** 選項。 該 app 會登錄為 **WPD\\ImageSourceAutoPlay** 事件的處理常式。 當相機及其他影像裝置通知 Windows 可攜式裝置 (WPD) 系統它們是使用 MTP 的 ImageSource 時，該系統常會引發這個事件。 如需詳細資訊，請參閱 [Windows 可攜式裝置](https://msdn.microsoft.com/library/windows/hardware/ff597729)。

**重要** [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) Api 是[桌面裝置系列](https://msdn.microsoft.com/library/windows/apps/dn894631)的一部分。 應用程式可以使用這些 Api 只會在傳統型裝置系列，例如電腦中的 windows 10 裝置。

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>步驟 1：建立新專案以及新增自動播放宣告

1.  開啟 Visual Studio，然後選取 **\[檔案\]** 功能表的 **\[新增專案\]**。 在 **Visual C#** 區段中，在 **Windows** 下，選取 **\[空白的 App (通用 Windows)\]**。 將 app 命名為 **AutoPlayDevice\_Camera**，然後按一下 **\[確定\]**。
2.  開啟 Package.appxmanifest 檔案，然後選取 \[功能\]**** 索引標籤。選取 \[抽取式存放裝置\]**** 功能。 這可讓 app 存取做為卸除式存放磁碟區裝置的相機中的資料。
3.  在資訊清單檔案中，選取 \[宣告\]**** 索引標籤。在 \[可用宣告\]**** 下拉式清單中，選取 \[自動播放裝置\]****，然後按一下 \[新增\]****。 選取已經新增至 **\[支援的宣告\]** 清單中的新 **\[自動播放裝置\]** 項目。
4.  當自動播放引發已知事件的裝置事件時，**\[自動播放裝置\]** 宣告可以將您的 app 識別為選項。 在 **\[啟動動作\]** 區段中，為第一個啟動動作輸入下表的值。
5.  在 **\[可用宣告\]** 下拉式清單中，選取 **\[檔案類型關聯\]**，然後按一下 **\[加入\]**。 在新 **\[檔案類型關聯\]** 宣告的 \[屬性\] 中，將 **\[顯示名稱\]** 欄位設定成 **Show Images from Camera**，並將 **\[名稱\]** 欄位設定成 **camera\_association1**。 在 **\[支援的檔案類型\]** 區段中，如果有需要請按一下 **\[加入新的\]**。 將 **\[檔案類型\]** 欄位設定成 **.jpg**。 在 **\[支援的檔案類型\]** 區段中，再次按一下 **\[加入新的\]**。 將新檔案關聯的 **\[檔案類型\]** 欄位設定成 **.png**。 對於內容事件，自動播放會篩選掉未明確地與您 app 關聯的所有檔案類型。
6.  儲存並關閉資訊清單檔案。

| 設定             | 值            |
|---------------------|------------------|
| 動詞                | 顯示             |
| 動作顯示名稱 | 顯示圖片    |
| 內容事件       | WPD\\ImageSource |

**\[動作顯示名稱\]** 設定會識別「自動播放」為您的 app 所顯示的字串。 **\[動詞\]** 設定會識別針對選取的選項而傳遞至您 app 的值。 您可以為自動播放事件指定多個啟動動作，並使用 **\[動詞\]** 設定判斷使用者為您 app 選取的選項。 您可以檢查傳遞至 app 啟動事件引數的 **verb** 屬性，以判斷使用者選取的選項。 您可以在 **\[動詞\]** 設定使用保留字 **open** 以外的任何值。 如需在單一 app 中使用多個動詞的範例，請參閱[登錄自動播放內容](#register-for-autoplay-content)。

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>步驟 2：新增桌面延伸功能的組件參考

存取 Windows 可攜式裝置上存放區所需的 API ([**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654)) 是[傳統型裝置系列](https://msdn.microsoft.com/library/windows/apps/dn894631)的一部分。 這表示必須要有特殊的組件才能使用 API，且那些呼叫只會在傳統型裝置系列 (例如電腦) 的裝置上運作。

1.  在 **\[方案總管\]** 中，在 **\[參照\]** 上按一下滑鼠右鍵，然後按一下 **\[加入參考\]**。
2.  展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。
3.  然後選取 **\[UWP 的 Windows 桌面延伸\]** 並按一下 **\[確定\]**。

### <a name="step-3-add-xaml-ui"></a>步驟 3：新增 XAML UI

開啟 MainPage.xaml 檔案，將下列 XAML 新增至預設的 &lt;Grid&gt; 區段。

```xml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>步驟 4：新增啟用程式碼

這個步驟中的程式碼透過將相機的裝置資訊識別碼傳遞至 [**FromId**](https://msdn.microsoft.com/library/windows/apps/br225655) 方法，以將相機參考為 [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654)。 相機的裝置資訊識別碼的取得方式，是先將事件引數轉換為 [**DeviceActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224710)，然後再從 [**DeviceInformationId**](https://msdn.microsoft.com/library/windows/apps/br224711) 屬性取得值。

開啟 App.xaml.cs 檔案，然後將下列程式碼新增到 **App** 類別。

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **注意：**`ShowImages`方法會在下列步驟新增。

### <a name="step-5-add-code-to-display-device-information"></a>步驟 5：新增程式碼以顯示裝置資訊

您可以從 [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) 類別的屬性取得相機的相關資訊。 這個步驟中的程式碼會在 app 執行時，向使用者顯示裝置名稱和其他資訊。 程式碼會接著呼叫您將在下一個步驟中新增的 GetImageList 和 GetThumbnail 方法，以顯示相機中所儲存影像的縮圖。

在 MainPage.xaml.cs 檔案中，將下列程式碼加到 **MainPage** 類別。

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **注意：**`GetImageList`和`GetThumbnail`方法會在下列步驟新增。

### <a name="step-6-add-code-to-display-images"></a>步驟 6：新增程式碼以顯示影像

這個步驟中的程式碼會顯示相機中所儲存影像的縮圖。 程式碼會對相機進行非同步呼叫，以取得縮圖影像。 不過，在上一個非同步呼叫完成之前，將不會進行下一個非同步呼叫。 這可確保一次只對相機提出一個要求。

在 MainPage.xaml.cs 檔案中，將下列程式碼加到 **MainPage** 類別。

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder)
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img)
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### <a name="step-7-build-and-run-the-app"></a>步驟 7：建置並執行 app

1.  按 F5 以建置和部署 app (偵錯模式)。
2.  若要執行您的 app，請將相機連接到您的電腦。 然後從自動播放選項清單中選取 app。
    **注意：** 並非所有相機都都會針對**WPD\\ImageSource**自動播放裝置事件。

## <a name="configure-removable-storage"></a>設定卸除式存放裝置

將磁碟區裝置 (例如記憶卡或隨身碟) 連接到電腦時，您可以將它識別為 **「自動播放」** 裝置。 當您想為磁碟區裝置建立特定的 **「自動播放」** app 關聯以提供給使用者時，這項功能格外有用。

我們將在此處示範如何將磁碟區裝置識別為 **「自動播放」** 裝置。

若要將磁碟區裝置識別為 **「自動播放」** 裝置，請在裝置的根磁碟機新增一個 autorun.inf 檔案。 在 autorun.inf 檔案中，將 **CustomEvent** 機碼新增到 **\[AutoRun\]** 區段。 當您的磁碟區裝置連接到電腦時，**「自動播放」** 將尋找 autorun.inf 檔案，並將您的磁碟區視為裝置。 **「自動播放」** 將使用您為 **CustomEvent** 機碼提供的名稱來建立 **「自動播放」** 事件。 您可以接著建立一個 app，然後將該 app 登錄為 **「自動播放」** 事件的處理常式。 當裝置連接到電腦時，**「自動播放」** 會將您的 app 顯示為磁碟區裝置的處理常式。 如需 autorun.inf 檔案的詳細資訊，請參閱 [autorun.inf 項目](https://msdn.microsoft.com/library/windows/desktop/cc144200)。

### <a name="step-1-create-an-autoruninf-file"></a>步驟 1：建立 autorun.inf 檔案

在磁碟區裝置的根磁碟機中，新增一個名為 autorun.inf 的檔案。 開啟 autorun.inf 檔案，然後新增下列文字。

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>步驟 2：建立新專案以及新增自動播放宣告

1.  開啟 Visual Studio，然後選取 **\[檔案\]** 功能表的 **\[新增專案\]**。 在 **Visual C#** 區段中，在 **Windows** 下，選取 **\[空白的 App (通用 Windows)\]**。 將應用程式命名為 **AutoPlayCustomEvent**，然後按一下 **\[確定\]**。
2.  開啟 Package.appxmanifest 檔案，然後選取 \[功能\]**** 索引標籤。選取 \[抽取式存放裝置\]**** 功能。 這可讓 app 存取抽取式存放裝置上的檔案與資料夾。
3.  在資訊清單檔案中，選取 \[宣告\]**** 索引標籤。在 \[可用宣告\]**** 下拉式清單中選取 \[檔案類型關聯\]****，然後按一下 \[自動播放內容\]****。 選取已經新增至 **\[支援的宣告\]** 清單中的新 **\[自動播放內容\]** 項目。

    **注意：** 或者，您也可以選擇新增**自動播放裝置**宣告為您自訂的 「 自動播放 」 事件。  
4.  在您的 **\[自動播放內容\]** 事件宣告的 **\[啟動動作\]** 區段中，為第一個啟動動作輸入下表中的值。
5.  在 **\[可用宣告\]** 下拉式清單中，選取 **\[檔案類型關聯\]**，然後按一下 **\[加入\]**。 在新 **\[檔案類型關聯\]** 宣告的 \[屬性\] 中，將 **\[顯示名稱\]** 欄位設定成 **Show .ms Files**，並將 **\[名稱\]** 欄位設定成 **ms\_association**。 在 **\[支援的檔案類型\]** 區段中，按一下 **\[加入新的\]**。 將 **\[檔案類型\]** 欄位設定成 **.ms**。 對於內容事件，「自動播放」會過濾掉未明確與您的 app 關聯的所有檔案類型。
6.  儲存並關閉資訊清單檔案。

| 設定             | 值                         |
|---------------------|-------------------------------|
| 動詞                | 顯示                          |
| 動作顯示名稱 | 顯示檔案                    |
| 內容事件       | AutoPlayCustomEventQuickstart |

**\[內容事件\]** 值是您在 autorun.inf 檔案中為 **CustomEvent** 機碼提供的文字。 **\[動作顯示名稱\]** 設定會識別「自動播放」為您的 app 所顯示的字串。 **\[動詞\]** 設定會識別針對選取的選項而傳遞至您 app 的值。 您可以為自動播放事件指定多個啟動動作，並使用 **\[動詞\]** 設定判斷使用者為您 app 選取的選項。 您可以檢查傳遞至 app 啟動事件引數的 **verb** 屬性，以判斷使用者選取的選項。 您可以在 **\[動詞\]** 設定使用保留字 **open** 以外的任何值。

### <a name="step-3-add-xaml-ui"></a>步驟 3：新增 XAML UI

開啟 MainPage.xaml 檔案，將下列 XAML 新增至預設的 &lt;Grid&gt; 區段。

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>步驟 4：新增啟用程式碼

這個步驟中的程式碼會呼叫一個方法，以顯示磁碟區裝置的根磁碟機中的資料夾。 對於自動播放內容事件，自動播放會傳遞在 **OnFileActivated** 事件期間傳遞至應用程式啟動引數中的存放裝置根資料夾。 您可以從 **Files** 屬性的第一個元素擷取這個資料夾。

開啟 App.xaml.cs 檔案，然後將下列程式碼新增到 **App** 類別。

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **注意：**`DisplayFiles`方法會在下列步驟新增。

 

### <a name="step-5-add-code-to-display-folders"></a>步驟 5：新增程式碼以顯示資料夾

在 MainPage.xaml.cs 檔案中，將下列程式碼加到 **MainPage** 類別。

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>>
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### <a name="step-6-build-and-run-the-qpp"></a>步驟 6：建置並執行 app

1.  按 F5 以建置和部署 app (偵錯模式)。
2.  若要執行 app，請將記憶卡或其他存放裝置插入電腦中。 然後從「自動播放」處理常式選項清單中選取您的 app。

## <a name="autoplay-event-reference"></a>「 自動播放 」 事件參照


**「自動播放」** 系統允許 app 登錄多種裝置和磁碟區 (磁碟) 連接事件。 若要登錄 **「自動播放」** 內容事件，您必須在套件資訊清單中啟用 **\[抽取式存放裝置\]** 功能。 下表顯示您可以登錄的事件以及事件的引發時機。

| 案例                                                           | 事件                              | 說明   |
|--------------------------------------------------------------------|------------------------------------|---------------|
| 使用相機上的相片                                           | **WPD\ImageSource**                | 如果相機被識別為 Windows 可攜式裝置且提供 ImageSource 功能，則引發事件。 |
| 使用音訊播放器上的音樂                                     | **WPD\AudioSource**                | 如果媒體播放器被識別為 Windows 可攜式裝置且提供 AudioSource 功能，則引發事件。 |
| 使用相機上的視訊                                     | **WPD\VideoSource**                | 如果相機被識別為 Windows 可攜式裝置且提供 VideoSource 功能，則引發事件。 |
| 存取已連接的快閃磁碟機或外接式硬碟              | **StorageOnArrival**               | 磁碟機或磁碟區連接到電腦後引發事件。   如果磁碟機或磁碟區在磁碟的根目錄有 [DCIM]、[AVCHD] 或 [PRIVATE\ACHD] 資料夾，則會引發 **ShowPicturesOnArrival** 事件來代替。 |
| 使用大型存放裝置 (傳統裝置) 的相片                            | **ShowPicturesOnArrival**          | 當磁碟機或磁碟區在磁碟的根目錄有 [DCIM]、[AVCHD] 或 [PRIVATE\ACHD] 資料夾時，就會引發事件。 如果使用者已經在 [自動播放] 控制台中啟用 **[選擇要對每種媒體類型執行的動作]**，則「自動播放」會檢查連接至電腦的磁碟區，以判斷磁碟上的內容類型。 找到相片時，會引發 **ShowPicturesOnArrival**。 |
| 使用近接感測分享 (輕觸並傳送) 方式接收相片             | **ShowPicturesOnArrival**          | 當使用者利用近接感測 (輕觸並傳送) 功能傳送內容後，自動播放會檢查分享的檔案以判斷內容的類型。 找到相片時，會引發 **ShowPicturesOnArrival**。 |
| 使用大型存放裝置 (傳統裝置) 上的音樂                             | **PlayMusicFilesOnArrival**        | 如果使用者已經在 \[自動播放\] 控制台中啟用 **\[選擇要對每種媒體類型執行的動作\]**，則「自動播放」會檢查連接至電腦的磁碟區，以判斷磁碟上的內容類型。  找到音樂檔案時，會引發 **PlayMusicFilesOnArrival**。 |
| 使用近接感測分享 (輕觸並傳送) 方式接收音樂。              | **PlayMusicFilesOnArrival**        | 當使用者利用近接感測 (輕觸並傳送) 功能傳送內容後，自動播放會檢查分享的檔案以判斷內容的類型。 找到音樂檔案時，會引發 **PlayMusicFilesOnArrival**。 |
| 使用大型存放裝置 (傳統裝置) 上的影片                            | **PlayVideoFilesOnArrival**        | 如果使用者已經在 \[自動播放\] 控制台中啟用 **\[選擇要對每種媒體類型執行的動作\]**，則「自動播放」會檢查連接至電腦的磁碟區，以判斷磁碟上的內容類型。 找到影片檔案時，會引發 **PlayVideoFilesOnArrival**。 |
| 使用近接感測分享 (輕觸並傳送) 方式接收影片             | **PlayVideoFilesOnArrival**        | 當使用者利用近接感測 (輕觸並傳送) 功能傳送內容後，自動播放會檢查分享的檔案以判斷內容的類型。 找到影片檔案時，會引發 **PlayVideoFilesOnArrival**。 |
| 處理來自已連接裝置的混合類型檔案               | **MixedContentOnArrival**          | 如果使用者已經在 \[自動播放\] 控制台中啟用 **\[選擇要對每種媒體類型執行的動作\]**，則「自動播放」會檢查連接至電腦的磁碟區，以判斷磁碟上的內容類型。 如果找不到特定的內容類型 (例如，相片)，會引發 **MixedContentOnArrival**。 |
| 處理以近接感測分享 (輕觸並傳送) 方式傳送的混合類型檔案 | **MixedContentOnArrival**          | 當使用者利用近接感測 (輕觸並傳送) 功能傳送內容後，自動播放會檢查分享的檔案以判斷內容的類型。 如果找不到特定的內容類型 (例如，相片)，會引發 **MixedContentOnArrival**。 |
| 處理光學媒體的視訊                                    | **PlayDVDMovieOnArrival**<br />**PlayBluRayOnArrival**<br />**PlayVideoCDMovieOnArrival**<br />**PlaySuperVideoCDMovieOnArrival** | 當磁碟插入光碟機時，自動播放會檢查檔案以判斷其類型。 找到視訊檔案時，便會引發與光碟機類型對應的事件。 |
| 處理光學媒體的音樂                                    | **PlayCDAudioOnArrival**<br />**PlayDVDAudioOnArrival** | 當磁碟插入光碟機時，自動播放會檢查檔案以判斷其類型。 找到音樂檔案時，便會引發與光碟機類型對應的事件。 |
| 播放增強型磁碟                                                | **PlayEnhancedCDOnArrival**<br />**PlayEnhancedDVDOnArrival** | 當磁碟插入光碟機時，自動播放會檢查檔案以判斷其類型。 找到增強的磁碟時，便會引發與光碟機類型對應的事件。 |
| 處理可燒錄的光碟片                                     | **HandleCDBurningOnArrival** <br />**HandleDVDBurningOnArrival** <br />**HandleBDBurningOnArrival** | 當磁碟插入光碟機時，自動播放會檢查檔案以判斷其類型。 找到可寫入的磁碟時，便會引發與光碟機類型對應的事件。 |
| 處理任何其他裝置或磁碟區連線                       | **UnknownContentOnArrival**        | 萬一找到的內容不符合任何自動播放內容類型時，則為所有事件引發。 不建議使用這個事件。 您只能為應用程式註冊它可以處理的相關「自動播放」事件。 |

您可以在磁碟區的 autorun.inf 檔案中使用 **CustomEvent** 項目，指定「自動播放」引發自訂的「自動播放」內容事件。 如需詳細資訊，請參閱 [Autorun.inf 項目](https://msdn.microsoft.com/library/windows/desktop/cc144200)。

您可以針對應用程式的 package.appxmanifest 檔案新增延伸，將應用程式登錄為「自動播放內容」或「自動播放裝置」事件處理常式。 如果您使用的是 Visual Studio，您可以在 \[宣告\]**** 索引標籤中新增 \[自動播放內容\]**** 或 \[自動播放裝置\]**** 宣告。如果您正直接編輯應用程式的 package.appxmanifest 檔案，請將[**延伸模組**](https://msdn.microsoft.com/library/windows/apps/br211400)元素新增至您的套件資訊清單，將 \[windows.autoPlayContent\]**** 或 \[windows.autoPlayDevice\]**** 指定為 \[類別\]****。 例如，套件資訊清單中的下列項目可新增 **「自動播放內容」** 延伸，以便將 app 登錄為 **ShowPicturesOnArrival** 事件的處理常式。

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures"
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 
