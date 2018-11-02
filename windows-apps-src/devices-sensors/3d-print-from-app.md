---
author: PatrickFarley
title: 從 app 進行 3D 列印
description: 了解如何將 3D 列印功能加入通用 Windows 應用程式。 本主題涵蓋如何在確保 3D 模型為可列印且為正確的格式之後，啟動 3D 列印對話方塊。
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，3dprinting，3d 列印
ms.localizationpriority: medium
ms.openlocfilehash: 818e1338a1d36d24990f22316dc2072c2c0d7cc5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5946334"
---
# <a name="3d-printing-from-your-app"></a>從應用程式進行 3D 列印

**重要 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

了解如何將 3D 列印功能加入通用 Windows 應用程式。 本主題涵蓋如何將 3D 幾何資料載入 app，以及如何在確保 3D 模型為可列印且為正確的格式之後，啟動 3D 列印對話方塊。 如需這些程序運作的範例，請參閱 [3D 列印 UWP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)。

> [!NOTE]
> 為了保持簡潔，本指南中的範例程式碼、錯誤報告及處理已大幅簡化。

## <a name="setup"></a>設定


在需要 3D 列印功能的應用程式類別中，加入 [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169) 命名空間。

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

下列其他的命名空間，將會在這個指南中使用。

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

接下來，為您的類別提供實用的成員欄位。 宣告 [**Print3DTask**](https://msdn.microsoft.com/library/windows/apps/dn998044) 物件以代表要傳遞至列印驅動程式的列印工作參考。 宣告 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件來保存將載入到 App 的原始 3D 資料檔案。 宣告 [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/dn998063) 物件，這個物件代表含有所有必要中繼資料的列印就緒 3D 模型。

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>建立簡單的 UI

這個範例具備三個使用者控制項：將檔案帶入程式記憶體的 \[載入\] 按鈕、會視需要修改檔案的 \[修正\] 按鈕，以及初始化列印工作的 \[列印\] 按鈕。 下列程式碼會在您 .cs 類別對應的 XAML 檔案中建立這些按鈕 (包含其點擊事件處理常式)。

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

加入 **TextBlock** 以用於 UI 回饋。

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>取得 3D 資料


您的 App 取得 3D 幾何資料的方法會有差異。 您 App 擷取資料的方式可能是 3D 掃描、從網路資源下載模型資料，或以程式設計方式使用數學公式或使用者輸入來產生 3D 網格。 為了簡單起見，本指南會示範從裝置存放空間將 3D 資料檔案 (任何常見的檔案類型) 載入程式記憶體。 [3D Builder 模型庫](https://developer.microsoft.com/windows/hardware/3d-builder-model-library)提供您輕鬆就能下載到裝置上的各種模型。

在 `OnLoadClick` 方法中，使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 類別將單一檔案載入至 App 記憶體。

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>使用 3D Builder 轉換為 3D 製造格式 (.3mf)

這時，您就可以將 3D 資料檔案載入 app 的記憶體。 不過，3D 幾何資料有許多不同格式，且對 3D 列印並非都有效率。 Windows 10 對於所有的 3D 列印工作皆使用 3D 製造格式 (.3mf) 檔案類型。

> [!NOTE]  
> .3mf 檔案類型提供多於本教學課程可涵蓋的功能。 若要深入了解 3MF 及它提供給 3D 產品生產者與消費者的功能，請參閱 [3MF 規格](http://3mf.io/what-is-3mf/3mf-specification/)。 若要了解如何以 Windows 10 API 運用這些功能，請參閱[產生 3MF 套件](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)教學課程。

[3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) App 可以開啟大部分熱門的 3D 格式，並另存為 .3mf 檔案。 在這個範例中，當檔案類型有所不同時，一種非常簡單的解決方案是開啟 3D Builder App 並提示使用者將匯入的資料另存為 .3mf 檔案，然後重新載入。

> [!NOTE]  
> 除了轉換檔案格式之外，3D Builder 提供簡單的工具來編輯模型、新增顏色資料，以及執行其他列印特定作業，因此通常值得將它整合到處理 3D 列印的 app 中。

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>修復模型資料以供 3D 列印

並非所有 3D 模型資料都可以列印，即使是 .3mf 類型也一樣。 若要讓印表機正確地判斷要填滿的空間與要保留的空白，(每個) 列印的模型必須是單一無縫的網格、具有對外區面法線，且具有多面幾何。 這些區域中的問題可能會以多種不同形式出現，且在複雜圖形中難以察覺。 不過，新式的軟體解決方案通常可將原始幾何檔案轉換為可列印的 3D 圖形。 這稱為將模型「修復」**，它會在 `OnFixClick` 方法中完成。

必須將 3D資料檔案轉換以實作 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)，這可接著用來產生 [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/mt203679) 物件。

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

**Printing3DModel** 物件現在已經修復且可供列印。 使用 [**SaveModelToPackageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) 將模型指派到您建立類別時宣告的 **Printing3D3MFPackage** 物件。

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>執行列印工作︰建立 TaskRequested 處理常式


稍後對使用者顯示 3D 列印對話方塊且使用者選擇開始列印時，您的 app 必須將所需的參數傳遞到 3D 列印管線。 3D 列印 API 會引發 **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** 事件。 您必須撰寫一個方法來適當地處理這個事件。 如同以往，處理常式方法必須與其事件類型相同︰**TaskRequested** 事件有參數 [**Print3DManager**](https://msdn.microsoft.com/library/windows/apps/dn998029) (其傳送者物件的參考) 和會保存大部分相關資訊的 [**Print3DTaskRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn998051) 物件。

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

這個方法的核心目的是使用 *args* 參數來將 **Printing3D3MFPackage** 向下傳送到管線。 **Print3DTaskRequestedEventArgs** 類型有一個屬性︰[**Request**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequestedeventargs.request.aspx)。 它是 [**Print3DTaskRequest**](https://msdn.microsoft.com/library/windows/apps/dn998050) 類型，並代表一個列印工作要求。 它的方法 [**CreateTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequest.createtask.aspx) 可讓程式為列印工作送出正確資訊，而且它會傳回向下傳遞到 3D 列印管線之 **Print3DTask** 物件的參考。

**CreateTask** 具有下列輸入參數：列印工作名稱的字串、要使用之印表機的識別碼字串，及 [**Print3DTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedhandler.aspx) 委派。 當引發 **3DTaskSourceRequested** 事件時，會自動叫用委派 (這會由 API 本身完成)。 要注意的重點是，這個委派在列印工作初始化時即已叫用，它會負責提供正確的 3D 列印封包。

**Print3DTaskSourceRequestedHandler** 會接受一個參數，該參數提供要傳遞之資料的 [**Print3DTaskSourceRequestedArgs**](https://msdn.microsoft.com/library/windows/apps/dn998056) 物件。 這個類別的唯一公用方法 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource.aspx) 會接受要列印的封包。 實作 **Print3DTaskSourceRequestedHandler** 委派，如下所示。

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

接著，使用新定義的委派 `sourceHandler` 呼叫 **CreateTask**。

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

傳回的 **Print3DTask** 會指派到在開頭宣告的類別變數。 您現在可以 (選擇性地) 使用此參考，來處理工作擲回的特定事件。

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> 如果您希望將 `Task_Submitting` 和 `Task_Completed` 方法登錄到這些事件，您必須實作它們。

## <a name="execute-printing-task-open-3d-print-dialog"></a>執行列印工作︰開啟 3D 列印對話方塊


所需的最終一段程式碼，是啟動 3D 列印對話方塊。 如同傳統的列印對話方塊視窗，3D 列印對話方塊提供數個最終列印選項，並讓使用者選取要使用的印表機 (無論是透過 USB 或網路連接)。

將您的 `MyTaskRequested` 方法向 **TaskRequested** 事件登錄。

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

登錄您的 **TaskRequested** 事件處理常式後，您可以叫用 [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dmanager.showprintuiasync.aspx) 方法，這會在目前的應用程式視窗中顯示 3D 列印對話方塊。

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

最後，當 App 繼續控制時，解除登錄事件處理常式是很好的做法。  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>相關主題

[產生 3MF 套件](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[3D 列印 UWP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
