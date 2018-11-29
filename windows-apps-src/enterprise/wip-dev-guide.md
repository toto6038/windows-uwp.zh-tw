---
Description: This guide helps you enlighten your app to handle enterprise data managed by Windows Information Protection (WIP) policy as well as personal data.
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows 資訊保護 (WIP) 開發人員指南
ms.date: 06/21/2017
ms.topic: article
keywords: Windows 10, uwp, wip, Windows 資訊保護, 企業資料, 企業資料保護, edp, 啟發式應用程式
ms.assetid: 913ac957-ea49-43b0-91b3-e0f6ca01ef2c
ms.localizationpriority: medium
ms.openlocfilehash: 229d97c137344de26be0168be437825bea8e9700
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8192773"
---
# <a name="windows-information-protection-wip-developer-guide"></a>Windows 資訊保護 (WIP) 開發人員指南

*啟發式*應用程式可區別公司和個人資料，並根據系統管理員定義的 Windows 資訊保護 (WIP) 原則判斷應保護的資料。

在本指南中，我們將說明如何建置啟發式應用程式。 當您完成後，原則系統管理員將會信任並允許您的應用程式使用其組織資料。 而員工最愛的是，就算他們從組織的行動裝置管理 (MDM) 取消註冊或是完全離該組織，其裝置上的個人資料也能完好如初。

__注意__：本指南可協助您啟發 UWP app。 如果您想要啟發 C++ Windows 傳統型應用程式，請參閱 [Windows 資訊保護 (WIP) 開發人員指南 (C++)](http://go.microsoft.com/fwlink/?LinkId=822192)。

如需 WIP 與啟發式應用程式的詳細資訊，請參閱︰[Windows 資訊保護 (WIP)](wip-hub.md)。


            [您可以在](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)找到完整的範例。

如果您已準備好進行每項工作，我們就開始吧。

## <a name="first-gather-what-you-need"></a>首先請備妥您需要的項目：

您將需要：

* 執行 Windows 10 (1607 版或更新版本) 的測試虛擬機器 (VM)。 這個測試 VM 將用來對您的應用程式偵錯。

* 執行 Windows10 (1607 版或更新版本) 的開發電腦。 這可能是您的測試 VM (如果您有在其中安裝 Visual Studio)。

## <a name="setup-your-development-environment"></a>設定開發環境

您將執行下列工作︰

* [在測試 VM 上安裝「WIP 設定開發人員小幫手」](#install-assistant)

* [使用「WIP 設定開發人員小幫手」建立保護原則](#create-protection-policy)

* [設定 Visual Studio 專案](#setup-vs-project)

* [設定遠端偵錯](#setup-remote-debugging)

* [將命名空間新增到程式碼檔案](#add-namespaces)

<a id="install-assistant" />

### <a name="install-the-wip-setup-developer-assistant-onto-your-test-vm"></a>在測試 VM 上安裝「WIP 設定開發人員小幫手」

 使用此工具在您的測試 VM 上設定 Windows 資訊保護原則。

 請在此下載工具︰[WIP 設定開發人員小幫手](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf)。

<a id="create-protection-policy" />

### <a name="create-a-protection-policy"></a>建立保護原則

藉由將資訊新增到 WIP 設定開發人員小幫手的每個區段以定義您的原則。 選擇任一設定旁的 [說明] 圖示，以了解如何使用它 。

如需更多如何使用此工具的一般指導方針，請參閱應用程式下載頁面上的＜版本資訊＞一節。

<a id="setup-vs-project" />

### <a name="setup-a-visual-studio-project"></a>設定 Visual Studio 專案

1. 在開發電腦上開啟您的專案。

2. 針對通用 Windows 平台 (UWP)，加入對桌面和行動延伸模組的參考。

    ![新增 UWP 延伸模組](images/extensions.png)

3. 將這個功能新增到您的封裝資訊清單檔案︰

    ```xml
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*選擇性閱讀*："rescap" 前置詞表示 *「受限制的功能」*。 請參閱[特殊和受限制的功能](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

4. 將這個命名空間新增到您的封裝資訊清單檔案︰

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. 將命名空間前置詞新增到封裝資訊清單檔案的 ``<ignorableNamespaces>`` 元素。

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    如此一來，如果您的應用程式在不支援受限制功能的 Windows 作業系統版本上執行，Windows 將會忽略 ``enterpriseDataPolicy`` 功能。

<a id="setup-remote-debugging" />

### <a name="setup-remote-debugging"></a>設定遠端偵錯

只有當您在 VM 以外的電腦上開發應用程式時，才需在您的測試 VM 上安裝 Visual Studio 遠端工具。 然後，在開發電腦上啟動遠端偵錯工具，並查看您的應用程式是否在測試 VM 上執行。

請參閱[遠端電腦指示](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#remote-pc-instructions)。

<a id="add-namespaces" />

### <a name="add-these-namespaces-to-your-code-files"></a>將這些命名空間新增到程式碼檔案

將這些 using 陳述式新增到程式碼檔案的上方 (本指南中的程式碼片段會使用它們)：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml;
using Windows.ApplicationModel.Activation;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Data.Xml.Dom;
using Windows.Foundation.Metadata;
using Windows.Web.Http.Headers;
```

## <a name="determine-whether-to-use-wip-apis-in-your-app"></a>判斷是否要在 App 中使用 WIP API

確定執行您的 App 的作業系統支援 WIP 且裝置上的 WIP 已啟用。

```csharp
bool use_WIP_APIs = false;

if ((ApiInformation.IsApiContractPresent
    ("Windows.Security.EnterpriseData.EnterpriseDataContract", 3)
    && ProtectionPolicyManager.IsProtectionEnabled))
{
    use_WIP_APIs = true;
}
else
{
    use_WIP_APIs = false;
}
```
如果作業系統不支援 WIP 且裝置上的 WIP 未啟用，請勿呼叫 WIP API。

## <a name="read-enterprise-data"></a>讀取企業資料

若要讀取受保護的檔案、網路端點、剪貼簿資料以及接受的分享協定資料，您的 App 必須要求存取權。

如果您的 App 列於保護原則的允許清單中，Windows 資訊保護就會授與您的 App 權限。

**本節內容：**

* [從檔案讀取資料](#read-file)
* [從網路端點讀取資料](#read-network)
* [從剪貼簿讀取資料](#read-clipboard)
* [從分享協定讀取資料](#read-share)

<a id="read-file" />

### <a name="read-data-from-a-file"></a>從檔案讀取資料

**步驟 1︰ 取得檔案控制代碼**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**步驟 2︰ 判斷您的應用程式是否可以開啟檔案**

呼叫 [FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx) 以判斷您的 App 是否可以開啟檔案。

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if ((protectionInfo.Status != FileProtectionStatus.Protected &&
    protectionInfo.Status != FileProtectionStatus.Unprotected))
{
    return false;
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```

[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx) 值為 **Protected**，即表示檔案已受保護，而且您的 App 可以開啟該檔案，因為 App 列在原則的允許清單上。

[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx) 值為 **UnProtected**，即表示檔案未受保護，縱使您的 App 不在原則的允許清單上，App 也能開啟該檔案。

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx)<br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)

**步驟 3︰將檔案讀到資料流或緩衝區**

*將檔案讀到資料流*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*將檔案讀到緩衝區*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```
<a id="read-network" />

### <a name="read-data-from-a-network-endpoint"></a>從網路端點讀取資料

建立要從企業端點讀取的受保護執行緒內容。

**步驟 1︰取得網路端點的身分識別**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

如果端點不受原則管理，則會傳回空字串。

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)


**步驟 2︰建立受保護的執行緒內容**

如果端點由原則管理，請建立受保護的執行緒內容。 這樣會將您在同一個執行緒上建立的任何網路連線標記到身分識別。

它也可以讓您存取該原則所管理的企業網路資源。

```csharp
if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
    }
}
else
{
    return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
}
```
這個範例將通訊端呼叫括在 ``using`` 區塊中。 如果您不要這樣做，請務必在您擷取資源之後結束執行緒內容。 請參閱 [ThreadNetworkContext.Close](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.threadnetworkcontext.close.aspx)。

請勿在受保護的執行緒上建立任何個人檔案，因為這些檔案會自動加密。

無論端點是否由原則管理，[**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx) 方法一律會傳回 [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.threadnetworkcontext.aspx) 物件。 如果您的應用程式同時會處理個人和企業資源，請呼叫 [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx) 以取得所有身分識別。  取得資源之後，請捨棄 ThreadNetworkContext 以清除來自目前執行緒的任何身份識別標記。

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)

**步驟 3︰將資源讀到緩衝區**

```csharp
private static async Task<IBuffer> GetDataFromNetworkHelperMethod(Uri resourceURI)
{
    HttpClient client;

    client = new HttpClient();

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**(選擇性) 使用標頭語彙基元，而不建立受保護的執行緒內容**

```csharp
public static async Task<IBuffer> GetDataFromNetworkbyUsingHeader(Uri resourceURI)
{
    HttpClient client;

    Windows.Networking.HostName hostName =
        new Windows.Networking.HostName(resourceURI.Host);

    string identity = await ProtectionPolicyManager.
        GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

    if (!string.IsNullOrEmpty(identity))
    {
        client = new HttpClient();

        HttpRequestHeaderCollection headerCollection = client.DefaultRequestHeaders;

        headerCollection.Add("X-MS-Windows-HttpClient-EnterpriseId", identity);

        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }
    else
    {
        client = new HttpClient();
        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }

}

private static async Task<IBuffer> GetDataFromNetworkbyUsingHeaderHelperMethod(HttpClient client, Uri resourceURI)
{

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**處理頁面重新導向**

有時候，網頁伺服器會將流量重新導向至較新版本的資源。

為因應這種情況，請在要求的回應狀態值為**確定**時再提出要求。

接著再使用該回應的 URI 來取得端點的身分識別。 以下是其中一種做法：

```csharp
private static async Task<IBuffer> GetDataFromNetworkRedirectHelperMethod(Uri resourceURI)
{
    HttpClient client = null;

    HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
    filter.AllowAutoRedirect = false;

    client = new HttpClient(filter);

    HttpResponseMessage response = null;

        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

    if (response.StatusCode == HttpStatusCode.MultipleChoices ||
        response.StatusCode == HttpStatusCode.MovedPermanently ||
        response.StatusCode == HttpStatusCode.Found ||
        response.StatusCode == HttpStatusCode.SeeOther ||
        response.StatusCode == HttpStatusCode.NotModified ||
        response.StatusCode == HttpStatusCode.UseProxy ||
        response.StatusCode == HttpStatusCode.TemporaryRedirect ||
        response.StatusCode == HttpStatusCode.PermanentRedirect)
    {
        message = new HttpRequestMessage(HttpMethod.Get, message.RequestUri);
        response = await client.SendRequestAsync(message);

        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
    else
    {
        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
}

```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

<a id="read-clipboard" />

### <a name="read-data-from-the-clipboard"></a>從剪貼簿讀取資料

**取得使用剪貼簿資料的權限**

若要從剪貼簿取得資料，請向 Windows 要求權限。 請使用 [**DataPackageView.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx) 執行此作業。

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **API** <br>
[DataPackageView.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx)

**隱藏或停用使用剪貼簿資料的功能**

判斷目前的檢視是否有權限取得剪貼簿上的資料。

如果沒有，您可以停用或隱藏讓使用者從剪貼簿貼上資訊或預覽其內容的控制項。

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **API** <br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

**避免提示使用者同意對話方塊**

有一份既不是 *「個人」* 也不是 *「企業」* 的新文件。 它只是一份新文件。 如果使用者將企業資料貼入該文件，Windows 會強制執行原則，並提示使用者同意對話方塊。 這個程式碼可避免此狀況。 這項工作與保護資料無關。 而是關於在應用程式建立全新項目時，避免讓使用者接收同意對話方塊。

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **API** <br>
[DataPackageView.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx)<br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

<a id="read-share" />

### <a name="read-data-from-a-share-contract"></a>從分享協定讀取資料

當員工選擇您的應用程式來分享資訊時，您的應用程式會開啟一個包含該內容的新項目。

如先前所述，新項目不是 *「個人」* 或 *「企業」* 文件。 它只是一份新文件。 如果您的程式碼會將企業內容新增至該項目，Windows 會強制執行原則，並提示使用者同意對話方塊。 這個程式碼可避免此狀況。

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **API** <br>
[ProtectionPolicyManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn705789.aspx)<br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

## <a name="protect-enterprise-data"></a>保護企業資料

保護離開應用程式的企業資料。 當您在頁面中顯示資料、將資料儲存至檔案或網路端點或是透過分享協定時，資料會離開您的應用程式。

**本節內容：**

* [保護頁面中出現的資料](#protect-pages)
* [以背景處理程序保護寫入檔案的資料](#protect-background)
* [保護檔案的一部分](#protect-part-file)
* [讀取檔案受保護的部分](#read-protected)
* [保護寫入資料夾的資料](#protect-folder)
* [保護寫入網路端點的資料](#protect-network)
* [保護您的應用程式透過分享協定共用的資料](#protect-share)
* [保護您複製到其他位置的檔案](#protect-other-location)
* [於裝置畫面鎖定時保護企業資料](#protect-locked)

<a id="protect-pages" />

### <a name="protect-data-that-appears-in-pages"></a>保護頁面中出現的資料

當您在頁面中顯示資料時，請讓 Windows 知道是什麼類型的資料 (個人或企業)。 若要這樣做，請 *「標記」* 目前的應用程式檢視，或標記整個應用程式處理程序。

當您標記檢視或處理程序時，Windows 會對它強制執行原則。 這有助於防止因為您的應用程式無法控制的動作所造成的資料遺漏。 例如在電腦上，使用者可以使用 CTRL-V 來複製檢視中的企業資訊，然後將該資訊貼到另一個應用程式。 Windows 會提供針對該動作的防護。 Windows 也可強制執行分享協定。

**標記目前的應用程式檢視。**

如果您的應用程式有多個檢視，其中的某些檢視使用企業資料，某些使用個人資料，則請執行這項操作。

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty;
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

**標記處理程序**

如果您的應用程式中的所有檢視只會使用一種類型的資料 (個人或企業)，請執行這項操作。

這樣您就不需要管理單獨標記的檢視。

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
ProtectionPolicyManager.ClearProcessUIPolicy();
```

> **API** <br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

<a id="protect-file" />

### <a name="protect-data-to-a-file"></a>保護寫入檔案的資料

建立受保護的檔案，然後寫入資料。

**步驟 1︰判斷您的應用程式是否可以建立企業檔案**

如果身分識別字串由原則管理，而且您的應用程式在該原則的允許清單中，您的應用程式就可以建立企業檔案。

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)


**步驟 2︰建立檔案，並保護身分識別**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **API** <br>
[FileProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.protectasync.aspx)

**步驟 3︰將該資料流或緩衝區寫入檔案**

*寫入資料流*

```csharp
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*寫入緩衝區*

```csharp
     if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **API** <br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>

<a id="protect-background" />

### <a name="protect-data-to-a-file-as-a-background-process"></a>以背景處理程序保護寫入檔案的資料

當裝置的畫面被鎖定時，可以執行這個程式碼。 如果系統管理員已設定「鎖定時的資料保護」(DPL) 原則，Windows 會移除從裝置記憶體存取受保護資源所需的加密金鑰。 這可防止裝置遺失時的資料遺漏問題。 這個功能也會在受保護檔案的控制代碼被關閉時，移除與受保護檔案關聯的金鑰。

您必須使用一種方法，以在建立檔案時讓檔案控制代碼保持開啟。  

**步驟 1︰判斷您是否可以建立企業檔案**

如果您使用的身分識別字串由原則管理，而且您的應用程式在該原則的允許清單中，您就可以建立企業檔案。

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)

**步驟 2︰建立檔案，並保護身分識別**


            [
              **FileProtectionManager.CreateProtectedAndOpenAsync**
            ](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync.aspx) 會建立一個受保護的檔案，並在您寫入該檔案時讓檔案控制代碼保持開啟。

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **API** <br>
[FileProtectionManager.CreateProtectedAndOpenAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync.aspx)

**步驟 3︰將資料流或緩衝區寫入檔案**

這個範例會將資料流寫入檔案。

```csharp
if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **API** <br>
[ProtectedFileCreateResult.ProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>
[ProtectedFileCreateResult.Stream](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedfilecreateresult.stream.aspx)<br>

<a id="protect-part-file" />

### <a name="protect-part-of-a-file"></a>保護檔案的一部分

在大部分情況下，個別儲存企業和個人資料會較為井然有序，但您也可以將它們儲存到同一個檔案。 例如，Microsoft Outlook 可以將企業郵件連同個人郵件儲存在單一封存檔案中。

請加密企業資料，而非整個檔案。 如此一來，即使使用者在 MDM 中取消註冊或企業資料的存取權限被撤銷，也能繼續使用該檔案。 此外，您的應用程式應記錄它所加密的資料，以分辨將檔案讀回記憶體時要保護的資料。

**步驟 1︰將企業資料新增到加密的資料流或緩衝區**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **API** <br>
[DataProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.protectasync.aspx)<br>
[BufferProtectUnprotectResult.buffer](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.buffer.aspx)


**步驟 2︰將個人資料新增到未加密的資料流或緩衝區**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**步驟 3︰將資料流或緩衝區寫入檔案**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**步驟 4︰記錄您的企業資料在檔案中的位置**

您的應用程式應記錄該檔案中由企業擁有的資料。

您可以將該資訊儲存在與檔案關聯的屬性中、資料庫內或檔案的一部分標頭文字中。

這個範例會將該資訊儲存到不同的 XML 檔案。

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```
<a id="read-protected" />

### <a name="read-the-protected-part-of-a-file"></a>讀取檔案受保護的部分

以下是從該檔案讀取企業資料的方式。

**步驟 1︰取得您的企業資料在檔案中的位置**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**步驟 2︰開啟資料檔案，確定資料未受保護**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx)<br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>

**步驟 3：從檔案讀取企業資料**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**步驟 4︰解密包含企業資料的緩衝區**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **API** <br>
[DataProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectioninfo.aspx)<br>
[DataProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync.aspx)<br>

<a id="protect-folder" />

### <a name="protect-data-to-a-folder"></a>保護寫入資料夾的資料

您可以建立一個受保護的資料夾。 如此一來，您新增至該資料夾的任何項目都會自動受到保護。

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

保護資料夾之前，請確定該資料夾是空的。 您無法保護已包含項目的資料夾。

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)<br>
[FileProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.protectasync.aspx)<br>
[FileProtectionInfo.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.identity.aspx)<br>
[FileProtectionInfo.Status](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.status.aspx)

<a id="protect-network" />

### <a name="protect-data-to-a-network-end-point"></a>保護寫入網路端點的資料

建立受保護的執行緒內容，將該資料傳送到企業端點。  

**步驟 1︰取得網路端點的身分識別**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)

**步驟 2︰建立受保護的執行緒內容，並將資料傳送到網路端點**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)

<a id="protect-share" />

### <a name="protect-data-that-your-app-shares-through-a-share-contract"></a>保護您的應用程式透過分享協定共用的資料

如果您要讓使用者透過您的應用程式分享內容，您必須實作分享協定和處理 [**DataTransferManager.DataRequested**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) 事件。

在您的事件處理常式中，設定資料套件中的企業身分識別內容。

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

<a id="protect-other-location" />

### <a name="protect-files-that-you-copy-to-another-location"></a>保護您複製到其他位置的檔案

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **API** <br>
[FileProtectionManager.CopyProtectionAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync.aspx)<br>

<a id="protect-locked" />

### <a name="protect-enterprise-data-when-the-screen-of-the-device-is-locked"></a>於裝置畫面鎖定時保護企業資料

當裝置鎖定時，移除記憶體中所有的敏感資料。 當使用者解除鎖定裝置後，您的應用程式可以安全地重新加回該資料。

處理 [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending.aspx) 事件，使您的應用程式知道螢幕何時鎖定。 只有當系統管理員根據鎖定原則設定安全的資料保護時，才會引發這個事件。 Windows 會暫時移除裝置上佈建的資料保護金鑰。 當裝置被鎖定，而且可能不在其擁有者手上時，Windows 會移除這些金鑰，以確保無法對加密資料進行未授權的存取。  

處理 [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx) 事件，使您的應用程式知道螢幕何時解除鎖定。 無論系統管理員是否根據鎖定原則設定安全的資料保護，都會引發這個事件。

#### <a name="remove-sensitive-data-in-memory-when-the-screen-is-locked"></a>當螢幕鎖定時，移除記憶體中的敏感資料

保護敏感資料，並關閉您的應用程式在受保護的檔案上開啟的任何檔案資料流，以協助確保系統不會快取記憶體中的所有敏感資料。

這個範例會將文字區塊中的內容儲存至加密的緩衝區，並移除該文字區塊的內容。

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessSuspending](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)</br>
[DataProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.protectasync.aspx)<br>
[BufferProtectUnprotectResult.buffer](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.buffer.aspx)<br>
[ProtectedAccessSuspendingEventArgs.GetDeferral](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral.aspx)<br>
[Deferral.Complete](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx)<br>

#### <a name="add-back-sensitive-data-when-the-device-is-unlocked"></a>當裝置解除鎖定後，將敏感資料重新加回

當裝置解除鎖定，而且裝置上的金鑰又再次可用時，就會引發 [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx)。

如果系統管理員未根據鎖定原則設定安全的資料保護，[**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedaccessresumedeventargs.identities.aspx) 會是空集合。

這個範例與前面的範例相反。 它會解密緩衝區，將該緩衝區中的資訊重新加回文字區塊，然後捨棄該緩衝區。

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessResumed](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)</br>
[DataProtectionManager.UnprotectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.unprotectasync.aspx)<br>
[BufferProtectUnprotectResult.Status](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.aspx)<br>

## <a name="handle-enterprise-data-when-protected-content-is-revoked"></a>在受保護的內容被撤銷時處理企業資料

如果您想要您的應用程式在裝置從 MDM 取消註冊時收到通知，或是原則系統管理員明確撤銷企業資料的存取權時，請處理 [**ProtectionPolicyManager_ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked.aspx) 事件。

這個範例會判斷是否已撤銷電子郵件應用程式的企業信箱資料。

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **API** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked.aspx)<br>

## <a name="related-topics"></a>相關主題

[Windows 資訊保護 (WIP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)
