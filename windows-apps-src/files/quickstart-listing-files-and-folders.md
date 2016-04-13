---
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
列舉和查詢檔案和資料夾
存取位於資料夾、媒體櫃、裝置或網路位置中的檔案和資料夾。 您也可以建構檔案和資料夾查詢，來查詢位置中的檔案和資料夾。
---
# 列舉和查詢檔案和資料夾


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


存取位於資料夾、媒體櫃、裝置或網路位置中的檔案和資料夾。 您也可以建構檔案和資料夾查詢，來查詢位置中的檔案和資料夾。

**注意**：另請參閱[資料夾列舉範例](http://go.microsoft.com/fwlink/p/?linkid=619993)。

 
## 先決條件

-   **了解通用 Windows 平台 (UWP) app 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **位置的存取權限**

    例如，這些範例中的程式碼需要 **picturesLibrary** 功能，但是您的位置可能需要其他功能或完全不需要功能。 若要深入了解，請參閱[檔案存取權限](file-access-permissions.md)。

## 列舉位置中的檔案和資料夾

> **注意**：請記得宣告 **picturesLibrary** 功能。

在這個範例中，我們會先使用 [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) 方法來取得 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 的根資料夾中 (不在子資料夾) 的所有檔案，並列出每個檔案的名稱。 接下來，我們會使用 [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) 方法來取得 **PicturesLibrary** 中的所有子資料夾，並列出每個子資料夾的名稱。

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"] > ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections; > using namespace concurrency; > using namespace std; > > // 請務必在 appxmanifext 檔案中指定「圖片資料夾」功能。
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary; > > // 使用 shared_ptr，讓字串保留於記憶體中
> // 直到最後一個工作完成為止e.
> auto outputString = make_shared<wstring>(); > *outputString += L"Files:\n"; > > // 取得檔案物件的唯讀向量
> // 並將它傳送到接續n. 
> create_task(picturesFolder->GetFilesAsync())        
> // outputString 是依值擷取的，其會建立 a copy > // shared_ptr 的複本，並遞增它的參考e cou計數。
> .then ([outputString] (IVectorView\<StorageFile^>^ files) > {        
> for ( unsigned int i = 0 ; i < files->Size; i++) > { > *outputString += files->GetAt(i)->Name->Data(); > *outputString += L"\n"; > } > })
> // 我們需要在此處明確地敘述傳回類型pe 
> // 這裡：-> IAsyncOperation<...>
>     .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^ > {
> return picturesFolder->GetFoldersAsync();
> }) > // 擷取 "this"，從 Lambda 內存取 m_OutputTextBlockambda。
> .then([this, outputString](IVectorView\<StorageFolder^>^ folders) > {        
> *outputString += L"Folders:\n"; > > for ( unsigned int i = 0; i < folders->Size; i++) > { > *outputString += folders->GetAt(i)->Name->Data(); > *outputString += L"\n"; > } > > // 假設 m_OutputTextBlock 是 XAML 中定義的 TextBlock。
> m_OutputTextBlock->Text = ref new String((*outputString).c_str()); > }); > ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<StorageFile> fileList = > await picturesFolder.GetFilesAsync(); > > outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> { > outputText.Append(file.Name + "\n"); > } > > IReadOnlyList<StorageFolder> folderList = > await picturesFolder.GetFoldersAsync(); > > outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> { > outputText.Append(folder.DisplayName + "n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
> 
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
> 
>     outputText.Append(file.Name & vbLf)
> 
> Next file
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
> 
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
> 
>     outputText.Append(folder.DisplayName & vbLf)
> 
> Next folder
> ```


> **注意**：在 C# 或 Visual Basic 中，請務必在您使用 **await** 運算子的任何方法的方法宣告中放置 **async** 關鍵字。
 

或者，您可以使用 [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) 方法取得特定位置中的所有項目 (檔案與子資料夾)。 下列範例使用 **GetItemsAsync** 方法取得 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 的根資料夾中 (不在子資料夾) 的所有檔案與子資料夾。 接著範例會列出每個檔案或子資料夾的名稱。 如果項目是子資料夾，範例會將 `"folder"` 附加到名稱。

> [!div class="tabbedCodeSnippets"] > ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>(); > > create_task(picturesFolder->GetItemsAsync())        
> .then ([this, outputString] (IVectorView<IStorageItem^>^ items) > {        
> for ( unsigned int i = 0 ; i < items->Size; i++) > { > *outputString += items->GetAt(i)->Name->Data(); > if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder)) > { > *outputString += L" folder\n"; > } > else > { > *outputString += L"\n"; > } > m_OutputTextBlock->Text = ref new String((*outputString).c_str()); > } > }); > ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<IStorageItem> itemsList = > await picturesFolder.GetItemsAsync(); > > foreach (var item in itemsList)
> {
> if (item is StorageFolder) >     {
> outputText.Append(item.Name + " folder\n"); > > } > else > { > outputText.Append(item.Name + "\n"); > > } > } > ``` > ```vb > Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary > Dim outputText As New StringBuilder > > Dim itemsList As IReadOnlyList(Of IStorageItem) = > Await picturesFolder.GetItemsAsync() > > For Each item In itemsList > > If TypeOf item Is StorageFolder Then > > outputText.Append(item.Name & " folder" & vbLf) > > Else > > outputText.Append(item.Name & vbLf) > > End If > > Next item > ``` ## 查詢位置中的檔案並列舉相符的檔案 在這個範例中，我們會查詢依月份分組的 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 中的所有檔案，這次範例會遞迴到子資料夾。 首先，我們會呼叫 [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) 並將 [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) 值傳遞到方法。 我們會得到 [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066) 物件。

接著我們會呼叫 [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074)，它會傳回代表虛擬資料夾的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 物件。 在這個案例中，我們依月份分組，讓每個虛擬資料夾代表相同月份的檔案群組。

> [!div class="tabbedCodeSnippets"] > ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search; > using namespace concurrency; > using namespace Platform::Collections; > using namespace Windows::Foundation::Collections; > using namespace std; > > StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary; > > StorageFolderQueryResult^ queryResult = 
> picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth); > > // 使用 shared_ptr，讓 outputString 保留於記憶體中
> // 直到工作完成為止，即函式超出範圍之後e.
> auto outputString = std::make_shared<wstring>(); > > create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view) > {        
> for ( unsigned int i = 0; i < view->Size; i++) >     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files) > { > *outputString += view->GetAt(i)->Name->Data(); > *outputString += L"("; > *outputString += to_wstring(files->Size); > *outputString += L")\r\n"; > for (unsigned int j = 0; j < files->Size; j++) > { > *outputString += L" "; > *outputString += files->GetAt(j)->Name->Data(); > *outputString += L"\r\n"; > } > }).then([this, outputString]() > { > m_OutputTextBlock->Text = ref new String((*outputString).c_str()); > }); > } > }); > ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> 
> StorageFolderQueryResult queryResult = 
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList = > await queryResult.GetFoldersAsync(); > > StringBuilder outputText = new StringBuilder(); > > foreach (StorageFolder folder in folderList)
> {
> IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync(); > > // 列印月份和此群組中的檔案數。
> outputText.AppendLine(folder.Name + " (" + fileList.Count + ")"); > > foreach (StorageFile file in fileList) > { > // 列印檔案的名稱。
> outputText.AppendLine(" " + file.Name); > } > } > ``` > ```vb > Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary > Dim outputText As New StringBuilder > > Dim queryResult As StorageFolderQueryResult = > picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth) > > Dim folderList As IReadOnlyList(Of StorageFolder) = > Await queryResult.GetFoldersAsync() > > For Each folder As StorageFolder In folderList > > Dim fileList As IReadOnlyList(Of StorageFile) = > Await folder.GetFilesAsync() > > ' 列印月份和此群組中的檔案數。
> outputText.AppendLine(folder.Name & " (" & fileList.Count & ")") > > For Each file As StorageFile In fileList > > ' 列印檔案的名稱。
> outputText.AppendLine(" " & file.Name) > > Next file > > Next folder > ``` 範例的輸出看起來如下所示。

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```



<!--HONumber=Mar16_HO1-->


