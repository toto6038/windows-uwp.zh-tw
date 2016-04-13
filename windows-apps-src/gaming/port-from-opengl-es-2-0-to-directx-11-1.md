---
從 OpenGL ES 2.0 移植到 Direct3D 11
包含適用於將 OpenGL ES 2.0 圖形管線移植到 Direct3D 11 與 Windows 執行階段的文章、概觀及逐步解說。
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
---

# 從 OpenGL ES 2.0 移植到 Direct3D 11


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

包含適用於將 OpenGL ES 2.0 圖形管線移植到 Direct3D 11 與 Windows 執行階段的文章、概觀及逐步解說。

> **注意**：移植 OpenGL ES 2.0 專案的中間步驟是使用適用於 Windows 市集的 ANGLE。 ANGLE 可讓您透過將 OpenGL ES API 呼叫轉譯為 DirectX 11 API 呼叫，在 Windows 上執行 OpenGL ES 內容。 如需關於 ANGLE 的詳細資訊，請移至[適用於 Windows 市集的 ANGLE Wiki](http://go.microsoft.com/fwlink/p/?linkid=618387)。

 

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
<td align="left"><p>[Map OpenGL ES 2.0 to Direct3D 11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>當您第一次開始進行將圖形架構從 OpenGL ES 2.0 移植到 Direct3D 的程序時，請讓您自己熟悉這些 API 間的重要差異。 本節中的主題可以協助您計劃移植策略，以及當您將圖形處理移至 Direct3D 時必須進行的 API 變更。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Walkthrough sample ports from OpenGL ES 2.0](walkthrough-sample-ports-from-opengl-es-2-0.md)</p></td>
<td align="left"><p>這系列主題提供一些不同複雜程度的 OpenGL ES 2.0 圖形管線移植案例。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[OpenGL ES 2.0 to Direct3D 11.1 reference](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>從 OpenGL ES 2.0 移植到 Direct3D 11 時，可以使用這些參考主題來查詢 API 對應和簡短的程式碼範例。</p></td>
</tr>
</tbody>
</table>

 

> **注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

 

 






<!--HONumber=Mar16_HO1-->


