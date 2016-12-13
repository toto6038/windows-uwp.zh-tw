---
author: TylerMSFT
title: "建立和取用 App 服務"
description: "了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。"
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: "app 間通訊, 處理序間通訊, IPC, 背景傳訊, 背景通訊, app 到 app"
translationtype: Human Translation
ms.sourcegitcommit: fadfab2f03d5cfda46d5c9f29c28ad561e6ab2db
ms.openlocfilehash: 81786f6bf76d1d3840d5cd8c6191550b98a248b2

---

# <a name="create-and-consume-an-app-service"></a>建立和取用 App 服務


\[ 針對 Windows&nbsp;10 上的 UWP app 更新。 如需 Windows&nbsp;8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。

從 Windows&nbsp;10 版本 1607 開始，您便可以建立與主控 App 在相同處理程序中執行的應用程式服務。 本文重點為建立在個別處理序中執行的應用程式服務。 如需有關與提供者在相同處理序中執行之應用程式服務的詳細資訊，請參閱[轉換 App 服務，以便與其主控 App 在相同處理序中執行](convert-app-service-in-process.md)。

## <a name="create-a-new-app-service-provider-project"></a>建立新的應用程式服務提供者專案

在此做法中，為了簡單起見，我們將在一個方案中建立所有項目。

-   在 Microsoft Visual Studio 2015 中，建立新的 UWP App 專案，並命名為 AppServiceProvider。 (在 [新增專案] 對話方塊中，選取 [範本] &gt; [其他語言] &gt; [Visual C#] &gt; [Windows] &gt; [Windows 通用] &gt; [空白 App (Windows 通用)])。 這會是提供 App 服務的 App。

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>將 App 服務延伸模組新增到 package.appxmanifest

在 AppServiceProvider 專案的 Package.appxmanifest 檔案中，將下列 AppService 延伸模組新增到 **&lt;Application&gt;** 元素。 此範例會公告 `com.Microsoft.Inventory` 服務，以及將此 App 識別為 App 服務提供者的項目。 實際的服務將實作為背景工作。 App 服務 App 會將服務公開至其他 App。 我們建議針對服務名稱使用反向網域名稱樣式。

``` syntax
...
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

**Category** 屬性會將此 app 識別為 app 服務提供者。

**EntryPoint** 屬性會識別實作服務的類別，我們將在下一步驟中實作此類別。

## <a name="create-the-app-service"></a>建立 app 服務

1.  App 服務將實作為背景工作。 這讓前景應用程式能夠叫用另一個應用程式中的 App 服務來執行背景的工作。 將新的 Windows 執行階段元件專案 (命名為 MyAppService) 新增到方案 ([檔案] &gt; [新增] &gt; [新增專案]) (在 [加入新的專案] 對話方塊中，選擇 [已安裝] &gt; [其他語言] &gt; [Visual C#] &gt; [Windows] &gt; [Windows 通用] &gt; [Windows 執行階段元件 (Windows 通用)])
2.  在 AppServiceProvider 專案中，新增 MyAppService 專案的參考。
3.  在 MyappService 專案中，將下列 **using** 陳述式新增到 Class1.cs 頂端：
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  使用名為 **Inventory** 的新背景工作類別，來取代 **Class1** 的虛設常式程式碼：

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    這個類別是 app 服務將完成其工作的位置。

    建立背景工作時會呼叫 **Run()**。 因為背景工作會在 **Run** 完成之後終止，所以程式碼會延遲，讓背景工作能夠持續服務要求。

    當工作取消時會呼叫 **OnTaskCanceled()**。 當用戶端 app 處置 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704)、用戶端 app 暫停、作業系統關機或處於睡眠狀態，或者作業系統因資源不足而無法執行工作時，會取消工作。

## <a name="write-the-code-for-the-app-service"></a>撰寫 app 服務的程式碼

**OnRequestedReceived()** 是撰寫 app 服務的程式碼所在位置。 使用這個範例中的程式碼，來取代 MyAppService 的 Class1.cs 中的虛設常式 **OnRequestedReceived()**。 這個程式碼會取得庫存項目的索引，並將它和命令字串一起傳送到服務，來擷取指定庫存項目的名稱和價格。 為求簡潔，已移除錯誤處理程式碼。

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
         inventoryIndex.Value >= 0 &&
         inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
}
```

請注意，**OnRequestedReceived()** 是 **async**，因為我們要在這個範例中對 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 建立可等候的方法呼叫。

隨即會產生延遲，讓服務能夠在 OnRequestReceived 處理常式中使用 **async** 方法。 它可確保 OnRequestReceived 的呼叫在其完成處理訊息之後才會完成。 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 是在完成時用來傳送回應。 **SendResponseAsync** 不會發出完成呼叫的訊號。 它是對 [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) 發出訊號表示 OnRequestReceived 已完成的延遲完成。

app 服務會使用[**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 來交換資訊。 您可能傳送的資料大小僅受限於系統資源。 沒有任何預先定義的機碼可供您在 **ValueSet** 中使用。 您必須決定要用來定義 app 服務通訊協定的機碼值。 在撰寫呼叫者時，必須以該通訊協定來考量。 在這個範例中，我們選擇名為 "Command" 的機碼，它的值會指出我們是否想要 app 服務提供庫存項目的名稱或其價格。 庫存名稱的索引儲存在 "ID" 機碼下方。 傳回的值會儲存在 "Result" 機碼下方。

[ **AppServiceClosedStatus** ](https://msdn.microsoft.com/library/windows/apps/dn921703) 列舉會傳回給呼叫者，指出 app 服務的呼叫成功或失敗。 app 服務呼叫可能會失敗的範例，就是作業系統中止服務端點、資源過度使用等。 您可以透過 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 傳回其他錯誤資訊。 在這個範例中，我們使用名為 "Status" 的機碼，為呼叫者傳回更詳細的錯誤資訊。

呼叫 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 會為呼叫者傳回 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>部署服務 app，並取得套件系列名稱

您必須先部署 app 服務提供者 app，才能從用戶端呼叫它。 您也需要 app 服務 app 的套件系列名稱，才能呼叫它。

-   取得 app 服務應用程式套件系列名稱的一種方式是從 **AppServiceProvider** 專案 (例如，從 App.xaml.cs 中的 `public App()`) 內呼叫 [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670)，並記錄結果。 若要在 Microsoft Visual Studio 中執行 AppServiceProvider，請在 [方案總管] 視窗中將它設定為啟始專案並執行專案。
-   取得套件系列名稱的另一種方法是部署方案 ([建置] &gt; [部署方案])，並記下輸出視窗中的完整套件名稱 ([檢視] &gt; [輸出])。 您必須在輸出視窗中移除字串的平台資訊，以衍生套件名稱。 例如，如果輸出視窗中報告的完整套件名稱為 "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra"，則您必須抽出 "1.0.0.0\_x86\_\_"，留下 "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra" 做為套件系列名稱。

## <a name="write-a-client-to-call-the-app-service"></a>撰寫呼叫 App 服務的用戶端

1.  將新的空白 Windows 通用 App 專案 (命名為 ClientApp) 新增到方案 ([檔案] &gt; [新增] &gt; [新增專案]) (在 [加入新的專案] 對話方塊中，選擇 [已安裝] &gt; [其他語言] &gt; [Visual C#] &gt; [Windows] &gt; [Windows 通用] &gt; [空白 App (Windows 通用)])。
2.  在 ClientApp 專案中，將下列 **using** 陳述式新增到 MainPage.xaml.cs 頂端：
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  在 MainPage.xaml 中新增文字方塊和按鈕。
4.  新增適用於按鈕的按鈕點選處理常式，並在按鈕處理常式的簽章中新增關鍵字 **async**。
5.  使用下列程式碼取代按鈕點選處理常式的虛設常式。 請務必包含 `inventoryService` 欄位宣告。

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section.
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
                return;
            }
        }

        // Call the service.
        int idx = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("Command", "Item");
        message.Add("ID", idx);
        AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
        string result = "";

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data  that the service sent  to us.
            if (response.Message["Status"] as string == "OK")
            {
                result = response.Message["Result"] as string;
            }
        }

        message.Clear();
        message.Add("Command", "Price");
        message.Add("ID", idx);
        response = await this.inventoryService.SendMessageAsync(message);

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data that the service sent to us.
            if (response.Message["Status"] as string == "OK")
            {
                result += " : Price = " + response.Message["Result"] as string;
            }
        }

        button.Content = result;
    }
    ```

    將 `this.inventoryService.PackageFamilyName = "replace with the package family name";` 一行中的套件系列名稱以 **AppServiceProvider** 專案 (在 \[步驟 5：部署服務 App，並取得套件系列名稱\] 中取得) 的套件系列名稱取代。

    該程式碼會先建立與 App 服務的連線。 該連線會保持開啟，直到您處置 **this.inventoryService**。 App 服務名稱必須符合您新增至 AppServiceProvider 專案之 Package.appxmanifest 檔案的 **AppService 名稱**屬性。 在此範例中，它是 `<uap:AppService Name="com.microsoft.inventory"/>`。

    建立名為 **message** 的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 來指定我們要傳送到 App 服務的命令。 範例 App 服務會預期要採取兩個動作中哪一個動作的命令。 從 ClientApp 中的文字方塊取得索引，並以「Item」命令呼叫服務，以取得項目的描述。 然後，呼叫「Price」命令來取得項目的價格。 按鈕文字將根據結果設定。

    因為 [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) 只會指出作業系統是否能夠連線到 App 服務，我們必須檢查從 App 服務收到之 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 中的「Status」索引鍵來確定它能滿足要求。

6.  在 Visual Studio 的 [方案總管] 視窗中，將 ClientApp 專案設定為啟始專案並執行該方案。 在文字方塊中輸入數字 1，然後按一下按鈕。 服務應會傳回「Chair : Price = 88.99」。

    ![顯示 Chair price=88.99 的範例 App](images/appserviceclientapp.png)

如果 App 服務呼叫失敗，請檢查 ClientApp 中的下列項目：

1.  確認指派給庫存服務連線的套件系列名稱符合 AppServiceProvider App 的套件系列名稱。 請參閱：**button\_Click()**`this.inventoryService.PackageFamilyName = "...";`)。
2.  在 **button\_Click()** 中，確認指派給庫存服務連線的 App 服務名稱符合 AppServiceProvider 的 Package.appxmanifest 檔案中的 App 服務名稱。 請參閱：`this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  確定已部署 AppServiceProvider App (在 [方案總管] 中，以滑鼠右鍵按一下方案，然後選擇 [部署])。

## <a name="debug-the-app-service"></a>偵錯 App 服務


1.  偵錯之前，請確定已部署整個方案，因為在呼叫服務之前，必須先呼叫 App 服務提供者 App。 (在 Visual Studio 中，[建置] &gt; [部署方案])。
2.  在 [方案總管] 中，以滑鼠右鍵按一下 AppServiceProvider 專案，然後選擇 [屬性]。 從 [偵錯] 索引標籤，將 [起始動作] 變更為 [不啟動，但在我的程式碼啟動時進行偵錯]。
3.  在 MyAppService 專案的 Class1.cs 檔案中，於 OnRequestReceived() 中設定中斷點。
4.  將 AppServiceProvider 專案設定為啟始專案，然後按 F5。
5.  從 [開始] 功能表 (而不是從 Visual Studio) 啟動 ClientApp。
6.  在文字方塊中輸入數字 1，然後按下按鈕。 偵錯工具將會在 app 服務呼叫中，於 app 服務的中斷點上停止。

## <a name="debug-the-client"></a>偵錯用戶端

1.  依照前述步驟中的指示來偵錯 app 服務。
2.  從 [開始] 功能表啟動 ClientApp。
3.  將偵錯工具附加到 ClientApp.exe 處理序 (而不是 ApplicationFrameHost.exe 處理序)。 (在 Visual Studio 中，選擇 \[偵錯\] \[附加到處理序\])。
4.  在 ClientApp 專案中，於 **button_Click()** 中設定中斷點。
5.  現在，當您在 ClientApp 的文字方塊中輸入 1，並按下按鈕時，即會叫用用戶端與 App 服務中的中斷點。

## <a name="remarks"></a>備註

這個範例提供建立 app 服務並從另一個 app 呼叫它的簡介。 請注意下列重點：建立背景工作來裝載 App 服務；將 windows.appservice 延伸模組新增到 App 服務提供者 App 的 Package.appxmanifest 檔案；取得 App 服務提供者 App 的套件系列名稱，讓我們能夠從用戶端 App 連接到它；以及使用 [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) 來呼叫服務。

## <a name="full-code-for-myappservice"></a>MyAppService 的完整程式碼

```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## <a name="related-topics"></a>相關主題

* [轉換 App 服務，以便與其主控 App 在相同處理序中執行](convert-app-service-in-process.md)
* [使用背景工作支援 App](support-your-app-with-background-tasks.md)



<!--HONumber=Dec16_HO1-->


