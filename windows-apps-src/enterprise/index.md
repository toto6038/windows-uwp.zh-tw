---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: 此藍圖概述適用於 Windows 10&\#160; 通用 Windows 平台 (UWP) 應用程式的重要企業功能。
title: 企業
author: awkoren
---

# 企業


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

此藍圖概述適用於 Windows 10 通用 Windows 平台 (UWP) app 的重要企業功能。 Windows 10 可讓您只需要撰寫一次，就能部署到所有裝置，方法是建立一個可調整成適合任何裝置的 app。 這可讓您建置使用者期待的絕佳經驗，並針對組織所需的安全性、管理以及設定等提供控制功能。

**注意** 本文的對象是撰寫企業 UWP app 的開發人員。 針對一般 UWP 開發，請參閱 [Windows 10 app 使用方法指南](https://msdn.microsoft.com/library/windows/apps/mt244352)。 針對 WPF、Windows Form 或 Win32 開發，請瀏覽[傳統型開發人員中心](https://dev.windows.com/en-us/desktop)。 針對 IT 專業人員資源 (例如部署 Windows 10 或管理企業安全性功能)，請參閱 [TechNet 上的 Windows 10](https://msdn.microsoft.com/library/dn986868)。

 

## 安全性


Windows 10 提供一套安全性功能，讓 app 開發人員保護其使用者的身分識別、公司網路的安全性以及裝置上儲存的任何商務資料。 Windows 10 的新增功能是 Microsoft Passport，一種可使用 PIN 或 Windows Hello 存取的易部署雙因素密碼替代方式，可提供企業級安全性，以及支援指紋、臉部和虹膜辨識。

| 主題 | 描述 |
|-------|-------------|
| [安全開發 Windows app 的簡介](https://msdn.microsoft.com/library/windows/apps/mt622741) | 這篇簡介文章說明不同驗證階段 (包括傳輸中資料和靜態資料) 的各種 Windows 安全功能。 它也描述如何將這些階段整合到您的 app。 本文涵蓋大範圍的主題，主要目的是協助 app 設計人員更充分地了解可快速且輕易地建立通用 Windows 平台 app 的 Windows 功能。 |
| [驗證和使用者識別](https://msdn.microsoft.com/library/windows/apps/mt270184) | UWP app 有本文所述的數個使用者驗證選項。 若要用於企業，則強烈建議選用新的 Microsoft Passport 功能。 Microsoft Passport 以增強式雙因素驗證 (2FA) 取代密碼，方法是驗證現有的認證，以及建立以生物識別或 PIN 式使用者手勢所保護的裝置特定認證，以產生方便且高度安全的使用經驗。 |
| [密碼編譯](https://msdn.microsoft.com/library/windows/apps/mt270191) | 密碼編譯一節概述 UWP app 所提供的密碼編譯功能。 文章的範圍包括如何輕鬆地加密機密商業資料的簡介逐步解說，到操作密碼編譯金鑰以及使用 MAC、雜湊和簽章這類進階主題。 |
| [企業資料保護 (EDP)](edp-hub.md) | 這是中樞主題，涵蓋企業資料保護 (EDP) 與檔案的關聯、緩衝區、剪貼簿、網路、背景工作以及鎖定時的資料保護的完整開發人員描述。 |

 

## 資料繫結和資料庫


資料繫結可讓您的 app UI 顯示來自外部來源 (例如資料庫) 的資料，以及選擇性地與該資料保持同步。 資料繫結可讓您將資料與 UI 分開考量，為 app 建構更簡單的概念模型，以及更好的可讀性、測試性和維護性。

| 主題 | 描述 |
|-------|-------------|
| [資料繫結概觀](https://msdn.microsoft.com/library/windows/apps/mt269383) | 本主題示範如何在通用 Windows 平台 (UWP) app 中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目控制項繫結到項目集合。 此外，還會示範如何控制項目的呈現、根據選擇來實作詳細資料檢視，以及轉換資料以供顯示。 |
| [Entity Framework 7 for UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | 對大型資料集執行複雜查詢，可使用支援 UWP 的 Entity Framework 7 進行大幅簡化。 在這個逐步解說中，您將建置 UWP app，以使用 Entity Framework 對本機 SQLite 資料庫執行基本資料存取。 |
| [SQLite 本機資料庫](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 這個影片是使用 SQLite 的完整開發人員指南，而 SQLite 是本機 app 資料庫的建議方案。 請瀏覽 [SQLite](https://www.sqlite.org/download.html) 以下載 UWP 的最新版本，或使用 Windows 10 SDK 已隨附的版本。 |

 

## 網路功能和資料序列化


企業營運 app 通常需要與各種其他系統通訊，或在各種其他系統上儲存資料。 做法通常是連線到網路服務 (使用 REST 或 SOAP 這類通訊協定)，然後序列化資料或將資料還原序列化成通用格式。 在與 WPF、WinForm 和 ASP.NET 應用程式類似的 UWP app 中，使用網路和資料序列化。 如需詳細資訊，請參閱下列文章。

| 主題 | 描述 |
|-------|-------------|
| [網路功能基本知識](https://msdn.microsoft.com/library/windows/apps/mt280233) | 這個逐步解說說明與所有 UWP app 相關的基本網路概念，而不管使用中的通訊協定為何。  |
| [哪一種網路功能技術？](https://msdn.microsoft.com/library/windows/apps/mt280235) | 適用於 UWP app 的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。 |
| [XML 和 SOAP 序列化](https://msdn.microsoft.com/library/90c86ass.aspx) | XML 序列化會將物件轉換成符合特定 XML 結構描述定義語言 (XSD) 的XML 資料流。 若要在 XML 與強型別類別之間進行轉換，您可以使用原生 [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) 類別或外部程式庫。 |
| [JSON 序列化](https://msdn.microsoft.com/library/windows/apps/br240639) | JSON (JavaScript 物件標記法) 序列化是與 REST API 進行通訊的常用格式。 UWP app 完全支援的 [Newtonsoft Json.NET](http://www.newtonsoft.com/json)。 |

 

## 裝置


為了與企業營運工具 (如印表機、條碼掃描器或智慧卡讀卡機) 整合，您可能會發現有必要將外部裝置或感應器與您的 app 整合。 以下是一些功能範例，您可以使用本節所描述的技術將這些功能新增到您的 app 中。

| 主題  | 描述 |
|--------|-------------|
| [列舉裝置](https://msdn.microsoft.com/library/windows/apps/mt187355) | 本文說明如何使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空間尋找內部連接到系統、外部連接或者可透過無線或網路通訊協定偵測到的裝置。 如果您是建置任何與裝置搭配運作的 app，請從這裡開始。 |
| [列印和掃描](https://msdn.microsoft.com/library/windows/apps/mt204544) | 描述如何從您的 app 進行列印和掃描，包括連接和使用企業裝置，例如銷售點 (POS) 系統、收據印表機，以及高容量送紙器掃描器。 |
| [藍牙](https://msdn.microsoft.com/library/windows/apps/mt270288) | 除了使用傳統藍牙連線來傳送和接收資料或控制裝置，Windows 10 還可使用藍牙低功耗 (BTLE) 在背景傳送或接收指標。 使用這個項目，以在使用者接近或離開特定位置時顯示通知或啟用功能。 |
| [企業共用存放裝置](enterprise-shared-storage.md) | 在裝置鎖定案例中，了解如何在相同的應用程式內、應用程式執行個體之間或甚至應用程式之間共用資料。 |

 

## 裝置目標


許多使用者現在都會將他們的手機或平板電腦帶去上班，而手機或平板電腦的板型規格和螢幕大小都不同。 您可以使用通用 Windows 平台 (UWP) 撰寫在所有不同類型的裝置上流暢執行的單一企業營運 app (包括桌上型電腦電腦和 PPI 顯示器)，可讓您最接近 app 並將程式碼效率提到最高。

| 主題 | 描述 |
|-------|-------------|
| [UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631) | 在本簡介指南中，您將了解 Windows 10UWP 平台，包括︰裝置系列為何以及如何決定要設為目標的裝置系列、新的 UI 控制項和面板以讓您將 UI 調整為不同的裝置板型規格，以及如何了解與控制可供您的 app 使用的 API 表面。 |
| [彈性 XAML UI 程式碼範例](http://go.microsoft.com/fwlink/p/?LinkId=619992) | 這個程式碼範例示範您 app 的所有可能版面配置選項和控制項 (不論裝置類型為何)，並可讓您與面板互動以顯示如何達成您要尋找的任何版面配置。 除了顯示每個控制項如何回應不同的板型規格之外，app 本身也具有回應，並顯示達成彈性 UI 的各種方法。 |

 

## 部署


您有許多將 app 發佈至組織使用者的選項。 您可以使用商務用 Windows 市集或現有行動裝置管理，也可以將 app 側載至裝置。 您也可藉由發佈到 Windows 市集，將您的 app 提供給一般大眾。

| 主題 | 描述 |
|-------|-------------|
| [將 LOB app 發佈到企業](https://msdn.microsoft.com/library/windows/apps/mt608995) | 您可以透過商務用 Windows 市集，將企業營運 app 直接發佈到企業來進行大量取得，而不需要讓大眾廣泛取得 app。 |
| [側載 app](https://technet.microsoft.com/en-us/library/mt269549) | 當您側載 app 時，您要將簽署的 app 套件部署到裝置。 您要維護這些 app 的簽署、裝載和部署。 用於側載 app 的程序已經簡化成適用於 Windows 10。             |
| [將 app 發佈到 Windows 市集](https://dev.windows.com/publish) | 整合的 Windows 市集可讓您發佈與管理您為所有 Windows 裝置開發的所有 app。 透過每個市場價格、發佈和可見性控制項，以及其他選項來自訂您 app 的可用性。 |

 

## 模式和做法


大型企業級 app 的程式碼庫會變得雜亂無章。 Prism 是一個架構，用於在 WPF、Windows 10 UWP 和 Xamarin Forms 中建置鬆散結合、可維護和可測試的 XAML 應用程式。 Prism 提供一組設計模式的實作，這組設計模式有助於撰寫結構完善且可維護的 XAML 應用程式，包括 MVVM、相依性導入、命令、EventAggregator 和其他項目。

如需 Prism 的詳細資訊，請參閱 [GitHub 存放庫](https://github.com/PrismLibrary/Prism)。

 

 





<!--HONumber=May16_HO2-->


