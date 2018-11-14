---
author: andrewleader
Description: The following article describes all of the properties and elements within toast content.
title: 快顯通知內容結構描述
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 121302b93d5f786c332831bcf963c5e161e30f86
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6661696"
---
# <a name="toast-content-schema"></a>快顯通知內容結構描述

 

以下說明快顯通知內容中的所有屬性和元素。

如果您想要使用原始 XML，而不使用 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，請參閱 [XML 結構描述](toast-xml-schema.md)。

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent 是描述通知內容 (包括視覺效果、動作和音效) 的最上層物件。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Launch**| 字串 | false | 快顯通知啟動應用程式時，傳遞給應用程式的字串。 此字串的格式和內容是由應用程式定義以供應用程式自己使用。 當使用者點選或按一下快顯通知來啟動其相關聯應用程式時，啟動字串會提供相關內容給應用程式，以允許應用程式向使用者顯示與快顯通知內容有關的檢視，而非以其預設方式啟動。 |
| **Visual** | [ToastVisual](#toastvisual) | true | 描述快顯通知的視覺效果部分。 |
| **Actions** | [IToastActions](#itoastactions) | false | 選擇性使用按鈕和輸入建立自訂動作。 |
| **Audio** | [ToastAudio](#toastaudio) | false | 描述快顯通知的音效部分。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 指定使用者此快顯通知內文時，將會使用什麼啟用類型。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Creators Update 的新功能：與快顯通知啟用相關的其他選項。 |
| **Scenario** | [ToastScenario](#toastscenario) | false | 宣告使用快顯通知的案例，例如鬧鐘或提醒。 |
| **DisplayTimestamp** | DateTimeOffset? | false | Creators Update 的新功能：以自訂時間戳記覆寫預設時間戳記，表示實際傳遞通知內容的時間，而不是 Windows 平台收到通知的時間。 |
| **Header** | [ToastHeader](#toastheader) | false | Creators Update 的新功能：新增自訂標頭以在控制中心中將多個通知群組在一起。 |


### <a name="toastscenario"></a>ToastScenario
指定快顯通知表示哪些案例。

| 值 | 意義 |
|---|---|
| **Default** | 一般快顯通知行為。 |
| **Reminder** | 提醒通知。 這會以預先展開的方式顯示，並停留在使用者的螢幕上，直到關閉為止。 |
| **Alarm** | 鬧鐘通知。 這會以預先展開的方式顯示，並停留在使用者的螢幕上，直到關閉為止。 音效預設會重複播放，並使用鬧鐘音效。 |
| **IncomingCall** | 來電通知。 這會使用特殊通話格式以預先展開的方式顯示，並停留在使用者的螢幕上，直到關閉為止。 音效預設會重複播放，並使用鈴聲音效。 |


## <a name="toastvisual"></a>ToastVisual
快顯通知的視覺效果部分含有繫結，其中包含文字、影像、調適性內容等。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | 一般快顯通知繫結，可呈現在所有裝置上。 此繫結為必要項，但不可為 null。 |
| **BaseUri** | Uri | false | 與影像來源屬性中的相對 URL 結合的預設基底 URL。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| 字串 | false | 使用當地語系化資源時的視覺效果承載目標地區設定，指定為 BCP-47 語言標記，例如 "en-US" 或 "fr-FR"。 繫結或文字中指定的任何地區設定都會覆寫此地區設定。 如果未提供，則改用系統地區設定。 |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
泛型繫結是快顯通知的預設繫結，並且是您指定文字、影像、調適性內容等項目的位置。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | 快顯通知的內文內容，其中可能包含文字、影像和群組 (年度更新版新增功能)。 文字元素必須放在任何其他元素前面，但僅支援 3 個文字元素。 如果將文字元素放在任何其他元素後面，不是將其提拉到頂端，就是捨棄。 總之，根子系文字元素不支援某些像是 HintStyle 的文字屬性，只能在 AdaptiveSubgroup 中使用。 如果您在沒有年度更新版的裝置上使用 AdaptiveGroup，就會直接捨棄群組內容。 |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | 要覆寫 App 標誌的選用標誌。 |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | 選擇性精選「主角」影像，這會顯示在快顯通知和控制中心。 |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | 選擇性屬性文字，這會顯示在快顯通知的底部。 |
| **BaseUri** | Uri | false | 與影像來源屬性中的相對 URL 結合的預設基底 URL。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| 字串 | false | 使用當地語系化資源時的視覺效果承載目標地區設定，指定為 BCP-47 語言標記，例如 "en-US" 或 "fr-FR"。 繫結或文字中指定的任何地區設定都會覆寫此地區設定。 如果未提供，則改用系統地區設定。 |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
快顯通知子元素的標記介面，包含文字、影像、群組等。

| 實作 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
調適性文字元素。 如果放置在最上層 ToastBindingGeneric.Children，則只會套用 HintMaxLines。 但要是放置做為群組/子群組的子系，則支援全文檢索樣式。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Text** | string 或 [BindableString](#bindablestring) | false | 要顯示的文字。 Creators Update 新增的資料繫結支援，但僅適用於最上層文字元素。 |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 此樣式會控制文字的字型大小、粗細和不透明度。 僅適用於群組/子群組內的文字元素。 |
| **HintWrap** | bool? | false | 將此設定為 true 可啟用文字換行。 最上層文字元素會忽略此屬性，並且一律會換行 (您可以使用 HintMaxLines = 1 來停用最上層文字元素換行)。 群組/子群組內的文字元素將換行預設為 false。 |
| **HintMaxLines** | int? | false | 允許文字元素顯示的最大行數。 |
| **HintMinLines** | int? | false | 文字元素必須顯示的最小行數。 僅適用於群組/子群組內的文字元素。 |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 文字的水平對齊。 僅適用於群組/子群組內的文字元素。 |
| **Language** | 字串 | false | 指定為 BCP-47 語言標記的 XML 承載目標地區設定，例如 "en-US" or "fr-FR"。 此處指定的地區設定會覆寫任何其他位置 (例如繫結或視覺效果) 指定的地區設定。 如果此值為常值字串，則此屬性預設為使用者的 UI 語言。 如果此值為字串參考，則此屬性預設為 Windows 執行階段在解析字串時所選擇的地區設定。 |


### <a name="bindablestring"></a>BindableString
字串的繫結值。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **BindingName** | string | true | 取得或設定對應至您資料繫結值的名稱。 |


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
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 年度更新版的新功能：控制影像所需的裁剪。 |
| **HintRemoveMargin** | bool? | false | 群組/子群組內的影像周圍預設會有 8px 邊界。 您可將此屬性設定為 true 以移除此邊界。 |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 影像的水平對齊。 僅適用於群組/子群組內的影像。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


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
年度更新版的新功能：群組會在語意上識別，群組中的內容必須完整顯示，如果無法容納其中，則不顯示。 群組也允許建立多欄。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 子群組會顯示為垂直欄。 要在 AdaptiveGroup 中提供任何內容，您必須使用子群組。 |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
年度更新版的新功能：子群組是可容納文字和影像的垂直欄。

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


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Creators Update 中的新功能：進度列。 僅支援桌上型電腦組建 15063 或更新版本的快顯通知。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Title** | string 或 [BindableString](#bindablestring) | false | 取得或設定選用標題字串。 支援資料繫結。 |
| **Value** | double 或 [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) 或 [BindableProgressBarValue](#bindableprogressbarvalue) | false | 取得或設定進度列的值。 支援資料繫結。 預設為 0。 |
| **ValueStringOverride** | string 或 [BindableString](#bindablestring) | false | 取得或設定要顯示的選用字串，用於取代預設百分比字串。 如果未提供此項，將會顯示「70%」之類的內容。 |
| **Status** | string 或 [BindableString](#bindablestring) | true | 取得或設定狀態字串 (必要)，它會顯示在左側進度列的下方。 這個字串應該反映作業的狀態，例如「正在下載...」或「正在安裝...」 |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
類別，表示進度列的值。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Value** | double | false | 取得或設定值 (0.0 - 1.0)，代表完成百分比。 |
| **IsIndeterminate** | bool | false | 取得或設定值，指出進度列不確定。 如果這為 true，會略過 **Value**。 |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
可繫結的進度列值。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **BindingName** | string | true | 取得或設定對應至您資料繫結值的名稱。 |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
要替代 App 標誌顯示的標誌。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Source** | 字串 | true | 影像的 URL。 支援 ms-appx、ms-appdata 和 http。 Http 影像的大小必須是 200 KB 或更少。 |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | 指定想要裁剪影像的方式。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
控制 App 標誌影像的裁剪。

| 值 | 意義 |
|---|---|
| **Default** | 裁剪時會使用轉譯器的預設行為。 |
| **None** | 不裁剪影像，顯示為正方形。 |
| **Circle** | 將影像裁剪成圓形。 |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
精選「主角」影像，這會顯示在快顯通知和控制中心。

| 屬性 | 類型 | 必要 |描述 |
|---|---|---|---|
| **Source** | 字串 | true | 影像的 URL。 支援 ms-appx、ms-appdata 和 http。 Http 影像的大小必須是 200 KB 或更少。 |
| **AlternateText** | 字串 | false | 描述影像的替代文字，用於協助工具用途。 |
| **AddImageQuery** | bool? | false | 設定為 "true" 可讓 Windows 將查詢字串附加至快顯通知中提供的影像 URL。 如果您的伺服器裝載影像，並且可以處理查詢字串 (方式為根據查詢字串擷取影像變體，或忽略查詢字串並傳回未使用查詢字串所指定的影像)，請使用此屬性。 此查詢字串指定比例、對比設定和語言。例如，通知中指定的 "www.website.com/images/hello.png" 值會變成 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
顯示在快顯通知底部的屬性文字。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Text** | 字串 | true | 要顯示的文字。 |
| **Language** | 字串 | false | 使用當地語系化資源時的視覺效果承載目標地區設定，指定為 BCP-47 語言標記，例如 "en-US" 或 "fr-FR"。 如果未提供，則改用系統地區設定。 |


## <a name="itoastactions"></a>IToastActions
快顯通知動作/輸入的標記介面。

| 實作 |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*實作 [IToastActions](#itoastactions)*

使用按鈕、文字方塊和選擇輸入等控制項，自行建立自訂動作和輸入。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Inputs** | IList<[IToastInput](#itoastinput)> | false | 像文字方塊和選擇輸入之類的輸入。 最多僅允許 5 項輸入。 |
| **Buttons** | IList<[IToastButton](#itoastbutton)> | false | 按鈕會顯示在所有輸入之後 (如果當做快速回覆按鈕，則與輸入相鄰)。 最多僅允許 5 個按鈕 (如果還有操作功能表項目則更少)。 |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | 年度更新版的新功能：自訂操作功能表項目，如果使用者以滑鼠右鍵按一下通知，則會提供其他動作。 最多只能有 5 個按鈕與操作功能表項目搭配*組合*。 |


## <a name="itoastinput"></a>IToastInput
快顯通知輸入的標記介面。

| 實作 |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*實作 [IToastInput](#itoastinput)*

使用者可以輸入文字的文字方塊控制項。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Id** | 字串 | true | Id 是必要項，用來將使用者輸入的文字對應到 App 稍後取用之識別碼/值的索引鍵/值組。 |
| **Title** | 字串 | false | 要顯示在文字方塊上方的標題文字。 |
| **PlaceholderContent** | 字串 | false | 使用者尚未輸入任何文字時，要顯示在文字方塊中的預留位置文字。 |
| **DefaultInput** | 字串 | false | 要放在文字方塊中的初始文字。 如需空白文字方塊，請保持為 null。 |


## <a name="toastselectionbox"></a>ToastSelectionBox
*實作 [IToastInput](#itoastinput)*

可讓使用者從選項下拉式清單挑選的選擇方塊控制項。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Id** | 字串 | true | Id 為必要項。 如果使用者選取此項目，這個 Id 將會傳回到 App 的程式碼，表示他們選擇了哪些選取項目。 |
| **Content** | 字串 | true | Content 為必要項，並且是顯示在選取項目上的字串。 |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
選擇方塊項目 (使用者可從下拉式清單選取的項目)。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Id** | 字串 | true | Id 是必要項，用來將使用者輸入的文字對應到 App 稍後取用之識別碼/值的索引鍵/值組。 |
| **Title** | 字串 | false | 要顯示在選擇方塊上方的標題文字。 |
| **DefaultSelectionBoxItemId** | 字串 | false | 這會控制預設選取哪些項目，並且參考 [ToastSelectionBoxItem](#toastselectionboxitem) 的的 Id 屬性。 如果沒有提供這個屬性，預設選取項目會是空白 (使用者看不到任何項目)。 |
| **Items** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | 使用者可以從這個 SelectionBox 中挑選的選取項目。 只能新增 5 個項目。 |


## <a name="itoastbutton"></a>IToastButton
快顯通知按鈕的標記介面。

| 實作 |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*實作 [IToastButton](#itoastbutton)*

使用者可以按一下的按鈕。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **內容** | 字串 | true | 必要。 要在按鈕上顯示的文字。 |
| **Arguments** | 字串 | true | 必要。 App 定義的引數字串，如果使用者按一下此按鈕，App 稍後會收到該字串。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 控制此按鈕在使用者按一下時使用什麼類型的啟用。 預設為 Foreground。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Creators Update 的新功能：取得或設定與快顯通知按鈕啟用相關的其他選項。 |


### <a name="toastactivationtype"></a>ToastActivationType
決定使用者以特定動作進行互動時使用的啟用類型。

| 值 | 意義 |
|---|---|
| **Foreground** | 預設值。 啟動您的前景 App。 |
| **Background** | 觸發對應的背景工作 (假設您已設定所有項目)，而且您可以在不干擾使用者的情況下，於背景執行程式碼 (例如傳送使用者的快速回覆訊息)。 |
| **Protocol** | 使用通訊協定啟用來啟動不同的 App。 |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Creators Update 的新功能：與啟用相關的其他選項。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Fall Creators Update 的新功能：取得或設定當使用者叫用此動作時，快顯通知應該使用的行為。 這只適用於桌上型電腦、[ToastButton](#toastbutton) 和 [ToastContextMenuItem](#toastcontextmenuitem)。 |
| **ProtocolActivationTargetApplicationPfn** | 字串 | false | 如果您要使用 *ToastActivationType.Protocol*，則可以選擇性指定目標 PFN，因此無論是否已註冊多個 App 來處理相同的通訊協定 URI，都一律會啟動您想要的 App。 |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
指定當使用者對快顯通知採取動作時，快顯通知應該使用的行為。

| 值 | 意義 |
|---|---|
| **Default** | 預設行為。 當使用者對快顯通知採取動作時，將會關閉快顯通知。 |
| **PendingUpdate** | 在使用者按一下快顯通知的按鈕後，通知仍保持存在於「擱置中的更新」可見狀態。 您應該立即更新背景工作的快顯通知，讓使用者不會看到太久的這個「擱置中的更新」可見狀態。 |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*實作 [IToastButton](#itoastbutton)*

系統處理的延遲按鈕，此按鈕會自動處理通知延遲。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **CustomContent** | 字串 | false | 顯示在按鈕上的選擇性自訂文字，這會覆寫預設當地語系化的 "Snooze" 文字 (延遲)。 |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*實作 [IToastButton](#itoastbutton)*

系統處理的關閉按鈕，按一下時會關閉通知。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **CustomContent** | 字串 | false | 顯示在按鈕上的選擇性自訂文字，這會覆寫預設當地語系化的 "Dismiss" 文字 (關閉)。 |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*實作 [IToastActions](#itoastactions)

自動建構延遲間隔選取方塊並將按鈕延遲/關閉，全部自動進行當地語系化，並自動由系統處理延遲邏輯。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | 年度更新版的新功能：自訂操作功能表項目，如果使用者以滑鼠右鍵按一下通知，則會提供其他動作。 您最多只能有 5 個項目。 |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
操作功能表項目輸入。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **內容** | 字串 | true | 必要。 要顯示的文字。 |
| **Arguments** | 字串 | true | 必要。 this button.
App 定義的引數字串，當使用者按一下功能表項目時，只要 App 已啟用，稍後即可收到該字串。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 控制此功能表項目在使用者按一下時使用什麼類型的啟用。 預設為 Foreground。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Creators Update 的新功能：與快顯通知操作功能表項目啟用作業相關的其他選項。 |


## <a name="toastaudio"></a>ToastAudio
指定收到快顯通知時要播放的音效。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Src** | uri | false | 要取代預設音效播放的媒體檔案。 僅支援 ms-appx 和 ms-appdata。 |
| **Loop** | boolean | false | 如果快顯通知顯示多久就要重複音效多久，則設定為 true。false 表示只播放一次 (預設)。 |
| **Silent** | boolean | false | true 表示將音效設定為靜音。false 表示允許播放快顯通知音訊 (預設)。 |


## <a name="toastheader"></a>ToastHeader
Creators Update 的新功能：將控制中心內多個通知群組一起的自訂標頭。

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Id** | 字串 | true | 開發人員所建立唯一辨識此標頭的識別碼。 如果兩個通知標頭識別碼相同，這些通知將會在控制中心顯示於相同標頭下方。 |
| **Title** | 字串 | true | 標頭的標題。 |
| **Arguments**| 字串 | true | 取得或設定開發人員定義的引數字串，當使用者按下此標頭時會傳回至應用程式。 不可為 null。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 取得或設定此標頭在使用者按一下時要使用的啟用類型。 預設為 Foreground。 請注意，只支援 Foreground 和 Protocol。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 取得或設定與快顯通知標頭啟用相關的其他選項。 |


## <a name="related-topics"></a>相關主題

* [快速入門：傳送本機快顯通知及處理啟用](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [GitHub 上的 Notifications 程式庫](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)