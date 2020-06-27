---
title: 設定遊戲專案
description: 開發遊戲的第一個步驟是在 Microsoft Visual Studio 中設定專案。 在您設定特別用於遊戲開發的專案之後，您稍後可以重複使用它做為一種範本。
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 設定, directx
ms.localizationpriority: medium
ms.openlocfilehash: c0c8d82752d9b73a3e3e7ffcec3ced1515564072
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409587"
---
# <a name="set-up-the-game-project"></a>設定遊戲專案

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

開發遊戲的第一個步驟是在 Microsoft Visual Studio 中建立專案。 在您設定特別用於遊戲開發的專案之後，您稍後可以重複使用它做為一種範本。

## <a name="objectives"></a>目標

* 使用專案範本在 Visual Studio 中建立新的專案。
* 藉由檢查**應用程式**類別的來源檔案，瞭解遊戲的進入點和初始化。
* 查看遊戲迴圈。
* 檢查項目的**package.appxmanifest.xml**檔案。

## <a name="create-a-new-project-in-visual-studio"></a>在 Visual Studio 中建立新專案

> [!NOTE]
> 如需安裝和為 C++/WinRT 開發設定 Visual Studio 的相關資訊&mdash;包括如何安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援)&mdash;，請參閱 [C++/WinRT 的 Visual Studio 支援](/windows/uwp/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

第一次安裝（或更新至）最新版本的 c + +/WinRT Visual Studio 延伸模組（VSIX）;請參閱上面的注意事項。 然後，在 Visual Studio 中，根據 [ **Core 應用程式（c + +/WinRT）** ] 專案範本建立新的專案。 以 Windows SDK 最新的正式推出版本 (即非預覽版本) 為目標。

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>請參閱**應用程式**類別，以瞭解**IFrameworkViewSource**和**IFrameworkView**

在您的核心應用程式專案中，開啟原始程式碼檔案 `App.cpp` 。 在中，**應用程式**類別的實作為，其代表應用程式及其生命週期。 在此情況下，我們知道應用程式是遊戲。 但我們會將它稱為*應用程式*，以更普遍地討論通用 WINDOWS 平臺（UWP）應用程式如何初始化。

### <a name="the-wwinmain-function"></a>WWinMain 函式

**WWinMain**函式是應用程式的進入點。 以下是**wWinMain**的樣子（來自 `App.cpp` ）。

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

我們會建立**應用程式**類別的實例（這只是建立的**應用程式**實例），並將其傳遞給靜態[**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication.run)方法。 請注意， **CoreApplication**需要[**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)介面。 因此， **App**類別必須執行該介面。

本主題的下兩節將描述[**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)和[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)介面。 這些介面（以及**CoreApplication**）代表您的應用程式使用*視圖提供者*提供視窗的方式。 Windows 會使用該視圖提供者，將您的應用程式與 Windows shell 連線，以便處理應用程式生命週期事件。

### <a name="the-iframeworkviewsource-interface"></a>IFrameworkViewSource 介面

**App**類別確實會執行[**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)，如下列清單所示。

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

執行**IFrameworkViewSource**的物件是一個*視圖提供者 factory*物件。 該物件的工作是製造並傳回*視圖提供者*物件。

**IFrameworkViewSource**具有單一方法[**IFrameworkViewSource：： CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview)。 Windows 會在您傳遞給**CoreApplication**的物件上呼叫該函數。 如您在上面所見，該方法的**App：： CreateView**實會傳回 `*this` 。 換句話說，**應用程式**物件會傳回本身。 由於**IFrameworkViewSource：： CreateView**的傳回數值型別為[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)，因此**應用程式**類別也必須執行*該*介面。 您可以看到它在上面的清單中。

### <a name="the-iframeworkview-interface"></a>IFrameworkView 介面

執行[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)的物件是一個*視圖提供者*物件。 我們現在已提供具有該視圖提供者的視窗。 這就是我們在**wWinMain**中建立的相同**應用程式**物件。 因此， **App**類別可同時做為*視圖提供者 factory*和*視圖提供者*。

現在 Windows 可以呼叫**應用程式**類別的**IFrameworkView**方法的執行。 在這些方法的實現中，您的應用程式有機會執行一些工作，例如初始化、開始載入所需的資源、連接適當的事件處理常式，以及接收您的應用程式將用來顯示其輸出的[**CoreWindow**](/uwp/api/windows.ui.core.corewindow) 。

您的**IFrameworkView**方法的執行會依照下面所示的順序來呼叫。

- [**[初始化]**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**載入**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- 會引發[**CoreApplicationView：：已啟用**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)事件。 因此，如果您已註冊（選擇性）來處理該事件，則此時會呼叫您的**OnActivated**處理常式。
- [**進行**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**解除初始化**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

以下是**應用程式**類別（在中）的基本架構 `App.cpp` ，其中顯示這些方法的簽章。

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::CoreCoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::CoreCoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

這只是**IFrameworkView**的簡介。 在[定義遊戲的 UWP 應用程式架構](tutorial--building-the-games-uwp-app-framework.md)中，我們將更詳細說明這些方法，以及如何加以執行。

### <a name="tidy-up-the-project"></a>整理專案

您從專案範本建立的核心應用程式專案包含我們在此時間點應該整理的功能。 之後，我們就可以使用專案來重新建立「正在完成」圖庫遊戲（**Simple3DGameDX**）。 對中的**應用程式**類別進行下列變更 `App.cpp` 。

- 刪除其資料成員。
- 刪除**OnPointerPressed**、 **OnPointerMoved**和**AddVisual**
- 從**SetWindow**刪除程式碼。

專案將會建立並執行，但它只會在工作區中顯示純色。

## <a name="the-game-loop"></a>遊戲迴圈

若要瞭解遊戲迴圈的外觀，請查看您所下載[Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/)範例遊戲的原始程式碼。

**App**類別具有**GameMain**類型的資料成員，名為*m_main*。 而該成員會用於**App：： Run** ，如下所示。

```cppwinrt
void Run()
{
    m_main->Run();
}
```

您可以在中找到**GameMain：： Run** `GameMain.cpp` 。 這是遊戲的主要迴圈，下面是一個非常粗略的大綱，其中顯示最重要的功能。

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

以下簡要說明此主要遊戲迴圈的作用。

如果您遊戲的視窗未關閉，請分派所有事件、更新計時器，然後轉譯和呈現圖形管線的結果。 對於這些考慮，我們會有更多的想法，而我們將在[定義遊戲的 UWP 應用程式架構](tutorial--building-the-games-uwp-app-framework.md)、轉譯[架構 I：](tutorial--assembling-the-rendering-pipeline.md)轉譯的簡介和轉譯[架構 II：遊戲](tutorial-game-rendering.md)轉譯的主題中執行此動作。 但這是 UWP DirectX 遊戲的基本程式碼結構。

## <a name="review-and-update-the-packageappxmanifest-file"></a>檢視並更新 package.appxmanifest 檔案

**Package.appxmanifest.xml**檔案包含 UWP 專案的相關中繼資料。 這些中繼資料可用來封裝和啟動您的遊戲，並提交至 Microsoft Store。 此檔案也包含重要資訊，供玩家的系統用來提供遊戲必須執行之系統資源的存取權。

按兩下**方案總管**中的**package.appxmanifest.xml**檔案，即可啟動**資訊清單設計**工具。

![package.appx 資訊清單編輯器的螢幕擷取畫面。](images/simple-dx-game-setup-app-manifest.png)

如需 **package.appxmanifest** 檔案和封裝的詳細資訊，請參閱[資訊清單設計工具](/previous-versions/br230259(v=vs.140))。 現在，請查看 [**功能**] 索引標籤，並查看所提供的選項。

![direct3d App 的預設功能的螢幕擷取畫面。](images/simple-dx-game-setup-capabilities.png)

如果您未選取遊戲使用的功能，例如存取全球高計分面板的**網際網路**，則您將無法存取對應的資源或功能。 當您建立新的遊戲時，請確定您選取的是您的遊戲所呼叫之 Api 所需的任何功能。

現在讓我們來看一下 [ **DirectX 11 應用程式（通用 Windows）** ] 範本隨附的其餘檔案。

## <a name="review-other-important-libraries-and-source-code-files"></a>查看其他重要的程式庫和原始程式碼檔

如果您想要自行建立一種遊戲專案範本，讓您可以重複使用它做為未來專案的起點，則您會想要複製 `GameMain.h` 和 `GameMain.cpp` 移出您下載的[Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/)專案，並將其新增至新的核心應用程式專案。 研究這些檔案、瞭解其作用，並移除**Simple3DGameDX**特有的任何專案。 也將任何相依于您尚未複製之程式碼的專案加上批註。 只要透過範例， `GameMain.h` 取決於 `GameRenderer.h` 。 當您複製的檔案超出**Simple3DGameDX**時，您將能夠取消批註。

以下是**Simple3DGameDX**中一些檔案的簡短問卷調查，如果您想要加入範本，您將會發現這些檔案很有用。 在任何情況下，請務必瞭解**Simple3DGameDX**本身的工作方式。

|來源檔案|檔案資料夾|描述|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources.h/.cpp|公用事業|定義**DeviceResources**類別，以控制所有 DirectX[裝置資源](tutorial--assembling-the-rendering-pipeline.md#resource)。 也會定義**IDeviceNotify**介面，用來通知您的應用程式，圖形配接器裝置已遺失或重新建立。|
|DirectXSample.h|公用事業|執行 helper 函式，例如**ConvertDipsToPixels**。 **ConvertDipsToPixels** 會將裝置獨立像素 (DIP) 中的長度轉換為實體像素的長度。|
|GameTimer .h/.cpp|公用事業|定義高解析度的計時器，對於遊戲或互動式轉譯應用程式非常實用。|
|GameRenderer.h/.cpp|轉譯|定義**GameRenderer**類別，它會執行基本的轉譯管線。|
|GameHud .h/.cpp|轉譯|定義一個類別，以使用 Direct2D 和 DirectWrite 來呈現遊戲的列印頭顯示（抬頭顯示器）。|
|VertexShader. hlsl 和 VertexShaderFlat. hlsl|著色器|包含基本頂點著色器的高階著色器語言（HLSL）程式碼。|
|無效. hlsl 和 PixelShaderFlat. hlsl|著色器|包含基本圖元著色器的高階著色器語言（HLSL）程式碼。|
|ConstantBuffers.hlsli|著色器|包含常數緩衝區的資料結構定義，以及用來將模型視圖投射（MVP）矩陣和每個頂點的資料傳遞至頂點著色器的著色器結構。|
|pch.h/.cpp|N/A|包含 common c + +/WinRT、Windows 和 DirectX include。| 

### <a name="next-steps"></a>後續步驟

此時，我們已示範如何為 DirectX 遊戲建立新的 UWP 專案、查看其中一些部分，並開始思考如何將該專案轉換成一種可重複使用的遊戲範本。 我們也探討了**Simple3DGameDX**範例遊戲的一些重要部分。

下一節將[定義遊戲的 UWP 應用程式架構](tutorial--building-the-games-uwp-app-framework.md)。 我們將更進一步瞭解**Simple3DGameDX**的運作方式。
