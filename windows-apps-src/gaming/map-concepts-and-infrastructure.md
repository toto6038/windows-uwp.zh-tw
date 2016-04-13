---
將 OpenGL ES 2.0 對應到 Direct3D 11
當您第一次開始進行將圖形架構從 OpenGL ES 2.0 移植到 Direct3D 的程序時，請讓您自己熟悉這些 API 間的重要差異。
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
---

# 將 OpenGL ES 2.0 對應到 Direct3D 11


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

當您第一次開始進行將圖形架構從 OpenGL ES 2.0 移植到 Direct3D 的程序時，請讓您自己熟悉這些 API 間的重要差異。 本節中的主題可以協助您計劃移植策略，以及當您將圖形處理移至 Direct3D 時必須進行的 API 變更。
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Plan your port from OpenGL ES 2.0 to Direct3D](compare-opengl-es-2-0-api-design-to-directx.md)</p></td>
<td align="left"><p>如果您是從 iOS 或 Android 平台移植遊戲，可能已經在 OpenGL ES 2.0 投入了大量的心力。 在準備將圖形管線程式碼基底移到 Direct3D 11 與 Windows 執行階段時，有幾件事在開始之前必須先行考量。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Compare EGL code to DXGI and Direct3D](moving-from-egl-to-dxgi.md)</p></td>
<td align="left"><p>DirectX 圖形介面 (DXGI) 和數個 Direct3D API 都可提供與 EGL 相同的角色。 本主題可以協助您從 EGL 的觀點來了解 DXGI 和 Direct3D 11。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Compare OpenGL ES 2.0 buffers, uniforms, and vertex attributes to Direct3D](porting-uniforms-and-attributes.md)</p></td>
<td align="left"><p>在從 OpenGL ES 2.0 移植到 Direct3D 11 的程序期間，您必須變更用來在 app 與著色器程式之間傳送資料的語法與 API 行為。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Compare the OpenGL ES 2.0 shader pipeline to Direct3D](change-your-shader-loading-code.md)</p></td>
<td align="left"><p>在概念上來說，Direct3D 11 著色器管線與 OpenGL ES 2.0 中的著色器管線非常相似。 不過在 API 設計方面，建立與管理著色器階段的主要元件則分為兩個主要的介面，[<strong>ID3D11Device1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404575) 和 [<strong>ID3D11DeviceContext1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404598)。 本主題嘗試將常見的 OpenGL ES 2.0 著色器管線 API 模式對應到這些介面中的 Direct3D 11 對應項。</p></td>
</tr>
</tbody>
</table>

 

## 關於特定 OpenGL ES 2.0 提供者的注意事項


這些主題使用 Khronos OpenGL ES 2.0 規格與不限平台的 C。iOS 和 Android 也都是使用相同規格，儘管針對這些平台開發的 OpenGL ES 2.0 程式碼通常是當作物件導向 API 來公開，但是它們與我們將進行逐步解說的程式碼片段非常類似。 此外，由於每種平台的複雜性與語言差異，也會有些微不同，特別是在方法參數類型或一般語言語法方面。 例如，iOS 會使用 Objective-C。 Android 能夠使用 C++；但是，有一些開發人員可能依賴純 Java 實作。 請記住，這些主題仍然適合用來做為整體概念，OpenGL ES API 的結構與用法並無不同。

 

 






<!--HONumber=Mar16_HO1-->


