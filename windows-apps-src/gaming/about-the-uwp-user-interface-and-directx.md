---
author: mtoepke
title: App 物件和 DirectX
description: 使用 DirectX 的通用 Windows 平台 (UWP) 遊戲不會使用很多 Windows UI 使用者介面元素和物件。
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, DirectX 應用程式物件
ms.localizationpriority: medium
ms.openlocfilehash: fcbe68516e3ad8b2643faf68900e3305f18e8bbf
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "4353909"
---
# <a name="the-app-object-and-directx"></a>App 物件和 DirectX



使用 DirectX 的通用 Windows 平台 (UWP) 遊戲不會使用很多 Windows UI 使用者介面元素和物件。 相反地，由於它們在 Windows 執行階段堆疊的較低層次執行，因此必須以更基礎的方法來與使用者介面架構進行互通： 方法為直接與 app 物件進行存取和互通。 了解這個互通發生的時間和方式，以及身為 DirectX 開發人員如何 在開發 UWP app 時有效使用這個模型。

請參閱[Direct3D 圖形的詞彙](../graphics-concepts/index.md)如需有關不熟悉的圖形條款或在讀取時遇到的概念資訊。

## <a name="the-important-core-user-interface-namespaces"></a>重要的核心使用者介面命名空間


首先，請注意您必須加入 (利用 **using**) UWP app 中的 Windows 執行階段命名空間。 稍後我們會深入討論它。

-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/br241814)
-   [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021)

> **注意**：如果您不是開發 UWP app，請使用 JavaScript 或 XAML 專用程式庫和命名空間中提供的使用者介面元件，而不是使用這些命名空間中提供的類型。

 

## <a name="the-windows-runtime-app-object"></a>Windows 執行階段 App 物件


在您的 UWP app 中，您想取得一個視窗和一個檢視提供者，然後透過這個檢視提供者取得檢視，並將您的交換鏈結 (顯示緩衝區) 連接到這個檢視提供者。 您也可以將這個檢視與執行中 app 的視窗專屬事件連結。 若要取得應用程式物件的父視窗 (由 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 類型定義)，請建立實作 [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) 的類型 (和我們在上一個程式碼片段中做的一樣)。

下列基本步驟可以讓您使用核心使用者介面架構取得視窗：

1.  建立實作 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 的類型。 這是您的檢視。

    在此類型中，定義：

    -   將 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 的執行個體做為參數的 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法。 您可以呼叫 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 來取得這種類型的執行個體。 當 app 啟動時，app 物件會呼叫它。
    -   將 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 的執行個體做為參數的 [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) 方法。 您可以存取新 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 執行個體上的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 屬性，進而取得這個類型的執行個體。
    -   將進入點的字串做為唯一參數的 [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) 方法。 當您呼叫這個方法時，app 物件會提供進入點字串。 這是您設定資源的地方。 您可以在這裡建立裝置資源。 當 app 啟動時，app 物件會呼叫它。
    -   啟用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 物件並啟動視窗事件發送器的 [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法。 當應用程式的處理程序啟動時，應用程式物件會呼叫它。
    -   清除在 [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) 呼叫中設定之資源的 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) 方法。 當應用程式關閉時，應用程式物件會呼叫這個方法。

2.  建立實作 [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) 的類型。 這是您的檢視提供者。

    在此類型中，定義：

    -   名為 [**CreateView**](https://msdn.microsoft.com/library/windows/apps/hh700491) 的方法，它會傳回步驟 1 中建立的 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 實作執行個體。

3.  將檢視提供者的執行個體從 **main** 傳送至 [**CoreApplication.Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)。

了解這些基本知識後，讓我們看看還有什麼其他選項可以擴充這個方法。

## <a name="core-user-interface-types"></a>核心使用者介面類型


這裡是 Windows 執行階段中的其他核心使用者介面類型，可能對您有所幫助：

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)
-   [**Windows.UI.Core.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)

您可以使用這些類型來存取 App 的檢視 (特別是繪製 App 父視窗內容的位元)，並處理該視窗所引發的事件。 應用程式視窗的處理程序是一個 *「應用程式單一執行緒 Apartment (ASTA)」*，它可以獨立運作並處理所有回呼。

您的應用程式檢視是由應用程式視窗的檢視提供者所產生，在大部分情況下，都將由特定的架構套件或系統本身來實作，因此您不需要自己實作它。 針對 DirectX，您需要實作精簡型檢視提供者，如先前所討論。 下列元件和行為之間有特定的 1 對 1 關係：

-   [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 類型表示的應用程式檢視，可定義更新視窗的方法。
-   ASTA，這個元件的屬性可以定義應用程式執行緒的行為。 您無法在 ASTA 上建立 COM STA 屬性類型的執行個體。
-   應用程式從系統取得或您實作的檢視提供者。
-   一個父視窗，由 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 類型表示。
-   所有啟用事件的來源。 檢視和視窗都會有個別的啟用事件。

總而言之，app 物件會提供一個檢視提供者 Factory。 它會建立一個檢視提供者，然後具現化應用程式的父視窗。 檢視提供者會定義應用程式父視窗的應用程式檢視。 現在，讓我們深入討論檢視和父視窗的細節。

## <a name="coreapplicationview-behaviors-and-properties"></a>CoreApplicationView 行為和屬性


[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 代表目前的應用程式檢視。 App 單例會在初始化時建立 App 檢視，但除非啟用檢視，否則這個檢視會維持休眠狀態。 您可以透過存取檢視上的 [**CoreApplicationView.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 屬性來取得顯示檢視的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)，然後您可以將委派登錄到 [**CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件以處理啟用或停用事件。

## <a name="corewindow-behaviors-and-properties"></a>CoreWindow 行為和屬性


初始化應用程式物件時，會建立父視窗 ([**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 執行個體) 並將它傳送至檢視提供者。 如果應用程式有可以顯示的視窗，就會顯示該視窗；否則只會初始化檢視。

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 會提供與輸入和基本視窗行為相關的一系列事件。 將自己的委派登錄到這些事件後，您就可以處理它們了。

您也可以存取 [**CoreWindow.Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208264) 屬性 (提供 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 執行個體)，並藉此取得視窗的視窗事件發送器。

## <a name="coredispatcher-behaviors-and-properties"></a>CoreDispatcher 行為和屬性


您可以使用 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 類型，判斷針對視窗分派之事件的執行緒行為。 在這個類型上，只有一個特別重要的方法：[**CoreDispatcher.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 方法，這個方法會啟動視窗事件處理程序。 使用錯誤的選項為 App 呼叫這個方法，會導致各種非預期的事件處理行為。

| CoreProcessEventsOption 選項                                                           | 說明                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://msdn.microsoft.com/library/windows/apps/br208217) | 在佇列中分派所有目前可用的事件。 如果沒有擱置的事件，會等待下一個新事件。                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | 如果佇列中有擱置的事件，則會分派一個事件。 如果沒有擱置的事件，不要等待新事件出現，而是立即返回。                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217)        | 等待新事件，並分派所有可用的事件。 繼續這個行為，直到視窗關閉，或是應用程式呼叫 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 執行個體上的 [**Close**](https://msdn.microsoft.com/library/windows/apps/br208260) 方法為止。 |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | 在佇列中分派所有目前可用的事件。 如果沒有擱置的事件，則立即返回。                                                                                                                                          |

 

使用 DirectX 的 UWP 應該使用 [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217) 選項來防止干擾圖形更新的封鎖行為。

## <a name="asta-considerations-for-directx-devs"></a>DirectX 開發人員的 ASTA 考量


定義 UWP 和 DirectX app 之執行階段表示法的 app 物件，會使用稱為應用程式單一執行緒 Apartment (ASTA) 的執行緒模式 來裝載您 app 的 UI 檢視。 如果您要開發 UWP 和 DirectX app，您可能已經熟悉 ASTA 的屬性，因為您從 UWP 和 DirectX app 分派的任何執行緒，都必須使用 [**Windows::System::Threading**](https://msdn.microsoft.com/library/windows/apps/br229642) API， 或使用 [**CoreWindow::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)。 (您可以從 App 呼叫 [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)，以取得 ASTA 的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 物件)。

身為使用 UWP DirectX app 的開發人員，最重要的是您必須在 **main()** 上設定 **Platform::MTAThread**，以便讓 App 執行緒分派 MTA 執行緒。

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

當 UWP DirectX app 的 App 物件啟用時，它會建立用於 UI 檢視的 ASTA。 新的 ASTA 執行緒會呼叫到您的檢視提供者 Factory，為 app 物件建立檢視提供者，因此，檢視提供者程式碼將會在該 ASTA 執行緒上執行。

此外，您從 ASTA 分派的任何執行緒必須在 MTA 中。 請注意，您分派的任何 MTA 執行緒仍然可能造成重新進入問題，並且導致鎖死。

如果您正在移植現有的程式碼以在 ASTA 執行緒上執行，請記住下列考量：

-   等待基元 (如 [**CoWaitForMultipleObjects**](https://msdn.microsoft.com/library/windows/desktop/hh404144)) 在 ASTA 中的行為與 STA 中的不同。
-   COM 呼叫強制回應迴圈在 ASTA 中的運作方式不同。 當傳出呼叫進行時，您無法再接收到不相關的呼叫。 例如，下列行為會從 ASTA 建立鎖死狀況 (並且會立即讓應用程式當機)：
    1.  ASTA 呼叫 MTA 物件，並傳遞介面指標 P1。
    2.  之後，ASTA 呼叫相同的 MTA 物件。 MTA 物件在回傳到 ASTA 之前呼叫 P1。
    3.  P1 因為被封鎖而無法進入 ASTA，造成不相關的呼叫。 不過，MTA 執行緒在嘗試呼叫 P1 時被封鎖。

    解決方法如下：
    -   使用平行模式程式庫 (PPLTasks.h) 中定義的 **async** 模式
    -   儘快從 app 的 ASTA (app 的主要執行緒) 呼叫 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 以允許任意呼叫。

    也就是說，您無法依賴直接將不相關的呼叫傳遞至 app 的 ASTA。 如需非同步呼叫的詳細資訊， 請閱讀[使用 C++ 進行非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

總而言之，在設計 UWP app 時，請為 App 的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 與 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 使用 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 以處理所有 UI 執行緒，而非嘗試自行建立和管理您的 MTA 執行緒。 當您需要無法以 **CoreDispatcher** 處理的不同執行緒時，請使用非同步模式，並依循前述的指導原則以避免重新進入問題。

 

 




