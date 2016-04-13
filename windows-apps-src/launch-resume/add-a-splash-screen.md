---
title: 新增啟動顯示畫面
description: 使用 Microsoft Visual Studio 2015 設定應用程式的啟動顯示畫面影像與背景色彩。
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
---

# 新增啟動顯示畫面


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


使用 Microsoft Visual Studio 2015 設定 app 的啟動顯示畫面影像與背景色彩。

## 在 Visual Studio 2015 中設定啟動顯示畫面影像與背景色彩


當您使用 Visual Studio 2015 範本建立 app 時，會將預設影像新增到您的專案，並設定為啟動顯示畫面影像。 啟動顯示畫面的背景色彩預設為淺灰。 如果您想變更 app 啟動顯示畫面的預設影像或色彩，請遵循以下步驟：

1.  開啟 Visual Studio 2015 中的現有通用 Windows 平台 (UWP) app 專案。
2.  在 [方案總管]**** 中，開啟 Package.appxmanifest 檔案。 您也可以依序選擇 [專案]****、[市集]**** 和 [編輯應用程式資訊清單]****，來從功能表列開啟這個檔案。
3.  開啟 [視覺資產]**** 索引標籤，然後從 Package.appxmanifest 視窗左側的 [所有影像資產]**** 窗格選取 [啟動顯示畫面]****。 如果您是第一次變更啟動顯示畫面，會在 [啟動顯示畫面]**** 欄位看到 "Assets\\SplashScreen.png" 路徑。

    以下螢幕擷取畫面顯示 Visual Studio 2015 中的 [Package.appxmanifest] 視窗。 根據專案類型，您會看到一組稍有不同的視覺資產。

    ![顯示 visual studio 2013 中 [package.appxmanifest] 視窗的螢幕擷取畫面](images/appmanifest.png)

    如果您在文字編輯器中開啟 "Package.appxmanifest"，[**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467) 元素即會顯示為 [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471) 元素的子項。 在文字編輯器中，資訊清單檔中的預設啟動顯示畫面標記看起來像這樣：

    ```xaml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4.  若要為 UWP app 選取新的啟動顯示畫面影像，請按顯示在 [縮放的資產]**** 下方 [1240 x 600 px]**** 標籤旁含有省略符號的按鈕。 選擇要當作啟動顯示畫面影像的 1240 x 600 像素影像 (.png、.jpg 或 .jpeg)。

    **重要** 您選擇的啟動顯示畫面影像必須使用 1x 縮放比例的 620 x 300 像素。 此外，設計您的啟動顯示畫面時，請注意它將小於螢幕，並且置中對齊。 它並不像 Windows Phone 市集應用程式的啟動顯示畫面會填滿螢幕。

     

5.  若要為 Windows Phone 市集應用程式選取新的啟動顯示畫面影像，請按 [縮放的資產]**** 下方顯示在 [1152 x 1920 px]**** 標籤旁具有省略符號的按鈕。 選擇要當作啟動顯示畫面影像的 1152 x 1920 像素影像 (.png、.jpg 或 .jpeg)。

    **重要** 您所選擇的啟動顯示畫面影像必須是 1152 x 1920 像素，這是 2.4x 縮放比例的正確大小。 如果這是您所提供的唯一資產，它將會針對 1.4x 和 1x 的縮放比例縮小。

     

6.  在 [啟動顯示畫面]**** 區段的 [背景色彩]**** 欄位中，設定與您的啟動顯示畫面影像一起顯示的背景色彩。 您可以輸入色彩名稱，或是輸入 '\#' 與色彩的十六進位值。 如需可用色彩的名稱清單，請參閱 [**SplashScreen element**](https://msdn.microsoft.com/library/windows/apps/br211467)。

    您不一定要設定啟動顯示畫面的背景色彩。 如果您沒有為 UWP app 指定色彩，啟動顯示畫面背景色彩預設會是淺灰色 (十六進位值 \#464646)。 這個色彩與預設的 [磚]**** 背景色彩相同 (請參閱 [視覺資產]**** 索引標籤中 [磚影像和標誌]**** 區段的 [背景色彩]**** 欄位)。 如果您沒有為 Windows Phone 指定色彩，或是將它設定成「透明」，啟動顯示畫面背景色彩就會是透明的。

## 摘要與後續步驟


如果您的 app 需要一些時間載入，可考慮加入延長式啟動顯示畫面。 如需逐步指引，請參閱[建立自訂的啟動顯示畫面](create-a-customized-splash-screen.md)。

**注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相關主題

* [建立自訂的啟動顯示畫面](create-a-customized-splash-screen.md)

**參考資料**

* [**封裝資訊清單結構描述參考：SplashScreen 元素**](https://msdn.microsoft.com/library/windows/apps/br211467)
* [**Windows.ApplicationModel.Activation.SplashScreen 類別**](https://msdn.microsoft.com/library/windows/apps/br224763)

 

 





<!--HONumber=Mar16_HO1-->


