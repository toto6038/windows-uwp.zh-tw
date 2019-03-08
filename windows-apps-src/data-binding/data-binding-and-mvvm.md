---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 資料繫結和 MVVM
description: 資料繫結的 Model View ViewModel (MVVM) UI 架構的設計模式中，核心，可讓 UI 和非 UI 程式碼之間的鬆散結合。
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616733"
---
# <a name="data-binding-and-mvvm"></a>資料繫結和 MVVM

Model View ViewModel (MVVM) 是減少 UI 和非 UI 程式碼的 UI 架構的設計模式。 搭配 MVVM，您可以在 XAML 中以宣告方式定義您的 UI，並將它連結至包含資料和命令的其他圖層中使用資料繫結標記。 資料繫結基礎結構提供鬆散的結合，讓 UI 和連結的資料同步處理，並將路由傳送至適當的命令的使用者輸入。 

因為它提供鬆散結合時，使用資料繫結可降低不同類型的程式碼之間的硬式相依性。 這可讓您更輕鬆地變更而不會造成非預期的副作用，在其他單位中的個別程式碼單位 （方法、 類別、 控制項等）。 這項低耦合是舉例*關注點分離*，這是許多設計模式中的重要概念。 

## <a name="benefits-of-mvvm"></a>MVVM 的優點

減少您的程式碼有許多優點，包括：

* 正在啟用反覆、 探勘的程式碼撰寫樣式。 較不具風險且更輕鬆地試驗，就會是隔離的變更。
* 簡化單元測試。 個別或生產環境以外，您可以測試不會彼此隔離的程式碼單位。
* 支援團隊協同作業。 低耦合的程式碼符合設計完善的介面可以由不同人員或小組開發和更新版本整合。
* 改進可維護性。 低耦合的程式碼中修正的 bug 是較不可能導致在其他程式碼中的迴歸。

相較於 MVVM，具有更傳統的 「 程式碼後置 」 結構的應用程式，通常會使用僅顯示資料繫結的資料，並直接處理控制項所公開的事件以回應使用者輸入。 事件處理常式 （例如 MainPage.xaml.cs) 中的程式碼後置檔案中實作，而且通常會緊密結合至控制項，通常包含則會直接操作 UI 的程式碼。 這使得難以或無法取代的控制項，而不需要更新之事件處理常式程式碼。 此架構中，使用程式碼後置檔案通常會累積不直接相關的 UI，例如資料庫存取程式碼，最後會被複製並修改用於其他頁面的程式碼。

## <a name="app-layers"></a>應用程式層級

使用 MVVM 模式時，會將應用程式分成下列各層：

* **模型**層定義的類型，代表您的商務資料。 其中包括建立模型的核心應用程式定義域中，所需的所有項目，以及通常包含核心應用程式邏輯。 此圖層是完全獨立於檢視和檢視模型層，並通常位於雲端中的部分。 提供完全實作的模型層，您可以建立多個不同的用戶端應用程式如果您選擇如此，例如使用相同的基礎資料的 UWP 和 web 應用程式。
* **檢視**層定義使用 XAML 標記的 UI。 標記包含資料繫結運算式 (例如[X:bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) 可定義特定的 UI 元件與各種不同的檢視模型和模型成員之間的連線。 程式碼後置檔案有時候會用做為檢視層的一部分來包含自訂或操作的 UI，或擷取事件處理常式的引數中的資料，然後再呼叫可執行工作的檢視模型方法所需的其他程式碼。 
* **檢視模型**層提供資料繫結目標的檢視。 在許多情況下，檢視模型會直接公開模型，或提供換行特定的模型成員的成員。 檢視模型也可以定義追蹤的相關資料的成員至 UI，而非模型中，例如項目清單的顯示順序。 檢視模型也可作為與其他服務，例如資料庫存取程式碼的整合點。 針對簡單的專案，您可能不需要個別的模型圖層，但只能檢視模型封裝所需的所有資料。 

## <a name="basic-and-advanced-mvvm"></a>基本和進階 MVVM

如同任何設計模式，多個方式來實作 MVVM，還有許多不同的技術會視為 MVVM 的一部分。 基於這個理由，有數個不同的第三方 MVVM 架構支援各種以 XAML 為基礎平台，包括 UWP。 不過，這些架構通常會包含多個服務實作分離的架構，讓 MVVM 的確切定義有點模稜兩可。 

雖然複雜的 MVVM 架構可以是非常有用，特別是針對企業等級的專案，則通常採用任何特定模式或技術，產生相關成本和優點並不一定顯而易見的大小和小數位數您的專案。 幸運的是，您可以採用僅提供清楚且具體的好處，這些技術，並忽略其他人，直到需要為止。 

特別是，您可以取得許多只要了解及套用資料繫結和您的應用程式邏輯分成稍早所述的圖層的完整功能的優點。 這可以使用只將 Windows sdk，且未使用任何外部的架構所提供的功能來達成。 特別是， [{x： 繫結} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)使資料繫結，更方便且更高的執行效能高於前一個 XAML 平台，而不需要大量稍早所需的未定案程式碼。

如需使用基本、 的立即可用的 MVVM 的詳細指引，請參閱[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)GitHub 上。 其他許多[UWP 應用程式範例](https://github.com/Microsoft?q=windows-appsample
)也使用基本的 MVVM 架構中，而[流量的應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)包含程式碼後置和 MVVM 版本及描述的標註[MVVM 轉換](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>請參閱

### <a name="topics"></a>主題

[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x： 繫結} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>範例

[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 清查範例](https://github.com/Microsoft/InventorySample)  
[流量的應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)  
