---
author: eliotcowley
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: "本文章說明如何將包含 Microsoft PlayReady 內容保護的多媒體內容彈性資料流新增到通用 Windows 平台 (UWP) app。"
title: "搭配使用彈性資料流與 PlayReady"
translationtype: Human Translation
ms.sourcegitcommit: 176f8989aea5402106e3c14144948cec87a5dc27
ms.openlocfilehash: d76f50e97f16699f34f138fcd25af8a90696085a

---

# 搭配使用彈性資料流與 PlayReady

\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文章說明如何將包含 Microsoft PlayReady 內容保護的多媒體內容彈性資料流新增到通用 Windows 平台 (UWP) app。 

本功能目前支援播放 DASH (Dynamic Adaptive Streaming over HTTP) 內容。

PlayReady 不支援 HLS (Apple 的 HTTP 即時資料流)。

目前也不支援原生的 Smooth Streaming，不過，PlayReady 可以使用額外的程式碼或程式庫來延伸，因此可支援 PlayReady 保護的 Smooth Streaming，以利用軟體或甚至硬體 DRM (數位版權管理)。

這篇文章僅處理 PlayReady 特定的彈性資料流層面。 如需實作彈性資料流的一般資訊，請參閱[彈性資料流](adaptive-streaming.md)。

本文使用的程式碼來自 Microsoft 在 GitHub 上的 **Windows-universal-samples** 存放庫中的 [Adaptive streaming sample (彈性資料流範例)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming)。 Scenario 4 (案例 4) 會示範搭配 PlayReady 使用彈性資料流。 您可以將存放庫以 ZIP 檔案格式下載，方法是瀏覽到存放庫的根目錄，然後按一下 [Download ZIP] (下載 ZIP)**** 按鈕。

您將需要下列 using 陳述式：

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

**LicenseRequest** 命命空間是來自 **CommonLicenseRequest.cs** (Microsoft 提供給使用人的 PlayReady 檔案)。

您將需要宣告幾個全域變數：

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

您也會想要宣告下列常數：

```csharp
private const uint MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = 0x8004B895;
```

## 設定 MediaProtectionManager

若要將 PlayReady 內容保護新增到您的 UWP app，您將需要設定 [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040) 物件。 您會在初始化 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 物件時設定。

下列程式碼會設定 [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040)：

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

直接將這個程式碼複製到您的應用程式即可，因為需要它才能設定內容保護。

二進位資料載入失敗時會觸發 [ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041) 事件。 我們需要新增事件處理常式來處理此事件，發送表示載入未完成的訊號：

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

同樣地，我們需要為要求服務時觸發的 [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045) 事件新增事件處理常式。 這個程式碼會檢查要求的類型，並適當地回應：

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## 個人化服務要求

下列程式碼會被動發出 PlayReady 個人化服務要求。 我們將要求以參數方式傳遞給函式。 我們將呼叫以 try/catch 區塊括住，如果沒有出現例外狀況，要求便視為成功：

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null && comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

或者，我們可能想要主動發出個人化服務要求，這種情況下我們會呼叫以下的函式來取代呼叫 `ProtectionManager_ServiceRequested` 中之 `ReactiveIndivRequest` 的程式碼：

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## 授權取得服務要求

如果要求是 [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285)，則我們將呼叫以下函式來要求並取得 PlayReady 授權。 我們向 MediaProtectionServiceCompletion 物件告知我們傳入的要求是否成功，然後完成該要求：

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## 初始化 AdaptiveMediaSource

最後，您將需要一個函式來初始化 [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912)，可從已知的 [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 和 [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) 來建立。 **Uri** 應為連結至媒體檔案 (HLS 或 DASH) 的連結；**MediaElement** 應在您的 XAML 中定義。

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

您可以在任何處理啟動彈性資料流的事件中呼叫這個函式—例如，在按鈕點擊事件中。

 

 







<!--HONumber=Jun16_HO4-->


