---
title: Windows 10 遊戲開發指南
description: 開發「通用 Windows 平台」(UWP) 遊戲的資源與資訊的端對端指南。
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
---

# Windows 10 遊戲開發指南


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

歡迎使用 Windows 10 遊戲開發指南！

本指南說明有關您開發通用 Windows 平台 (UWP) 遊戲所需的端對端資源與資訊集合。

## 通用 Windows 平台 (UWP) 的遊戲開發簡介


當您建立 Windows 10 遊戲時，便擁有一個可與全球數百萬個遍及手機、電腦及 Xbox One 玩家接觸的機會。 有了 Windows 上的 Xbox、Xbox Live、跨裝置多人遊戲、優質的遊戲社群，以及「通用 Windows 平台」(UWP) 和 DirectX 12 等威力強大的新功能，Windows 10 遊戲讓所有年齡及性別的玩家都興奮不已。 新的「通用 Windows 平台」(UWP) 提供用於手機、電腦及 Xbox One 的通用 API，以及可針對各種裝置體驗量身打造您遊戲的工具與選項，為您的遊戲帶來跨 Windows 10 裝置的相容性。

本指南提供可在您開發遊戲的過程中協助您的端對端資源與資訊集合。 各個小節的編排是根據遊戲的開發階段，因此您會知道要從何處尋找所需的資訊。

若要開始，[遊戲開發資源](#resources)一節提供文件、程式及其他在建立遊戲時相當實用之資源的概略綜覽。

本指南將會隨著其他 Windows 10 遊戲開發資源和資料的推出進行更新。

## 遊戲開發資源


從文件到開發人員計劃、論壇、部落格及範例，有許多資源可在您的遊戲開發之路上協助您。 以下是您開發 Windows 10 遊戲時需了解的資源摘要報導。

> **注意：**管理 Xbox One 開發和選取 Windows 10 遊戲功能 (例如「Xbox Live 服務」) 時，是透過 ID@Xbox 和 Microsoft Studios 這類計畫來管理。 本指南涵蓋的資源範圍很廣，因此視您所參與的計畫或您的特定開發角色而定，您可能會發現有些資源無法使用。 解析成 developer.xboxlive.com、forums.xboxlive.com、xdi.xboxlive.com 或「遊戲開發人員網路」(GDN) 的連結就是其中幾例。 如需有關與 Microsoft 合作的資訊，請參閱[開發人員計畫](#programs)。

 

### 遊戲開發文件

在這整份指南中，您會發現相關文件的深層連結依工作、技術及遊戲開發階段編排。 為了讓您能夠完全檢視有哪些可用的文件，以下是 Windows 10 遊戲開發的主要文件入口網站。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 開發人員中心主要入口網站</td>
        <td>[Windows Dev Center](https://dev.windows.com)</td>
    </tr>
    <tr>
        <td>開發 Windows 應用程式</td>
        <td>[Develop Windows apps](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>通用 Windows 平台 app 開發</td>
        <td>[How-to guides for Windows 10 apps](https://msdn.microsoft.com/library/windows/apps/mt244352)</td>
    </tr>
    <tr>
        <td>UWP 遊戲使用方法指南</td>
        <td>[Games and DirectX](index.md) </td>
    </tr>
    <tr>
        <td>DirectX 參考與概觀</td>
        <td>[DirectX Graphics and Gaming](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Xbox Live 文件</td>
        <td>[Xbox Live SDK](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Xbox One 開發人員文件 (GDN)</td>
        <td>[Xbox One XDK documentation](https://developer.xboxlive.com/platform/development/documentation/Pages/home.aspx)</td>
    </tr>
    <tr>
        <td>Xbox One 開發人員白皮書 (GDN)</td>
        <td>[White Papers](https://developer.xboxlive.com/platform/development/education/Pages/WhitePapers.aspx)</td>
    </tr>     
</table>


### 開發人員計畫

Microsoft 提供數個可協助您開發及發行 Windows 遊戲的開發人員計畫。 若要在「Windows 市集」中發行遊戲，您將需要在「Windows 開發人員中心」建立一個開發人員帳戶。 視您的遊戲和工作室需求而定，其他計畫可能會有相關，並且可以創造像是 Xbox One 開發與 Xbox Live 整合的機會。

### Windows 開發人員中心

在「Windows 開發人員中心」註冊開發人員帳戶是邁向發行 Windows 遊戲的第一步。 開發人員帳戶可讓您保留您遊戲的名稱，以及將適用於所有 Windows 裝置的免費或付費遊戲提交給「Windows 市集」。 您可以使用開發人員帳戶來管理您的遊戲與遊戲內產品、取得詳細的分析，以及啟用可為您的全球玩家創造絕佳體驗的服務。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>註冊開發人員帳戶</td>
        <td>[Ready to sign up?](https://msdn.microsoft.com/library/windows/apps/bg124287)</td>
    </tr>
</table>  


### ID@Xbox

ID@Xbox 計畫可協助合格的遊戲開發人員在 Windows 和 Xbox One 上自行發行遊戲。 如果您想要為 Xbox One 開發遊戲，或是在您的 Windows 10 遊戲中新增 Xbox Live 功能 (例如玩家分數、成就及排行榜)，請向 ID@Xbox 註冊。 成為 ID@Xbox 開發人員以取得所需的工具與支援，讓您可以充分發揮您的創意並獲得最大的成功。 向 ID@Xbox 提出申請之前，請先在「Windows 開發人員中心」註冊一個開發人員帳戶。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox developer program</td>
        <td>[Independent Developer Program for Xbox One](http://go.microsoft.com/fwlink/p/?LinkID=526271)</td>
    </tr>
    <tr>
        <td>ID@Xbox 消費者網站</td>
        <td>[ID@Xbox](http://www.idatxbox.com/)</td>
    </tr>
</table>


### DirectX 優先使用計畫

專業遊戲開發人員如果想要優先預覽 Direct3D 12 API 變更並想要在論壇中提供意見反應，可以加入 DirectX 優先使用計畫。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>註冊 DirectX 12 優先使用計畫</td>
        <td>[DirectX Early Access Program](http://1drv.ms/1dgelm6)</td>
    </tr>
</table>


### Xbox 工具與中介軟體

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


### 遊戲範例

有許多 Windows 10 遊戲與應用程式範例可協助您了解 Windows 10 遊戲功能，並在遊戲開發上快速入門。 將會有更多範例以定期方式開發及發行，因此請別忘了偶爾回到範例入口網站上查看有什麼最新內容。 您也可以[查看](https://help.github.com/articles/watching-repositories/) GitHub 存放庫以獲知變更和新內容。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台 app 範例</td>
        <td>[Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples)</td>
    </tr>
    <tr>
        <td>Direct3D 12 圖形範例</td>
        <td>[DirectX-Graphics-Samples](https://github.com/Microsoft/DirectX-Graphics-Samples)</td>
    </tr>
    <tr>
        <td>Direct3D 11 第一人稱遊戲範例</td>
        <td>[Create a simple UWP game with DirectX](tutorial--create-your-first-metro-style-directx-game.md)</td>
    </tr>
    <tr>
        <td>Direct2D 自訂影像效果範例</td>
        <td>[D2DCustomEffects](http://go.microsoft.com/fwlink/p/?LinkId=620531)</td>
    </tr>
    <tr>
        <td>Direct2D 漸層網格範例</td>
        <td>[D2DGradientMesh](http://go.microsoft.com/fwlink/p/?LinkId=620532)</td>
    </tr>
    <tr>
        <td>Direct2D 相片調整範例</td>
        <td>[D2DPhotoAdjustment](http://go.microsoft.com/fwlink/p/?LinkId=620533)</td>
    </tr>
    <tr>
        <td>Xbox One 遊戲範例 (GDN)</td>
        <td>[Samples](https://developer.xboxlive.com/platform/development/education/Pages/Samples.aspx)</td>
    </tr>
    <tr>
        <td>Windows 8 遊戲範例 (MSDN Code Gallery)</td>
        <td>[Windows Store game samples](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft)</td>
    </tr>
    <tr>
        <td>JavaScript 與 HTML5 遊戲範例</td>
        <td>[JavaScript and HTML5 touch game sample](https://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)</td>
    </tr>      
</table>


### 開發人員論壇

開發人員論壇是一個詢問和回答遊戲開發問題，以及與遊戲開發社群連結的絕佳場所。 論壇也是極佳的資源，可供尋找開發人員過去已面對並解決之困難問題的現有解答。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 開發人員中心應用程式開發人員論壇</td>
        <td>[Windows app forums](https://social.msdn.microsoft.com/Forums/windowsapps/home)</td>
    </tr>
    <tr>
        <td>Windows 開發人員中心傳統型應用程式開發人員論壇</td>
        <td>[Windows desktop application forums](https://social.msdn.microsoft.com/Forums/windowsdesktop/home)</td>
    </tr>
    <tr>
        <td>DirectX Windows 市集遊戲 (已封存的論壇文章)</td>
        <td>[Building Windows Store games with DirectX (archived)](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=wingameswithdirectx)</td>
    </tr>
    <tr>
        <td>Windows 10 受管理的合作夥伴開發人員論壇</td>
        <td>[XBOX Developer Forums: Windows 10](http://aka.ms/win10devforums)</td>
    </tr>
    <tr>
        <td>DirectX 優先使用計畫論壇</td>
        <td>[DirectX 12 forum](http://directx12forum.azurewebsites.net/index.php)</td>
    </tr>
</table>


### 開發人員部落格

開發人員部落格是另一個提供最新遊戲開發相關資訊的絕佳資源。 您將可找到有關新功能、實作詳細資料、最佳做法、架構背景等等的貼文。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>建置適用於 Windows 的應用程式部落格</td>
        <td>[Building Apps for Windows](http://blogs.windows.com/buildingapps/)</td>
    </tr>
    <tr>
        <td>Windows 10 部落格 (部落格文章)</td>
        <td>[Posts in Windows 10](http://blogs.windows.com/blog/tag/windows-10/)</td>
    </tr>
    <tr>
        <td>Visual Studio 工程小組部落格</td>
        <td>[The Visual Studio Blog](http://blogs.msdn.com/b/visualstudio/)</td>
    </tr>
    <tr>
        <td>Visual Studio 開發人員工具部落格</td>
        <td>[Developer Tools Blogs](http://blogs.msdn.com/b/developer-tools/)</td>
    </tr>
    <tr>
        <td>Somasegar 開發人員工具部落格</td>
        <td>[Somasegar’s blog](http://blogs.msdn.com/b/somasegar/)</td>
    </tr>
    <tr>
        <td>DirectX 開發人員部落格</td>
        <td>[DirectX Developer blog](http://blogs.msdn.com/b/directx)</td>
    </tr>
    <tr>
        <td>DirectX 12 簡介 (部落格文章)</td>
        <td>[DirectX 12](http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ 工具小組部落格</td>
        <td>[Visual C++ team blog](http://blogs.msdn.com/b/vcblog/)</td>
    </tr>
    <tr>
        <td>ID@Xbox 開發人員部落格</td>
        <td>[ID@XBOX Developer Blog](http://www.idatxbox.com/category/developer-blog/)</td>
    </tr>
</table>
 

## 概念與規劃


在概念與規劃階段，您將決定您遊戲的未來面貌，以及要用來實現遊戲的技術與工具。

### 遊戲開發技術指南概觀

開始進行 UWP 遊戲開發時，會有多個圖形、輸入、音訊、網路、公用程式及程式庫選項可供您選擇。

如果您已經決定好所有將在遊戲中使用的技術，那真是好極了！ 如果沒有，則[適用於 UWP app 的遊戲技術](game-development-platform-guide.md)指南提供許多可用技術的絕佳概觀， 強烈建議您閱讀這份指南以協助您了解這些選項及如何將它們搭配使用。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 遊戲技術綜覽</td>
        <td>[Game technologies for UWP apps](game-development-platform-guide.md)</td>
    </tr>
</table>
 

這三個 GDC 2015 影片提供 Windows 10 遊戲開發與 Windows 10 遊戲體驗的絕佳概觀。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 10 遊戲開發概觀 (影片)</td>
        <td>[Developing Games for Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Windows 10 遊戲體驗 (影片)</td>
        <td>[Gaming Consumer Experience on Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10)</td>
    </tr>
    <tr>
        <td>遊戲在整個 Microsoft 生態系統的發展 (影片)</td>
        <td>[The Future of Gaming Across the Microsoft Ecosystem](http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem)</td>
    </tr>
</table>
 

### 選擇您的圖形技術與程式設計語言

有數種程式設計語言與圖形技術可在 Windows 10 遊戲中使用。 您所採取的途徑會取決於您要開發的遊戲類型、開發工作室的經驗與喜好，以及遊戲的特定功能需求。 您將會使用 C#、C++ 或 JavaScript？ DirectX、XAML 或 HTML5？

### DirectX

Microsoft DirectX 是適用於高效能 2D 及 3D 圖形與多媒體的選擇。 Windows 10 的新功能 Direct3D 12 使主控台式 API 變得更強大，而且比以往執行起來更快更有效率。 您的遊戲可以充分利用新式圖形硬體來提供更多物件、更豐富的場景，以及更好的效果。 Direct3D 12 針對 Windows 10 電腦與 Xbox One 提供了最佳化的圖形。 如果您想要使用熟悉的 Direct3D 11 圖形管線，您仍然可以從 Direct3D 11.3 中加入的新轉譯與最佳化功能中獲益。 而且如果您是千真萬確且有 Win32 情節的傳統型 Windows API 開發人員，您仍然可以在 Windows 10 中使用該選項。

DirectX 的廣泛功能與深度的平台整合可為要求最嚴苛的遊戲提供所需的強大功能與效能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 遊戲的使用方法指南</td>
        <td>[Games and DirectX](index.md)</td>
    </tr>
    <tr>
        <td>DirectX 概觀與參考</td>
        <td>[DirectX Graphics and Gaming](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 程式設計指南與參考</td>
        <td>[Direct3D 12 Graphics](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
</table>
 

### XAML

XAML 是一種容易使用的宣告式 UI 語言，擁有便利的功能，例如動畫、腳本、資料繫結、可調整的向量式圖形、動態調整大小及場景圖形。 XAML 非常適用於遊戲 UI、功能表、精靈及 2D 圖形。 為了讓 UI 版面配置變得簡單，XAML 與設計及開發工具 (例如 Expression Blend 與 Microsoft Visual Studio) 相容。 XAML 經常與 C# 搭配使用，但是如果您偏好使用 C++ 語言，或是您的遊戲對 CPU 的要求較高，那麼 C++ 也是一個很好的選擇。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML 平台概觀</td>
        <td>[XAML platform](https://msdn.microsoft.com/library/windows/apps/mt228259)</td>
    </tr>
    <tr>
        <td>XAML UI 與控制項</td>
        <td>[Controls, layouts, and text](https://msdn.microsoft.com/library/windows/apps/mt228348)</td>
    </tr>
</table>
 

### HTML 5

超文字標記語言 (HTML) 是用於網頁、應用程式及豐富型用戶端的常用 UI 標記語言。 Windows 遊戲可以使用 HTML5 做為完整的功能展示層，此展示層具備熟悉的 HTML 功能、可存取「通用 Windows 平台」，以及支援新式 Web 功能 (例如 AppCache、Web 工作者、畫布、拖放、非同步程式設計及 SVG)。 HTML 轉譯可在幕後利用 DirectX 硬體加速能力，因此您仍然可以享有 DirectX 的效能優點，而不需撰寫任何額外的程式碼。 如果您精通 Web 開發、移植 Web 遊戲，或是想要使用比其他選項更容易著手的語言和圖形層，HTML5 會是一個很好的選擇。 HTML5 是與 JavaScript 搭配使用，但也可以呼叫使用 C# 或 C++/CX 建立的元件。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HTML5 與文件物件模型資訊</td>
        <td>[HTML and DOM reference](https://msdn.microsoft.com/library/windows/apps/br212882.aspx)</td>
    </tr>
    <tr>
        <td>HTML5 W3C 建議</td>
        <td>[HTML5](http://go.microsoft.com/fwlink/p/?linkid=221374)</td>
    </tr>
</table>
 

### 結合呈現技術

Microsoft DirectX Graphics Infrastructure (DXGI) 可提供跨多種圖形技術的互通性與相容性。 針對高效能圖形，您可以結合 XAML 與 DirectX，將 XAML 用於功能表與其他簡單的 UI，然後將 DirectX 用於轉譯複雜的 2D 與 3D 場景。 DXGI 也提供 Direct2D、Direct3D、DirectWrite、DirectCompute 及 Microsoft Media Foundation 之間的相容性。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX Graphics Infrastructure 程式設計指南與參考</td>
        <td>[DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)</td>
    </tr>
    <tr>
        <td>結合 DirectX 與 XAML</td>
        <td>[DirectX and XAML interop](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

### C++

C++/CX 是一種高效能、低額外負荷的語言，可提供兼具速度、相容性及平台存取的強大組合。 C++/CX 可讓您很容易地使用 Windows 10 中所有優越的遊戲功能，包括 DirectX 與 Xbox Live。 您也可以重複使用現有的 C++ 程式碼與程式庫。 C++/CX 可建立快速、原生且不會導致產生記憶體回收額外負荷的程式碼，因此您的遊戲可以兼具效能優越與耗電量低的優點，進而延長電池壽命。 請將 C++/CX 與 DirectX 或 XAML 搭配使用，或建立將兩者結合使用的遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C++/CX 參考與概觀</td>
        <td>[Visual C++ Language Reference (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ 程式設計指南與參考</td>
        <td>[Visual C++ in Visual Studio 2015](https://msdn.microsoft.com/library/60k1461a.aspx)</td>
    </tr>
</table>
 

### C#

C# (發音為 "C sharp") 是一種簡單、強大、型別安全且物件導向的新式、創新語言。 C# 可讓您快速進行開發，同時又保有 C 式語言的熟悉性與表達方式。 C# 雖然容易使用，但它也有許多先進的語言功能，例如多型、委派、Lambda、關閉、Iterator 方法、共變數，以及 Language-Integrated Query (LINQ) 運算式。 如果您的目標是 XAML、想要快速開始開發您的遊戲，或之前有使用過 C# 的經驗，C# 就會是您的絕佳選擇。 C# 主要是與 XAML 搭配使用，因此如果您想要使用 DirectX，請改為選擇 C++，或將遊戲的部分內容撰寫成與 DirectX 互動的 C++ 元件。 或是考慮使用 [Win2D](https://github.com/Microsoft/Win2D)，這是一種適用於 C# 與 C++ 的直接模式 Direct2D 圖形程式庫。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# 程式設計指南與參考</td>
        <td>[C# language reference](https://msdn.microsoft.com/library/kx37x362.aspx)</td>
    </tr>
</table>
 

### JavaScript

JavaScript 是一種動態指令碼語言，廣泛地使用在新式 Web 與豐富型用戶端應用程式中。

Windows JavaScript 應用程式可以透過簡單且直覺化的方式 (以物件導向之 JavaScript 類別的方法與屬性) 存取「通用 Windows 平台」的強大功能。 如果您來自 Web 開發環境且已經熟悉 JavaScript，或是想要使用 HTML5、CSS、WinJS 或 JavaScript 程式庫，JavaScript 就會是您遊戲的良好選擇。 如果您的目標是 DirectX 或 XAML，請改為選擇 C# 或 C++/CX。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>JavaScript 與 Windows 執行階段參考</td>
        <td>[JavaScript reference](https://msdn.microsoft.com/library/windows/apps/jj613794)</td>
    </tr>
</table>


### 使用 Windows 執行階段元件來結合語言

有了「通用 Windows 平台」，結合以不同語言撰寫的元件就變得相當簡單。 您可以使用 C++、C# 或 Visual Basic 來建立 Windows 執行階段元件，然後從 JavaScript、C#、C++ 或 Visual Basic 呼叫那些元件。 這是使用您選擇的語言進行遊戲某些部分之程式設計的絕佳方式。 元件也可以讓您取用只以特定語言提供的外部程式庫，以及使用您已經撰寫完成的舊有程式碼。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>如何建立 Windows 執行階段元件</td>
        <td>[Creating Windows Runtime Components](https://msdn.microsoft.com/library/windows/apps/hh441572.aspx)</td>
    </tr>
</table>


### 您的遊戲應該使用哪一個版本的 DirectX？

如果您要為您的遊戲選擇 DirectX，您將需要決定要使用哪一個版本：Microsoft Direct3D 12 或 Microsoft Direct3D 11。

Direct3D 11.3 是一個使用熟悉的 Direct3D 程式設計模型的低層階圖形 API。 Direct3D 11 仍是通用 Windows app 的一個選項，您將可以使用 Direct3D 11.3 中加入的新轉譯與最佳化功能。

Direct3D 12 是 Windows 10 中的新功能，並且導入了新的管線程式設計模型。 Direct3D 12 與硬體較接近、使用較少的抽象概念，並可讓您的遊戲在資源的使用上有更好的控制。 Direct3D 12 提供更好的 CPU、GPU 及電源效能。

如果您有以 Direct3D 11 撰寫的現有引擎，並且尚未準備好改用 Direct3D 12， 則您可以 Direct3D 11 on 12 來達到一些效能改進，然後再開始轉換到 Direct3D 12。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>選擇 Direct3D 12 或 Direct3D 11</td>
        <td>[What is Direct3D 12?](https://msdn.microsoft.com/library/windows/desktop/dn899228)</td>
    </tr>
    <tr>
        <td>Direct3D 11 概觀</td>
        <td>[Direct3D 11 Graphics](https://msdn.microsoft.com/library/windows/desktop/ff476080)</td>
    </tr>
    <tr>
        <td>Direct3D 11 on 12 概觀</td>
        <td>[Direct3D 11 on 12](https://msdn.microsoft.com/library/windows/desktop/dn913195)</td>
    </tr>
</table>


### 橋接、遊戲引擎及中介軟體

視您遊戲的需求而定，使用橋接、遊戲引擎或中介軟體可以節省開發與測試時間及資源。 以下是橋接、遊戲引擎及中介軟體的一些概觀與資源，可協助您決定適合您的選項。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>適用於 Windows 10 的橋接與遊戲引擎 (部落格文章)</td>
        <td>[More ways to bring your code to fast-growing Windows 10 Store](http://blogs.windows.com/buildingapps/2015/09/17/more-ways-to-bring-your-code-to-fast-growing-windows-10-store/)</td>
    </tr>
    <tr>
        <td>使用中介軟體進行遊戲開發 (影片)</td>
        <td>[Accelerating Windows Store Game Development with Middleware](https://channel9.msdn.com/Events/Build/2013/3-187)</td>
    </tr>
    <tr>
        <td>Visual Studio 與 Unity、Unreal 及 Cocos2d (部落格文章)</td>
        <td>[Visual Studio for Game Development: New Partnerships with Unity, Unreal Engine and Cocos2d](http://blogs.msdn.com/b/somasegar/archive/2015/04/17/visual-studio-for-game-development-new-partnerships-with-unity-unreal-engine-and-cocos2d.aspx)</td>
    </tr>
    <tr>
        <td>遊戲中介軟體簡介 (部落格文章)</td>
        <td>[Game Development Middleware - What is it? Do I need it?](http://blogs.msdn.com/b/wsdevsol/archive/2014/05/02/game-development-middleware-what-is-it-do-i-need-it.aspx)</td>
    </tr>
</table>
 

### 通用 Windows 平台橋接

「通用 Windows 平台橋接」是可將您現有的應用程式或遊戲轉移到 UWP 的技術。 橋接是快速展開 UWP 遊戲開發的絕佳方式。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 橋接</td>
        <td>[Bring your code to Windows](https://dev.windows.com/bridges/)</td>
    </tr>
    <tr>
        <td>適用於 iOS 的 Windows 橋接器</td>
        <td>[Bring your iOS apps to Windows](https://dev.windows.com/bridges/ios)</td>
    </tr>
    <tr>
        <td>適用於 .NET 與 Win32 的 Windows 橋接 (百年專案 (Project Centennial)) 預覽</td>
        <td>[Windows Developer Preview Programs](http://go.microsoft.com/fwlink/p/?LinkID=624543)</td>
    </tr>
</table>
 

### Unity

Unity 5 是獲得獎項肯定、用於建立 2D 與3D 遊戲及互動式體驗的新一代開發平台。 Unity 5 不僅提供新的藝術表現能力，也提供增強的圖形功能及更高的效率。

在 [Unity 藍圖](https://unity3d.com/unity/roadmap)上，未來的 Unity 版本將會支援 DirectX 12。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unity 遊戲引擎</td>
        <td>[Unity - Game Engine](http://unity3d.com/)</td>
    </tr>
    <tr>
        <td>取得 Unity 5</td>
        <td>[Get Unity](http://unity3d.com/get-unity)</td>
    </tr>
    <tr>
        <td>Unity 5.2 中的通用 Windows 平台 app 支援 (部落格文章)</td>
        <td>[Windows 10 Universal Platform apps in Unity 5.2](http://blogs.unity3d.com/2015/09/09/windows-10-universal-apps-in-unity-5-2/)</td>
    </tr>
    <tr>
        <td>適用於 Windows 的 Unity 文件</td>
        <td>[Unity Manual / Windows](http://docs.unity3d.com/Manual/Windows.mdl)</td>
    </tr>
    <tr>
        <td>將您的 Unity 遊戲發行為通用 Windows 平台 app (影片)</td>
        <td>[How to publish your Unity game as a UWP app](https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app)</td>
    </tr>
    <tr>
        <td>使用 Unity 建置 Windows 遊戲與應用程式 (影片)</td>
        <td>[Making Windows games and apps with Unity](https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity)</td>
    </tr>
    <tr>
        <td>使用 Visual Studio (影片系列) 開發 Unity 遊戲</td>
        <td>[Using Unity with Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=722359)</td>
    </tr>
</table>
 

### Havok

Havok 的工具與技術模組套件可協助遊戲建立者達到新的互動與沉浸式情境層級。 Havok 能夠實現極逼真的物理效果、互動式模擬及令人驚豔的場景。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok 網站</td>
        <td>[Havok](http://www.havok.com/)</td>
    </tr>
    <tr>
        <td>Havok 工具套件</td>
        <td>[Havok Product Overview](http://www.havok.com/products/)</td>
    </tr>
    <tr>
        <td>Havok 支援論壇</td>
        <td>[Havok](https://software.intel.com/forums/havok/)</td>
    </tr>
</table>
 

### Cocos2d

Cocos2d-X 是一個跨平台的開放原始碼遊戲開發引擎與工具套件，可支援建置 UWP 遊戲。 從第 3 版開始，也新增了 3D 功能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td>[What is Cocos2d-X?](http://www.cocos2d-x.org/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x 程式設計人員指南</td>
        <td>[Cocos2d-x Programmers Guide v3.8](http://www.cocos2d-x.org/programmersguide/)</td>
    </tr>
    <tr>
        <td>Windows 10 上的 Cocos2d-x (部落格文章)</td>
        <td>[Running Cocos2d-x on Windows 10](https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x Windows 市集遊戲 (影片)</td>
        <td>[Build a Game with Cocos2d-x for Windows Devices](http://www.microsoftvirtualacademy.com/training-courses/build-a-game-with-cocos2d-x-for-windows-devices)</td>
    </tr>
</table>


### Unreal Engine

Unreal Engine 4 是一整套的遊戲開發工具，適合所有類型的遊戲與開發人員採用。 全球的遊戲開發人員都使用 Unreal Engine 來處理對效能要求最高的遊戲主機與電腦遊戲。 訂閱 Unreal Engine 4 的 [DirectX 12 優先使用計畫](#dxeap)成員將可獲得存取權來存取支援 DirectX 12 的 Unreal Engine 4.4 開發專案。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unreal Engine 概觀</td>
        <td>[What is Unreal Engine 4](https://www.unrealengine.com/what-is-unreal-engine-4)</td>
    </tr>
</table>
 

### 中介軟體與合作夥伴

有許多其他可根據您的遊戲開發需求提供解決方案的中介軟體與引擎合作夥伴。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 開發人員中心遊戲合作夥伴</td>
        <td>[Dev Center Partners (Gaming)](https://devcenterpartners.windows.com/directory#filter=gaming)</td>
    </tr>
    <tr>
        <td>Windows 開發人員中心合作夥伴</td>
        <td>[Dev Center Partners](https://devcenterpartners.windows.com/directory)</td>
    </tr>
</table>
 

### 移植您的遊戲

如果您有現有的遊戲，有許多資源與指南可協助您將遊戲快速轉移到 UWP。 若要啟動您的移植工作，您也可以考慮使用[通用 Windows 平台橋接](#uwp_bridges)。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>將 Windows 8 應用程式移植到通用 Windows 平台 app</td>
        <td>[Move from Windows Runtime 8.x to UWP](https://msdn.microsoft.com/library/windows/apps/mt238322)</td>
    </tr>
    <tr>
        <td>將 Windows 8 應用程式移植到通用 Windows 平台 app (影片)</td>
        <td>[Porting 8.1 Apps to Windows 10](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21)</td>
    </tr>
    <tr>
        <td>將 iOS app 移植到通用 Windows 平台 app</td>
        <td>[Move from iOS to UWP](https://msdn.microsoft.com/library/windows/apps/mt238320)</td>
    </tr>
    <tr>
        <td>將 Silverlight 應用程式移植到通用 Windows 平台 app</td>
        <td>[Move from Windows Phone Silverlight to UWP](https://msdn.microsoft.com/library/windows/apps/mt238323)</td>
    </tr>
    <tr>
        <td>將 XAML 或 Silverlight 移植到通用 Windows 平台 app (影片)</td>
        <td>[Porting an App from XAML or Silverlight to Windows 10](https://channel9.msdn.com/Events/Build/2015/3-741)</td>
    </tr>
    <tr>
        <td>將 Xbox 遊戲移植到通用 Windows 平台 app</td>
        <td>[Porting from Xbox One to Windows 10 UWP](https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)</td>
    </tr>
    <tr>
        <td>從 DirectX 9 移植到 DirectX 11</td>
        <td>[Port from DirectX 9 to Universal Windows Platform (UWP)](porting-your-directx-9-game-to-windows-store.md)</td>
    </tr>
    <tr>
        <td>從 Direct3D 11 移植到 Direct3D 12</td>
        <td>[Porting from Direct3D 11 to Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709)</td>
    </tr>
    <tr>
        <td>從 OpenGL ES 移植到 Direct3D 11</td>
        <td>[Port from OpenGL ES 2.0 to Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)</td>
    </tr>
    <tr>
        <td>使用 ANGLE 將 OpenGL ES 轉譯成 Direct3D 11</td>
        <td>[ANGLE](http://go.microsoft.com/fwlink/p/?linkid=618387)</td>
    </tr>
    <tr>
        <td>UWP 中的傳統型 Windows API 對應項</td>
        <td>[Alternatives to Windows APIs in Universal Windows Platform (UWP) apps](https://msdn.microsoft.com/library/windows/apps/hh464945)</td>
    </tr>
</table>


## 原型與設計


既然您已決定所要建立的遊戲類型，以及將用來建置遊戲的工具與圖形技術，您便已準備好開始進行設計和製作原型。 您遊戲的核心是一個通用 Windows 平台 app，因此這就是您的起始點。

### 通用 Windows 平台 (UWP) 簡介

Windows 10 導入了「通用 Windows 平台」(UWP)，此平台提供一個跨所有 Windows 10 裝置的共同 API 平台。 UWP 發展及擴充了「Windows 執行階段」模型，並將它打造成一個緊密結合、統一的核心。 以 UWP 為目標的遊戲可以呼叫所有裝置通用的 WinRT API。 由於 UWP 提供一個保證的核心 API 層，因此您可以選擇建立一個將會在所有 Windows 10 裝置安裝的單一應用程式套件。 而且如果您想要的話，您的遊戲仍然可以呼叫其執行所在之裝置特定的 API (包括一些來自 Win32 和 .NET 的傳統型 Windows API)。

UWP 的目標是要擁有：

-   一個核心作業系統
-   一個應用程式平台
-   一個遊戲社交網路
-   一個市集
-   一個擷取路徑

以下是詳細討論通用 Windows 平台 app 的出色指南，建議閱讀這些指南來協助您了解平台。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台 app 簡介</td>
        <td>[What's a Universal Windows Platform app?](https://msdn.microsoft.com/library/windows/apps/dn726767)</td>
    </tr>
    <tr>
        <td>UWP 概觀</td>
        <td>[Guide to UWP apps](https://msdn.microsoft.com/library/windows/apps/dn894631)</td>
    </tr>
</table>
 

### UWP 開發入門

設定並準備好開發通用 Windows 平台 app 是相當快速且輕鬆的程序。 下列指南將引導您逐步完成這個程序。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 開發入門</td>
        <td>[Get started with Windows apps](https://dev.windows.com/getstarted)</td>
    </tr>
    <tr>
        <td>開始 UWP 開發設定</td>
        <td>[Get set up](https://msdn.microsoft.com/library/windows/apps/dn726766)</td>
    </tr>
</table>

如果您是 UWP 程式設計的「初學者」，並且正考慮在您的遊戲中使用 XAML (請參閱[選擇您的圖形技術與程式設計語言](#choosing_technology))， 則[初學者的 Windows 10 開發入門](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)影片系列是一個很好的起始點。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>使用 XAML 進行 Windows 10 開發的初學者指南 (影片系列)</td>
        <td>[Windows 10 development for absolute beginners](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)</td>
    </tr>
    <tr>
        <td>發佈使用 XAML 的 Windows 10 初學者系列 (部落格文章)</td>
        <td>[Windows 10 development for absolute beginners](http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/)</td>
    </tr>
</table>

### UWP 開發概念

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台 app 開發概觀</td>
        <td>[Develop Windows apps](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>UWP 的網路程式設計概觀</td>
        <td>[Networking and web services](https://msdn.microsoft.com/library/windows/apps/mt280378)</td>
    </tr>
    <tr>
        <td>在遊戲中使用 Windows.Web.HTTP 與 Windows.Networking.Sockets</td>
        <td>[Networking for games](work-with-networking-in-your-directx-game.md)</td>
    </tr>
    <tr>
        <td>UWP 的非同步程式設計概念</td>
        <td>[Asynchronous programming](https://msdn.microsoft.com/library/windows/apps/mt187335)</td>
    </tr>
</table>
 

### 處理程序生命週期管理

處理程序生命週期管理 (或應用程式週期) 說明通用 Windows 平台 app 可經歷轉換的各種啟用狀態。 您的遊戲可以被啟用、暫停、繼續或終止，並且可以透過各種方式經歷這些狀態的轉換。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>處理應用程式週期轉換</td>
        <td>[App lifecycle](https://msdn.microsoft.com/library/windows/apps/mt243287)</td>
    </tr>
    <tr>
        <td>使用 Microsoft Visual Studio 觸發應用程式轉換</td>
        <td>[How to trigger suspend, resume, and background events for Windows Store apps in Visual Studio](https://msdn.microsoft.com/library/hh974425.aspx)</td>
    </tr>
</table>
 

### 設計遊戲 UX

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
        <td>[Designing UWP apps](https://dev.windows.com/design)</td>
    </tr>
    <tr>
        <td>針對 app 週期狀態進行設計</td>
        <td>[UX guidelines for launch, suspend, and resume](https://msdn.microsoft.com/library/windows/apps/dn611862)</td>
    </tr>
    <tr>
        <td>以多種裝置尺寸規格為目標 (影片)</td>
        <td>[Designing Games for a Windows Core World](http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World)</td>
    </tr>
</table>
 

### 色彩指南與調色盤

在您的遊戲中遵循一致的色彩指導方針除了可增加美學質感、協助瀏覽，還是一個可協助玩家使用功能表與 HUD 功能的強大手段。 讓遊戲元素 (例如警告、損毀、XP 及成就) 採用一致的色調可使 UI 更清晰，而降低使用明確標籤的需求。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>色彩指南</td>
        <td>[Best Practices: Color](https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip)</td>
    </tr>
</table>
 

### 印刷樣式

適當使用印刷樣式可增強遊戲的許多方面，包括 UI 版面配置、導覽、可讀性、氛圍、品牌及玩家沉浸度。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>印刷樣式指南</td>
        <td>[Best Practices: Typography](http://go.microsoft.com/fwlink/?LinkId=535007)</td>
    </tr>
</table>
 

### UI 對應

UI 對應是以流程圖方式呈現的遊戲導覽及功能表版面配置。 UI 對應有助於讓所有相關的專案關係人了解遊戲的介面與導覽路徑，而可以在開發週期中早日揭露潛在的障礙與無法解決的問題。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UI 對應指南</td>
        <td>[Best Practices: UI Map](http://go.microsoft.com/fwlink/?LinkId=535008)</td>
    </tr>
</table>
 

### DirectX 開發

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 上的 DirectX 遊戲開發</td>
        <td>[Games and DirectX](index.md)</td>
    </tr>
    <tr>
        <td>DirectX 與 UWP app 模型的互動</td>
        <td>[The app object and DirectX](about-the-metro-style-user-interface-and-directx.md)</td>
    </tr>
    <tr>
        <td>DirectX 概觀與參考</td>
        <td>[DirectX Graphics and Gaming](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 程式設計指南與參考</td>
        <td>[Direct3D 12 Graphics](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>DirectX 12 基礎 (影片)</td>
        <td>[Better Power, Better Performance: Your Game on DirectX 12](http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12)</td>
    </tr>
</table>
 

「DirectX 工具組」、DirectX 紋理處理程式庫和 DirectXMesh 幾何處理程式庫提供紋理、網格、精靈及其他公用程式功能與協助程式類別來進行 DirectX 開發。 與您自己實作這些功能相比，這些程式庫可以節省很多時間和精力。 雖然這些程式庫主要是針對 Direct3D 11 進行實作，但某些部分在 Direct3D 12 中也可以運作。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>取得 DirectX 工具組 (DirectX 11)</td>
        <td>[DirectXTK](http://go.microsoft.com/fwlink/?LinkId=248929)</td>
    </tr>
    <tr>
        <td>取得 DirectX 紋理處理程式庫 (DirectX 11)</td>
        <td>[DirectXTex](http://go.microsoft.com/fwlink/?LinkId=248926)</td>
    </tr>
    <tr>
        <td>取得 DirectXMesh 幾何處理程式庫</td>
        <td>[DirectXMesh](http://go.microsoft.com/fwlink/?LinkID=324981)</td>
    </tr>
    <tr>
        <td>DirectXTK 中的 Direct3D 12 支援 (部落格文章)</td>
        <td>[Support for DirectX 12](https://github.com/Microsoft/DirectXTK/issues/2)</td>
    </tr>
</table>


## 製作


您的工作室現在已完全進入並移到製作階段，工作已散佈至整個團隊。 您正在潤飾、重構及延伸原型，以將它精心製作成完整的遊戲。

### 通知與動態磚

磚是您遊戲在 [開始] 功能表上的呈現形式。 磚與通知可以讓玩家感興趣，即使他們目前並沒有玩您的遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>開發磚與徽章</td>
        <td>[Tiles, badges, and notifications](https://msdn.microsoft.com/library/windows/apps/mt185606)</td>
    </tr>
    <tr>
        <td>說明動態磚與通知的範例</td>
        <td>[Notifications sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)</td>
    </tr>
    <tr>
        <td>彈性磚範本 (部落格文章)</td>
        <td>[Adaptive Tile Templates - Schema and Documentation](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx)</td>
    </tr>
    <tr>
        <td>設計磚與徽章</td>
        <td>[Guidelines for tiles and badges](https://msdn.microsoft.com/library/windows/apps/hh465403)</td>
    </tr>
    <tr>
        <td>適用於以互動方式開發動態磚範本的 Windows 10 應用程式</td>
        <td>[Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)</td>
    </tr>
</table>
 

### 啟用應用程式內產品 (IAP) 購買

IAP (應用程式內產品) 是玩家可在遊戲內購買的補充項目。 IAP 可以是新的附加元件、遊戲關卡、項目或您玩家可能喜歡的任何其他東西。 如果使用得當，IAP 便可既改善遊戲體驗，又提供收益。 您需透過「Windows 開發人員中心」儀表板來定義和發行您遊戲的 IAP， 並且需在遊戲程式碼中啟用 App 內購買。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>耐久性應用程式內產品</td>
        <td>[Enable in-app product purchases](https://msdn.microsoft.com/library/windows/apps/mt219684)</td>
    </tr>
    <tr>
        <td>消費性應用程式內產品</td>
        <td>[Enable consumable in-app product purchases](https://msdn.microsoft.com/library/windows/apps/mt219683)</td>
    </tr>
    <tr>
        <td>應用程式內產品詳細資料與提交</td>
        <td>[IAP submissions](https://msdn.microsoft.com/library/windows/apps/mt148551)</td>
    </tr>
    <tr>
        <td>監視您遊戲的 IAP 銷售與人口統計</td>
        <td>[IAP acquisitions report](https://msdn.microsoft.com/library/windows/apps/mt148538)</td>
    </tr>
</table>
 

### 進階 DirectX 技術與概念

某些 DirectX 開發部分可以是相當微妙且複雜的。 當您進入製作階段而需要深入探討 DirectX 引擎細節，或對困難的效能問題進行偵錯時， 本節中的資源與資訊將可為您提供協助。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>將圖形與效能最佳化 (影片)</td>
        <td>[Advanced DirectX 12 Graphics and Performance](http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance)</td>
    </tr>
    <tr>
        <td>DirectX 圖形偵錯 (影片)</td>
        <td>[Solve the Tough Graphics Problems with Your Game Using DirectX Tools](http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools)</td>
    </tr>
    <tr>
        <td>Direct3D 12 程式設計指南</td>
        <td>[Direct3D 12 Programming Guide](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>結合 DirectX 與 XAML</td>
        <td>[DirectX and XAML interop](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

Microsoft DirectX 產品小組製作了一系列有關 DirectX 12 開發的深入影片。 它們涵蓋資源繫結、呈現模式、偵錯、資源障礙及許多其他 DirectX 12 概念的詳細資料。 這個系列有時也邀請來賓擔任主講。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>圖形與 DirectX 12 開發影片 (YouTube 頻道)</td>
        <td>[Microsoft DirectX 12 and Graphics Education](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
    <tr>
        <td>Direct3D 12 資源與堆積管理 (影片)</td>
        <td>[Heaps and Resources in DirectX 12](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
</table>
 

### 全球化與當地語系化

開發 Windows 上全球適用的遊戲，並了解 Microsoft 暢銷產品中的國際功能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>讓您的遊戲為全球市場做好準備</td>
        <td>[Guidelines when developing for a global audience](https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx)</td>
    </tr>
    <tr>
        <td>橋接語言、文化及技術</td>
        <td>[Online resource for language conventions and standard Microsoft terminology](http://www.microsoft.com/Language/Default.aspx)</td>
    </tr>
</table>


## 提交及發行您的遊戲


下列指南與資訊可協助讓發行及提交程序儘可能順利。

### 封裝及上傳

您將使用新的整合式「Windows 開發人員中心」儀表板來發行及管理您的遊戲套件。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 開發人員中心應用程式發行</td>
        <td>[Publish Windows apps](https://dev.windows.com/publish)</td>
    </tr>
    <tr>
        <td>為您的遊戲分級 (部落格文章)</td>
        <td>[Single workflow to assign age ratings using IARC system](https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/)</td>
    </tr>
    <tr>
        <td>封裝您的遊戲</td>
        <td>[Package your UWPDirectX game](package-your-windows-store-directx-game.md)</td>
    </tr>
    <tr>
        <td>以合作廠商開發人員的身分封裝您的遊戲 (部落格文章)</td>
        <td>[Create uploadable packages without publisher's store account access](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/)</td>
    </tr>
    <tr>
        <td>上傳及設定您遊戲的版本</td>
        <td>[Upload app packages](https://msdn.microsoft.com/library/windows/apps/mt148542)</td>
    </tr>
</table>
 

### 原則與認證

請勿讓認證問題延遲您的遊戲發行。 以下是需注意的原則與常見的認證問題。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 市集應用程式開發人員合約</td>
        <td>[App Developer Agreement](https://msdn.microsoft.com/library/windows/apps/hh694058)</td>
    </tr>
    <tr>
        <td>在 Windows 市集中發佈應用程式的原則</td>
        <td>[Windows Store Policies](https://msdn.microsoft.com/library/windows/apps/dn764944)</td>
    </tr>
    <tr>
        <td>如何避免一些常見的應用程式認證問題</td>
        <td>[Avoid common certification failures](https://msdn.microsoft.com/library/windows/apps/jj657968)</td>
    </tr>
</table>
 

### 市集資訊清單 (StoreManifest.xml)

市集資訊清單 (StoreManifest.xml) 是一個選用的組態檔，可以包含在您的應用程式套件中。 市集資訊清單提供不屬於 AppxManifest.xml 檔案一部分的額外功能。 例如，您可以使用市集資訊清單，在目標裝置不具備指定的最低 DirectX 功能層級或指定的最低系統記憶體時，阻止安裝您的遊戲。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>市集資訊清單結構描述</td>
        <td>[StoreManifest schema (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335)</td>
    </tr>
</table>
 

## 遊戲生命週期管理


在完成遊戲開發並傳送您的遊戲之後，還不算「遊戲結束」。 您可能已完成第一版的開發，但是您的遊戲在市場上的旅程才剛剛開始。 您將會想要監視使用狀況和錯誤報告、回應使用者的意見反應，以及發行遊戲更新。

### Windows 開發人員中心分析與推銷

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 開發人員中分析</td>
        <td>[Analytics](https://msdn.microsoft.com/library/windows/apps/mt148522)</td>
    </tr>
    <tr>
        <td>回應客戶評論</td>
        <td>[Respond to customer reviews](https://msdn.microsoft.com/library/windows/apps/mt148546)</td>
    </tr>
    <tr>
        <td>推銷您遊戲的方式</td>
        <td>[Promote your apps](https://dev.windows.com/store-promotion)</td>
    </tr>
</table>
 

### Visual Studio Application Insights

Visual Studio Application Insights 可為您已發行的遊戲提供效能、遙測及使用狀況分析。 Application Insights 可協助您在發行遊戲之後偵測及解決問題、持續監視及改善使用狀況，以及了解玩家與您遊戲的持續互動情況。 Application Insights 的運作方式是將 SDK 新增到您的應用程式中，以將遙測數據傳送給 [Azure 入口網站](http://portal.azure.com/)。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>應用程式效能與使用狀況分析</td>
        <td>[Visual Studio Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)</td>
    </tr>
    <tr>
        <td>在 Windows 應用程式中啟用 Application Insights</td>
        <td>[Application Insights for Windows Phone and Store apps](https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/)</td>
    </tr>
</table>
 

### 建立及管理內容更新

若要更新您已發行的遊戲，請提交具有較新版本號碼的新應用程式套件。 在套件完成提交與認證程序之後，就會自動以更新的形式提供給客戶。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>更新及設定您遊戲的版本</td>
        <td>[Package version numbering](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
    <tr>
        <td>遊戲套件管理指導方針</td>
        <td>[Guidance for app package management](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
</table>


## 將 Xbox Live 新增到您的遊戲


> **注意：**管理 Xbox Live 開發時，是透過 ID@Xbox 與 Microsoft Studios 這類計畫來管理。 本指南涵蓋的資源範圍很廣，因此視您所參與的計畫或您的特定開發角色而定，您可能會發現有些資源無法使用。 解析成 developer.xboxlive.com、forums.xboxlive.com、xdi.xboxlive.com 或「遊戲開發人員網路」(GDN) 的連結就是其中幾例。 如需有關與 Microsoft 合作的資訊，請參閱[開發人員計畫](#programs)。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>下載最新的 Xbox Live SDK</td>
        <td>[Xbox Live SDK](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>將 Xbox Live 新增到您的通用 Windows 平台 app</td>
        <td>[How to - Add Xbox Live SDK to Universal Windows Platform (UWP) Apps](http://aka.ms/xsapi2uwp)</td>
    </tr>
    <tr>
        <td>使用 Xbox Live 遊戲的需求</td>
        <td>[Xbox Requirements for Xbox Live on Windows 10](http://go.microsoft.com/fwlink/?LinkId=533217)</td>
    </tr>
    <tr>
        <td>Xbox Live 遊戲開發概觀 (影片)</td>
        <td>[Developing with Xbox Live for Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10)</td>
    </tr>
    <tr>
        <td>跨平台配對 (影片)</td>
        <td>[Xbox Live Multiplayer: Introducing services for cross-platform matchmaking and gameplay](http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay)</td>
    </tr>
    <tr>
        <td>「神鬼寓言：傳奇」(Fable Legends) 中的跨裝置遊戲 (影片)</td>
        <td>[Fable Legends: Cross-device Gameplay with Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live)</td>
    </tr>
    <tr>
        <td>Xbox Live 統計資料與成就 (影片)</td>
        <td>[Best Practices for Leveraging Cloud-Based User Stats and Achievements in Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live)</td>
    </tr>
</table>
 

## 其他資源

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>獨立製作遊戲開發 (影片)</td>
        <td>[New Opportunities for Independent Developers](http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers)</td>
    </tr>
    <tr>
        <td>多核心行動裝置的考量 (影片)</td>
        <td>[Sustained Gaming Performance in multi-core mobile devices](http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices)</td>
    </tr>
    <tr>
        <td>開發 Windows 10 傳統型遊戲 (影片)</td>
        <td>[PC Games for Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10)</td>
    </tr>
</table>



 

 

 


<!--HONumber=Mar16_HO4-->


