---
title: 在 DirectX 遊戲中載入資源
description: 大多數的遊戲會在某個時間點從本機存放區或一些其他資料串流中載入資源和資產 (如著色器、紋理、預先定義的網格或其他圖形資料)。
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, directx, 載入資源
ms.localizationpriority: medium
ms.openlocfilehash: ca16dd6115bbbe84529928ca58ee0d3074498728
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8206896"
---
# <a name="load-resources-in-your-directx-game"></a>在 DirectX 遊戲中載入資源



大多數的遊戲會在某個時間點從本機存放區或一些其他資料串流中載入資源和資產 (如著色器、紋理、預先定義的網格或其他圖形資料)。 本文會引導您了解在 DirectX C/C++ 通用 Windows 平台 (UWP) 遊戲中載入這些檔案來使用時必須考量的整體事項。

例如，您可能使用另一種工具建立遊戲中多邊形物件的網格，然後再以特定格式匯出。 紋理也是一樣 (甚至更是如此)：雖然通常可以使用大多數的工具來撰寫多數圖形 API 都能辨識的一般未壓縮點陣圖，但是用在遊戲中卻非常沒有效率。 本文說明載入下列三種不同類型的圖形資源以搭配 Direct3D 使用時的基本步驟：網格 (模型)、紋理 (點陣圖) 及編譯的著色器物件。

## <a name="what-you-need-to-know"></a>您需要知道的事項


### <a name="technologies"></a>技術

-   平行模式程式庫 (ppltasks.h)

### <a name="prerequisites"></a>先決條件

-   了解基本 Windows 執行階段
-   了解非同步工作
-   了解 3D 圖形程式設計的基本概念。

這個範例也包含資源載入和管理的三個程式碼檔案。 您將在本主題中遇到定義於這些檔案中的程式碼物件。

-   BasicLoader.h/.cpp
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

您可以在下列連結中找到這些範例的完整程式碼。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-basicloader.md">BasicLoader 的完整程式碼</a></p></td>
<td align="left"><p>將圖形網格物件轉換並載入記憶體的類別與方法的完整程式碼。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="complete-code-for-basicreaderwriter.md">BasicReaderWriter 的完整程式碼</a></p></td>
<td align="left"><p>讀取和寫入二進位資料檔的一般類別與方法的完整程式碼。 專供 <a href="complete-code-for-basicloader.md">BasicLoader</a> 類別使用。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-ddstextureloader.md">DDSTextureLoader 的完整程式碼</a></p></td>
<td align="left"><p>從記憶體載入 DDS 紋理的類別與方法的完整程式碼。</p></td>
</tr>
</tbody>
</table>

 

## <a name="instructions"></a>指示

### <a name="asynchronous-loading"></a>非同步載入

非同步載入使用平行模式程式庫 (PPL) 中的 **task** 範本進行處理。 **task** 包含一個後面跟著 Lambda 的方法呼叫，可以在完成後處理非同步呼叫的結果，格式通常如下：

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

使用 **.then()** 語法可以將多個工作鏈結在一起，因此，當一個作業完成後，便可執行相依於前一作業之結果的另一個非同步操作。 如此一來，您就能以玩家幾乎看不見的方式，載入、轉換及管理個別執行緒上的複雜資產。

如需詳細資訊，請閱讀 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

現在，我們來看看宣告和建立非同步檔案載入方法 **ReadDataAsync** 的基本結構。

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

在這個程式碼中，當您的程式碼呼叫上述定義的 **ReadDataAsync** 方法時，會建立一個從檔案系統讀取緩衝區的工作。 完成這個工作後，鏈結的工作會擷取緩衝區，並使用靜態的 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 類型將該緩衝區中的位元組串流處理到陣列。

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

下列是您對 **ReadDataAsync** 進行的呼叫。 呼叫完成後，您的程式碼會收到讀取自提供的檔案的位元組陣列。 由於 **ReadDataAsync** 本身定義為工作，因此當位元組陣列傳回時，您可以使用 Lambda 來執行特定的作業，像是將該位元組資料傳送到可使用該資料的 DirectX 函式。

如果您的遊戲夠簡單，請在使用者開始遊戲時，使用類似這個的方法來載入您的資源。 您可以在 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 實作的呼叫順序中開始主要遊戲迴圈之前，先執行此動作。 再次提醒您以非同步方式呼叫資源載入方法，如此可讓遊戲更快速地開始，而玩家不需等到載入完成，就能提早開始與遊戲互動。

但是，不要在所有非同步載入完成之前就開始遊戲！ 建立一些方法在載入完成後通知使用者 (例如特定的欄位)，並在完成時使用載入方法中的 Lambda 來設定該通知。 啟動使用那些載入資源的任一元件之前，請先檢查變數。

下列範例會在遊戲啟動時，使用定義在 BasicLoader.cpp 中的非同步方法來載入著色器、網格及紋理。 請注意，當所有載入方法完成後，它會在遊戲物件 **m\_loadingComplete** 中設定一個特定欄位。

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

請注意，工作已經使用 &amp;&amp; 運算子彙總，因此只有在所有工作完成後，才會觸發設定載入完成旗標的 Lambda。 請注意，如果您有多個旗標，可能會生競爭情況。 例如，假設 Lambda 連續將兩個旗標設為相同的值，如果另一個執行緒在第二個旗標設定之前就檢查了旗標，則可能只會看到設定的第一個旗標。

您已經了解如何以非同步方式載入資源檔案。 以同步方式載入檔案就更簡單了，您可以在 [BasicReaderWriter 的完整程式碼](complete-code-for-basicreaderwriter.md)和 [BasicLoader 的完整程式碼](complete-code-for-basicloader.md)中找到它們的範例。

當然，不同的資源和資產類型通常必須先進行額外的處理或轉換，才能用在您的圖形管線中。 我們來看看三個特定類型的資源：網格、紋理及著色器。

### <a name="loading-meshes"></a>載入網格

網格是頂點資料，可以是由遊戲的程式碼產生，或是從另一個應用程式 (像是 3DStudio MAX 或 Alias WaveFront) 或工具匯出到檔案。 這些網格代表您遊戲中的模型，可能是像立方體和球體的簡單基本類型，或者是房子或人物。 依格式而定，它們通常也包含色彩與動畫資料。 我們將重點放在僅包含頂點資料的網格。

為正確載入網格，您必須知道網格檔案中的資料格式。 我們上述的簡易 **BasicReaderWriter** 類型只會讀入位元組串流格式的資料；它並不知道位元組資料代表網格，更別說是由另一個應用程式所匯出的特定網格格式！ 當您將網格資料放入記憶體時必須執行轉換。

(您應該永遠盡可能地將資產資料以接近內部表示法的格式封裝。 這樣可以減少資源使用量並節省時間。)

讓我們從網格的檔案取得位元組資料吧。 範例中的格式假設檔案是尾碼 .vbo 的範例特定格式  (再次提醒，這個格式與 OpenGL's VBO 格式不同)。每個頂點本身均對應 **BasicVertex** 類型，那是定義在 obj2vbo 轉換器工具程式碼中的結構。 .vbo 檔案中的頂點資料版面配置看起來像這樣：

-   資料串流的前 32 個位元 (4 個位元組) 包含網格中的頂點數目 (numVertices)，以一個 uint32 值表示。
-   資料串流的後 32 個位元 (4 個位元組) 包含網格中的索引數目 (numIndices)，以一個 uint32 值表示。
-   在那之後，後續的 (numVertices \* sizeof(**BasicVertex**)) 位元則包含頂點資料。
-   資料最後的 (numIndices \* 16) 位元包含索引資料，以一連串的 uint16 值表示。

重點在於：了解您載入的網格資料的位元層級版面配置。 此外，也請確定使用一致的位元組順序。 所有 Windows8 平台都均是位元組由小到大。

在範例中，您從 **LoadMeshAsync** 方法中呼叫方法 CreateMesh 以執行此位元層級的解譯。

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**CreateMesh** 會解譯從檔案載入的位元組資料，然後會分別將頂點和索引清單傳送到 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)，並指定 D3D11\_BIND\_VERTEX\_BUFFER 或 D3D11\_BIND\_INDEX\_BUFFER，進而為網格建立頂點緩衝區和索引緩衝區。 下列是 **BasicLoader** 中使用的程式碼：

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

您通常會為遊戲所使用的每個網格建立一個頂點/索引緩衝區組。 您可以自行決定載入網格的位置和時機。 如果您有許多網格，則可能只想在遊戲的某些特定時間點從磁碟中載入一些網格，例如在特定的預先定義載入狀態期間。 對於大型網格 (如地形資料)，您可以從快取中串流處理頂點，但那是更複雜的程序，不在本主題的範圍內。

再次提醒您，請了解您的頂點資料格式！ 在用來建立模型的許多工具中，有非常多的方法可以呈現頂點資料。 也有許多不同的方法可以將頂點資料的輸入配置呈現為 Direct3D，像是三角形清單和寬帶。 如需頂點資料的詳細資訊，請閱讀 [Direct3D 11 中的緩衝區簡介](https://msdn.microsoft.com/library/windows/desktop/ff476898)與[基元](https://msdn.microsoft.com/library/windows/desktop/bb147291)。

接下來，讓我們來看看載入紋理。

### <a name="loading-textures"></a>載入紋理

遊戲中最常見的資產 (和組成磁碟和記憶體中大多數檔案的資產) 就是紋理。 紋理和網格一樣也有多種格式，您會在載入它們時將它們轉換成 Direct3D 可以使用的格式。 紋理也有多種類型，並可用來建立不同的效果。 紋理的 MIP 層級可用來改善距離物件的外觀與效能；污漬貼圖和光照貼圖可用來層疊效果，而細節可放在基本紋理上層；一般貼圖則用於每一像素光源計算。 在新型的遊戲中，一個典型的場景可能有數千種個別的紋理，您的程式碼必須有效地管理所有的紋理！

也和網格一樣，有多種特定格式可用來節省記憶體使用量。 由於紋理很容易便會耗用大量的 GPU (和系統) 記憶體，因此通常會以某些方式壓縮。 您不需在遊戲的紋理中使用壓縮，只要您提供 Direct3D 著色器可以理解之格式 (像是 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 點陣圖) 的資料，就可以使用您想要的任何壓縮/解壓縮演算法。

Direct3D 可為 DXT 紋理壓縮演算法提供支援，然而玩家的圖形硬體未必能夠支援每種 DXT 格式。 DDS 檔案包含 DXT 紋理 (還有其他紋理壓縮格式)，尾碼為 .dds。

DDS 檔案是包含下列資訊的二進位檔案：

-   包含四個字元的代碼值 'DDS ' (0x20534444) 的 DWORD (魔術數字)。

-   檔案中的資料描述。

    資料會使用 [**DDS\_HEADER**](https://msdn.microsoft.com/library/windows/desktop/bb943982) 的標頭描述來加以描述；而像素格式則是使用 [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) 加以定義。 請注意，**DDS\_HEADER** 和 **DDS\_PIXELFORMAT** 結構取代了已過時的 DDSURFACEDESC2、DDSCAPS2 及 DDPIXELFORMAT DirectDraw 7 結構。 **DDS\_HEADER** 等同於 DDSURFACEDESC2 與 DDSCAPS2 的二進位檔案。 **DDS\_PIXELFORMAT** 是等同於 DDPIXELFORMAT 的二進位檔案。

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    如果 [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) 中的 **dwFlags** 值設為 DDPF\_FOURCC，而 **dwFourCC** 設為 "DX10"，額外的 [**DDS\_HEADER\_DXT10**](https://msdn.microsoft.com/library/windows/desktop/bb943983) 結構將會存在以便配合無法以 RGB 像素格式 (例如浮點數格式及 sRGB 格式等) 表示的紋理陣列或 DXGI 格式。當 **DDS\_HEADER\_DXT10** 結構存在時，整個資料描述看起來會像這樣。

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   包含主要表面資料的位元組陣列指標。
    ```cpp
    BYTE bdata[]
    ```

-   包含其餘的表面 (如 Mipmap 層級、立方體貼圖中的面及容積紋理中的深度) 的位元組陣列指標。 請點選下列連結，以了解下列的 DDS 檔案配置相關資訊：[紋理](https://msdn.microsoft.com/library/windows/desktop/bb205578)、[立方體貼圖](https://msdn.microsoft.com/library/windows/desktop/bb205577)或[容積紋理](https://msdn.microsoft.com/library/windows/desktop/bb205579)。

    ```cpp
    BYTE bdata2[]
    ```

許多工具會匯出 DDS 格式。 如果您沒有可將紋理匯出為這個格式的工具，請考慮建立一個工具。 如需 DDS 格式和如何在您的程式碼中使用它的詳細資訊，請閱讀 [DDS 的程式設計指南](https://msdn.microsoft.com/library/windows/desktop/bb943991)。 在我們的範例中，我們將使用 DDS。

和其他資源類型一樣，您會從檔案中以位元組串流的形式讀取此資料。 在您的載入工作完成後，Lambda 呼叫會執行程式碼 (**CreateTexture** 方法)，以將位元組的串流處理為 Direct3D 可使用的格式。

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

在先前的程式碼片段中，Lambda 會查看檔案名稱是否包含副檔名 "dds"。 如果包含，假設其為 DDS 紋理。 如果不包含，則使用 Windows 影像處理元件 (WIC) API 來探索格式，並將資料解碼為點陣圖。 無論是何種方法，結果都會是 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 點陣圖 (或錯誤)。

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

當此程式碼完成後，您的記憶體中就會有從影像檔載入的 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)。 和網格一樣，您的遊戲和任一個場景中都會包含很多紋理。 請考慮為經常存取的每一場景或每一關卡紋理建立快取，而不要在遊戲或關卡開始時全部載入它們。

(上述範例中呼叫的 **CreateDDSTextureFromMemory** 方法將在 [DDSTextureLoader 的完整程式碼](complete-code-for-ddstextureloader.md)中完整探討。)

此外，個別的紋理或紋理「面板」可能會與特定的網格多邊形或表面對應。 此對應資料通常是由美術人員或設計師用來建立模型和紋理的工具所匯出的。 請確定在載入匯出的資料時也會擷取此資訊，因為當您執行片段著色時，將會使用它來對應正確的紋理和對應的表面。

### <a name="loading-shaders"></a>載入著色器

著色器是編譯的高階著色器語言 (HLSL) 檔案，它會載入記憶體，並在圖形管線的特定階段叫用。 最常見和基本的著色器是頂點和像素著色器，它們分別會處理網格中的個別頂點和場景檢視區中的像素。 執行 HLSL 程式碼可轉換幾何、套用光源效果與紋理，以及對轉譯的場景執行後續處理。

Direct3D 遊戲可以有數種不同的著色器，每種各編譯成不同的 CSO (編譯的著色器物件，.cso) 檔案。 您的著色器通常不會多到需要動態載入它們，在大多數情況下，您只需要在遊戲開始時或在每個關卡開始時載入它們即可 (像是下雨效果的著色器)。

**BasicLoader** 類別中的程式碼為不同的著色器 (包括頂點、幾何、像素和輪廓著色器) 提供數個超載。 下列程式碼以像素著色器為例。 (您可以在 [BasicLoader 的完整程式碼](complete-code-for-basicloader.md)中檢視完整的程式碼。)

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

在此範例中，您使用 **BasicReaderWriter** 執行個體 (**m\_basicReaderWriter**) 以位元組串流的形式讀取提供的編譯著色器物件 (.cso) 檔案。 在該工作完成後，Lambda 會以從檔案載入的位元組資料呼叫 [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)。 您的回呼必須設定一些旗標以指示載入完成，而您的程式碼必須在執行著色器之前檢查此旗標。

頂點著色器就比較複雜一些。 在頂點著色器中，您也必須載入定義頂點資料的個別輸入配置。 您可以使用下列程式碼，以非同步方式載入頂點著色器和自訂的頂點輸入配置。 請確定此輸入配置可以正確地呈現您從網格載入的頂點資訊！

在載入頂點著色器之前，讓我們先建立輸入配置。

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

在這個配置中，每個頂點的下列資料是由頂點著色器所處理：

-   模型座標空間中的 3D 座標位置 (x, y, z)，以三個為一組的 32 位元浮點數值表示。
-   頂點的一般向量，也是以三個 32 位元浮點數值表示。
-   轉換的 2D 紋理座標值 (u, v)，以一對 32 位元的浮點數值表示。

這些每一頂點的輸入元素稱為 [HLSL 語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)，它們是一組定義的登錄，用於傳送資料到您編譯的著色器物件，或從您編譯的著色器物件傳送資料。 您的管線會為您載入的每個網格頂點執行一次頂點著色器。 語意會在頂點著色器執行時定義它的輸入 (和輸出)，並為著色器的 HLSL 程式碼中的每一頂點運算提供此資料。

現在，請載入頂點著色器物件。

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}

```

在此程式碼中，一旦您讀取頂點著色器 CSO 檔案的位元組資料，就會呼叫 [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 來建立頂點著色器。 在這之後，您要在相同的 Lambda 中建立著色器的輸入配置。

其他著色器類型 (例如輪廓和幾何著色器) 可能也需要進行特定的設定。 在 [BasicLoader 的完整程式碼](complete-code-for-basicloader.md)和 [Direct3D 資源載入範例]( http://go.microsoft.com/fwlink/p/?LinkID=265132)中，提供了多種著色器載入方法的完整程式碼。

## <a name="remarks"></a>備註

此時，您應該了解並能夠建立或修改非同步載入一般遊戲資源和資產 (如網格、紋理及編譯的著色器) 的方法。

## <a name="related-topics"></a>相關主題

* [Direct3D 資源載入範例]( http://go.microsoft.com/fwlink/p/?LinkID=265132)
* [BasicLoader 的完整程式碼](complete-code-for-basicloader.md)
* [BasicReaderWriter 的完整程式碼](complete-code-for-basicreaderwriter.md)
* [DDSTextureLoader 的完整程式碼](complete-code-for-ddstextureloader.md)

 

 




