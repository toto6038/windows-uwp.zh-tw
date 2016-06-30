---
author: mtoepke
title: "遊戲的音訊"
description: "了解如何開發音樂和聲音，並將它們納入您的 DirectX 遊戲，以及如何處理音訊訊號來建立動態和聲音定位。"
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 98896d4966ee2adb3bf494dcba22656fc8bb6414

---

# 遊戲的音訊


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

了解如何開發音樂和聲音，並將它們納入您的 DirectX 遊戲，以及如何處理音訊訊號來建立動態和聲音定位。

對於音訊的程式設計，建議您使用 DirectX 中的 XAudio2 音訊庫，我們在這裡也會使用它。 XAudio2 是低階的音訊庫，為遊戲提供訊號處理 和混合的基礎，而且支援多種格式。

您也可以使用 [Microsoft 媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197)實作簡單的聲音和音樂播放。 Microsoft 媒體基礎是設計來播放音訊和視訊的媒體檔案和串流，但是也可以在遊戲中使用，而且對於遊戲中的電影場景或非互動式元件特別實用。

## 概念簡介


這裡是我們在本節中使用的一些音訊程式設計概念。

-   訊號是音訊程式設計的基本單位，類似於圖形中的像素。 處理它們的數位訊號處理器 (DSP) 就像是遊戲音訊的像素著色器一樣。 它們可以轉換訊號、組合訊號或過濾訊號。 藉由將訊號編排到 DSP，就可以修改遊戲的音效和音樂，這項作業可以很簡單也可以很複雜，視您的需要而定。
-   聲音則是兩個或多個訊號的混音複合。 XAudio2 聲音物件有 3 種類型：來源、次混音和主播放聲音。 來源聲音在用戶端提供的音訊資料上操作。 來源和次混音聲音會將它們的輸出傳送到一或多個次混音或主播放聲音。 次混音和主播放聲音會將傳入的所有聲音的音訊混合在一起，然後在結果上操作。 主播放聲音會將音訊資料寫入音訊裝置。
-   混音是將數個不連貫的聲音 (例如在場景中播放的音效和背景音訊) 組合成一個單一串流的程序。 次混音是將數個不連貫的訊號 (例如引擎雜音的元件聲音) 組合起來並建立聲音的程序。
-   音訊格式。 您可以將遊戲的音樂和音效儲存為各種數位格式。 有未壓縮格式 (如 WAV) 和壓縮格式 (如 MP3 和 OGG)。 取樣壓縮得越小 (通常是由它的位元速率指定，其中位元速率越低，壓縮的失真度越高)，逼真度就越差。 逼真度會因為壓縮配置和位元速率而有所不同，所以請試驗看看，找出最適合您遊戲的格式。
-   取樣率和品質。 聲音可以用不同的速率取樣，而以較低速率取樣的聲音逼真度較差。 CD 品質的取樣率是 44.1 Khz (44100 Hz)。 如果您不需要高逼真度的聲音，可以選用較低的取樣率。 較高的速率可能適合專業的音訊應用程式，但是除非您的遊戲需要專業逼真度的聲音，否則可能不需要它們。
-   聲音發射器 (或來源)。 在 XAudio2 中，聲音發射器是發射聲音的位置，它可能只是出現一下子的背景雜音，或是遊戲內點唱機播放的震耳搖滾樂音軌。 您可以使用全局座標指定發射器。
-   聲音接聽器。 聲音接聽器通常是播放器，或也許是更進階遊戲中的人工智慧 (AI) 實體，用來處理從接聽器接收到的聲音。 您可以將聲音以次混音的方式加入在播放器中播放的音訊串流，或者使用它來接受特定的遊戲內動作，像是將喚醒人工智慧防衛標示為接聽器。

## 設計考量


音訊是遊戲設計和開發中極為重要的部分。 許多遊戲玩家對於平庸的遊戲晉升為傳奇經典之作而感到記憶深刻，就只是因為遊戲有令人懷念的原聲帶，或絕佳的聲音處理以及混音，或動聽的音訊製作。 音樂和聲音可用來定位遊戲的特色，而且造就了主要引吸力，界定遊戲的特色，並讓它在其他類似的遊戲中脫穎而出。 您在設計和開發遊戲音訊設定檔上所花費的努力絕對是值得的。

3D 音訊定位可以將 3D 圖形提供的擬真經驗提升到更高的層次。 如果您正在開發一個模擬真實世界的複雜遊戲，或者遊戲需要電影式的風格，請考慮使用 3D 音訊定位技術，才能真正吸引玩家。

## DirectX 音訊開發藍圖


### XAudio2 概念性資源

XAudio2 是 DirectX 的音訊混合程式庫，主要用於開發高效能的遊戲音訊引擎。 對於想在新型遊戲中增加音效和背景音樂的遊戲開發人員而言，XAudio2 可提供低延遲的音訊圖形和混合引擎，並且支援動態緩衝區、同步取樣精確播放，以及隱含的來源速率轉換。

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
<td align="left"><p>[XAudio2 簡介](https://msdn.microsoft.com/library/windows/desktop/ee415813)</p></td>
<td align="left"><p>本主題提供 XAudio2 支援的音訊程式設計功能清單。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 入門](https://msdn.microsoft.com/library/windows/desktop/ee415762)</p></td>
<td align="left"><p>本主題提供主要 XAudio2 概念、XAudio2 版本及 RIFF 音訊格式的資訊。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[一般音訊程式設計概念](https://msdn.microsoft.com/library/windows/desktop/ee415692)</p></td>
<td align="left"><p>本主題提供音訊開發人員應熟悉的一般音訊概念概觀。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 聲音](https://msdn.microsoft.com/library/windows/desktop/ee415825)</p></td>
<td align="left"><p>本主題包含 XAudio2 聲音的概觀，XAudio2 聲音可用來次混音、操作和控制音訊資料。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[XAudio2 回呼](https://msdn.microsoft.com/library/windows/desktop/ee415745)</p></td>
<td align="left"><p>本主題涵蓋 XAudio 2 回呼，XAudio 2 回呼可用來避免音訊播放中斷。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 音訊圖形](https://msdn.microsoft.com/library/windows/desktop/ee415739)</p></td>
<td align="left"><p>本主題涵蓋 XAudio2 音訊處理圖形，它會從用戶端擷取一組音訊串流做為輸入，然後進行處理，並將最終結果傳遞到音訊裝置。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[XAudio2 音訊效果](https://msdn.microsoft.com/library/windows/desktop/ee415756)</p></td>
<td align="left"><p>本主題涵蓋 XAudio2 音訊效果，它會擷取傳入的音訊資料，並在傳送資料之前執行一些操作 (例如殘響效果)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用 XAudio2 串流處理音訊資料](https://msdn.microsoft.com/library/windows/desktop/ee415821)</p></td>
<td align="left"><p>本主題涵蓋使用 XAudio2 串流處理音訊。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[X3DAudio](https://msdn.microsoft.com/library/windows/desktop/ee415714)</p></td>
<td align="left"><p>本主題涵蓋 X3DAudio，這個 API 搭配 XAudio2 使用時，可創造音效來自 3D 空間中某個點的效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 程式設計參考](https://msdn.microsoft.com/library/windows/desktop/ee415899)</p></td>
<td align="left"><p>本節包含 XAudio2 API 的完整參考。</p></td>
</tr>
</tbody>
</table>

 

### XAudio2「使用方法」資源

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
<td align="left"><p>[使用方法：初始化 XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779)</p></td>
<td align="left"><p>了解如何透過建立 XAudio2 引擎的執行個體和建立主播放聲音，以初始化 XAudio2 進行音訊播放。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：在 XAudio2 中載入音訊資料檔](https://msdn.microsoft.com/library/windows/desktop/ee415781)</p></td>
<td align="left"><p>了解如何產生在 XAudio2 中播放音訊資料所需的結構。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：使用 XAudio2 播放音效](https://msdn.microsoft.com/library/windows/desktop/ee415787)</p></td>
<td align="left"><p>了解如何在 XAudio2 中播放先前載入的音訊資料。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：使用次混音聲音](https://msdn.microsoft.com/library/windows/desktop/ee415794)</p></td>
<td align="left"><p>了解如何設定聲音群組，以將它們的輸出傳送至相同的次混音聲音。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：使用來源聲音回呼](https://msdn.microsoft.com/library/windows/desktop/ee415769)</p></td>
<td align="left"><p>了解如何使用 XAudio2 來源聲音回呼。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：使用引擎回呼](https://msdn.microsoft.com/library/windows/desktop/ee415774)</p></td>
<td align="left"><p>了解如何使用 XAudio2 引擎回呼。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：建立基本音訊處理圖形](https://msdn.microsoft.com/library/windows/desktop/ee415767)</p></td>
<td align="left"><p>了解如何建立音訊處理圖形 (由單一主播放聲音和單一來源聲音建構而成)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：從音訊圖形動態新增或移除聲音](https://msdn.microsoft.com/library/windows/desktop/ee415772)</p></td>
<td align="left"><p>了解如何在依照[使用方法：建立基本音訊處理圖形](https://msdn.microsoft.com/library/windows/desktop/ee415767)步驟建立的圖形中，新增或移除次混音聲音。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：建立效果鏈](https://msdn.microsoft.com/library/windows/desktop/ee415789)</p></td>
<td align="left"><p>了解如何將效果鏈套用至聲音，以允許自訂該聲音的音訊資料處理。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：建立 XAPO](https://msdn.microsoft.com/library/windows/desktop/ee415730)</p></td>
<td align="left"><p>了解如何實作 [<strong>IXAPO</strong>](https://msdn.microsoft.com/library/windows/desktop/ee415893) 以建立 XAudio2 音訊處理物件 (XAPO)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：新增 XAPO 的執行階段參數支援](https://msdn.microsoft.com/library/windows/desktop/ee415728)</p></td>
<td align="left"><p>了解如何透過實作 [<strong>IXAPOParameters</strong>](https://msdn.microsoft.com/library/windows/desktop/ee415896) 介面，新增 XAPO 的執行階段參數支援。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：在 XAudio2 中使用 XAPO](https://msdn.microsoft.com/library/windows/desktop/ee415733)</p></td>
<td align="left"><p>了解如何在 XAudio2 效果鏈中使用實作為 XAPO 的效果。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：在 XAudio2 中使用 XAPOFX](https://msdn.microsoft.com/library/windows/desktop/ee415723)</p></td>
<td align="left"><p>了解如何在 XAudio2 效果鏈中使用 XAPOFX 中的其中一個效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：從磁碟串流處理音效](https://msdn.microsoft.com/library/windows/desktop/ee415791)</p></td>
<td align="left"><p>了解如何透過建立讀取音訊緩衝區的個別執行緒來串流處理 XAudio2 中的音訊資料，以及如何使用回呼以控制該執行緒。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[使用方法：整合 X3DAudio 與 XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415798)</p></td>
<td align="left"><p>了解如何使用 X3DAudio 來提供 XAudio2 聲音的音量與音調值，以及 XAudio2 內建的殘響效果的參數。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用方法：將音訊方法群組為操作集](https://msdn.microsoft.com/library/windows/desktop/ee415783)</p></td>
<td align="left"><p>了解如何使用 XAudio2 操作集，同時讓一組方法呼叫生效。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[對 XAudio2 中的音訊問題進行偵錯](https://msdn.microsoft.com/library/windows/desktop/ee415765)</p></td>
<td align="left"><p>了解如何設定 XAudio2 的偵錯記錄層次。</p></td>
</tr>
</tbody>
</table>

 

### 媒體基礎資源

媒體基礎 (MF) 是串流處理音訊和視訊播放的媒體平台。 您可以使用媒體基礎 API 來串流處理以多種演算法編碼和壓縮的音訊及視訊。 它不是針對即時的遊戲案例而設計的；它的主要目的是提供強大的工具和廣泛的轉碼器支援以支援更多音訊和視訊元件的線性擷取和呈現。

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
<td align="left"><p>[關於媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms696274)</p></td>
<td align="left"><p>本節包含媒體基礎 API 的一般資訊，和可支援它們的工具。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[媒體基礎：基本概念](https://msdn.microsoft.com/library/windows/desktop/ee663601)</p></td>
<td align="left"><p>本主題介紹撰寫媒體基礎應用程式之前必須了解的一些概念。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[媒體基礎架構](https://msdn.microsoft.com/library/windows/desktop/ms696219)</p></td>
<td align="left"><p>本節描述 Microsoft 媒體基礎的一般設計，及其使用的媒體基本類型和處理管線。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[音訊/視訊擷取](https://msdn.microsoft.com/library/windows/desktop/dd317910)</p></td>
<td align="left"><p>本主題描述如何使用 Microsoft 媒體基礎來執行音訊和視訊擷取。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[音訊/視訊播放](https://msdn.microsoft.com/library/windows/desktop/dd317914)</p></td>
<td align="left"><p>本主題描述如何在應用程式中實作音訊/視訊播放。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[媒體基礎中支援的媒體格式](https://msdn.microsoft.com/library/windows/desktop/dd757927)</p></td>
<td align="left"><p>本主題列出 Microsoft 媒體基礎原始支援的媒體格式 (協力廠商廠商可撰寫自訂的外掛程式來支援其他格式)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[編碼與檔案編寫](https://msdn.microsoft.com/library/windows/desktop/dd318778)</p></td>
<td align="left"><p>本主題描述如何使用 Microsoft 媒體基礎來執行音訊和視訊編碼，以及編寫媒體檔案。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Windows Media 轉碼器](https://msdn.microsoft.com/library/windows/desktop/ff819508)</p></td>
<td align="left"><p>本主題描述如何使用 Windows Media 音訊和視訊轉碼器來製作和使用壓縮的資料串流。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[媒體基礎程式設計參考](https://msdn.microsoft.com/library/windows/desktop/ms704847)</p></td>
<td align="left"><p>本節包含媒體基礎 API 的參考資訊。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[媒體基礎 SDK 範例](https://msdn.microsoft.com/library/windows/desktop/aa371827)</p></td>
<td align="left"><p>本節列出示範如何使用媒體基礎的範例應用程式。</p></td>
</tr>
</tbody>
</table>

 

### Windows 執行階段 XAML 媒體類型

如果您使用的是 [DirectX-XAML 互通性](https://msdn.microsoft.com/library/windows/apps/hh825871)，您可以在較簡單的遊戲案例使用 DirectX 搭配 C++，將 Windows 執行階段 XAML 媒體 API 合併到您的 Windows 市集應用程式。

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
<td align="left"><p>[<strong>Windows.UI.Xaml.Controls.MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)</p></td>
<td align="left"><p>代表包含音訊、視訊或兩者之物件的 XAML 元素。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[音訊、視訊和相機](https://msdn.microsoft.com/library/windows/apps/mt203788)</p></td>
<td align="left"><p>了解如何在通用 Windows 平台 (UWP) App 中包含基本音訊和視訊。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272)</p></td>
<td align="left"><p>了解如何在您的 UWP App 中播放本機儲存的媒體檔案。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272)</p></td>
<td align="left"><p>了解如何在您的 UWP App 中串流處理低延遲的媒體檔案。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[媒體投射](https://msdn.microsoft.com/library/windows/apps/mt282143)</p></td>
<td align="left"><p>了解如何使用「播放至」協定，將 UWP App 中的媒體串流處理至另一個裝置。</p></td>
</tr>
</tbody>
</table>

 

## 參考資料


-   [XAudio2 簡介](https://msdn.microsoft.com/library/windows/desktop/ee415813)
-   [XAudio2 程式設計指南](https://msdn.microsoft.com/library/windows/desktop/ee415737)
-   [Microsoft 媒體基礎概觀](https://msdn.microsoft.com/library/windows/desktop/ms694197)

> **注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相關主題


-   [XAudio2 程式設計指南](https://msdn.microsoft.com/library/windows/desktop/ee415737)

 

 







<!--HONumber=Jun16_HO4-->


