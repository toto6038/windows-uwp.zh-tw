---
author: stevewhims
description: 如果您有通用 8.1 應用程式與 \#8212;whether 它針對 Windows8.1、 Windows Phone 8.1 或這兩個與 \#8212;then 您會發現，您的原始程式碼和技能將可順暢地移植到 windows 10。
title: 從 Windows Runtime 8.x 移至 UWP
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eebd0467696b78458835425f7feac903ba435f42
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7440728"
---
# <a name="move-from-windows-runtime-8x-to-uwp"></a>從 Windows Runtime 8.x 移至 UWP


如果您有通用 8.1 應用程式 — 它針對 Windows8.1、 Windows Phone 8.1，或兩者 — 則會發現，您的原始程式碼和技能將可順暢地移植到 windows 10。 使用 windows 10，您可以建立通用 Windows 平台 (UWP) 應用程式，這是可供客戶安裝至各種類型裝置的單一應用程式套件。 如詳細背景資訊 windows 10，UWP app，以及調適型程式碼與我們將在此移植指南中，提及之調適型 UI 概念的請參閱[UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

在移植時，您會發現，windows 10 與先前的平台，以及 XAML 標記、 UI 架構和工具，共用大部分的 Api，而且您會發現它全都熟悉。 就像以前一樣，您還是可以選擇 C++、C# 或 Visual Basic 做為與 XAML UI 架構搭配使用的程式設計語言。 當您計劃目前的應用程式將執行哪些工作時，第一個步驟將視您的應用程式類型與專案類型而定。 下列小節中會加以說明。

## <a name="if-you-have-a-universal-81-app"></a>如果您有通用 8.1 應用程式

通用 8.1 app是從 8.1 通用 app 專案建置的。 假設專案的名稱是 AppName\_81。 它包含這些子專案。

-   AppName\_81.Windows。 這是針對 Windows8.1 建置應用程式套件的專案。
-   AppName\_81.WindowsPhone。 這是建立適用於 Windows Phone 8.1 之應用程式套件的專案。
-   AppName\_81.Shared。 這個專案當中包含其他兩個專案都會用到的原始程式碼、標記檔案及其他資產與資源。

通常，8.1 通用 Windows app 中提供相同的功能 — — 使用相同的程式碼和標記 — 在其 Windows8.1 與 Windows Phone 8.1 的形式。 這類 app 是理想的候選項目移植到單一 windows 10 應用程式，以通用裝置系列為目標 （以及您可以將它安裝到種類的裝置）。 基本上，您將會移植共用專案的內容，還需要使用來自其他兩個專案的少數內容 (或完全不使用)，因為其他兩個專案中只有少數內容適用 (或完全不適用) 。

其他時候、 Windows8.1 和/或 Windows Phone 8.1 形式的應用程式包含獨特的功能。 或者它們包含相同的功能，但它們使用不同的技巧或不同的技術來實作它們。 使用這樣的應用程式時，您可以選擇將它移植到針對通用裝置系列設計的單一應用程式 (在這種情況下，您會希望應用程式能針對不同的裝置自我調整)，或者您可以選擇將它移植到多個應用程式 (例如一個應用程式針對傳統型裝置系列，另一個針對行動裝置系列)。 通用 8.1 應用程式的特性將決定哪一項選擇最適合您的案例。

1.  將共用專案的內容移植到針對通用裝置系列設計的應用程式。 如果可以的話，請回收 Windows 和 WindowsPhone 專案的任何其他內容，以便在應用程式中無條件地使用那些內容，或在您的應用程式當時執行所在的裝置上有條件地使用那些內容 (後者的行為稱為「調適型」** 行為)。
2.  將 WindowsPhone 專案的內容移植到針對通用裝置系列設計的應用程式。 如果可以的話，請回收 Windows 專案中的任何其他內容，以便無條件或以調適型方式使用那些內容。
3.  將 Windows 專案的內容移植到針對通用裝置系列設計的應用程式。 如果可以的話，請回收 WindowsPhone 專案中的任何其他內容，以便無條件或以調適型方式使用那些內容。
4.  將 Windows 專案的內容移植到針對通用或傳統型裝置系列設計的應用程式，以及將 WindowsPhone 專案的內容移植到針對通用或行動裝置系列設計的應用程式。 您可以建立一個包含共用專案的解決方案，然後繼續在兩個專案之間共用原始程式碼、標記檔案及其他資產和資源。 或者您可以建立不同的解決方案，但仍然使用連結來共用相同的項目。

## <a name="if-you-have-a-windows-81-app"></a>如果您有 Windows 8.1 應用程式

請將專案移植到針對通用或傳統型裝置系列設計的應用程式。 如果您選擇通用裝置系列，且您的應用程式會呼叫只在傳統型裝置系列中實作的 API，您就可以使用調適型程式碼來保護那些呼叫。

## <a name="if-you-have-a-windows-phone-81-app"></a>如果您有 Windows Phone 8.1 應用程式

請將專案移植到針對通用或行動裝置系列設計的應用程式。 如果您選擇通用裝置系列，且您的應用程式會呼叫只在行動裝置系列中實作的 API，您就可以使用調適型程式碼來保護那些呼叫。

## <a name="adapting-your-app-to-multiple-form-factors"></a>調整您的應用程式以適應多種尺寸

您在前面小節中所做的選擇將決定用來執行應用程式的裝置範圍，而且此裝置範圍可能非常廣泛。 即使將您的應用程式限制只在行動裝置系列上執行，您還是必須支援非常多種不同的螢幕尺寸。 因此，如果您的應用程式將以之前未支援的尺寸執行，請用那些尺寸來測試 UI 並進行任何必要的變更，讓 UI 可針對各個尺寸適當地調適。 您可以將這個當成後續移植工作[Bookstore2](w8x-to-uwp-case-study-bookstore2.md) 與 [QuizGame](w8x-to-uwp-case-study-quizgame.md) 案例研究中會有一些實際操作範例。

## <a name="approaching-porting-layer-by-layer"></a>逐層進行移植

將通用 8.1 應用程式移植到 UWP 應用程式的模型時，您所有的知識和經驗，以及大部分您所使用的原始程式碼、標記及軟體模式，都會一併轉移過去。

-   **檢視**。 檢視 (連同檢視模型) 會構成您的應用程式 UI。 在理想的情況下，檢視是由繫結至檢視模型之可觀察屬性的標記所組成。 另一種模式 (常見且方便，但僅限短期) 是針對程式碼後置檔案中要直接操作 UI 元素的命令式程式碼。 不論是哪一種情況，您的 UI 標記和設計—甚至是操作 UI 元素的命令式程式碼—都會直接移植。
-   **檢視模型和資料模型**。 即使您沒有正式遵守關注點分離模式 (例如 MVVM)，您的應用程式中也無法避免會有執行檢視模型和資料模型的函式。 檢視模型程式碼會利用 UI 架構命名空間中的類型。 檢視模型和資料模型程式碼也都使用非視覺作業系統和 .NET Framework API (包括用於資料存取的 API)。 而且那些 API 也都[適用於 UWP 應用程式](https://msdn.microsoft.com/library/windows/apps/br211369)，所以此程式碼大部分 (如果不是全部的話) 都將在沒有任何改變的情況下移植。
-   **雲端服務**。 您的應用程式某些部分 (也許佔相當大的部分) 可能是在雲端以服務的形式執行。 在用戶端裝置上執行的應用程式部分則會連線到那些服務。 這是分散式 app 中最有可能在移植用戶端部分時保持不變的部分。 如果您還沒有雲端服務，[Microsoft Azure 行動服務](http://azure.microsoft.com/services/mobile-services/)會是您 UWP 應用程式的一個絕佳雲端服務選項，它提供強大的後端元件，可供您的應用程式呼叫以取得服務，服務涵蓋範圍可從簡單的動態磚更新，一直到伺服器陣列可提供的那種高難度延展性。

在移植之前或在移植期間，請考量是否可藉由重構來改善您的 app，以便將目的類似的程式碼一起收集在一些分層中，而不是任意散佈。 將應用程式分解為如上所述的分層，可讓您更容易更正應用程式、加以測試，並進行後續的讀取和維護。 您可以遵循 Model-View-ViewModel ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)) 模式，讓功能具備更高的可重複使用性。 這種模式會將您 app 的資料、商務及 UI 部分彼此分開。 即便是在 UI 中，它也能將狀態和行為分開，以及個別地將可測試項目從視覺效果分離。 使用 MVVM 時，您可以只撰寫一次您的資料與商務邏輯，然後在所有裝置上使用，而不需要擔心 UI 的問題。 您可能也可以在不同的裝置上重複使用大部分的檢視模型與檢視組件。

| 主題 | 說明 |
|-------|-------------|
| [移植專案](w8x-to-uwp-porting-to-a-uwp-project.md) | 當您開始移植程序時，會有兩個選項。 其中一個是編輯現有專案檔案的複本，包括應用程式套件資訊清單 (若要了解該選項，請參閱[將 app 移轉至通用 Windows 平台 (UWP)](https://msdn.microsoft.com/library/mt148501.aspx) 中關於更新您專案檔案的資訊)。 另一個選項是在 Visual Studio 中建立新的 Windows 10 專案，並將檔案複製到其中。 |
| [疑難排解](w8x-to-uwp-troubleshooting.md) | 強烈建議您將此移植指南從頭到尾讀一遍，但是我們也了解您急著想要儘快進入建置及執行專案的階段。 為此，您可以暫時將任何非必要的程式碼標成註解或移除，稍後再回來清償該負債。 本主題中的疑難排解問題和解決方式表格雖然無法用來替代閱讀接下來的幾個主題，但是在這個階段可能對您很有幫助。 在您進展到後續的主題時，隨時可以回來參考這個表格。 |
| [移植 XAML 與 UI](w8x-to-uwp-porting-xaml-and-ui.md) | 以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從通用 8.1 app 轉譯至 UWP app。 您會發現大部分標記都相容，雖然您可能需要對系統的「資源」索引鍵或您使用的自訂範本進行一些調整。 |
| [I/O、裝置與 app 模型的移植](w8x-to-uwp-input-and-sensors.md) | 與裝置本身及其感應器整合的程式碼牽涉到從使用者輸入和輸出到使用者。 它也可能涉及資料處理。 但是這個程式碼通常不被認為是 UI 層或資料層。 這個程式碼包含與震動控制器、加速計、陀螺儀、麥克風和喇叭 (與語音辨識和合成交集)、(地理) 位置以及輸入形式 (例如觸控、滑鼠、鍵盤及手寫筆) 的整合。 |
| [案例研究：Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | 本主題提供的案例研究可將非常簡單的通用 8.1 app 移植到 Windows 10 UWP app。 通用 8.1 app 會針對 Windows 8.1 建置一個應用程式套件，並針對 Windows Phone 8.1 建置另一個應用程式套件。 您可以使用 Windows 10，來建立可供客戶安裝至各種類型裝置的單一 app 套件，而那就是我們將在這個案例研究中執行的工作。 請參閱 [UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。 |
| [案例研究：Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | 這個案例研究是以 [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601) 控制項中提供的資訊為基礎建置。 在檢視模型中，每個 Author 類別執行個體都代表該作者所著之書籍的群組，而在 SemanticZoom 中，我們可以檢視依作者分組的書籍清單，或是縮小來查看作者的捷徑清單。 |
| [案例研究：QuizGame](w8x-to-uwp-case-study-quizgame.md) | 本主題提供的案例研究會將對等運作的測驗遊戲 WinRT 8.1 app 範例移植到 Windows 10 UWP app。 |

## <a name="related-topics"></a>相關主題

**文件**
* [Windows 執行階段參考](https://msdn.microsoft.com/library/windows/apps/br211377)
* [建置適用於所有 Windows 裝置的通用 Windows 應用程式](http://go.microsoft.com/fwlink/p/?LinkID=397871)
* [設計 app 的 UX](https://msdn.microsoft.com/library/windows/apps/hh767284)
