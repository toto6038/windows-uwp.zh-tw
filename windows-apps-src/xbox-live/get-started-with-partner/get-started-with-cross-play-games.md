---
title: 開始使用跨玩遊戲
description: 了解如何開發 PC 和 Xbox One 主控台執行的跨-玩遊戲。
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 一個跨 play xbox、 任何位置播放
ms.localizationpriority: medium
ms.openlocfilehash: 79b0c211291d8a27456126ea0378f9093d1dccab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646723"
---
# <a name="get-started-with-cross-play-games"></a>開始使用跨玩遊戲

## <a name="summary"></a>摘要

在推出的 Windows 10，遊戲開發人員將能夠發行單一產品，同時在 Xbox One （如有 XDK 的遊戲） 與 Windows 10 （當作 UWP 遊戲）。 在某些情況下，開發人員會想要啟用跨播放這些遊戲，遊戲的 Xbox One 和 Windows 10 版本，統一 Xbox Live 的服務，例如多人遊戲、 遊戲儲存、 成積、 和更多功能。 若要啟用跨播放，這些遊戲會在遊戲的 XDK 和 UWP 版本之間共用單一的標題的識別碼和 Xbox Live 服務組態。

擷取目前遊戲 XDK + UWP 需要 4 個主要步驟：

1.  建立您的 UWP 產品在合作夥伴中心

2.  建立您 XDK 的遊戲中 XDP，選取您想要共用 XBL 設定跨平的台

3.  繫結中 XDP 的 XDK 產品 UWP 產品資訊

4.  設定及發佈 Xbox Live 透過 XDP

這份文件的目的是要進一步詳細說明要以最輕鬆的方式擷取 XDK + UWP 跨平台遊戲的 Xbox 員工 4 個步驟

## <a name="terminology"></a>詞彙

### <a name="scenario-terms"></a>案例條款

1.  跨 Play:發行於一個以上的平台，但共用單一的 Xbox 的遊戲標題識別碼和服務組態。 最終結果是遊戲的兩個版本都共用相同 Xbox Live 設定-成就、 排行榜、 遊戲儲存、 多人遊戲，和更多功能。

2.  合作夥伴中心：[入口網站](https://partner.microsoft.com/dashboard)您可以在其中保留用於 UWP 開發今天和安裝 Xbox Live 設定適用於 UWP 應用程式身分識別。

3.  XDP:Xbox 開發人員入口網站中，這存在於目前若要擷取，設定及發行 Xbox One XDK 及 SRA 遊戲，和將會看到已新增的用以擷取、 設定及發佈 XDK + UWP 跨玩遊戲。

### <a name="identity-terms"></a>識別詞彙

1.  標題的識別碼：這是用來識別每個遊戲至 Xbox Live 的 Xbox 標題識別碼。 標題的識別碼會對應至單一產品，可能會跨越多個平台。

2.  服務組態識別碼 (SCID):每 Xbox 的標題 （Title 識別碼所識別） 都有對應的服務組態識別碼 (也稱為 SCID)。 此識別碼可讓 Xbox Live 來唯一識別規則 / 組態時要使用互動與您的標題。

3.  套件系列名稱 (PFN):這是指派給每個產品在合作夥伴中心內建立的身分識別。 一旦您將您的 UWP 繫結此合作夥伴中心產品的身分識別時，它會擔任這個 PFN。 PFNs 是唯一的產品識別碼，其可能會跨越多個平台。 PFNs 是 1:1 Xbox 標題的識別碼。

4.  MSA 應用程式識別碼：也稱為 MSA 用戶端識別碼，這會是在產品建立時指派的 MSA 在合作夥伴中心內的另一個應用程式身分識別。 此身分識別可協助識別您的應用程式的 Microsoft 服務。 MSA 應用程式識別碼是與 PFNs 1:1 (並據以 Xbox 標題識別碼)。

## <a name="scenario-overview"></a>案例概觀

### <a name="what-is-cross-play"></a>什麼是跨播放？

展示 Windows 10 體驗;跨 play 是跨裝置之間的 Xbox One 和電腦中的遊戲與遊戲地跨裝置多人遊戲、 成積和排行榜，這類案例的裝置版本之間共用單一的 Xbox Live 設定，並將儲存的遊戲。

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>跨播放的優缺點為何？

如果您想要您的 XDK 及 UWP 版本跨 Play 可能是遊戲的正確的方法，讓您：

-   參與跨裝置多人遊戲 (Xbox One vs。PC) 中至少一個多人遊戲的遊戲模式

-   共用單一的遊戲儲存的使用者可以使用這兩個裝置上

-   有一組的成就和玩家分數 / 挑戰 / 可以火進度針對這兩個裝置的排行榜

跨播放時可能不正確的方法，讓您：

-   您想要防止電腦及 Xbox One 遊戲的玩家參與任何及所有多人遊戲的遊戲模式中的裝置上的多人遊戲

-   您想要保留 Xbox One 和 PC 遊戲儲存個別 （也許是基於安全性或信任的原因）

-   您想要有不同的玩家分數 （也稱為購買 Xbox One 和電腦的使用者可以接收的每個平台，而不是共用的 1000年 1000年玩家分數） 遊戲的 Xbox One 和 PC 版本

一般情況下，跨 Play 將大部分的值，加入：

-   免費玩 Xbox 遊戲任何地方，強調遊戲的 Xbox One 和 PC 版本之間的持續性 /

-   提供跨裝置 Xbox One 和 PC 之間的多人遊戲

**附註**：跨播放可同時要同時釋出他們的遊戲的 XDK 及 UWP 版本的新遊戲，以及遊戲已出貨 XDK，但要新增 UWP 版本。

### <a name="what-are-the-restrictions-of-cross-play"></a>跨播放的限制有哪些？

Xbox One 支援 UWP 遊戲，直到 （適用於 Xbox One 主控台） 的 XDK 及遊戲 （適用於 Windows 10 電腦） 的 UWP 版本，也會需要跨玩遊戲。

任何 XDK + UWP 跨播放遊戲隨附一些重要的限制：

1.  **XDK 標題必須內嵌在 XDP**。 同時從服務組態和主線發佈的體驗，以支援架構的 XDK 標題不配合作夥伴中心。

2.  **可以是單一的服務組態建立 XDP 中使用這兩者的 XDK 和 UWP 版本遊戲**。 我們已加入 XDP 才能共用其 XDK 及 UWP 版本之間的單一服務組態的遊戲的新功能。 UWP 版本仍然需要在合作夥伴中心內發行的套件 / 完成類別目錄中，但所有的服務組態發佈 XDP。

3.  **服務組態無法分割 XDP 與合作夥伴中心之間**。 XDP 和合作夥伴中心並不知道彼此的 – 發行中任何現有發行另一個過度寫入。 這有可能會無法挽回地破壞服務組態，並建立可怕的使用者經驗 （消失成積、 遺失遊戲儲存等），並因此我們所需的 100%的服務組態，以完成 XDP XDK + UWP 跨播放遊戲。

### <a name="create-your-uwp-product-in-partner-center"></a>建立您的 UWP 產品在合作夥伴中心

依照下列指南，在合作夥伴中心建立 UWP 產品：[將 加入新的或現有 UWP 專案的 Xbox Live](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>安裝程式中 XDP 的 XDK 產品

現在，建立您的 UWP 時，即準備好設定中 XDP 的 XDK 產品。 如果您還沒有的 XDK 標題，您必須建立一個。

### <a name="create-your-xdp-product"></a>建立 XDP 產品

使用您的帳戶管理員，在 XDP 中建立新的產品，在您的發行者 ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher))。

當 XDP 中建立的產品，請確定您捲動至底部，選取您的平台 UI 的左側區段。 檢查每個平台，您**想要假以時日**版本上的遊戲與 Xbox Live 跨 play 整合。

一旦您選取您的平台，指定您的遊戲 （最有可能 XDK 標題） 的資源存取的類型，並想要發行此產品的機制。


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>更新您的 XDP 產品平台

如果您沒有現有的 XDK 產品 XDP 中，它就必須更新為支援 PC 平台。 若要這樣做，在此產品中，瀏覽至產品安裝程式&gt;平台類型。

![](../images/ingesting_crossplay_games_xdp/image10.png)

在此頁面上，選取您想要支援的平台 （選項為 Xbox One、 閾值 PC 和 Windows 行動裝置）。 一旦您的選擇感到滿意後，選取 [送出的平台] 按鈕。

這項變更會立即進行即時 （不需要的服務組態、 類別目錄或二進位檔發佈這個才會生效）。 請注意，此設定跨越沙箱，您不能有不同的平台類型，每個沙箱為遊戲。

### <a name="enter-your-msa-app-id"></a>輸入您的 MSA 應用程式識別碼

XDP 產品建立之後，請移至 [產品安裝] 頁面，輸入稍早建立的 MSA 應用程式識別碼的產品。 產品安裝程式，可以上最左邊的 觸達 「 狀態 」 工作軸方塊的產品中 XDP，如下所示。

![](../images/ingesting_crossplay_games_xdp/image11.png)

一旦您進行產品安裝程式頁面，選取 [應用程式識別碼 Setup] 區段。 在此區域中，您可以輸入您所擷取的 MSA 應用程式識別碼，並將它放在"Application ID"欄位中，如下所示。

![](../images/ingesting_crossplay_games_xdp/image12.png)

**您****不需要輸入名稱和發行者屬性**，**特別不應使用 「 取得應用程式識別碼 」 連結的頁面和**，因為您已經有您需要在此輸入 MSA 應用程式識別碼欄位，而且不想為您的應用程式產生新的資源。

一旦您在"Application ID"欄位中輸入您的 MSA 應用程式識別碼，請按一下 [提交應用程式識別碼 Setup] 按鈕。 這將會儲存您的 MSA 應用程式識別碼資訊與 Xbox Live 的安全性-每當您提出要求來擷取您的 UWP XToken 標題宣告內含要現在對應到此 XDP 產品 （只要用正確 UWP AppX 資訊清單識別您所建立d 在合作夥伴中心 ！)

### <a name="enter-the-partner-center-pfn-into-xdp"></a>輸入 XDP 合作夥伴中心 PFN

雖然上述的步驟足以讓您的 UWP 遊戲驗證和使用 Xbox Live 服務組態，您建立及發佈 XDP 中，某些 Xbox Live 的功能 （例如多人遊戲的邀請） 必須要知道您的 UWP 遊戲 PFN 才能順利運作。

若要這樣做，請瀏覽產品安裝程式&gt;繫結開發人員中心

![](../images/ingesting_crossplay_games_xdp/image13.png)

在此頁面上，輸入 PFN UWP 應用程式 （如下一節 4.1.1 中擷取），然後選取 [儲存] 按鈕。

此組態不立即進行即時。 未來透過即時重新上線服務組態會發佈至沙箱。 同樣地，此資訊已沙箱化，並需要發佈至每個可用的沙箱。

### <a name="flag-your-app-for-xbox-cert-in-partner-center"></a>您的應用程式的旗標在合作夥伴中心內的 Xbox 憑證

現在，讓您辨識為 Xbox 憑證已啟用的 Xbox Live 的遊戲需要一些手動介入的情況。 使用您的發行管理員為您的應用程式的旗標是在合作夥伴中心為 SmartCert 偵測 Xbox Live 啟用。

### <a name="update-your-uwp-game-in-the-xbox-app"></a>更新您的 Xbox 應用程式中的 UWP 遊戲

一般來說，Xbox 應用程式中使用自動產生的標題識別碼所有 UWP 遊戲與 Xbox Live Xbox 應用程式內體驗。 若要有適當的 Xbox 應用程式中使用標題識別碼 XDP 產生 UWP 遊戲時，資料更新必須讓合作夥伴中心內**提交您的 UWP 遊戲版本之前。**

若要這樣做，請連絡您的補西牆，並告訴他們您想要更新您的 Xbox 應用程式標題的識別碼標題名稱即可。  務必要包含在 XDP 所建立的標題識別碼 (顯示在產品安裝程式在 XDP&gt;產品詳細資料) 和 URL 適用於 Windows 10 的適用於您的 UWP (應用程式管理 下顯示在合作夥伴中心&gt;應用程式身分識別)。

## <a name="configure-xbox-live-in-xdp"></a>設定 Xbox Live 中 XDP

### <a name="service-configuration"></a>服務組態

與我們 XDP 和合作夥伴中心產品正確設定，而且繫結，您可自行設定 XDP 內您共用的 Xbox Live 設定，可以用正常方式 XDK 標題項目。

**提醒**– 繫結 XDK + UWP 遊戲應該、 在任何情況下，啟用、 設定或發行透過合作夥伴中心 Xbox Live 服務組態。 遵循本指南的失敗可能永久會傷害您的遊戲的 Xbox Live 設定。

### <a name="catalog-configuration"></a>全文檢索目錄組態

XDK + UWP 遊戲時，全文檢索目錄組態必須安裝程式兩次： 一次在您的 XDK 的 XDP 及適用於您的 UWP 的合作夥伴中心中。

XDP 設定時，這正是與正常的 XDK 產品相同。 合作夥伴中心設定時，可以找到更詳細的步驟[此處](https://dev.windows.com/en-us/publish)。

<table>
  <tr>
    <td>
      適用於僅限 UWP 遊戲時，僅提供有限的全文檢索目錄組態必須是安裝程式。 特別是，因為服務組態會取用任何 XDP 設定 Xbox Live 的遊戲在此處輸入的字串，就應該行銷資訊填妥適用於僅限 UWP 的遊戲。 此外，需應該使用 「 無法使用 」 的安裝程式以確保，遊戲絕不會出現在 Xbox One 目錄時的類別目錄資訊取得已發行的遊戲。
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>二進位的組態

XDK + UWP 遊戲時，二進位的組態必須安裝程式兩次： 一次在您的 XDK 的 XDP 及適用於您的 UWP 的合作夥伴中心中。

XDP 設定時，這正是與正常的 XDK 產品相同。 合作夥伴中心設定時，可以找到更詳細的步驟[此處](https://dev.windows.com/en-us/publish)。

<table>
  <tr>
    <td>
      適用於僅限 UWP 遊戲中，則不需要在 XDP 二進位的組態。 您可以完全略過此步驟。
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>在 XDP 中發行

一般而言，發行的 XDK + UWP 遊戲會遵循相同的程序，為分開 XDP 和您在合作夥伴中心 UWP 中發行的 XDK 標題。 因此以下重點放在唯一的處理程序或對跨玩遊戲的考量。

### <a name="dev-sandbox-publishing"></a>開發人員沙箱發行

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>沒有開發沙箱目錄的對等的 Microsoft Store

XDP 可讓您將您的目錄和二進位檔發佈到 Xbox One 目錄開發人員沙箱版本，Microsoft Store 目錄有沒有沙箱支援。 因此，在開發沙箱中測試您的 UWP 需要您要側載該 UWP，並直接播放。 這不會影響 Xbox Live 測試，但可能會改變您測試處理程序的標準。

<table>
  <tr>
    <td>
      針對僅限 UWP 遊戲時，它是仍然需要發佈 XDK 解除封鎖服務組態項目類別目錄資訊發行，即使僅限 UWP 遊戲的不 Xbox One 的目錄是否存在。
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>CERT 沙箱發行

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>協調 XDP 發佈與合作夥伴中心發佈

XDP，從憑證移至零售版，是兩個不同且明確的動作。 不過，在合作夥伴中心提交程序會自動移您透過憑證和入零售的遊戲。 此情況下，有其應遵守的作業優先順序。

當您準備好移至憑證時，您應該遵循下列步驟順序：

1.  在 憑證 （包括目錄、 二進位和服務組態） 的 XDP 發行 XDK 產品

2.  啟動您的合作夥伴中心提交您的 UWP 產品

    1.  **務必選取 「 手動發佈此應用程式 」 或 「 即\[日期\]」 發行日期 欄位中 ！** 如果您不這樣做，可能會 UWP 遊戲發行到零售不需要您介入的自動

<table>
  <tr>
    <td>
      僅限 UWP 遊戲時，它是仍然需要發佈您的目錄和服務組態至 XDP 中的憑證，再開始您的合作夥伴中心提交，即使您沒有您要發行的 XDK 標題二進位檔。
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>零售沙箱發行

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>協調 XDP 發佈與合作夥伴中心發佈

一旦完成 XDK 標題的憑證，且您的 UWP 離開憑證，且準備好要發行，您就準備好發佈您的應用程式為零售版。 同樣地，務必依照此程序，以確保您的 XDK 標題與 UWP 保持對齊以特定順序。

1.  一旦您準備好，將 XDK 產品發行中 XDP 為零售版 （包括目錄、 二進位和服務組態）

2.  在合作夥伴中心，在您的產品，[憑證] 頁面上選取 [立即發佈] 如果您先前已選取 [手動： 發佈此應用程式]，或等候發佈到就會發生，如果您選取 「 即\[日期\]"。

這些步驟都完成時，您應該有 XDK 標題和發行至世界中，使用共用的服務組態中零售的 UWP 遊戲。 恭喜！

<table>
  <tr>
    <td>
      針對僅限 UWP 遊戲時，仍然需要發佈您的目錄，並在發佈完成之前在合作夥伴中心，否則發行的 UWP XDP 中零售的服務組態將無法存取 Xbox Live。
    </td>
  </tr>
</table>
