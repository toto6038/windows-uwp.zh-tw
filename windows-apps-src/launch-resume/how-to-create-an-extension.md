---
author: TylerMSFT
title: 建立和使用應用程式延伸模組
description: 撰寫和裝載通用 Windows 平台 (UWP) 應用程式延伸模組可讓您透過使用者可從 Microsoft Store 安裝的套件來擴充應用程式。
keywords: 應用程式延伸模組, 應用程式服務, 背景
ms.author: twhitney
ms.date: 10/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c4c326dbafa719273c4535a42d58184c7ce360fe
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7286358"
---
# <a name="create-and-host-an-app-extension"></a>建立和裝載應用程式延伸模組

本文說明如何建立 UWP 應用程式延伸模組，並將其裝載於 UWP app。

本文附有程式碼範例：
- 下載並解壓縮[數學延伸模組程式碼範例](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)。
- 在 Visual Studio 2017 中，開啟 MathExtensionSample.sln。 將組建類型設定為 x86 (**\[建置\]** > **\[組態管理員\]**，然後將兩個專案的 **\[平台\]** 變更為 **\[x86\]**)。
- 部署方案：**\[建置\]** > **\[部署方案\]**。

## <a name="introduction-to-app-extensions"></a>應用程式延伸模組簡介

在通用 Windows 平台 (UWP) 中，擴充功能提供類似於外掛模組、增益集和附加元件在其他平台上所執行的功能。 舉例來說，Microsoft Edge 延伸模組即是 UWP 應用程式延伸模組。 UWP 應用程式延伸模組是在 Windows 10 年度更新版 (版本 1607 組建 10.0.14393) 中引進的。

UWP 應用程式延伸模組是具有延伸宣告的 UWP app，此宣告可讓延伸模組與主機應用程式共用內容和部署事件。 延伸應用程式可以提供多個延伸模組。

應用程式延伸模組就是 UWP app，因此同樣可以做為功能完整的應用程式、裝載延伸模組，以及提供延伸模組給其他應用程式，完全不需要建立不同的應用程式套件。

建立應用程式延伸主機時，可能會提供以您的應用程式為中心發展生態系統的機會，其他開發人員可在其中用您想像不到的方式或您沒有的資源來增強該應用程式，。 請考慮建立 Microsoft Office 延伸模組、Visual Studio 延伸模組、瀏覽器延伸等延伸模組。這些延伸模組建立為這些應用程式更加豐富且超出其所隨附功能的體驗。 延伸模組可以增加應用程式的價值並延長其使用壽命。

**概觀**

概略來說，要設定應用程式延伸模組關聯性，我們必須：

1. 宣告要成為延伸主機的應用程式。
2. 宣告要成為延伸模組的應用程式。
3. 決定要將延伸模組實作為應用程式服務、背景工作，還是其他形式。
4. 定義主機與其延伸模組通訊的方式。
5. 在主機應用程式中使用 [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions) API 來存取延伸模組。

我們來仔細查看實作假想小算盤的[數學延伸模組程式碼範例](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)，看看如何完成這項工作，您可以使用延伸模組來加入新功能。 在 Microsoft Visual Studio 2017 中，從程式碼範例載入 **MathExtensionSample.sln**。

![數學延伸模組程式碼範例](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>宣告要成為延伸主機的應用程式

應用程式藉由在 Package.appxmanifest 檔案中宣告 `<AppExtensionHost>` 元素，將本身識別為應用程式延伸主機。 若要了解此作法，請參閱 **MathExtensionHost** 專案中的 **Package.appxmanifest** 檔案。

_MathExtensionHost 專案中的 Package.appxmanifest_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

請注意 `xmlns:uap3="http://..."`，以及 `uap3` 是否存在於 `IgnorableNamespaces`。 由於我們要使用 uap3 命名空間，因此上述注意事項是必要。

`<uap3:Extension Category="windows.appExtensionHost">` 將此應用程式識別為延伸主機。

`<uap3:AppExtensionHost>` 中的 **Name** 元素是_延伸協定_名稱。 延伸模組指定相同的延伸協定時，主機將可以找到它。 依慣例，我們建議您使用應用程式名稱或發佈者名稱建立延伸協定名稱，以避免與其他延伸協定名稱產生可能的衝突。

您可以在同一個應用程式中定義多個主機和多個延伸模組。 在此範例中，我們宣告了一個主機。 延伸模組定義在其他應用程式中。

## <a name="declare-an-app-to-be-an-extension"></a>宣告要成為延伸模組的應用程式

應用程式藉由在 **Package.appxmanifest** 檔案中宣告 `<uap3:AppExtension>` 元素，將本身識別為應用程式延伸模組。 若要了解此作法，請開啟 **MathExtension** 專案中的 **Package.appxmanifest** 檔案。

_MathExtension 專案中的 Package.appxmanifest：_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

再次注意 `xmlns:uap3="http://..."` 程式碼行，以及 `uap3` 是否存在於 `IgnorableNamespaces`。 由於我們要使用 `uap3` 命名空間，因此上述注意事項是必要。

`<uap3:Extension Category="windows.appExtension">` 將此應用程式識別為延伸模組。

`<uap3:AppExtension>` 屬性的意義如下：

|屬性|描述|必要|
|---------|-----------|:------:|
|**Name**|這是延伸協定名稱。 符合主機中宣告的 **Name** 時，該主機將可找到此延伸模組。| :heavy_check_mark: |
|**ID**| 唯一識別此延伸模組。 由於可能會有多個使用相同延伸協定的延伸模組 (試想支援數個延伸模組的繪圖應用程式)，您可以使用 ID 來加以區分。 應用程式延伸主機可以使用 ID 推斷一些關於延伸模組類型的資訊。 例如，您可能會有一個延伸模組針對傳統型設計，另一個則針對行動裝置設計，此 ID 即為區分方法。 您也可以使用 **Properties** 元素進行區分，如下所述。| :heavy_check_mark: |
|**DisplayName**| 可以從主機應用程式中用來讓使用者認明延伸模組。 此屬性可查詢 (而且可以使用) [新資源管理系統](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) 進行當地語系化。 當地語系化內容是從應用程式延伸套件 (而非主機應用程式) 載入。 | |
|**Description** | 可以從主機應用程式中用來向使用者描述延伸模組。 此屬性可查詢 (而且可以使用) [新資源管理系統](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) 以進行當地語系化。 當地語系化內容是從應用程式延伸套件 (而非主機應用程式) 載入。 | |
|**PublicFolder**|與封裝根目錄相關之資料夾的名稱，您可以透過此資料夾與延伸主機共用內容。 依慣例，名稱是「Public」，但您可以使用任何符合延伸模組中資料夾的名稱。| :heavy_check_mark: |

`<uap3:Properties>` 是包含主機可於執行階段讀取之自訂中繼資料的選用元素。 程式碼範例將延伸模組實作為應用程式服務，因此主機需要一個取得該應用程式服務名稱的方式，以便呼叫該服務。 應用程式服務的名稱是在我們已定義的 <Service> 中定義 (我們可以隨意命名)。 程式碼範例中的主機於執行階段尋找此屬性，以得知應用程式服務的名稱。

## <a name="decide-how-you-will-implement-the-extension"></a>決定實作延伸模組的方式。

[關於應用程式延伸的組建 2016 工作階段](https://channel9.msdn.com/Events/Build/2016/B808)示範如何使用主機與延伸模組之間共用的公用資料夾。 在該範例中，主機叫用的延伸模組是由儲存在公用資料夾中的 Javascript 檔案所實作。 此方式的優點是輕量型、不需要編譯，以及可支援建立提供延伸模組相關指示和主機應用程式 Microsoft Store 頁面連結的預設登陸頁面。 請參閱[組建 2016 應用程式延伸模組程式碼範例](https://github.com/Microsoft/App-Extensibility-Sample)如需詳細資訊。 具體來說，請參閱 **InvertImageExtension** 專案，以及 **ExtensibilitySample** 專案中 ExtensionManager.cs 的 `InvokeLoad()`。

在此範例中，我們會使用應用程式服務來實作延伸模組。 應用程式服務有下列優點：

- 如果延伸模組損毀，並不會關閉主機應用程式，因為主機應用程式是其本身的處理序中執行。
- 您可以使用您選擇的語言來實作服務。 不需要符合用來實作主機應用程式的語言。
- 應用程式服務可以存取其本身的應用程式容器，而此容器可能會有不同於主機的功能。
- 服務與主機應用程式的資料之間會有隔離。

### <a name="host-app-service-code"></a>主機應用程式服務程式碼

以下是叫用延伸模組之應用程式服務的主機程式碼：

_MathExtensionHost 專案中的 ExtensionManager.cs_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

這是叫用應用程式服務的一般程式碼。 如需有關如何實作和呼叫應用程式服務的詳細資訊，請參閱[如何建立和使用應用程式服務](how-to-create-and-consume-an-app-service.md)。

有一點需要注意的是，如何判斷要呼叫之應用程式服務的名稱。 由於主機沒有實作延伸模組的相關資訊，延伸模組必須提供其應用程式服務的名稱。 在程式碼範例中，延伸模組於 `<uap3:Properties>` 元素宣告其檔案中應用程式服務的名稱：

_MathExtension 專案中的 Package.appxmanifest_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

您可以在 `<uap3:Properties>` 元素中定義您自己的 XML。 在此情況下，我們定義應用程式服務的名稱，讓主機可以在叫用延伸模組時呼叫該服務？

主機載入延伸模組時，像這樣的程式碼會從延伸模組之 Package.appxmanifest 所定義的屬性中擷取服務的名稱：

_`Update()` 在 MathExtensionHost 專案的 ExtensionManager.cs 中_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

有了儲存在 `_serviceName` 中的應用程式服務名稱，主機就可以使用該名稱來叫用應用程式服務。

叫用應用程式服務還需要包含應用程式服務之套件的系列名稱。 幸好應用程式延伸模組 API 提供這項資訊 (使用下列程式碼行來取得)： `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>定義主機與其延伸模組通訊的方式

應用程式服務會使用 [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) 來交換資訊。 身為主機的作者，您必須提供通訊協定，才能與彈性的延伸模組進行通訊。 在程式碼範例中，這意味著要解釋如何處理未來可能接受 1、2 或更多個引數的延伸模組。

就此範例而言，引數的通訊協定是 **ValueSet**，包含以「'Arg' + 引數編號」命名 (例如 `Arg1` 和 `Arg2`) 的索引鍵值組。 主機傳遞 **ValueSet** 中所有的引數，而延伸模組則使用其所需的引數。 如果延伸模組可以計算結果，則主機預期延伸模組傳回的 **ValueSet** 會有包含計算值的 `Result`。 如果該索引鍵不存在，主機會假設延伸模無法完成計算。

### <a name="extension-app-service-code"></a>延伸應用程式服務程式碼

在程式碼範例中，延伸模組的應用程式服務不是實作成背景工作， 而是使用單一處理序應用程式服務模型，也就是應用程式服務與進行裝載的延伸應用程式會在同一個處理序中執行。 這仍然是不同於主機應用程式的處理序，不僅提供處理序區隔的好處，同時也因為在延伸模組與實作應用程式服務的背景處理序之間避免了跨處理序通訊，而獲得一些效能優勢。 請參閱[轉換應用程式服務，以便與其主機應用程式在相同處理序中執行](convert-app-service-in-process.md)，了解以背景工作方式執行的相較於在相同處理序中執行的應用程式服務之間有何差異。

系統會在啟動應用程式服務時呼叫 `OnBackgroundActivate()`。 程式碼設定了事件處理常式，在實際的應用程式服務呼叫發生時 (`OnAppServiceRequestReceived()`) 加以處理，同時也處理內務處理事件，例如取得處理取消或關閉事件的延遲物件。

_MathExtension 專案中的 App.xaml.cs。_
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

在延伸模組中負責運作的程式碼位於 `OnAppServiceRequestReceived()`。 叫用應用程式服務來執行計算時，會呼叫此函式。 函式會從 **ValueSet** 擷取其所需的值。 如果可以執行計算，就會在名為 **Result** 的索引鍵下，將結果置入傳回給主機的 **ValueSet**。 回想一下，依據針對此主機與其延伸模組通訊方式所定義的通訊協定，如果存在 **Result** 索引鍵則表示成功，否則表示失敗。

_MathExtension 專案中的 App.xaml.cs。_
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>管理延伸模組

現在已經了解如何實作主機與其延伸模組之間的關聯性，我們來看看主機如何尋找系統中已安裝的延伸模組，以及其對新增和移除包含延伸模組之套件的回應方式。

Microsoft Store 以套件形式來提供延伸模組。 [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) 會尋找包含符合主機延伸協定名稱的已安裝套件，並提供會在安裝或移除與主機相關之應用程式延伸模組套件時引發的事件。

在程式碼範例中，`ExtensionManager` 類別 (定義在 **MathExtensionHost** 專案的 **ExtensionManager.cs** 中) 包裝的邏輯會載入延伸模組並回應延伸模組套件的安裝與解除安裝。

`ExtensionManager` 建構函式使用 `AppExtensionCatalog`，在延伸協定名稱與主機相同的系統上尋找應用程式延伸模組：

_MathExtensionHost 專案中的 ExtensionManager.cs。_
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

安裝延伸模組套件時，`ExtensionManager` 會在延伸協定名稱與主機相同的套件中收集延伸模組的相關資訊。 安裝相當於更新，此時受影響延伸模組的資訊也會更新。 解除安裝延伸模組套件時，`ExtensionManager` 會移除受影響延伸模組的相關資訊，讓使用者知道哪些延伸模組已無法使用。

程式碼範例建立的 `Extension` 類別 (定義在 **MathExtensionHost** 專案的 **ExtensionManager.cs** 中) 是用來存取延伸模組識別碼、描述、標誌，以及應用程式特定資訊，例如使用者是否已啟用延伸模組。

所謂延伸模組已載入 (請參閱 **ExtensionManager.cs** 中的 `Load()`) 即表示套件狀態正常，我們已取得其識別碼、標誌、描述，以及公用資料夾 (未用於此範例，只是展示您取得的方式)。 延伸模組套件本身不會載入。

卸載的概念用於追蹤不再提供哪些延伸模組給使用者。

`ExtensionManager` 提供 `Extension` 執行個體集合，讓延伸模組、其名稱、描述及標記可以和 UI 建立資料繫結。 **ExtensionsTab** 頁面會繫結至此集合，並提供可用來啟用/停用和移除延伸模組的 UI。

![[延伸模組] 索引標籤範例 UI](images/mathextensionhost-extensiontab.png)

 移除延伸模組時，系統會提示使用者確認是否要解除安裝包含延該伸模組 (而且可能包含其他延伸模組) 的套件。 如果使用者同意，就會已解除安裝套件，而 `ExtensionManager` 則從主機應用程式可用的延伸模組清單中移除已解除安裝之套件中的延伸模組。

 ![解除安裝 UI](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>偵錯應用程式延伸模組與主機

主機延伸與延伸模組通常不屬於相同的方案。 若要在此呼叫下偵錯主機和延伸模組：

1. 在 Visual Studio 的一個執行個體中載入主機專案。
2. 在 Visual Studio 的另一個執行個體中載入延伸模組。
3. 在偵錯工具中啟動主機應用程式。
4. 在偵錯工具中啟動延伸模組  (如果您想要部署而非偵錯延伸模組，以測試主機的套件安裝事件時，請改為執行 **\[建置\] &gt; \[部署方案\]**)。

現在您可以在主機和延伸模組中叫用中斷點。
如果開始偵延伸模組應用程式本身，您將會看到應用程式的空白視窗。 如果不想看到空白視窗，您可以將延伸模組專案的偵錯設定變更為不要在開始偵錯時啟動應用程式而是進行偵錯 (以滑鼠右鍵按一下延伸模組專案、**\[屬性\]** > **\[偵錯\]** > 選取 **\[不啟動，但在我的程式碼啟動時進行偵錯\]**)。您依舊需要對延伸模組專案啟動偵錯 (**F5**)，但會等候直到主機啟動延伸模組後，才叫用延伸模組中的中斷點。

**偵錯範例程式碼**

在程式碼範例中，主機與延伸模組位於相同的方案。 執行下列動作以進行偵錯：

1. 確認 **MathExtensionHost** 是啟始專案 (以滑鼠右鍵按一下 **MathExtensionHost** 專案，按一下 **\[設定為啟始專案\]**)。
2. 在 **MathExtensionHost** 專案中 ExtensionManager.cs 的 `Invoke` 上放置中斷點。
3. 按下 **F5** 以執行 **MathExtensionHost** 專案。
4. 在 **MathExtension** 專案中 App.xaml.cs 的 `OnAppServiceRequestReceived` 上放置中斷點。
5. 開始偵錯 **MathExtension** 專案 (以滑鼠右鍵按一下 **MathExtension** 專案，**\[偵錯\] > \[開始新執行個體\]**)，這樣將會進行部署，並在主機中觸發套件安裝事件。
6. 在 **MathExtensionHost** 應用程式中，瀏覽至 **\[Calculation\]** 頁面，然後按一下 **\[x^y\]** 以啟動延伸模組。 首先會叫用 `Invoke()`中斷點，您可以看到正在進行延伸模組應用程式服務呼叫。 接著叫用延伸模組中的 `OnAppServiceRequestReceived()` 方法，您可以看到應用程式服務計算結果並將結果傳回。

**疑難排解實作為背景工作的應用程式服務**

如果延伸主機有無法連接邀延伸模組的應用程式服務，請確定 `<uap:AppService Name="...">` 屬性符合您置入 `<Service>` 元素中的資訊。 如果與延伸模組提供的服務名稱不相符，主機就無法符合您實作的應用程式服務名稱，而且主機也無法啟動延伸模組。

_MathExtension 專案中的 Package.appxmanifest：_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>要測試的基本案例檢查清單

當您建置延伸主機，並且準備好要測試其支援延伸模組的程度時，以下是一些要嘗試的基本案例：

- 執行主機，然後部署延伸模組應用程式  
    - 主機是否會選擇在其執行時一起出現的新延伸模組？  
- 部署延伸模組應用程式，然後部署並執行主機。
    - 主機是否會選擇先前既有的延伸模組？  
- 執行主機，然後移除延伸模組應用程式。
    - 主機是否正確偵測到移除？
- 執行主機，然後將延伸模組應用程式更新為較新的版本。
    - 主機是否會選擇變更的版本，並正確卸除延伸模組的舊版本？  

**要測試的進階案例：**

- 執行主機、將延伸模組應用程式移到抽取式媒體、移除媒體
    - 主機是否偵測到套件狀態變更並停用延伸模組？
- 執行主機，然後損毀延伸應用程式 (使之無效、以不同方式簽署等)。
    - 主機是否偵測到竄改的延伸模組並正確進行處理？
- 執行主機，然後部署內容或屬性無效的延伸應用程式
    - 主機是否偵測到無效的內容並正確進行處理？

## <a name="design-considerations"></a>設計考量

- 提供會向使用者顯示有哪些延伸模組可用，且可讓他們加以啟用/停用的 UI。 您也可以考慮新增會因為套件離線等狀況而變成無法使用的延伸模組字符。
- 將使用者導向至他們可以取得延伸模組的位置。 您的延伸模組頁面或許可以提供 Microsoft Store 搜尋查詢，顯示可與應用程式搭配使用的延伸模組清單。
- 考慮向使用者通知新增和移除延伸模組的方式。 您可能會建立通知以告知何時安裝了新的延伸模組，並邀請使用者啟用該延伸模組。 預設會停用延伸模組，讓使用者可以控制。

## <a name="how-app-extensions-differ-from-optional-packages"></a>應用程式延伸模組與選用套件有何不同

[選用套件](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)與應用程式延伸模組之間的主要區分在於開放式生態系統與封閉式生態系統，以及相依套件與獨立套件之間的差異。

應用程式延伸模組會參與開放式生態系統。 如果您的應用程式可以裝載應用程式延伸模組，任何人都能針對您的主機撰寫延伸模組，只要這些延伸模組符合您從延伸模組傳送/接收資訊的方法即可。 這點與參與封閉式生態系統的選用套件不同，其中是由發佈者決定允許誰來建立可與應用程式搭配使用的選用套件。

應用程式延伸模組是獨立套件，而且可以成為獨立應用程式。 這些延伸模組對其他應用程式不能有部署相依性。選用套件必須有主要套件，沒有就無法執行。

遊戲的擴充套件緊密繫結至遊戲，因此是很好的選用套件候選項目，無法單獨在遊戲之外執行，而且您可能也不希望生態系統中的任何開發人員都來建立擴充套件。

如果該遊戲已有可自訂 UI 附加元件或主題，則應用程式延伸模組會是不錯的選擇，因為提供延伸模組的應用程式無法獨自執行，而且任何協力廠商都能建立這些延伸模組。

## <a name="remarks"></a>備註

本主題提供應用程式延伸模組的簡介。 要注意的重點是，建立主機並於 Package.appxmanifest 檔案中依此標示；建立延伸模組並於 Package.appxmanifest 檔案中依此標示；決定實作延伸模組的方式 (例如應用程式服務、背景工作或開啟方式)、定義主機與延伸模組的通訊方式；以及使用 [AppExtensions API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) 來存取和管理延伸模組。

## <a name="related-topics"></a>相關主題

* [應用程式延伸模組簡介](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [關於應用程式延伸的組建 2016 工作階段](https://channel9.msdn.com/Events/Build/2016/B808)
* [組建 2016 應用程式延伸模組程式碼範例](https://github.com/Microsoft/App-Extensibility-Sample)
* [使用背景工作支援應用程式](support-your-app-with-background-tasks.md)
* [如何建立和使用應用程式服務](how-to-create-and-consume-an-app-service.md)。
* [AppExtensions 命名空間](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [透過服務、延伸模組和套件擴充您的應用程式](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)
