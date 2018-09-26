---
author: TylerMSFT
title: 建立和取用 App 服務
description: 了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 應用程式通訊，處理序間通訊，IPC，背景傳訊，背景通訊，以應用程式的應用程式的應用程式服務
ms.author: twhitney
ms.date: 09/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7475ae8db964b23de89488d883c135158ea20e74
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2018
ms.locfileid: "4178711"
---
# <a name="create-and-consume-an-app-service"></a>建立和取用 App 服務

應用程式服務是為其他 UWP app 提供服務的 UWP app。 這些服務可比擬為裝置上的網頁服務。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。 或者，App 的企業套件也許會有可供套件中其他 App 使用的一般拼字檢查應用程式服務。  應用程式服務可讓您建立 App 可於其所在裝置呼叫的無 UI 服務，而從 Windows 10 版本 1607 開始，則可在遠端裝置上呼叫這些服務。 

從 Windows10 版本 1607 開始，您便可以建立與主控 App 在相同處理程序中執行的應用程式服務。 本文重點為建立和取用在個別背景處理序中執行的應用程式服務。 如需有關與提供者在相同處理序中執行應用程式服務的詳細資訊，請參閱[轉換應用程式服務，以便與其主控應用程式在相同處理序中執行](convert-app-service-in-process.md)。

如需應用程式服務程式碼範例，請參閱[通用 Windows 平台 (UWP) App 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)。

## <a name="create-a-new-app-service-provider-project"></a>建立新的應用程式服務提供者專案

在此做法中，為了簡單起見，我們將在一個方案中建立所有項目。

-   在 Microsoft Visual Studio 中，建立新的 UWP 應用程式專案，並命名為 **AppServiceProvider**。 (在 **\[新增專案\]** 對話方塊中，選取 **\[範本\] &gt; \[其他語言\] &gt; \[Visual C#\] &gt; \[Windows\] &gt; \[Windows 通用\] &gt; \[空白 App (Windows 通用)\]**)。 此應用程式將會讓應用程式服務可供其他 UWP 應用程式使用。
-   當要求為該專案選取 \[目標版本\]**** 時，請至少選取 \[10.0.14393\]****。 如果您想要使用新的 `SupportsMultipleInstances` 屬性，則必須使用 Visual Studio 2017 和目標 **10.0.15063** (**Windows 10 Creators Update**) 或更高版本。

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>將應用程式服務延伸模組新增到 package.appxmanifest

在 AppServiceProvider 專案的 Package.appxmanifest 檔案中，在 `&lt;Application&gt;` 元素內部新增下列 AppService 延伸模組。 此範例會公告 `com.Microsoft.Inventory` 服務，以及將此應用程式識別為應用程式服務提供者的項目。 實際的服務將實作為背景工作。 應用程式服務專案會將服務公開至其他應用程式。 我們建議針對服務名稱使用反向網域名稱樣式。

請注意，`xmlns:uap4` 命名空間前置詞和 `uap4:SupportsMultipleInstances` 屬性只有在您以 Windows SDK 版本 10.0.15063 或更高版本為目標時才會有效。 如果您以舊版 SDK 為目標，則可以放心地移除它們。

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServicesProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServicesProvider.App">
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

**Category** 屬性會將此應用程式識別為應用程式服務提供者。

**EntryPoint** 屬性會識別實作服務的命名空間限定類別，我們將在下一步驟中實作此類別。

**SupportsMultipleInstances** 屬性表示每次呼叫應用程式服務時，應於新的處理序中執行。 此功能並非必要，但如果您需要此功能且以 `10.0.15063` SDK (**Windows 10 Creators Update**) 或更高版本為目標，則可供您使用。 其前面也應該有 `uap4` 命名空間。

## <a name="create-the-app-service"></a>建立應用程式服務

1.  可將應用程式服務實作為背景工作。 這讓前景應用程式能夠叫用另一個應用程式中的應用程式服務。 若要將應用程式服務建立成背景工作，則將新的 Windows 執行階段元件專案 (命名為 MyAppService) 新增到方案 (\[檔案\] &gt; \[新增\] &gt; \[新增專案\]****) (在 **\[加入新的專案\]** 對話方塊中，選擇 **\[已安裝\] &gt; \[其他語言\] &gt; \[Visual C#\] &gt; \[Windows\] &gt; \[Windows 通用\] &gt; \[Windows 執行階段元件 (Windows 通用)\]**)
2.  在 **AppServiceProvider** 專案中，將專案對專案參照新增至新的 **MyAppService** 專案 (在方案總管中，以滑鼠右鍵按一下 \[AppServiceProvider 專案\]****> \[新增\]**** > \[參照\]**** > \[專案\]**** > \[解決方案\]****，選取 \[MyAppService\]**** > \[確定\]****)。 此步驟十分重要，因為如果未新增該參照，應用程式服務在執行階段將不會連線。
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
            // This function is called when the app service receives a request
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

    建立背景工作時會呼叫 **Run()**。 因為背景工作會在 **Run** 完成之後終止，所以程式碼會進行延遲，讓背景工作能夠持續服務要求。 實作成為背景工作的應用程式服務，在收到呼叫後將保持運作約 30 秒，除非在該時間範圍內再次呼叫它或進行延遲。如果將該應用程式服務實作在與呼叫者相同的處理序中，則應用程式服務的存留期將繫結至呼叫者的存留期。

    應用程式服務的存留期取決於呼叫者：

    1. 如果呼叫者在前景中，則應用程式服務存留期會與呼叫者相同。
    2. 如果呼叫者在背景中，則應用程式服務會執行 30 秒鐘。 進行延遲會再額外提供 5 秒鐘一次。

    當取消工作時，會呼叫 **OnTaskCanceled()**。 當用戶端 app 處置 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704)、用戶端 app 暫停、作業系統關機或處於睡眠狀態，或者作業系統因資源不足而無法執行工作時，會取消工作。

## <a name="write-the-code-for-the-app-service"></a>撰寫應用程式服務的程式碼

**OnRequestReceived()** 是撰寫應用程式服務的程式碼所在位置。 以此範例中的程式碼來取代 MyAppService 的 Class1.cs 的虛設常式 **OnRequestReceived()**。 這個程式碼會取得庫存項目的索引，並將它和命令字串一起傳送到服務，來擷取指定庫存項目的名稱和價格。 針對您自己的專案，請新增錯誤處理程式碼。

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

    try
    {
        await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    }
    catch (Exception e)
    {
        // your exception handling code here
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

請注意，**OnRequestReceived()** 是 **async**，因為我們要在這個範例中對 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 建立可等候的方法呼叫。

隨即會產生延遲，讓服務能夠在 OnRequestReceived 處理常式中使用 **async** 方法。 這可確保 **OnRequestReceived** 的呼叫在其完成訊息處理之後才完成。  [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 會將結果傳送至呼叫者。 **SendResponseAsync** 不會發出完成呼叫的訊號。 這表示延遲完成，也就是發出訊號讓 [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) 知道 **OnRequestReceived** 已完成。 由於必須完成延遲，即使是 **SendResponseAsync()** 擲回例外狀況亦要完成，所以將對 **SendResponseAsync()** 的呼叫包含在 try/finally 區塊中。

應用程式服務會使用 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 來交換資訊。 您可能傳送的資料大小僅受限於系統資源。 沒有任何預先定義的機碼可供您在 **ValueSet** 中使用。 您必須決定要用來定義 app 服務通訊協定的機碼值。 在撰寫呼叫者時，必須以該通訊協定來考量。 在這個範例中，我們選擇名為 `Command` 的機碼，它的值會指出我們是否想要應用程式服務提供庫存項目的名稱或其價格。 庫存名稱的索引儲存在 `ID` 機碼下方。 傳回的值會儲存在 `Result` 機碼下方。

[**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) 列舉會傳回給呼叫者，指出應用程式服務的呼叫成功或失敗。 應用程式服務呼叫會失敗，舉例來說，作業系統會因為其資源過度使用而中止服務端點。 您可以透過 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 傳回其他錯誤資訊。 在這個範例中，我們使用名為 `Status` 的機碼，為呼叫者傳回更詳細的錯誤資訊。

呼叫 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 會為呼叫者傳回 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>部署服務 app，並取得套件系列名稱

您必須先部署 app 服務提供者 app，才能從用戶端呼叫它。 您也需要 app 服務 app 的套件系列名稱，才能呼叫它。

取得 app 服務應用程式套件系列名稱的一種方式是從 **AppServiceProvider** 專案 (例如，從 App.xaml.cs 中的 `public App()`) 內呼叫 [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670)，並記錄結果。 若要在 Microsoft Visual Studio 中執行 AppServiceProvider，請在 [方案總管] 視窗中將它設定為啟始專案並執行專案。

取得套件系列名稱的另一種方法是部署方案 (**\[建置\] &gt; \[部署方案\]**)，並記下輸出視窗中的完整套件名稱 (**\[檢視\] &gt; \[輸出\]**)。 您必須在輸出視窗中移除字串的平台資訊，以衍生套件名稱。 例如，如果輸出視窗中回報的完整套件名稱為 `Microsoft.SDKSamples.AppServicesProvider.CPP_1.0.0.0_x86__8wekyb3d8bbwe`，您會擷取  `1.0.0.0\_x86\_\_" leaving "Microsoft.SDKSamples.AppServicesProvider.CPP_8wekyb3d8bbwe` 作為套件系列名稱。

## <a name="write-a-client-to-call-the-app-service"></a>撰寫呼叫應用程式服務的用戶端

1.  透過 \[檔案\] &gt; \[新增\] &gt; \[新增專案\]****，將新的空白 Windows 通用應用程式專案新增到方案。 在 \[加入新的專案\]**** 對話方塊中，選擇 \[已安裝\] &gt; \[其他語言\] &gt; \[Visual C#\] &gt; \[Windows\] &gt; \[Windows 通用\] &gt; \[空白應用程式 (Windows 通用)\]****，然後將它命名為 **ClientApp**。
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
               textBox.Text= "Failed to connect";
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

       textBox.Text = result;
   }
   ```
將 `this.inventoryService.PackageFamilyName = "replace with the package family name";` 一行中的套件系列名稱，取代成為上述依照[部署服務應用程式並取得套件系列名稱](#deploy-the-service-app-and-get-the-package-family-name) 取得的 **AppServiceProvider** 專案套件系列名稱。

該程式碼會先建立與應用程式服務的連線。 該連線會保持開啟，直到您處置 `this.inventoryService`。 應用程式服務名稱必須符合您新增至 AppServiceProvider 專案其 Package.appxmanifest 檔案的 **AppService 名稱**屬性。 在此範例中，它是 `<uap:AppService Name="com.microsoft.inventory"/>`。

建立名為 **message** 的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 來指定我們要傳送到 App 服務的命令。 在範例應用程式服務中會有一個命令指示要採取兩個動作中的哪一個動作。 我們會從用戶端應用程式中的文字方塊取得索引，並以 `Item` 命令呼叫服務，以取得項目的描述。 接著，我們會使用 `Price` 命令來取得項目的價格。 按鈕文字將根據結果設定。

因為 [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) 只會指出作業系統是否能夠將呼叫連線到應用程式服務，所以我們會檢查從應用程式服務所收到 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 中的 `Status` 索引鍵，來確定它滿足了要求。

6.  在 Visual Studio 的 [方案總管] 視窗中，將 ClientApp 專案設定為啟始專案並執行該方案。 在文字方塊中輸入數字 1，然後按一下按鈕。 服務應會傳回「Chair : Price = 88.99」。

    ![顯示 Chair price=88.99 的範例 App](images/appserviceclientapp.png)

如果 App 服務呼叫失敗，請檢查 ClientApp 中的下列項目：

1.  確認指派給庫存服務連線的套件系列名稱符合 AppServiceProvider App 的套件系列名稱。 請參閱：**button\_Click()**`this.inventoryService.PackageFamilyName = "...";`)。
2.  在 **button\_Click()** 中，確認指派給庫存服務連線的 App 服務名稱符合 AppServiceProvider 的 Package.appxmanifest 檔案中的 App 服務名稱。 請參閱：`this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  確定已部署 AppServiceProvider App (在 \[方案總管\] 中，以滑鼠右鍵按一下方案，然後選擇 **\[部署\]**)。

## <a name="debug-the-app-service"></a>偵錯應用程式服務

1.  偵錯之前，請確定已部署整個方案，因為在呼叫服務之前，必須先呼叫應用程式服務提供者應用程式。 (在 Visual Studio 中，**\[建置\] &gt; \[部署方案\]**)。
2.  在 \[方案總管\] 中，以滑鼠右鍵按一下 **AppServiceProvider** 專案，然後選擇 \[屬性\]****。 從 \[偵錯\]**** 索引標籤，將 \[起始動作\]**** 變更為 \[不啟動，但在我的程式碼啟動時進行偵錯\]****。 (請注意，如果您是使用 C++ 實作您的應用程式服務提供者，您可以從 \[偵錯\]**** 索引標籤將 \[啟動應用程式\]**** 變更為 \[否\]****)。
3.  在 MyAppService 專案的 Class1.cs 檔案中，於 `OnRequestReceived()` 中設定中斷點。
4.  將 AppServiceProvider 專案設定為啟始專案，然後按 F5。
5.  從 [開始] 功能表 (而不是從 Visual Studio) 啟動 ClientApp。
6.  在文字方塊中輸入數字 1，然後按下按鈕。 偵錯工具將會在 app 服務呼叫中，於 app 服務的中斷點上停止。

## <a name="debug-the-client"></a>偵錯用戶端

1.  依照前述步驟中的指示來偵錯呼叫應用程式服務的用戶端。
2.  從 \[開始\] 功能表啟動 ClientApp。
3.  將偵錯工具附加到 ClientApp.exe 處理序 (而不是 ApplicationFrameHost.exe 處理序)。 (在 Visual Studio 中，選擇 **\[偵錯\] &gt; \[附加到處理序\]**)。
4.  在 ClientApp 專案中，於 **button_Click()** 中設定中斷點。
5.  現在，當您在 ClientApp 的文字方塊中輸入 1，並按下按鈕時，即會叫用用戶端與 App 服務中的中斷點。

## <a name="general-app-service-troubleshooting"></a>一般應用程式服務疑難排解 ##

如果您在嘗試連線到應用程式服務後碰到 \[AppUnavailable\]**** 狀態，請檢查下列項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆需部署，才能執行用戶端，因為若未部署，用戶端將沒有任何可連線的項目。 您可以使用 \[組建\]**** >  \[部署方案\]**** 從 Visual Studio 部署。
- 在方案總管中，確定您的應用程式服務提供者專案有專案對專案參照，可參照實作該應用程式服務的專案。
- 確認 `<Extensions>` 項目及其子元素皆已新增至屬於先前依照[將應用程式服務延伸模組新增到 package.appxmanifest](#appxmanifest) 中所指定應用程式服務提供者專案的 Package.appxmanifest 檔案。
- 確認在您用戶端中呼叫應用程式服務提供者的 `AppServiceConnection.AppServiceName` 字串，符合您在應用程式服務提供者專案的 Package.appxmanifest 檔案中指定的 `<uap3:AppService Name="..." />`。
- 確認 `AppServiceConnection.PackageFamilyName` 符合先前依照[將應用程式服務延伸模組新增到 package.appxmanifest](#appxmanifest) 中指定的應用程式服務提供者元件其套件系列名稱。
- 對於跨處理序應用程式服務，例如此範例中的服務，請確認在您應用程式服務提供者專案其 Package.appxmanifest 檔案的  `<uap:Extension ...>` 元素中指定的 `EntryPoint`，符合在您應用程式服務專案中實作 `IBackgroundTask` 的公用類別其命名空間與類別名稱。

### <a name="troubleshoot-debugging"></a>疑難排解偵錯

如果偵錯工具不會在您應用程式服務提供者或應用程式服務專案的中斷點處停止，請檢查以下項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆須部署，才能執行用戶端。 您可以使用 \[組建\]**** >  \[部署方案\]**** 從 Visual Studio 部署這兩者。
- 確定您要偵錯的專案設定成啟始專案，且該專案的偵錯屬性設定為當按下 F5 時不執行專案。 在專案上按一下滑鼠右鍵，然後按一下 \[屬性\]****，再按一下 \[偵錯\]**** (或在 C++ 中按一下 \[偵錯\]****)。 在 C# 中，將 \[開始動作\]**** 變更為 \[不啟動，但在我的程式碼啟動時進行偵錯\]****。 在 C++ 中，將 \[啟動應用程式\]**** 設定為 \[否\]****。

## <a name="remarks"></a>備註

這個範例介紹如何建立作為背景工作執行的應用程式服務，並從另一個應用程式呼叫該服務。 請注意下列重點：建立背景工作來裝載應用程式服務；將 windows.appservice 延伸模組新增到應用程式服務提供者應用程式的 Package.appxmanifest 檔案；取得應用程式服務提供者應用程式的套件系列名稱，讓我們能夠從用戶端應用程式與其連線；新增從應用程式服務提供者專案到應用程式服務專案的專案對專案參照；以及使用 [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) 來呼叫服務。

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
