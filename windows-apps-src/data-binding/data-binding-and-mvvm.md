---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 資料繫結和 MVVM
description: 資料繫結是 Model-View-ViewModel (MVVM) UI 架構設計模式的核心，提供了 UI 和非 UI 程式碼之間的鬆散結合。
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63798102"
---
# <a name="data-binding-and-mvvm"></a>資料繫結和 MVVM

Model-View-ViewModel (MVVM) 是分離 UI 和非 UI 程式碼的 UI 架構設計模式。 使用 MVVM 時，您可以在 XAML 中以宣告方式定義 UI，並使用資料繫結標記將 UI 連結到包含資料和命令的其他圖層。 資料繫結基礎結構提供鬆散結合，讓 UI 和連結的資料保持同步，並將使用者輸入路由傳送至適當的命令。 

因為鬆散結合，所以使用資料繫結會減少不同種類程式碼之間的硬式相依。 這可讓您更輕鬆地變更個別程式碼單元 (方法、類別、控制項等)，而不會在其他單元中造成非預期的副作用。 這種分離是「關注點分離」  的範例，是許多設計模式中的重要概念。 

## <a name="benefits-of-mvvm"></a>MVVM 的優點

分離您的程式碼有許多優點，包括：

* 能夠採用反覆性探索性的程式碼撰寫方式。 隔離的變更風險較低，而且更容易進行實驗。
* 簡化單元測試。 彼此隔離的程式碼單元可以在生產環境之外個別進行測試。
* 支援小組共同作業。 遵守妥善設計介面的分離程式碼可以由不同的個人或小組開發，並於稍後整合。
* 改善維護性。 在分離的程式碼中修正錯誤，比較不可能會造成其他程式碼退化。

相較於 MVVM，具有更傳統「程式碼後置」結構的應用程式通常會在僅限顯示的資料使用資料繫結，並以回應使用者輸入的方式直接處理控制項公開的事件。 事件處理常式是在程式碼後置檔案 (例如 MainPage.xaml.cs) 中執行，而且通常會與控制項緊密結合，常包含直接操控 UI 的程式碼。 這使得您若不更新事件處理常式的程式碼，就很難或無法取代控制項。 使用此架構時，程式碼後置檔案通常會累積與 UI 不直接相關的程式碼，例如資料庫存取程式碼，且最後為了與其他頁面搭配使用就要複製和修改。

## <a name="app-layers"></a>應用程式各層

使用 MVVM 模式時，應用程式會分成下列各層：

* **模型**層定義類型，用來代表您的商務資料。 這包括建立核心應用程式域模型所需的所有東西，而且通常包含核心應用程式邏輯。 這一層完全獨立於「檢視」和「檢視-模型」層，而且通常部分在雲端。 有了完全實作的模型層，您可以建立多個不同的用戶端應用程式 (如果您選擇這麼做)，例如 UWP、使用相同基礎資料的 Web 應用程式。
* **檢視**層使用 XAML 標記來定義 UI。 標記包含的資料繫結運算式 (例如 [x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) 可定義特定 UI 元件和各種「檢視-模型」和模型成員之間的連線。 程式碼後置檔案有時會當做檢視層的一部分使用，以包含自訂或操作 UI 所需的額外程式碼，或是在呼叫執行工作的「檢視-模型」方法之前，從事件處理常式引數中解壓縮資料。 
* **檢視-模型**層提供檢視的資料繫結目標。 在許多情況下，檢視-模型會直接公開模型，或提供包裝特定模型成員的成員。 檢視-模型也可以定義成員，以便追蹤與 UI 相關但與模型無關的資料，例如項目清單的顯示順序。 檢視-模型也可做為與其他服務 (例如資料庫存取程式碼) 的整合點。 就簡單的專案來說，您可能不需要個別的模型層，只需要會封裝您所需之所有資料的檢視-模型層。 

## <a name="basic-and-advanced-mvvm"></a>基本和進階 MVVM

就像任何設計模式一樣，有多種方式可以實作 MVVM，且許多不同的技術會被視為 MVVM 的一部分。 因此，有數個不同的協力廠商 MVVM 架構支援各種以 XAML 為基礎的平台，包括 UWP。 不過，這些架構通常包含多個服務來實作分離的架構，使得 MVVM 的確切定義變得有點模糊。 

雖然精密的 MVVM 架構很有用，特別是針對企業規模的專案，但通常會伴隨採用任何特定模式或技術的相關成本，而且優點不一定清楚，視您的專案規模和大小而定。 幸運的是，您可以只採用能提供清楚明確優點的技術，略過其他技術，直到您需要它們。 

特別是，您只要瞭解並套用資料繫結的完整功能，並將您的應用程式邏輯分成稍早所述的各層，即可獲得很大的好處。 這只要使用 Windows SDK 所提供的功能就能達成，不需任何外部架構。 尤其是，[{x:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)讓資料繫結比之前在 XAML 平台中更容易、執行更高效能，不再像以前一樣需要許多模板程式碼。

如需使用基本、現成 MVVM 的更多指引，請參閱 GitHub 上的[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 許多其他 [UWP 應用程式範例](https://github.com/Microsoft?q=windows-appsample
)也使用基本的 MVVM 架構，[流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)同時包含程式碼後置和 MVVM 版本，並附上描述 [MVVM 轉換](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md)的筆記。 

## <a name="see-also"></a>另請參閱

### <a name="topics"></a>主題

[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>範例

[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 清查範例](https://github.com/Microsoft/InventorySample)  
[流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)  
