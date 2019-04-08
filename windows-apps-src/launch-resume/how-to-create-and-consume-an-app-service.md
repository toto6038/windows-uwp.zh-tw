---
title: 建立和取用應用程式服務
description: 了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 即可在應用程式的通訊、 IPC、 訊息、 背景通訊，即可在應用程式，應用程式服務的背景處理序間通訊
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 96ecad8494ad82dc4927b3675ad322f07a8ca7f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643573"
---
# <a name="create-and-consume-an-app-service"></a>建立和取用應用程式服務

應用程式服務是為其他 UWP app 提供服務的 UWP app。 這些服務可比擬為裝置上的網頁服務。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。 或者，App 的企業套件也許會有可供套件中其他 App 使用的一般拼字檢查應用程式服務。  應用程式服務可讓您建立 App 可於其所在裝置呼叫的無 UI 服務，而從 Windows 10 版本 1607 開始，則可在遠端裝置上呼叫這些服務。

從 Windows 10 版本 1607 開始，您便可以建立與主控 App 在相同處理程序中執行的應用程式服務。 本文重點為建立和取用在個別背景處理序中執行的應用程式服務。 如需有關與提供者在相同處理序中執行應用程式服務的詳細資訊，請參閱[轉換應用程式服務，以便與其主控應用程式在相同處理序中執行](convert-app-service-in-process.md)。

如需應用程式服務程式碼範例，請參閱[通用 Windows 平台 (UWP) App 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)。

## <a name="create-a-new-app-service-provider-project"></a>建立新的應用程式服務提供者專案

在此做法中，為了簡單起見，我們將在一個方案中建立所有項目。

1. 在 Visual Studio 2015 或更新版本中，建立新的 UWP 應用程式專案並將它命名**AppServiceProvider**。
    1. 選取**檔案 > 新增 > 專案...** 
    2. 在 **新的專案**對話方塊中，選取**已安裝 > Visual C# > 空白應用程式 (通用 Windows)**。 此應用程式將會讓應用程式服務可供其他 UWP 應用程式使用。
    3. 將專案命名為**AppServiceProvider**，選擇的位置，然後按一下**確定**。

2. 當系統要求您選取**目標**並**最低版本**針對專案中，選取 至少**10.0.14393**。 如果您想要使用新**SupportsMultipleInstances**屬性，您必須使用 Visual Studio 2017 和目標**10.0.15063** (**Windows 10 Creators Update**) 或更高版本。

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Package.appxmanifest 中加入應用程式服務擴充功能

在  **AppServiceProvider**專案中，開啟**Package.appxmanifest**在文字編輯器中的檔案： 

1. 以滑鼠右鍵按一下它**方案總管 中**。 
2. 選取 **以開啟**。 
3. 選取  **XML （文字） 編輯器**。 

新增下列`AppService`延伸模組內`<Application>`項目。 此範例會公告 `com.microsoft.inventory` 服務，以及將此 App 識別為 App 服務提供者的項目。 實際的服務將實作為背景工作。 應用程式服務專案會將服務公開至其他應用程式。 我們建議針對服務名稱使用反向網域名稱樣式。

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

`Category`屬性會識別此應用程式做為應用程式服務提供者。

`EntryPoint`屬性會識別實作服務，接下來，我們將實作限定的命名空間類別。

`SupportsMultipleInstances`屬性會指出每次應用程式服務呼叫時，它應該執行新處理序中。 這不是必要，但如果您需要這項功能，並設為目標的 10.0.15063 是您可以使用 SDK (**Windows 10 Creators Update**) 或更高版本。 其前面也應該有 `uap4` 命名空間。

## <a name="create-the-app-service"></a>建立 app 服務

1.  可將應用程式服務實作為背景工作。 這讓前景應用程式能夠叫用另一個應用程式中的應用程式服務。 若要建立 app service 背景工作的形式，將新的 Windows 執行階段元件專案加入方案 (**檔案&gt;新增&gt;新增專案**) 名為**MyAppService**。 在 **加入新的專案**對話方塊方塊中，選擇**已安裝 > Visual C# > Windows 執行階段元件 (通用 Windows)**。
2.  在 [ **AppServiceProvider**專案中，加入新的專案對專案參考**MyAppService**專案 (在**方案總管] 中**，以滑鼠右鍵按一下**AppServiceProvider**專案 >**新增** > **參考** > **專案** >  **方案**，選取**MyAppService** > **確定**)。 此步驟十分重要，因為如果未新增該參照，應用程式服務在執行階段將不會連線。
3.  在  **MyAppService**專案中，新增下列**使用**陳述式的頂端**Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  重新命名**Class1.cs**要**Inventory.cs**，並取代為虛設常式程式碼**Class1**新的背景工作類別名為**清查**:

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

    **執行**建立背景工作時呼叫。 因為背景工作會在 **Run** 完成之後終止，所以程式碼會進行延遲，讓背景工作能夠持續服務要求。 會實作為背景工作的 app service 大約 30 秒之後它會接收呼叫，除非它在該時段內再次呼叫或延遲中取出會保持作用。如果呼叫端相同的程序中實作的 app service，app service 的存留期會繫結至呼叫端的存留期。

    應用程式服務的存留期取決於呼叫者：

    * 如果呼叫端在前景中，應用程式服務的存留期相當於呼叫端。
    * 如果呼叫端在背景中，應用程式服務都會獲得 30 秒執行的時間。 進行延遲會再額外提供 5 秒鐘一次。

    **OnTaskCanceled**工作取消時呼叫。 工作已取消時用戶端應用程式處置[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704)、 用戶端應用程式已暫止、 OS 已關閉或處於睡眠狀態，或作業系統會執行工作的資源不足。

## <a name="write-the-code-for-the-app-service"></a>撰寫 app 服務的程式碼

**OnRequestReceived**是應用程式服務的程式碼在何處。 取代 stub **OnRequestReceived**中**MyAppService**的**Inventory.cs**本範例的程式碼。 這個程式碼會取得庫存項目的索引，並將它和命令字串一起傳送到服務，來擷取指定庫存項目的名稱和價格。 針對您自己的專案，請新增錯誤處理程式碼。

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

請注意， **OnRequestReceived**是**非同步**因為我們要呼叫的 awaitable 方法[SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722)在此範例中。

延遲，讓服務可以使用採取**非同步**中的方法**OnRequestReceived**處理常式。 這可確保 **OnRequestReceived** 的呼叫在其完成訊息處理之後才完成。  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722)將結果傳送給呼叫者。 **SendResponseAsync** 不會發出完成呼叫的訊號。 它是完成的延遲，以告知[SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712)可**OnRequestReceived**已完成。 在呼叫**SendResponseAsync**會包裝在 try/finally 區塊中，因為您必須先完成延遲即使**SendResponseAsync**會擲回例外狀況。

應用程式服務會使用[ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)來交換資訊的物件。 您可能傳送的資料大小僅受限於系統資源。 沒有任何預先定義的機碼可供您在 **ValueSet** 中使用。 您必須決定要用來定義 app 服務通訊協定的機碼值。 在撰寫呼叫者時，必須以該通訊協定來考量。 在這個範例中，我們選擇名為 `Command` 的機碼，它的值會指出我們是否想要應用程式服務提供庫存項目的名稱或其價格。 庫存名稱的索引儲存在 `ID` 機碼下方。 傳回的值會儲存在 `Result` 機碼下方。

[AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703)列舉會傳回呼叫端，以指出應用程式服務的呼叫成功或失敗。 應用程式服務呼叫會失敗，舉例來說，作業系統會因為其資源過度使用而中止服務端點。 您可以透過 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 傳回其他錯誤資訊。 在這個範例中，我們使用名為 `Status` 的機碼，為呼叫者傳回更詳細的錯誤資訊。

呼叫 [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) 會為呼叫者傳回 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>部署服務 app，並取得套件系列名稱

必須先部署 app service 提供者，您可以從用戶端呼叫它。 您可以選取來部署**建置 > 部署方案**Visual Studio 中。

您也將需要應用程式服務提供者的套件系列名稱才能夠呼叫它。 您可以開啟來取得**AppServiceProvider**專案的**Package.appxmanifest**設計工具檢視中的檔案 (中按兩下該**方案總管 中**)。 選取 **封裝**索引標籤上，複製值旁邊**套件系列名稱**，並將它貼某處 記事本 之類現在。

## <a name="write-a-client-to-call-the-app-service"></a>撰寫呼叫 App 服務的用戶端

1.  透過 \[檔案\] &gt; \[新增\] &gt; \[新增專案\]，將新的空白 Windows 通用應用程式專案新增到方案。 在 **加入新的專案**對話方塊方塊中，選擇**已安裝 > Visual C# > 空白應用程式 (通用 Windows)** 並將它命名**ClientApp**。

2.  在  **ClientApp**專案中，新增下列**使用**陳述式的頂端**MainPage.xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  新增文字方塊，稱為**textBox**並按鈕，以**MainPage.xaml**。

4.  新增按鈕 click 處理常式呼叫的按鈕**button_Click**，並加入關鍵字**非同步**按鈕處理常式的簽章。

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
    > 請務必在字串常值，而不是將它放在變數中貼上。 它將無法運作的如果您使用的變數。

    該程式碼會先建立與 App 服務的連線。 該連線會保持開啟，直到您處置 `this.inventoryService`。 應用程式服務名稱必須符合`AppService`項目的`Name`屬性新增至**AppServiceProvider**專案**Package.appxmanifest**檔案。 在此範例中，它是 `<uap3:AppService Name="com.microsoft.inventory"/>`。

    A [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)名為`message`會建立以指定我們想要傳送至 app service 的命令。 範例 App 服務會預期要採取兩個動作中哪一個動作的命令。 我們取得的索引，從用戶端應用程式中，在文字方塊中，然後呼叫服務與`Item`命令，以取得項目的描述。 接著，我們會使用 `Price` 命令來取得項目的價格。 按鈕文字將根據結果設定。

    因為[AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724)只會指出是否能夠連線至 app service 中，呼叫我們會檢查作業系統`Status`中的索引鍵[ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)我們收到來自應用程式若要確保它無法完成要求的服務。

6. 設定**ClientApp**專案是啟始專案 (以滑鼠右鍵按一下它在**方案總管** > **設定為啟始專案**) 和執行解決方案。 在文字方塊中輸入數字 1，然後按一下按鈕。 您應該會看到"Chair:價格 = 88.99"從服務傳回。

    ![顯示 Chair price=88.99 的範例 App](images/appserviceclientapp.png)

如果應用程式服務呼叫失敗，簽入下列**ClientApp**專案：

1.  確認套件系列名稱指派給清查服務的連線與相符的套件系列名稱**AppServiceProvider**應用程式。 請參閱中的一行 **] 按鈕\_按一下 [** 使用`this.inventoryService.PackageFamilyName = "...";`。
2.  在  ** 按鈕\_按一下 **，確認指派給清查服務連線的應用程式服務名稱符合中的應用程式服務名稱**AppServiceProvider**的**Package.appxmanifest**檔案。 請參閱：`this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  請確認**AppServiceProvider**已部署應用程式。 (在**方案總管**，以滑鼠右鍵按一下方案，然後選擇**部署方案**)。

## <a name="debug-the-app-service"></a>偵錯 App 服務

1.  偵錯之前，請確定已部署整個方案，因為在呼叫服務之前，必須先呼叫應用程式服務提供者應用程式。 (在 Visual Studio 中，[建置] &gt; [部署方案])。
2.  在 **方案總管**，以滑鼠右鍵按一下**AppServiceProvider**專案，然後選擇**屬性**。 從 [偵錯] 索引標籤，將 [起始動作] 變更為 [不啟動，但在我的程式碼啟動時進行偵錯]。 (請注意，如果您是使用 C++ 實作您的應用程式服務提供者，您可以從 \[偵錯\] 索引標籤將 \[啟動應用程式\] 變更為 \[否\])。
3.  在  **MyAppService**專案中，在**Inventory.cs**檔案中，在設定的中斷點**OnRequestReceived**。
4.  設定**AppServiceProvider**專案為啟始專案並按下**F5**。
5.  開始**ClientApp**從開始功能表 （不是從 Visual Studio)。
6.  在文字方塊中輸入數字 1，然後按下按鈕。 偵錯工具將會在 app 服務呼叫中，於 app 服務的中斷點上停止。

## <a name="debug-the-client"></a>偵錯用戶端

1.  依照前述步驟中的指示來偵錯呼叫應用程式服務的用戶端。
2.  啟動**ClientApp**從 [開始] 功能表。
3.  附加偵錯工具**ClientApp.exe**程序 (不**ApplicationFrameHost.exe**程序)。 (在 Visual Studio 中，選擇 \[偵錯\] \[附加到處理序\])。**&gt;**
4.  在 [ **ClientApp**專案中，在設定的中斷點 **] 按鈕\_按一下**。
5.  在用戶端和 app service 中的中斷點會現在當您輸入數字 1 到文字方塊中的點擊**ClientApp** ，然後按一下按鈕。

## <a name="general-app-service-troubleshooting"></a>一般應用程式服務疑難排解

如果您遇到**AppUnavailable**狀態之後嘗試連線至 app service，檢查下列項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆需部署，才能執行用戶端，因為若未部署，用戶端將沒有任何可連線的項目。 您可以使用 \[組建\] >  \[部署方案\] 從 Visual Studio 部署。
- 在 [**方案總管] 中**，確保您的應用程式服務提供者專案已實作的 app service 的專案的專案對專案參考。
- 確認`<Extensions>`項目和其子項目，已新增至**Package.appxmanifest**屬於應用程式服務提供者專案，依上文中指定的檔案[新增至應用程式服務擴充功能Package.appxmanifest](#appxmanifest)。
- 請確認[AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename)在您呼叫的應用程式服務提供者的用戶端中的字串符合`<uap3:AppService Name="..." />`中的應用程式服務提供者專案指定**Package.appxmanifest**檔案。
- 請確認[AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname)符合應用程式服務提供者元件，依上文中指定的套件系列名稱[Package.appxmanifest 中加入應用程式服務擴充功能](#appxmanifest)
- 對於跨處理序應用程式服務，例如此範例中，確認`EntryPoint`中指定`<uap:Extension ...>`您的應用程式服務提供者專案項目的**Package.appxmanifest**檔案符合命名空間和類別名稱的公用類別可實作[IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask)應用程式服務專案中。

### <a name="troubleshoot-debugging"></a>疑難排解偵錯

如果偵錯工具不會在您應用程式服務提供者或應用程式服務專案的中斷點處停止，請檢查以下項目：

- 確定已部署應用程式服務提供者專案和應用程式服務專案。 兩者皆須部署，才能執行用戶端。 您可以使用 \[組建\] >  \[部署方案\] 從 Visual Studio 部署這兩者。
- 請確定您想要偵錯的專案已設定為啟始專案，並為該專案的偵錯屬性會設定為不執行專案時**F5**按下。 在專案上按一下滑鼠右鍵，然後按一下 \[屬性\]，再按一下 \[偵錯\] (或在 C++ 中按一下 \[偵錯\])。 在 C# 中，將 \[開始動作\] 變更為 \[不啟動，但在我的程式碼啟動時進行偵錯\]。 在 C++ 中，將 \[啟動應用程式\] 設定為 \[否\]。

## <a name="remarks"></a>備註

這個範例介紹如何建立作為背景工作執行的應用程式服務，並從另一個應用程式呼叫該服務。 要注意的重要事項如下：

* 建立背景工作，以裝載應用程式服務。
* 新增`windows.appService`給應用程式服務提供者的延伸模組**Package.appxmanifest**檔案。
* 取得應用程式服務提供者的套件系列名稱，以便我們可以從用戶端應用程式連接到它。
* 從應用程式服務提供者專案中加入應用程式服務專案的專案對專案參考。
* 使用[Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704)來呼叫服務。

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

* [轉換應用程式服務，以便與其主機應用程式在相同處理序中執行](convert-app-service-in-process.md)
* [支援您的應用程式使用背景工作](support-your-app-with-background-tasks.md)
* [應用程式服務的程式碼範例 (C#，c + + 和 VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
