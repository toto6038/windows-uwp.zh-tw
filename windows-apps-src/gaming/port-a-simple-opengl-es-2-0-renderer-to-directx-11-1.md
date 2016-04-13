---
title: 使用方法：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11
description: 針對首次移植練習，我們將從頭開始 -- 將適用於頂點已著色的旋轉立方體的簡單轉譯器從 OpenGL ES 2.0 帶入 Direct3D，如此讓它符合 Visual Studio 2015 的 DirectX 11 App (通用 Windows) 範本。
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
---

# 使用方法：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

針對首次移植練習，我們將從頭開始：將適用於頂點已著色的旋轉立方體的簡單轉譯器從 OpenGL ES 2.0 帶入 Direct3D，如此讓它符合 Visual Studio 2015 的 DirectX 11 App (通用 Windows) 範本。 當我們逐步解說這個移植程序時，您將了解下列內容：

-   如何將一組簡單的頂點緩衝區移植到 Direct3D 輸入緩衝區
-   如何將 Uniform 與屬性移植到常數緩衝區
-   如何設定 Direct3D 著色器物件
-   如何在 Direct3D 著色器開發中使用基本的 HLSL 語意
-   如何將非常簡單的 GLSL 移植到 HLSL

本主題一開始假設您已建立新的 DirectX 11 專案。 若要了解如何建立新的 DirectX 11 專案，請閱讀[建立適用於通用 Windows 平台 (UWP) 的新 DirectX 11 專案](user-interface.md)。

從這其中任一個連結建立的專案都會包含所有針對 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476345) 基礎結構所準備的程式碼，而您可以立即開始進行將轉譯器從 Open GL ES 2.0 移植到 Direct3D 11 的程序。

本主題將逐步解說兩個執行相同基本圖形工作的程式碼路徑：在視窗中顯示旋轉的頂點著色立方體。 在這兩個案例中，程式碼會涵蓋下列程序：

1.  從硬式編碼的資料建立立方體網格。 此網格會顯示為頂點清單，每個頂點都會處理一個位置、一個一般向量及一個色彩向量。 此網格會放入著色管線的頂點緩衝區以進行處理。
2.  建立著色器物件以處理立方體網格。 著色器有兩種：頂點著色器 (可處理要點陣化的頂點)，以及片段 (像素) 著色器 (可在點陣化之後將立方體的個別像素上色)。 這些像素會寫入轉譯器目標以供顯示。
3.  組成著色語言，可分別用來處理頂點和片段著色器中的頂點與像素。
4.  在畫面上顯示轉譯的立方體。

![簡單的 OpenGL 立方體](images/simple-opengl-cube.png)

完成這個逐步解說之後，您應該會熟悉在 Open GL ES 2.0 與 Direct3D 11 之間的下列基本差異：

-   頂點緩衝區與頂點資料的表示方式。
-   建立與設定著色器的程序。
-   著色語言，以及著色器物件的輸入與輸出。
-   螢幕繪製行為。

在這個逐步解說中，我們會參考一個簡單又一般的 OpenGL 轉譯器結構，其定義如下：

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

這個結構含有一個執行個體，並包含所有用來轉譯非常簡單且頂點已著色的網格所需的元件。

> **注意**：本主題中的任何 OpenGL ES 2.0 程式碼都是以 Khronos Group 所提供的 Windows API 實作為基礎，並使用 Windows C 程式設計語法。

 

## 您需要知道的事項


### 技術

-   [Microsoft Visual C++](http://msdn.microsoft.com/library/vstudio/60k1461a.aspx)
-   OpenGL ES 2.0

### 先決條件

-   選用。 檢閱[將 EGL 程式碼移植到 DXGI 和 Direct3D](moving-from-egl-to-dxgi.md)。 閱讀本主題以更加了解 DirectX 所提供的圖形介面。

## <span id="keylinks_steps_heading"> </span>步驟


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
<td align="left"><p>[Port the shader objects](port-the-shader-config.md)</p></td>
<td align="left"><p>從 OpenGL ES 2.0 移植簡單的轉譯器時，第一個步驟是在 Direct3D 11 中設定對等的頂點和片段著色器物件，以及確定主程式可以在著色器物件編譯完成之後與這些物件通訊。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Port the vertex buffers and data](port-the-vertex-buffers-and-data-config.md)</p></td>
<td align="left"><p>在這個步驟中，您將定義包含網格的頂點緩衝區，以及允許著色器以特定順序周遊頂點的索引緩衝區。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Port the GLSL](port-the-glsl.md)</p></td>
<td align="left"><p>一旦將建立和設定緩衝區與著色器物件的程式碼移過去之後，就可以將這些著色器內部的程式碼從 OpenGL ES 2.0 的 GL 著色器語言 (GLSL) 移植到 Direct3D 11 的高階著色器語言 (HLSL)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Draw to the screen](draw-to-the-screen.md)</p></td>
<td align="left"><p>最後，我們要將繪製旋轉立方體的程式碼移植到螢幕。</p></td>
</tr>
</tbody>
</table>

 

## <span id="additional_resources"> </span>其他資源


-   [為 UWP DirectX 遊戲開發準備開發環境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [建立適用於 UWP 的新 DirectX 11 專案](user-interface.md)
-   [將 OpenGL ES 2.0 概念與基礎結構對應到 Direct3D 11](map-concepts-and-infrastructure.md)

 

 






<!--HONumber=Mar16_HO1-->


