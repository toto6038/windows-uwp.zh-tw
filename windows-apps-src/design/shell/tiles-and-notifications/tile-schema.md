---
author: andrewleader
Description: The following article describes all of the properties and elements within tile content.
title: 磚內容結構描述
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 07/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 磚, 磚通知, 磚內容, 結構描述, 磚裝載
ms.localizationpriority: medium
ms.openlocfilehash: d2baa2e2d7b8d68505159eb480ea3be78750f507
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "4960686"
---
# <a name="tile-content-schema"></a>磚內容結構描述

 

以下說明磚內容中的所有屬性和元素。

如果您想要使用原始 XML，而不使用 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，請參閱 [XML 結構描述](../tiles-and-notifications/adaptive-tiles-schema.md)。

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#TileBindingContentAdaptive)
    * [TileBindingContentIconic](#TileBindingContentIconic)
    * [TileBindingContentContact](#TileBindingContentContact)
    * [TileBindingContentPeople](#TileBindingContentPeople)
    * [TileBindingContentPhotos](#TileBindingContentPhotos)


## <a name="tilecontent"></a>TileContent
TileContent 是描述磚通知內容 (包括視覺效果) 的最上層物件。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Visual** | [ToastVisual](#tilevisual) | true | 描述磚通知的視覺效果部分。 |


## <a name="tilevisual"></a>TileVisual
磚的視覺效果部分包含所有磚大小的視覺規格，以及更多視覺相關屬性。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | 提供選擇性小型繫結，以指定小型磚大小的內容。 |
| **TileMedium** | [TileBinding](#tilebinding) | false | 提供選擇性中型繫結，以指定中型磚大小的內容。 |
| **TileWide** | [TileBinding](#tilebinding) | false | 提供選擇性寬型繫結，以指定寬型磚大小的內容。 |
| **TileLarge** | [TileBinding](#tilebinding) | false | 提供選擇性大型繫結，以指定大型磚大小的內容。 |
| **Branding** | [TileBranding](#tilebranding) | false | 磚應用來顯示應用程式品牌的表單。 根據預設，繼承自預設磚的品牌。 |
| **DisplayName** | 字串 | false | 選擇性字串，顯示此通知時會覆寫磚的顯示名稱。 |
| **Arguments** | 字串 | false | 年度更新版的新功能：應用程式定義的資料，當使用者透過動態磚啟動您的應用程式，會透過 LaunchActivatedEventArgs 上的 TileActivatedInfo 屬性傳回至您的應用程式。 這可讓您知道當使用者點選動態磚時，看到哪個磚通知。 在沒有年度更新版的裝置上，這會被忽略。 |
| **LockDetailedStatus1** | 字串 | false | 若您指定此項，也必須提供 TileWide 繫結。 如果使用者選取磚做為詳細狀態應用程式，這會是顯示在鎖定畫面上的第一行文字。 |
| **LockDetailedStatus2** | 字串 | false | 若您指定此項，也必須提供 TileWide 繫結。 如果使用者選取磚做為詳細狀態應用程式，這會是顯示在鎖定畫面上的第二行文字。 |
| **LockDetailedStatus3** | 字串 | false | 若您指定此項，也必須提供 TileWide 繫結。 如果使用者選取磚做為詳細狀態應用程式，這會是顯示在鎖定畫面上的第三行文字。 |
| **BaseUri** | Uri | false | 與影像來源屬性中的相對 URL 結合的預設基底 URL。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| 字串 | false | 使用當地語系化資源時的視覺效果承載目標地區設定，指定為 BCP-47 語言標記，例如 "en-US" 或 "fr-FR"。 繫結或文字中指定的任何地區設定都會覆寫此地區設定。 如果未提供，則改用系統地區設定。 |


## <a name="tilebinding"></a>TileBinding
繫結物件包含特定磚大小的視覺內容。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Content** | [ITileBindingContent](#itilebindingcontent) | false | 要在磚上顯示的視覺內容。 其中一個 [TileBindingContentAdaptive](#tilebindingcontentadaptive)，[TileBindingContentIconic](#TileBindingContentIconic)、[TileBindingContentContact](#TileBindingContentContact)、[TileBindingContentPeople](#TileBindingContentPeople) 或 [TileBindingContentPhotos](#TileBindingContentPhotos)。 |
| **Branding** | [TileBranding](#tilebranding) | false | 磚應用來顯示應用程式品牌的表單。 根據預設，繼承自預設磚的品牌。 |
| **DisplayName** | 字串 | false | 選擇性字串，覆寫此磚大小的磚顯示名稱。 |
| **Arguments** | 字串 | false | 年度更新版的新功能：應用程式定義的資料，當使用者透過動態磚啟動您的應用程式，會透過 LaunchActivatedEventArgs 上的 TileActivatedInfo 屬性傳回至您的應用程式。 這可讓您知道當使用者點選動態磚時，看到哪個磚通知。 在沒有年度更新版的裝置上，這會被忽略。 |
| **BaseUri** | Uri | false | 與影像來源屬性中的相對 URL 結合的預設基底 URL。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| 字串 | false | 使用當地語系化資源時的視覺效果承載目標地區設定，指定為 BCP-47 語言標記，例如 "en-US" 或 "fr-FR"。 繫結或文字中指定的任何地區設定都會覆寫此地區設定。 如果未提供，則改用系統地區設定。 |


## <a name="itilebindingcontent"></a>ITileBindingContent
磚繫結內容的標記介面。 這些可讓您選擇想以什麼指定您的磚視覺效果 - 彈性，或其中一個特殊範本。

| 實作 |
| --- |
| [TileBindingContentAdaptive](#TileBindingContentAdaptive) |
| [TileBindingContentIconic](#TileBindingContentIconic) |
| [TileBindingContentContact](#TileBindingContentContact) |
| [TileBindingContentPeople](#TileBindingContentPeople) |
| [TileBindingContentPhotos](#TileBindingContentPhotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
支援所有的大小。 這是指定磚內容的建議方式。 彈性磚範本是 Windows 10 的新功能，您可以透過彈性建立各種不同的自訂磚。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Children** | IList<[ITileBindingContentAdaptiveChild](#ITileBindingContentAdaptiveChild)> | false | 內嵌視覺元素。 可以新增 [AdaptiveText](#adaptivetext)、[AdaptiveImage](#adaptiveimage) 和 [AdaptiveGroup](#adaptivegroup) 物件。 這些子系會以垂直 StackPanel 方式顯示。 |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | 選用背景影像，會顯示在所有磚內容後面，跨頁顯示。 |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | 選用預覽影像，可從磚的上方動畫顯示進來。 |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | 控制整體子系內容的文字堆疊 (垂直對齊)。 |


## <a name="adaptivetext"></a>AdaptiveText
調適性文字元素。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Text** | 字串 | false | 要顯示的文字。 |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 此樣式會控制文字的字型大小、粗細和不透明度。 |
| **HintWrap** | bool? | false | 將此設定為 true 可啟用文字換行。 預設為 false。 |
| **HintMaxLines** | int? | false | 允許文字元素顯示的最大行數。 |
| **HintMinLines** | int? | false | 文字元素必須顯示的最小行數。 |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 文字的水平對齊。 |
| **Language** | 字串 | false | 指定為 BCP-47 語言標記的 XML 承載目標地區設定，例如 "en-US" or "fr-FR"。 此處指定的地區設定會覆寫任何其他位置 (例如繫結或視覺效果) 指定的地區設定。 如果此值為常值字串，則此屬性預設為使用者的 UI 語言。 如果此值為字串參考，則此屬性預設為 Windows 執行階段在解析字串時所選擇的地區設定。 |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
文字樣式會控制字型大小、粗細和不透明度。 輕微不透明度是 60% 不透明。

| 值 | 意義 |
|---|---|
| **Default** | 預設值。 樣式取決於轉譯器。 |
| **Caption** | 小於段落字型大小。 |
| **CaptionSubtle** | 和 Caption 一樣，只是有輕微不透明度。 |
| **Body** | 段落字型大小。 |
| **BodySubtle** | 和 Body 一樣，只是有輕微不透明度。 |
| **Base** | 段落字型大小、粗體粗細。 基本上是 Body 的粗體版本。 |
| **BaseSubtle** | 和 Base 一樣，只是有輕微不透明度。 |
| **Subtitle** | H4 字型大小。 |
| **SubtitleSubtle** | 和 Subtitle 一樣，只是有輕微不透明度。 |
| **Title** | H3 字型大小。 |
| **TitleSubtle** | 和 Title 一樣，只是有輕微不透明度。 |
| **TitleNumeral** | 和 Title 一樣，只是上/下邊框間距已移除。 |
| **Subheader** | H2 字型大小。 |
| **SubheaderSubtle** | 和 Subheader 一樣，只是有輕微不透明度。 |
| **SubheaderNumeral** | 和 Subheader 一樣，只是上/下邊框間距已移除。 |
| **Header** | H1 字型大小。 |
| **HeaderSubtle** | 和 Header 一樣，只是有輕微不透明度。 |
| **HeaderNumeral** | 和 Header 一樣，只是上/下邊框間距已移除。 |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
控制文字的水平對齊。

| 值 | 意義 |
|---|---|
| **Default** | 預設值。 對齊方式自動由轉譯器決定。 |
| **Auto** | 對齊方式取決於目前的語言及文化特性。 |
| **Left** | 將文字水平對齊左側。 |
| **Center** | 將文字水平對齊中央。 |
| **Right** | 將文字水平對齊右側。 |


## <a name="adaptiveimage"></a>AdaptiveImage
內嵌影像。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Source** | 字串 | true | 影像的 URL。 支援 ms-appx、ms-appdata 和 http。 從 Fall Creators Update 開始，一般連線的網頁影像可以高達 3 MB，而計量付費連線可以高達 1 MB。 在尚未執行 Fall Creators Update 的裝置上，網頁影像不得超過 200 KB。 |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 控制影像所需的裁剪。 |
| **HintRemoveMargin** | bool? | false | 群組/子群組內的影像周圍預設會有 8px 邊界。 您可將此屬性設定為 true 以移除此邊界。 |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 影像的水平對齊。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至磚通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
指定影像所需的裁剪。

| 值 | 意義 |
|---|---|
| **Default** | 預設值。 裁剪行為取決於轉譯器。 |
| **None** | 不裁剪影像。 |
| **Circle** | 將影像裁剪成圓形形狀。 |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
指定影像的水平對齊方式。

| 值 | 意義 |
|---|---|
| **Default** | 預設值。 對齊行為取決於轉譯器。 |
| **Stretch** | 影像自動縮放到填滿可用寬度 (和可能會有的可用高度，視放置影像的位置而定)。 |
| **Left** | 將影像對齊左側，並以其原生解析度來顯示影像。 |
| **Center** | 將影像對齊中央，並以其原生解析度來顯示影像。 |
| **Right** | 將影像對齊右側，並以其原生解析度來顯示影像。 |


## <a name="adaptivegroup"></a>AdaptiveGroup
群組會在語意上識別，群組中的內容必須完整顯示，如果無法容納其中，則不顯示。 群組也允許建立多欄。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 子群組會顯示為垂直欄。 要在 AdaptiveGroup 中提供任何內容，您必須使用子群組。 |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
子群組是可容納文字和影像的垂直欄。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) 和 [AdaptiveImage](#adaptiveimage) 是子群組的有效子系。 |
| **HintWeight** | int? | false | 藉由指定相對於其他子群組的粗細，控制這個子群組欄的寬度。 |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | 控制這個子群組內容的垂直對齊方式。 |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
子群組子系的標記介面。

| 實作 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking 指定內容的垂直對齊方式。

| 值 | 意義 |
|---|---|
| **Default** | 預設值。 轉譯器自動選取預設垂直對齊方式。 |
| **Top** | 垂直對齊最上方。 |
| **Center** | 垂直對齊中央。 |
| **Bottom** | 垂直對齊底部。 |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
背景影像會在磚上跨頁顯示。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Source** | 字串 | true | 影像的 URL。 支援 ms-appx、ms-appdata 和 http(s)。 Http 影像的大小必須是 200 KB 或更少。 |
| **HintOverlay** | int? | false | 背景影像上的全黑重疊。 此值控制全黑重疊的透明度，0 代表不重疊，100 代表全黑。 預設為 20。 |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | 1511 中的新功能：指定想要裁剪影像的方式。 在 1511 之前的版本中，這將會被忽略，背景影像不使用任何裁剪來顯示。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至磚通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
控制背景影像的裁剪。

| 值 | 意義 |
|---|---|
| **Default** | 裁剪時會使用轉譯器的預設行為。 |
| **None** | 不裁剪影像，顯示為正方形。 |
| **Circle** | 將影像裁剪成圓形。 |


## <a name="tilepeekimage"></a>TilePeekImage
預覽影像，可從磚的上方動畫顯示進來。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Source** | 字串 | true | 影像的 URL。 支援 ms-appx、ms-appdata 和 http(s)。 Http 影像的大小必須是 200 KB 或更少。 |
| **HintOverlay** | int? | false | 1511 中的新功能：預覽影像上的黑色重疊。 此值控制全黑重疊的透明度，0 代表不重疊，100 代表全黑。 預設為 20。 在舊版中，將會忽略此值，並以 0 重疊顯示預覽影像。 |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | 1511 中的新功能：指定想要裁剪影像的方式。 在 1511 之前的版本中，這將會被忽略，預覽影像不使用任何裁剪來顯示。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至磚通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
控制預覽影像的裁剪。

| 值 | 意義 |
|---|---|
| **Default** | 裁剪時會使用轉譯器的預設行為。 |
| **None** | 不裁剪影像，顯示為正方形。 |
| **Circle** | 將影像裁剪成圓形。 |


### <a name="tiletextstacking"></a>TileTextStacking
文字堆疊指定內容的垂直對齊方式。

| 值 | 意義 |
|---|---|
| **Default** | 預設值。 轉譯器自動選取預設垂直對齊方式。 |
| **Top** | 垂直對齊最上方。 |
| **Center** | 垂直對齊中央。 |
| **Bottom** | 垂直對齊底部。 |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
支援小型和中型。 啟用圖示磚範本，您可以真正的經典 Windows Phone 樣式，在磚的每個項目旁顯示圖示和徽章。 圖示旁邊的號碼透過不同徽章通知達成。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Icon** | [TileBasicImage](#tilebasicimage) | true | 至少支援桌面和行動裝置版、小型和中型磚，提供正方形的長寬比例影像、解析度 200x200、PNG 格式、具有透明度，只使用白色。 如需詳細資訊，請參閱：[特殊磚範本](../tiles-and-notifications/special-tile-templates-catalog.md)。 |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
僅限行動裝置版。 支援小型、中型和寬型。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Image** | [TileBasicImage](#tilebasicimage) | true | 要顯示的影像。 |
| **Text** | [TileBasicText](#tilebasictext) | false | 要顯示的文字行。 不會顯示在小型磚上。 |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
1511 中的新功能：支援中型、寬型以及大型 (桌面和行動裝置版)。 之前此功能僅限行動裝置版並僅限中型及寬型。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | 影像會以圓圈滾動。 |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
透過相片的投影片製作動畫。 支援所有的大小。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | 可提供高達 12 個影像 (行動裝置版只會顯示最多 9 個)，將用於投影片放映。 新增超過 12 個會擲回例外狀況。 |


### <a name="tilebasicimage"></a>TileBasicImage
用於各種特殊範本的影像。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Source** | 字串 | true | 影像的 URL。 支援 ms-appx、ms-appdata 和 http(s)。 Http 影像的大小必須是 200 KB 或更少。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至磚通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="tilebasictext"></a>TileBasicText
用於各種特殊範本的基本文字元素。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Text** | 字串 | false | 要顯示的文字。 |
| **Language** | 字串 | false | 指定為 BCP-47 語言標記的 XML 承載目標地區設定，例如 "en-US" or "fr-FR"。 此處指定的地區設定會覆寫任何其他位置 (例如繫結或視覺效果) 指定的地區設定。 如果此值為常值字串，則此屬性預設為使用者的 UI 語言。 如果此值為字串參考，則此屬性預設為 Windows 執行階段在解析字串時所選擇的地區設定。 |


## <a name="related-topics"></a>相關主題

* [快速入門︰傳送本機磚通知](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [GitHub 上的 Notifications 程式庫](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)