---
title: 何謂通用 Windows 平台 (UWP) 應用程式？
description: 了解可以在各式各樣執行 Windows 10 裝置上執行的「通用 Windows 平台」(UWP) 應用程式。
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 09/15/2020
ms.topic: article
ms.custom: contperf-fy21q1
keywords: windows 10, uwp, universal, 通用
ms.localizationpriority: medium
ms.openlocfilehash: 6871512e26b4c1f960034a5e97d88ade0b211094
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860422"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>何謂通用 Windows 平台 (UWP) 應用程式？

UWP 是建立 Windows 用戶端應用程式的許多方式之一。 UWP 應用程式會使用 WinRT API 來提供功能強大的 UI 和進階的非同步功能，適用於與網際網路連線的裝置。

若要下載開始建立 UWP 應用程式所需的工具，請查看 [開始設定](/windows/apps/get-set-up)，然後 [撰寫第一個應用程式](your-first-app.md)。


## <a name="where-does-uwp-fit-in-the-microsoft-development-story"></a>UWP 在 Microsoft 開發案例中的定位為何？

UWP 是建立在 Windows 10 裝置上執行之應用程式的一種選擇，並且可以與其他平台結合使用。 UWP 應用程式可以使用 Win32 API 和 .NET 類別 (請參閱[適用於 UWP 應用程式的 API 集](/previous-versions/mt186421(v=vs.85))、[適用於 UWP 應用程式的 Dll](/previous-versions/mt186422(v=vs.85))，以及[適用於 UWP 應用程式的 .NET](/dotnet/api/index?view=dotnet-uwp-10.0))。

Microsoft 開發案例持續演進，並提供 [WinUI](/windows/apps/winui/)、[MSIX](/windows/msix/) 和 [Project Reunion](https://github.com/microsoft/ProjectReunion) 等計畫，UWP 是建立用戶端應用程式的強大工具。


## <a name="features-of-a-uwp-app"></a>UWP 應用程式的功能

UWP 應用程式：

- 很安全：UWP 應用程式會宣告它們存取的裝置資源和資料。 使用者必須授權該存取。
- 能在任何執行 Windows 10 的裝置上使用一般的 API。
- 可以使用裝置特定的功能，以及配合不同的裝置螢幕大小、解析度與 DPI 調整 UI。
- 在所有執行 Windows 10 的裝置 (或只有您指定的裝置) 上，可從 Microsoft Store 提供。 Microsoft Store 在您的應用程式上提供多種方式賺錢。
- 可以安裝和解除安裝，而對電腦無風險或產生「電腦垃圾」。
- 互動：使用動態磚、推播通知和使用者活動，與 Windows 時間軸和 Cortana 接續未完成的部分互動，來使使用者積極參與。
- 可以 C#、C++、Visual Basic 與 Javascript 進行程式設計。 對於 UI，使用 WinUI、XAML、HTML 或 DirectX。

讓我們更仔細看看這些。

### <a name="secure"></a>安全

UWP 應用程式在資訊清單中宣告他們所需的裝置功能，例如存取麥克風、定位、網路攝影機、USB 裝置、檔案等等。 使用者必須認可並授權該存取之後，才能授與應用程式能力。

### <a name="a-common-api-surface-across-all-devices"></a>跨所有裝置的通用 API 表面

Windows 10 引進了通用 Windows 平台 (UWP)，提供可在所有執行 Windows 10 的裝置上使用的通用 App 平台。 UWP 核心 API 在所有 Windows 裝置上是相同的。 如果您的 App 只使用核心 API，它會在任何 Windows 10 裝置上執行，無論您的目標是桌上型電腦、Xbox 或混合實境耳機等。

以 C++ /WinRT 或 C++ /CX 撰寫的 UWP 應用程式具有屬於 UWP 之 Win32 API 的存取權。 這些 Win32 API 是由所有 Windows 10 裝置實作。

### <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>擴充功能 SDK 會公開特定裝置類型的獨特功能

如果您的目標是通用 API，則應用程式可以在所有執行 Windows 10 的裝置上執行。 但如果您想要讓 UWP 應用程式利用裝置特定的 API，則您也可以這麼做。

擴充功能 SDK 可讓您為不同裝置呼叫特殊的 API。 例如，如果您的 UWP 應用程式的目標是 IoT 裝置，您可以將 IoT 擴充功能 SDK 新增至您的專案，將特定功能瞄準 IoT 裝置。 如需新增擴充功能的 SDK 的詳細資訊，請參閱 [使用擴充功能 SDK 進行程式設計](/uwp/extension-sdks/device-families-overvieww#extension-sdks)中的 **擴充功能 SDK** 一節。

您可以撰寫您的應用程式，使其按照您所預期只在特定類型的裝置上執行，然後限定其從 Microsoft Store 發佈到僅該類型的裝置。 或者，您也可以條件式地測試執行階段出現的 API 並據以調整您的應用程式行為。 如需詳細資訊，請參閱 [使用擴充功能 SDK 進行程式設計](/uwp/extension-sdks/device-families-overview#writing-code)中的 **撰寫程式碼** 一節。<br>

下列影片提供簡短的裝置系列概觀和調適型程式設計：
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

### <a name="adaptive-controls-and-input"></a>調適型控制項和輸入

UI 元素因應應用程式所執行的螢幕大小和 DPI 來調整它們的版面配置和縮放比例。 UWP 應用程式與多種輸入類型皆配合良好，例如，鍵盤、滑鼠、觸控、手寫筆和 Xbox One 控制器。 如果您需要進一步調整您的 UI 以適合特定畫面大小或裝置，新的配置面板和工具可以協助您設計可因應您的應用程式所執行的不同裝置和外形規格的 UI。

![執行 Windows 的裝置](images/1894834-hig-device-primer-01-500.png)

Windows 使用下列功能，協助您讓您的 UI 以多個裝置為目標：

- 通用控制項與配置面板可協助您針對裝置的螢幕解析度最佳化您的 UI。 例如，按鈕和滑桿等控制項會自動調整以符合裝置螢幕大小與 DPI 密度。 配置面板可協助您根據螢幕大小調整版面配置。 跨裝置針對解析度和 DPI 差異進行彈性縮放比例調整。
- 通用輸入處理可讓您透過觸控、手寫筆、滑鼠、鍵盤，或像是 Microsoft Xbox 控制器的控制器接收輸入。
- 工具會協助您設計可因應不同螢幕解析度的 UI。

您的應用程式 UI 的某些層面會在不同裝置上自動調整。 不過，您的應用程式使用者經驗設計可能需要依據應用程式執行所在的裝置進行調整。 例如，在小型、手持裝置上執行相片應用程式時可調整其 UI，以確保適合單手操作使用。 在桌上型電腦上執行相片應用程式時，應該調整 UI 以充分利用額外的螢幕空間。

### <a name="theres-one-store-for-all-devices"></a>還有一個適用於所有裝置的市集。

整合的 App Store 可讓您的 App 在 Windows 10 裝置上使用，例如個人電腦、平板電腦、Xbox、HoloLens、Surface Hub 及物聯網 (IoT) 裝置。 您可以將應用程式提交至 Microsoft Store，使其可用於所有類型的裝置，或僅適用於您選擇的裝置。 您在同一個地方提交和管理適用於 Windows 裝置的所有應用程式。 有您想要用 UWP 功能現代化，並在 Microsoft Store 中銷售的 C++ 傳統型應用程式嗎？ 這也沒有問題。

UWP 應用程式與 [Application Insights](https://azure.microsoft.com/services/application-insights/) 整合以進行詳細的遙測和分析，這是一項重要工具，可用以了解您的使用者並提升您的應用程式。

UWP 應用程式可以與 [MSIX](/windows/msix/) 一起封裝，並透過 Microsoft Store 或以其他方式散發。 無論應用程式以何種方式散發，MSIX 均允許更新應用程式，請參閱[從您的程式碼更新非 Store 發佈的應用程式套件](/windows/msix/non-store-developer-updates)。

### <a name="monetize-your-app"></a>從您的應用程式獲利

您可以選擇從您的應用程式獲利的方式。 有數種方法可以利用您的應用程式來賺錢。 您只需要選擇一個最適合您的方式，例如：

- 付費下載是最簡單的選項。 只要訂出價格即可。
- 試用版讓使用者在購買應用程式之前先試用，提供比傳統「免費增值」選項更為簡單的可搜尋性和轉換。
- 激勵使用者的銷售價格。
- App 內購買。


### <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>傳送相關、即時的資訊給您的使用者，並吸引他們更頻繁使用您的應用程式

您有各種不同的方式您的 UWP 應用程式吸引使用者：

- 動態磚和鎖定畫面磚可簡單清楚地從您的應用程式呈現與內容相關的適當資訊。
- 推播通知可提供即時的警示吸引使用者的注意。
- 使用者活動可讓使用者即使跨裝置也能在您的應用程式中接續未完成的部分。
- 控制中心可組織整理來自您應用程式的通知。
- 背景執行和觸發會在使用者需要您的應用程式時，讓應用程式開始運作。
- 您的應用程式可以使用語音和藍牙 LE 裝置，來協助使用者與外界互動。
- 整合 Cortana 以新增語音命令功能至您的應用程式。

##  <a name="use-a-language-you-already-know"></a>使用您已知的語言

UWP 應用程式使用 Windows 執行階段，這是作業系統提供的原生 API。 這個 API 以 C++ 實作，而且支援在 C#、Visual Basic、C++ 及 JavaScript 語言中使用。 撰寫 UWP 應用程式的部分選項包括：

- XAML UI 和 C#、VB 或 C++
- DirectX UI 和 C++
- JavaScript 和 HTML
- WinUI

## <a name="links-to-help-you-get-going"></a>連結可協助您開始

### <a name="get-set-up"></a>開始設定

請查看[開始設定](/windows/apps/get-started/get-set-up)，下載開始建立應用程式所需的工具，然後[撰寫第一個應用程式](your-first-app.md)。

### <a name="design-your-app"></a>設計您的應用程式

Microsoft 設計系統命名為 Fluent。 Fluent Design 系統是一組與最佳做法結合的 UWP 功能，用於建立在所有執行 Windows 裝置類型上都能展現絕佳效能的應用程式。 Fluent 體驗從平板電腦到膝上型電腦，從個人電腦到電視等裝置以及虛擬實境裝置均自然流暢。 如需 Fluent Design 簡介，請參閱[適用於 UWP 應用程式的 Fluent Design 系統](/windows/uwp/design/fluent-design-system)。

良好的[設計](http://design.windows.com/)是決定應用程式與使用者的互動方式、外觀，以及功能的程序。 使用者經驗在判斷使用者使用您的應用程式時有多愉快佔有舉足輕重的地位，因此請不要跳過這個步驟。 [設計基本知識](https://developer.microsoft.com/windows/apps/design)會為您介紹如何設計通用 Windows 應用程式。 請參閱[適用於設計人員的通用 Windows 平台 (UWP) 應用程式簡介](../design/basics/design-and-ui-intro.md)，以取得設計能讓使用者滿意的 UWP 應用程式的詳細資訊。 開始撰寫程式碼之前，請參閱[裝置入門](../design/devices/index.md)，協助您思考在目標裝置所有不同構成要素上使用您的應用程式的互動體驗。

除了在不同裝置上的互動之外，請妥善[計劃您的應用程式](./plan-your-app.md)以納入跨多個裝置工作的好處。 例如：

- 使用 [UWP 應用程式瀏覽設計基本知識](../design/basics/navigation-basics.md)，設計在行動裝置、小螢幕與大螢幕裝置均可行的工作流程。 [配置您的使用者介面](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)以回應不同的螢幕大小與解析度。

- 請考慮如何容納多個輸入類型。 請參閱[互動的指導方針](../design/layout/index.md)以了解使用者如何使用 [Cortana](/cortana/skills/)、[語音](../design/input/speech-interactions.md)、[觸控互動](../design/input/touch-interactions.md)、[觸控式鍵盤](../design/input/keyboard-interactions.md)等等方式與您的應用程式互動。  或者，請參閱[文字和文字輸入的指導方針](../design/controls-and-patterns/text-controls.md)以取得更多傳統互動體驗。

### <a name="add-services"></a>新增服務

- 使用[雲端服務](https://azure.microsoft.com/documentation/services/cloud-services)進行跨裝置同步。
- 了解如何[連線到 Web 服務](/previous-versions/windows/apps/hh761504(v=win.10))以支援您的應用程式體驗。
- 在您的計劃中包含[推播通知](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)和[在應用程式內購買](../monetize/enable-in-app-product-purchases.md)。 這些功能應該可以跨裝置運作。

### <a name="submit-your-app-to-the-store"></a>將您的應用程式提交到 Windows 市集

[合作夥伴中心](https://partner.microsoft.com/dashboard)可讓您集中管理和提交您為 Windows 裝置開發的所有應用程式。 請參閱[發佈 Windows 應用程式和遊戲](../publish/index.md)以了解如何提交您的應用程式，以便在 Microsoft Store 中發佈。

新功能不只簡化程序，同時還讓您更好控制。 您在這裡還能找到結合[支付詳細資料](../publish/payout-summary.md)的詳細[分析報告](../publish/analytics.md)、[促銷應用程式和吸引客戶](../publish/attract-customers-and-promote-your-apps.md)的方式，以及更多好用功能。

如需更多的簡介資料，請參閱[建置適用於 Windows 10 裝置的 Windows 應用程式的簡介](/archive/msdn-magazine/2015/may/windows-10-an-introduction-to-building-windows-apps-for-windows-10-devices)

### <a name="more-advanced-topics"></a>其他進階主題

- 了解如何使用[使用者活動](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97)，讓您應用程式中的使用者活動顯示在 Windows 時間軸和 Cortana 接續未完成的部分功能中。
- 了解如何使用 [UWP 應用程式的磚、徽章及通知](../design/shell/tiles-and-notifications/index.md)。
- 如需可用於 UWP 應用程式之 Win32 API 完整清單，請參閱[適用於 UWP 應用程式的 API 集合](/previous-versions/mt186421(v=vs.85))和[適用於 UWP 應用程式的 Dll](/previous-versions/mt186422(v=vs.85))。
- 如需撰寫 .NET UWP 應用程式的概觀，請參閱 [.NET 中的通用 Windows 應用程式](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/)。
- 如需您可以在 UWP 應用程式中使用的 .NET 類型清單，請參閱[適用於 UWP 應用程式的 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)
- [使用 .NET Native 編譯應用程式](/dotnet/framework/net-native/)
- 了解如何新增 Windows 10 使用者的現代體驗到現有的傳統型應用程式，以及使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)在 Microsoft Store 中散發其功能。

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>通用 Windows 平台與 Windows 執行階段 API 有何關聯
如果您正在建置通用 Windows 平台 (UWP) 應用程式，將「通用 Windows 平台 (UWP)」和「Windows 執行階段 (WinRT)」詞彙視為多少有點同義，可讓您獲得許多好處和便利性。 但「可以」  深入了解技術，並判斷這兩個想法之間的差異。 如果您想知道，請參閱最後一節。

Windows 執行階段和 WinRT API 是 Windows API 的進化版。 起初，Windows 透過一般、C 樣式 Win32 API 撰寫程式。 新增的 COM API ([DirectX](/windows/desktop/directx) 是明顯的範例)。 Windows Forms、WPF、.NET 和受控語言有其自己撰寫 Windows 應用程式的方式，以及自己喜好的 API 技術。 Windows 執行階段實際上是 COM 的下一個階段。 在實際的應用程式二進位介面 (ABI) 層，其在 COM 中的根目錄會變成可見。 但是，Windows 執行階段的設計訴求是可從各種不同的程式設計語言進行呼叫。 而且能夠以對每種語言都很自然的方式呼叫。 為了這個目的，可透過所謂的語言投影存取 Windows 執行階段。 Windows 執行階段語言投影成 C#、Visual Basic、標準 C++、JavaScript 等等。 此外，適當封裝後 (請參閱[傳統型橋接器](/windows/msix/desktop/source-code-overview))，您可以從某個應用程式模型內建的應用程式呼叫 WinRT API：Win32、.NET、WinForms 和 WPF。

您當然可以從您的 UWP 應用程式呼叫 WinRT API。 UWP 是以 Windows 執行階段為基礎的應用程式模型。 在技術上，UWP 應用程式模型是以 [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication) 為基礎，不過視您所選擇的程式設計語言而定，您可能看不到細節。 如本主題所闡述，從價值主張的觀點來看，UWP 適合用於撰寫可發佈至 Microsoft Store 並在任何一種裝置板型規格上執行的單一二進位檔。 UWP 應用程式的裝置範圍取決於您限制您的應用程式呼叫或您有條件呼叫的 Windows 執行階段 API 子集。

希望這一節已成功地說明技術基礎 Windows 執行階段 API，以及通用 Windows 平台的機制與商業價值之間的差異。
