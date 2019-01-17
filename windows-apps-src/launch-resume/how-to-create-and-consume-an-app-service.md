---
title: 建立和取用 App 服務
description: 了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 應用程式通訊，處理序間通訊，IPC，背景傳訊，背景通訊，以應用程式的應用程式的應用程式服務
ms.date: 1/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5239bff53bb0e5383bce28b4d781a0ab6a41c3af
ms.sourcegitcommit: cfdc854fede8e586202523cdb59d3d0a2f5b4b36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2019
ms.locfileid: "9013957"
---
# <a name="create-and-consume-an-app-service"></a>建立和取用 App 服務

應用程式服務是為其他 UWP app 提供服務的 UWP app。 這些服務可比擬為裝置上的網頁服務。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。 或者，App 的企業套件也許會有可供套件中其他 App 使用的一般拼字檢查應用程式服務。  應用程式服務可讓您建立 App 可於其所在裝置呼叫的無 UI 服務，而從 Windows 10 版本 1607 開始，則可在遠端裝置上呼叫這些服務。

從 Windows10 版本 1607 開始，您便可以建立與主控 App 在相同處理程序中執行的應用程式服務。 本文重點為建立和取用在個別背景處理序中執行的應用程式服務。 如需有關與提供者在相同處理序中執行應用程式服務的詳細資訊，請參閱[轉換應用程式服務，以便與其主控應用程式在相同處理序中執行](convert-app-service-in-process.md)。

如需應用程式服務程式碼範例，請參閱[通用 Windows 平台 (UWP) App 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)。

## <a name="create-a-new-app-service-provider-project"></a>建立新的應用程式服務提供者專案

在此做法中，為了簡單起見，我們將在一個方案中建立所有項目。

1. 在 Visual Studio 2015 或更新版本，建立新的 UWP app 專案，並將它命名為**AppServiceProvider**。
    1. 選取**檔案 > 新 > 專案...** 
    2. 在 [**新增專案**] 對話方塊中，選取**已安裝 > Visual C# > 空白的應用程式 (通用 Windows)**。 此應用程式將會讓應用程式服務可供其他 UWP 應用程式使用。
    3. **AppServiceProvider**專案的名稱、 選擇的位置，並按一下 **[確定]**。

2. 當您要求選取**目標**和**最小版本**的專案，請至少選取**10.0.14393**。 如果您想要使用新的**SupportsMultipleInstances**屬性，您必須使用 Visual Studio 2017 和目標**10.0.15063** (**Windows 10 Creators Update**) 或更高版本。

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>將應用程式服務延伸模組新增到 Package.appxmanifest

在**AppServiceProvider**專案中，開啟**Package.appxmanifest**檔案在文字編輯器中： 

1. 以滑鼠右鍵按一下它在**方案總管] 中**。 
2. 選取 [**開啟方式**。 
3. 選取 [ **XML （文字） 編輯器**。 

新增下列`AppService`內的擴充功能`<Application>`元素。 此範例會公告 `com.microsoft.inventory` 服務，以及將此應用程式識別為應用程式服務提供者的項目。 實際的服務將實作為背景工作。 應用程式服務專案會將服務公開至其他應用程式。 我們建議針對服務名稱使用反向網域名稱樣式。

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

`Category`屬性會識別為應用程式服務提供者這個應用程式。

`EntryPoint`屬性會識別實作服務，接下來，我們會實作的命名空間限定類別。

`SupportsMultipleInstances`屬性表示每次呼叫應用程式服務時，它應該執行新的處理程序中。 這並非必要，但如果您需要此功能，並且針對 10.0.15063 則可供您 SDK (**Windows 10 Creators Update**) 或更高版本。 其前面也應該有 `uap4` 命名空間。

## <a name="create-the-app-service"></a>建立應用程式服務

1.  可將應用程式服務實作為背景工作。 這讓前景應用程式能夠叫用另一個應用程式中的應用程式服務。 若要建立作為背景工作的應用程式服務，將新的 Windows 執行階段元件專案新增到方案 (**檔案&gt;新增&gt;新的專案**) 命名為**MyAppService**。 在 [**加入新的專案**] 對話方塊中，選擇 [**已安裝 > Visual C# > Windows 執行階段元件 (通用 Windows)**]。
2.  在**AppServiceProvider**專案中，新增**MyAppService**專案的專案對專案參考 (在**方案總管]** 中，以滑鼠右鍵按一下**AppServiceProvider**專案 >**新增** >  **參考** > **專案** > **解決方案**，選取**MyAppService** > **確定**)。 此步驟十分重要，因為如果未新增該參照，應用程式服務在執行階段將不會連線。
3.  在**MyAppService**專案中，將下列**使用**陳述式新增到**Class1.cs**頂端：
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  將**Class1.cs**重新命名為**Inventory.cs**，並將虛設常式程式碼的**Class1**取代新的背景工作類別，名為**詳細目錄**：

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

    建立背景工作時，會呼叫**執行**。 因為背景工作會在 **Run** 完成之後終止，所以程式碼會進行延遲，讓背景工作能夠持續服務要求。 實作成為背景工作的應用程式服務，在收到呼叫後將保持運作約 30 秒，除非在該時間範圍內再次呼叫它或進行延遲。如果將該應用程式服務實作在與呼叫者相同的處理序中，則應用程式服務的存留期將繫結至呼叫者的存留期。

    應用程式服務的存留期取決於呼叫者：

    * 如果呼叫者在前景時，應用程式服務存留期是呼叫者相同。
    * 如果呼叫者在背景中，應用程式服務取得執行 30 秒鐘。 進行延遲會再額外提供 5 秒鐘一次。

    工作取消時會呼叫**OnTaskCanceled** 。 當用戶端 app 處置[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704)、 用戶端 app 暫停、 作業系統關機或睡眠或作業系統執行以執行工作的資源不足，會被取消工作。

## <a name="write-the-code-for-the-app-service"></a>撰寫應用程式服務的程式碼

**OnRequestReceived**是應用程式服務的程式碼到哪裡去。 此範例中的程式碼來取代**OnRequestReceived** **MyAppService**的**Inventory.cs**中的虛設常式。 這個程式碼會取得庫存項目的索引，並將它和命令字串一起傳送到服務，來擷取指定庫存項目的名稱和價格。 針對您自己的專案，請新增錯誤處理程式碼。

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

請注意**OnRequestReceived**為**非同步**，因為我們要建立可等候的方法，在此範例中以[SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722)呼叫。

延遲不會採取，讓服務能夠在**OnRequestReceived**處理常式中使用的**非同步**方法。 這可確保 **OnRequestReceived** 的呼叫在其完成訊息處理之後才完成。  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) 會將結果傳送至呼叫者。 **SendResponseAsync** 不會發出完成呼叫的訊號。 這表示延遲完成，也就是發出訊號讓 [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712) 知道 **OnRequestReceived** 已完成。 **SendResponseAsync**呼叫會包裝在 try/finally 區塊中，因為您必須完成延遲，即使**SendResponseAsync**擲回例外狀況。

應用程式服務會使用[ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)物件來交換資訊。 您可能傳送的資料大小僅受限於系統資源。 沒有任何預先定義的機碼可供您在 **ValueSet** 中使用。 您必須決定要用來定義 app 服務通訊協定的機碼值。 在撰寫呼叫者時，必須以該通訊協定來考量。 在這個範例中，我們選擇名為 `Command` 的機碼，它的值會指出我們是否想要應用程式服務提供庫存項目的名稱或其價格。 庫存名稱的索引儲存在 `ID` 機碼下方。 傳回的值會儲存在 `Result` 機碼下方。

[AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) 列舉會傳回給呼叫者，指出應用程式服務的呼叫成功或失敗。 應用程式服務呼叫會失敗，舉例來說，作業系統會因為其資源過度使用而中止服務端點。 您可以透過 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 傳回其他錯誤資訊。 在這個範例中，我們使用名為 `Status` 的機碼，為呼叫者傳回更詳細的錯誤資訊。

呼叫 [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) 會為呼叫者傳回 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>部署服務 app，並取得套件系列名稱

必須先部署應用程式服務提供者，您可以從用戶端呼叫它。 您可以在 Visual Studio 中選取 [**建置 > 部署解決方案**部署。

您也需要應用程式服務提供者的套件系列名稱才能呼叫它。 您可以藉由開啟**AppServiceProvider**專案的**Package.appxmanifest**檔案中的設計工具檢視來取得它 （將它按兩下**方案總管]** 中）。 選取 [**封裝**] 索引標籤、**套件系列名稱**，旁邊的值複製和貼上它某處記事本之類現在。

## <a name="write-a-client-to-call-the-app-service"></a>撰寫呼叫應用程式服務的用戶端

1.  透過 \[檔案\] &gt; \[新增\] &gt; \[新增專案\]****，將新的空白 Windows 通用應用程式專案新增到方案。 在 [**加入新的專案**] 對話方塊中，選擇 [**已安裝 > Visual C# > 空白的應用程式 (通用 Windows)** ，並將它命名為**ClientApp**。

2.  在**ClientApp**專案中，將下列**using**陳述式新增至**MainPage.xaml.cs**頂端：
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  新增名為**MainPage.xaml**按鈕和**文字方塊**的文字方塊。

4.  新增 [按鈕按一下按鈕名為**button_Click**，處理常式，並新增關鍵字**async**按鈕處理常式的簽章。

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
    > 請確定字串常值，而不是將它放在變數中貼上。 它將無法運作的如果您使用的變數。

    該程式碼會先建立與應用程式服務的連線。 該連線會保持開啟，直到您處置 `this.inventoryService`。 應用程式服務名稱必須符合`AppService`元素的`Name`您新增至**AppServiceProvider**專案的**Package.appxmanifest**檔案的屬性。 在此範例中，它是 `<uap3:AppService Name="com.microsoft.inventory"/>`。

    名為[ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) `message`來指定我們要傳送到應用程式服務的命令會建立。 在範例應用程式服務中會有一個命令指示要採取兩個動作中的哪一個動作。 我們從用戶端應用程式中的文字方塊取得索引，然後以呼叫服務，`Item`命令來取得項目的描述。 接著，我們會使用 `Price` 命令來取得項目的價格。 按鈕文字將根據結果設定。

    因為 [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724) 只會指出作業系統是否能夠將呼叫連線到應用程式服務，所以我們會檢查從應用程式服務所收到 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 中的 `Status` 索引鍵，來確定它滿足了要求。

6. **ClientApp**將專案設定為啟始專案 (以滑鼠右鍵按一下**方案總管]** 中 > **設定為啟始專案**) 並執行該方案。 在文字方塊中輸入數字 1，然後按一下按鈕。 服務應會傳回「Chair : Price = 88.99」。

    ![顯示 Chair price=88.99 的範例 App](images/appserviceclientapp.png)

如果應用程式服務呼叫失敗，請檢查**ClientApp**專案中的下列：

1.  確認指派給庫存服務連線的套件系列名稱符合**AppServiceProvider** app 的套件系列名稱。 使用**button\_Click**中看到命令`this.inventoryService.PackageFamilyName = "...";`。
2.  在**button\_Click**，確認指派給庫存服務連線的應用程式服務名稱符合**AppServiceProvider**的**Package.appxmanifest**檔案中的應用程式服務名稱。 請參閱：`this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  請確定已部署**AppServiceProvider** app。 （在**方案總管]** 中，以滑鼠右鍵按一下方案和選擇 [**部署方案**]）。

## <a name="debug-the-app-service"></a>偵錯應用程式服務

1.  偵錯之前，請確定已部署整個方案，因為在呼叫服務之前，必須先呼叫應用程式服務提供者應用程式。 (在 Visual Studio 中，**\[建置\] &gt; \[部署方案\]**)。
2.  在**方案總管]** 中，以滑鼠右鍵按一下**AppServiceProvider**專案，並選擇 [**屬性**]。 從 \[偵錯\]**** 索引標籤，將 \[起始動作\]**** 變更為 \[不啟動，但在我的程式碼啟動時進行偵錯\]****。 (請注意，如果您是使用 C++ 實作您的應用程式服務提供者，您可以從 \[偵錯\]**** 索引標籤將 \[啟動應用程式\]**** 變更為 \[否\]****)。
3.  在**MyAppService**專案中，在**Inventory.cs**檔案中，設定**OnRequestReceived**中的中斷點。
4.  設定為啟始專案，然後按**F5** **AppServiceProvider**專案。
5.  從 [開始] 功能表 （不是從 Visual Studio) 啟動**ClientApp** 。
6.  在文字方塊中輸入數字 1，然後按下按鈕。 偵錯工具將會在 app 服務呼叫中，於 app 服務的中斷點上停止。

## <a name="debug-the-client"></a>偵錯用戶端

1.  依照前述步驟中的指示來偵錯呼叫應用程式服務的用戶端。
2.  從 [開始] 功能表啟動**ClientApp** 。
3.  將偵錯工具附加到**ClientApp.exe**處理序 （而不**ApplicationFrameHost.exe**處理序）。 (在 Visual Studio 中，選擇 **\[偵錯\] &gt; \[附加到處理序\]**)。
4.  在**ClientApp**專案中，請在**button\_Click**中設定中斷點。
5.  當您在**ClientApp**的文字方塊中輸入數字 1，並按一下按鈕時，現在會叫用戶端與應用程式服務中的中斷點。

## <a name="general-app-service-troubleshooting"></a>一般應用程式服務疑難排解

如果您遇到**AppUnavailable**狀態在嘗試連線到應用程式服務後，請檢查以下項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆需部署，才能執行用戶端，因為若未部署，用戶端將沒有任何可連線的項目。 您可以使用 \[組建\]**** >  \[部署方案\]**** 從 Visual Studio 部署。
- 在**方案總管]** 中，確定您的應用程式服務提供者專案有實作應用程式服務專案的專案對專案參照。
- 確認`<Extensions>`項目，以及它的子元素新增至屬於[新增到 Package.appxmanifest 應用程式服務延伸模組](#appxmanifest)中所指定應用程式服務提供者專案的**Package.appxmanifest**檔案。
- 請確定您的用戶端中呼叫應用程式服務提供者中的[AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename)字串，符合`<uap3:AppService Name="..." />`應用程式服務提供者專案的**Package.appxmanifest**檔案中指定。
- 確保[AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname)符合應用程式服務提供者元件[新增到 Package.appxmanifest 應用程式服務延伸模組](#appxmanifest)中所指定的套件系列名稱
- 對於跨處理序應用程式服務，例如服務在此範例中，確認`EntryPoint`中指定`<uap:Extension ...>`的應用程式服務提供者專案的**Package.appxmanifest**檔案中的項目符合命名空間和類別名稱的公用類別，在您的應用程式服務專案中實作[IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) 。

### <a name="troubleshoot-debugging"></a>疑難排解偵錯

如果偵錯工具不會在您應用程式服務提供者或應用程式服務專案的中斷點處停止，請檢查以下項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆須部署，才能執行用戶端。 您可以使用 \[組建\]**** >  \[部署方案\]**** 從 Visual Studio 部署這兩者。
- 確定您要偵錯的專案設定為啟始專案，且該專案的偵錯屬性設定為當按下**F5**時不執行專案。 在專案上按一下滑鼠右鍵，然後按一下 \[屬性\]****，再按一下 \[偵錯\]**** (或在 C++ 中按一下 \[偵錯\]****)。 在 C# 中，將 \[開始動作\]**** 變更為 \[不啟動，但在我的程式碼啟動時進行偵錯\]****。 在 C++ 中，將 \[啟動應用程式\]**** 設定為 \[否\]****。

## <a name="remarks"></a>備註

這個範例介紹如何建立作為背景工作執行的應用程式服務，並從另一個應用程式呼叫該服務。 要注意的重點是：

* 建立背景工作來裝載應用程式服務。
* 新增`windows.appService`延伸至應用程式服務提供者的**Package.appxmanifest**檔案。
* 取得套件系列名稱的應用程式服務提供者，以便我們能夠從用戶端 app 連接到它。
* 從應用程式服務提供者專案的專案對專案參照新增到應用程式服務專案中。
* 您可以使用[Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704)來呼叫服務。

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

* [轉換 App 服務，以便與其主控 App 在相同處理序中執行](convert-app-service-in-process.md)
* [使用背景工作支援應用程式](support-your-app-with-background-tasks.md)
* [應用程式服務程式碼範例 (C#、C++ 與 VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
