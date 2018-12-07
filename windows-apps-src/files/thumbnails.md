---
Description: How to use thumbnail images to help users preview files in UWP apps.
title: UWP app 中縮圖影像的指導方針
label: Thumbnail images
template: detail.hbs
ms.date: 01/08/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 92899333f0bc787fc6538e0bb22c148ac9f5c56f
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8785615"
---
# <a name="thumbnail-images"></a>縮圖影像

下列指導方針描述如何使用縮圖影像協助使用者預覽檔案，在您的 UWP app 中瀏覽時。 

> **重要 API**：[ThumbnailMode 列舉](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>我的 app 應該包含縮圖嗎？

如果您的 app 允許使用者瀏覽檔案，您可以顯示縮圖影像，協助使用者快速預覽這些檔案。 

使用縮圖的時機： 
- 顯示圖庫集合中多個項目的預覽 (像是檔案和資料夾)。 例如，影像中心應該使用縮圖提供每個圖片的小視圖給使用者，在使用者瀏覽其相片檔案時。

    ![影片陳列庫](images/thumbnail-gallery.png)

- 顯示清單中各別項目的預覽 (例如特定檔案)。 例如，使用者可能想要查看有關檔案的更多資訊，包括方便預覽的較大縮圖，再決定是否要開啟檔案。 

    ![影片預覽](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>可行與禁止事項
- 指定[縮圖模式](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView、VideosView、DocumentsView、MusicView、ListView 或 SingleItem)，當您擷取縮圖時。 這樣可確保縮圖影像最佳化，顯示使用者想要看見的檔案類型。 
    - 使用 SingleItem 模式擷取單一項目的縮圖，無論檔案類型為何。 其他縮圖模式適用於顯示多個檔案的預覽。 

- 顯示一般預留位置影像，取代縮圖載入時的縮圖。 使用預留位置幫助 app 變得回應更快，因為使用者可以在縮圖載入前與預覽互動。 

    預留位置影像應該是：
    * 要取代的項目類型所專屬。 例如，資料夾、圖片及影片應該都有自己專用的預留位置。 
    * 與要取代的縮圖影像的大小與外觀比例相同。 
    * 一直顯示，直到縮圖影像載入完成。 

- 使用包含文字標籤的預留位置影像來代表資料夾和檔案群組，以便與個別檔案加以區別。

- 如果您無法擷取縮圖，請顯示預留位置影像。 

- 提供文件和音樂檔案的預覽時，顯示其他檔案資訊。 然後使用者就可以找出縮圖影像可能還未提供的檔案重要資訊。 例如，若是音樂檔案，您可顯示演出者的姓名以及專輯封面的縮圖。 

- 不要顯示圖片及影片檔案的其他檔案資訊。 在大多數情況下，縮圖影像就已足夠，可讓使用者瀏覽圖片和影片。 

## <a name="additional-usage-guidelines"></a>其他用法指導方針
建議的[縮圖模式](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)與其功能：

<table>
<tr>
<th> 顯示其預覽</th>
<th> 縮圖模式 </th>
<th> 所擷取縮圖影像的功能 </th>
</tr>
<tr>
<td> 圖片<br /> 影片 </td>
<td> PicturesView <br />VideosView </td>
<td> <b>大小</b>：中，最好至少 190 (如果影像大小 190x130) <br />
<b>外觀比例</b>：統一，寬度外觀比例約 0.7 (大小為 190 時 190x130) <br />
裁剪以供預覽。 <br /> 
適用於在方格中對齊影像，因為統一的外觀比例。  </td>
</tr>
<tr>
<td> 文件<br />音樂 </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>大小</b>：小，最好至少 40 x 40 像素 <br />
<b>外觀比例</b>：統一，正方形外觀比例  <br />
適用於預覽專輯封面，因為正方形外觀比例。 <br /> 
文件在檔案選擇器視窗中看起來相同 (使用相同圖示)。 </td>
</tr>
</tr>
<tr>
<td> 任何單一項目 (無論檔案類型為何) </td>
<td> SingleItem </td>
<td> <b>大小</b>：小，最好至少 40 x 40 像素 <br />
<b>外觀比例</b>：統一，正方形外觀比例  <br />
適用於預覽專輯封面，因為正方形外觀比例。 <br /> 
文件在檔案選擇器視窗中看起來相同 (使用相同圖示)。 </td>
</tr>
</table>

以下範例顯示擷取的縮圖影像根據檔案類型與縮圖模式的差異：
<div class="mx-responsive-img">
<table>
<tr>
<th>項目類型</th>
<th>使用下列方式擷取時： <ul><li>PicturesView <li>VideosView</ul></th>
<th>使用下列方式擷取時： <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>使用下列方式擷取時： <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>圖片</td>
<td>縮圖影像使用檔案的原始外觀比例。 <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>縮圖會裁剪成正方形外觀比例。 <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>縮圖影像使用檔案的原始外觀比例。<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>影片</td>
<td>縮圖有圖示，可與圖片區分。 <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>縮圖會裁剪成正方形外觀比例。 <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>縮圖影像使用檔案的原始外觀比例。 <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>音樂</td>
<td>縮圖有適當大小的背景圖示。 背景色彩是由 app 的磚背景色彩決定。 <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>如果檔案有專輯封面，縮圖就是專輯封面。  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
否則，縮圖有適當大小的背景圖示。</td>
<td>如果檔案有專輯封面，則縮圖就是專輯封面，並採用檔案的原始外觀比例。  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
否則，縮圖會是圖示。 </td>
</tr>
<tr>
<td>文件</td>
<td>縮圖有適當大小的背景圖示。 背景色彩是由 app 的磚背景色彩決定。 <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>縮圖有適當大小的背景圖示。 背景色彩是由 app 的磚背景色彩決定。 <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>文件縮圖，如果有的話。 <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
否則，縮圖會是圖示。 <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>資料夾</td>
<td>如果資料夾中有圖片檔案，則會使用圖片縮圖。  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
否則不會擷取任何縮圖。</td>
<td>不會擷取任何縮圖影像。</td>
<td>縮圖是資料夾圖示。<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>檔案群組</td>
<td>如果資料夾中有圖片檔案，則會使用圖片縮圖。<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> 否則不會擷取任何縮圖。 </td>
<td>如果群組中的檔案當中有某一個檔案有專輯封面，則縮圖會是專輯封面。 <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />否則不會擷取任何縮圖。 </td>
<td>如果群組中的檔案當中有某一個檔案有專輯封面，則縮圖會是專輯封面，並使用檔案的原始外觀比例。 <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />否則，縮圖會是代表檔案群組的圖示。 <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>相關主題
- [ThumbnailMode 列舉](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [StorageItemThumbnail 類別](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [StorageFile 類別](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [檔案和資料夾縮圖範例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [清單和方格檢視](../design/controls-and-patterns/lists.md)