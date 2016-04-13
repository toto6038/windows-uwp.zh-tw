---
title: 逐步解說 -- 將簡單的 Direct3D 9 app 移植到 DirectX 11 和通用 Windows 平台 (UWP)
description: 這個移植練習示範如何將簡單的轉譯架構從 Direct3D 9 移到 Direct3D 11 和通用 Windows 平台 (UWP)。
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
---

# 逐步解說：將簡單的 Direct3D 9 app 移植到 DirectX 11 和通用 Windows 平台 (UWP)


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

這個移植練習示範如何將簡單的轉譯架構從 Direct3D 9 移到 Direct3D 11 和通用 Windows 平台 (UWP)。
## 
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
<td align="left"><p>[Initialize Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)</p></td>
<td align="left"><p>示範如何將 Direct3D 9 初始化程式碼轉換成 Direct3D 11，包含如何取得 Direct3D 裝置的控制代碼與裝置內容，以及如何使用 DXGI 來設定交換鏈結。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Convert the rendering framework](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)</p></td>
<td align="left"><p>示範如何將簡單的轉譯架構從 Direct3D 9 轉換到 Direct3D 11，包含如何移植幾何緩衝區、如何編譯和載入 HLSL 著色器程式，以及如何在 Direct3D 11 中實作轉譯鏈結。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Port the game loop](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)</p></td>
<td align="left"><p>示範如何為 UWP 遊戲實作視窗，以及如何帶入遊戲迴圈，其中包含如何建置 [<strong>IFrameworkView</strong>](https://msdn.microsoft.com/library/windows/apps/hh700478) 以控制全螢幕的 [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/apps/br208225)。</p></td>
</tr>
</tbody>
</table>

 

本主題將逐步解說兩個執行相同基本圖形工作的程式碼路徑：顯示旋轉的頂點著色立方體。 在這兩個案例中，程式碼會涵蓋下列程序：

1.  建立 Direct3D 裝置和交換鏈結。
2.  建立頂點緩衝區與索引緩衝區，表示色彩豐富的立方體網格。
3.  建立將頂點轉換成螢幕空間的頂點著色器、混合色彩值的像素著色器、編譯著色器，以及將著色器載入為 Direct3D 資源。
4.  實作轉譯鏈結以及在螢幕上呈現繪製的立方體。
5.  建立視窗、開始主要迴圈與負責視窗訊息處理。

完成這個逐步解說之後，您應該就會熟悉以下的 Direct3D 9 和 Direct3D 11 基本差異：

-   裝置、裝置內容和圖形基礎結構的區隔。
-   編譯著色器的程序以及在執行階段載入著色器位元組程式碼。
-   如何為輸入組合語言 (IA) 階段設定每個頂點資料。
-   如何使用 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 建立 CoreWindow 檢視。

請注意，為了簡單起見，本逐步解說使用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)，而且不涵蓋 XAML 互通性。

## 先決條件


您應[為 UWP DirectX 遊戲開發準備開發環境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)。 您還不需要用到範本，但是需要 Microsoft Visual Studio 2015 載入這個逐步解說的程式碼範例。

如需進一步了解這個逐步解說中說明的 DirectX 11 和 UWP 程式設計概念，請瀏覽[移植概念和考量](porting-considerations.md)。

## 相關主題


**Direct3D**
[在 Direct3D 9 撰寫 HLSL 著色器](https://msdn.microsoft.com/library/windows/desktop/bb944006)

[建立適用於 UWP 的新 DirectX 11 專案](user-interface.md)

**Windows 市集**
[
            **Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)

[**物件控制代碼運算子 (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx

 

 






<!--HONumber=Mar16_HO1-->


