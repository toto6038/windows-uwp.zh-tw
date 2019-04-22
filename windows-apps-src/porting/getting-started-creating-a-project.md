---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Microsoft Visual Studio 對 Windows 來說就像是 Xcode 與 iOS 和 Mac OS 的關係。 在此逐步解說中，我們會協助您能順利使用 Visual Studio。
title: 在 Visual Studio 中建立專案
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1b6ea9fdf2e504e1ceee71658eab308751e1745c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601443"
---
# <a name="getting-started-creating-a-project"></a>開始使用：建立專案

## <a name="creating-a-project"></a>建立專案

Microsoft Visual Studio 對 Windows 來說就像是 Xcode 與 iOS 和 Mac OS 的關係。 在此逐步解說中，我們會協助您能順利使用 Visual Studio。 其中會說明某些您在開始之前必須知道的基本資訊。 每次建立應用程式時，您將遵循與這些步驟類似步驟。

下列影片提供 Xcode 與 Visual Studio 的比較。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

您也會發現此[建置適用於 Windows 之應用程式的部落格文章](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/)非常有用。

適用於 Windows 10 （更正式的說法是稱為通用 Windows 平台 (UWP) 應用程式） 建立應用程式是而不是像是建立 iOS 應用程式使用分鏡腳本。 Windows 10 應用程式通常建構數個頁面，透過每個頁面包含使用者介面，例如網站上的不同部分。 每個頁面通常都有兩個相關聯的來源檔案：一個用來以 [XAML 概觀](https://msdn.microsoft.com/library/windows/apps/mt185595)格式儲存使用者介面，另一個則包含原始程式碼，通常是 C#。 當使用者與您的 App 互動時，他們將在這些頁面中瀏覽。 在這個逐步解說中，您將建立含有兩個頁面的 app。

**附註**  Windows 10 應用程式的重要功能是相同的原始程式碼中和相同的 API 集，是無論平台可用的事實。 如您所知，當您在撰寫適用於 iPhone 和 iPad 的通用 iOS app 時，您在執行階段就可以判斷您的 app 會在什麼平台上執行，並採取適當動作。 同樣地，在 Windows 10 應用程式所見，在執行階段執行的裝置。 有了為 UWP 應用程式，就不需要使用\#ifdef 的程式碼中建立手機和桌上型電腦的組建。 為了方便起見，Windows 10 應用程式也以聰明的方式使用其使用者介面控制項，視裝置而定： 比方說，您的應用程式可能會參考日期選擇器控制項，而控制項將自動外觀和功能以不同的方式取決於是否有在桌面或 [phone] 畫面上執行。 然而，您的原始程式碼仍將維持不變。

我們來看看如何建立 Windows 10 應用程式。 從執行 Visual Studio 開始。 第一次執行 Visual Studio 的時候，它會要求您取得開發人員授權。 開發人員授權讓您在本機電腦上安裝和測試 UWP 應用程式，然後再將應用程式送出到 Microsoft Store。 若要取得授權，請按照螢幕上的指示操作，以使用 Microsoft 帳戶登入。 如果您沒有授權，請按一下 [開發人員授權] 對話方塊中的 [註冊] 連結，然後按照螢幕上的指示操作。

做為比較，在您啟動 Xcode 的時候，會看到的第一個畫面是 [**Welcome to Xcode**]，與下圖類似。

![Xcode 歡迎畫面](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio 是非常相似的。 您會看見如下圖中顯示的 [**開始頁面**]。

![Visual Studio 開始畫面](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

若要建立新 app，請執行下列其中一個動作，以從建立專案開始：

-   在 [開始] 區域，點選 [新增專案]。
-   點選 [檔案] 功能表，然後點選 [新增專案]。

做為比較，當您在 Xcode 中建立新專案的時候，會看到如下圖中類似的專案範本清單。

![Xcode 新增專案對話方塊](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

Visual Studio 中也提供好幾個可以選用的專案範本，如下圖中所示。

![visual studio 新專案] 對話方塊](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) 對於此逐步解說中，點選**Visual C#** ，然後點選 [ **Windows**， **Windows 通用**及**空白應用程式 （Windows 通用）**。 在 \[名稱\] 方塊中，輸入「MyApp」，然後點選 \[確定\]。 Visual Studio 會建立並接著顯示您的第一個專案。 現在您可以開始設計您的應用程式並在其中新增程式碼。

## <a name="next-step"></a>後續步驟

[開始使用：選擇程式設計語言](getting-started-choosing-a-programming-language.md)
