---
author: TylerMSFT
title: 何謂通用 Windows 平台 (UWP) app？
description: 了解可以在各式各樣執行 Windows10 之裝置上執行的「通用 Windows 平台」(UWP) app。
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.author: twhitney
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, universal, 通用
ms.localizationpriority: medium
ms.openlocfilehash: 7f0168f0a1baef5e68bccdf0a33c3ac7eb7683a7
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4687686"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>何謂通用 Windows 平台 (UWP) app？

![通用 Windows 平台 app 在各種裝置上執行、支援彈性的使用者介面、自然的使用者輸入、一個 Microsoft Store、一個開發人員中心和雲端服務](images/universalapps-overview.png)

UWP app：

- 安全：UWP app 會宣告它們存取的裝置資源和資料。 使用者必須授權該存取。
- 無法在所有執行 Windows 10 的裝置上使用一般的 API。
- 可以使用裝置特定的功能，以及配合不同的裝置螢幕大小、解析度與 DPI 調整 UI。
- 在所有執行 Windows 10 的裝置 (或只有您指定的裝置) 上，可從 Microsoft Store 提供。 Microsoft Store 在您的 App 上提供多種方式賺錢。
- 可以安裝和解除安裝，而對電腦無風險或產生「電腦垃圾」。
- 互動：使用動態磚、推播通知和使用者活動，與 Windows 時間軸和 Cortana 接續未完成的部分互動，來使使用者積極參與。
- 可以 C#、C++、Visual Basic 與 Javascript 進行程式設計。 對於 UI，使用 XAML、HTML 或 DirectX。

讓我們更仔細看看這些。

## <a name="secure"></a>安全

UWP app 在資訊清單中宣告他們所需的裝置功能，例如存取麥克風、定位、網路攝影機、USB 裝置、檔案等等。 使用者必須認可並授權該存取之後，才能授與 App 能力。

## <a name="a-common-api-surface-across-all-devices"></a>跨所有裝置的通用 API 表面

Windows 10 引進了通用 Windows 平台 (UWP)，提供可在所有執行 Windows 10 的裝置上使用的通用 App 平台。 UWP 核心 API 在所有 Windows 裝置上是相同的。 如果您的 App 只使用核心 API，它會在任何 Windows 10 裝置上執行，無論您的目標是桌上型電腦、Xbox 或混合實境耳機等。

以 C++ /WinRT 或 C++ /CX 撰寫的 UWP app 具有屬於 UWP 之 Win32 API 的存取權。 這些 Win32 API 是由所有 Windows10 裝置實作。

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>擴充功能 SDK 會公開特定裝置類型的獨特功能

如果您的目標是通用 API，您的 App 可以在所有執行 Windows 10 的裝置上執行。 如果您想要您的 UWP app 利用裝置特定您的 API，您可以做到的。

擴充功能 SDK 可讓您為不同裝置呼叫特殊的 API。 例如，如果您的 UWP app 的目標是 IoT 裝置，您可以將 IoT 擴充功能 SDK 新增至您的專案，將特定功能瞄準 IoT 裝置。 如需新增擴充功能的 SDK 的詳細資訊，請參閱[裝置系列概觀](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks)中的**擴充功能 SDK** 一節。

您可以撰寫您的 App，使其按照您所預期只在特定類型的裝置上執行，然後限定其從 Microsoft Store 發佈到僅該類型的裝置。 或者，您也可以條件式地測試執行階段出現的 API 並據以調整您的 App 行為。 如需詳細資訊，請參閱[裝置系列概觀](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code)中的**撰寫程式碼**一節。<br>

下列影片提供簡短的裝置系列概觀和調適型程式設計：
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>調適型控制項和輸入

UI 元素因應 App 所執行的螢幕大小和 DPI 來調整它們的版面配置和縮放比例。 UWP app 與多種輸入類型皆配合良好，例如，鍵盤、滑鼠、觸控、手寫筆和 Xbox One 控制器。 如果您需要進一步調整您的 UI 以適合特定畫面大小或裝置，新的配置面板和工具可以協助您設計可因應您的 App 所執行的不同裝置和外形規格的 UI。

![執行 Windows 的裝置](images/1894834-hig-device-primer-01-500.png)

Windows 使用下列功能，協助您讓您的 UI 以多個裝置為目標：

- 通用控制項與配置面板可協助您針對裝置的螢幕解析度最佳化您的 UI。 例如，按鈕和滑桿等控制項會自動調整以符合裝置螢幕大小與 DPI 密度。 配置面板可協助您根據螢幕大小調整版面配置。 跨裝置針對解析度和 DPI 差異進行彈性縮放比例調整。
- 通用輸入處理可讓您透過觸控、手寫筆、滑鼠或鍵盤，或像是 Microsoft Xbox 控制器的控制器接收輸入。
- 工具會協助您設計可因應不同螢幕解析度的 UI。

您的 app UI 的某些層面會跨裝置自動調整。 不過，您的 app 使用者經驗設計可能需要依據 app 執行所在的裝置進行調整。 例如，在小型、手持裝置上執行相片 App 時可調整其 UI，以確保適合單手操作使用。 在桌上型電腦上執行相片 App 時，應該調整 UI 以充分利用額外的螢幕空間。

## <a name="theres-one-store-for-all-devices"></a>還有一個適用於所有裝置的 Microsoft Store

整合的 App Store 可讓您的 App 在 Windows 10 裝置上使用，例如個人電腦、平板電腦、Xbox、HoloLens、Surface Hub 及物聯網 (IoT) 裝置。 您可以將 App 提交至 Microsoft Store，使其可用於所有類型的裝置，或僅適用於您選擇的裝置。 您在同一個地方提交和管理適用於 Windows 裝置的所有 app。 有您想要用 UWP 功能現代化，並在 Microsoft Store 中銷售的 C++ 傳統型應用程式嗎？ 這也沒有問題。

UWP app 與 [Application Insights](http://azure.microsoft.com/services/application-insights/) 整合以進行詳細的遙測和分析，這是一項重要工具，可用以了解您的使用者並提升您的 App。

### <a name="monetize-your-app"></a>從您的 app 獲利

您可以選擇從您的 App 獲利的方式。 有數種方法可以利用您的 App 來賺錢。 您只需要選擇一個最適合您的方式，例如：

- 付費下載是最簡單的選項。 只要訂出價格即可。
- 試用版讓使用者在購買 app 之前先試用，提供比傳統「免費增值」選項更為簡單的可搜尋性和轉換。
- 激勵使用者的銷售價格。
- 此外，您還可以使用 App 內購買與廣告。

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>Microsoft Store 的 App 提供順暢的安裝、解除安裝和升級體驗

所有 UWP app 是使用保護使用者、裝置和系統的封裝系統散發。 使用者完全不需要生命安裝 App，因為 UWP app 可以解除安裝，而不會留下任何以 App 建立的文件以外的任何東西。

App 可以順暢地部署和更新。 App 封裝可以模組化，讓您能夠隨選下載內容及擴充功能。

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>傳送相關、即時的資訊給您的使用者，並吸引他們更頻繁使用您的 App

您有各種不同的方式您的 UWP app 吸引使用者：

- 動態磚和鎖定畫面磚可簡單清楚地從您的 App 呈現與內容相關的適當資訊。
- 推播通知可提供即時的警示吸引使用者的注意。
- 使用者活動可讓使用者即使跨裝置也能在您的 App 中接續未完成的部分。
- 控制中心可組織整理來自您 App 的通知。
- 背景執行和觸發會在使用者需要您的 App 時讓 App 開始運作。
- 您的 App 可以使用語音和藍牙 LE 裝置，來協助使用者與外界互動。
- 整合 Cortana 以新增語音命令功能至您的 App。

##  <a name="use-a-language-you-already-know"></a>使用您已知的語言

UWP app 使用 Windows 執行階段，這是作業系統提供的原生 API。 這個 API 以 C++ 實作，而且支援在 C#、Visual Basic、C++ 及 JavaScript 語言中使用。 撰寫 UWP app 的部分選項包括：

- XAML UI 和 C#、VB 或 C++
- DirectX UI 和 C++
- JavaScript 和 HTML

## <a name="links-to-help-you-get-going"></a>連結可協助您開始

### <a name="get-set-up"></a>開始設定

請查看[開始設定](get-set-up.md)，下載開始建立 App 所需的工具，然後[撰寫第一個 App](your-first-app.md)。

### <a name="design-your-app"></a>設計您的 App

Microsoft 設計系統命名為 Fluent。 Fluent Design 系統是一組與最佳做法結合的 UWP 功能，用於建立在所有執行 Windows 裝置類型上都能展現絕佳效能的 App。 Fluent 體驗從平板電腦到膝上型電腦，從個人電腦到電視等裝置以及虛擬實境裝置均自然流暢。 如需 Fluent Design 簡介，請參閱[適用於 UWP app 的 Fluent Design 系統](https://docs.microsoft.com/windows/uwp/design/fluent-design-system)。

良好的[設計](http://go.microsoft.com/fwlink/?LinkId=258848)是決定您 App 與使用者的互動方式、外觀，以及功能的程序。 使用者經驗在判斷使用者使用您的 App 時有多愉快佔有舉足輕重的地位，因此請不要跳過這個步驟。 [設計基本知識](https://dev.windows.com/design)會為您介紹如何設計通用 Windows 應用程式。 請參閱[適用於設計人員的通用 Windows 平台 (UWP) app 簡介](https://msdn.microsoft.com/library/windows/apps/dn958439)，以取得設計能讓使用者滿意的 UWP app 的詳細資訊。 開始撰寫程式碼之前，請參閱[裝置入門](../design/devices/index.md)，協助您思考在您要做為目標的所有不同表單係數上使用您的 app 的互動體驗。

除了在不同裝置上的互動之外，[規劃您的 App](https://msdn.microsoft.com/library/windows/apps/hh465427) 以納入跨多個裝置工作的好處。 例如：

- 使用 [UWP app 瀏覽設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)設計您的工作流程，以容納行動、小螢幕與大螢幕裝置。 [配置您的使用者介面](https://msdn.microsoft.com/library/windows/apps/dn958435)以因應不同的螢幕大小與解析度。

- 請考慮如何容納多個輸入類型。 請參閱[互動的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611861)以了解使用者如何使用 [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)、[語音](https://msdn.microsoft.com/library/windows/apps/dn596121)、[觸控互動](https://msdn.microsoft.com/library/windows/apps/hh465370)、[觸控式鍵盤](https://msdn.microsoft.com/library/windows/apps/hh972345)等等方式與您的 app 互動。  或者，請參閱[文字和文字輸入的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611864)以取得更多傳統互動體驗。

### <a name="add-services"></a>新增服務

- 使用[雲端服務](http://go.microsoft.com/fwlink/?LinkId=526377)跨裝置同步。
- 了解如何[連線到 Web 服務](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)以支援您的 App 體驗。
- 了解如何[新增 Cortana 到您的 App](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382)，讓您的 App 得以回應語音指令。
- 在您的計劃中包含[推播通知](https://msdn.microsoft.com/library/windows/apps/mt187203)和[在 App 內購買](https://msdn.microsoft.com/library/windows/apps/mt219684)。 這些功能應該可以跨裝置運作。

### <a name="submit-your-app-to-the-store"></a>將您的 App 提交到 Microsoft Store

全新整合的 Windows 開發人員中心儀表板，可讓您集中管理和提交您為所有 Windows 裝置開發的 app。 請參閱[使用整合的 Windows 開發人員中心儀表板](../publish/using-the-windows-dev-center-dashboard.md)了解如何提交您的 App 在 Microsoft Store 中發行。

新功能不只簡化程序，同時還讓您更好控制。 您在這裡還能找到結合[支付詳細資料](https://msdn.microsoft.com/library/windows/apps/dn986925)的詳細[分析報告](https://msdn.microsoft.com/library/windows/apps/mt148522)、[促銷應用程式和吸引客戶](https://msdn.microsoft.com/library/windows/apps/mt148526)的方式，以及更多實用功能。

如需更多的簡介資料，請參閱[建置適用於 Windows10 裝置的 Windows 應用程式的簡介](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>其他進階主題

- 了解如何使用[使用者活動](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97)，讓您 App 中的使用者活動顯示在 Windows 時間軸和 Cortana 接續未完成的部分功能中。
- 了解如何使用 [UWP app 的磚、徽章及通知](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/)。
- 如需可用於 UWP app 之 Win32 API 的完整清單，請參閱[適用於 UWP app 的 API 集合](https://msdn.microsoft.com/library/windows/desktop/mt186421)和[適用於 UWP app 的 Dll](https://msdn.microsoft.com/library/windows/desktop/mt186422)。
- 如需撰寫 .NET UWP app 的概觀，請參閱 [.NET 中的通用 Windows 應用程式](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)。
- 如需您可以在 UWP app 中使用的 .NET 類型的清單，，請參閱[適用於 UWP app 的 .NET](https://msdn.microsoft.com/library/mt185501.aspx)
- [.NET Native - 它對「通用 Windows 平台」(UWP) 開發人員有何意義](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
- 了解如何新增 Windows 10 使用者的現代體驗到現有的傳統型應用程式，以及使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)在 Microsoft Store 中散發其功能。

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>通用 Windows 平台如何與 Windows 執行階段 Api 建立關聯
如果您正在建置通用 Windows 平台 (UWP) app，您可以取得大量里程與不使用 「 通用 Windows 平台 (UWP) 」 和 「 Windows 執行階段 (WinRT) 」 的條款視為同義多或少的便利性。 但它*是*可能看起來背後的技術，並判斷只是什麼不同之處在於這些想法之間。 如果您是想了解，，最後一個本節就很適合您。

Windows 執行階段，並 WinRT Api 是 Windows Api 的進化版。 原本 Windows 的程式設計透過單層式，C 樣式的 Win32 Api。 那些已新增 COM Api ([DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)正在顯著的範例)。 Windows Forms、 WPF、.NET 和受管理的語言將拖欠款項自己撰寫 Windows 應用程式，以及他們自己的 API 技術類別的方式。 Windows 執行階段是，背後下, 一個階段的 com。 在實際應用程式二進位介面 (ABI) 層，才會顯示在 COM 中的根源。 但在 Windows 執行階段設計為可從不同的程式設計語言的絕佳範圍呼叫。 及可呼叫的方式，是很自然的每個這些語言。 為此，存取 Windows 執行階段會立即提供透過所謂語言投影。 沒有 Windows 執行階段語言投影到 C#、 Visual Basic 到、 到標準 c + +、 到 JavaScript，以此類推。 此外，一次已封裝的適當地 （請參閱[傳統型橋接器](/windows/uwp/porting/desktop-to-uwp-root)），您可以呼叫 WinRT Api，從應用程式模型的絕佳範圍的其中一個內建的應用程式： Win32，.NET，WinForms，WPF。

以及，當然，您可以呼叫 WinRT Api，從您的 UWP app。 UWP 是 Windows 執行階段為基礎所建置的應用程式模型。 技術上來說，UWP 應用程式模型，根據[CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication)，雖然可能會向您，隱藏該詳細資料，視您所選擇的程式設計語言而定。 依照本主題有說明，從值主張的觀點，在 UWP 本身撰寫單一的二進位檔，可，您應該選擇，發佈到 Microsoft 網上商店和任何一個絕佳範圍內的裝置外形規格上執行。 您限制您的應用程式呼叫，或您有條件地呼叫，您的 UWP app 的裝置觸及範圍會取決於 UWP Api 子集。

希望，本節已成功在描述 Windows 執行階段 Api，以及機制和商務值的通用 Windows 平台基礎技術之間的差異。
