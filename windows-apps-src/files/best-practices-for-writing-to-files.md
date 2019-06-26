---
title: 寫入檔案的最佳做法
description: 了解使用 FileIO 和 PathIO 類別的各種檔案寫入方法的最佳做法。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a6a1d93b1deaad084ff25db946199b678b35703c
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369515"
---
# <a name="best-practices-for-writing-to-files"></a>寫入檔案的最佳做法

**重要 API**

* [**FileIO 類別**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 類別**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

開發人員在使用 [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 類別的 **Write** 方法來執行檔案系統 I/O 作業時，有時候會遇到一些常見問題。 例如，常見問題包括：

* 部分寫入檔案。
* 呼叫其中一個方法時，應用程式會收到例外狀況。
* 作業會留下檔案名稱類似於目標檔案名稱的 .TMP 檔案。

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 類別的 **Write** 方法包括下列項目：

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 本文提供有關這些方法的運作方式詳細資訊，讓開發人員能進一步了解何時及如何使用它們。 本文提供指導方針，並不會嘗試為所有可能的檔案 I/O 問題提供解決方案。 

> [!NOTE]
> 本文著重於範例和討論中的 **FileIO**方法。 不過，**PathIO** 方法會遵循類似的模式，而本文中大部分的指引適用於這些方法。 

## <a name="convenience-vs-control"></a>方便與控制

[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 物件不是像原生 Win32 程式設計模型的檔案控制代碼。 然而，[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 是具有方法可操作其內容的檔案表示法。

執行 **StorageFile** 的 I/O 時，了解這個概念很有用。 例如，[寫入至檔案](quickstart-reading-and-writing-files.md#writing-to-a-file)一節會顯示三種寫入檔案的方式：

* 使用 [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) 方法。
* 建立緩衝區，然後呼叫 [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) 方法。
* 使用資料流的四個步驟模型：
  1. [開啟](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync)檔案以取得資料流。
  2. [取得](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)輸出資料流。
  3. 建立 [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)物件並呼叫對應的 **Write** 方法。
  4. [認可](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)資料寫入器中的資料並[排清](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)輸出資料流。

前兩個案例是應用程式最常使用的案例。 在單一作業中寫入檔案，比較容易維護程式碼，而且也讓應用程式免於承擔處理許多複雜檔案 I/O 的責任。 不過，這種便利性是要付出代價的：失去整個作業的控制權，以及在特定時間點擷取錯誤的能力。

## <a name="the-transactional-model"></a>交易模型

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 類別的 **Write** 方法會包裝上述第三個寫入方法的步驟，並新增一層。 這一層會封裝在儲存體交易中。

如果在寫入資料時發生錯誤，為了保護原始檔的完整性，**Write** 方法使用 [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync) 開啟此檔案，以便使用交易模型。 此程序會建立 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 物件。 建立此交易物件之後，API 會將樣式類似的資料寫入 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 一文中的[檔案存取](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)範例或程式碼範例。

下圖說明 **WriteTextAsync** 方法在成功的寫入作業中所執行的基礎工作。 此圖提供作業的簡化檢視。 例如，它會略過一些步驟，例如不同執行緒上的文字編碼和非同步完成。

![寫入檔案的 UWP API 呼叫順序圖](images/file-write-call-sequence.svg)

使用 [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 類別的 **Write** 方法，而不使用更複雜的四步驟模型的優點如下：

* 一個 API 呼叫可處理所有中繼步驟，包括錯誤。
* 如果發生錯誤，則會保留原始檔案。
* 系統狀態會試著盡可能保持乾淨。

不過，有這麼多可能的中繼失敗點，失敗機會就會提高。 發生錯誤時，可能難以了解程序失敗的位置。 下列各節呈現在使用 **Write** 方法時可能遭遇的一些失敗，並提供可能的解決方案。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 和 PathIO 類別的 Write 方法常見的錯誤碼

此表格顯示應用程式開發人員在使用 **Write** 方法時遇到的常見錯誤碼。 表格中的步驟會對應到上圖中的步驟。

|  錯誤名稱 (值)  |  步驟  |  原因  |  解決方案  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  原始檔案可能會標示為要刪除 (可能在前一項作業中)。  |  重試作業。</br>確定檔案的存取會同步處理。  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  另一項獨佔寫入會開啟原始檔案。   |  重試作業。</br>確定檔案的存取會同步處理。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  無法取代原始檔案 (file.txt)，因為它正在使用中。 在可以取代檔案之前，另一個程序或作業取得了其存取權。  |  重試作業。</br>確定檔案的存取會同步處理。  |
|  ERROR_DISK_FULL (0x80070070)  |  7、14、16、20  |  此交易模型會建立一個額外的檔案，而這會取用額外的儲存體。  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14、16  |  由於多項未處理的 I/O 作業或大型的檔案大小，而可能發生這錯誤。  |  更細微的資料流控制方法可能解決此錯誤。  |
|  E_FAIL (0x80004005) |  任何值  |  其他  |  重試作業。 如果仍然失敗，有可能是平台錯誤，而應用程式因為處於不一致狀態而終止。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>檔案狀態的其他考量可能會導致錯誤

除了 **Write** 方法所傳回的錯誤，以下是應用程式寫入檔案時預期狀況的一些指導方針。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>作業完成後，資料才會寫入檔案

寫入作業正在進行時，您的應用程式不得對檔案中的資料做出任何相關假設。 嘗試在作業完成前存取檔案，可能會導致資料不一致。 您的應用程式應負責追蹤未處理的 I/O。

### <a name="readers"></a>讀取者

如果正常的讀取器也在使用正在寫入的檔案 (亦即，開啟方式為 [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode))，則後續讀取會失敗並出現錯誤 EERROR_OPLOCK_HANDLE_CLOSED (0x80070323)。 當 **Write** 作業正在進行時，應用程式有時會嘗試重新開啟檔案以便再次讀取。 這可能會造成競爭情況，以致 **Write** 最終在嘗試覆寫原始檔案時因為無法取代該檔案而失敗。

### <a name="files-from-knownfolders"></a>KnownFolders 中的檔案

您的應用程式可能不是唯一嘗試存取任何 [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 上檔案的應用程式。 如果作業成功，不保證應用程式下次嘗試讀取檔案時，其寫入檔案的內容會保持一致。 此外，在這種情況下，共用或存取拒絕錯誤變得更常見。

### <a name="conflicting-io"></a>I/O 衝突

如果我們的應用程式針對其本機資料中的檔案使用 **Write** 方法，則可能降低並行錯誤的機會，但仍需小心謹慎。 如果有多項 **Write** 作業同時傳送至檔案，則無法保證檔案中最後會有哪些資料。 若要緩解這種情況，我們建議您的應用程式將 **Write** 作業序列化處理至檔案。

### <a name="tmp-files"></a>~TMP 檔案

有時候，如果強制取消作業 (例如，如果 OS 暫停或終止應用程式)，則不會適當地認可或關閉交易。 這可能會留下具有 (.~TMP) 副檔名的檔案。 請考慮在處理應用程式啟動時刪除這些暫存檔 (如果它們存在於應用程式的本機資料)。

## <a name="considerations-based-on-file-types"></a>根據檔案類型的考量

某些錯誤可能因為檔案類型、其存取頻率及其檔案大小，而變得更普遍。 一般來說，您的應用程式可以存取三種類別的檔案：

* 使用者在您應用程式的本機資料夾中建立和編輯的檔案。 這些檔案只存在於您的應用程式內，且只有在使用該應用程式時才能建立和編輯。
* 應用程式中繼資料。 您的應用程式會使用這些檔案來追蹤自己的狀態。
* 檔案系統中您的應用程式已宣告存取功能的位置中的其他檔案。 這些通常位於其中一個 [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)。

您的應用程式具有前兩個檔案類別的完全控制權，因為其屬於您應用程式的套件檔案，並由您的應用程式獨佔存取。 對於最後一個類別中的檔案，您的應用程式必須注意，其他應用程式和 OS 服務可能會同時存取檔案。

視應用程式而定，檔案的存取權可隨著頻率而有所不同：

* 非常低。 這些通常是在應用程式啟動時開啟的檔案，並且會在應用程式暫停時儲存。
* 低。 這些是使用者特別採取動作 (例如儲存或載入) 的檔案。
* 中或度。 這些是應用程式必須不斷更新資料的檔案 (例如，自動儲存功能或持續中繼資料追蹤)。

對於檔案大小，請考慮下圖中 **WriteBytesAsync** 方法的效能資料。 這個圖會透過受控環境中每個檔案大小的 10000 項作業平均效能，比較完成作業所需的時間與檔案大小。

![WriteBytesAsync 效能](images/writebytesasync-performance.png)

這個圖表刻意省略 y 軸上的時間值，因為不同的硬體和組態會產生不同的絕對時間值。 不過，我們已在我們的測試中一致地觀察到這些趨勢：

* 非常小的檔案 (<= 1 MB)：完成作業的時間始終很快。
* 較大的檔案 (> 1 MB)：完成作業所需的時間會開始以指數方式增加。

## <a name="io-during-app-suspension"></a>應用程式暫停期間的 I/O

如果您想要保留狀態資訊或中繼資料，以便在後續工作階段中使用，則必須將您的應用程式設計成可處理暫停。 如需應用程式暫停的背景資訊，請參閱[應用程式生命週期](../launch-resume/app-lifecycle.md)和[這篇部落格文章](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)。

除非 OS 准許您的應用程式延長執行，否則當應用程式暫停時，它有有 5 秒的時間可釋出其所有資源並儲存其資料。 為了達到最佳的可靠性和使用者體驗，一律假設您必須處理暫停工作的時間有限。 在處理暫停工作的 5 秒鐘期間內，請記住下列指導方針：

* 嘗試將 I/O 保留為最小值，以避免排清和釋出作業所造成的競爭情況。
* 避免寫入需要數百毫秒或更多寫入時間的檔案。
* 如果您的應用程式使用 **Write** 方法，請記住這些方法需要的所有中繼步驟。

如果您的應用程式在暫停期間操作少量的狀態資料，在大部分情況下，您都可以使用 **Write** 方法來排清資料。 不過，如果您的應用程式使用大量的狀態資料，請考慮使用資料流來直接存放您的資料。 這有助於降低 **Write** 方法的交易模型所造成的延遲。 

如需範例，請參閱 [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) 範例。

## <a name="other-examples-and-resources"></a>其他範例和資源

以下是特定案例的幾個範例和其他資源。

### <a name="code-example-for-retrying-file-io-example"></a>可供重試檔案 I/O 範例的程式碼範例

以下是假設在使用者挑選檔案以供儲存之後進行寫入，有關如何重試寫入 (C#) 的虛擬程式碼範例：

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>同步存取檔案

[使用 .NET 平行程式設計部落格](https://devblogs.microsoft.com/pfxteam/)是平行程式設計相關指引的絕佳資源。 尤其是，[AsyncReaderWriterLock 相關貼文](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)描述如何維護檔案的獨佔存取權，同時允許並行讀取權限。 請記住，將 I/O 序列化會影響效能。

## <a name="see-also"></a>請參閱

* [建立、寫入和讀取檔案](quickstart-reading-and-writing-files.md)
