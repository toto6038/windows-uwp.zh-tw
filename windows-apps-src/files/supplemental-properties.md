---
title: 使用的補充屬性
description: 如何在 Windows 中已實作中使用的補充屬性和詳細資料的簡介
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10 uwp、 WinRT API，索引子搜尋
localizationpriority: medium
ms.openlocfilehash: b2ac43c9aa2d27f8745e9075abc13d8feaba2370
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592683"
---
# <a name="using-supplemental-properties"></a>使用的補充屬性  

## <a name="summary"></a>摘要  
- 補充屬性允許屬性的標記檔案的應用程式，而不需要變更檔案 
- 幫助的情況下造成難以進行計算的屬性或檔案不能修改 
- 使用的補充屬性就是使用 Windows 屬性系統上的任何其他屬性  

## <a name="introduction"></a>簡介 
許多令人興奮的新應用程式在過去幾年來要求執行有用的屬性擷取的檔案超出基本事項，例如建立日期的使用者檔案的 CPU 密集作業。 這些應用程式一起進行物件辨識影像中，在電子郵件和文字分析意圖的擷取群組的文件中。 這所驅動的強大運算現在已提供下載大部分取用者的電腦。   

立即進行這項中繼資料可搜尋可讓使用者以指數方式更具生產力。 只要了解您的女兒位於圖片很有趣，但能夠搜尋她使用她的祖母的圖片是更有用。 它會使用更加個人化和更多執行電腦操作的體驗。 像是機器中的某個人會連絡可協助您尋找您 treasured 的回憶。 

數十年來，在 Windows 上的快速搜尋解決方案已經過索引子，以及 Creators update 它已更新為支援這些新的案例。 應用程式現在已能夠與其他屬性以外，系統會擷取標記檔案。 這些屬性會被視為第一級公民  

## <a name="windows-properties"></a>Windows 屬性 
[Windows 屬性系統](https://msdn.microsoft.com/library/windows/desktop/ff728898)已與檔案互動，多年來的重要部分。 它可讓應用程式，以讀取檔案中的屬性，而不需要了解所有不同的檔案格式或語言的檔案可能會在內部運作。 所有的抽象化為您身為開發人員，您只需要要求清單，並指定遞增或遞減。  

使用 Windows 索引子密不可分屬性系統，它從其範圍內的檔案讀取所有屬性，並將它們儲存。 稍後當應用程式會要求一份依修改日期排序資料夾中的所有.docx 但不包括作者 John Smith 索引子可以傳回清單，立即。  

這些系統如何一起運作的缺點是，用來要求所有屬性的索引子它會儲存有關要立即可用的檔案。 這受限於它了解有關更有趣的屬性需要較長的時間，計算沒有固定的時間需求。  

透過使用屬性是簡單、 應用程式可以要求的檔案，如同使用資料庫時，工作的相關屬性的已排序資料集，或它可以傳遞類似使用搜尋引擎的查詢。 索引子會處理查詢，並傳回結果。 這可讓開發人員彈性結合其篩選條件 （例如只搜尋 jpg 檔案） 與使用者的查詢 （檔案名稱開頭為"bird"）。 

## <a name="supplemental-properties"></a>補充屬性 

補充屬性的運作方式一般 Windows 屬性與一個非常重要的差異，它們不會在寫入時的檔案新增至索引子。 在更新版本的系統上的另一個應用程式必須新增的補充屬性。 它可能是兩分鐘更新一次完成物件辨識，或可能是天後。 

一旦將屬性寫入可搜尋、 篩選、 排序，或就像任何其他系統上的屬性分組。 以及它可用於合併的查詢，與其他屬性，在系統上，可能是補充與否。 這會讓您的彈性，而不需要進行重寫結合輕鬆地與您現有的檔案系統程式碼的補充屬性。  

### <a name="example-scenarios"></a>範例案例 

有數千個不同的屬性，您可以撰寫為補充的屬性，但有幾本教學課程中會保留回到的重要案例：  

#### <a name="tagging-pictures-with-extracted-properties"></a>使用擷取的屬性標記的圖片 
這些應用程式可以使用 ML 定型的模型，來擷取功能，例如映像中的物件並不知道系統映像。 它然後它會識別影像中的物件，並將它們新增至更新版本時搜尋或分組的屬性系統。  

#### <a name="tagging-files-with-an-app-specific-id"></a>標記應用程式特定識別碼的檔案 
許多檔案同步處理應用程式會使用自己的唯一識別碼來追蹤檔案，因為它們在伺服器和各種用戶端裝置之間移動。 同步處理用戶端可以寫入這個 ID 屬性系統而不影響檔案。 此識別碼現在是適用於快速存取更新版本的應用程式，並可供讀取與同步處理提供者通訊時系統上的任何其他應用程式。 

還有許多其他選項使用的補充屬性，但這兩種讓很好的例子，因為它們需要快速查閱或搜尋，系統並不知道，而且不能對檔案本身的資訊。  

### <a name="using-supplemental-properties"></a>使用的補充屬性 
使用的補充屬性就是寫入檔案系統中的一般屬性。 如果您是想要使用 StorageFiles 和屬性，則可略過此。 否則，請讓我們逐步解說的寫出至檔案的單一屬性，然後稍後讀取相同的屬性，以回快速範例。  

### <a name="writing-supplemental-properties"></a>寫入的補充屬性  
此範例只會修改為了簡單起見，找到的第一個檔案，但應用程式通常會將屬性加入至每個找到的檔案。  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

如果之前寫出屬性的位置會編製索引，有很重要的檢查。 在此範例中，我們會使用查詢選項來篩選只編製索引位置。 如果這不可行，您就可以檢查索引的狀態的父資料夾 （檔案。GetParentAsync()。GetIndexedStateAsync())。 無論何種方式會產生相同的結果 

### <a name="reading-supplemental-properties"></a>讀取的補充屬性 
同樣地，讀取附加屬性是讀取任何其他檔案系統屬性相同。 在此範例應用程式將只是一個屬性從檔案讀取已 StorageFile，但它也能讀取其他屬性，在相同的時間。  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
會檢查並確定來自屬性系統的值是您的預期。 雖然不太可能，但很可能的值已清除，因為您的應用程式具有寫入它。 這會在下方詳細說明。  

### <a name="implementation-notes"></a>實作注意事項 
有一些微妙選擇的補充屬性的設計中的變更。 若要可協助您實作下列各節已複製從工程設計規格的功能。 它們會提供一窺功能的設計方式，和一些限制存在的原因。 

### <a name="supplemental-properties-available"></a>可用的補充屬性 
有可供使用的應用程式一開始只有兩個屬性：System.Supplemental.ResourceId 和 System.Supplemental.AlbumID。 如果沒有更多可以將它們加入的需求。 專輯識別碼是可用於許多不同的應用程式的多重值字串，ResourceId 雲端同步處理提供者時，可做為唯一的識別碼。 

#### <a name="file-system-support"></a>檔案系統支援 
因為格式化為 FAT 卸除式媒體的重要案例，補充屬性將會支援 FAT 與 NTFS 磁碟機。 這可確保的補充屬性即將可供所有使用者，不論其裝置類型。   

### <a name="non-indexed-locations"></a>非索引位置  
在桌面上有許多未編製索引的資料夾。 在這些情況下，應用程式可能仍然想要能夠存取的補充屬性。 不過，不可以在索引位置之外使用的補充屬性。 這項取捨所做的幾個原因：  

- 所有的程式庫和雲端儲存體位置的索引是由預設值。   
  這些是主要是使用該 UWP 應用程式的位置。 不是索引 （系統或網路磁碟機） 的其他位置，但它們較不常用來儲存使用者資料。 

- WinRT API 介面設計假設索引子幾乎都可以使用。  
  因此，索引子已可在大部分的應用程式感興趣的位置。 如果是將資料儲存在非編製索引的位置中找到使用者，最簡單的解決方案將會將該位置新增至索引。 然後補充屬性工作中，列舉型別將會更快，和應用程式都可以變更追蹤的位置。

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>讀取或寫入的補充屬性從 Non-Indexed 位置中的檔案 
在應用程式嘗試寫入的位置不目前編製索引的補充屬性的情況下，然後 API 呼叫會擲回例外狀況。 這會是相同時，有人嘗試更新 System.Music.AlbumArtist.docx 檔案 （無效的引數） 做為擲回的例外狀況。  
 
### <a name="change-notifications"></a>變更通知：  
UWP 變更通知和變更追蹤會繼續運作的補充屬性的標準內容所顯示的一樣。 這可讓應用程式會提供追蹤其應用程式的其中一個發生的所有變更的資料 
  
### <a name="invalidating-properties"></a>失效的屬性：  
每當修改或在系統上移動檔案時，可能會過期的檔案上的補充屬性。 將資料推送的應用程式會使用資訊的相關如果資料有效，或需要更新，讓系統只會提供適用於以找出自己的工具。  
 
在案例中有檔案遭到修改，但無法移動或重新命名，在檔案上的補充屬性會維持不變。 應用程式將能夠透過現有的 API 介面的變更通知註冊，並視需要更新的屬性。 
 
當移動檔案時，屬性將會失效。 在應用程式將會收到變更通知的刪除、 重新命名或移動方式建立完全作業完成。 一旦應用程式已收到變更通知它可以檢查檔案，並視需要更新檔案的補充屬性。 
 
### <a name="indexer-rebuilds"></a>重建索引子  
系統索引偶爾會有多種原因所造成的其中一個重新建立 – 屬性結構描述可能會變更，使用者無法啟用 EDP，或只是資料庫檔案可能會損毀。 在這些情況下，將不會保留的補充屬性。 我們考慮工作，以嘗試保留的補充屬性，當重建索引時，但有幾個主要的封鎖程式：  

### <a name="protecting-the-data"></a>保護資料 
萬一其中資料庫檔案已損毀，磁碟錯誤或惡意軟體，它會無法保護儲存在該檔案中的資料。 您必須儲存在系統上的任一處，或是以某種方式與資料庫其餘部分隔離。 

因為我們已進行許多工作，讓比較不可能已損毀的索引，這會仍要減少這種情況的發生率速率。  
維護檔案與其中繼資料之間的對應，在重建期間 

即使索引可以跨重建保護的資料，就無法知道檔案是否已變更時重新索引。 如果修改或移動檔案，可能會有效不再到索引從檔案保護的資料。  
行為 

在索引子重建的情況下將會遺失所有的補充資料。 應用程式會負責將資料放入失去重建期間的索引子的上一步。 這會將額外的負擔放入應用程式，但被視為合理，因為它們永遠會保留其所有資料的主要狀態。  

### <a name="recovering"></a>復原 
一旦應用程式已經注意到重建的索引，他們會負責更新在方便的補充屬性。  
### <a name="privacy"></a>隱私權 
部分屬性可能會寫入檔案，將使得使用者可能不會希望他們與其他應用程式共用。 應用程式應該要能夠表示他們正在撰寫屬性的資訊即將成為私用為他們的應用程式，與其他少數的應用程式，共用或公用的系統上的每個應用程式。  

雖然這可能是有趣的功能，某些功能的早期採用者，他們覺得，取得公用屬性還是要在設計中新增大量的值。 因此，這標示為，可有可無，而且我們應該繼續建置不支援隱藏值，如有必要的功能。 因此一定要考慮在任何的設計將它加入到稍後將會開啟更多案例。  

## <a name="conclusions"></a>結論 
就這麼簡單，補充屬性是用來在系統中儲存更多的檔案屬性。 使用這些當然是選擇性的但它可以讓您的應用程式邊緣移轉其他應用程式無法排序和搜尋其資料的速度一樣快。 

我們期待看到應用程式開始使用這些屬性。 如果您有任何關於如何使用標頭，請讓我們知道在下列註解的問題 
