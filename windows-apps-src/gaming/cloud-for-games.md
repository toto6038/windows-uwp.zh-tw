---
author: joannaleecy
title: "針對 UWP 遊戲使用雲端服務"
description: "深入了解將雲端實作為 UWP 遊戲的後端。"
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 8bf42e9c2c2e074240eb8e7b94efdfbca65cc7f9

---
#  針對 UWP 遊戲使用雲端服務

Windows 10 中的通用 Windows 平台 (UWP) 提供一組 API，可用來在 Microsoft 裝置上開發遊戲。 開發跨平台與裝置的遊戲時，您可以利用雲端後端來協助根據需求調整遊戲。

##  什麼是雲端運算？

雲端運算透過網際網路使用隨選 IT 資源和應用程式，來為您的裝置儲存和處理資料。 _雲端_一詞是一種隱喻，代表外面 (本機資源之外) 可供使用的大量資源，而您可以從非特定位置存取這些資源。
雲端運算的原則，提供使用資源與軟體的新方式。 使用者不再需要預先支付整套完整的產品或資源，而能以服務的方式使用平台、軟體及資源。 雲端提供者通常會根據客戶的使用量或服務方案供應項目來計費。

##  為什麼要使用雲端服務？

使用適用於遊戲的雲端服務，其中一項優勢是您不需要預先投資實體硬體伺服器，而只要在稍後的階段根據使用量或服務方案支付費用即可。 這是開發新遊戲時，有助於管理所牽涉風險的一種方式。 

另一項優點，則是您的遊戲將可以存取大量的雲端資源以達到延展性 (有效率地管理任何突然增加的最大同時玩家數、密集的即時遊戲運算，或是資料需求)。 這能使您遊戲的效能持續維持穩定。 此外，雲端資源可以由世界各地執行任何平台的任何裝置存取，這代表您可以將遊戲帶給世界上的任何人。

將酷炫的遊戲體驗傳遞給玩家是非常重要的。 因為遊戲伺服器是在雲端中執行並和用戶端更新無關，因此整體而言，它們可以為遊戲提供更受控制且安全的環境。   藉由雲端，您也可以透過一律不信任用戶端並使用伺服器端遊戲邏輯，來達成遊戲方式的一致性。 您也可以設定服務對服務連線，以允許更多整合的遊戲體驗，範例包含將遊戲內購買和多種不同的付款方式連結、引入不同的遊戲網路，以及將遊戲內更新分享到熱門的社交媒體入口網站 (例如 Facebook 和 Twitter)。 

您也可以使用專用的雲端伺服器建立大型的持續性遊戲世界、建立玩家社群、隨時間收集並分析玩家資料以改善遊戲，以及最佳化遊戲的獲利設計模型。

此外，需要密集遊戲資料管理功能的遊戲 (例如具有非同步多人遊戲機制的社交遊戲) 可以使用雲端服務來實作。

##  遊戲公司如何使用雲端技術？

了解其他開發人員如何在遊戲中實作雲端解決方案。

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header">
        <th>開發人員</th>
        <th>描述</th>
        <th>主要遊戲案例</th>
        <th>深入了解</th>
    </tr>
    <tr>
        <td>[343 Industries](https://www.halowaypoint.com/)</td>
        <td>_Halo 5: Guardians_ 使用 Microsoft Azure DocumentDB 將 [Halo: Spartan Companies](https://www.halowaypoint.com/spartan-companies) 實作為其社交遊戲平台，選取此技術是因為其自動索引功能所提供的速度及彈性。</td>
        <td>
            <ul>
                <li>透過可調整資料層來處理多人遊戲的群組建立/管理 <li>遊戲與社交媒體整合 <li>透過多個屬性進行即時資料查詢 <li>遊戲成就與統計資料的同步處理 </ul>
        </td>
        <td>
            <ul>
                <li>[使用 Azure DocumentDB 實作的社交遊戲](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)</td>
            </ul>
    </tr>
    <tr>
        <td>[Illyriad Games](http://web.ageofascent.com/)</td>
        <td>Illyriad Games 建立了 _Age of Ascent_，一款大型多人線上 (MMO) 史詩 3D 太空遊戲，可在具有現代化瀏覽器的裝置上進行。 因此這款遊戲可在電腦、膝上型電腦、手機和其他行動裝置上進行，而不需要外掛程式。 遊戲使用 ASP.NET Core、HTML5、WebGL 及 Microsoft Azure。</td>
        <td>
            <ul>
                <li>跨平台的瀏覽器型遊戲 <li>單一的大型持續性開放世界 <li>處理密集的即時遊戲運算 <li>隨著玩家數目進行調整 </ul>
        </td>
        <td>
            <ul>
                <li>[使用 Azure Service Fabric 將遊戲元件以微服務的形式進行管理 (影片)](https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s)  
                <li>[Age of Ascent 開發人員訪談 (影片)](https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET)
            </ul>
        </td>
    </tr>
    <tr>
        <td>[Next Games](http://www.nextgames.com/)</td>
        <td>Next Games 是 _The Walking Dead: No Man's Land_ 電玩遊戲的製作者，此遊戲是根據 AMC 的原創影集改編。 The Walking Dead 遊戲使用 Azure 做為後端。 它在首個週末便達到 1,000,000 次的下載次數，並在首週便拿下美國 App Store 中 iPhone 與 iPad 免費 App 的第 1 名、12 個國家中免費 App 的第 1 名，以及 13 個國家中免費遊戲的第 1 名。
        </td>
        <td>
            <ul>
                <li>跨平台 <li>回合制多人遊戲 <li>有彈性地調整效能 </ul>
        </td>
        <td>
            <ul>
                <li>[Kalle Hiitola，Next Games 技術總監訪談 (影片)](https://channel9.msdn.com/Blogs/AzureDocumentDB/azure-documentdb-walking-dead)
                <li>[Walking Dead 使用 DocumentDB 以獲得更快的開發週期與更吸引人的遊戲方式](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)
            </ul>
    </tr>
    </td>
        <td>[Pixel Squad](http://www.crimecoast.com/)</td>
        <td>Pixel Squad 使用 Unity 遊戲引擎與 Azure 開發 _Crime Coast_。 _Crime Coast_ 是一款社交策略遊戲，可在 Android、iOS 和 Windows 平台上取得。 他們在遊戲中使用 Azure Blob 儲存體、受管理的 Azure Redis Cache、負載平衡的 IIS VM 陣列，以及 Microsoft 通知中樞。 了解他們如何管理調整，並處理同時湧入的 5000 名玩家。
        </td>
        <td>
            <ul>
                <li>跨平台 <li>多人線上遊戲 <li>隨著玩家數目調整 </ul>
        </td>
        <td>
            <ul>
                <li>[Crime Coast MMO 遊戲使用 Azure 雲端服務的方式](https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te)
            </ul>
        </td>
    </tr> 
</table>

    
### 其他連結

* [Azure 是 Hitcents、Game Troopers 及 InnoSpark 的幕後秘訣](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [使用 Azure 的 Bizspark 計畫遊戲新創公司](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## 如何設計雲端後台

在製作人與遊戲設計人員討論必須包含在遊戲內的特色及功能時，您最好開始考慮要如何設計遊戲的基礎結構。 當您想為不同裝置及不同的主要平台開發遊戲時，可以使用 Azure 雲端做為遊戲的後端。

### 逐步解說學習指南

* [Build 2016 Codelabs：使用 Microsoft Azure App Service 與 Microsoft SQL Azure 後端儲存遊戲分數](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* [設計遊戲的行動吸引力策略](https://azure.microsoft.com/documentation/articles/mobile-engagement-gaming-scenario/)
* [針對 Unity iOS 部署使用 Azure Mobile Engagement](https://azure.microsoft.com/documentation/articles/mobile-engagement-unity-ios-get-started/)

### 了解 IaaS、PaaS 或 SaaS

首先，您需要了解最適合您遊戲的服務層級。 了解以下三種服務的差異，有助於您決定建置後端所要採取的方法。

* [基礎結構即服務 (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    基礎結構即服務 (IaaS) 是即時運算基礎結構，透過網際網路進行佈建及管理。 想像一下能根據需求讓許多電腦隨時快速相應增加和減少的可能性。 IaaS 協助您避免購買和管理自己的實體伺服器及其他資料中心基礎結構的成本和複雜度。

* [平台即服務 (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    平台即服務 (PaaS) 就像 IaaS，不過它也包含基礎結構 (例如伺服器、儲存體和網路) 的管理。 因此在不購買實體伺服器與資料中心基礎結構的基礎上，您也不需要購買和管理軟體授權、基礎的應用程式基礎結構、中介軟體、開發工具或其他資源。

* 軟體即服務 (SaaS)

    軟體即服務通常是已為您建置，並在現有雲端平台上託管的應用程式。 它的設計目的是讓您可以在他們的服務上，更輕鬆地開始執行您的遊戲。


### 使用 Azure 設計遊戲基礎結構

以下是可以針對遊戲使用 Azure 雲端供應項目的一些方法。 Azure 可以搭配 Windows、Linux，以及其他常見的開放原始碼技術 (例如 Ruby、Python、Java 和 PHP) 使用。 如需詳細資訊，請參閱 [Azure 雲端](https://azure.microsoft.com)。

| 需求                 | 活動案例                            | 產品供應項目                      | 產品功能                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| 在雲端託管您的網域     | 有效率地回應 DNS 查詢            | [Azure DNS](https://azure.microsoft.com/services/dns/) | 以具有高效能和可用性的方式託管您的網域  |
| 登入、身分識別驗證      | 已驗證玩家登入和玩家身分識別  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | 使用多重要素驗證來單一登入至任何雲端與內部部署 Web 應用程式            |
| 使用基礎結構即服務 (IaaS) 模型的遊戲      | 遊戲託管於雲端的虛擬機器上       | [Azure VM](https://azure.microsoft.com/services/virtual-machines/) | 做為具有內建虛擬網路和負載平衡的遊戲伺服器，在 1 部到數千部虛擬機器執行個體之間進行調整。搭配混合式一致性的內部部署系統           |
| 使用平台即服務 (PaaS) 模型的網路或行動裝置遊戲            | 遊戲託管於受管理的平台上                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | 適用於網站或行動裝置遊戲的 PaaS (代表具有中介軟體/開發工具/BI/DB 管理的 Azure VM)   |
| 遊戲資料的雲端儲存體       | 最新的遊戲資料將儲存於雲端中，並會傳送到用戶端裝置 | [Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)| 無限制的可儲存檔案類型。適用於大量未結構化資料 (例如影像、音訊、視訊等等) 的物件儲存體。  |
| 暫時資料儲存體表格| 遊戲交易 (遊戲狀態的變更) 會暫時儲存在表格中 | [Azure 表格儲存體](https://azure.microsoft.com/services/storage/tables/)| 遊戲資料可根據遊戲的需求，以彈性的結構描述進行儲存 |
| 佇列遊戲交易/要求| 遊戲交易會以佇列的形式處理 | [Azure 佇列儲存體](https://azure.microsoft.com/services/storage/queues/)| 佇列可以緩衝未預期的突發流量，並且能防止伺服器在遊戲期間因突然湧入大量要求而造成效能遲緩   |
| 可調整的關聯式遊戲資料庫| 結構化的關聯式資料儲存體，例如傳輸到資料庫中的遊戲內交易 | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)| SQL 資料庫即服務 ([與 VM 上的 SQL 比較](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| 可調整的分散式低延遲遊戲資料庫| 使用彈性結構描述快速讀取、寫入及查詢遊戲與玩家資料 | [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)| 低延遲 NoSQL 文件資料庫即服務   |
| 使用自己的資料中心搭配 Azure 服務 | 遊戲將擷取自您的資料中心並傳送到用戶端裝置 | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | 可讓您的組織從自己的資料中心傳遞 Azure 服務，以協助完成更多工作  |
| 大型資料區塊傳輸| 大型檔案 (例如遊戲影像、音訊及視訊檔案) 可以透過 Azure CDN 從最近的內容傳遞網路 (CDN) pop 位置傳送給使用者  | [Azure 內容傳遞網路](https://azure.microsoft.com/services/cdn/) | Azure CDN 建置在大型集中式節點的現代化網路拓樸上，能夠處理突發的流量尖峰和大量負載以大幅增加速度與可用性，並產生顯著的使用者體驗改善  |
| 低延遲               | 執行快取以建置快速、可調整的遊戲，並針對隔離資料取得更多的控制和保證，也可以用於改善遊戲的配對功能。 | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | 高輸送量、穩定的低延遲資料存取，可提供快速、可調整的 Azure 應用程式  |
| 高延展性、低延遲 | 以低延遲的讀取和寫入來處理遊戲使用者數量的波動 | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | 可以提供最複雜、低延遲、密集資料的案例及可靠的調整，以同時處理更多的使用者。 針對無狀態 App 的需求，Service Fabric 可讓您在不需要個別建立儲存區或快取的情況下建置遊戲 |
| 能夠每秒從裝置收集數以百萬計事件的能力                         | 能夠每秒從裝置記錄數以百萬計的事件 | [Azure 事件中樞](https://azure.microsoft.com/services/event-hubs/) | 來自遊戲、網站、App 及裝置的雲端級別遙測擷取  |
| 即時處理遊戲資料  | 執行玩家資料的即時分析以改善遊戲| [Azure 串流分析](https://azure.microsoft.com/services/stream-analytics/) | 在雲端即時串流處理  |
| 開發預測式遊戲方式         | 根據玩家資料建立自訂的動態遊戲方式  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | 完全受管理的雲端服務，可讓您輕鬆地建置、部署及共用預測式分析解決方案  |
| 收集與分析遊戲資料| 大量平行處理來自關聯式與非關聯式資料庫的資料 | [Azure 資料倉儲](https://azure.microsoft.com/services/sql-data-warehouse/)| 彈性資料倉儲即服務與企業級功能   |
| 建立行銷活動以增加使用量與留存率  | 根據資料分析傳送推播通知給目標玩家，以產生興趣並鼓勵特定遊戲活動 | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) |  增加所有主要平台 (iOS、Android、Windows、Windows Phone) 上的遊戲時間與使用者留存率 |


##  新創公司與開發人員資源

* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)

    Microsoft BizSpark 是一項全球性計畫，提供 Azure 雲端服務、軟體及支援的免費存取，以協助新創公司成功。 BizSpark 成員會收到五個 Visual Studio Enterprise 含 MSDN 的訂用帳戶，每一個都含有每月 150 美元的 Azure 點數。 五位開發人員每月合計將會有 750 美元可以花費在 Azure 服務上。 BizSpark 適用於私人所有、成立於 5 年內、年度營收低於 1 百萬美金的新創公司。 Microsoft 相信藉由協助新創公司取得成功，將有助於建立重要的長期合作關係。
    
* [ID@Xbox](http://www.xbox.com/Developers/id)

    如果您想要將 Xbox Live 的功能，例如多人遊戲、跨平台配對、玩家分數、成就及排行榜加入到您的 Windows 10 遊戲，請向 ID@Xbox 註冊以取得所需之工具和支援，以盡情發揮您的創意並取得最大的成功。 向 ID@Xbox 提出申請之前，請先在 [Windows 開發人員中心](https://developer.microsoft.com/windows/programs/join)註冊一個開發人員帳戶。

## 適用於遊戲後端的軟體即服務

以下公司能為基於主要雲端服務提供者的遊戲提供雲端後端，好讓您能夠將焦點放在開發您的遊戲上。

* [GameSparks](http://www.gamesparks.com/)

    GameSparks 是一個適合遊戲開發人員的雲端開發平台，並能讓他們建置遊戲伺服器端的所有內容

* [Photon Engine](https://www.photonengine.com/en/Photon)

    Photon 是適用於遊戲的獨立網路引擎與多人平台。 它提供 Photon Cloud，這是一個可以提供完全受管理的軟體即服務 (SaaS) 服務。 您可以在託管期間完全專注在應用程式用戶端上，伺服器作業及調整會完全由 Exit Games 處理。

* [Playfab](https://playfab.com/)

    Playfab 能夠簡單又快速地為您的行動裝置、電腦或主機遊戲帶來世界級的即時遊戲管理和後端技術。

## 相關連結

* [Windows 10 遊戲開發指南](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Microsoft Azure](https://azure.microsoft.com/)
* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 



<!--HONumber=Jul16_HO2-->


