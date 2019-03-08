---
title: 寫入檔案的最佳作法
description: 了解使用撰寫 FileIO 和 PathIO 類別方法的各種檔案的最佳作法。
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605833"
---
# <a name="best-practices-for-writing-to-files"></a>寫入檔案的最佳作法

**重要的 Api**

* [**FileIO 類別**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 類別**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

開發人員使用時，有時候遇到的常見問題集**撰寫**種[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)並[ **PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)類別來執行檔案系統 I/O 作業。 例如，常見的問題包括：

• 檔案部分寫入 • 應用程式會呼叫其中一個方法時，會收到例外狀況。 • 作業拋諸腦後。使用類似於目標檔案名稱的檔案名稱的 TMP 檔案。

**撰寫**種[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)並[ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio)類別包括下列：

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 本文章提供有關這些方法的是開發人員的運作方式的詳細資料進一步了解何時及如何使用它們。 本文提供指導方針，並不會嘗試為所有可能的檔案 I/O 問題提供解決方案。 

> [!NOTE]
> 這篇文章著重**FileIO**範例和討論區中的方法。 不過， **PathIO**方法遵循類似的模式和大部分的這篇文章中的指導方針也適用於這些方法。 

## <a name="conveience-vs-control"></a>與控制項 Conveience

A [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)物件不是像原生 Win32 程式設計模型的檔案控制代碼。 相反地， [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)是一種方法來管理其內容的檔案。

執行使用 I/O 時了解這個概念很有用**StorageFile**。 例如，[寫入至檔案](quickstart-reading-and-writing-files.md#writing-to-a-file)區段會顯示三種方式可寫入檔案：

* 使用[ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)方法。
* 藉由建立緩衝區，然後再呼叫[ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync)方法。
* 使用資料流，在四個步驟模型：
  1. [開啟](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync)来取得的資料流的檔案。
  2. [取得](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)輸出資料流。
  3. 建立[**資料寫入元**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)物件，並呼叫對應**撰寫**方法。
  4. [認可](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)中的資料寫入器的資料並[排清](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)輸出資料流。

前兩個案例是最常使用的應用程式。 單一作業中的檔案寫入程式碼，並維護變得更加容易，而且它也會從許多複雜的檔案 I/O 處理移除的應用程式的責任。 不過，這種便利要付出成本： 控制整個作業，並且能夠在特定時間點擷取錯誤的遺失。

## <a name="the-transactional-model"></a>交易式模型

**撰寫**種[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)並[ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio)類別包裝的步驟，在第三個寫入上述，新增一層的模型。 此圖層被封裝在儲存體交易。

萬一發生錯誤時寫入資料，保護原始檔的完整性**撰寫**方法會使用交易式模型開啟檔案使用[ **OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). 此程序會建立[ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)物件。 Api 會建立此交易物件之後，寫入資料遵循類似的方式[檔案存取權](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)範例或中的程式碼範例[ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)一文。

下圖說明所執行的基礎工作**WriteTextAsync**中成功的寫入作業的方法。 此圖會提供作業的簡化的檢視。 比方說，它會略過步驟，例如文字編碼方式和非同步完成不同的執行緒上。

![UWP API 呼叫順序圖表，以寫入檔案](images/file-write-call-sequence.svg)

使用的優點**撰寫**方法[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)並[ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio)改為類別更複雜的四個步驟模型使用的資料流屬於：

* 若要處理的所有中繼步驟，包括錯誤的一個 API 呼叫。
* 如果發生錯誤，則會保留原始的檔案。
* 系統狀態將會嘗試保留為清除越好。

不過，有這麼多中繼失敗的可能點，就可能的失敗。 發生錯誤時，它可能難以了解此程序失敗的位置。 下列各節提供一些使用時可能遭遇的失敗**寫入**方法，並提供可能的解決方案。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>常見的錯誤碼 FileIO 和 PathIO 類別的寫入方法

此表顯示常見應用程式開發人員在使用時所遇到的錯誤碼**寫入**方法。 在資料表中的步驟會對應到在上圖中的步驟。

|  錯誤名稱 （值）  |  步驟  |  原因  |  解決方案  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  原始的檔案可能會標示為刪除，可能是從先前的作業。  |  重試作業。</br>請確定檔案的存取會同步處理。  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  原始檔案會開啟另一個的獨佔寫入。   |  重試作業。</br>請確定檔案的存取會同步處理。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  不會取代原始的檔案 (file.txt)，因為它正在使用中。 另一個處理序或作業取得檔案的存取權，可以取代之前。  |  重試作業。</br>請確定檔案的存取會同步處理。  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  此交易的模型會建立一個額外的檔案，這會消耗額外的儲存體。  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  這可能是由於多個未處理 I/O 作業或大型檔案的大小。  |  更細微的方法，藉由控制資料流可能會解決此錯誤。  |
|  E_FAIL (0x80004005) |  任何值  |  其他  |  重試作業。 如果仍然失敗，它可能是平台錯誤，而應用程式應終止，因為它處於不一致的狀態。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>其他考量可能會導致錯誤的檔案狀態

除了由傳回的錯誤**寫入**方法，以下是一些指導方針的應用程式可以預期會發生什麼寫入檔案。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>資料已寫入至檔案，才完成的作業

寫入作業正在進行時您的應用程式不應在檔案中建立資料相關的任何假設。 嘗試存取檔案，作業完成之前，可能會導致不一致的資料。 您的應用程式應負責追蹤未處理 I/o。

### <a name="readers"></a>讀取者

如果是正常的讀取器正在使用之檔案的寫入也 (亦即，以開啟[ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)，後續的讀取會失敗並產生錯誤 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323)。 有時候應用程式重試一次開啟檔案進行讀取時再次**寫入**作業正在進行中。 這可能會造成競爭情形所在**寫入**最後失敗時嘗試覆寫原始的檔案，因為它無法被取代。

### <a name="files-from-knownfolders"></a>從 KnownFolders 檔案

您的應用程式可能不是唯一的應用程式嘗試存取檔案所在的任何[ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)。 下一次嘗試讀取檔案沒有保證，如果作業成功時，應用程式寫入檔案的內容會保持不變。 此外，共用或存取被拒錯誤變得更為普遍，在這種情況下。

### <a name="conflicting-io"></a>衝突的 I/O

如果我們的應用程式使用，可能會降低並行錯誤的機會**寫入**方法在其本機資料，但某些警告是否仍需要。 如果有多個**寫入**作業傳送同時寫入檔案，則無法保證有關哪些資料最後會在檔案中。 若要緩解這個情況，我們建議您的應用程式序列化**寫入**檔案的作業。

### <a name="tmp-files"></a>~ TMP 檔案

有時候，將強制取消作業 （例如，如果應用程式已暫止或終止由 OS），如果交易未認可或適當地關閉。 這可以留下的檔案 (。 ~ TMP) 延伸模組。 刪除這些暫存檔，（如果它們存在於應用程式的本機資料），請考慮處理應用程式啟動時。

## <a name="considerations-based-on-file-types"></a>根據檔案類型的考量

某些錯誤可能會變得更普遍視檔案、 在其要存取它們，頻率和其檔案大小的類型而定。 一般來說，有三種類別的應用程式可以存取的檔案：

* 檔案建立和編輯您的應用程式本機資料夾中的使用者。 這些是建立，而且只有在使用您的應用程式時，才能編輯，在於只在應用程式內。
* 應用程式中繼資料。 您的應用程式會使用這些檔案，來追蹤自己的狀態。
* 您的應用程式已經宣告功能，可存取的位置的檔案系統位置中的其他檔案。 這些通常位於其中一種[ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)。

您的應用程式有完全控制權限的檔案前, 兩個類別，因為它們是您的應用程式套件檔案的一部分，而且會以獨佔方式存取您的應用程式。 最後一個分類中的檔案，您的應用程式必須注意，其他應用程式和 OS 服務可能會存取檔案同時。

根據應用程式中，檔案的存取權可以有所不同頻率：

* 非常低。 這些通常是一旦當應用程式啟動次數並會儲存在暫止應用程式時開啟的檔案。
* 低。 這些是使用者特別採取動作 （例如儲存或載入） 的檔案。
* 中度或高度。 這些是在其中應用程式必須經常更新資料 （例如，自動儲存功能或追蹤常數的中繼資料） 的檔案。

檔案大小，請考慮下列圖表中的效能資料**WriteBytesAsync**方法。 此圖表會比較完成的作業與檔案大小，而非每個受控制的環境中的檔案大小的 10000 個作業的平均效能的時間。

![WriteBytesAsync 效能](images/writebytesasync-performance.png)

從這個圖表，因為不同的硬體和組態會產生不同的絕對時間值的 y 軸上的時間值會刻意省略。 不過，我們已經一致地觀察到這些趨勢在我們的測試：

* 非常小的檔案 (< = 1 MB):完成作業的時間會一致地快速。
* 較大的檔案 (> 1 MB):若要完成之作業的時間就會開始以指數方式增加。

## <a name="io-during-app-suspension"></a>在應用程式暫止期間的 I/O

您的應用程式必須設計可處理擱置，如果您想要保留狀態資訊或中繼資料，以用於後續的工作階段。 如需應用程式暫止的背景資訊，請參閱[應用程式生命週期](../launch-resume/app-lifecycle.md)並[此部落格文章](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)。

除非 OS 授與您的應用程式的擴充的執行暫止您的應用程式時有 5 秒，以釋出其所有資源，並儲存其資料。 最佳的可靠性和使用者體驗，一律假設您必須處理暫止工作的時間有限。 請記住下列指導方針在 5 秒鐘處理暫止工作的時間週期內：

* 嘗試將保留在最小值，以避免排清和發行作業所造成的競爭情形的 I/O。
* 避免撰寫需要數百毫秒或多個要寫入的檔案。
* 如果您的應用程式會使用**寫入**方法，請記住所有這些方法需要的中繼步驟。

如果您的應用程式會在暫止期間操作少量的狀態資料，在大部分情況下您可以使用**寫入**排清資料的方法。 不過，如果您的應用程式使用大量的狀態資料，請考慮使用資料流直接將您的資料。 這可以協助降低延遲的交易式模型帶來**寫入**方法。 

如需範例，請參閱[BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension)範例。

## <a name="other-examples-and-resources"></a>其他範例和資源

以下是幾個範例，以及針對特定案例的其他資源。

### <a name="code-example-for-retrying-file-io-example"></a>重試檔案 I/O 的範例程式碼範例

以下是有關如何重試一次寫入的虛擬程式碼範例 (C#)，假設寫入完成後，使用者會取得儲存的檔案是：

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

### <a name="synchronize-access-to-the-file"></a>同步處理檔案的存取權

[平行程式設計.NET 部落格](https://blogs.msdn.microsoft.com/pfxteam/)是平行程式設計的相關指引的絕佳資源。 特別是，[發表 AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)描述如何維護的獨佔存取權的檔案進行寫入，同時允許並行的讀取權限。 請注意，序列化的 I/O 會影響效能。

## <a name="see-also"></a>請參閱

* [建立、 寫入和讀取檔案](quickstart-reading-and-writing-files.md)
