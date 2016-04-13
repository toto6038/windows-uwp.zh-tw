---
title: 從 app 進行 3D 列印
description: 了解如何將 3D 列印功能加入通用 Windows app。 本主題涵蓋如何在確保 3D 模型為可列印且為正確的格式之後，啟動 3D 列印對話方塊。
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
---

# 從 app 進行 3D 列印


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

了解如何將 3D 列印功能加入通用 Windows app。 本主題涵蓋如何在確保 3D 模型為可列印且為正確的格式之後，啟動 3D 列印對話方塊。

## 類別設定


在需要 3D 列印功能的類別中，加入 [Windows.Graphics.Printing3D](https://msdn.microsoft.com/library/windows/apps/dn998169) 命名空間。

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

下列其他的命名空間，將會在這個特定的指南中使用：

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

接下來，為您的類別提供一些有用的成員欄位。 宣告 [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) 物件做為要傳遞至列印驅動程式的列印工作參考。 宣告 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 物件以保留原始 3D 資料檔案。 最後，宣告 [Printing3D3MFPackage](https://msdn.microsoft.com/library/windows/apps/dn998063) 物件，這個物件代表含有所有必要之中繼資料的列印就緒 3D 模型。

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## 建立簡單的 UI


這個範例使用三個使用者控制項：將檔案帶入程式記憶體的讀取按鈕、會視需要修改檔案的修正按鈕，以及會初始化列印工作的列印按鈕。 下列程式碼會在您類別的 XAML 檔案中產生這些按鈕 (包含 Click 事件處理常式)：

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

加入 **TextBlock** 以取得 UI 回饋。

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]

## 取得 3D 資料


此方法依據您 app 取得 3D 幾何資料進行列印的方式而異。 您 app 擷取資料的方式可能是 3D 掃描、從網路資源提取模型資料，或使用方程式以程式設計方式產生 3D 網格。 為了簡單起見，本指南從 [檔案總管] 將 3D 資料檔案 (任何常見的檔案類型) 載入程式記憶體。

在 `OnLoadClick` 方法中，使用 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 類別將單一檔案載入至 app 記憶體。

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## 使用 3D Builder 轉換為 3D 製造格式 (.3mf)

這時，您就可以將 3D 資料檔案載入 app 的記憶體。 不過，3D 幾何資料有許多不同格式，且對 3D 列印並非都有效率。 Windows 10 對於所有的 3D 列印工作皆使用 3D 製造格式 (.3mf) 檔案類型。

> **注意** 3MF 檔案類型提供大量的功能，但本教學課程無法涵蓋。 若要深入了解 3MF 及其提供給 3D 產品生產者與消費者的功能，請參閱 [3MF 規格](http://3mf.io/what-is-3mf/3mf-specification/)。

幸運的是，[3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) app 可以開啟大部分熱門的 3D 格式，並另存為 .3mf 檔案。 在這個範例中，當檔案類型有所不同時，一種非常簡單的解決方案是開啟 3D Builder 並提示使用者將匯入的檔案另存為 .3mf 檔案，然後重新載入。

> **注意** 除了轉換檔案格式，**3D Builder** 提供簡單的工具來編輯模型、新增顏色資料，並執行其他列印特定的作業，因此通常值得將它整合到處理 3D 列印的 app 中。

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## 修復模型資料以供 3D 列印

即使是 .3mf 格式，也並非所有的 3D 模型資料都可以列印。 若要讓印表機正確地判斷要填滿的空間與要保留的空白，列印的模型必須是單一無縫的網格、具有對外區面法線，且具有多面幾何。 這些區域中的問題可能會以多種不同形式出現，且在複雜圖形中難以察覺。 幸運的是，目前的軟體解決方案通常可將原始幾何檔案轉換為可列印的 3D 圖形。 這可在 `OnFixClick` 方法中完成。

必須將 3D資料檔案轉換以實作 [IRandomAccessStream](https://msdn.microsoft.com/library/windows/apps/br241731)，這可接著用來產生 [Printing3DModel](https://msdn.microsoft.com/library/windows/apps/mt203679) 物件。

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

**Printing3DModel** 物件現在已經修復且可供列印。 當建立類別時，使用 **SaveModelToPackageAsync** 以指派模型到您宣告的 Printing3D3MFPackage 物件。

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## 執行列印工作︰建立 TaskRequested 處理常式


稍後對使用者顯示 3D 列印對話方塊且使用者選擇開始列印時，您的 app 必須將所需的參數傳遞到 3D 列印管線。 3D 列印 API 會引發 **TaskRequested** 事件。 您必須撰寫一個方法來適當地處理這個事件。 如同以往，它必須與其事件類型相同︰**TaskRequested** 事件有參數 [Print3DManager](https://msdn.microsoft.com/library/windows/apps/dn998029) (其傳送者物件) 和會保留大部分的相關資訊的 [Print3DTaskRequestedEventArgs](https://msdn.microsoft.com/library/windows/apps/dn998051) 物件。 它傳回的類型是 **void**。

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

這個方法的核心目的是使用 *args* 物件來將 **Printing3D3MFPackage** 向下傳送到管線。 **Print3DTaskRequestedEventArgs** 類型都有一個屬性︰**Request**。 它是 [Print3DTaskRequest](https://msdn.microsoft.com/library/windows/apps/dn998050) 類型，並代表一個列印工作要求。 它的方法 **CreateTask** 可讓程式為列印工作送出正確資訊，而且它會傳回向下傳遞到 3D 列印管線之 [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) 物件的參考。

**CreateTask** 具有下列輸入參數：針對列印工作名稱的**字串**、針對要使用之印表機識別碼的**字串**，及 **Print3DTaskSourceRequestedHandler** 委派。 當引發 **3DTaskSourceRequested** 事件時，會自動叫用委派 (這會由 API 本身完成)。 要注意的重點是，這個委派在列印工作初始化時即已叫用，它會負責提供正確的 3D 列印封包。

**Print3DTaskSourceRequestedHandler** 會採用一個參數，提供要傳遞之資料的 [Print3DTaskSourceRequestedArgs](https://msdn.microsoft.com/library/windows/apps/dn998056) 物件。 這個類別的其中一個公用方法 **SetSource** 會接受要列印的封包。 實作 **Print3DTaskSourceRequestedHandler** 委派，如下所示：

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

接著，使用新定義的代理人 `sourceHandler` 呼叫 **CreateTask**：

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

傳回的 **Print3DTask** 會指派到開頭宣告的類別變數。 您現在可以 (選擇性地) 使用此參考，來處理工作擲回的特定事件：

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> **注意** 如果您希望將 `Task_Submitting` 和 `Task_Completed` 方法登錄到這些事件，您必須實作它們。

## 執行列印工作︰開啟 3D 列印對話方塊


從您的 app 列印所需的最終一段程式碼，是啟動 3D 列印對話方塊。 如同傳統的列印對話方塊視窗，3D 列印對話方塊提供數個最終列印規格，並讓使用者選取要使用的印表機 (無論是透過 USB 或網路連接)。

首先，將您的 `MyTaskRequested` 方法向 **TaskRequested** 事件登錄。

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

登錄您的 **TaskRequested** 事件處理常式後，您可以叫用 **ShowPrintUIAsync** 方法，這會在目前的應用程式視窗中顯示 3D 列印對話方塊。

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

最後，當 app 繼續控制時，解除登錄事件處理常式是很好的做法。
[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]


 

 






<!--HONumber=Mar16_HO5-->


