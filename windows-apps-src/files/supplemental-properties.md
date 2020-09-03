---
title: 使用補充屬性
description: 使用補充屬性的簡介，以及此類屬性在 Windows 中實作之方式的詳細資料
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, WinRT API, 索引編製程式, 搜尋
localizationpriority: medium
ms.openlocfilehash: 3103074e7d691897e9a8982a254ba36ee331a2b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175392"
---
# <a name="using-supplemental-properties"></a>使用補充屬性  

## <a name="summary"></a>摘要  
- 補充屬性允許應用程式在不變更檔案的情況下搭配屬性標記該檔案 
- 此功能很適合用於有難以計算的屬性，或是檔案無法被修改的情況 
- 使用補充屬性的方式，與使用 Windows 屬性系統上任何其他屬性的方式相同  

## <a name="introduction"></a>簡介 
在過去幾年中，有許多令人興奮的新應用程式都需要在使用者檔案上執行 CPU 密集的作業，以從檔案中擷取超越檔案建立日期等基本資訊的有用屬性。 這些應用程式會在影像中進行物件辨識、在電子郵件中進行意圖擷取，以及進行文字分析來將文件分組在一起。 這些都是透過現在大多數消費者電腦上的強大計算能力來達成。   

透過使此中繼資料可供搜尋，將能立即大幅提升使用者的生產力。 知道自己的女兒位於某張相片中固然有趣，但若能夠搜尋同時包含她和其祖母的相片，將會更加有用。 這會讓使用電腦的體驗變得更加個人化，也更具人性化。 如同電腦中存在著某個人，其會主動向您伸出援手，並協助您找出重要回憶的人一樣。 

在過去數十年來，索引編製程式一直都是在 Windows 上快速搜尋的解決方案；而在 Creators Update 中，它已被更新以支援這些新的案例。 除了由系統所擷取的屬性之外，應用程式現在可以搭配其他屬性來標記檔案。 這些屬性會被視為第一級公民  

## <a name="windows-properties"></a>Windows 屬性 
[Windows 屬性系統](/windows/desktop/properties/windows-properties-system) \(英文\) 多年來一直都是與檔案互動的重要部分。 它允許應用程式從檔案讀取屬性，而不需要了解各種不同的檔案格式或語言。 身為開發人員，系統會為您將那些資訊都抽象化；您只需要要求清單並指定要依遞增還是遞減排序。  

屬性系統與 Windows 索引編製程式密不可分。Windows 索引編製程式會從檔案讀取位於其範圍內的所有屬性並儲存它們。 之後，當某個應用程式要求包含某個資料夾中的所有 .docx 檔案、依修改日期排序，且排除作者為 John Smith 之所有檔案的清單時，索引編製程式便能立即傳回該清單。  

這些系統搭配運作的缺點，在於索引編製程式之前必須要能立即存取其針對某個檔案所儲存的所有相關屬性。 如此固定的時間需求，會限制其得知其他更加有意思，但需要花費更長時間進行計算之屬性的能力。  

不過使用屬性本身相當簡單，應用程式可以要求某個檔案的相關排序屬性集 (和使用資料庫類似)，或是像搜尋引擎般地傳遞查詢。 索引編製程式將會處理查詢，然後傳回結果。 這能讓開發人員彈性地結合其篩選 (例如僅搜尋 jpg 檔案) 及使用者的查詢 (以 “bird” 為開頭的檔案)。 

## <a name="supplemental-properties"></a>補充屬性 

補充屬性的行為與一般 Windows 屬性相同，但有一個非常重要的差異：它們不會隨著檔案被加入至索引編製程式而被寫入。 補充屬性必須在之後由系統上的另一個應用程式加入。 這可以是物件辨識完成兩分鐘後，也可以是數天之後。 

當屬性被寫入後，它就可以被搜尋、篩選、排序或分組，如同系統上任何其他屬性一般。 此外，它可以搭配系統上其他屬性 (不一定必須是補充屬性) 用於合併查詢。 這可讓您彈性地輕鬆結合補充屬性和現有的檔案系統程式碼，而不需要重寫程式碼。  

### <a name="example-scenarios"></a>範例案例 

有數以千計的不同屬性可供您寫入至補充屬性，但本教學課程將會一直造訪數個重要案例：  

#### <a name="tagging-pictures-with-extracted-properties"></a>搭配擷取的屬性標記圖片 
這些應用程式可以使用 ML 定型的模型來從影像中擷取系統不知道的特徵，例如影像中的物件。 它可以將其在影像中所識別的物件加入至屬性系統中，以供稍後進行搜尋或分組。  

#### <a name="tagging-files-with-an-app-specific-id"></a>搭配應用程式特定識別碼來標記檔案 
許多檔案同步應用程式會使用自己的唯一識別碼，來在於伺服器和各種用戶端裝置之間移動時持續追蹤檔案。 同步用戶端可在不影響檔案的前提下，將此識別碼寫入至屬性系統。 此識別碼現在可供該應用程式稍後進行快速存取，並可供系統上其他應用程式在與同步提供者通訊時用於讀取。 

雖然還有許多其他使用補充屬性的選項，但上述兩個案例是很好的例子，因為它們都需要進行快速查閱或搜尋、都是系統不知道的資訊片段，而且都無法被加入至檔案本身。  

### <a name="using-supplemental-properties"></a>使用補充屬性 
使用補充屬性的方式，與將一般屬性寫入至檔案系統相同。 如果您熟悉使用 StorageFiles 和屬性，則可以略過此內容。 否則，讓我們逐步完成將單一屬性寫出至檔案，然後稍後重新讀取回相同屬性的快速範例。  

### <a name="writing-supplemental-properties"></a>寫入補充屬性  
為了簡單起見，範例將只會修改其所找到的第一個檔案；一般而言，應用程式會將屬性加入至其所找到的所有檔案。  

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

在寫出屬性之前，必須先進行確認位置是否已編製索引的重要檢查。 在此範例中，我們會使用查詢選項來僅篩選至已編製索引的位置。 如果這不可行，您可以檢查父資料夾 (file.GetParentAsync().GetIndexedStateAsync()) 的編製索引狀態。 兩種方法都會產生相同的結果 

### <a name="reading-supplemental-properties"></a>讀取補充屬性 
同樣地，讀取補充屬性的方式和讀取任何其他檔案系統屬性的方式皆相同。 在此範例中，應用程式只會從其已具有 StorageFile 的檔案讀取單一屬性，但它同時也可以讀取其他屬性。  

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
您應該先進行檢查以確保從屬性系統傳回的值是您所預期的。 雖然機率很小，但該值還是有可能已經被清除，因為您的應用程式已寫入它。 這將會涵蓋於下列的詳細資料中。  

### <a name="implementation-notes"></a>實作注意事項 
在設計補充屬性時，開發團隊做出了幾個細微的設計選擇。 為了協助您進行實作，下列小節的內容是從此功能的工程設計規格中複製過來的。 它們能讓您一窺此功能的設計方式，以及一些限制存在的原因。 

### <a name="supplemental-properties-available"></a>可用的補充屬性 
一開始只有兩個屬性可供用於應用程式：System.Supplemental.ResourceId 和 System.Supplemental.AlbumID。 如果還需要更多，則可以加入它們。 AlbumID 是多值字串，可用於許多不同的應用程式；而 ResourceId 則是用來作為適用於雲端同步提供者的唯一識別碼。 

#### <a name="file-system-support"></a>系統系統支援 
由於格式化為 FAT 的抽取式媒體是很重要的案例，補充屬性將會支援 FAT 和 NTFS 磁碟機。 這將能確保補充屬性可供所有使用者使用，無論其裝置類型為何。   

### <a name="non-indexed-locations"></a>未編製索引的位置  
在桌面上有數個尚未編製索引的資料夾。 在這種情況下，應用程式可能仍然會想要存取補充屬性。 不過，補充屬性並無法在已編製索引位置以外的地方提供使用。 此妥協是基於幾個原因：  

- 所有程式庫和雲端儲存體位置預設都會編製索引。   
  這些是 UWP 應用程式主要會使用的位置。 還有其他尚未編製索引的位置 (系統或網路磁碟機)，但它們較少被用來儲存使用者資料。 

- WinRT API 表面設計會假設索引編製程式幾乎隨時都可供使用。  
  因此索引編製程式在應用程式感興趣的大部分位置中皆已可供使用。 如果使用者是在未編製索引的位置中儲存資料，最簡單的解決方案是將該位置加入索引中。 然後補充屬性便能順利運作、列舉將會更加快速，且應用程式將能變更追蹤該位置。

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>從位於未編製索引之位置中的檔案讀取或寫入補充屬性 
若應用程式嘗試將補充屬性寫入目前尚未編製索引的位置中，API 呼叫將會擲回例外狀況。 此例外狀況將會和有人嘗試更新 .docx 檔案上的 System.Music.AlbumArtist 時的例外狀況 (無效的引數) 相同。  
 
### <a name="change-notifications"></a>變更通知：  
UWP 變更通知和變更追蹤將能繼續搭配補充屬性運作，如同其搭配標準屬性一般。 這將能讓提供資料的應用程式追蹤發生在它的其中一個應用程式上的所有變更 
  
### <a name="invalidating-properties"></a>失效的屬性：  
檔案上的補充屬性在該檔案被修改或在系統上移動時可能會變成過期。 推送資料的應用程式將會具有資料是否有效或是需要進行更新的相關資訊，因此系統會直接為它們提供工具來自行釐清這點。  
 
在檔案已被修改，但尚未被移動或重新命名的情況下，該檔案上的所有補充屬性都會保持不變。 應用程式將能透過現有 API 表面註冊變更通知，並視需要更新屬性。 
 
如果檔案已被移動，則屬性都會失效。 應用程式將會收到刪除、建立、重新命名或移動的變更通知，視該作業實際完成的方式而定。 當應用程式接收到變更通知之後，它將能檢查該檔案，並視需要更新檔案上的補充屬性。 
 
### <a name="indexer-rebuilds"></a>索引編製程式重建  
有時候系統索引基於幾個原因必須重建：屬性結構描述可能已變更、使用者可能已啟用 EDP，或資料庫檔案單純已經損毀。 在這些情況下，補充屬性將不會被保留。 我們曾考慮嘗試在索引重建時保留補充屬性，但有幾個主要的障礙導致我們無法這麼做：  

### <a name="protecting-the-data"></a>保護資料 
在資料庫檔案損毀 (無論是因磁碟錯誤或惡意軟體) 的情況下，將會沒有任何辦法來保護儲存在該檔案中的資料。 它必須被儲存在系統上的其他地方，或是透過某種方法與其餘的資料庫隔離。 

由於我們已經進行許多工作來使索引較不容易被損毀，這本身已經能減少此情況發生的頻率。  
在重建期間維護檔案和其中繼資料之間的對應 

就算索引可以在重建期間保護資料，我們也不可能在索引重建期間得知檔案是否被變更。 如果檔案已被修改或移動，索引從該檔案中保護的資料便可能不再有效。  
行為 

在索引編製程式重建的情況下，所有補充資料都會遺失。 應用程式必須負責將重建期間所遺失的資料重新放入索引編製程式。 這確實會對應用程式造成額外的負荷，但我們認為這是合理的情況，因為它們一律會保留其所有資料的主要狀態。  

### <a name="recovering"></a>復原 
當應用程式注意到索引正在被重建時，它們便必須負責在對其方便的時機更新補充屬性。  
### <a name="privacy"></a>私密性 
針對某些可能會被寫入檔案的屬性，使用者可能不會想將它們與其他應用程式共用。 應用程式應該要能指出其寫入至屬性的資訊是僅限其應用程式使用的私人資訊、可與幾個其他應用程式共用的資訊，或是可供系統上所有應用程式使用的公用資訊。  

不過此功能對一些早期採用者來說具有相當的潛在可能性，他們覺得取得公用屬性仍然能為設計帶來許多價值。 因此，我們將此功能標記為可有可無，而我們應該會繼續在無法視需要隱藏值的情況下建置此功能。 在未來加入此功能將會導入更多案例，因此我們應該在所有設計中都考慮它。  

## <a name="conclusions"></a>結論 
就這樣，補充屬性是在系統中儲存更多檔案屬性的簡單方式。 它們的使用當然是選擇性的，但可以讓您的應用程式比起其他無法如此快速地排序及搜尋其資料的應用程式來得更加有效率。 

我們很期待看到應用程式開始使用這些屬性。 如果您對於使用標頭的方式有任何疑問，請在下方留言來讓我們知道