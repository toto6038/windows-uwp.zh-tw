---
author: TylerMSFT
Description: '本主題說明達成一些最常見的串流和緩衝區相關企業資料保護 EDP 案例所需的編碼工作範例。'
MS-HAID: 'dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: '使用企業資料保護 (EDP) 來保護串流和緩衝區'
---

# 使用企業資料保護 (EDP) 來保護串流和緩衝區

__注意__：企業資料保護 (EDP) 原則無法套用於 Windows 10 1511 版 (組建 10586) 或更早版本。

本主題說明達成一些最常見的串流和緩衝區相關企業資料保護 EDP 案例所需的編碼工作範例。 如需 EDP 如何與檔案、串流、剪貼簿、網路、背景工作及鎖定時的資料保護產生關係的完整開發人員說明，請參閱[企業資料保護 (EDP)](../enterprise/edp-hub.md)。

**注意**：[企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵蓋許多與本主題所示範的案例相同的案例。

## 先決條件


-   **設定 EDP**

    請參閱[設定您的電腦使用 EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力於建置企業開明 app**

    可自主確保企業資料受企業管控的 app，稱之為企業開明 app。 開明應用程式比非開明應用程式的功能更強、更有智慧、更有彈性且更值得信任。 您的應用程式會透過宣告限制的 **enterpriseDataPolicy** 功能，以向系統宣告它是開明應用程式。 但是，比起設定功能，還有更多開明的行為。 若要深入了解，請參閱[企業開明 app](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) app 的非同步程式設計**

    若要了解如何使用 C# 或 Visual Basic 撰寫非同步的應用程式，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 撰寫非同步的應用程式，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 保護企業身分識別的資料流


**注意：**每當您保護串流或緩衝區時，強烈建議您訂閱 [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411) 事件，以便讓您的應用程式在 EDP 於裝置上變成停用時能夠得知。 發生這種情況時，您應該取消對所有串流和緩衝區的保護。 如果使用者將裝置從行動裝置管理 (MDM) 中取消註冊，所有保留在受保護狀態的串流或緩衝區都會變成可供撤銷。 而如果在建立資源時已停用 EDP，該撤銷作業便不恰當。 在已停用 EDP 的情況下，取消資源保護即可避免這種情況。



在這個案例中，您的 app 可以存取包含企業資料的受保護串流。 為了確保在於裝置內部和外部傳輸此串流時，此串流會受到保護，您的 app 會保護資料到其所屬的企業身分識別。 這會允許企業在必要時抹除該資料。 為了在稍後決定是否要在串流上呼叫 "unprotect" 方法，app 必須維護指出串流是否受到保護的狀態，這也就是為什麼此程式碼範例中的函式會傳回該狀態的原因。 如果所傳遞的身分識別未受管理，或 app 不是該身分識別所允許的 app，串流將不會受到保護，而呼叫 [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021) 時就會傳回 ‘Unprotected’ 狀態。

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

為了示範如何使用類似上方程式碼範例中的方法，以下是一個協助程式方法，此方法會使用它將字串轉換成未受保護的串流，然後再保護該串流。

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## 抓取串流的狀態


在這個案例中，您的 app 先前已保護一個您必須防止未經授權存取的串流。 為了在需要時抓回串流的內容，您的 app 可以檢查串流的狀態。

請注意，串流的狀態也會從 [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) 傳回。 這個 API 永遠不會傳回 ‘Unprotected’ 的狀態，因為它要求輸入資源必須受到保護 (不可能可靠地確認資源未受保護)。 請注意，如果您有會比較含有 'Unprotected' 之結果的程式碼，則暗示有設計上的瑕疵存在。 這表示您的程式碼已失去串流是否受到保護的線索。

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## 取消資料流保護


在這個案例中，您的 app 想要將先前所保護的串流取消保護。 這個程式碼範例會拿取一個受保護的串流 (請注意，此串流必須受到保護，這個程序才能運作)，然後呼叫 [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) 來將它取消保護。 它會接著從該串流讀取一個字串並將其傳回。

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

為了示範如何使用到目前為止所指定的協助程式方法，以下是一個處理常式，此處理常式會從文字方塊拿取一個字串、將該字串寫入串流中、保護該串流、將該串流取消保護 (如果已成功保護)，最後再從未受保護的串流中讀回該字串並顯示在 UI 中。

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## 抓取靜態資料緩衝區的狀態


在這個案例中，您的 app 先前已保護一個您必須防止未經授權存取的緩衝區。 為了在需要時抓回緩衝區的內容，您的 app 可以檢查緩衝區的狀態。

請注意，緩衝區的狀態也會從 [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022) 傳回。 這個 API 永遠不會傳回 ‘Unprotected’ 的狀態，因為它要求輸入資源必須受到保護。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## 保護從企業資源抓取的靜態資料


這個案例的觀點與串流程式碼範例相同，不同之處在於它處理的是資料緩衝區。 同樣地，您也需要維護指出緩衝區是否受到保護的狀態，如所示。 如果所傳遞的身分識別未受管理，或應用程式不是該身分識別所允許的應用程式，緩衝區將不會受到保護，而呼叫 [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020) 時就會傳回 ‘Unprotected’ 狀態。

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## 根據串流或緩衝區的受保護身分識別來啟用 UI 原則強制執行


當您的應用程式即將在其 UI 上顯示受保護串流或緩衝區的內容時，它必須根據資源保護對象的身分識別，啟用 UI 原則強制執行。 您應查詢資源的保護資訊，然後從擷取到的身分識別啟用系統的 UI 原則強制執行。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // 'Unprotected' is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```

**注意**：本文適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相關主題


[企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 命名空間**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=May16_HO2-->


