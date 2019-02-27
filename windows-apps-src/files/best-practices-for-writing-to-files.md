---
title: 寫入檔案的最佳做法
description: 了解使用各種不同的檔案，撰寫 FileIO 和 PathIO 類別的方法的最佳做法。
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115703"
---
# <a name="best-practices-for-writing-to-files"></a>寫入檔案的最佳做法

**重要 API**

* [**FileIO 類別**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 類別**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

開發人員有時會遇到一組常見的問題時，使用**撰寫**方法的[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)類別來執行檔案系統 I/O 作業。 例如，包括常見的問題：

• 檔案部分是撰寫 • 應用程式會呼叫其中一個方法時，會收到例外狀況。 • 作業留下的東西。TMP 檔案，以類似於目標檔案名稱的檔案名稱。

**撰寫** [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)類別的方法包括：

* **Fileio**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **Fileio**

 本文提供關於這些方法，開發人員的運作方式的詳細資料更深入了解何時及如何使用它們。 本文提供指導方針，並不會嘗試提供的所有可能的檔案 I/O 問題的解決方案。 

> [!NOTE]
> 本文著重在**FileIO**中的方法的範例及討論。 不過， **PathIO**方法遵循類似的模式，而且大部分的這篇文章中的指導方針也適用於這些方法。 

## <a name="conveience-vs-control"></a>Conveience 與控制項

[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)物件不是像原生的 Win32 程式設計模型的檔案控制代碼。 相反地， [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)是檔案的方法來操控其內容的表示法。

執行與**StorageFile**I/O 時，了解這個概念很有用。 例如，[檔案寫入](quickstart-reading-and-writing-files.md#writing-to-a-file)區段提供三種方式來寫入檔案：

* 使用[**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)方法。
* 藉由建立一個緩衝區，然後呼叫 [ [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync)方法。
* 使用資料流的四個步驟模型：
  1. [開啟](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync)檔案取得資料流。
  2. [取得](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)輸出資料流。
  3. 建立的[**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)物件並呼叫對應的**寫入**方法。
  4. [認可](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)中的資料寫入器和[排清](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)輸出資料流的資料。

前兩個案例是最常使用的應用程式。 在單一作業中的檔案寫入容易程式碼和維護，且它也會在應用程式的責任移除處理的許多檔案 I/O 的複雜性。 不過，雖然方便卻有其代價： 遺失控制整個作業和攔截錯誤上特定點的能力。

## <a name="the-transactional-model"></a>交易的模型

**撰寫**類別的方法[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)換行上的第三個的寫入模型，如上所述，與一層額外的步驟。 這個層級會封裝在儲存空間交易。

為了保護的原始檔案的完整性，以便在發生錯誤寫入資料時，**撰寫**的方法會使用交易模型藉由開啟使用[**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync)檔案。 此程序會建立[**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)物件。 建立這個交易物件之後，Api 撰寫資料[檔案存取](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)範例或[**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)文章中的程式碼範例遵循類似的方式。

下圖說明基礎所執行的工作中成功的寫入作業的**Fileio**方法。 這個圖例提供簡化的作業的檢視。 例如，它略過步驟，例如文字編碼和非同步完成不同執行緒上。

![UWP API 的呼叫順序圖表來寫入檔案](images/file-write-call-sequence.svg)

使用**撰寫** [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)類別的方法，而不使用資料流的更複雜的四個步驟模型的優點包括：

* 一次 API 呼叫，以處理所有中繼的步驟，包括錯誤。
* 如果發生錯誤，會保留原始的檔案。
* 系統狀態將會嘗試以全新儘可能保持。

不過，許多可能中繼點失敗，沒有增加機會讓的失敗。 發生錯誤時可能會難以了解此程序失敗的位置。 下列各節會呈現部分失敗，您可能會遇到時使用**撰寫**的方法，並提供可能的解決方案。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>常見的錯誤代碼寫入的 FileIO 和 PathIO 類別的方法

下表顯示應用程式開發人員在使用的**撰寫**方法時遇到的常見錯誤代碼。 此表格中的步驟會對應到先前的圖表中的步驟。

|  錯誤名稱 （值）  |  步驟  |  原因  |  解決方案  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  原始的檔案會在可能標示為刪除，可能是從先前的作業。  |  重試作業。</br>請確定同步處理檔案的存取權。  |
|  ERROR_SHARING_VIOLATION (0X80070020)  |  5  |  由另一個專屬寫入開啟原始的檔案。   |  重試作業。</br>請確定同步處理檔案的存取權。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19-20  |  無法取代的原始的檔案 (file.txt)，因為它是使用中。 另一個處理程序或作業獲得檔案的存取權之前可能會取代。  |  重試作業。</br>請確定同步處理檔案的存取權。  |
|  ERROR_DISK_FULL (0X80070070)  |  7、 14、 16、 20  |  交易的模型建立額外的檔案，並這樣會消耗額外的儲存空間。  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14、 16  |  這可能會發生，因為多個未完成的 I/O 作業或大型檔案的大小。  |  藉由控制資料流的更細微的方法可能會解決錯誤。  |
|  E_FAIL (0X80004005) |  任何值  |  其他  |  重試作業。 如果仍然失敗，可能會平台錯誤，並將應用程式應該終止，因為它是以一致的狀態。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>檔案狀態，可能會導致錯誤的其他考量

姑且**撰寫**方法所傳回的錯誤，以下是在寫入檔案時，應用程式功能可以預期的一些指導方針。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>只有在作業完成資料已寫入檔案

您的應用程式不應任何假設有關資料檔案中寫入作業進行時。 嘗試存取檔案，在作業完成之前，可能會導致不一致的資料。 您的應用程式應該負責追蹤待處理的 I/o。

### <a name="readers"></a>閱讀程式

如果正在使用的檔案寫入也禮貌讀卡機 （也就是以[**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)開啟，後續的讀取會將會失敗 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323) 有錯誤。 有時候應用程式重試重新開啟檔案讀取的**寫入**作業正在進行時。 這可能會導致所在的**寫入**最後失敗時嘗試覆寫原始的檔案，因為它無法取代的競爭情形。

### <a name="files-from-knownfolders"></a>從 KnownFolders 檔案

您的應用程式可能不會嘗試存取任何[**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)所在的檔案的唯一應用程式。 沒有作業是否成功，應用程式寫入檔案的內容會保持固定保證下一次嘗試讀取檔案。 此外，共用或拒絕存取得在本案例下更為常見的錯誤。

### <a name="conflicting-io"></a>衝突的 I/O

如果我們的應用程式使用的**寫入**方法的檔案，在其本機資料，但仍需要一些警告，可以降低並行錯誤的機會。 如果多個**寫入**作業都能傳送到檔案的同時，就不保證有關哪些資料檔案中之中。 若要減輕此問題，我們建議您的應用程式會序列化至檔案的**寫入**作業。

### <a name="tmp-files"></a>~ TMP 檔案

偶而，如果將強制取消作業 （例如，如果應用程式暫停或終止作業系統），不認可或交易適當地關閉。 這可以留下的東西的檔案 (。 ~ TMP) 延伸模組。 請考慮刪除這些暫存檔案 （如果他們存在於應用程式的本機資料） 時處理 app 啟用。

## <a name="considerations-based-on-file-types"></a>根據檔案類型的考量

有些錯誤會變得更普遍根據檔案、 頻率所在它們正在存取，以及其檔案大小的類型。 一般而言，有三個類別的應用程式可以存取的檔案：

* 檔案建立和編輯使用者在您的應用程式的本機資料資料夾中。 這些會建立並使用您的應用程式時，只能編輯，而且它們存在於只在應用程式中。
* 應用程式中繼資料。 您的應用程式會使用這些檔案來追蹤自己的狀態。
* 您的應用程式已宣告功能來存取的位置的檔案系統位置中的其他檔案。 這些是最常位於其中一個[**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)。

您的應用程式會有完全控制所使用的前兩個類別的檔案，因為它們是您的應用程式套件檔案的一部分，並以獨佔方式存取您的應用程式。 最後一個類別中的檔案，您的應用程式必須注意的其他應用程式和作業系統服務可能會存取檔案同時。

根據應用程式，可能會有所不同頻率的存取的檔案：

* 很低。 這些通常是後的應用程式啟動和會儲存時暫止應用程式時所開啟的檔案。
* 低。 這些是使用者特別採取的動作 （例如儲存或載入） 的檔案。
* 中型或高。 這些是的檔案中的應用程式必須不斷地更新資料 （例如，自動儲存功能或追蹤常數的中繼資料）。

檔案的大小，請考慮下列圖表**WriteBytesAsync**方法中的效能資料。 此圖表比較的時間才能完成作業 vs 檔案的大小，透過每個受控制的環境中的檔案大小的 10000 作業的平均效能。

![WriteBytesAsync 效能](images/writebytesasync-performance.png)

此圖表中，因為不同的硬體和設定將會產生不同的絕對時間值，y 軸的時間值是刻意省略。 不過，我們以一致的方式觀察到這些趨勢在我們的測試：

* 適用於非常小的檔案 (< = 1 MB): 是以一致的方式快速完成作業的時機。
* 針對較大的檔案 (> 1 MB): 以指數方式增加啟動的時間才能完成作業。

## <a name="io-during-app-suspension"></a>在 app 暫停期間 I/O

您的應用程式必須設計成處理暫停，如果您想要保留狀態資訊或中繼資料，在稍後的工作階段中使用。 如需應用程式暫停的背景資訊，請參閱[應用程式生命週期](../launch-resume/app-lifecycle.md)和[此部落格文章](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)。

作業系統會授與延伸的執行您的應用程式，除非您的應用程式暫止時它有 5 秒釋出其所有資源，並儲存自己的資料。 最佳的可靠性和使用者體驗，一律會假設您需要處理暫停工作的時間是有限。 請記住下列指導方針 5 秒時間期間，處理暫停工作期間：

* 請嘗試讓 I/O 保持在最小值，以避免排清和發行作業所造成的競爭情形。
* 避免撰寫需要數百毫秒或撰寫更多的檔案。
* 如果您的應用程式使用的**寫入**的方法，請記住這些方法需要的所有中繼步驟。

如果您的應用程式會在暫停期間運作上少量的狀態資料，在大部分情況下您可以使用**寫入**方法將資料排清。 不過，如果您的應用程式使用大量狀態資料，請考慮使用資料流直接儲存您的資料。 這可協助降低所**撰寫**的方法的交易模型的延遲。 

如需範例，請參閱[BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension)範例。

## <a name="other-examples-and-resources"></a>其他範例和資源

以下是幾個範例與特定案例的其他資源。

### <a name="code-example-for-retrying-file-io-example"></a>適用於重試檔案 I/O 範例程式碼範例

以下是有關如何重試寫入 (C# 中)，假設使用者挑選一個檔案可供儲存之後完成寫入的虛擬程式碼範例：

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

[使用.NET 部落格的平行程式設計](https://blogs.msdn.microsoft.com/pfxteam/)是關於平行程式設計的指導方針的絕佳資源。 尤其是，[張貼 AsyncReaderWriterLock 相關](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)的說明如何維護獨佔存取權檔案寫入，同時允許並行讀取權限。 請記住序列化 I/O 會影響效能。

## <a name="see-also"></a>請參閱

* [建立、寫入和讀取檔案](quickstart-reading-and-writing-files.md)
