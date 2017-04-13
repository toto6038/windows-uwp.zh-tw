---
description: "針對特定類型的輸入與裝置自訂您的 UWP 應用程式。 利用觸控和語音命令。 在 Xbox、手機，甚至電視上執行您的應用程式。"
title: "UWP 應用程式輸入和裝置設計 - Windows 應用程式開發"
author: kbridge
keywords: "裝置基本資訊,應用程式輸入, 自訂 UWP 應用程式"
label: Input & devices
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: b771d452-c3ac-4d97-8482-eaf81bf34306
ms.openlocfilehash: 6bcc81d80bb3e2167b6d6e5ee078279bd830f04c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="inputs-and-devices"></a>輸入和裝置

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

UWP 應用程式可自動處理各種輸入並在各種不同的裝置上執行 — 例如，您不需要額外做任何動作即可啟用觸控輸入，或讓您的應用程式在手機上執行。

但是您有時候可能會想要針對特定類型的輸入或裝置將您的應用程式最佳化。 例如，如果您建立繪圖應用程式，您可能想要自訂手寫筆輸入的處理方式。

本節中的設計與程式碼撰寫指示可協助您針對特定類型輸入與裝置自訂您的 UWP app。

## <a name="input-primer"></a>輸入基本資訊

請參閱我們的<b>[輸入基本資訊](input-primer.md)</b>，以熟悉與特定尺寸規格配對使用時，各個輸入裝置類型及其行為、功能與限制。

## <a name="inputs-and-interactions"></a>輸入與互動

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Surface Dial](windows-wheel-interactions.md)</b><br/>
了解如何將這個全新的輸入裝置類別整合到您的 Windows 應用程式。</br>
此裝置是做為次要、多重強制回應的輸入裝置，補充或修改主要裝置的輸入。
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Cortana](cortana-interactions.md)</b><br/>
使用可在外部應用程式中啟動及執行單一動作的語音命令，來延伸 Cortana 的基本功能。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[語音](speech-interactions.md)</b><br/>
將語音辨識和文字轉換語音 (也稱為 TTS，或語音合成) 直接整合到 app 的使用者體驗。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[手寫筆](pen-and-stylus-interactions.md)</b><br/>
將您的 UWP app 針對手寫筆輸入進行最佳化，以便既能為您的使用者提供標準指標裝置功能，也能提供最佳的 Windows Ink 體驗。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[鍵盤](keyboard-interactions.md)</b><br/>
鍵盤輸入是應用程式整體使用者互動體驗的一個重要部分。 對於某些行動不便或認為使用鍵盤與應用程式互動較有效率的使用者來說，鍵盤是不可或缺的。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[觸控](touch-interactions.md)</b><br/>
UWP 包含一些可處理觸控輸入的不同機制，所有這些機制都可讓您建立能夠讓使用者自信探索的沈浸式體驗。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[觸控板](touchpad-interactions.md)</b><br/>
觸控板結合了間接多點觸控輸入與指標裝置 (如滑鼠) 精確輸入。 這項結合讓觸控板既適用於觸控最佳化 UI，也適用於較小的生產力 app 目標。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[滑鼠](mouse-interactions.md)</b><br/>
滑鼠輸入最適合在指向及點選方面要求精確的使用者互動。 Windows 的 UI 已針對觸控的不精確本質進行最佳化，可自然地支援這種固有的精確度。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[遊戲台與遙控器](gamepad-and-remote-interactions.md)</b><br/>
UWP 應用程式現在支援遊戲台與遙控器輸入。 遊戲台與遙控器是 Xbox 和 TV 體驗的主要輸入裝置。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[多重輸入](multiple-input-design-guidelines.md)</b><br/>
為了儘可能配合較多的使用者和裝置，建議您將應用程式設計成可以與越多的輸入類型 (手勢、語音、觸控、觸控板、滑鼠及鍵盤) 搭配使用越好。 這樣會最大化彈性、可用性及可存取性。
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[識別輸入裝置](identify-input-devices.md)</b><br/>
識別連接至通用 Windows 平台 (UWP) 裝置的輸入裝置，以及識別它們的功能和屬性。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[處理指標輸入](handle-pointer-input.md)</b><br/>
在通用 Windows 平台 (UWP) App 中，接收、處理及管理來自指標裝置 (例如觸控、滑鼠、畫筆/手寫筆及觸控板) 的輸入資料。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[自訂文字輸入](custom-text-input.md)</b><br/>
Windows.UI.Text.Core 命名空間中的核心文字 API 可讓 UWP 應用程式接收來自 Windows 裝置上所支援之任何文字服務的文字輸入。 這會讓 App 能夠接收採用任何語言及來自任何輸入類型 (例如鍵盤、語音或手寫筆) 的文字。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[選取文字和影像](guidelines-for-textselection.md)</b><br/>
本文說明如何選取及操作文字、影像以及控制項，並提供在應用程式中使用這些機制時，所應考慮的使用者經驗指導方針。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[移動瀏覽](guidelines-for-panning.md)</b><br/>
移動瀏覽或捲動可讓使用者在單一檢視內進行瀏覽，以顯示無法容納在檢視區中的檢視內容。
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[視覺化縮放和調整大小](guidelines-for-optical-zoom.md)</b><br/>
本文描述 Windows 縮放和調整元素大小的方式，並提供在 App 中使用這些互動機制時的使用者經驗指導方針。
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[旋轉](guidelines-for-rotation.md)</b><br/>
本文說明旋轉的新 Windows UI，並提供在 UWP 應用程式中使用這項新的互動機制時，所應考慮的使用者經驗指導方針。
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[目標預測](guidelines-for-targeting.md)</b><br/>
Windows 中的觸控目標預測，使用觸控數位板所偵測到每一根手指的完整接觸區域。 數位板所回報之較大、較複雜的一組輸入資料可用來提高判斷使用者意向 (或最有可能的) 目標時的準確度。
</p>
</div>
<div class="side-by-side-content-right">
<p><b>[視覺化回饋](guidelines-for-visualfeedback.md)</b><br/>
使用視覺化回饋以向使用者顯示系統已偵測到、解譯及處理他們的互動。 視覺化回饋可以透過激發互動意願來協助使用者。 它會指出互動是否成功來改善使用者的控制感應。 它也會轉送系統狀態並減少錯誤。
</p>
</div>
</div>
</div>

## <a name="devices"></a>裝置

認識支援 UWP 應用程式的裝置，可協助您針對各種尺寸提供最佳的使用者體驗。 針對特定裝置進行設計時，主要的考量包括應用程式在該裝置上將如何顯示、將在該裝置上的何處使用應用程式、何時使用和如何使用，以及使用者將如何與該裝置互動。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[裝置基本資訊](device-primer.md)</b><br/>認識支援 UWP 應用程式的裝置，可協助您針對各種尺寸提供最佳的使用者體驗。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[針對 Xbox 和電視進行設計](designing-for-tv.md)</b><br/>設計您的「通用 Windows 平台」(UWP) 應用程式，讓它在 Xbox One 及電視螢幕上看起來既美觀又能正常運作。
</p>
  </div>
</div>
</div>
