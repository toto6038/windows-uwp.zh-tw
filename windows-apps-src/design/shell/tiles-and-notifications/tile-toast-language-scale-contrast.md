---
author: stevewhims
Description: Your tiles and toasts can load strings and images tailored for display language, display scale factor, high contrast, and other runtime contexts.
title: 對語言、縮放比例及高對比的磚和快顯通知支援
template: detail.hbs
ms.author: stwhi
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 87aafe36d05298a8fa157426e39c530190f98908
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "4149615"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>對語言、縮放比例及高對比的磚和快顯通知支援

您的磚和快顯通知可以載入針對顯示語言、[顯示縮放比例](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)、高對比及其他執行階段內容量身訂做的文字與影像。 如需有關如何在資源檔案的名稱中使用限定詞的背景，請參閱[您的資源，針對語言、 縮放比例及其他限定詞量身打造](../../../app-resources/tailor-resources-lang-scale-contrast.md)和[應用程式圖示和標誌。](/windows/uwp/design/style/app-icons-and-logos)

如需有關將您的 App 當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../../globalizing/globalizing-portal.md)。

## <a name="refer-to-a-string-resource-from-a-template"></a>從範本參考字串資源

在您的磚或快顯通知範本中，可以使用後面加上簡單字串資源識別碼的 `ms-resource` URI (統一資源識別項) 配置來參考字串資源。 例如，如果您的 Resources.resx 檔案包含名稱為「Farewell」的資源項目，則您會有包含識別碼「Farewell」的字串資源。 如需字串資源識別項和資源檔案 (.resw) 的詳細資訊，請參閱[將 UI 及應用程式套件資訊清單中的字串當地語系化](../../../app-resources/localize-strings-ui-manifest.md)。

這是「Farewell」字串資源識別項參考使用 `ms-resource` 在範本內容[文字](/uwp/schemas/tiles/tilesschema/element-text?branch=live)本文中顯示的樣貌。

```xml
<text id="1">ms-resource:Farewell</text>
```

如果您省略 `ms-resource` URI 配置，則文字本文就只是字串常值，而*非*識別項參考。

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>從範本參考影像資源

在您的磚或快顯通知範本中，可以使用後面加上影像資源名稱的 `ms-appx` URI (統一資源識別項) 配置來參考影像資源。 這與您在 XAML 標記中參考影像資源的方式相同 (如需詳細資訊，請參閱[從 XAML 標記和程式碼參考影像或其他資產](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code))。

例如，您可能會像這樣來命名資料夾。

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

在此情況下，您有單一影像資源，其名稱 (與絕對路徑相同) 為 `/Assets/Images/welcome.png`。 以下說明如何在範本中使用該名稱。

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

注意這個範例中的 URI 配置 ("`ms-appx`) 如何在後面加上 "`://`"，再後接絕對路徑 (以 "`/`" 開頭的絕對路徑)。

## <a name="hosting-and-loading-images-in-the-cloud"></a>在雲端裝載和載入影像

`ms-resource` 和 `ms-appx` URI 配置會執行自動限定詞比對來尋找最適合目前內容的資源。 Web URI 配置 (例如，`http`、`https` 和 `ftp`) 不會執行任何這樣的自動比對。

反而會將描述所要求限定詞值的查詢字串附加至您的影像 URI。

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

然後在提供影像的應用程式服務中實作 HTTP 處理常式，在其中檢查和使用查詢字串來判斷要傳回哪一個影像。

您也必須在[磚](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)或[快顯通知](/uwp/schemas/tiles/toastschema/schema-root?branch=live) XML 承載中，將 [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) 屬性設定為 `true`。 **addImageQuery** 屬性會在磚和快顯通知結構描述的 `visual`、`binding` 及 `image` 元素中出現。 明確設定元素上的 **addImageQuery** 會覆寫上階設定的任何值。 例如，`image` 元素的 **addImageQuery** 值 `true` 會覆寫其上層 `binding` 元素的 **addImageQuery** 值 `false`。

以下是您可以使用的查詢字串。

| 限定詞 | 查詢字串 | 範例 |
| --------- | ------------ | ------- |
| Scale | ms-scale | ?ms-scale=400 |
| Language | ms-lang | ?ms-lang=en-US |
| Contrast | ms-contrast | ?ms-contrast=high |

如需所有可在查詢字串中使用之可能限定詞值的參考表，請參閱 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)。

## <a name="important-apis"></a>重要 API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>相關主題

* [回應式設計的螢幕大小與中斷點](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [針對語言、縮放比例及其他限定詞量身打造您的資源](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [磚和圖示資產的指導方針](app-assets.md)。
* [全球化和當地語系化](../../globalizing/globalizing-portal.md)
* [當地語系化您 UI 及應用程式封裝資訊清單中的字串](../../../app-resources/localize-strings-ui-manifest.md)
* [從 XAML 標記和程式碼參考影像或其他資產](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [磚結構描述](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [快顯通知結構描述](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
