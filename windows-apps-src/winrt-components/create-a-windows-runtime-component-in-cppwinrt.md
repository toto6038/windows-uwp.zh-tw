---
title: 使用 C++/WinRT 的 Windows 執行階段元件
description: 本主題說明如何使用 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 來建立和取用 Windows 執行階段元件 &mdash; ，該元件可從使用任何 Windows 執行階段語言建立的通用 Windows 應用程式中呼叫。
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10、uwp、windows、執行時間、元件、元件、Windows 執行階段元件、WRC、c + +/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646291"
---
# <a name="windows-runtime-components-with-cwinrt"></a>使用 C++/WinRT 的 Windows 執行階段元件

本主題說明如何使用 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 來建立和取用 Windows 執行階段元件 &mdash; ，該元件可從使用任何 Windows 執行階段語言建立的通用 Windows 應用程式中呼叫。

在 c + + 中建立 Windows 執行階段元件有幾個原因/Winrt
- 在複雜或需要大量運算的作業中享有 c + + 的效能優勢。
- 重複使用已撰寫並測試的標準 c + + 程式碼。
- 若要將 Win32 功能公開給以撰寫的通用 Windows 平臺 (UWP) 應用程式，例如 c #。

一般情況下，當您撰寫 c + +/WinRT 元件時，您可以使用 standard c + + 程式庫中的類型和內建類型，但在應用程式二進位介面 (ABI) 界限（您將資料傳入和寫入其他封裝中的程式碼） `.winmd` 。 在 ABI 上，使用 Windows 執行階段類型。 此外，在您的 c + +/WinRT 程式碼中，請使用委派和事件之類的型別，來執行可從元件引發並以其他語言處理的事件。 如需 c + +/Winrt 的詳細資訊，請參閱[c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)

本主題的其餘部分將逐步引導您瞭解如何在 c + +/WinRT 中撰寫 Windows 執行階段元件，以及如何從應用程式使用它。

您將在本主題中建立的 Windows 執行階段元件包含代表溫度計的執行時間類別。 本主題也會示範取用溫度計執行時間類別的核心應用程式，並呼叫函式來調整溫度。

> [!NOTE]
> 如需安裝和使用 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 如需一些基本概念和詞彙，以協助了解如何以 C++/WinRT 使用及撰寫執行階段類別，請參閱[使用 C++/WinRT 來使用 API](../cpp-and-winrt-apis/consume-apis.md) 和[使用 C++/WinRT 撰寫 API](../cpp-and-winrt-apis/author-apis.md)。

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>建立 Windows 執行階段元件 (ThermometerWRC) 

請先在 Microsoft Visual Studio 中，建立新的專案。 建立 ** (c + +/WinRT) 專案的 Windows 執行階段元件 ** ，並 *將它* 命名為 "溫度計 Windows 執行階段 Component" ) 的 (。 確定已取消核取 [將方案和專案放置於同一個目錄]。 以 Windows SDK 最新的正式推出版本 (即非預覽版本) 為目標。 命名專案 *ThermometerWRC* 可讓您使用本主題中其餘步驟的最簡單體驗。 

尚未建置專案。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 在方案總管中，將該檔案 `Thermometer.idl` 重新命名 (重新命名 `.idl` 檔案也會自動將相依的 `.h` 和 `.cpp` 檔案重新命名)。 以下面的清單取代 `Thermometer.idl` 的內容。

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

儲存檔案。 目前，專案不會建立完成，但現在建立是很好的作法，因為它會產生您將在其中執行 **溫度計** 執行時間類別的原始程式碼檔。 因此請繼續並立即建置 (您可預期在這個階段看到的建置錯誤有找不到 `Class.h` 和 `Class.g.h`有關)。

在建置程序期間，執行 `midl.exe` 工具建立元件的 Windows 執行階段中繼資料檔案 (其為 `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含存根，可讓您開始執行您在 IDL 中宣告的 **溫度計** 執行時間類別。 這些虛設常式為 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` 與 `Thermometer.cpp`。

以滑鼠右鍵按一下專案節點，然後按一下 [在檔案總管中開啟資料夾]。 這會在檔案總管中開啟專案資料夾。 然後，從 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` 資料夾將虛設常式檔案 `Thermometer.h` 和 `Thermometer.cpp` 複製到包含專案檔案的資料夾中，也就是 `\ThermometerWRC\ThermometerWRC\` 並取代目的地中的檔案。 現在要開啟 `Thermometer.h` 與 `Thermometer.cpp`，並實作我們的執行階段類別。 在中 `Thermometer.h` ，將新的私用成員新增至實 (*而非***溫度計**的 factory 實) 。

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

在中 `Thermometer.cpp` ，執行 **AdjustTemperature** 方法，如下列清單所示。

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

您也必須從這兩個檔案中刪除 `static_assert`。

如果有任何警告妨礙您建置，則加以解決或將專案屬性 **C/C++**  > **一般** > **視警告為錯誤** 設定為 **No (/WX-)** ，並再試一次建置專案。

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>建立 (ThermometerCoreApp) 的核心應用程式，以測試 Windows 執行階段元件

現在，在您的 *ThermometerWRC* 方案中建立新的專案，或在新的 (中) 。 建立 ** (c + +/WinRT) 專案的核心應用程式 ** ，並將其命名為 *ThermometerCoreApp*。 如果兩個專案都在相同的方案中，請將 *ThermometerCoreApp* 設定為啟始專案。

> [!NOTE]
> 如稍早所述，您 Windows 執行階段元件的 Windows 執行階段中繼資料檔案， (其名為 *ThermometerWRC*) 的專案會建立在資料夾中 `\ThermometerWRC\Debug\ThermometerWRC\` 。 該路徑的第一個區段是包含解決方案檔案的資料夾名稱；下一個區段是名為 `Debug` 的子目錄；最後一個區段是為 Windows 執行階段元件所命名的子目錄。 如果您未將專案命名為 *ThermometerWRC*，則您的中繼資料檔案將會位於資料夾中 `\<YourProjectName>\Debug\<YourProjectName>\` 。

現在，在您的核心應用程式專案中 (*ThermometerCoreApp*) 、加入參考，然後流覽至 `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` (或加入專案對專案參考（如果兩個專案都在相同的方案) 中）。 按一下 [新增]，然後按一下 [確定]。 現在建立 *ThermometerCoreApp*。 在不太可能發生的情況下，您看到承載檔案 `readme.txt` 不存在的錯誤，請從 Windows 執行階段元件專案中排除該檔案，然後重建 *ThermometerCoreApp*。

在建置程序期間，執行 `cppwinrt.exe` 工具將被參考的 `.winmd` 檔案處理到包含投影類型的原始碼檔案中，在使用元件裡支援您。 適用於您元件執行階段類別的投影類型標頭 &mdash;名為`ThermometerWRC.h`&mdash;會在資料夾 `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\` 中產生。

在 `App.cpp` 中包含該標頭。

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

此外，在中 `App.cpp` ，加入下列程式碼以具現化 **溫度計** 物件 (使用投影的型別預設的函式) ，然後在溫度計物件上呼叫方法。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

每次您按一下視窗時，就會遞增溫度計物件的溫度。 如果您想要逐步執行程式碼，以確認應用程式確實呼叫 Windows 執行階段元件，您可以設定中斷點。

## <a name="next-steps"></a>後續步驟

若要將更多功能或新的 Windows 執行階段類型新增至 c + +/WinRT Windows 執行階段元件，您可以遵循如上所示的相同模式。 首先，使用 IDL 來定義您想要公開的功能。 然後，在 Visual Studio 中建立專案，以產生存根的執行。 然後視需要完成執行。 使用您的 Windows 執行階段元件的應用程式，可以看到您在 IDL 中定義的任何方法、屬性和事件。 如需 IDL 的詳細資訊，請參閱 [Microsoft 介面定義語言3.0 簡介](/uwp/midl-3/intro)。

如需如何將事件加入 Windows 執行階段元件的範例，請參閱 [在 c + + 中撰寫事件/WinRT](../cpp-and-winrt-apis/author-events.md)。

## <a name="troubleshooting"></a>疑難排解

| 徵狀 | 補救方法 |
|---------|--------|
|在 C++/WinRT 應用程式中，當取用一個使用 XAML 的 [C# Windows 執行階段元件](./creating-windows-runtime-components-in-csharp-and-visual-basic.md)時，編譯器會產生格式為 " *'MyNamespace_XamlTypeInfo': is not a member of 'winrt::MyNamespace'* "&mdash;其中 *MyNamespace* 是 Windows 執行階段元件命名空間的名稱。 | 在取用 C++/WinRT 應用程式的 `pch.h` 中，視適當情況新增 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash;取代 *MyNamespace*。 |
