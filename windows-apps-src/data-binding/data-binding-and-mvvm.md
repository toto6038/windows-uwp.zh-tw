---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 資料繫結和 MVVM
description: 資料繫結的模型-檢視-ViewModel (MVVM) UI 架構設計模式，核心，可 UI 和非 UI 程式碼之間的鬆散結合。
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740346"
---
# <a name="data-binding-and-mvvm"></a>資料繫結和 MVVM

模型-檢視-ViewModel (MVVM) 是聯繫 UI 和非 UI 程式碼 UI 架構設計模式。 在 MVVM 中，使用您在 XAML 中以宣告方式定義您的 UI，並使用資料繫結標記將其連結至包含資料和命令其他層。 資料繫結基礎結構提供保持 UI 鬆散結合和連結的資料同步處理，並將路由到適當的命令的使用者輸入。 

因為它提供鬆散結合，使用資料繫結可以減少不同種類的程式碼之間的硬碟相依性。 這可讓您輕鬆變更個別的程式碼單位 （方法、 類別、 控制項等等），而不會造成非預期的副作用其他秒為單位。 聯繫是範例的*區分不同的考量*，這是許多設計模式中的一個重要概念。 

## <a name="benefits-of-mvvm"></a>MVVM 的優點

聯繫您的程式碼有許多好處，包括：

* 啟用反覆、 探的程式碼撰寫樣式。 隔離的變更會是較不會有風險，且更容易地進行實驗。
* 簡化的單元測試。 個別和生產環境之外，程式碼的單位之間彼此隔離，可以進行測試。
* 支援小組共同作業。 分離遵守精心設計介面的程式碼可以開發的個別個人或團隊中，並稍後整合。
* 改善可維護性。 修正 bug 分離的程式碼中是較不可能會造成其他程式碼中的迴歸。

相較於 MVVM，具有更傳統的 「 程式碼後置 」 結構的應用程式通常會使用資料繫結為僅顯示資料，並回應使用者輸入，藉由直接處理控制項所公開的事件。 事件處理常式實作於程式碼後置檔案 （例如 MainPage.xaml.cs)，而且通常緊密結合到控制項，通常包含直接操作 UI 的程式碼。 這可讓您難以或無法取代控制項，而不需要更新的事件處理程式碼。 使用這個架構，程式碼後置檔案通常累積不直接相關的 UI，例如資料庫存取程式碼，最後會被複製和修改以搭配其他頁面的程式碼。

## <a name="app-layers"></a>應用程式層級

使用 MVVM 模式時，應用程式分成下列層級：

* **模型**層定義類型代表您的商務資料。 這包含模型核心應用程式網域，所需的所有項目，並通常包含核心應用程式邏輯。 這個層級是完全獨立的檢視與檢視模型層，以及通常存放在雲端中的部分。 提供完整的實作模型的層級，您可以建立多個不同的用戶端應用程式如果您選擇如此，例如 UWP 和 web 應用程式使用相同的基礎資料。
* **檢視**層級定義 UI 使用 XAML 標記。 在標記包含資料繫結運算式 （例如[X:bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) 為定義特定的 UI 元件和各種不同的檢視模型和模型成員之間的連線。 程式碼後置檔案有時會用於做為檢視層的一部分包含自訂或操作 UI，或呼叫執行工作的檢視模型方法之前，從事件處理常式引數擷取資料所需的額外程式碼。 
* **檢視模型**層提供資料繫結目標檢視。 在許多情況下，檢視模型直接，公開的模型，或提供特定模型成員會換行的成員。 檢視模型也可以定義為追蹤相關的資料的成員的 ui，但不可模型，例如項目清單的顯示順序。 檢視模型也會與其他服務，例如資料庫存取程式碼的整合點。 針對簡單的專案，您可能不需要個別的模型圖層，但僅檢視模型所需的所有資料封裝。 

## <a name="basic-and-advanced-mvvm"></a>基本和進階 MVVM

如同任何設計模式，沒有一個以上的方式來實作 MVVM，而且許多不同的技術都被視為 MVVM 的一部分。 基於這個原因，有數個不同的第三方 MVVM 架構支援不同 XAML 為基礎的平台，包括 UWP。 不過，這些架構通常包含實作分離的架構，讓有點模稜兩可 MVVM 的確切定義多個服務。 

雖然複雜的 MVVM 架構可能是非常有用，尤其是針對縮放比例企業的專案，通常是採用任何特定模式或技術，相關聯的成本和優點並不一定清楚，根據縮放和大小您的專案。 幸運的是，您可以採用只提供清楚和有形項權益，這些技術，直到您需要他們忽略其他人。 

尤其是，您可以取得許多的好處，只需透過了解並套用資料繫結，並將您的應用程式邏輯分開到稍早所述的層級的完整功能。 這可以使用僅由 Windows SDK，且不使用任何外部架構提供的功能，來達成。 尤其是， [{X:bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，可讓資料繫結更容易和較舊版本更高版本執行比先前的 XAML 平台，而無需將大量重複使用程式碼中的所需。

如需使用基本、 的方塊跨 MVVM 的其他指導方針，請查看 GitHub 上的[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 許多其他[UWP app 範例](https://github.com/Microsoft?q=windows-appsample
)也會使用基本的 MVVM 架構，並[流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)包括程式碼後置與 MVVM 版本與描述[MVVM 轉換](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md)的附註。 

## <a name="see-also"></a>請參閱

### <a name="topics"></a>主題

[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>範例

[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 詳細目錄範例](https://github.com/Microsoft/InventorySample)  
[車流量 App 範例](https://github.com/Microsoft/Windows-appsample-trafficapp)  
