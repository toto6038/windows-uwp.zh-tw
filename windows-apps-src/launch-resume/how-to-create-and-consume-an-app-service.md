---
title: 建立和取用應用程式服務
description: 了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 應用程式對應用程式通訊、處理序間通訊、IPC、背景訊息、背景通訊、應用程式對應用程式、app service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd67c8a94ae7fc46956f5a6f7c3358e0e28965d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158822"
---
# <a name="create-and-consume-an-app-service"></a>建立和取用應用程式服務

應用程式服務是為其他 UWP app 提供服務的 UWP app。 這些服務可比擬為裝置上的網頁服務。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。 或者，App 的企業套件也許會有可供套件中其他 App 使用的一般拼字檢查應用程式服務。  應用程式服務可讓您建立 App 可於其所在裝置呼叫的無 UI 服務，而從 Windows 10 版本 1607 開始，則可在遠端裝置上呼叫這些服務。

從 Windows 10 版本 1607 開始，您便可以建立與主控 App 在相同處理程序中執行的應用程式服務。 本文重點為建立和取用在個別背景處理序中執行的應用程式服務。 如需有關與提供者在相同處理序中執行應用程式服務的詳細資訊，請參閱[轉換應用程式服務，以便與其主控應用程式在相同處理序中執行](convert-app-service-in-process.md)。

如需應用程式服務程式碼範例，請參閱[通用 Windows 平台 (UWP) App 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)。

## <a name="create-a-new-app-service-provider-project"></a>建立新的應用程式服務提供者專案

在此做法中，為了簡單起見，我們將在一個方案中建立所有項目。

1. 在 Visual Studio 2015 或更新版本中，建立新的 UWP 應用程式專案，並將其命名為 **AppServiceProvider**。
    1. 選取 [檔案] **> 新增 > 專案 ...** 
    2. 在 [ **建立新專案** ] 對話方塊中，選取 [ **空白應用程式 (通用 Windows) c #**]。 此應用程式將會讓應用程式服務可供其他 UWP 應用程式使用。
    3. 按 **[下一步]**，然後將專案命名為 **AppServiceProvider**，選擇該專案的位置，然後按一下 [ **建立**]。

2. 當系統要求您選取專案的 **目標** 和 **最小版本** 時，請選取 [至少 **10.0.14393**]。 如果您想要使用新的 **SupportsMultipleInstances** 屬性，您必須使用 Visual Studio 2017 或 Visual Studio 2019，而目標 **10.0.15063** (**Windows 10 Creators Update**) 或更高版本。

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>將 app service 延伸模組新增至 Package. package.appxmanifest

在 **AppServiceProvider** 專案的文字編輯器中，開啟 **package.appxmanifest** 檔案： 

1. 在 **方案總管**中，以滑鼠右鍵按一下該檔案。 
2. 選取 [ **開啟方式**]。 
3. 選取 [ **XML (Text) 編輯器**]。 

在專案內新增下列 `AppService` 擴充功能 `<Application>` 。 此範例會公告 `com.microsoft.inventory` 服務，以及將此應用程式識別為應用程式服務提供者的項目。 實際的服務將實作為背景工作。 應用程式服務專案會將服務公開至其他應用程式。 我們建議針對服務名稱使用反向網域名稱樣式。

請注意，`xmlns:uap4` 命名空間前置詞和 `uap4:SupportsMultipleInstances` 屬性只有在您以 Windows SDK 版本 10.0.15063 或更高版本為目標時才會有效。 如果您以舊版 SDK 為目標，則可以放心地移除它們。

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

屬性會將 `Category` 此應用程式識別為 app service 提供者。

`EntryPoint`屬性會識別用來執行服務的命名空間限定類別，我們接下來將會執行此服務。

`SupportsMultipleInstances`屬性指出每次呼叫 app service 時，都應該在新的進程中執行。 這並不是必要的，但如果您需要該功能且以 10.0.15063 SDK 為目標 (**Windows 10 Creators Update**) 或更高版本，則可供您使用。 其前面也應該有 `uap4` 命名空間。

## <a name="create-the-app-service"></a>建立應用程式服務

1.  可將應用程式服務實作為背景工作。 這讓前景應用程式能夠叫用另一個應用程式中的應用程式服務。 若要建立應用程式服務做為背景工作，請將新的 Windows 執行階段元件專案加入至方案 (檔新增名為**MyAppService**的** &gt; &gt; 新專案**) 。 在 [ **加入新專案** ] 對話方塊中，選擇 [ **已安裝 > Visual c #] > Windows 執行階段元件 (通用 Windows) **。
2.  在**AppServiceProvider**專案中，將專案對專案的參考加入至**方案總管**中的 [新增**MyAppService**專案] (，以滑鼠右鍵按一下 [ **AppServiceProvider** ] 專案 >**加入**  >  **參考**  >  **專案**  >  **方案**]，然後選取 [ **MyAppService**  >  **確定]**) 。 此步驟十分重要，因為如果未新增該參照，應用程式服務在執行階段將不會連線。
3.  在 **MyAppService** 專案中，將下列 **using** 語句新增至 **Class1.cs**頂端：
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  將**Class1.cs**重新命名為**Inventory.cs**，並以名為**清查**的新背景工作類別取代**Class1**的 stub 程式碼：

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
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

    建立背景工作時，會呼叫**Run** 。 因為背景工作會在 **Run** 完成之後終止，所以程式碼會進行延遲，讓背景工作能夠持續服務要求。 實作成為背景工作的應用程式服務，在收到呼叫後將保持運作約 30 秒，除非在該時間範圍內再次呼叫它或進行延遲。如果將該應用程式服務實作在與呼叫者相同的處理序中，則應用程式服務的存留期將繫結至呼叫者的存留期。

    應用程式服務的存留期取決於呼叫者：

    * 如果呼叫端在前景中，則應用程式服務存留期與呼叫端相同。
    * 如果呼叫端在背景中，則 app service 會取得30秒的時間來執行。 進行延遲會再額外提供 5 秒鐘一次。

    當工作取消時，會呼叫**OnTaskCanceled** 。 當用戶端應用程式處置 [AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)、用戶端應用程式已暫停、os 已關機或進入睡眠狀態，或作業系統用盡資源來執行工作時，即會取消此工作。

## <a name="write-the-code-for-the-app-service"></a>撰寫應用程式服務的程式碼

**OnRequestReceived** 是 app service 的程式碼所移至的位置。 將**MyAppService**的**Inventory.cs**中的存根**OnRequestReceived**取代為此範例中的程式碼。 這個程式碼會取得庫存項目的索引，並將它和命令字串一起傳送到服務，來擷取指定庫存項目的名稱和價格。 針對您自己的專案，請新增錯誤處理程式碼。

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
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

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

請注意， **OnRequestReceived** 是 **非同步** 的，因為我們對此範例中的 [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 進行可等候方法呼叫。

採用延遲，讓服務可以在**OnRequestReceived**處理常式中使用**非同步**方法。 它可確保在完成處理訊息之前，對 **OnRequestReceived** 的呼叫不會完成。  [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 會將結果傳送至呼叫者。 **SendResponseAsync** 不會發出完成呼叫的訊號。 這是完成延遲的作業，表示 [>sendmessageasync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) 該 **OnRequestReceived** 已完成。 對 **SendResponseAsync** 的呼叫會包裝在 try/finally 區塊中，因為即使 **SendResponseAsync** 擲回例外狀況，您也必須完成延遲。

應用程式服務會使用 [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) 物件來交換資訊。 您可能傳送的資料大小僅受限於系統資源。 沒有任何預先定義的機碼可供您在 **ValueSet** 中使用。 您必須決定要用來定義 app 服務通訊協定的機碼值。 在撰寫呼叫者時，必須以該通訊協定來考量。 在這個範例中，我們選擇名為 `Command` 的機碼，它的值會指出我們是否想要應用程式服務提供庫存項目的名稱或其價格。 庫存名稱的索引儲存在 `ID` 機碼下方。 傳回的值會儲存在 `Result` 機碼下方。

[AppServiceClosedStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) 列舉會傳回給呼叫者，指出應用程式服務的呼叫成功或失敗。 應用程式服務呼叫會失敗，舉例來說，作業系統會因為其資源過度使用而中止服務端點。 您可以透過 [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) 傳回其他錯誤資訊。 在這個範例中，我們使用名為 `Status` 的機碼，為呼叫者傳回更詳細的錯誤資訊。

呼叫 [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 會為呼叫者傳回 [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>部署服務 app，並取得套件系列名稱

您必須先部署 app service 提供者，才能從用戶端呼叫該服務提供者。 您可以在 Visual Studio 中選取 [ **組建 > 部署方案** ] 來部署它。

您也需要 app service 提供者的套件系列名稱，才能呼叫它。 您可以在設計工具視圖中開啟 **AppServiceProvider** 專案的 **package.appxmanifest** 檔案， (在 **方案總管**) 中按兩下它來取得它。 選取 [ **封裝** ] 索引標籤，複製 [ **套件系列名稱**] 旁的值，然後將它貼到 [記事本] 之類的位置。

## <a name="write-a-client-to-call-the-app-service"></a>撰寫呼叫應用程式服務的用戶端

1.  透過 \[檔案\] &gt; \[新增\] &gt; \[新增專案\]****，將新的空白 Windows 通用應用程式專案新增到方案。 在 [ **加入新專案** ] 對話方塊中，選擇 [ **已安裝] > Visual c # > 空白應用程式 (通用 Windows) ** 並將它命名為 **ClientApp**。

2.  在 **ClientApp** 專案中，將下列 **using** 語句新增至 **MainPage.xaml.cs**頂端：
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  將名為**textBox**和 button 的文字方塊新增至**MainPage。**

4.  針對名為 **button_Click**的按鈕新增按鈕點擊處理常式，並將關鍵字 **async** 新增至按鈕處理常式的簽章。

5. 使用下列程式碼取代按鈕點選處理常式的虛設常式。 請務必包含 `inventoryService` 欄位宣告。
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
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
           // Get the data  that the service sent to us.
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

       textBox.Text = result;
   }
   ```
    
    將 `this.inventoryService.PackageFamilyName = "Replace with the package family name";` 一行中的套件系列名稱，取代成為上述依照[部署服務應用程式並取得套件系列名稱](#deploy-the-service-app-and-get-the-package-family-name) 取得的 **AppServiceProvider** 專案套件系列名稱。

    > [!NOTE]
    > 請務必貼上字串常值，而不是將它放在變數中。 如果您使用變數，它將無法運作。

    該程式碼會先建立與應用程式服務的連線。 該連線會保持開啟，直到您處置 `this.inventoryService`。 App service 名稱必須符合您在 `AppService` `Name` **AppServiceProvider** 專案的 **package.appxmanifest** 檔案中新增的元素屬性。 在此範例中為 `<uap3:AppService Name="com.microsoft.inventory"/>`。

    會建立名為的 [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) ， `message` 以指定我們要傳送給 app service 的命令。 在範例應用程式服務中會有一個命令指示要採取兩個動作中的哪一個動作。 我們會從用戶端應用程式中的文字方塊取得索引，然後使用命令呼叫服務， `Item` 以取得專案的描述。 接著，我們會使用 `Price` 命令來取得項目的價格。 按鈕文字將根據結果設定。

    因為 [AppServiceResponseStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) 只會指出作業系統是否能夠將呼叫連線到應用程式服務，所以我們會檢查從應用程式服務所收到 [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) 中的 `Status` 索引鍵，來確定它滿足了要求。

6. 將**ClientApp**專案設定為啟始專案 (在 [**方案總管**  >  **設定為啟始專案**]) 中，以滑鼠右鍵按一下該專案，然後執行方案。 在文字方塊中輸入數字 1，然後按一下按鈕。 服務應會傳回「Chair : Price = 88.99」。

    ![顯示 Chair price=88.99 的範例 App](images/appserviceclientapp.png)

如果 app service 呼叫失敗，請在 **ClientApp** 專案中檢查下列專案：

1.  確認指派給清查服務連接的套件系列名稱符合 **AppServiceProvider** 應用程式的套件系列名稱。 請參閱中的行按鈕，並 ** \_ 按一下** `this.inventoryService.PackageFamilyName = "...";` 。
2.  ** \_ 按一下按鈕**，確認指派給清查服務連線的 app service 名稱，與**AppServiceProvider** **package.appxmanifest**檔案中的 app service 名稱相符。 請參閱：`this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  確定已部署 **AppServiceProvider** 應用程式。  (在 **方案總管**中，以滑鼠右鍵按一下方案，然後選擇 [ **部署方案**) 。

## <a name="debug-the-app-service"></a>偵錯應用程式服務

1.  偵錯之前，請確定已部署整個方案，因為在呼叫服務之前，必須先呼叫應用程式服務提供者應用程式。  (在 Visual Studio 中， **組建 &gt; 部署解決方案**) 。
2.  在 [ **方案總管**中，以滑鼠右鍵按一下 **AppServiceProvider** 專案，然後選擇 [ **屬性**]。 在 [ **調試** 程式] 索引標籤中，將 [ **啟動動作** ] 變更為 [ **不啟動]，但在程式碼啟動時進行調試**程式。 (請注意，如果您是使用 C++ 實作您的應用程式服務提供者，您可以從 \[偵錯\]**** 索引標籤將 \[啟動應用程式\]**** 變更為 \[否\]****)。
3.  在 **MyAppService** 專案的 **Inventory.cs** 檔案中，于 **OnRequestReceived**中設定中斷點。
4.  將 **AppServiceProvider** 專案設定為啟始專案，然後按下 **F5**。
5.  從 [開始] 功能表的 (開始 **ClientApp** ，而不是從 Visual Studio) 。
6.  在文字方塊中輸入數字 1，然後按下按鈕。 偵錯工具將會在 app 服務呼叫中，於 app 服務的中斷點上停止。

## <a name="debug-the-client"></a>偵錯用戶端

1.  依照前述步驟中的指示來偵錯呼叫應用程式服務的用戶端。
2.  從 [開始] 功能表啟動 **ClientApp** 。
3.  將偵錯工具附加至 **ClientApp.exe** 進程 (而不是 **ApplicationFrameHost.exe** 的進程) 。 在 Visual Studio 中 (，選擇 [ **Debug &gt; 附加至進程 ...**]。) 
4.  在 **ClientApp** 專案中，于 **按鈕 \_ 按一下**，設定中斷點。
5.  當您在 **ClientApp** 的文字方塊中輸入數位1，並按一下按鈕時，現在會叫用用戶端和 app service 中的中斷點。

## <a name="general-app-service-troubleshooting"></a>一般應用程式服務疑難排解

如果您在嘗試連線到 app service 之後遇到 **AppUnavailable** 狀態，請檢查下列各項：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆需部署，才能執行用戶端，因為若未部署，用戶端將沒有任何可連線的項目。 您可以使用**組建**  >  **部署解決方案**從 Visual Studio 部署。
- 在 **方案總管**中，確定您的 app service 提供者專案具有可執行 app service 之專案的專案對專案參考。
- 確認 `<Extensions>` 專案及其子項目已新增至屬於 app service 提供者專案的 **package.appxmanifest** 檔案，如以上所述，在 [ [將 App service 延伸模組新增至 package.appxmanifest](#appxmanifest)] 中所指定。
- 請確定用戶端中呼叫 app service 提供者的 [AppServiceConnection. AppServiceName](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) 字串符合 `<uap3:AppService Name="..." />` app service 提供者專案的 **package.appxmanifest** 檔案中指定的。
- 請確定 [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) 與 app service 提供者元件的套件系列名稱相符，如上所述， [將 app Service 延伸模組新增至 package. package.appxmanifest](#appxmanifest)
- 若為跨進程應用程式服務（例如此範例中的服務），請驗證 `EntryPoint` `<uap:Extension ...>` app service 提供者專案的 **package.appxmanifest** 檔案之元素中所指定的，是否符合在 app Service 專案中執行 [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask) 之公用類別的命名空間和類別名稱。

### <a name="troubleshoot-debugging"></a>疑難排解偵錯

如果偵錯工具不會在您應用程式服務提供者或應用程式服務專案的中斷點處停止，請檢查以下項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆須部署，才能執行用戶端。 您可以使用**組建**  >  **部署解決方案**，從 Visual Studio 部署它們。
- 確定您要進行偵錯工具的專案已設定為啟始專案，而且在按下 **F5** 時，該專案的 [偵錯工具屬性] 設定為 [不執行專案]。 在專案上按一下滑鼠右鍵，然後按一下 \[屬性\]****，再按一下 \[偵錯\]**** (或在 C++ 中按一下 \[偵錯\]****)。 在 C# 中，將 \[開始動作\]**** 變更為 \[不啟動，但在我的程式碼啟動時進行偵錯\]****。 在 C++ 中，將 \[啟動應用程式\]**** 設定為 \[否\]****。

## <a name="remarks"></a>備註

這個範例介紹如何建立作為背景工作執行的應用程式服務，並從另一個應用程式呼叫該服務。 要注意的重要事項如下：

* 建立用來裝載 app service 的背景工作。
* 將延伸模組新增 `windows.appService` 至 app service 提供者的 **package.appxmanifest** 檔案。
* 取得 app service 提供者的套件系列名稱，讓我們可以從用戶端應用程式連接到它。
* 從 app service 提供者專案將專案對專案參考新增至 app service 專案。
* 使用 [ApplicationModel. AppService. AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) 來呼叫服務。

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
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
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

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
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

* [轉換應用程式服務，以便與其主控應用程式在相同處理序中執行](convert-app-service-in-process.md)
* [使用背景工作支援應用程式](support-your-app-with-background-tasks.md)
* [應用程式服務程式碼範例 (C#、C++ 與 VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)