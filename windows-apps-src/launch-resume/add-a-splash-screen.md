---
title: 新增啟動顯示畫面
description: 使用 Microsoft Visual Studio 設定應用程式的啟動顯示畫面影像與背景色彩。
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 956e4050e3077ac827cf8107470698b42878a5e1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370868"
---
# <a name="add-a-splash-screen"></a>新增啟動顯示畫面

使用 Microsoft Visual Studio 設定應用程式的啟動顯示畫面影像與背景色彩。

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>在 Visual Studio 中設定啟動顯示畫面影像與背景色彩

當您使用 Visual Studio 範本建立應用程式時，會將預設影像新增到您的專案，並設定為啟動顯示畫面影像。 啟動顯示畫面的背景色彩預設為淺灰。 如果您想變更 app 啟動顯示畫面的預設影像或色彩，請遵循以下步驟：

1. 開啟 Visual Studio 中的現有通用 Windows 平台 (UWP) 應用程式專案。
2. 從 [方案總管]  中，開啟 "Package.appxmanifest" 檔案。 您也可以開啟這個檔案從功能表列選擇**專案** &gt; **存放區** &gt; **編輯應用程式資訊清單**。
3. 開啟 **\[視覺資產\]** 索引標籤，然後從「Package.appxmanifest」視窗左側的 **\[所有視覺資產\]** 窗格中選取 **\[啟動顯示畫面\]** 。 如果您要變更您的啟動顯示畫面，第一次，您會看到 「 資產\\SplashScreen.png 」 中的路徑**啟動顯示畫面**欄位。

    以下螢幕擷取畫面顯示 Visual Studio 中的「Package.appxmanifest」視窗。 根據專案類型，您會看到一組稍有不同的視覺資產。

    ![顯示 visual studio 2017 中 [package.appxmanifest] 視窗的螢幕擷取畫面](images/appmanifest.png)

    如果您在文字編輯器中開啟 Package.appxmanifest，[**SplashScreen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) 元素即會顯示為 [**VisualElements**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) 元素的子項。 在文字編輯器中，資訊清單檔中的預設啟動顯示畫面標記看起來像這樣：

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. 若要為 UWP app 選取新的啟動顯示畫面影像，請按顯示在 [縮放的資產]  下方 [1240 x 600 px]  標籤旁含有省略符號的按鈕。 選擇要當作啟動顯示畫面影像的 1240 x 600 像素影像 (.png、.jpg 或 .jpeg)。

    **重要**  您選擇啟動顯示畫面影像必須是使用 1x 縮放比例的 620 x 300 像素。 此外，設計您的啟動顯示畫面時，請注意它將小於螢幕，並且置中對齊。 它並不像 Windows Phone 市集應用程式的啟動顯示畫面會填滿螢幕。

5. 若要為 Windows Phone 市集應用程式選取新的啟動顯示畫面影像，請按 [縮放的資產]  下方顯示在 [1152 x 1920 px]  標籤旁具有省略符號的按鈕。 選擇要當作啟動顯示畫面影像的 1152 x 1920 像素影像 (.png、.jpg 或 .jpeg)。

    **重要**  您選擇啟動顯示畫面影像必須是 1152 x 1920 像素即 2.4 倍的縮放比例的正確大小。 如果這是您所提供的唯一資產，它將會針對 1.4x 和 1x 的縮放比例縮小。

6. 在 [啟動顯示畫面]  區段的 [背景色彩]  欄位中，設定與您的啟動顯示畫面影像一起顯示的背景色彩。 您可以輸入色彩的名稱或 '\#' 和色彩的十六進位值。 如需可用色彩的名稱清單，請參閱 [**SplashScreen element**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)。 您不一定要設定啟動顯示畫面的背景色彩。 如果您未指定的 UWP 應用程式的色彩，啟動顯示畫面背景色彩會預設為淺灰色 (十六進位值\#464646)。 這個色彩與預設的 [磚]  背景色彩相同 (請參閱 [視覺資產]  索引標籤中 [磚影像和標誌]  區段的 [背景色彩]  欄位)。 如果您沒有為 Windows Phone 指定色彩，或是將它設定成「透明」，啟動顯示畫面背景色彩就會是透明的。

## <a name="summary-and-next-steps"></a>摘要與後續步驟

如果您的 app 需要一些時間載入，可考慮加入延長式啟動顯示畫面。 如需逐步指引，請參閱[建立自訂的啟動顯示畫面](create-a-customized-splash-screen.md)。

## <a name="related-topics"></a>相關主題

* [建立自訂的啟動顯示畫面](create-a-customized-splash-screen.md)
* [封裝資訊清單的結構描述參考：啟動顯示畫面項目](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [Windows.ApplicationModel.Activation.SplashScreen 類別](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)