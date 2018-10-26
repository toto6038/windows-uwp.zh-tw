---
author: PatrickFarley
Description: Describes the structure of the 3D Manufacturing Format file type and how it can be created and manipulated with the Windows.Graphics.Printing3D API.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 產生 3MF 套件
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9d7acfdb6312770f51d8870549218344ad8c4330
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5547867"
---
# <a name="generate-a-3mf-package"></a>產生 3MF 套件

**重要 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx)

本指南描述 3D 製造格式文件的結構以及如何使用 [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx) API 來建立和操作文件。

## <a name="what-is-3mf"></a>什麼是 3MF？

3D 製造格式 (3MF) 是一組慣例，基於製造 (3D 列印) 而使用 XML 來描述 3D 模型的外觀和結構。
 它定義一組組件 (一些是必要組件，一些是選用組件) 與其關係，目標是將所有必要資訊提供給 3D 製造裝置。 遵守 3D 製造格式的資料集可以儲存為副檔名為 .3mf 的檔案。

在 windows 10， **Windows.Graphics.Printing3D**命名空間中的[**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.aspx)類別是單一.3mf 檔案類似，而其他類別則對應到檔案中的特定 XML 元素。 本指南描述如何透過程式設計方式建立和設定 3MF 文件的每個主要組件、如何使用 3MF 材質延伸，以及 **Printing3D3MFPackage** 物件如何轉換和儲存為 .3mf 檔案。 如需 3MF 或 3MF 材質延伸標準的詳細資訊，請參閱 [3MF 規格](http://3mf.io/what-is-3mf/3mf-specification/)。

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>3MF 結構中的核心類別

**Printing3D3MFPackage** 類別代表完整 3MF 文件，而 3MF 文件的核心是其模型組件 (以 [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmodel.aspx) 類別表示)。 大部分我們希望指定的 3D 模型相關資訊，都將透過設定 **Printing3DModel** 類別屬性和其基本類別屬性來加以儲存。

[!code-cs[InitClasses](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetInitClasses)]

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>中繼資料

3MF 文件的模型組件可以保留中繼資料，並以字串的機碼/值組形式儲存在 **Metadata** 屬性中。 會有一些預先定義的中繼資料名稱，但其他組可以新增為延伸的一部分 (詳述於 [3MF 規格](http://3mf.io/what-is-3mf/3mf-specification/)中)。 是由套件的接收者 (3D 製造裝置) 判斷是否處理中繼資料和其作法，但最好在 3MF 套件中包括最多的基本資訊：

[!code-cs[Metadata](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMetadata)]

## <a name="mesh-data"></a>網格資料

在本指南的內容中，網格是透過一組頂點所建構的 3D 幾何主體 (但它不需要顯示為單一純色)。
 網格部分是以 [**Printing3DMesh**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmesh.aspx) 類別表示。 有效的網格物件必須包含其所有頂點位置的資訊，以及存在於某些組頂點之間的所有三角形表面位置的資訊。

下列方法會新增網格的頂點，然後提供它們在 3D 空間中的位置︰

[!code-cs[Vertices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetVertices)]

下一種方法定義要跨這些頂點繪製的所有三角形︰

[!code-cs[TriangleIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTriangleIndices)]

> [!NOTE]
> 所有三角形都必須以逆時鐘順序定義其索引 (從網格物件外部檢視三角形時)，使其 face-normal 向量指向外部。

Printing3DMesh 物件包含一組有效的頂點和三角形時，應該將它新增至模型的 **Meshes** 屬性。 套件中的所有 **Printing3DMesh** 物件都必須儲存至 **Printing3DModel** 類別的 **Meshes** 屬性。

[!code-cs[MeshAdd](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMeshAdd)]


## <a name="create-materials"></a>建立材質


3D 模型可以保留多個材質的資料。 這個慣例是要利用可對單一列印工作使用多個材質的 3D 製造裝置。 也有多種*類型*的材質群組，各可以支援數個不同的個別材質。 每個材質群組都必須有唯一的參考識別碼，而且該群組內的每一種材質也必須有唯一識別碼。

模型內的不同網格物件接著可以參考這些材質。 此外，每個網格上的個別三角形可以指定不同的材質。 甚至，不同的材質可在單一三角形內表示，而且每個三角形頂點都已獲指派不同的材質，而且表面材質計算為其中的漸層。

本指南會先示範如何在其各自的材質群組內建立不同類型的材質，並將其儲存為模型物件上的資源。 接著，我們會將不同的材質指派給個別網格和個別三角形。

### <a name="base-materials"></a>基本材質

預設材質類型是 [**基本材質**]，其具有 [**色彩材質**] 值 (如下所述) 以及用來指定要使用的材質*類型*的 name 屬性。

[!code-cs[BaseMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetBaseMaterialGroup)]

> [!NOTE]
> 3D 製造裝置將決定哪些可用的實體材質對應到 3MF 中儲存的哪些虛擬材質元素。 材質對應不一定要是 1:1︰如果 3D 印表機只使用一個材質，則會使用該材質列印整個模型 (不管物件或表面已獲指派不同的材質)。

### <a name="color-materials"></a>色彩材質

**色彩材質**與**基本材質**類似，但不包含名稱。 因此，它們不會提供有關電腦應該使用的材質類型的指示。 它們只會保留色彩資料，並讓電腦選擇材質類型 (而電腦接著可能會提示使用者進行選擇)。 在下面的程式碼中，會自行使用先前方法中的 `colrMat` 物件。

[!code-cs[ColorMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetColorMaterialGroup)]

### <a name="composite-materials"></a>複合材質

**複合材質**只會指示製造裝置使用不同**基底材質**的統一混合。 每個**複合材質群組**都只能參考一個用來繪製組成部分的**基本材質群組**。 此外，此群組內要設為可用的**基本材質**必須列在**材質索引**清單中，而每個**複合材質**在指定比例時都會參考該清單 (每個**複合材質**都只是**基本材質**的比例)。

[!code-cs[CompositeMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetCompositeMaterialGroup)]

### <a name="texture-coordinate-materials"></a>紋理座標材質

3MF 支援使用 2D 影像來將 3D 模型的表面著色。 因此，模型的每個三角形表面可以傳送更多色彩資料 (而不是每個三角形頂點只有一個色彩值)。 與**色彩材質**類似，紋理座標材質只會傳送色彩資料。 若要使用 2D 紋理，必須先宣告紋理資源︰

[!code-cs[TextureResource](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTextureResource)]

> [!NOTE]
> 紋理資料屬於 3MF 套件本身，而不屬於套件內的模型組件。

接下來，我們必須填寫 **Texture3Coord 材質**。 所有這些項目都參考紋理資源，並指定影像上的特定點 (UV 座標)。

[!code-cs[Texture2CoordMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTexture2CoordMaterialGroup)]

## <a name="map-materials-to-faces"></a>將材質對應到表面

為了規定哪些材質對應到每個三角形上的哪些頂點，我們必須對我們模型的網格物件執行一些其他工作 (如果模型包含多個網格，則它們必須各有其分別指派的材質)。 如上所述，會根據每個頂點、每個三角形來指派材質。 請參閱下面的程式碼，以查看如何輸入和解譯這項資訊。

[!code-cs[MaterialIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMaterialIndices)]

## <a name="components-and-build"></a>元件和建置

元件結構可讓使用者在可列印的 3D 模型中放入多個網格物件。 [**Printing3DComponent**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponent.aspx) 物件包含單一網格以及其他元件的參考清單。 這實際上是 [**Printing3DComponentWithMatrix**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponentwithmatrix.aspx) 物件的清單。 **Printing3DComponentWithMatrix** 物件各包含一個 **Printing3DComponent**，而且重要的是套用至網格的轉換矩陣以及 **Printing3DComponent** 的內含元件。

例如，汽車模型可能包含保留汽車主體網格的 "Body" **Printing3DComponent**。 "Body" 元件接著可能會包含對四個不同 **Printing3DComponentWithMatrix** 物件的參考，這四個物件都參考具有 "Wheel" 網格的相同 **Printing3DComponent**，並且包含四個不同的轉換矩陣 (將車輪對應到汽車主體的四個不同位置)。 在這個案例中，"Body" 網格和 "Wheel" 網格各只需要儲存一次，即使最終產品共具備五個網格也是一樣。

所有 **Printing3DComponent** 物件都必須在模型的 **Components** 屬性中直接參考。 要用在列印工作的一個特定元件儲存在 **Build** 屬性中。

[!code-cs[Components](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetComponents)]

## <a name="save-package"></a>儲存套件
現在，我們已經有一個具有已定義材質和元件的模型，可以將它儲存到套件中。

[!code-cs[SavePackage](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSavePackage)]

接下來，我們可以起始應用程式內的列印工作 (請參閱[從應用程式進行 3D 列印](https://msdn.microsoft.com/library/windows/apps/mt204541.aspx))，或將此 **Printing3D3MFPackage** 儲存為 .3mf 檔案。

下列方法採用已完成的 **Printing3D3MFPackage**，並將其資料儲存至 .3mf 檔案。

[!code-cs[SaveTo3mf](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSaveTo3mf)]

## <a name="related-topics"></a>相關主題

[從 App 進行 3D 列印](https://msdn.microsoft.com/windows/uwp/devices-sensors/3d-print-from-app)  
[3D 列印 UWP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 
