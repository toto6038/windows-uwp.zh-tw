---
description: 對話方塊和飛出視窗會在使用者要求暫時性 UI 元素，或發生需要通知或核准的情況時顯示暫時性 UI 元素。
title: 對話方塊和飛出視窗
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: bf4746a6d3a024fec31045bbf9c63d3b11315b8c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033841"
---
# <a name="dialogs-and-flyouts"></a>對話方塊和飛出視窗

對話方塊和飛出視窗為在發生需要來自使用者的通知、核准或是其他資訊的情況時，所顯示的暫時性 UI 元素。

> **平台 API：** [ContentDialog 類別](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、 [Flyout 類別](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

**對話方塊**

![對話方塊範例](../images/dialogs/dialog_RS2_delete_file.png)

對話方塊是提供內容相關應用程式資訊的強制回應 UI 重疊項目。 對話方塊會阻擋與應用程式視窗的互動，直到對話方塊確實關閉為止， 而且它們通常會需要使用者執行某種動作。

**飛出視窗**

![飛出視窗的範例](../images/flyout-example2.png)

飛出視窗是一種輕量型的內容相關快顯視窗，會顯示與使用者動作相關的 UI。 其中包含位置和大小調整邏輯，並且可以用來顯示次要控制項或顯示更多有關項目的詳細資料。

與對話方塊不同的是，飛出視窗可以透過點選或按一下飛出視窗以外的地方、按 Esc 鍵或 [上一頁] 按鈕、重新調整 App 視窗的大小，或是變更裝置的方向，來快速將它關閉。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

對話方塊和飛出視窗可確保使用者知曉重要的資訊，但也會干擾使用者體驗。 因為對話方塊為強制回應 (封鎖)，所以會打擾使用者，並防止他們繼續執行動作，直到他們與對話方塊互動為止。 飛出視窗能提供較不突兀的體驗，但是顯示太多的飛出視窗也可能會造成困擾。

在您決定要使用對話方塊或飛出視窗之後，您必須選擇要使用哪一個。

由於對話方塊會阻止互動，而飛出視窗不會，您應該將對話方塊用在想讓使用者放下手上的工作，並將注意力放到特定資訊或是回答問題的情況。 相反地，飛出視窗適合在您想要引起使用者注意，但即使他們想要忽略它也無妨的情況下使用。

   <p><b>適合使用對話方塊的情況...</b> <br/>
<ul>
<li>表示使用者<b>「必須」</b>先閱讀並確認才能繼續執行工作的重要資訊。 範例包括：
<ul>
  <li>當使用者的安全性可能受到破壞時</li>
  <li>當使用者將要永久修改重要資產時</li>
  <li>當使用者將要刪除重要資產時</li>
  <li>確認 App 內購買</li>
</ul>

</li>
<li>套用到整個應用程式內容的錯誤訊息，例如連線錯誤。</li>
<li>問題，當應用程式需要詢問使用者關於造成應用程式無法繼續執行的問題時 (例如當應用程式無法代替使用者選擇時)。 封鎖性問題不能忽略或延遲回應，因此應該向使用者提供明確定義的選擇。</li>
</ul>
</p>


   <p><b>適合使用飛出視窗的情況...</b> <br/>
<ul>
<li>在完成動作之前收集所需的其他資訊。</li>
<li>顯示僅在某些情況下有關聯的資訊。 例如，在影像中心 App 中，當使用者按一下影像縮圖時，您可以使用飛出視窗顯示該影像的大尺寸版本。</li>
<li>顯示詳細資訊，例如頁面上項目的詳細資料或較長描述。</li>
</ul></p>

## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>避免使用對話方塊與飛出視窗的方法

考量您要分享之資訊的重要性：是否重要到必須打擾使用者？ 也請考慮需顯示資訊的頻率。如果您每隔幾分鐘便需要顯示對話方塊或通知，您可以考慮將這項資訊配置在主要 UI 上。 例如，在聊天用戶端中，與其在有朋友登入時就顯示飛出視窗，您可以顯示當時在線上的朋友清單，並在有朋友登入時針對他們進行醒目提示。

對話方塊常用來在執行動作之前確認該動作 (例如刪除檔案)。 如果您預期使用者會頻繁執行某項特定的動作，請考慮讓使用者能夠在錯誤地做出該動作時進行復原，而不是強制使用者每次都必須確認該動作。

## <a name="how-to-create-a-dialog"></a>如何建立對話方塊

請參閱[對話方塊文章](dialogs.md)。 

## <a name="how-to-create-a-flyout"></a>如何建立飛出視窗

請參閱[飛出視窗文章](flyouts.md)。 

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

