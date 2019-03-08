---
title: 開發人員計畫概觀
description: 深入了解不同的開發人員計劃可供使用 Xbox Live。
ms.assetid: 1166308a-4079-41b4-8550-ce04b82b4f72
ms.date: 05/30/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 開發人員計劃、 建立者
ms.localizationpriority: medium
ms.openlocfilehash: 05abd3f28328f4418f5a8a772049b3869b488ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603883"
---
# <a name="developer-program-overview"></a>開發人員計畫概觀

如果您想要開發 Xbox Live 啟用項目，有數個選項可供您。 每個提供不同層級的組件，提供給您，功能上需要投入的時間，以及支援選項。

## <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

如果您想要熟悉 Xbox Live 開發 Xbox Live 創作者計劃會是不錯的起點的 Xbox Live。 Microsoft 需要核准程序，才能加入此計畫，並有最少的認證和發佈需求。

Xbox Live 創作者計劃只支援建立的標題[通用 Windows 平台](https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide)(UWP)。  建立 Xbox One 主控台和 Windows 10 電腦上執行的 UWP 遊戲為這些項目。  如需進一步瞭解如何在 Xbox One 上執行 UWP 遊戲的詳細資訊，請參閱[Xbox One 上的 UWP](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/index)。  

Xbox One 上，以提供玩家策劃的市集體驗，透過 Xbox Live 創作者計劃發行的遊戲會在新的 [建立者收集] 區段的 Microsoft Store，在 Xbox 上販售。 這提供了確保任何人都可以在其中開發及出貨遊戲時和策劃的市集體驗主控台玩家都是知道並預期的開放平台之間取得平衡。 在 Windows 10 中，標題就會發佈之間的所有其他 Xbox Live 遊戲在 Microsoft Store 中。

### <a name="publishing-and-certification"></a>發行和憑證
您必須註冊於[合作夥伴中心開發人員計劃](https://developer.microsoft.com/store/register)Xbox Live 創作者計劃的一部分發行的遊戲。 有兩組您的遊戲必須遵循的需求：

1. Xbox Live 登入整合，並顯示使用者的身分識別 （玩家代號、 Gamerpic 等等）。 所有其他 Xbox Live 服務是選擇性的。
2. 請遵循標準[Microsoft Store 原則](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)。

### <a name="supported-xbox-live-services"></a>支援的 Xbox Live 服務
排行榜、 精選的統計資料、 標題儲存體、 連接的儲存體和一組有限的社交功能，可以使用 Xbox Live 創作者計劃下已啟用的標題。 成就、 線上多人遊戲和許多社交功能都**不**支援 Xbox Live 創作者計劃中的項目。

如需支援的服務的完整清單，請參閱 <<c0> [ 特徵資料表](#feature-table)。

### <a name="supported-third-party-game-development-engines"></a>支援的協力廠商開發遊戲引擎
Xbox Live 創作者計劃標題都可以使用一些受歡迎的遊戲引擎建置的 UWP 遊戲。 Microsoft 提供的整合到 UWP 的 Xbox Live 服務文件與所建置的遊戲[Unity 遊戲引擎](https://unity.com)。 您可以找到[文件](get-started-with-creators/develop-creators-title-with-unity.md)詳述與 Unity 遊戲這裡本網站上的 Xbox Live 的整合，以及下載和了解 Microsoft 建置[Xbox Live Unity 外掛程式](https://github.com/Microsoft/xbox-live-unity-plugin)。

Xbox Live 創作者計劃標題的類型也與遊戲引擎建置[(2 & 3) 的建構](https://www.scirra.com/construct2)，並[GameMaker Studio 2](https://www.yoyogames.com/gamemaker)。 同時遊戲引擎已新增 Xbox Live 的支援，不過，支援由遊戲引擎建立者和非 Microsoft。 詳細資料和加入的 Xbox Live 建構或 GameMaker Studio 2 專案的支援，您必須分別參考每個遊戲引擎文件。

[了解如何整合到您的建構專案的 Xbox Live。](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)

[了解如何整合 Xbox Live GameMaker Studio 2 專案。](https://www.yoyogames.com/gamemaker/xblc)

針對其他遊戲開發引擎，例如[MonoGame](https://www.monogame.net/)或[Xenko](https://xenko.com/)，不要不具有內建在 Xbox Live 功能或外掛程式，您仍然可以使用 Xbox Live Api 新增至您的標題的 Xbox Live。 若要從您的專案使用 Xbox Live API，您可以使用 NuGet 套件新增二進位檔的參考，或是新增 API 來源。 新增 NuGet 套件可以加快編譯過程，而新增來源可以讓偵錯變容易。

### <a name="support-and-feedback"></a>支援與意見反應
您可能會有任何問題可以回答上[MSDN 論壇](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev)。  您也可以詢問程式設計相關的問題，以[Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live)使用"xbox live"標記。  Xbox Live 團隊將會投入社群互動，根據在那裡收到的意見反應持續改善我們的 API、工具和文件。

Xbox Live 創作者計劃中的開發人員，您可以[提交新的想法](https://xbox.uservoice.com/forums/363186--new-ideas?category_id=196261)或是[對現有想法進行投票](https://xbox.uservoice.com/forums/251649?category_id=210838)在我們[Xbox User Voice](https://xbox.uservoice.com/forums/363186--new-ideas)。

## <a name="idxbox"></a>ID@Xbox

Xbox Live 創作者計劃最適合許多遊戲和開發人員。 但是，如果您想要存取完整 Xbox Live 的堆疊，包括線上多人遊戲、 成積和玩家分數，或您想要存取的完整功能的裝置使用硬體開發套件，Xbox One 家族[ ID@Xbox ](https://www.xbox.com/en-US/developers/id)程式適合您。

在遊戲ID@Xbox程式必須核准的概念並通過了完整的憑證上 Xbox One 和 Windows 10，大於綁約時間在您的組件。
ID@Xbox 標題取得主要存放區，與建立者集合中，可能會讓客戶更高的曝光度的一節中的位置。

中的開發人員ID@Xbox程式也存取開發人員支援和促銷協助 Microsoft，以及完整的私用的技術白皮書和開發人員技術論壇。 您可以繼續使用[MSDN 論壇](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev)詢問程式設計相關的問題或[Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live)視使用的"xbox live"的標記。

## <a name="microsoft-partners"></a>Microsoft 合作夥伴

使用的是 Microsoft 合作夥伴的遊戲發行商的開發人員可以存取完整的 Xbox Live 功能集，並專門用來協助您開發、 認證和發行程序的 Microsoft 代表。

## <a name="feature-table"></a>功能資料表

下表說明 Xbox Live 創作者計劃，可用的功能及[ ID@Xbox ](https://www.xbox.com/en-US/developers/id)程式。  

<table>

<tr>
<th>功能區</th>
<th>功能</th>
<th>描述</th>
<th> ID@Xbox </th>
<th>Xbox Live 創作者計畫</th>
</tr>

<tr>
<td rowspan="2" class="dev-program-feature-name">身份識別</td>
<td>登入/註冊</td>
<td>讓玩家能登入 Xbox Live 內您的標題，或視需要建立新的 Xbox Live 帳戶</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-required">必要</td>
</tr>

<tr>
<td>使用者身分識別</td>
<td>顯示的玩家代號、 Gamerpic 等，來使用 Xbox Live 的身分識別</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-required">必要</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="13" class="dev-program-feature-name">社交活動</td>

<td>基本組態選項</td>
<td>顯示顯示標題中的使用者活動的基本組態選項字串。  例如：「 Steve 扮演 Minecraft 」</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>最近播放</td>
<td>會出現在最近播放的項目中的 Xbox 應用程式 」 或 「 Xbox One</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>活動摘要</td>
<td>會出現在活動摘要中的 Xbox 應用程式 」 或 「 Xbox One</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>遊戲中心</td>
<td>有標題顯示在標題的特定摘要中的統計資料、 影片和其他內容相關聯的遊戲中樞</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>社團</td>
<td>播放程式可以使用 Xbox 應用程式或 Xbox One，若要建立可以與您的標題 （選擇性） 相關聯的社團。</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>尋找群組 (LFG)</td>
<td>LFG 可讓播放程式尋找其他項目外的-遊戲排程多人連線遊戲。</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>GameDVR</td>
<td>播放程式可以擷取他們的遊戲工作階段的視訊，並共用這些數字的活動摘要。</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>廣播</td>
<td>播放程式可以即時廣播串流服務，例如 Mixer 和 Twitch 透過其遊戲</td>
<td class="xbl-features-automatic">自動</td>
<td class="xbl-features-automatic">自動</td>
</tr>

<tr>
<td>豐富顯示狀態</td>
<td>在您的標題會顯示播放器的詳細的資訊。  基本的存在可能會顯示 「 使用者是汽車賽車遊戲中 」、 豐富的組態選項可讓您指定更詳細的字串，而 [使用者開車 SuperCar RainyForest 中]</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr>
<td>好友</td>
<td>擷取登入使用者的好友名單，以啟用社交遊戲案例中您的標題。</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-limited">選擇性 / 有限 （公開只能播放您的標題的好友）</td>
</tr>

<tr>
<td>隱私權</td>
<td>允許為靜音或封鎖的播放程式或其他玩家</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-optional">選擇性</td>
</tr>

<tr>
<td>評價</td>
<td>播放程式獲得或失去透過其行為的評價。 行為會在配對，並可供您的標題以自訂方式。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr>
<td>身分證管理員</td>
<td>有效率地擷取玩家的社交圖形的相關資訊</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-limited">選擇性 / 有限 （公開只能播放您的標題的好友）</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="4" class="dev-program-feature-name">資料平台</td>

<td>遊戲者統計資料</td>
<td>上傳用於排行榜的播放程式的相關統計資料。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-optional">選擇性 （資料平台 2017年只）</td>
</tr>

<tr>
<td>精選的統計資料</td>
<td>將特定統計資料指定為 「 精選的統計資料 會顯示在 遊戲 中樞。</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-optional">選擇性 （資料平台 2017年只）</td>
</tr>

<tr>
<td>排行榜</td>
<td>擷取並顯示播放程式統計資料的排序方式與鼓勵競賽。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-optional">選擇性 （資料平台 2017年只）</td>
</tr>

<tr>
<td>玩家分數的成就</td>
<td>將特定統計資料指定為 「 精選的統計資料 會顯示在 遊戲 中樞。</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="1" class="dev-program-feature-name">Media</td>

<td>內容相關式搜尋</td>
<td>加上註解 GameDVR 多媒體項目，方便尋找對應到他們想要觀看的剪輯的播放器的關鍵字。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>


<tr class="dev-program-feature-start">
<td rowspan="2" class="dev-program-feature-name">儲存體</td>

<td>連接的儲存空間</td>
<td>跨一個 Xbox 和電腦的漫遊遊戲儲存</td>
<td class="xbl-features-required">必要</td>
<td class="xbl-features-optional">選擇性</td>
</tr>

<tr>
<td>遊戲儲存空間</td>
<td>雲端儲存體進行大量的個別使用者或每個標題的資料。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-optional">選擇性</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="6" class="dev-program-feature-name">線上多人遊戲</td>

<td>多人遊戲工作階段目錄 (MPSD)</td>
<td>儲存多人連線的工作階段，例如清單的播放程式、 狀態等相關資訊。</td>
<td class="xbl-features-optional">必要</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr>
<td>配對</td>
<td>Xbox Live 可以比對不同的玩家在一起的多人連線的工作階段。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr>
<td>競技場</td>
<td>聯賽樣式時，會針對每個其他爭奪播放程式。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr>
<td>遊戲的對談</td>
<td>多人連線遊戲中的播放程式的語音聊天</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

<tr>
<td>Xbox Live 計算</td>
<td>部署可執行檔和您的標題可以通訊，來卸載來自用戶端的計算資產。</td>
<td class="xbl-features-optional">選擇性</td>
<td class="xbl-features-notavailable">不支援</td>
</tr>

</table>
