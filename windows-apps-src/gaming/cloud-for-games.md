---
author: joannaleecy
title: 針對 UWP 遊戲使用雲端服務
description: 深入了解將雲端實作為 UWP 遊戲的後端。
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.author: joanlee
ms.date: 03/27/2018
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 雲端服務
ms.localizationpriority: medium
ms.openlocfilehash: 5d15d3e6b6beb773a8d606db7a5d8a17544270be
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5926524"
---
#  <a name="using-cloud-services-for-uwp-games"></a>針對 UWP 遊戲使用雲端服務

Windows10 中的通用 Windows 平台 (UWP) 提供一組 API，可用來在 Microsoft 裝置上開發遊戲。 開發跨平台與裝置的遊戲時，您可以利用雲端後端來協助根據需求調整遊戲。

如果您在為您的遊戲尋找完整的雲端後端解決方案，請參閱[適用於遊戲後端的軟體即服務](#software-as-a-service-for-game-backend)。

##  <a name="what-is-cloud-computing"></a>什麼是雲端運算？

雲端運算透過網際網路使用隨選 IT 資源和應用程式，來為您的裝置儲存和處理資料。 _雲端_一詞是一種隱喻，代表外面 (本機資源之外) 可供使用的大量資源，而您可以從非特定位置存取這些資源。
雲端運算的原則，提供使用資源與軟體的新方式。 使用者不再需要預先支付整套完整的產品或資源，而能以服務的方式使用平台、軟體及資源。 雲端提供者通常會根據客戶的使用量或服務方案供應項目來計費。

##  <a name="why-use-cloud-services"></a>為什麼要使用雲端服務？

使用適用於遊戲的雲端服務，其中一項優勢是您不需要預先投資實體硬體伺服器，而只要在稍後的階段根據使用量或服務方案支付費用即可。 這是開發新遊戲時，有助於管理所牽涉風險的一種方式。 

另一項優點，則是您的遊戲將可以存取大量的雲端資源以達到延展性 (有效率地管理任何突然增加的最大同時玩家數、密集的即時遊戲運算，或是資料需求)。 這能使您遊戲的效能持續維持穩定。 此外，雲端資源可以由世界各地執行任何平台的任何裝置存取，這代表您可以將遊戲帶給世界上的任何人。

將酷炫的遊戲體驗傳遞給玩家是非常重要的。 因為遊戲伺服器是在雲端中執行並和用戶端更新無關，因此整體而言，它們可以為遊戲提供更受控制且安全的環境。   藉由雲端，您也可以透過一律不信任用戶端並使用伺服器端遊戲邏輯，來達成遊戲方式的一致性。 您也可以設定服務對服務連線，以允許更多整合的遊戲體驗，範例包含將遊戲內購買和多種不同的付款方式連結、引入不同的遊戲網路，以及將遊戲內更新分享到熱門的社交媒體入口網站 (例如 Facebook 和 Twitter)。 

您也可以使用專用的雲端伺服器建立大型的持續性遊戲世界、建立玩家社群、隨時間收集並分析玩家資料以改善遊戲，以及最佳化遊戲的獲利設計模型。

此外，需要密集遊戲資料管理功能的遊戲 (例如具有非同步多人遊戲機制的社交遊戲) 可以使用雲端服務來實作。

##  <a name="how-game-companies-use-the-cloud-technology"></a>遊戲公司如何使用雲端技術？

了解其他開發人員如何在遊戲中實作雲端解決方案。

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>開發人員</th>
        <th>描述</th>
        <th>主要遊戲案例</th>
        <th>深入了解</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent 遊戲</a></td>
        <td><b>Tencent 遊戲</b>具有使用 Azure Service Fabric 開發的創新解決方案，可用來將傳統電腦遊戲當做服務來提供。 其雲端遊戲解決方案使用在後端以微服務方式執行工作負載的「精簡型用戶端 + 豐富型雲端」模型。</td>
        <td>
            <ul>
                <li>傳統電腦遊戲當做雲端遊戲提供給世界各地的使用者 <li>最佳化的遊戲傳遞程序 <li>遊戲功能當做微服務來隔離，以降低複雜性、減少工作負載因相依性而發生的重複，而且有能力獨立升級新功能 <li>小型安裝套件下載，以便於玩最新的遊戲內容 (將套件大小從 GB 縮減為 MB) <li>降低維護成本 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent 遊戲和 Microsoft 建置了雲端遊戲解決方案</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">使用 Service Fabric 建置遊戲：關於 Tencent 實作的詳細資訊 (影片)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>Halo 5: Guardians</b> 使用 Azure Cosmos DB (透過 DocumentDB API) 實作 <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan Companies</a> 做為其社交遊戲平台，選取此技術是因為其自動索引功能所提供的速度及彈性。</td>
        <td>
            <ul>
                <li>透過可調整資料層來處理多人遊戲的群組建立/管理 <li>遊戲與社交媒體整合 <li>透過多個屬性進行即時資料查詢 <li>遊戲成就與統計資料的同步處理 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">使用 Azure Cosmos DB (透過 DocumentDB API) 實作的社交遊戲</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games 建立了 <b>Age of Ascent</b>，一款大型多人線上 (MMO) 史詩 3D 太空遊戲，可在具有現代化瀏覽器的裝置上進行。 因此這款遊戲可在電腦、膝上型電腦、手機和其他行動裝置上進行，而不需要外掛程式。遊戲使用 ASP.NET Core、HTML5、WebGL 及 Azure。</td>
        <td>
            <ul>
                <li>跨平台的瀏覽器型遊戲 <li>單一的大型持續性開放世界 <li>處理密集的即時遊戲運算 <li>隨著玩家數目進行調整 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">使用 Service Fabric 建置遊戲：Age of Ascent MMO 遊戲 (影片)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">使用 Azure Service Fabric 將遊戲元件以微服務的形式進行管理 (影片)</a> 
                <li><a href="https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Age of Ascent 開發人員訪談 (影片)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.nextgames.com/">Next Games</a></td>
        <td>Next Games 是 <b>The Walking Dead: No Man's Land</b> 電玩遊戲的製作者，此遊戲是根據 AMC 的原創影集改編。 The Walking Dead 遊戲使用 Azure 做為後端。 它在首個週末便達到 1,000,000 次的下載次數，並在首週便拿下美國 App Store 中 iPhone 與 iPad 免費 App 的第 1 名、12 個國家中免費 App 的第 1 名，以及 13 個國家中免費遊戲的第 1 名。
        </td>
        <td>
            <ul>
                <li>跨平台 <li>回合制多人遊戲 <li>有彈性地調整效能 <li>玩家防詐騙保護 <li>動態內容傳遞 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">我們的建置方式：Azure 上的 Next Games 全球線上遊戲平台 (部落格與影片)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">Walking Dead 使用 Azure Cosmos DB (透過 DocumentDB API) 獲得更快的開發週期與更吸引人的遊戲方式</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixel Squad</a></td>
        <td>Pixel Squad 使用 Unity 遊戲引擎與 Azure 開發 <b>Crime Coast</b>。 <b>Crime Coast</b> 是一款社交策略遊戲，可在 Android、iOS 和 Windows 平台上取得。 他們在遊戲中使用 Azure Blob 儲存體、受管理的 Azure Redis Cache、負載平衡的 IIS VM 陣列，以及 Microsoft 通知中樞。 了解他們如何管理調整，並處理同時湧入的 5000 名玩家。
        </td>
        <td>
            <ul>
                <li>跨平台 <li>多人線上遊戲 <li>隨著玩家數目調整 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Crime Coast MMO 遊戲使用 Azure 雲端服務的方式</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>其他連結

* [Hitman 和 Azure：建立像 Elusive Target 一樣的遊戲功能，只有使用雲端才可能實現這些功能](https://channel9.msdn.com/Series/Hitman)
* [Azure 是 Hitcents、Game Troopers 及 InnoSpark 的幕後秘訣](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [使用 Azure 的 Bizspark 計畫遊戲新創公司](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>如何設計雲端後台

在製作人與遊戲設計師討論必須包含在遊戲內的特色及功能時，最好也開始考慮要如何設計遊戲的基礎結構。 當您想為不同裝置及不同的主要平台開發遊戲時，可以使用 Azure 做為遊戲的後端。

### <a name="understanding-iaas-paas-or-saas"></a>了解 IaaS、PaaS 或 SaaS

首先，您需要了解最適合您遊戲的服務層級。 了解以下三種服務的差異，有助於您決定建置後端所要採取的方法。

* [基礎結構即服務 (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    基礎結構即服務 (IaaS) 是即時運算基礎結構，透過網際網路進行佈建及管理。 想像一下能根據需求讓許多電腦隨時快速相應增加和減少的可能性。 IaaS 協助您避免購買和管理自己的實體伺服器及其他資料中心基礎結構的成本和複雜度。

* [平台即服務 (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    平台即服務 (PaaS) 就像 IaaS，不過它也包含基礎結構 (例如伺服器、儲存體和網路) 的管理。 因此在不購買實體伺服器與資料中心基礎結構的基礎上，您也不需要購買和管理軟體授權、基礎的應用程式基礎結構、中介軟體、開發工具或其他資源。

* [軟體即服務 (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    軟體即服務 (SaaS) 可讓使用者透過網際網路連接至雲端式應用程式並使用這些應用程式。 這可提供您按照隨用即付方式向雲端解決方案提供者購買的完整軟體解決方案。  例如，電子郵件、行事曆和辦公室工具 (例如 Microsoft Office 365) 等解決方案便是。 您為自己的組織租用應用程式，您的使用者再透過網際網路 (通常使用網頁瀏覽器) 連接至該應用程式。 所有的基礎結構、中介軟體、應用程式軟體及應用程式資料都位於服務提供者的資料中心。 服務提供者會管理硬體和軟體，並在適當服務合約的規範下，確保遊戲以及您的資料的可用性與安全性。 SaaS 可讓您的組織以最低的先期成本來快速啟動並執行應用程式。


### <a name="design-your-game-infrastructure-using-azure"></a>使用 Azure 設計遊戲基礎結構

以下是可以針對遊戲使用 Azure 雲端供應項目的一些方法。 Azure 可以搭配 Windows、Linux，以及 Ruby、Python、Java 和 PHP 等常見的開放原始碼技術使用。 如需詳細資訊，請參閱[用於遊戲的 Azure](https://azure.microsoft.com/solutions/gaming/)。

| 需求                 | 活動案例                            | 產品供應項目                      | 產品功能                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| 在雲端託管您的網域     | 有效率地回應 DNS 查詢            | [Azure DNS](https://azure.microsoft.com/services/dns/) | 以具有高效能和可用性的方式託管您的網域  |
| 登入、身分識別驗證      | 已驗證玩家登入和玩家身分識別  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | 使用多重要素驗證來單一登入至任何雲端與內部部署 Web 應用程式            | 
| 使用基礎結構即服務 (IaaS) 模型的遊戲      | 遊戲託管於雲端的虛擬機器上       | [Azure VM](https://azure.microsoft.com/services/virtual-machines/) | 做為具有內建虛擬網路和負載平衡的遊戲伺服器，在 1 部到數千部虛擬機器執行個體之間進行調整。搭配混合式一致性的內部部署系統           |
| 使用平台即服務 (PaaS) 模型的網路或行動裝置遊戲            | 遊戲託管於受管理的平台上                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | 適用於網站或行動裝置遊戲的 PaaS (代表具有中介軟體/開發工具/BI/DB 管理的 Azure VM)   |
| 高度可用的可調整多層式架構 (N-Tier) 雲端遊戲，提供更多對作業系統的控制權 (PaaS)        | 遊戲託管於受管理的平台上                | [Azure 雲端服務](https://azure.microsoft.com/services/app-service/) | 為了支援可調整、可靠且運作成本低的應用程式而設計的 PaaS   |
| 提升效能及可用性的跨區域負載平衡 | 路由傳送傳入遊戲要求。 可以當做第一層負載平衡。       | [Azure 流量管理員](https://azure.microsoft.com/en-us/services/traffic-manager/) | 提供多個自動容錯移轉選項，並且有能力依均等方式或加權值分散您的流量。 可以順暢地結合內部部署與雲端系統。 |
| 遊戲資料的雲端儲存體       | 最新的遊戲資料將儲存於雲端中，並會傳送到用戶端裝置 | [Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)| 無限制的可儲存檔案類型。適用於大量未結構化資料 (例如影像、音訊、視訊等等) 的物件儲存體。  |
| 暫時資料儲存體表格| 遊戲交易 (遊戲狀態的變更) 會暫時儲存在表格中 | [Azure 表格儲存體](https://azure.microsoft.com/services/storage/tables/)| 遊戲資料可根據遊戲的需求，以彈性的結構描述進行儲存 |
| 佇列遊戲交易/要求| 遊戲交易會以佇列的形式處理 | [Azure 佇列儲存體](https://azure.microsoft.com/services/storage/queues/)| 佇列可以緩衝未預期的突發流量，並且能防止伺服器在遊戲期間因突然湧入大量要求而造成效能遲緩   |
| 可調整的關聯式遊戲資料庫| 結構化的關聯式資料儲存體，例如傳輸到資料庫中的遊戲內交易 | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)| SQL 資料庫即服務 ([與 VM 上的 SQL 比較](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| 可調整的分散式低延遲遊戲資料庫| 使用彈性結構描述快速讀取、寫入及查詢遊戲與玩家資料 | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| 低延遲 NoSQL 文件資料庫即服務   |
| 使用自己的資料中心搭配 Azure 服務 | 遊戲將擷取自您的資料中心並傳送到用戶端裝置 | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | 可讓您的組織從自己的資料中心傳遞 Azure 服務，以協助完成更多工作  |
| 大型資料區塊傳輸| 大型檔案 (例如遊戲影像、音訊及視訊) 可以透過 Azure CDN 從最近的內容傳遞網路 (CDN) pop 位置傳送給使用者    | [Azure 內容傳遞網路](https://azure.microsoft.com/services/cdn/) | Azure CDN 建置在大型集中式節點的現代化網路拓樸上，能夠處理突發的流量尖峰和大量負載以大幅增加速度與可用性，並產生顯著的使用者體驗改善  |
| 低延遲               | 執行快取以建置快速、可調整的遊戲，並針對隔離資料取得更多的控制和保證，也可以用於改善遊戲的配對功能。 | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | 高輸送量、穩定的低延遲資料存取，可提供快速、可調整的 Azure 應用程式  |
| 高延展性、低延遲 | 以低延遲的讀取和寫入來處理遊戲使用者數量的波動 | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | 可以提供最複雜、低延遲、密集資料的案例及可靠的調整，以同時處理更多的使用者。 針對無狀態 App 的需求，Service Fabric 可讓您在不需要個別建立儲存區或快取的情況下建置遊戲 |
| 能夠每秒從裝置收集數以百萬計事件的能力                         | 能夠每秒從裝置記錄數以百萬計的事件 | [Azure 事件中樞](https://azure.microsoft.com/services/event-hubs/) | 來自遊戲、網站、App 及裝置的雲端級別遙測擷取  |
| 即時處理遊戲資料  | 執行玩家資料的即時分析以改善遊戲| [Azure 串流分析](https://azure.microsoft.com/services/stream-analytics/) | 在雲端即時串流處理  |
| 開發預測式遊戲方式         | 根據玩家資料建立自訂的動態遊戲方式  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | 完全受管理的雲端服務，可讓您輕鬆地建置、部署及共用預測式分析解決方案  |
| 收集與分析遊戲資料| 大量平行處理來自關聯式與非關聯式資料庫的資料 | [Azure 資料倉儲](https://azure.microsoft.com/services/sql-data-warehouse/)| 彈性資料倉儲即服務與企業級功能   |
| 吸引使用者以增加使用量和留客率| 從任何後端傳送目標式推播通知至任何平台，以激發興趣並鼓勵特定遊戲活動 | [Azure 通知中樞](https://azure.microsoft.com/services/notification-hubs/)| 快速廣播推送，使接觸範圍擴及所有主要平台上的數百萬行動裝置 &mdash; iOS、Android、Windows、Kindle、Baidu。 您的遊戲可以裝載於任何後端 &mdash; 雲端或內部部署。|
| 串流媒體內容至您當地與全球的目標對象，同時保護您的內容| 可以從所有裝置上廣播高品質的遊戲預告片及電影短片| [Azure 媒體服務](https://azure.microsoft.com/services/media-services/)| 使用整合式內容傳遞網路功能的隨選及直播視訊串流。 使用一個播放器滿足您所有的播放需求，包括內容保護與加密。| 
| 開發、散發和 Beta 測試您的行動應用程式 | 測試和散發您的行動應用程式。 應用程式效能及使用者體驗管理。 | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| 將當機報告及使用者計量與應用程式散發及使用者意見反應平台整合。 支援 Android、Cordova、iOS、OS X、Unity、Windows 及 Xamarin 應用程式。 此外，請針對應用程式結合豐富多樣分析、當機報告、推播通知、應用程式散發等功能的應用程式考慮 [Visual Studio Mobile Center](https://www.visualstudio.com/vs/mobile-center/) &mdash; 任務控制項。 |
| 建立行銷活動以增加使用量和留客率  | 根據資料分析傳送推播通知給目標玩家，以激發興趣並鼓勵特定遊戲活動 | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) - 將於 2018 年 3 月淘汰，目前僅供現有客戶使用 |  增加所有主要平台 (iOS、Android、Windows、Windows Phone) 上的遊戲時間與使用者留存率 |


##  <a name="startup-and-developer-resources"></a>新創公司與開發人員資源

* [Microsoft for Startups 新創火花計畫](https://startups.microsoft.com)

    Microsoft for Startups 新創火花計畫提供技術以及上市優勢以協助加快新創公司的成長。 其中一個優勢包含 Azure 免費帳戶。 您有 $200 美元額度可探索服務 30 天、12 個月的熱門免費服務，以及始終免費的 25 項服務以上。 如需詳細資訊，請參閱[使用 Azure 免費帳戶將您的新創想法帶入生活中](https://azure.microsoft.com/free/startups/)。
    
* [開發人員計畫](e2e.md#developer-programs)

    Microsoft 提供數個可協助您開發及發行遊戲的開發人員計畫，例如 [ID@Xbox](http://www.xbox.com/Developers/id) 和 [Xbox Live 創作者計畫](https://developer.microsoft.com/games/xbox/xboxlive/creator)。

## <a name="learning-resources"></a>學習資源

* //組建 2016：[CodeLabs &mdash; 使用 Microsoft Azure App Service 與 Microsoft SQL Azure 後端儲存 Unity 中的遊戲分數](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* 組建 2017年:[提供世界級遊戲體驗使用 Microsoft Azure： 從 Halo、 Hitman 和 WalkingDead （影片） 學習的課程](https://channel9.msdn.com/Events/Build/2017/P4062)
* 可重複使用的一組設計要使用 Azure GitHub 來支援一般遊戲工作負載的建置組塊、專案、服務及最佳做法：[Azure 上的遊戲建置組塊](https://github.com/MicrosoftDX/nether)
* [Azure 上的遊戲服務 (影片)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>工具及其他實用連結

* [MSDN 論壇 &mdash; Azure 平台](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [雲端式負載測試工具](https://www.visualstudio.com/team-services/cloud-load-testing/)
* [SDK 和命令列工具](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>適用於遊戲後端的軟體即服務

[Playfab](https://playfab.com/) 目前支援擁有每個月 8 千萬名玩家的 1200 多個及時遊戲。 它是一個包含完整堆疊 LiveOps 和即時控制的後端平台。 

您可以將此解決方案整合到您的行動裝置、電腦或使用 SDK 的主控台遊戲。 適用於所有受歡迎的遊戲引擎和平台 (包括 Android、iOS、Unreal、Unity，以及 Windows) 的 SDK。 若要開始使用，請參閱[文件](https://api.playfab.com/)。

它可提供遊戲服務，例如驗證、玩家資料管理、多人遊戲和即時分析，以協助您的遊戲拓展其使用者數量。 掌握即時資料管線和 LiveOps 的威力，以自訂的遊戲中項目、事件和促銷促進使用者的參與。 您也可以進行 A/B 測試、產生報告、傳送推播通知等等。 

我們會持續創新和增加新的功能。 如需詳細資訊，請參閱[功能](https://playfab.com/features/)，如需得知價格，請參閱[配合您進行調整的簡易定價](https://playfab.com/pricing/)。

## <a name="related-links"></a>相關連結

* [Windows10 遊戲開發指南](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [用於遊戲的 Azure](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft for Startups 新創火花計畫](https://startups.microsoft.com)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 
