---
author: joannaleecy
title: Windows10 遊戲開發指南
description: 開發「通用 Windows 平台」(UWP) 遊戲的資源與資訊的端對端指南。
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.author: joanlee
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 遊戲開發
ms.localizationpriority: medium
ms.openlocfilehash: 7481c1d0f64ccb25168200cdf5e6ccc068f769b9
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7418487"
---
# <a name="windows-10-game-development-guide"></a>Windows10 遊戲開發指南


歡迎使用 Windows10 遊戲開發指南！

本指南說明有關您開發通用 Windows 平台 (UWP) 遊戲所需的端對端資源與資訊集合。 本指南有 [PDF](http://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf) 格式的英文 (美國) 版本。

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP) 的遊戲開發簡介


當您建立 Windows10 遊戲時，便擁有一個可與全球數百萬個遍及手機、電腦及 Xbox One 玩家接觸的機會。 有了 Windows 上的 Xbox、Xbox Live、跨裝置多人遊戲、優質的遊戲社群，以及「通用 Windows 平台」(UWP) 和 DirectX 12 等威力強大的新功能，Windows10 遊戲讓所有年齡及性別的玩家都興奮不已。 新的「通用 Windows 平台」(UWP) 提供用於手機、電腦及 Xbox One 的通用 API，以及可針對各種裝置體驗量身打造您遊戲的工具與選項，為您的遊戲帶來跨 Windows10 裝置的相容性。

本指南提供可在您開發遊戲的過程中協助您的端對端資源與資訊集合。 各個小節的編排是根據遊戲的開發階段，因此您會知道要從何處尋找所需的資訊。

如果您是開發 Windows 或 Xbox 遊戲的新手，[開始使用](getting-started.md)指南可能是您可以開始的地方。 [遊戲開發資源](#game-development-resources)一節也提供文件、程式及其他在建立遊戲時相當實用之資源的概略綜覽。 如果您想要改為從一些 UWP 程式碼開始，請參閱[遊戲範例](#game-samples)。

本指南將會隨著其他 Windows10 遊戲開發資源和資料的推出進行更新。

## <a name="game-development-resources"></a>遊戲開發資源

從文件到開發人員計劃、論壇、部落格及範例，有許多資源可在您的遊戲開發之路上協助您。 以下是您開發 Windows10 遊戲時需了解的資源摘要報導。

> [!Note]
> 某些功能可透過各種程式管理。 本指南涵蓋的資源範圍很廣，因此視您所參與的計畫或您的特定開發角色而定，您可能會發現有些資源無法使用。 解析成 developer.xboxlive.com、forums.xboxlive.com、xdi.xboxlive.com 或「遊戲開發人員網路」(GDN) 的連結就是其中幾例。 如需有關與 Microsoft 合作的資訊，請參閱[開發人員計畫](#developer-programs)。


### <a name="game-development-documentation"></a>遊戲開發文件

在這整份指南中，您會發現相關文件的深層連結依工作、技術及遊戲開發階段編排。 為了讓您能夠完全檢視有哪些可用的文件，以下是 Windows10 遊戲開發的主要文件入口網站。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 開發人員中心主要入口網站</td>
        <td><a href="https://dev.windows.com">Windows 開發人員中心</a></td>
    </tr>
    <tr>
        <td>開發 Windows 應用程式</td>
        <td><a href="https://dev.windows.com/develop">開發 Windows 應用程式</a></td>
    </tr>
    <tr>
        <td>通用 Windows 平台 app 開發</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt244352">Windows10 應用程式使用方法指南</a></td>
    </tr>
    <tr>
        <td>UWP 遊戲使用方法指南</td>
        <td><a href="index.md">遊戲和 DirectX</a> </td>
    </tr>
    <tr>
        <td>DirectX 參考與概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX 圖形與遊戲</a></td>
    </tr>
    <tr>
        <td>用於遊戲的 Azure</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">使用 Azure 建置及調整您的遊戲</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">即時遊戲的完整後端解決方案</a></td>
    </tr>
    <tr>
        <td>Xbox One 上的 UWP</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/xbox-apps/index">在 Xbox One 上建置 UWP App</a></td>
    </tr>
    <tr>
        <td>HoloLens 上的 UWP</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">在 HoloLens 上建置 UWP App</a></td>
    </tr>
    <tr>
        <td>Xbox Live 文件</td>
        <td><a href="../xbox-live/index.md">Xbox Live 開發人員指南</a></td>
    </tr>
    <tr>
        <td>Xbox One 開發文件 (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Xbox One 開發</a></td>
    </tr>
    <tr>
        <td>Xbox One 開發白皮書 (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">白皮書</a></td>
    </tr>
    <tr>
        <td>Mixer Interactive 文件</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">為遊戲新增互動性</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>合作夥伴中心

[註冊在合作夥伴中心開發人員帳戶](https://developer.microsoft.com/store/register)是邁向發行 Windows 遊戲的第一個步驟。 開發人員帳戶可讓您保留您遊戲的名稱，以及將適用於所有 Windows 裝置的免費或付費遊戲提交到 Microsoft Store。 您可以使用開發人員帳戶來管理您的遊戲與遊戲內產品、取得詳細的分析，以及啟用可為您的全球玩家創造絕佳體驗的服務。 

Microsoft 也提供數個可協助您開發及發行 Windows 遊戲的開發人員計畫。 我們建議您查看如果有任何適合您之前先註冊合作夥伴中心帳戶。 如需詳細資訊，請移至[開發人員計畫](#developer-programs)


### <a name="developer-programs"></a>開發人員計畫

Microsoft 提供數個可協助您開發及發行 Windows 遊戲的開發人員計畫。 如果您想要為 Xbox One 開發遊戲，並將 Xbox Live 功能整合到遊戲中，請考慮加入開發人員計畫。 若要在 Microsoft Store 中發佈遊戲，您將也需要在[合作夥伴中心](https://partner.microsoft.com/dashboard)建立開發人員帳戶。

#### <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

Xbox Live 創作者計畫允許任何人將 Xbox Live 整合至其遊戲中，並發佈到 Xbox One 和 Windows 10。 簡化的認證處理程序，而且在標準 [Microsoft Store 原則](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx) 以外不需要核准概念。

您可以只使用零售硬體在創作者計畫中部署、設計和發佈您的遊戲，不需要專用的開發套件。 若要開始使用，請在您的 Xbox 在One 上下載[啟用開發人員模式 App](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation)。

如果您想要存取其他 Xbox Live 功能、專用的行銷和開發支援，以及有機會獲得主要 Xbox One 市集的推薦，您可以申請加入 [ID@Xbox](http://www.xbox.com/Developers/id) 計畫。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live 創作者計畫</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">深入了解 Xbox Live 創作者計畫</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

ID@Xbox 計畫可協助合格的遊戲開發人員在 Windows 和 Xbox One 上自行發行遊戲。 如果您想要為 Xbox One 開發遊戲，或是在您的 Windows10 遊戲中新增 Xbox Live 功能 (例如玩家分數、成就及排行榜)，請向 ID@Xbox 註冊。 成為 ID@Xbox 開發人員以取得所需的工具與支援，讓您可以充分發揮您的創意並獲得最大的成功。 我們建議您將套用到ID@Xbox第一次，再註冊開發人員帳戶在合作夥伴中心。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox 開發人員計畫</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkID=526271">適用於 Xbox One 的獨立開發人員計畫</a></td>
    </tr>
    <tr>
        <td>ID@Xbox 消費者網站</td>
        <td><a href="http://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Xbox 工具與中介軟體

「Xbox 工具與中介軟體計畫」會將 Xbox 開發套件授權給遊戲工具與中介軟體的專業開發人員。 接受加入計畫的開發人員可以將他們的 Xbox XDK 技術分享及散佈給其他獲授權的 Xbox 開發人員。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>洽談工具與中介軟體計畫</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>


### <a name="game-samples"></a>遊戲範例

有許多 Windows10 遊戲與應用程式範例可協助您了解 Windows10 遊戲功能，並在遊戲開發上快速入門。 將會有更多範例以定期方式開發及發行，因此請別忘了偶爾回到範例入口網站上查看有什麼最新內容。 您也可以[查看](https://help.github.com/articles/watching-repositories/) GitHub 存放庫以獲知變更和新內容。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台 app 範例</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-universal-samples</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 圖形範例</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 圖形範例</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 第一人稱遊戲範例</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">使用 DirectX 建立簡單的 UWP 遊戲</a></td>
    </tr>
    <tr>
        <td>Direct2D 自訂影像效果範例</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620531">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Direct2D 漸層網格範例</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620532">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Direct2D 相片調整範例</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620533">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Xbox Advanced Technology Group 公開範例</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Xbox Live 範例</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">xbox-live-samples</a></td>
    </tr>
    <tr>
        <td>Xbox One 遊戲範例 (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">範例</a></td>
    </tr>
    <tr>
        <td>Windows 遊戲範例 (MSDN Code Gallery)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Microsoft Store 遊戲範例</a></td>
    </tr>
    <tr>
        <td>JavaScript 2D 遊戲範例</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">使用 JavaScript 建立 UWP 遊戲</a></td>
    </tr>
    <tr>
        <td>JavaScript 3D 遊戲範例</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">使用 three.js 建立 3D JavaScript 遊戲</a></td>
    </tr>
    <tr>
        <td>MonoGame 2D UWP 遊戲範例</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">使用 MonoGame 2D 建立 UWP 遊戲</a></td>
    </tr>      
</table>


### <a name="developer-forums"></a>開發人員論壇

開發人員論壇是一個詢問和回答遊戲開發問題，以及與遊戲開發社群連結的絕佳場所。 論壇也是極佳的資源，可供尋找開發人員過去已面對並解決之困難問題的現有解答。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>發佈應用程式和遊戲的開發人員論壇</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">發佈與應用程式廣告</a></td>
    </tr>
    <tr>
        <td>UWP app 開發人員論壇</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">開發通用 Windows 平台 App</a></td>
    </tr>
    <tr>
        <td>傳統型應用程式開發人員論壇</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Windows 傳統型應用程式論壇</a></td>
    </tr>
    <tr>
        <td>DirectX Microsoft Store 遊戲 (已封存的論壇文章)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">使用 DirectX 建置 Microsoft Store 遊戲 (已封存)</a></td>
    </tr>
    <tr>
        <td>Windows10 受管理的合作夥伴開發人員論壇</td>
        <td><a href="http://aka.ms/win10devforums">XBOX 開發人員論壇：Windows10</a></td>
    </tr>
    <tr>
        <td>DirectX 論壇</td>
        <td><a href="http://forums.directxtech.com/index.php">DirectX 12 論壇</a></td>
    </tr>
    <tr>
        <td>Azure 平台論壇</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Azure 論壇</a></td>
    </tr>
    <tr>
        <td>Xbox Live 論壇</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Xbox Live 開發論壇</a></td>
    </tr>
    <tr>
        <td>PlayFab 論壇</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab 論壇</a></td>
    </tr>
</table>


### <a name="developer-blogs"></a>開發人員部落格

開發人員部落格是另一個提供最新遊戲開發相關資訊的絕佳資源。 您將可找到有關新功能、實作詳細資料、最佳做法、架構背景等等的貼文。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>建置適用於 Windows 的應用程式部落格</td>
        <td><a href="http://blogs.windows.com/buildingapps/">建置適用於 Windows 的應用程式</a></td>
    </tr>
    <tr>
        <td>Windows10 部落格 (部落格文章)</td>
        <td><a href="http://blogs.windows.com/blog/tag/windows-10/">Windows10 文章</a></td>
    </tr>
    <tr>
        <td>Visual Studio 工程小組部落格</td>
        <td><a href="http://blogs.msdn.com/b/visualstudio/">Visual Studio 部落格</a></td>
    </tr>
    <tr>
        <td>Visual Studio 開發人員工具部落格</td>
        <td><a href="http://blogs.msdn.com/b/developer-tools/">開發人員工具部落格</a></td>
    </tr>
    <tr>
        <td>Somasegar 開發人員工具部落格</td>
        <td><a href="http://blogs.msdn.com/b/somasegar/">Somasegar 部落格</a></td>
    </tr>
    <tr>
        <td>DirectX 開發人員部落格</td>
        <td><a href="http://blogs.msdn.com/b/directx">DirectX 開發人員部落格</a></td>
    </tr>
    <tr>
        <td>DirectX 12 簡介 (部落格文章)</td>
        <td><a href="http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Visual C++ 工具小組部落格</td>
        <td><a href="http://blogs.msdn.com/b/vcblog/">Visual C++ 小組部落格</a></td>
    </tr>
    <tr>
        <td>PIX 小組部落格</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/">在 Windows 與 Xbox 上調整與偵錯 DirectX 12 的效能</a></td>
    </tr>
    <tr>
        <td>通用 Windows 應用程式部署團隊部落格</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">建置及部署 UWP 應用程式團隊部落格</a></td>
    </tr>
</table>
 

## <a name="concept-and-planning"></a>概念與規劃


在概念與規劃階段，您將決定您遊戲的未來面貌，以及要用來實現遊戲的技術與工具。

### <a name="overview-of-game-development-technologies"></a>遊戲開發技術指南概觀

開始進行 UWP 遊戲開發時，會有多個圖形、輸入、音訊、網路、公用程式及程式庫選項可供您選擇。

如果您已經決定好所有將在遊戲中使用的技術，那真是好極了！ 如果沒有，則[適用於 UWP app 的遊戲技術](game-development-platform-guide.md)指南提供許多可用技術的絕佳概觀， 強烈建議您閱讀這份指南以協助您了解這些選項及如何將它們搭配使用。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 遊戲技術綜覽</td>
        <td><a href="game-development-platform-guide.md">適用於 UWP app 的遊戲技術</a></td>
    </tr>
</table>
 

這三個 GDC 2015 影片提供 Windows10 遊戲開發與 Windows10 遊戲體驗的絕佳概觀。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows10 遊戲開發概觀 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">開發適用於 Windows10 的遊戲</a></td>
    </tr>
    <tr>
        <td>Windows10 遊戲體驗 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Windows10 上的遊戲消費者體驗</a></td>
    </tr>
    <tr>
        <td>遊戲在整個 Microsoft 生態系統的發展 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">遊戲在 Microsoft 生態圈的未來</a></td>
    </tr>
</table>

### <a name="game-planning"></a>遊戲規劃

以下是在規劃遊戲時可考量的高層級概念和規劃主題。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>設計無障礙遊戲</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games">遊戲的協助工具</a></td>
    </tr>
    <tr>
        <td>使用雲端建置遊戲</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games">遊戲的雲端</a></td>
    </tr>
    <tr>
        <td>利用遊戲獲利</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games">遊戲的獲利</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>選擇您的圖形技術與程式設計語言

有數種程式設計語言與圖形技術可在 Windows10 遊戲中使用。 您可以依據您正在開發的遊戲類型、開發工作室的經驗與喜好，以及遊戲的特定功能需求來選擇。 您將會使用 C#、C++ 或 JavaScript？ DirectX、XAML 或 HTML5？

#### <a name="directx"></a>DirectX

Microsoft DirectX 是適用於高效能 2D 與 3D 圖形與多媒體的選擇。

DirectX 12 比任何之前的版本都更快速且更有效率。 Direct3D 12 可提供更豐富的場景、更多物件、更複雜得效果，以及在 Windows 10 電腦和 Xbox One 上充分運用現代的 GPU 硬體。

如果您想要使用熟悉的 Direct3D 11 圖形管線，您仍然可以從 Direct3D 11.3 中加入的新轉譯與最佳化功能中獲益。 而且如果您正嘗試且確實是使用 Win32 的傳統型 Windows API 開發人員，您仍然可以在 Windows10 中使用該選項。

DirectX 的廣泛功能與深度的平台整合可為要求最嚴苛的遊戲提供所需的強大功能與效能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 開發適用的 DirectX</td>
        <td><a href="directx-programming.md">DirectX 程式設計</a></td>
    </tr>
    <tr>
        <td>教學課程：如何建立 UWP DirectX 遊戲</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">使用 DirectX 建立簡單的 UWP 遊戲</a></td>
    </tr>
    <tr>
        <td>DirectX 概觀與參考</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX 圖形與遊戲</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 程式設計指南與參考</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D 12 圖形</a></td>
    </tr>
    <tr>
        <td>圖形與 DirectX 12 開發影片 (YouTube 頻道)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 與圖形教育訓練</a></td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

XAML 是一種容易使用的宣告式 UI 語言，擁有便利的功能，例如動畫、腳本、資料繫結、可調整的向量式圖形、動態調整大小及場景圖形。 XAML 非常適用於遊戲 UI、功能表、精靈及 2D 圖形。 為了簡化 UI 版面配置，XAML 與設計及開發工具相容，例如 Expression Blend 與 Microsoft Visual Studio。 XAML 經常與 C# 搭配使用，但是如果您偏好使用 C++ 語言，或是您的遊戲對 CPU 的要求較高，那麼 C++ 也是一個很好的選擇。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML 平台概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228259">XAML 平台</a></td>
    </tr>
    <tr>
        <td>XAML UI 與控制項</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228348">控制項、版面配置及文字</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML 5

超文字標記語言 (HTML) 是用於網頁、應用程式及豐富型用戶端的常用 UI 標記語言。 Windows 遊戲可以使用 HTML5 做為具備 HTML 熟悉功能、可存取 Universal Windows Platform，以及支援現代化 Web 功能 (例如 AppCache、Web 工作者、畫布、拖放功能、非同步程式設計及 SVG) 的完整功能展示層。 HTML 轉譯可在幕後利用 DirectX 硬體加速能力，因此您仍然可以享受到 DirectX 的效能優點，不需要撰寫任何額外的程式碼。 如果您精通 Web 開發、移植 Web 遊戲，或想要使用語言和圖形層，HTML5 是一個很好的選擇，比其他選擇更容易讓您完成工作。 HTML5 是與 JavaScript 搭配使用，但也可以呼叫使用 C# 或 C++/CX 建立的元件。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HTML5 與文件物件模型資訊</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/br212882.aspx">HTML 與 DOM 參考</a></td>
    </tr>
    <tr>
        <td>HTML5 W3C 建議</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=221374">HTML5</a></td>
    </tr>
</table>
 

#### <a name="combining-presentation-technologies"></a>結合呈現技術

Microsoft DirectX Graphics Infrastructure (DXGI) 可提供跨多種圖形技術的互通性與相容性。 針對高效能圖形，您可以結合 XAML 與 DirectX，將 XAML 用於功能表與其他簡單的 UI，然後將 DirectX 用於轉譯複雜的 2D 與 3D 場景。 DXGI 也提供 Direct2D、Direct3D、DirectWrite、DirectCompute 及 Microsoft Media Foundation 之間的相容性。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX Graphics Infrastructure 程式設計指南與參考</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh404534">DXGI</a></td>
    </tr>
    <tr>
        <td>結合 DirectX 與 XAML</td>
        <td><a href="directx-and-xaml-interop.md">DirectX 與 XAML 互通性</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX 是一種高效能、低額外負荷的語言，可提供結合速度、相容性及平台存取的強大效能。 C++/CX 可讓您很容易地使用 Windows10 中所有優越的遊戲功能，包括 DirectX 與 Xbox Live。 您也可以重複使用現有的 C++ 程式碼與程式庫。 C++/CX 可建立快速、原生，且不會因收集廢棄項目而產生額外負荷的程式碼，所以您的城市可以擁有優越的效能與低耗電量，進而延長電池使用時間。 請將 C++/CX 與 DirectX 或 XAML 搭配使用，或建立將兩者結合使用的遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C++/CX 參考與概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh699871.aspx">Visual C++ 語言參考 (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++ 程式設計指南與參考</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual Studio 2017 中的 Visual C++</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C#

C# (發音為 "C sharp") 是一種簡單、強大、型別安全且物件導向的新式、創新語言。 C# 可讓您快速進行開發，同時保有 C 式語言的熟悉性與運算式表示方式。 C# 雖然容易使用，但它也有許多先進的語言功能，例如多型、委派、關閉、Iterator 方法、共變數，及 Language-Integrated Query (LINQ) 運算式。 如果您的目標是 XAML、想要快速開始開發您的遊戲，或之前有使用過 C# 的經驗，C# 就會是您的最佳選擇。 C# 主要是與 XAML 搭配使用，因此如果您想要使用 DirectX，請改為選擇 C++，或將遊戲的部分內容撰寫成與 DirectX 互動的 C++ 元件。 或是考慮使用 [Win2D](https://github.com/Microsoft/Win2D)，這是一種適用於 C# 與 C++ 的直接模式 Direct2D 圖形程式庫。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# 程式設計指南與參考</td>
        <td><a href="https://msdn.microsoft.com/library/kx37x362.aspx">C# 語言參考</a></td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

JavaScript 是一種動態指令碼語言，廣泛地使用在現代化網站與豐富型用戶端應用程式中。

Windows JavaScript 應用程式可以透過簡單且直覺化的方式 (以物件導向之 JavaScript 類別的方法與屬性) 存取 Universal Windows Platform 的強大功能。 如果您是在 Web 開發環境中，且已經很熟悉 JavaScript，或想要使用 HTML5、CSS、WinJS 或 JavaScript 程式庫，JavaScript 是您的遊戲適用的好選擇。 如果您的目標是 DirectX 或 XAML，請改為選擇 C# 或 C++/CX。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>JavaScript 與 Windows 執行階段參考</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj613794">JavaScript 參考</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>使用 Windows 執行階段元件來結合語言

有了「通用 Windows 平台」，結合以不同語言撰寫的元件就變得相當簡單。 使用 C++、C# 或 Visual Basic 建立 Windows 執行階段元件，然後從 JavaScript、C#、C++ 或 Visual Basic 使用那些元件。 這是以您選擇的語言為遊戲的某些部分撰寫程式碼的絕佳方式。 元件也可以讓您取用只以特定語言提供的外部程式庫，以及使用您已經撰寫完成的舊有程式碼。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>如何建立 Windows 執行階段元件</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">建立 Windows 執行階段元件</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>您的遊戲應該使用哪一個版本的 DirectX？

如果您要為您的遊戲選擇 DirectX，您將需要決定要使用哪一個版本： Microsoft Direct3D12 或 Microsoft Direct3D11。

DirectX 12 比任何之前的版本都更快速且更有效率。 Direct3D 12 可提供更豐富的場景、更多物件、更複雜得效果，以及在 Windows 10 電腦和 Xbox One 上充分運用現代的 GPU 硬體。 由於 Direct3D 12 的運作層級非常低，因此它可以提供專業的圖形開發團隊或有經驗的 DirectX 11 開發團隊所有所需的控制，以最大化圖形最佳化。

Direct3D 11.3 是低層級圖形 API，使用常見的 Direct3D 程式設計模型，並且可以為您處理更多 GPU 轉譯所牽涉到的複雜度。 Windows10 與 Xbox One 也支援它。 如果您有以 Direct3D 11 撰寫的現有引擎，並且尚未準備好改用 Direct3D 12，則您可以在 Direct3D 12 上使用 Direct3D 11 來達到一些效能改進。 11.3 以上的版本包含新的轉譯與最佳化功能，這些功能也可以在 Direct3D 12 中啟用。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>選擇 Direct3D12 或 Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899228">什麼是 Direct3D12？</a></td>
    </tr>
    <tr>
        <td>Direct3D11 的概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D 11 圖形</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 on 12 概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn913195">Direct3D 11 on 12</a></td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>橋接、遊戲引擎及中介軟體

視您遊戲的需求而定，使用橋接、遊戲引擎或中介軟體可以節省開發與測試時間及資源。 以下是橋接、遊戲引擎及中介軟體的一些概觀與資源。

#### <a name="universal-windows-platform-bridges"></a>通用 Windows 平台橋接

「通用 Windows 平台橋接」是可將您現有的應用程式或遊戲轉移到 UWP 的技術。 橋接是快速展開 UWP 遊戲開發的絕佳方式。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 橋接</td>
        <td><a href="https://dev.windows.com/bridges/">將您的程式碼轉移到 Windows</a></td>
    </tr>
    <tr>
        <td>適用於 iOS 的 Windows 橋接器</td>
        <td><a href="https://dev.windows.com/bridges/ios">將您的 iOS 應用程式轉移到 Windows</a></td>
    </tr>
    <tr>
        <td>適用於傳統型應用程式 (.NET 和 Win32) 的 Windows 橋接器</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">將您的傳統型應用程式轉換成 UWP app</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

PlayFab 現在是 Microsoft 家庭成員，它是直播遊戲的完整後端平台，也是獨立遊戲工作室創業的強大工具。 透過遊戲服務、即時分析與 LiveOps 增加收益、參與度和客戶保留度，同時縮減成本。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">工具和服務的概觀</a></td>
    </tr>
    <tr>
        <td>開始使用</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">開始使用指南</a></td>
    </tr>
    <tr>
        <td>影片教學課程系列</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">有關 PlayFab 的核心系統的系列示範影片</a></td>
    </tr>
    <tr>
        <td>做法</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">熱門遊戲機制與設計模式範例</a></td>
    </tr>
    <tr>
        <td>平台</td>
        <td><a href="https://api.playfab.com/platforms">各種不同平台和遊戲引擎的特定文件</a></td>
    </tr>
    <tr>
        <td>GitHub 存放庫</td>
        <td><a href="https://github.com/PlayFab">取得各種不同平台 (包括 Android、iOS，Windows、Unity 以及 Unreal) 的指令碼和 SDK。</a></td>
    </tr>
    <tr>
        <td>API 文件</td>
        <td><a href="https://api.playfab.com/documentation/">透過類似 REST 的 Web API 直接存取 PlayFab 服務</a></td>
    </tr>
    <tr>
        <td>論壇</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab 論壇</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity 提供一個平台建立美麗而吸引人的 2D、3D、VR 和 AR 遊戲及 App。 它可讓您快速實現您的創意，並傳遞內容到幾乎任何媒體或裝置。

從 Unity 5.4 開始，Unity 支援 Direct3D 12 開發。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unity 遊戲引擎</td>
        <td><a href="http://unity3d.com/">Unity - 遊戲引擎</a></td>
    </tr>
    <tr>
        <td>取得 Unity</td>
        <td><a href="http://unity3d.com/get-unity">取得 Unity</a></td>
    </tr>
    <tr>
        <td>適用於 Windows 的 Unity 文件</td>
        <td><a href="http://docs.unity3d.com/Manual/Windows.html">Unity 手冊 / Windows</a></td>
    </tr>
    <tr>
        <td>使用 PlayFab 新增 LiveOps</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">開始使用 - 從您的 Unity 遊戲第一次呼叫 PlayFab API</a></td>
    </tr>
    <tr>
        <td>如何使用 Mixer Interactive 為您的遊戲新增互動性</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">入門指南</a></td>
    </tr>
    <tr>
        <td>Mixer SDK for Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Mixer Unity 增益集</a></td>
    </tr>
    <tr>
        <td>Mixer SDK for Unity 參考文件</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Mixer Unity 外掛程式 API 參照</a></td>
    </tr>
    <tr>
        <td>將您的 Unity 遊戲發行至 Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">移植指南</a></td>
    </tr>
    <tr>
        <td>疑難排解有關 .NET API 遺失組件參考</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Unity 和 UWP 中遺失 .NET API</a></td>
    </tr>
    <tr>
        <td>將您的 Unity 遊戲發行為通用 Windows 平台 app (影片)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">如何將您的 Unity 遊戲發行為 UWP app</a></td>
    </tr>
    <tr>
        <td>使用 Unity 建置 Windows 遊戲與應用程式 (影片)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">使用 Unity 建置 Windows 遊戲與應用程式</a></td>
    </tr>
    <tr>
        <td>使用 Visual Studio (影片系列) 開發 Unity 遊戲</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=722359">搭配 Visual Studio 2015 使用 Unity</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

Havok 的工具與技術模組套件可協助遊戲建立者達到新的互動與沉浸式情境層級。 Havok 能夠實現極逼真的物理效果、互動式模擬及令人驚豔的場景。 2015.1 版和更新版本正式在 x86、64 位元和 ARM 的 Visual Studio 2015 中支援 UWP。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok 網站</td>
        <td><a href="http://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Havok 工具套件</td>
        <td><a href="http://www.havok.com/products/">Havok 產品概觀</a></td>
    </tr>
    <tr>
        <td>Havok 支援論壇</td>
        <td><a href="http://support.havok.com">Havok</a></td>
    </tr>
</table>
 

#### <a name="monogame"></a>MonoGame

MonoGame 是開放原始碼的跨平台遊戲開發架構，最初是以 Microsoft 的 XNA Framework 4.0 為基礎。 Monogame 目前支援 Windows、Windows Phone 和 Xbox，以及 Linux、macOS、iOS、Android 與數種其他平台。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="http://www.monogame.net">MonoGame 網站</a></td>
    </tr>
    <tr>
        <td>MonoGame 文件</td>
        <td><a href="http://www.monogame.net/documentation/">MonoGame 文件 (最新)</a></td>
    </tr>
    <tr>
        <td>Monogame 的下載項目</td>
        <td><a href="http://www.monogame.net/downloads/">從 MonoGame 網站下載發行版、開發版組建和原始程式碼</a>，或透過 <a href="https://www.nuget.org/profiles/MonoGame">NuGet 取得最新發行的版本</a>。
    </tr>
    <tr>
        <td>MonoGame 2D UWP 遊戲範例</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">使用 MonoGame 2D 建立 UWP 遊戲</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x 是一個跨平台的開放原始碼遊戲開發引擎與工具套件，可支援建置 UWP 遊戲。 從第 3 版開始，也新增了 3D 功能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/">什麼是 Cocos2d-x？</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x 程式設計人員指南</td>
        <td><a href="http://www.cocos2d-x.org/programmersguide/">Cocos2d-x 程式設計人員指南</a></td>
    </tr>
    <tr>
        <td>Windows10 上的 Cocos2d-x (部落格文章)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">在 Windows10 上執行 Cocos2d-x</a></td>
    </tr>
    <tr>
        <td>使用 PlayFab 新增 LiveOps</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">開始使用 - 從您的 Cocos2d 遊戲第一次呼叫 PlayFab API</a></td>
    </tr>
</table>


#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4 是一整套的遊戲開發工具，適合所有類型的遊戲與開發人員採用。 全球的遊戲開發人員都使用 Unreal Engine 來處理對效能要求最高的遊戲主機與電腦遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unreal Engine 概觀</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine 4</a></td>
    </tr>
    <tr>
        <td>使用 PlayFab 新增 LiveOps - C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">開始使用 - 從您的 Unreal 遊戲第一次呼叫 PlayFab API</a></td>
    </tr>
    <tr>
        <td>使用 PlayFab 新增 LiveOps - Blueprints</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">開始使用 - 從您的 Unreal 遊戲第一次呼叫 PlayFab API</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS 是完整的 JavaScript 架構，可用於搭配 HTML5、WebGL、WebVR 及 Web Audio 建置 3D 遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="http://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>WebGL 3D 搭配 HTML5 和 BabylonJS (系列影片)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">學習 WebGL 3D 和 BabylonJS</a></td>
    </tr>
    <tr>
        <td>使用 BabylonJS 建置跨平台 WebGL 遊戲</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">使用 BabylonJS 開發跨平台遊戲</a></td>
    </tr>    
</table>

### <a name="porting-your-game"></a>移植您的遊戲

如果您有現有的遊戲，有許多資源與指南可協助您將遊戲快速轉移到 UWP。 若要啟動您的移植工作，您也可以考慮使用[通用 Windows 平台橋接](#universal-windows-platform-bridges)。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>將 Windows8 應用程式移植到通用 Windows 平台 app</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238322">從 Windows Runtime 8.x 移至 UWP</a></td>
    </tr>
    <tr>
        <td>將 Windows8 應用程式移植到通用 Windows 平台 app (影片)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">將 8.1 應用程式移植到 Windows10</a></td>
    </tr>
    <tr>
        <td>將 iOS app 移植到通用 Windows 平台 app</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238320">從 iOS 移到 UWP</a></td>
    </tr>
    <tr>
        <td>將 Silverlight 應用程式移植到通用 Windows 平台 app</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238323">從 Windows Phone Silverlight 移到 UWP</a></td>
    </tr>
    <tr>
        <td>將 XAML 或 Silverlight 移植到通用 Windows 平台 app (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">將應用程式從 XAML 或 Silverlight 移植到 Windows10</a></td>
    </tr>
    <tr>
        <td>將 Xbox 遊戲移植到通用 Windows 平台 app</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">從 Xbox One 移植到 Windows10 UWP</a></td>
    </tr>
    <tr>
        <td>從 DirectX 9 移植到 DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">從 DirectX 9 移植到通用 Windows 平台 (UWP)</a></td>
    </tr>
    <tr>
        <td>從 Direct3D 11 移植到 Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709">從 Direct3D 11 移植到 Direct3D 12</a></td>
    </tr>
    <tr>
        <td>從 OpenGL ES 移植到 Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">從 OpenGL ES 2.0 移植到 Direct3D 11</a></td>
    </tr>
    <tr>
        <td>使用 ANGLE 將 OpenGL ES 轉譯成 Direct3D 11</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=618387">ANGLE</a></td>
    </tr>
    <tr>
        <td>UWP 中的傳統型 Windows API 對應項</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh464945">通用 Windows 平台 (UWP) app 中 Windows API 的替代方法</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>原型與設計


既然您已決定所要建立的遊戲類型，以及將用來建置遊戲的工具與圖形技術，您便已準備好開始進行設計和製作原型。 您遊戲的核心是一個通用 Windows 平台 app，因此這就是您的起始點。

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP) 簡介

Windows10 導入了「通用 Windows 平台」(UWP)，此平台提供一個跨所有 Windows10 裝置的共同 API 平台。 UWP 發展及擴充了「Windows 執行階段」模型，並將它打造成一個緊密結合、統一的核心。 以 UWP 為目標的遊戲可以呼叫所有裝置通用的 WinRT API。 由於 UWP 提供一個保證的 API 層，因此您可以選擇建立一個將會在所有 Windows 10 裝置安裝的單一應用程式套件。 而且如果您想要的話，您的遊戲仍然可以呼叫其執行所在之裝置特定的 API (包括一些來自 Win32 和 .NET 的傳統型 Windows API)。

以下是詳細討論通用 Windows 平台 app 的出色指南，建議閱讀這些指南來協助您了解平台。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台 app 簡介</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726767">何謂通用 Windows 平台 app？</a></td>
    </tr>
    <tr>
        <td>UWP 概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn894631">UWP app 指南</a></td>
    </tr>
</table>
 

### <a name="getting-started-with-uwp-development"></a>UWP 開發入門

設定並準備好開發通用 Windows 平台 app 是相當快速且輕鬆的程序。 下列指南將引導您逐步完成這個程序。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 開發入門</td>
        <td><a href="https://dev.windows.com/getstarted">Windows 應用程式入門</a></td>
    </tr>
    <tr>
        <td>開始 UWP 開發設定</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726766">開始設定</a></td>
    </tr>
</table>

如果您是 UWP 程式設計的「初學者」，並且正考慮在您的遊戲中使用 XAML (請參閱[選擇您的圖形技術與程式設計語言](#choosing-your-graphics-technology-and-programming-language))， 則[初學者的 Windows10 開發入門](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)影片系列是一個很好的起始點。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>使用 XAML 進行 Windows10 開發的初學者指南 (影片系列)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">初學者的 Windows10 開發入門</a></td>
    </tr>
    <tr>
        <td>發佈使用 XAML 的 Windows10 初學者系列 (部落格文章)</td>
        <td><a href="http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">初學者的 Windows10 開發入門</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>UWP 開發概念

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台 app 開發概觀</td>
        <td><a href="https://dev.windows.com/develop">開發 Windows 應用程式</a></td>
    </tr>
    <tr>
        <td>UWP 的網路程式設計概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt280378">網路和 Web 服務</a></td>
    </tr>
    <tr>
        <td>在遊戲中使用 Windows.Web.HTTP 與 Windows.Networking.Sockets</td>
        <td><a href="work-with-networking-in-your-directx-game.md">遊戲的網路功能</a></td>
    </tr>
    <tr>
        <td>UWP 的非同步程式設計概念</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt187335">非同步程式設計</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Windows 桌面 APIsto UWP

以下提供一些連結，可幫助您將 Windows 傳統型遊戲移至 UWP。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>使用現有的 C++ 程式碼開發 UWP 遊戲</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">做法：在 UWP 應用程式中使用現有的 C++ 程式碼</a></td>
    </tr>
    <tr>
        <td>Win32 UWP API 與 COM API</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">適用於 UWP 應用程式的 Win32 與 COM API</a></td>
    </tr>
    <tr>
        <td>UWP 中不支援的 CRT 功能</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj606124.aspx">通用 Windows 平台應用程式不支援 CRT 功能</a></td>
    </tr>
    <tr>
        <td>Windows API 的替代方法</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt592894.aspx">通用 Windows 平台 (UWP) 應用程式中 Windows API 的替代方法</a></td>
    </tr>
</table>
 

### <a name="process-lifetime-management"></a>處理程序生命週期管理

處理程序生命週期管理 (或應用程式週期) 說明通用 Windows 平台 app 可經歷轉換的各種啟用狀態。 您的遊戲可以被啟用、暫停、繼續或終止，並且可以透過各種方式經歷這些狀態的轉換。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>處理應用程式週期轉換</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt243287">App 週期</a></td>
    </tr>
    <tr>
        <td>使用 Microsoft Visual Studio 觸發 App 轉換</td>
        <td><a href="https://msdn.microsoft.com/library/hh974425.aspx">如何在 Visual Studio 中觸發 UWP app 的暫停、繼續及背景事件</a></td>
    </tr>
</table>
 

### <a name="designing-game-ux"></a>設計遊戲 UX

出色的遊戲起源於充滿靈感的設計。

遊戲與應用程式共用一些通用的使用者介面元素和設計原則，但是遊戲通常在針對其使用者體驗方面具有獨特的外觀、感覺及設計目標。 遊戲只要針對以下兩個方面謹慎設計就能成功：您的遊戲何時應使用測試過的 UX，以及何時應跳脫窠臼並邁向創新。 您為您的遊戲選擇使用的演示技術 (DirectX、XAML、HTML5 或三種技術的結合) 可能會影響實作細節，但是您套用的設計原則大部分與該選擇無關。

遊戲設計 (例如關卡設計、步調、世界設計，以及其他方面) 與 UX 設計是分開的，具有自己的藝術形式由您與您的團隊決定，不包含在本開發指南中。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 設計基本知識與指導方針</td>
        <td><a href="https://dev.windows.com/design">設計 UWP app</a></td>
    </tr>
    <tr>
        <td>針對 app 週期狀態進行設計</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn611862">啟動、暫停和繼續的 UX 指導方針</a></td>
    </tr>
    <tr>
        <td>針對 Xbox One 與電視螢幕設計您的 UWP 應用程式</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">針對 Xbox 和電視進行設計</a></td>
    </tr>
    <tr>
        <td>以多種裝置尺寸規格為目標 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">針對 Windows Core 世界設計遊戲</a></td>
    </tr>   
</table>
 

#### <a name="color-guideline-and-palette"></a>色彩指南與調色盤

在您的遊戲中遵循一致的色彩指南可增加美學質感、有助於瀏覽，且可成為協助遊戲玩家使用功能表與 HUD 功能的強大工具。 讓遊戲元素 (例如警告、損毀、XP 及成就) 採用一致的色調可使 UI 更清晰，而降低使用明確標籤的需求。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>色彩指南</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">最佳做法：色彩</a></td>
    </tr>
</table>
 

#### <a name="typography"></a>印刷樣式

適當使用印刷樣式可增強遊戲的許多方面，包括 UI 版面配置、導覽、可讀性、氛圍、品牌及玩家沉浸度。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>印刷樣式指南</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535007">最佳做法：印刷樣式</a></td>
    </tr>
</table>
 

#### <a name="ui-map"></a>UI 對應

UI 對應是以流程圖方式呈現的遊戲導覽及功能表版面配置。 UI 對應有助於讓所有相關的專案關係人了解遊戲的介面與導覽路徑，而可以在開發週期中早日揭露潛在的障礙與無法解決的問題。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UI 對應指南</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535008">最佳做法：UI 對應</a></td>
    </tr>
</table>

### <a name="game-audio"></a>遊戲音訊

使用 XAudio2、XAPO 與 Windows Sonic 在遊戲中實作音訊的指南與參考。 XAudio2 是低階音訊 API，為開發高效能音訊引擎提供訊號處理與混音的基礎。 XAPO API 允許建立跨平台音訊處理物件 (XAPO) 以用於 Windows 與 Xbox 上的 XAudio2 中。 Windows Sonic 音訊支援您將杜比全景聲家庭影院版、杜比全景聲耳機版及 Windows HRTF 支援新增至您的遊戲或串流媒體應用程式。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAudio2 API</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049.aspx">XAudio2 的程式設計指南與 API 參考</a></td>
    </tr>
    <tr>
        <td>建立跨平台音訊處理物件</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee415735.aspx">XAPO 概觀</a></td>
    </tr>
    <tr>
        <td>音訊概念簡介</td>
        <td><a href="working-with-audio-in-your-directx-game.md">遊戲的音訊</a></td>
    </tr>
    <tr>
        <td>Windows Sonic 概觀</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt807491.aspx">空間音效</a></td>
    </tr>
    <tr>
        <td>Windows Sonic 空間音效範例</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Xbox Advanced Technology Group 音訊範例</a></td>
    </tr>
    <tr>
        <td>了解如何將 Windows Sonic 整合到遊戲中 (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">適用於 Xbox andWindows 空間音訊功能簡介</a></td>
    </tr>
</table>

### <a name="directx-development"></a>DirectX 開發

DirectX 遊戲開發的指南與參考資料。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 開發適用的 DirectX</td>
        <td><a href="directx-programming.md">DirectX 程式設計</a></td>
    </tr>
    <tr>
        <td>教學課程：如何建立 UWP DirectX 遊戲</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">使用 DirectX 建立簡單的 UWP 遊戲</a></td>
    </tr>
    <tr>
        <td>DirectX 與 UWP app 模型的互動</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">App 物件和 DirectX</a></td>
    </tr>
    <tr>
        <td>圖形與 DirectX 12 開發影片 (YouTube 頻道)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 與圖形教育訓練</a></td>
    </tr>
    <tr>
        <td>DirectX 概觀與參考</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX 圖形與遊戲</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 程式設計指南與參考</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D 12 圖形</a></td>
    </tr>
    <tr>
        <td>DirectX 12 基礎 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">功能更強、效能更佳：DirectX 12 的遊戲</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>了解 Direct3D 12

了解 Direct3D 12 中的變更，和如何使用 Direct3D 12 開始進行程式設計。 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>設定程式設計環境</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx">Direct3D 12 程式設計環境設定</a></td>
    </tr>
    <tr>
        <td>如何建立基本元件</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx">建立基本的 Direct3D 12 元件</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 中的變更</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx">從 Direct3D 11 移轉到 Direct3D 12 的重要變更</a></td>
    </tr>
    <tr>
        <td>如何從 Direct3D 11 移植到 Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx">從 Direct3D 11 移植到 Direct3D 12</a></td>
    </tr>
    <tr>
        <td>資源繫結概念 (涵蓋描述元、描述元表、描述元堆積及根簽章) </td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx">Direct3D 12 中的資源繫結</a></td>
    </tr>
    <tr>
        <td>管理記憶體</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx">Direct3D 12 中的記憶體管理</a></td>
    </tr>
</table>
 

#### <a name="directx-tool-kit-and-libraries"></a>DirectX 工具組和程式庫

DirectX 工具組、DirectX 紋理處理程式庫、DirectXMesh 幾何處理程式庫、UVAtlas 程式庫，以及 DirectXMath 程式庫能提供紋理、網格、精靈及其他公用程式功能與協助程式類別來進行 DirectX 開發。 這些程式庫能幫您節省開發時間與精力。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>取得適用於 DirectX 11 的 DirectX 工具組</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectXTK</a></td>
    </tr>
    <tr>
        <td>取得適用於 DirectX 12 的 DirectX 工具組</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615561">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>DirectX 紋理處理程式庫</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a></td>
    </tr>
    <tr>
        <td>取得 DirectXMesh 幾何處理程式庫</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>取得可建立和封裝 Isochart 紋理地圖集的 UVAtlas</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=512686">UVAtlas</a></td>
    </tr>
    <tr>
        <td>取得 DirectXMath 程式庫</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615560">DirectXMath</a></td>
    </tr>
    <tr>
        <td>DirectXTK （部落格文章） 中的 Direct3D12 支援</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">DirectX 12 支援</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>來自合作伙伴的 DirectX 資源

以下是一些由外部合作伙伴所建立的其他 DirectX 文件。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia：DX12 可行與禁止事項 (部落格文章) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">Nvidia GPU 上的 DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel：使用 DirectX 12 有效率地轉譯</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Intel Graphics 上的 DirectX 12 轉譯</a></td>
    </tr>
    <tr>
        <td>Intel︰DirectX 12 中的多重介面卡支援</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">如何使用 DirectX 12 實作明確的多重介面卡應用程式</a></td>
    </tr>
    <tr>
        <td>Intel：DirectX 12 教學課程</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Intel、Suzhou Snail 及 Microsoft 的共同作業白皮書</a></td>
    </tr>
</table>


## <a name="production"></a>製作


您的工作室現在已完全進入並移到製作階段，工作已散佈至整個團隊。 您正在潤飾、重構及延伸原型，以將它精心製作成完整的遊戲。

### <a name="notifications-and-live-tiles"></a>通知與動態磚

磚是您遊戲在 [開始] 功能表上的呈現形式。 磚與通知可以讓玩家感興趣，即使他們目前並沒有玩您的遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>開發磚與徽章</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt185606">磚、徽章及通知</a></td>
    </tr>
    <tr>
        <td>說明動態磚與通知的範例</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">通知範例</a></td>
    </tr>
    <tr>
        <td>彈性磚範本 (部落格文章)</td>
        <td><a href="http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx">彈性磚範本 - 結構描述和文件</a></td>
    </tr>
    <tr>
        <td>設計磚與徽章</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh465403">磚與徽章的指導方針</a></td>
    </tr>
    <tr>
        <td>適用於以互動方式開發動態磚範本的 Windows10 應用程式</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">通知視覺化工具</a></td>
    </tr>
    <tr>
        <td>適用於 Visual Studio 的 UWP Tile Generator 擴充功能</td>
        <td><a href="https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2">此工具可使用單一影像建立所有必要的磚</a></td>
    </tr>
    <tr>
        <td>適用於 Visual Studio 的 UWP Tile Generator 擴充功能 (部落格文章)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">使用 UWP Tile Generator 工具的提示</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-add-on-purchases"></a>啟用 （附加元件） 的應用程式內產品購買

附加元件 （應用程式內產品） 是玩家可購買的遊戲中的補充項目。 附加元件可以是遊戲關卡、 項目，或您玩家可能喜歡的任何人。 如果使用得當，附加元件可以提供營收，同時改善遊戲體驗。 定義和發佈您的遊戲附加元件，透過合作夥伴中心，並啟用您的遊戲程式碼中的應用程式內購買。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>耐久性附加元件</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219684">啟用應用程式內產品購買</a></td>
    </tr>
    <tr>
        <td>消費性附加元件</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219683">啟用消費性應用程式內產品購買</a></td>
    </tr>
    <tr>
        <td>附加元件的詳細資料與提交</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148551">附加元件提交</a></td>
    </tr>
    <tr>
        <td>監視附加元件銷售與人口統計為您的遊戲</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148538">附加元件下載數報告</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>偵錯、效能最佳化及監視

若要將效能最佳化，請完整利用目前硬體的功能，藉此充分利用 Windows 10 中的遊戲模式為玩家提供最佳的遊戲體驗。

Windows Performance Toolkit (WPT) 是一組效能監視工具，可產生深入的 Windows 作業系統與應用程式效能分析結果。 對於監視記憶體使用狀況及改進遊戲效能而言，此工具特別有用。 Windows Performance Toolkit 包含在 Windows10 SDK 和 Windows ADK 之中。 此工具組包含兩項獨立工具：Windows Performance Recorder (WPR) 和 Windows Performance Analyzer (WPA)。 ProcDump 命令列公用程式是 [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default) 的一部分，可監視 CPU 尖峰並在遊戲當機時產生傾印檔案。 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>測試您的程式碼效能</td>
        <td><a href="https://www.visualstudio.com/team-services/cloud-load-testing/">雲端式負載測試</a></td>
    </tr>
    <tr>
        <td>使用遊戲裝置資訊取得 Xbox 主機類型</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt825235">遊戲裝置資訊</a></td>
    </tr>
    <tr>
        <td>透過使用遊戲模式 API 排除硬體資源或安排存取硬體資源的優先順序以改善效能</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808808">遊戲模式</a></td>
    </tr>
    <tr>
        <td>從 Windows10 SDK 取得 Windows Performance Toolkit (WPT)</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">Windows10 SDK</a></td>
    </tr>
    <tr>
        <td>從 Windows ADK 取得 Windows Performance Toolkit (WPT)</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">Windows ADK</a></td>
    </tr>
    <tr>
        <td>使用 Windows Performance Analyzer 針對沒有回應的 UI 進行疑難排解 (影片)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">使用 WPA 進行關鍵路徑分析</a></td>
    </tr>
    <tr>
        <td>使用 Windows Performance Recorder 診斷記憶體使用狀況與流失 (影片)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">記憶體使用量與流失</a></td>
    </tr>
    <tr>
        <td>取得 ProcDump</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>了解如何使用 ProcDump (影片)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">設定 ProcDump 來建立傾印檔案</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>進階 DirectX 技術與概念

某些 DirectX 開發部分可以是相當微妙且複雜的。 當您進入製作階段而需要深入探討 DirectX 引擎細節，或對困難的效能問題進行偵錯時， 本節中的資源與資訊將可為您提供協助。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 上的 PIX</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/">在 Windows 上 DirectX 12 的調整與偵錯效能</a></td>
    </tr>
    <tr>
        <td>D3D12 開發的偵錯與驗證工具 (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12 效能調整和偵錯含 PIX 和 GPUValidation</a></td>
    </tr>
    <tr>
        <td>將圖形與效能最佳化 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">進階 DirectX 12 圖形與效能</a></td>
    </tr>
    <tr>
        <td>DirectX 圖形偵錯 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">使用 DirectX 工具解決遊戲難纏的圖形問題</a></td>
    </tr>
    <tr>
        <td>適用於偵錯 DirectX 12 的 Visual Studio 2015 工具 (影片)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Visual Studio 2015 中適用於 Windows10 的 DirectX 工具</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 程式設計指南</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D 12 程式設計指南</a></td>
    </tr>
    <tr>
        <td>結合 DirectX 與 XAML</td>
        <td><a href="directx-and-xaml-interop.md">DirectX 與 XAML 互通性</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>高動態範圍 (HDR) 內容開發

建置使用 HDR 的全彩功能的遊戲內容。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>簡介 HDR 和色彩概念 (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">在 DirectX 中加上 HDR 和進階的色彩</a></td>
    </tr>
    <tr>
        <td>了解如何轉譯 HDR 內容及偵測目前顯示是否支援</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">HDR 範例</a></td>
    </tr>
    <tr>
        <td>使用 DirectX 建立和設定進階的色彩</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Direct2D 進階的色彩影像轉譯範例</a></td>
    </tr>   
</table>


### <a name="globalization-and-localization"></a>全球化和當地語系化

開發 Windows 上全球適用的遊戲，並了解 Microsoft 暢銷產品中的國際功能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>讓您的遊戲為全球市場做好準備</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx">針對全球使用者進行開發的指導方針</a></td>
    </tr>
    <tr>
        <td>橋接語言、文化及技術</td>
        <td><a href="http://www.microsoft.com/Language/Default.aspx">語言慣例與標準 Microsoft 詞彙的線上資源</a></td>
    </tr>
</table>

### <a name="security"></a>安全性

建立一個您的玩家可以玩和公平競賽的環境。 在 TruePlay 中註冊的遊戲會在受保護程序中執行，減輕一些常見的攻擊。 遊戲監控系統也可協助識別常見的作弊情況。 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>電腦遊戲內抵抗作弊的工具</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808781">TruePlay</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>提交及發行您的遊戲

下列指南與資訊可協助讓發行及提交程序儘可能順利。

### <a name="publishing"></a>發行

您需要透過[合作夥伴中心](https://partner.microsoft.com/dashboard)來發佈和管理您的遊戲套件。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>合作夥伴中心的應用程式發佈</td>
        <td><a href="https://dev.windows.com/publish">發行 Windows 應用程式</a></td>
    </tr>
    <tr>
        <td>合作夥伴中心進階發行 (GDN)</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">合作夥伴中心進階發行指南</a></td>
    </tr>
    <tr>
        <td>使用 Azure Active Directory (AAD) 將使用者新增到您的合作夥伴中心帳戶</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">管理帳戶使用者</a></td>
    </tr>   
    <tr>
        <td>為遊戲分級 (部落格文章)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">使用 IARC 系統指派年齡分級的單一工作流程</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>封裝及上傳

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>了解使用串流安裝與選擇性套件 (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Nextgen UWP 應用程式發佈： 建立可延伸、 能夠串流，componentizedapps</a></td>
    </tr>
    <tr>
        <td>分隔與群組內容以啟用串流安裝</td>
        <td><a href="../packaging/streaming-install.md">UWP 應用程式串流安裝</a></td>
    </tr>
    <tr>
        <td>建立選擇性套件，例如 DLC 遊戲內容</td>
        <td><a href="../packaging/optional-packages.md">選用套件及相關集合的製作</a></td>
    </tr>
    <tr>
        <td>封裝您的 UWP 遊戲</td>
        <td><a href="../packaging/index.md">封裝應用程式</a></td>
    </tr>
    <tr>
        <td>封裝您的 UWP DirectX 遊戲</td>
        <td><a href="package-your-windows-store-directx-game.md">封裝您的 UWP DirectX 遊戲</a></td>
    </tr>
    <tr>
        <td>以合作廠商開發人員的身分封裝您的遊戲 (部落格文章)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">在沒有發行人員的市集帳戶存取權之下建立可上傳的套件</a></td>
    </tr>
    <tr>
        <td>使用 MakeAppx 建立應用程式套件和應用程式套件組合</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool">使用 App 封裝程式工具 MakeAppx.exe 建立套件</a></td>
    </tr>
    <tr>
        <td>使用 SignTool 數位簽署您的檔案</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/aa387764">使用 SignTool 簽署檔案及驗證檔案中的簽章</a></td>
    </tr>    
    <tr>
        <td>上傳及設定您遊戲的版本</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148542">上傳應用程式套件</a></td>
    </tr>
</table>


### <a name="policies-and-certification"></a>原則與認證

請勿讓認證問題延遲您的遊戲發行。 以下是需注意的原則與常見的認證問題。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Microsoft Store 應用程式開發人員合約</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh694058">應用程式開發人員合約</a></td>
    </tr>
    <tr>
        <td>在 Microsoft Store 中發佈應用程式的原則</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn764944">Microsoft Store 原則</a></td>
    </tr>
    <tr>
        <td>如何避免一些常見的應用程式認證問題</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj657968">避免常見的認證失敗</a></td>
    </tr>
</table>
 

### <a name="store-manifest-storemanifestxml"></a>市集資訊清單 (StoreManifest.xml)

市集資訊清單 (StoreManifest.xml) 是一個選用的組態檔，可以包含在您的應用程式套件中。 市集資訊清單提供不屬於 AppxManifest.xml 檔案一部分的額外功能。 例如，您可以使用市集資訊清單，在目標裝置不具備指定的最低 DirectX 功能層級或指定的最低系統記憶體時，阻止安裝您的遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>市集資訊清單結構描述</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt617335">StoreManifest 結構描述 (Windows10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>遊戲生命週期管理


在完成遊戲開發並傳送您的遊戲之後，還不算「遊戲結束」。 您可能已完成第一版的開發，但是您的遊戲在市場上的旅程才剛剛開始。 您將會想要監視使用狀況和錯誤報告、回應使用者的意見反應，以及發行遊戲更新。

### <a name="partner-center-analytics-and-promotion"></a>合作夥伴中心分析與促銷方案

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>合作夥伴中心分析</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148522">分析應用程式效能</a></td>
    </tr>
    <tr>
        <td>了解客戶在您的遊戲中如何使用 Xbox 功能</td>
        <td><a href="../publish/xbox-analytics-report.md">Xbox 分析報告</a></td>
    </tr>
    <tr>
        <td>回應客戶評論</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148546">回應客戶評論</a></td>
    </tr>
    <tr>
        <td>推銷您遊戲的方式</td>
        <td><a href="https://dev.windows.com/store-promotion">推銷您的應用程式</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights 可為您已發行的遊戲提供效能、遙測及使用狀況分析。 Application Insights 可協助您在發行遊戲之後偵測及解決問題、持續監視及改善使用狀況，以及了解玩家與您遊戲的持續互動情況。 Application Insights 的運作方式是將 SDK 新增到您的應用程式中，以將遙測數據傳送給 [Azure 入口網站](http://portal.azure.com/)。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>應用程式效能與使用狀況分析</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Visual Studio Application Insights</a></td>
    </tr>
    <tr>
        <td>在 Windows 應用程式中啟用 Application Insights</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">適用於 Windows Phone 與市集應用程式的 Application Insights</a></td>
    </tr>
</table>


### <a name="third-party-solutions-for-analytics-and-promotion"></a>第三方分析與促銷方案

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>了解使用 GameAnalytics 的播放程式行為</td>
        <td><a href="http://www.gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>將 UWP 遊戲與 Google Analytics 連接</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">取得適用於 Google Analytics 的 Windows SDK</a></td>
    </tr>
    <tr>
        <td>了解如何使用適用於 Google Analytics 的 Windows SDK (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">開始使用適用於 Google Analytics 的 Windows SDK</a></td>
    </tr>    
    <tr>
        <td>使用 Facebook 應用程式安裝廣告向 Facebook 使用者宣傳您的遊戲</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">取得適用於 Facebook 的 Windows SDK</a></td>
    </tr>
    <tr>
        <td>了解如何使用 Facebook 應用程式安裝廣告 (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">開始使用適用於 Facebook 的 Windows SDK</a></td>
    </tr>
    <tr>
        <td>使用 Vungle 將視訊廣告新增至您的遊戲</td>
        <td><a href="https://v.vungle.com/sdk">取得適用於 Vungle 的 Windows SDK</a></td>
    </tr>
</table>
 

### <a name="creating-and-managing-content-updates"></a>建立及管理內容更新

若要更新您已發行的遊戲，請提交具有較新版本號碼的新應用程式套件。 在套件完成提交與認證程序之後，就會自動以更新的形式提供給客戶。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>更新及設定您遊戲的版本</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">套件版本編號</a></td>
    </tr>
    <tr>
        <td>遊戲套件管理指導方針</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">應用程式套件管理指導方針</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>將 Xbox Live 新增到您的遊戲

Xbox Live 是連接世界各地數以百萬計的玩家的首要遊戲網路。 開發人員可存取能夠增加他們遊戲的對象的 Xbox Live 功能，包括 Xbox Live 目前狀態、排行榜、雲端儲存、遊戲中心、俱樂部、派對交談、遊戲 DVR 等。

> [!Note]
> 如果您想要開發 Xbox Live 支援的遊戲，則有數個選項可供您使用。 如需各種計畫的相關資訊，請參閱[開發人員計畫概觀](../xbox-live/developer-program-overview.md)。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live 概觀</td>
        <td><a href="../xbox-live/index.md">Xbox Live 開發人員指南</a></td>
    </tr>
    <tr>
        <td>了解視計畫可使用哪些功能</td>
        <td><a href="../xbox-live/developer-program-overview.md#feature-table">開發人員計畫概觀：功能表格</a></td>
    </tr>
    <tr>
        <td>開發 Xbox Live 遊戲的實用資源的連結</td>
        <td><a href="../xbox-live/xbox-live-resources.md">Xbox Live 資源</a></td>
    </tr>
    <tr>
        <td>了解如何從 Xbox Live 服務取得資訊</td>
        <td><a href="../xbox-live/introduction-to-xbox-live-apis.md">Xbox Live API 簡介</a></td>
    </tr>
</table>


### <a name="for-developers-in-the-xbox-live-creators-program"></a>對於 Xbox Live 創作者計畫的開發人員

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>概觀</td>
        <td><a href="../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">開始使用 Xbox Live 創作者計畫</a></td>
    </tr>
    <tr>
        <td>將 Xbox Live 新增到您的遊戲</td>
        <td><a href="../xbox-live/get-started-with-creators/creators-step-by-step-guide.md">整合 Xbox Live 創作者計畫的逐步解說指南</a></td>
    </tr>
    <tr>
        <td>將 Xbox Live 新增至您使用 Unity 建立的 UWP 遊戲</td>
        <td><a href="../xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">開始使用 Unity 遊戲引擎開發 Xbox Live 創作者計畫遊戲</a></td>
    </tr>
    <tr>
        <td>設定開發沙箱</td>
        <td><a href="../xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Xbox Live 沙箱簡介</a></td>
    </tr>
    <tr>
        <td>設定帳戶供測試用</td>
        <td><a href="../xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">在您的測試環境授權 Xbox Live 帳戶</a></td>
    </tr>
    <tr>
        <td>Xbox Live 創作者計畫的範例</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">適用於創作者計畫開發人員的程式碼範例</a></td>
    </tr>
    <tr>
        <td>了解如何在 UWP 遊戲中整合跨平台 Xbox Live 體驗 (影片)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Xbox Live 創作者計畫</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>對於 ID@Xbox 計劃中的受管理合作夥伴與開發人員

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>概觀</td>
        <td><a href="../xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">開始使用 Xbox Live 作為受管理的合作夥伴或 ID 開發人員</a></td>
    </tr>
    <tr>
        <td>將 Xbox Live 新增到您的遊戲</td>
        <td><a href="../xbox-live/get-started-with-partner/partners-step-by-step-guide.md">為受管理合作夥伴與 ID 成員整合 Xbox Live 的逐步解說指南</a></td>
    </tr>
    <tr>
        <td>將 Xbox Live 新增至您使用 Unity 建立的 UWP 遊戲</td>
        <td><a href="../xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">為 ID 與受管理合作夥伴將 Xbox Live 新增支援至具有 IL2CPP 指令碼後端之 UWP 適用的 Unity</a></td>
    </tr>
    <tr>
        <td>設定開發沙箱</td>
        <td><a href="../xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">進階 Xbox Live 沙箱</a></td>
    </tr>
    <tr>
        <td>使用 Xbox Live 之遊戲的需求 (GDN)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=533217">Windows10 上 Xbox Live 的 Xbox 需求</a></td>
    </tr>
    <tr>
        <td>範例</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">適用於 ID@Xbox 開發人員的程式碼範例</a></td>
    </tr>  
    <tr>
        <td>Xbox Live 遊戲開發概觀 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">使用適用於 Windows10 的 Xbox Live 進行開發</a></td>
    </tr>
    <tr>
        <td>跨平台配對 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live 多人遊戲：跨平台配對和遊戲服務簡介</a></td>
    </tr>
    <tr>
        <td>「神鬼寓言：傳奇」(Fable Legends) 中的跨裝置遊戲 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">「神鬼寓言：傳奇」(Fable Legends)：使用 Xbox Live 進行跨裝置遊戲</a></td>
    </tr>
    <tr>
        <td>Xbox Live 統計資料與成就 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">利用 Xbox Live 中雲端使用者統計資料與成就的最佳做法</a></td>
    </tr>
</table>


## <a name="additional-resources"></a>其他資源

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>遊戲開發影片</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">來自 GDC 和 //build 等重要會議的影片</a></td>
    </tr>
    <tr>
        <td>獨立製作遊戲開發 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">獨立開發人員的新機會</a></td>
    </tr>
    <tr>
        <td>多核心行動裝置的考量 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">多核心行動裝置中持續的遊戲效能</a></td>
    </tr>
    <tr>
        <td>開發 Windows10 傳統型遊戲 (影片)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">適用於 Windows10 的電腦遊戲</a></td>
    </tr>
</table>



 

 

 
