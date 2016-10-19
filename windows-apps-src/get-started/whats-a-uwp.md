---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "何謂通用 Windows 平台 (UWP) app？"
description: "了解我們稱為通用 Windows app 的 app 有哪些不同類型：Windows 市集 app、Windows Phone 市集 app，以及 Windows 執行階段 app。"
translationtype: Human Translation
ms.sourcegitcommit: 6be8bb0a78b614e160fb40629601bdab2d7fc71a
ms.openlocfilehash: 3fd9f4d539977c4b531e08efe0719328365c757c

---

# 何謂通用 Windows 平台 (UWP) app？

如果您是 Windows 平台新手，或是來自 .NET、Windows Forms 或 Silverlight 的世界，您可能想要知道 UWP app 到底是「什麼」**。 

就像一本名著曾說的一樣，「別慌！」 一切很快就會變得明朗。 

「通用 Windows 平台」(UWP) app 是一項建立在「通用 Windows 平台」(UWP) 上的 Windows 體驗，最初是在 Windows 8 中以「Windows 執行階段」的形式引進。 UWP app 的核心是一個想法，就是使用者希望他們的「體驗」**能夠橫跨其「所有」裝置靈活移動，並想要使用任何最便利或最具生產力的裝置來完成手邊的工作。

Windows 10 讓開發適用於 UWP 的 App 更容易，只需使用一個 API 集、一個 App 套件及一個市集，就可以觸及所有 Windows 10 裝置 – 電腦、平板電腦、手機、Xbox、HoloLens、Surface Hub 等等。 不僅更容易支援一些螢幕大小，同時也支援各種互動模型，無論是觸控、滑鼠與鍵盤、遊戲控制器還是手寫筆。

結果︰您既可花費時間全都在單一專案中使用熟悉的程式設計語言和 API，又可讓完全相同的程式碼在現今存在的眾多 Windows 硬體上執行。

![執行 Windows 的裝置](images/1894834-hig-device-primer-01-500.png)

##那麼，UWP app「到底」**是什麼？


UWP app 有什麼特別？ 以下是一些讓 Windows 10 上的 UWP 應用程式和別人不一樣的特性。

-   您的目標是裝置系列，不是作業系統。

    裝置系列會識別 API、系統特性以及您對於裝置系列中的裝置可以預期的行為。 它也會判斷可以從市集安裝您的 app 的一組裝置。

-   App 是使用 .AppX 封裝格式進行封裝和散佈。

    所有 UWP 應用程式是以 AppX 套件格式散佈。 這提供可靠的安裝機制並確保您的 app 可以順暢部署和更新。

-   還有一個適用於所有裝置的市集。

    註冊成為 app 開發人員之後，您可以將 app 提交至市集，使其可用於所有裝置系列，或僅適用於您選擇的裝置。 您在同一個地方提交和管理適用於 Windows 裝置的所有 app。

-   有一個跨裝置系列的通用 API 表面。

    通用 Windows 平台 (UWP) 核心 API 對於所有 Windows 裝置系列均相同。 如果您的 app 僅使用核心 API，它會在任何 Windows 10 裝置上執行。

-   擴充功能 SDK 讓您的 app 可以在特殊的裝置上啟動。

    擴充功能 SDK 為每個裝置系列新增特殊的 API。 如果您的 app 用於特定裝置系列，您可以藉由使用這些 API 來啟動它。 您仍然可以有一個在所有裝置上執行的 app 套件，方法是在呼叫擴充功能 API 之前檢查您的 app 執行所在的裝置系列有哪些。

-   彈性控制項和輸入

    UI 元素使用*有效的像素* (請參閱[適用於 UWP 應用程式的回應式設計入門](https://msdn.microsoft.com/library/windows/apps/Dn958435))，因此它們會根據裝置可用的螢幕像素數自行自動調整。 它們與多種輸入類型皆配合良好，例如，鍵盤、滑鼠、觸控、手寫筆和 Xbox One 控制器。 如果您需要進一步自訂您的 UI 適合特定畫面大小或裝置，新的配置面板和工具可以協助您調整 UI 適合您的 app 執行所在的裝置。

如需進一步了解 UWP，請參閱[通用 Windows 平台應用程式的指南](universal-application-platform-guide.md)。

## 使用您已知的語言


您可以使用最熟悉的程式設計語言來建立 UWP 應用程式，例如 C# 或 Visual Basic 與 XAML、JavaScript 與 HTML 或 C++ 與 DirectX 和/或 Extensible Application Markup Language (XAML)。 您甚至可以使用一種語言撰寫元件，然後在使用另一種程式設計語言撰寫的 app 中使用它們。

UWP 應用程式使用 Windows 執行階段，這是作業系統內建的原生 API。 這個 API 以 C++ 實作，而且支援在 C#、Visual Basic、C++ 及 JavaScript 語言中使用，如同使用每種語言本身的 API 一樣。

Microsoft Visual Studio 2015 為每種語言提供 UWP 應用程式範本，可讓您為所有裝置建立單一專案。 當您的工作完成時，您可以從 Visual Studio 產生應用程式套件並將其提交至 Windows 市集，讓任何使用 Windows 10 裝置的客戶都可取得您的 app。

## UWP 應用程式最適合在 Windows 上展現功能


在 Windows，您的 app 可以將相關的即時資訊呈現給使用者，並吸引他們更頻繁使用您的 app。 在現代化的應用程式經濟結構中，您的應用程式必須有足夠的吸引力，才能成為使用者生活中的首選。 您可以利用 Windows 提供的一些資源，促使使用者持續使用您的應用程式：

-   動態磚和鎖定畫面可簡單清楚的呈現與內容相關的適當資訊。
-   推播通知可在使用者需要時，提供即時的重要警示。

-   重要訊息中心提供一個位置，讓您組織和呈現使用者必須採取行動的通知和內容。

-   背景執行和觸發會在使用者需要您的 app 時讓 app 開始運作。

-   您的 app 可以使用語音和藍牙 LE 裝置， 來協助使用者與外界互動。

最後是，您可以使用漫遊資料和 Windows 認證保險箱，在使用者執行您的應用程式的所有 Windows 畫面獲得一致的漫遊經驗。 漫遊資料提供一個簡單的方式將使用者的喜好設定和各項設定儲存在雲端，您無須建立自己的同步基礎結構。 此外，您還可以將使用者認證儲存在認證保險箱中，這是最安全且可靠的位置。

##  以自己的方式讓您的 app 獲利


在 Windows 上，您可以選擇以自己的方式讓您的 app 獲利 (透過手機、平板電腦、個人電腦和其他裝置)。 我們提供您數種方法，透過您的 app 和提供的服務獲利。 您只需要選擇最適合您的方式：

-   付費下載是最簡單的選項。 只要訂出您要的價格即可。
-   試用版提供一個絕佳的方式讓使用者在購買 app 之前先試用，提供比傳統「免費增值」選項更為簡單的可搜尋性和轉換。
-   app 內購買提供您更多讓 app 獲利的彈性。

## 那就開始吧


如需進一步了解 UWP，請參閱[通用 Windows 平台 app 指南](universal-application-platform-guide.md)。 然後，請參考[開始設定](get-set-up.md)，下載開始建立 app 所需的工具。

## 相關主題


* [通用 Windows 平台 app 指南](universal-application-platform-guide.md)
* [開始設定](get-set-up.md)

## 其他進階主題

* [.NET Native - 它對「通用 Windows 平台」(UWP) 開發人員有何意義](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [.NET 中的通用 Windows app](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [適用於 UWP App 的 .NET](https://msdn.microsoft.com/en-us/library/mt185501.aspx)



<!--HONumber=Sep16_HO3-->


