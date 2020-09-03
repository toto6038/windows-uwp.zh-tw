---
title: 將剪貼簿範例從 C# 移植到 C++/WinRT (案例研究)
description: 本主題會提供將其中一個[通用 Windows 平台 (UWP) 應用程式範例](https://github.com/microsoft/Windows-universal-samples)從 [C#](/visualstudio/get-started/csharp) 移植到 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 的案例研究。
ms.date: 04/13/2020
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 移植, 移轉, C#, 範例, 剪貼簿, 案例研究
ms.localizationpriority: medium
ms.openlocfilehash: 5a7ec46b28a8ddf0b4accadb37b40e786ac8c47a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170412"
---
# <a name="porting-the-clipboard-sample-tocwinrtfromcmdasha-case-study"></a>將剪貼簿範例從 C# 移植到 C++/WinRT &mdash; 案例研究

本主題會提供將其中一個[通用 Windows 平台 (UWP) 應用程式範例](https://github.com/microsoft/Windows-universal-samples)從 [C#](/visualstudio/get-started/csharp) 移植到 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 的案例研究。 您可以依照逐步解說移植自己範例，以了解如何移植並獲取經驗。

如需有關從 C# 移植至 C++/WinRT 所涉及之技術詳細資料的完整說明，請參閱附屬主題：[從 C# 移至 C++/WinRT](./move-to-winrt-from-csharp.md)。

## <a name="a-brief-preface-about-c-and-c-source-code-files"></a>有關 C# 和 C++ 原始程式碼檔案的簡短序言

在 C# 專案中，您的原始程式碼檔案主要是 `.cs` 檔案。 當您移至 C++ 時，您會發現有更多種類的原始程式碼檔案可供使用。 原因在於編譯器之間的差異、重複使用 C++ 原始程式碼的方式，以及「宣告」和「定義」類型及其函式 (其方法) 的概念。

函式「宣告」 僅描述函式的「簽章」 (其傳回類型、其名稱及其參數類型和名稱)。 函式「定義」包含函式的「主體」 (其實作)。

在類型方面則有點不同。 您藉由提供名稱和 (至少)「宣告」其所有成員函式 (和其他成員) 來「定義」類型。 沒錯，您可以「定義」類型，即使您未定義其成員函式亦然。

- 常見 C++ 原始程式碼檔案為 `.h` 和 `.cpp` 檔案。 `.h` 檔案是「標頭」檔案，而且會定義一或多個類型。 雖然您「可以」在標頭中定義成員函式，但這通常是 `cpp` 檔案的用途。 因此對於假設 C++ 類型 **MyClass**，您會在 `MyClass.h` 中定義 **MyClass**，而且會在 `MyClass.cpp` 中定義其成員函式。 若要讓其他開發人員重複使用您的類別，只要分享 `.h` 檔案和物件程式碼即可。 您會將 `.cpp` 檔案保密，因為實作會構成您的智慧財產。
- 預先編譯的標頭 (`pch.h`)。 通常，您的應用程式中會包含一組標頭檔，而這組標頭檔很少變更。 因此，您可以將這組標頭彙總成一個標頭，編譯該標頭一次，然後在每次建置時使用該預先編譯步驟的輸出，而不是在每次編譯時處理這組標頭的內容。 您可以透過「預先編譯的標頭」 檔案(通常名為 `pch.h`) 來執行此動作。
- `.idl` 檔案。 這些檔案包含介面定義語言 (IDL)。 您可將 IDL 視為 Windows 執行階段類型的標頭檔。 我們將在 [**MainPage** 類型的 IDL](#idl-for-the-mainpage-type) 一節中進一步討論 IDL。

## <a name="download-and-test-the-clipboard-sample"></a>下載並測試剪貼簿範例

瀏覽[剪貼簿範例](/samples/microsoft/windows-universal-samples/clipboard/) \(英文\) 網頁，然後按一下 [下載 ZIP] \(英文\)。 將下載的檔案解壓縮，並查看資料夾結構。

- C# 版本的範例原始程式碼包含在名為 `cs` 的資料夾中。
- C++/WinRT 版本的範例原始程式碼包含在名為 `cppwinrt` 的資料夾中。
- 其他檔案 (由 C# 版本和 C++/WinRT 版本所使用) 可以在 `shared` 和 `SharedContent` 資料夾中找到。

此主題中的逐步解說將說明如何從 C# 原始程式碼移植 Clipboard 範例的 C++/WinRT 來重新建立該版本。 如此一來，您就可以了解如何將自己 C# 專案移植到 C++/WinRT。

若要了解範例的用途，請開啟 C# 解決方案 (`\Clipboard_sample\cs\Clipboard.sln`)、適當地變更設定 (也許變更為 *x64*)、建置，然後執行。 範例本身的使用者介面 (UI) 會逐步引導您完成其各種功能。

## <a name="create-a-blank-app-cwinrt-named-clipboard"></a>建立空白應用程式 (C++/WinRT)，並將其命名為 Clipboard

> [!NOTE]
> 如需安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

在 Microsoft Visual Studio 中建立新的 C++/WinRT 專案，開始移植程序。 使用 [空白應用程式 (C++/WinRT)] 專案範本建立新的專案。 將其名稱設定為 *Clipboard*，並 (讓您的資料夾結構符合此逐步解說) 確保取消核取 [將解決方案和專案放置於同一個目錄]。

若要取得基準，請確認這個新的空白專案可建置並執行。

## <a name="packageappxmanifest-and-asset-files"></a>Package.appxmanifest 和資產檔案

如果 C# 和 C++/WinRT 版本的範例不需要並存安裝在同一部電腦上，則這兩個專案的應用程式封裝資訊清單來源檔案 (`Package.appxmanifest`) 可以完全相同。 在該情況下，您只需要將 `Package.appxmanifest` 從 C# 專案複製到 C++/WinRT 專案，就大功告成了。

為了讓兩個版本的範例並存，需要不同的識別碼。 在該情況下，請在 C++/WinRT 專案中，使用 XML 編輯器開啟 `Package.appxmanifest` 檔案，並記下這三個值。

- 在 **/Package/Identity** 元素內，記下 **Name** 屬性的值。 這是*封裝名稱*。 針對新建立的專案，專案將會為其提供唯一 GUID 的初始值。
- 在 **/Package/Applications/Application** 元素內，記下 **Id** 屬性的值。 這是*應用程式識別碼*。
- 在 **/Package/mp:PhoneIdentity** 元素內，記下 **PhoneProductId** 屬性的值。 同樣地，針對新建立的專案，這將會設定為與封裝名稱設定相同的 GUID。

接著，將 `Package.appxmanifest` 從 C# 專案複製到 C++/WinRT 專案。 最後，您可以還原所記下的三個值。 或者，您可以編輯複製的值，使其成為唯一的且/或適合應用程式和您的組織 (如同您平常針對新專案所做的)。 例如，在此案例中，我們可以只將複製的值從 *Microsoft.SDKSamples.Clipboard.CS* 變更為 *Microsoft.SDKSamples.Clipboard.CppWinRT*，而不還原封裝名稱的值。 而且，我們可以將應用程式識別碼設定為 *App*。 只要封裝名稱*或*應用程式識別碼不同，則這兩個應用程式的應用程式使用者模型識別碼 (AUMID) 就不同。

基於此逐步解說的目的，在 `Package.appxmanifest` 中進行一些其他變更是合理的。 *剪貼簿 C# 範例*字串出現三次。 將其變更為*剪貼簿 C++/WinRT 範例*。

在 C++/WinRT 專案中，`Package.appxmanifest` 檔案和專案現在與所參考的資產檔案不同步。 若要解決該問題，請先選取 `Assets` 資料夾 (在 Visual Studio 的 [方案總管] 內) 中的所有檔案，並移除這些檔案 (選擇對話方塊中的 [刪除])，就可以移除 C++/WinRT 專案中的資產。

C# 專案會參考共用資料夾中的資產檔案。 您可以在 C++/WinRT 專案中執行相同的動作，也可以像我們在此逐步解說中一樣，用複製檔案的方式。

瀏覽至 `\Clipboard_sample\SharedContent\media` 資料夾。 選取 C# 專案所包含的七個檔案 (`microsoft-sdk.png` 至 `windows-sdk.png`)、複製這些檔案，然後將其貼到新專案的 `\Clipboard\Clipboard\Assets` 資料夾中。

以滑鼠右鍵按一下 `Assets` 資料夾 (在 C++/WinRT 專案的 [方案總管] 中) > [新增] >  > [現有項目...]，然後瀏覽至 `\Clipboard\Clipboard\Assets`。 在檔案選擇器中，選取七個檔案，然後按一下 [新增]。

`Package.appxmanifest` 現在會與專案的資產檔案同步。

## <a name="mainpage-including-the-functionality-that-configures-the-sample"></a>**MainPage**，包括範例設定功能

剪貼簿範例&mdash;如同所有[通用 Windows 平台 (UWP) 應用程式範例](https://github.com/microsoft/Windows-universal-samples)&mdash;是由使用者一次逐步執行一個案例的集合所組成。 給定範例中的案例集合是在範例的原始程式碼中設定的。 集合中的每個案例都是一個資料項目，可儲存標題，以及專案中實作案例之類別的類型。

在 C# 版本的範例中，如果您查看 `SampleConfiguration.cs` 原始程式碼檔案，您將會看到兩個類別。 大部分的設定邏輯都是在 **MainPage** 類別中，這是部分類別 (結合 `MainPage.xaml` 中的標記和 `MainPage.xaml.cs` 中的命令式程式碼時，其會形成完整的類別)。 這個原始程式碼檔案中的另一個類別是 **Scenario**，其中包含 **Title** 和 **ClassType** 屬性。

在接下來的幾個小節中，我們將探討如何移植 **MainPage** 和 **Scenario**。

### <a name="idl-for-the-mainpage-type"></a>**MainPage** 類型的 IDL

這一節，我們首先會討論介面定義語言 (IDL)，以及其如何在使用 C++/WinRT 進行程式設計時協助我們。 IDL 是一種原始程式碼，其描述 Windows 執行階段類型的可呼叫介面。 類型的可呼叫 (或公用) 介面會「投影」在世界中，以便取用該類型。 類型的該「投影」部分與類型的實際內部實作相反，投影部分當然無法呼叫，且不是公用的。 這只是我們在 IDL 中定義的投影部分。

擁有撰寫的 IDL 原始程式碼 (在 `.idl` 檔案內) 後，您就可以將 IDL 編譯成電腦可讀取的中繼資料檔案 (也稱為 Windows 中繼資料)。 這些中繼資料檔的副檔名為 `.winmd`，以下是其中一些用途。

- `.winmd` 可以描述元件中的 Windows 執行階段類型。 當您從應用程式專案參考 Windows 執行階段元件 (WRC) 時，應用程式專案會讀取屬於 WRC 的 Windows 中繼資料 (該中繼資料可能位於不同的檔案中，或者可能封裝到與 WRC 本身相同的檔案中)，以便您在應用程式內取用 WRC 的類型。
- `.winmd` 可以在應用程式的某個部分中描述 Windows 執行階段類型，讓相同應用程式的不同部分可加以取用。 例如，在相同應用程式中，XAML 頁面所取用的 Windows 執行階段類型。
- 為了讓您更輕鬆地取用 Windows 執行階段類型 (內建或第三方)，C++/WinRT 建置系統會使用 `.winmd` 檔案來產生包裝函式類型，以代表這些 Windows 執行階段類型的投影部分。
- 為了讓您更輕鬆地實作自己的 Windows 執行階段類型，C++/WinRT 建置系統會將您的 IDL 轉換成 `.winmd` 檔案，然後使用該檔案來為您的投影產生包裝函式，以及產生可作為您的實作基礎的 stub (我們稍後會在本主題中進一步討論這些 stub)。

我們搭配 C++/WinRT 使用的特定 IDL 版本是 [Microsoft 介面定義語言 3.0](/uwp/midl-3/intro)。 在本節的其餘部分，我們將稍為詳細地檢查 C# **MainPage** 類型。 我們會決定該類型有哪些部分必須在 C++/WinRT **MainPage** 類型的「投影」 中 (也就是，其可呼叫 (或公用) 介面)，而哪些部可能只是其實作的一部分。 這種區別很重要，因為當我們要撰寫 IDL (我們將在下一節中進行) 時，我們只會在其中定義可呼叫的部分。

一起實作 **MainPage** 類型的 C# 原始程式碼檔案包括：`MainPage.xaml` (我們很快就會透過複製來移植)、`MainPage.xaml.cs` 和 `SampleConfiguration.cs`。

在 C++/WinRT 版本中，我們會以類似的方式，將 **MainPage** 類型分解到原始程式碼檔案中。 我們將會採用 `MainPage.xaml.cs` 中的邏輯，並將其大部分轉譯為 `MainPage.h` 和 `MainPage.cpp`。 置於 `SampleConfiguration.cs` 中的邏輯，我們則將其轉譯為 `SampleConfiguration.h` 和 `SampleConfiguration.cpp`。

C# 通用 WINDOWS 平台 (UWP) 應用程式中的類別當然是 Windows 執行階段類型。 但是當您在 C++/WinRT 應用程式中撰寫類型時，可以選擇該類型為 Windows 執行階段類型，還是一般 C++ 類別/結構/列舉。

專案中的任何 XAML 頁面都必須是 Windows 執行階段類型，因此 **MainPage** 必須是 Windows 執行階段類型。 在 C++/WinRT 專案中，**MainPage** 已經是 Windows 執行階段類型，因此我們不需要變更其外觀比例。 具體而言，這是*執行階段類別*。

- 如需有關是否要撰寫特定類別之執行階段類別的詳細資訊，請參閱[使用 C++/WinRT 撰寫 API](./author-apis.md) 主題。
- 在 C++/WinRT 中，執行階段類別的內部實作，以及其投影 (公用) 部分，會以兩個不同類別的形式存在。 這些也稱為「實作類型」和「投影類型」。 您可以在上述要點提到的主題，以及[使用 C++/WinRT 取用 API](./consume-apis.md) 中進一步了解這些類型。
- 如需有關執行階段類別和 IDL( `.idl` 檔案) 之間連線的詳細資訊，請閱讀並遵循 [XAML 控制項；繫結至一個 C++/WinRT 屬性](./binding-property.md)主題。 該主題會逐步解說撰寫新執行階段類別的程序，其第一步是要將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。

對於 **MainPage**，我們在 C++/WinRT 專案中已經有所需的 `MainPage.idl` 檔案。 這是因為專案範本為我們建立了該檔案。 但稍後在此逐步解說中，我們會將進一步的 `.idl` 檔案新增至專案。

我們很快將會看到一份清單，列出我們需要將哪些 IDL 新增至現有的 `MainPage.idl` 檔案。 在那之前，我們要推論在 IDL 中需要做什麼和不需要做什麼。

若要判斷我們需要在 `MainPage.idl` 中宣告 **MainPage** 的哪些成員 (使其成為 **MainPage** 執行階段類別的一部分)，而且可以直接成為 **MainPage** 實作類型的成員，讓我們製作一份 C# **MainPage** 類別的成員清單。 我們會藉由查看 `MainPage.xaml.cs` 和 `SampleConfiguration.cs` 來尋找這些成員。

我們總共找到十二個 `protected` 和 `private` 欄位與方法。 而且我們還找到下列 `public` 成員。

- 預設建構函式 `MainPage()`。
- 靜態欄位 **Current** 和 **FEATURE_NAME**。
- **IsClipboardContentChangedEnabled** 和 **Scenarios** 屬性。
- **BuildClipboardFormatsOutputString**、**DisplayToast**、**EnableClipboardContentChangedNotifications** 和 **NotifyUser** 方法。

這是在 `MainPage.idl` 中宣告之候選項目的 `public` 成員。 因此，讓我們檢查上述每一個，並查看其是需要成為 **MainPage** 執行階段類別的一部分，還是只需要成為其實作的一部分。

- 預設建構函式 `MainPage()`。 對於 XAML **頁面**而言，在其 IDL 中宣告預設建構函式是正常的。 如此一來，XAML UI 架構就可以啟用該類型。
- 靜態欄位 **Current** 是從個別案例 XAML 頁面中使用的，以存取應用程式的 **MainPage** 執行個體。 **Current** 不是用來與 XAML 架構交互操作的 (也不是跨編譯單位使用的)，因此我們可以將其保留，僅作為實作類型的成員。 如果您自己的專案是這樣的情況，您可能會選擇那麼做。 但是此欄位是投射類型的執行個體，因此在 IDL 中宣告該欄位是合乎邏輯的。 這就是我們要在這裡執行的動作 (而且這麼做也會讓程式碼更簡潔)。
- 靜態 **FEATURE_NAME** 欄位的情況類似，其是在 **MainPage** 類型中存取的。 同樣地，選擇在 IDL 中宣告該欄位會使我們的程式碼更簡潔。
- **IsClipboardContentChangedEnabled** 屬性僅用於 **OtherScenarios** 類別。 因此在移植期間，我們將簡化一些動作，並將其設為 **OtherScenarios** 執行階段類別的私用欄位。 如此便不會將該欄位放入 IDL 中。
- **Scenarios** 屬性是 **Scenario** 類型 (我們稍早提到的類型) 的物件集合。 我們將在下一個小節討論 **Scenario**，因此，在此之前，也讓我們先保留 **Scenarios** 屬性。
- **BuildClipboardFormatsOutputString**、**DisplayToast** 和 **EnableClipboardContentChangedNotifications** 方法是公用程式函式，相較於主頁面，這些函式對於範例的一般狀態更相關。 因此在移植期間，我們會將這三種方法重構至名為 **SampleState** 的新公用程式類型 (不需要是 Windows 執行階段類型)。 基於這個理由，這三種方法不會放入 IDL 中。
- **NotifyUser** 方法是從靜態 *Current* 欄位傳回之 **MainPage** 執行個體上的個別案例 XAML 頁面中呼叫的。 (如先前所述) **Current** 是投影類型的執行個體，因此我們必須在 IDL 中宣告 **NotifyUser**。 **NotifyUser** 接受 **NotifyType** 類型的參數。 我們將在下一個小節中討論這一點。

您想要對其進行資料繫結的任何成員也必須在 IDL 中宣告 (無論使用 `{x:Bind}` 或 `{Binding}`)。 如需詳細資訊，請參閱[資料繫結](../data-binding/index.md)。

我們的進度：我們正在開發要在 `MainPage.idl` 檔案中新增及不新增哪些成員的清單。 但我們還是必須討論 **Scenarios** 屬性，以及 **NotifyType** 類型。 那麼，接下來就讓我們來看看。

### <a name="idl-for-the-scenario-and-notifytype-types"></a>**Scenario** 和 **NotifyType** 類型的 IDL

**Scenario** 類別是在 `SampleConfiguration.cs` 中定義的。 我們必須決定如何將該類別移植到 C++/WinRT。 根據預設，我們可能會將其設為一般 C++ `struct`。 但是，如果在二進位檔之間使用 **Scenario**，或要與 XAML 架構交互操作，則必須在 IDL 中宣告為 Windows 執行階段類型。

研究 C# 原始程式碼時，我們發現此內容中使用了 **Scenario**。

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

**Scenario** 物件的集合將指派給 **ListBox** (屬於項目控制項) 的 [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性。 **Scenario** *需要*與 XAML 交互操作，因此必須是 Windows 執行階段類型。 因而必須在 IDL 中定義。 在 IDL 中定義 **Scenario** 類型，會使 C++/WinRT 組建系統在幕後標頭檔 (其名稱和位置在此逐步解說中不重要) 中為您產生 **Scenario** 的原始程式碼定義。

回想一下，**MainPage.Scenarios** 是 **Scenario** 物件的集合，我們剛才說過，這些物件必須在 IDL 中。 基於這個理由，您也必須在 IDL 中宣告 **MainPage.Scenarios**。

**NotifyType** 是在 C# 的 `MainPage.xaml.cs` 中宣告的 `enum`。 我們會將 **NotifyType** 傳遞至屬於 **MainPage** 執行階段類別的方法，因此 **NotifyType** 也必須是 Windows 執行階段類型，而且必須在 `MainPage.idl` 中定義。

現在，讓我們將新的類型以及我們決定要在 IDL 中宣告之 **Mainpage** 的新成員新增至 `MainPage.idl` 檔案。 同時，我們會將 Visual Studio 專案範本提供給我們的 **Mainpage** 的預留位置成員從 IDL 中移除。

因此，在您的 C++/WinRT 專案中，開啟 `MainPage.idl` 並加以編輯，使其看起來像下面的清單。 請注意，其中一項編輯是將命名空間名稱從 **Clipboard** 變更為 **SDKTemplate**。 如有需要，您可以使用下列程式碼來取代 `MainPage.idl` 的整個內容。 另一個要注意的調整是，我們要將 **Scenario::ClassType** 的名稱變更為 **Scenario::ClassName**。

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> 如需有關 C++/WinRT 專案中 `.idl` 檔案內容的詳細資訊，請參閱 [Microsoft 介面定義語言 3.0](/uwp/midl-3/)。

有了您自己的移植工作，您可能不想要也不需要變更命名空間名稱，就像我們先前所做的一樣。 我們在這裡這麼做只是因為我們要移植之 C# 專案的預設命名空間是 **SDKTemplate**，而專案和組件的名稱是 **Clipboard**。

但是，當我們繼續在此逐步解說中進行移植時，我們會將原始程式碼中出現的每個 **Clipboard** 命名空間名稱變更為 **SDKTemplate**。 在 C++/WinRT 專案屬性中也有一個地方會出現 **Clipboard** 命名空間名稱，因此我們現在將會利用這個機會變更該名稱。

在 Visual Studio 中，針對 C++/WinRT 專案，將專案屬性 [通用屬性] ****  \>[C++/WinRT] ****  \>[根命名空間] **** 設定為值 *SDKTemplate*。

### <a name="save-the-idl-and-re-generate-stub-files"></a>儲存 IDL 並重新產生 stub 檔案

[XAML 控制項；繫結至 C++/WinRT 屬性](./binding-property.md)主題引進「stub 檔案」的概念，並顯示其運作方式的逐步解說。 我們在本主題稍早提到 C++/WinRT 建置系統會將 `.idl` 檔案的內容轉換成 Windows 中繼資料，然後該中繼資料中名為 `cppwinrt.exe` 的工具會產生可作為您的實作基礎的 stub 時，也提到了 stub。

每次在您的 IDL 和組建中進行新增、移除或變更時，建置系統都會更新這些 stub 檔案中的 stub 實作。 因此，每當您變更 IDL 和組建時，我們建議您檢視這些 stub 檔案、複製任何已變更的簽章，並將其貼到您的專案中。 我們稍後會提供更多有關確切做法的細節和範例。 但這麼做的好處是，讓您隨時都能順利得知您的實作類型應有的形式，以及其方法的簽章為何。

目前在此逐步解說中，我們已經暫時完成編輯 `MainPage.idl` 檔案，因此您應該立即加以儲存。 專案目前還沒有建置完成，但是現在執行建置很有幫助，因為該專案會重新產生 **MainPage** 的 stub 檔案。

針對這個 C++/WinRT 專案，這些 stub 檔案是在 `\Clipboard\Clipboard\Generated Files\sources` 資料夾中產生的。 在部分建置完成之後，您將會在該處找到這些檔案 (同樣地，如預期般，建置不會完全成功。 但我們想要&mdash;產生 stub&mdash; 的步驟*將*已經成功)。 我們感興趣的檔案是 `MainPage.h` 和 `MainPage.cpp`。

在這兩個 stub 檔案中，您將會看到我們新增至 IDL (例如，**Current** 和 **FEATURE_NAME**) 之 **MainPage** 成員的新 stub 實作。 您會想要將這些 stub 實作複製到已經存在於專案的 `MainPage.h` 和 `MainPage.cpp` 檔案中。 同時，如同我們對 IDL 的處理方式，我們將從這些現有的檔案中移除 Visual Studio 專案範本提供給我們的 **Mainpage** 預留位置成員 (名為 **MyProperty** 的虛擬屬性，以及名為 **ClickHandler** 的事件處理常式)。

事實上，我們想要保留的最新版 **MainPage** 的唯一成員是建構函式。

當您從 stub 檔案複製新成員、刪除不想要的成員，並更新命名空間之後，您專案中的 `MainPage.h` 和 `MainPage.cpp` 檔案看起來應該像以下列出的程式碼。 請注意，有兩個 **MainPage** 類型。 其中一個在**實作**命名空間，另一個在 **factory_implementation** 命名空間中。 請注意，我們對 **factory_implementation** 類型所做的唯一變更是在其命名空間中新增 **SDKTemplate**。

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

若為字串，C# 會使用 **System.String**。 如需範例，請參閱 **MainPage.NotifyUser** 方法。 在我們的 IDL 中，我們會以 **String** 宣告字串，而當 `cppwinrt.exe` 工具為我們產生 C++/WinRT 程式碼時，其會使用 [**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring) 類型。 每當我們在 C# 程式碼中遇到字串時，我們會將其移植到 **winrt::hstring**。 如需詳細資訊，請參閱 [C++/WinRT 中的字串處理](./strings.md)。

如需方法簽章中 `const&` 參數的說明，請參閱[參數傳遞](./concurrency.md#parameter-passing)。

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>更新其餘所有命名空間宣告/參考並建置

建置 C++/WinRT 專案之前，請先尋找 **Clipboard** 命名空間的任何宣告和參考，然後將其變更為 **SDKTemplate**。

- `MainPage.xaml` 和 `App.xaml`。 命名空間會出現在 `x:Class` 和 `xmlns:local` 屬性的值中。
- `App.idl`。
- `App.h`。
- `App.cpp`。 有兩個 `using namespace` 指示詞 (搜尋子字串 `using namespace Clipboard`) 和兩個 **MainPage** 類型的資格 (搜尋 `Clipboard::MainPage`)。 這些需要變更。

我們已從 **MainPage** 移除事件處理常式，因此也請移至 `MainPage.xaml`，並從標記中刪除 **Button** 元素。

儲存所有檔案。 清除方案 ([建置] > [清除剪貼簿]) 後再建立該方案。 如果您到目前為止所做的變更都有效，建置將會成功。

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>實作我們在 IDL 中宣告的 **MainPage** 成員

#### <a name="the-constructor-current-and-feature_name"></a>建構函式、**Current** 和 **FEATURE_NAME**

以下是我們需要移植的相關程式碼 (來自 C# 專案)。

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

我們很快將全部重複使用 `MainPage.xaml` (藉由複製)。 目前，我們會暫時將具有適當名稱的 **TextBlock** 元素新增到 C++/WinRT 專案的 `MainPage.xaml` 中。

**FEATURE_NAME** 是 **MainPage** 的靜態欄位 (C# `const` 欄位在其行為上，基本上是靜態的)，定義於 `SampleConfiguration.cs` 中。 對於 C++/WinRT (而不是 (靜態) 欄位)，我們會將其設為 (靜態) 唯讀屬性的 C++/WinRT 運算式。 表示屬性 getter 的 C++/WinRT 方式就是當作傳回屬性值的函式，而且不接受任何參數 (存取子)。 因此，C# **FEATURE_NAME** 靜態欄位會變成 C++/WinRT **FEATURE_NAME** 靜態存取子函式 (在此案例中，會傳回字串常值)。

順便一提，如果我們要移植 C# 唯讀屬性，就會執行相同的動作。 若為 C# 可寫入的屬性，表示屬性 setter 的 C++/WinRT 方式就是作為採用屬性值作為參數 (變動子) 的 `void` 函式。 不論是哪一種情況，如果 C# 欄位或屬性是靜態的，則 C++/WinRT 存取子和/或變動子也是靜態的。

**Current** 是 **MainPage** 的靜態 (非常數) 欄位。 同樣地，我們會將其 (C++/WinRT 運算式) 設為唯讀屬性，然後再將其設為靜態。 其中 **FEATURE_NAME** 是常數，**Current** 則不是。 因此在 C++/WinRT 中，我們需要一個支援欄位，而且我們的存取子將會傳回該欄位。 所以在 C++/WinRT 專案中，我們將在 `MainPage.h` 中宣告一個名為 **current** 的私用靜態欄位、我們將在 `MainPage.cpp` 中定義/初始化 **current** (因為其具有靜態儲存持續時間)，而我們將透過名為 **current** 的公用靜態存取子函式加以存取。

此建構函式本身會執行一些指派，可以直接移植。

在 C++/WinRT 專案中，加入名稱為 `SampleConfiguration.cpp` 的新 **Visual C++**  > **程式碼** > **C++ 檔案 (.cpp)** 項目。

編輯 `MainPage.xaml`、`MainPage.h`、`MainPage.cpp` 和 `SampleConfiguration.cpp`，以符合下列清單。

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

此外，請務必從 **MainPage::Current()** 和 **MainPage::FEATURE_NAME()** 的 `MainPage.cpp` 中刪除現有的函式主體，因為我們現在會在別處定義這些方法。

如您所見，**MainPage::current** 宣告為屬於 **SDKTemplate::MainPage** 類型，這是投影類型。 其不屬於 **SDKTemplate::implementation::MainPage** 類型，這是實作類型。 投影類型是針對用於 XAML 交互操作專案中或跨二進位檔而設計的類型。 實作類型是您用來實作您已在投影類型上公開之設備的類型。 由於 **MainPage::current** (在 `MainPage.h` 中) 的宣告出現在實作命名空間 (**winrt::SDKTemplate::implementation**) 中，因此不合格的 **MainPage** 會參考實作類型。 所以，我們符合 **SDKTemplate::** 的資格，以清楚指出我們希望 **MainPage::current** 是 **winrt::SDKTemplate::MainPage** 投影類型的執行個體。

在建構函式中，有一些與 `MainPage::current = *this;` 相關的重點需要說明。
- 當您在實作類型的成員內部使用 `this` 指標時，`this` 指標當然是*實作類型的指標*。
- 若要將 `this` 指標轉換為對應的投影類型，請將其取值。 假設您從 IDL 產生您的實作類型 (如我們在這裡所述)，則實作類型的轉換運算子會轉換成其投影類型。 這就是為什麼指派在這裡運作的原因。

如需有關這些細節的詳細資訊，請參閱[具現化並傳回實作類型與介面](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces)。

此外，在建構函式中為 `SampleTitle().Text(FEATURE_NAME());`。 `SampleTitle()` 部分對名為 **SampleTitle** 的簡單存取子函式的呼叫，此函式會傳回我們新增至 XAML 的 **TextBlock**。 每當您 `x:Name` XAML 元素時，XAML 編譯器就會為您產生為該元素命名的存取子。 `.Text(...)` 部分會在 **SampleTitle** 存取子傳回的 **TextBlock** 物件上，呼叫 **Text** 更動子函式。 而 `FEATURE_NAME()` 會呼叫我們的靜態 **MainPage::FEATURE_NAME** 存取子函式，以傳回字串常值。 總之，該行程式碼會設定名為 *SampleTitle* 之 **TextBlock** 的 **Text** 屬性。

請注意，字串在 Windows 執行階段中是寬的，若要移植字串常值，我們會在其前面加上寬字元編碼的首碼 `L`。 因此，我們將「字串常值」變更為 L「字串常值」。 另請參閱[寬字元串常值](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals)。

#### <a name="scenarios"></a>**案例**

以下是我們需要移植的相關 C# 程式碼。

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

從先前的調查中，我們知道 **Scenario** 物件的這個集合會顯示在 **ListBox** 中。 在 C++/WinRT 中，我們可以指派給項目控制項的 **ItemsSource** 屬性的*集合種類*有一些限制。 此集合必須是向量或可觀察的向量，而且其元素必須為下列其中一項。

- 執行階段類別，或
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。

針對 **IInspectable** 案例，如果元素本身不是執行階段類別，則這些元素的類型必須可在 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 中進行 Box 和 Bnbox 處理。 也就是說，這些必須是 Windows 執行階段類型 (請參閱[將純量數值 Boxing 和 unboxing 到 IInspectable](./boxing.md))。

在此案例研究中，我們不會將 **Scenario** 設為執行階段類別。 但是，這仍然是合理的選項。 而且在您自己的移植工作中，有時候一定會遇到執行階段類別。 例如，如果您需要讓元素類型變成*可觀察的* (請參閱 [XAML 控制項；繫結至一個 C++/WinRT 屬性](./binding-property.md))，或如果元素因為其他任何原因而需要有方法，而且不只是一組資料成員，就會遇到執行階段類別。

在此逐步解說中，我們*不會*將執行階段類別用於 **Scenario** 類型，因此我們需要考慮如何進行 Box 處理。 如果我們將 **Scenario** 設為一般 C++ `struct`，就無法為其進行 Box 處理。 但我們將 **Scenario** 宣告為 IDL 中的 `struct`，因此我們*可以*進行 Box 處理。

我們可以選擇為 **Scenario** 預先進行 Box 處理，或等到我們即將指派給 **ItemsSource**，然後再及時為其進行 Box 處理。 以下是有關這兩個選項的一些考量。

- 預先進行 Box 處理。 針對此選項，我們的資料成員是 **IInspectable** 的集合，準備好要指派給 UI。 在初始化時，我們會在該資料成員中，為 **Scenario** 物件進行 Box 處理。 我們只需要該集合的一個複本，但我們必須在每次需要讀取其欄位時，為元素進行 Unbox 處理。
- 及時進行 Box 處理。 針對此選項，我們的資料成員是 **Scenario** 的集合。 當您要指派給 UI 時，我們會將 **Scenario** 物件從資料成員 Box 到 **IInspectable** 的新集合中。 我們可以讀取資料成員中的元素欄位，而不需進行 Unbox 處理，但我們需要該集合的兩個複本。

如您所見，對於像這樣的小型集合，優缺點都使其站得住腳。 因此，在此案例研究中，我們將會使用及時選項。

**Scenarios** 成員是 **MainPage** 的一個欄位，在 `SampleConfiguration.cs` 中定義並初始化。 而且 **Scenarios** 是 **MainPage** 的一個唯讀屬性，在 `MainPage.xaml.cs` 中定義 (並實作以直接傳回 **scenarios** 欄位)。 我們將在 C++/WinRT 專案中執行類似的動作；但我們會將這兩個成員設為靜態 (因為我們在應用程式中只需要一個執行個體，因此我們可以存取這些成員，而不需要類別執行個體)。 此外，我們會分別將其命名為 *scenariosInner* 和 *scenarios*。 我們將會在 `MainPage.h` 中宣告 *scenariosInner*。 而且，因為該成員有靜態儲存持續時間，所以我們會在 `.cpp` 檔案 (在此案例中為 `SampleConfiguration.cpp`) 中定義/初始化該成員。

編輯 `MainPage.h` 和 `SampleConfiguration.cpp`，以符合下列清單。

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

此外，請務必從 **MainPage::scenarios()** 的 `MainPage.cpp` 中刪除現有的函式主體，因為我們現在是在標頭檔中定義該方法。

如您所見，在 `SampleConfiguration.cpp` 中，我們會藉由呼叫名為 [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 的 C++/WinRT helper 函式，初始化靜態資料成員 *scenariosInner*。 該函式會為我們建立新的 Windows 執行階段集合物件，並以 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) 介面的形式傳回。 在此範例中，集合不是*可觀察的* (不需要是，因為其不會在初始化之後新增或移除元素)，因此我們可以改為選擇呼叫 [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)。 該函式會以 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) 介面的形式傳回集合。

如需有關集合以及繫結至集合的詳細資訊，請參閱 [XAML 項目控制項；繫結至一個 C++/WinRT 集合](./binding-collection.md)和[使用 C++/WinRT 的集合](./collections.md)。

您剛剛加入的初始化程式碼會參考尚未存在於專案中的類型 (例如，**winrt::SDKTemplate::CopyText**)。 為解決這個問題，讓我們將五個新的空白 XAML 頁面新增至專案。

#### <a name="add-five-new-blank-xaml-pages"></a>新增五個新的空白 XAML 頁面

將新的 [XAML] > [空白頁面 (C++/WinRT)]  項目新增至專案 (確定是 [空白頁面 (C++/WinRT)] 項目範本，而不是 [空白頁面] 項目範本)。 將其命名為  `CopyText`。 新的 XAML 頁面是在 **SDKTemplate** 命名空間內定義的，這就是我們想要的。

再重複上述程序四次，並將 XAML 頁面命名為 `CopyImage`、`CopyFiles`、`HistoryAndRoaming` 和 `OtherScenarios`。

如果需要，您現在可以再次進行建置。

#### <a name="notifyuser"></a>**NotifyUser**

在 C# 專案中，您可以在 `MainPage.xaml.cs` 內找到 **MainPage.NotifyUser** 方法的實作。 **MainPage.NotifyUser** 與 **MainPage.UpdateStatus** 相依，然後該方法會與我們還未移植的 XAML 元素相依。 因此，現在我們只會清除 C++/WinRT 專案中的 **UpdateStatus** 方法，而且我們之後將會進行移植。

以下是我們需要移植的相關 C# 程式碼。

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser** 會使用 [**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority) 列舉。 在 C++/WinRT 中，每當您想要使用 Windows 命名空間中的類型時，您必須納入對應的 C++/WinRT Windows 命名空間標頭檔 (如需相關的詳細資訊，請參閱[開始使用 C++/WinRT](./get-started.md))。 在此情況下，如同您在以下的程式碼清單中所見，標頭是 `winrt/Windows.UI.Core.h`，而且我們會將其納入 `pch.h` 中。

**UpdateStatus** 是私用的。 因此，我們將會在我們的 **MainPage** 實作類型上，將其設為私用方法。 **UpdateStatus** 並非要在執行階段類別上呼叫，因此我們將不會在 IDL 中宣告。

在移植 **MainPage.NotifyUser**，並清除 **MainPage.UpdateStatus** 之後，這就是我們在 C++/WinRT 專案中的內容。 在此程式碼清單之後，我們將檢查一些細節。

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

在 C# 中，您可以使用點標記法來「點入」巢狀屬性。 因此，C# **MainPage** 類型可以使用語法 `Dispatcher` 存取自己的 **Dispatcher** 屬性。 而 C# 則可以使用 `Dispatcher.HasThreadAccess` 之類的語法，進一步*進入*該值。 在 C++/WinRT 中，系統會將屬性當作存取子函式實作，因此語法的不同之處僅在於為每個函式呼叫加上括號。

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

當 C# 版本的 **NotifyUser** 呼叫 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 時，其會將非同步回呼委派當作 lambda 函式實作。 C++/WinRT 版本會執行相同的工作，但語法稍有不同。 在 C++/WinRT 中，我們會*擷取*我們將使用的兩個參數，以及 `this` 指標 (因為我們要呼叫成員函式)。 如需有關將委派當作 lambda 實作的詳細資訊，以及程式碼範例，請參閱[藉由在 C++/WinRT 使用委派來處理事件](./handle-events.md)。 此外，我們也可以忽略此特定案例中的 `var task =` 部分。 我們不會等候傳回的非同步物件，因此不需要將其儲存起來。 

### <a name="implement-the-remaining-mainpage-members"></a>實作剩餘的 **MainPage** 成員

讓我們建立 **MainPage** (跨 `MainPage.xaml.cs` 和 `SampleConfiguration.cs` 實作) 成員的完整清單，如此我們可以查看到目前為止已移植的成員，以及還未移植的成員。

|成員|存取權|狀態|
|-|-|-|
|**MainPage** 建構函式|`public`|已移植|
|**Current** 屬性|`public`|已移植|
|**FEATURE_NAME** 屬性|`public`|已移植|
|**IsClipboardContentChangedEnabled** 屬性|`public`|尚未開始|
|**Scenarios** 屬性|`public`|已移植|
|**BuildClipboardFormatsOutputString** 方法|`public`|尚未開始|
|**DisplayToast** 方法|`public`|尚未開始|
|**EnableClipboardContentChangedNotifications** 方法|`public`|尚未開始|
|**NotifyUser** 方法|`public`|已移植|
|**OnNavigatedTo** 方法|`protected`|尚未開始|
|**isApplicationWindowActive** 欄位|`private`|尚未開始|
|**needToPrintClipboardFormat** 欄位|`private`|尚未開始|
|**scenarios** 欄位|`private`|已移植|
|**Button_Click** 方法|`private`|尚未開始|
|**DisplayChangedFormats** 方法|`private`|尚未開始|
|**Footer_Click** 方法|`private`|尚未開始|
|**HandleClipboardChanged** 方法|`private`|尚未開始|
|**OnClipboardChanged** 方法|`private`|尚未開始|
|**OnWindowActivated** 方法|`private`|尚未開始|
|**ScenarioControl_SelectionChanged** 方法|`private`|尚未開始|
|**UpdateStatus** 方法|`private`|已清除|

接著，我們將在接下來的幾個小節中討論尚未移植的成員。

> [!NOTE]
> 有時候，我們會在原始程式碼中遇到 XAML 標記中 UI 元素的參考 (在 `MainPage.xaml` 中)。 當我們提到這些參考時，會將簡單的預留位置元素加入至 XAML，暫時解決這些問題。 如此一來，專案將會在每個子區段之後繼續建置。 替代方法是立即將 `MainPage.xaml` 的*整個*內容從 C# 專案複製到 C++/WinRT 專案，以解析參考。 但是，如果這麼做，需要很長的時間才能休息並再次建置 (因此可能會遮蔽我們在過程中所造成的任何錯字或其他錯誤)。
>
> 一旦完成移植 **MainPage** 類別的命令式程式碼之後，*則*我們將會複製 XAML 檔案的內容，並確信該專案仍然會建置。

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

這是預設為 `false` 的 get-set C# 屬性。 此屬性是 **MainPage** 的成員，而且是在 `SampleConfiguration.cs` 中定義的。

若是 C++/WinRT，我們需要一個存取子函式、一個更動子函式，以及一個支援資料成員作為欄位。 由於 **IsClipboardContentChangedEnabled** 代表範例中其中一個案例的狀態，而不是 **MainPage** 本身的狀態，因此我們將針對一個稱為 **SampleState** 的新公用程式類型，建立新的成員。 此外，我們將在我們的 `SampleConfiguration.cpp` 原始程式碼中進行實作，而且我們會建立成員 `static` (因為我們在應用程式中只需要一個執行個體，因此我們可以存取這些成員，而不需要類別執行個體)。

若要在 C++/WinRT 專案中搭配我們的 `SampleConfiguration.cpp`，請加入名稱為 `SampleConfiguration.h` 的新 **Visual C++**  > **程式碼** > **標頭檔 (.h)** 項目。 編輯 `SampleConfiguration.h` 和 `SampleConfiguration.cpp`，以符合下列清單。

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

同樣地，具有 `static` 儲存體的欄位 (例如 **SampleState::isClipboardContentChangedEnabled**) 必須在應用程式中定義一次，而 `.cpp` 檔案是進行該作業的好位置 (在此案例中為 `SampleConfiguration.cpp`)。

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

此方法是 **MainPage** 的公用成員，而且是在 `SampleConfiguration.cs` 中定義的。

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

在 C++/WinRT 中，我們會將 **BuildClipboardFormatsOutputString** 設為 **SampleState** 的公用靜態方法。 我們可以將其設為 `static`，因為其不會存取任何執行個體成員。

若要在 C++/WinRT 中使用 **Clipboard** 和 **DataPackageView** 類型，我們必須納入 C++/WinRT Windows 命名空間標頭檔 `winrt/Windows.ApplicationModel.DataTransfer.h`。

在 C# 中，**DataPackageView.AvailableFormats** 屬性是 **IReadOnlyList**，因此我們可以存取其 **Count** 屬性。 在 C++/WinRT 中，**DataPackageView::AvailableFormats** 存取子函式會傳回 **IVectorView**，其具有我們可以呼叫的 **Size** 存取子函式。

若要移植 C# **System.Text.StringBuilder** 類型的用法，我們將使用標準 C++ 類型 [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream)。 該類型是寬字串的輸出資料流 (若要使用該類型，我們必須納入 `sstream` 標頭檔)。 您不要使用 **Append** 方法 (如同您處理 **StringBuilder** 一樣)，而是要使用[插入運算子](/cpp/standard-library/using-insertion-operators-and-controlling-format) (`<<`) 搭配輸出資料流，例如 **wostringstream**。 如需詳細資訊，請參閱 [iostream 程式設計](/cpp/standard-library/iostream-programming)和[設定 C++/WinRT 字串的格式](./strings.md#formatting-strings)。

C# 程式碼會使用 `new` 關鍵字建構 **StringBuilder**。 在 C# 中，物件預設是參考類型，並使用 `new`，在堆積上宣告。 在新式標準 C++ 中，物件預設為值類型，在堆疊上宣告 (不使用 `new`)。 因此，我們會將 `StringBuilder output = new StringBuilder();` 單純地移植到 C++/WinRT，作為 `std::wostringstream output;`。

C# `var` 關鍵字會要求編譯器推斷類型。 您可以在 C++/WinRT 中將 `var` 移植到 `auto`。 但在 C++/WinRT 中，有一些情況 (為了避免複製) 會讓想*參考*推斷 (或推算) 的類型，並以 `auto&` 表示 lvalue 參考推算類型。 也有一些情況下，您想要一種特殊並正確繫結的參考，不論它是使用 *lvalue*，還是使用 *rvalue* 進行初始化。 另外，您可以使用 `auto&&` 來表示該參考。 這是您看到的格式，用於下面移植程式碼中的 `for` 迴圈。 如需 *lvalues* 和 *rvalues* 的簡介，請參閱[值類別和它們的參考](./cpp-value-categories.md)。

編輯 `pch.h`、`SampleConfiguration.h` 和 `SampleConfiguration.cpp`，以符合下列清單。

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> 程式碼 `DataPackageView clipboardContent{ Clipboard::GetContent() };` 行中的語法使用稱為 *統一初始化*的新式標準 C++ 功能，其特性是使用大括弧，而不是 `=` 符號。 該語法清楚指出初始化 (而不是指派) 正在進行中。 如果您偏好*看起來*像是指派的語法格式 (但實際上並不是)，則可以將上述語法取代為對等的 `DataPackageView clipboardContent = Clipboard::GetContent();`。 但是，使用這兩種方式來表示初始化是很好的主意，因為您可能會看到這兩種方式經常在您遇到的程式碼中使用。

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast** 是 C# **MainPage** 類別的公用靜態方法，而且是在 `SampleConfiguration.cs` 中定義的。 在 C++/WinRT 中，我們會將其設為 **SampleState** 的公用靜態方法。

我們已經說明與移植此方法相關的大部分細節和技術。 要注意的一個新項目是，您會將 C# 逐字字串常值 (`@`) 移植到標準 C++ [原始字串常值](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11) (`LR`)。

此外，當您在 C++/WinRT 中參考 [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) 和 [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) 類型時，您可以依命名空間名稱來限定這些類型，也可以編輯 `SampleConfiguration.cpp` 並加入 `using namespace` 指示詞，如下列範例所示。

```cppwinrt
using namespace Windows::UI::Notifications;
```

當您參考 [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) 類型時，以及每當您參考其他任何 Windows 執行階段類型時，都會有相同的選擇。

除了這些項目之外，只要依照您先前執行的相同指導方針，就可以完成下列步驟。

- 在 `SampleConfiguration.h` 中宣告方法，並在 `SampleConfiguration.cpp` 中加以定義。
- 編輯 `pch.h` 以納入任何所需的 C++/WinRT Windows 命名空間標頭檔。
- 在堆疊上建構 C++/WinRT 物件，而不是在堆積上。
- 將屬性 get 存取子的呼叫取代為 function-call 語法 (`()`)。

編譯器/連結器錯誤的一個非常常見的原因是忘記納入所需的 C++/WinRT Windows 命名空間標頭檔。 如需有關一個可能錯誤的詳細資訊，請參閱[為何連結器顯示「LNK2019:無法解析的外部符號」錯誤？](./faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)。

如果您想要依照逐步解說，自行移植 **DisplayToast**，可以將結果與您所下載之[剪貼簿範例](/samples/microsoft/windows-universal-samples/clipboard/)原始程式碼 ZIP 中 C++/WinRT 版本的程式碼進行比較。

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications** 是 C# **MainPage** 類別的公用靜態方法，而且是在 `SampleConfiguration.cs` 中定義的。

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

在 C++/WinRT 中，我們會將其設為 **SampleState** 的公用靜態方法。

在 C# 中，您可以使用 `+=` 和 `-=` 運算子語法來註冊和撤銷事件處理委派。 在 C++/WinRT 中，您有數個語法選項可供您註冊/撤銷委派，如[藉由在 C++/WinRT 使用委派來處理事件](./handle-events.md)中所述。 但是一般的形式是，您呼叫針對事件命名的一對函數來進行註冊及撤銷。 若要註冊，您可以將委派傳遞至註冊函數，然後擷取撤銷權杖作為回報 ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token))。 若要撤銷，請將該權杖傳遞至撤銷函數。 在此情況下，處理常式是靜態的，而且 (如您在下列程式碼清單中所見) 函式呼叫語法很簡單。

在 C#，實際上「會」使用類似的權杖。 但程式語言會隱含該詳細資料。 C++/WinRT 會讓其明確顯示。

**object** 類型會出現在 C# 事件處理常式簽章中。 在 C# 語言中，**object** 是 .NET [**System.Object**](/dotnet/api/system.object) 類型的[別名](/dotnet/csharp/language-reference/builtin-types/reference-types)。 C++/WinRT 中的對等項是 [**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)。 因此，您將會在 C++/WinRT 事件處理常式中看到 **IInspectable**。

編輯 `SampleConfiguration.h` 和 `SampleConfiguration.cpp`，以符合下列清單。

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

將事件處理委派本身 (**OnClipboardChanged** 和 **OnWindowActivated**) 暫時保留為 stub。 這些委派已經在我們要移植的成員清單上，所以我們將會在稍後的小節中加以說明。

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo** 是 C# **MainPage** 類別的受保護方法，而且是在 `MainPage.xaml.cs` 中定義的。 在這裡，該方法會搭配所參考的 XAML **ListBox** 一起使用。

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

這是一個重要且有趣的方法，因為這是 **Scenario** 物件集合指派給 UI 的所在位置。 C# 程式碼會建置 **Scenario** 物件的 [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1)，並將其指派給 **ListBox** (也就是項目控制項) 的 [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性。 此外，在 C# 中，我們使用[字串插值 ](/dotnet/csharp/language-reference/tokens/interpolated)，為每個 **Scenario** 物件建置標題 (請注意 `$` 特殊字元的使用)。

在 C++/WinRT 中，我們會將 **OnNavigatedTo** 設為 **MainPage** 的公用方法。 我們會將 stub **ListBox** 元素加入至 XAML，讓組建成功。 在此程式碼清單之後，我們將檢查一些細節。

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

同樣地，我們將呼叫 [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 函式，但這次是建立 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 的集合。 這是我們所做的決定之一，就是以及時的方式，對 **Scenario** 物件執行 Box。

我們結合使用 [**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 函數與 **winrt::hstring** 的[串連運算子](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)，取代 C# 在這裡使用的[字串插補](/dotnet/csharp/language-reference/tokens/interpolated)。

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

在 C# 中，**isApplicationWindowActive** 是屬於 **MainPage** 類別的簡單私用 `bool` 欄位，而且是在 `SampleConfiguration.cs` 中定義的。 其預設為 `false`。 在 C++/WinRT 中，我們會在 `SampleConfiguration.h` 和 `SampleConfiguration.cpp` 檔案中使用相同的預設值，將其設為 **SampleState** 的公用靜態欄位 (基於我們已經描述的原因)。

我們已經看過如何宣告、定義和初始化靜態欄位。 如需重新整理程式，請回顧我們對 **isClipboardContentChangedEnabled** 欄位所執行的動作，並對 **isApplicationWindowActive** 執行相同的動作。

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

與 **isApplicationWindowActive** 相同的模式 (請參閱緊接在此標題前的標題)。

#### <a name="button_click"></a>**Button_Click**

**Button_Click** 是 C# **MainPage** 類別的私用 (事件處理) 方法，而且是在 `MainPage.xaml.cs` 中定義的。 在這裡，其會連同所參考的 XAML **SplitView**，以及加以註冊的 **ToggleButton**。

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
<ToggleButton Click="Button_Click" .../>
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

以及移植到 C++/WinRT 的對等項目。 請注意，在 C++/WinRT 版本中，事件處理常式為 `public` (如您所見，您是在`private:`宣告*之前*，將其宣告)。 這是因為在 XAML 標記中註冊的事件處理常式 (例如這一項) 必須在 C++/WinRT 中為 `public`，XAML 標記才能存取。 如果您在命令式程式碼中註冊事件處理常式 (如同我們在先前的 **MainPage::EnableClipboardContentChangedNotifications** 中進行的)，則事件處理常式無需為 `public`。

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

在 C# 中，**DisplayChangedFormats** 是屬於 **MainPage** 類別的私用方法，而且是在 `SampleConfiguration.cs` 中定義的。

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

在 C++/WinRT 中，我們將會在 `SampleConfiguration.h` 和 `SampleConfiguration.cpp` 檔案中，將其設為 **SampleState** 的私用靜態欄位 (其不會存取任何執行個體成員)。 這個方法的 C# 程式碼不會使用 **System.Text.StringBuilder**，但其會針對 C++/WinRT 版本執行足夠的字串格式設定，這是使用 **std::wostringstream** 的另一個理想位置。

我們會將標準 C++ `std::endl` (換行字元) 插入輸出資料流中，而不是在 C# 程式碼中使用的靜態 [**System.Environment.NewLine**](/dotnet/api/system.environment.newline) 屬性。

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

上述的 C++/WinRT 版本設計有點沒效率。 首先，我們會建立 **std::wostringstream**。 但我們也會呼叫我們稍早移植的 **BuildClipboardFormatsOutputString** 方法。 該方法會建立自己的 **std::wostringstream**。 而且該方法會將其資料流轉換成 **winrt::hstring** 並傳回。 我們會呼叫 [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 函式，將該傳回的 **hstring** 轉換回 C 樣式字串，然後將其插入資料流中。 只建立一個 **std::wostringstream**，並傳遞該參考會更有效率，讓方法可以直接在其中插入字串。

這就是我們在 C++/WinRT 版本的[剪貼簿範例](/samples/microsoft/windows-universal-samples/clipboard/)原始程式碼 (在您下載的 ZIP 中) 中所執行的操作。 在該原始程式碼中，有一個名為 **SampleState::AddClipboardFormatsOutputString** 的新私用靜態方法，其會採用並操作輸出資料流的參考。 接著，系統重構 **SampleState::DisplayChangedFormats** 和 **SampleState::BuildClipboardFormatsOutputString** 方法，以呼叫該新方法。 其在功能上相當於此主題中的程式碼清單，但更有效率。

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click** 是屬於 C# **MainPage** 類別的非同步事件處理常式，而且是在 `MainPage.xaml.cs` 中定義的。 以下程式碼清單在功能上相當於您所下載之原始程式碼中的方法。 但我在這裡將其從一行解壓縮成四行，讓您更輕鬆地查看其所執行的操作，以及之後如何進行移植。

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

雖然在技術上，方法是非同步的，但不會在 `await` 之後執行任何動作，因此不需要 `await` (也不需要 `async` 關鍵字)。 此方法可能會使用上述內容，以避免在 Visual Studio 中出現 IntelliSense 訊息。

對等 C++/WinRT 方法也將是非同步的 (因為其會呼叫 [**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync))。 但是，此方法不需要 `co_await`，也不會傳回非同步物件。 如需有關 `co_await` 和非同步物件的詳細資訊，請參閱[使用 C++/WinRT 的並行和非同步作業](./concurrency.md)。

現在讓我們來談談方法的作用。 因為這是用於 **HyperlinkButton** 之 **Click** 事件的事件處理常式，因此名為 *sender* 的物件實際上是 **HyperlinkButton**。 因此類型轉換是安全的 (我們也可以將此轉換表示為 `sender as HyperlinkButton`)。 接下來，我們會擷取 **Tag** 屬性的值 (如果您查看 C# 專案中的 XAML 標記，就會看到這會設定為代表 Web URL 的字串)。 雖然 **FrameworkElement.Tag** 屬性 (**HyperlinkButton** 是 **FrameworkElement**) 屬於 **object** 類型，但在 C# 中，我們可以使用 [**Object.ToString**](/dotnet/api/system.object.tostring) 將其字串化。 從產生的字串中，我們會建構 **Uri** 物件。 最後 (透過 Shell 的協助)，我們會啟動瀏覽器並瀏覽至該 URL。

以下是移植到 C++/WinRT 的方法 (同樣地，為清楚起見，將其展開)，之後是細節的描述。

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

一如往常，我們會將事件處理常式設為 `public`。 我們會對 *sender* 物件使用 [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 函式，以便將其轉換成 **HyperlinkButton**。 在 C++/WinRT 中，**Tag** 屬性是 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (等同於 [**Object**](/dotnet/api/system.object))。 但是在 **IInspectable** 上沒有 **Tostring**。 我們必須改為將 **IInspectable** Unbox 為純量值 (在此案例中為字串)。 同樣地，如需有關 Boxing 和 Unboxing 的詳細資訊，請參閱[將純量數值 Boxing 和 unboxing 到 IInspectable](./boxing.md)。

最後兩行會重複我們之前所看到的移植模式，而這些模式會對 C# 版本有相當大的回應。

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

移植此方法的內容則相同。 您可以比較下載的[剪貼簿範例](/samples/microsoft/windows-universal-samples/clipboard/)原始程式碼 ZIP 中的 C# 和 C++/WinRT 版本。

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** 和 **OnWindowActivated**

到目前為止，我們只有空的 stub 可用於這兩個事件處理常式。 但是移植這兩個事件處理常式很簡單，而且不會引發任何新的討論內容。

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

這是屬於 C# **MainPage** 類別的另一個私用事件處理常式，而且是在 `MainPage.xaml.cs` 中定義的。 在 C++/WinRT 中，我們會將其設為公用，並在 `MainPage.h` 和 `MainPage.cpp` 中加以實作。

針對此方法，我們將需要 **MainPage::navigating**，這是一個已初始化為 `false` 的私用布林值欄位。 此外，您在 `MainPage.xaml` 中還需要一個名為 *ScenarioFrame* 的 **Frame**。 但是除了這些細節以外，移植此方法也不會顯示任何新的技術。

#### <a name="updatestatus"></a>**UpdateStatus**

我們目前只有一個 stub 可用於 **MainPage.UpdateStatus**。 同樣地，移植其實作在很大程度上涵蓋了舊的基礎。 其中一個要注意的重點是，在 C# 中，我們可以比較 **string** 與 **String.Empty**，但在 C++/WinRT 中，我們會改為呼叫 [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function) 函式。 另一個重點是，`nullptr` 是 C# `null` 的標準 C++ 對等用法。

您可以使用我們已經涵蓋的技術，執行其餘的移植。 以下是您必須在此方法的移植版本編譯之前執行的事項種類清單。

- 若要 `MainPage.xaml`，加入一個名為 *StatusBorder* 的 **Border**。
- 若要 `MainPage.xaml`，加入一個名為 *StatusBlock* 的 **TextBlock**。
- 若要 `MainPage.xaml`，加入一個名為 *StatusPanel* 的 **StackPanel**。
- 若要 `pch.h`，加入 `#include "winrt/Windows.UI.Xaml.Media.h"`。
- 若要 `pch.h`，加入 `#include "winrt/Windows.UI.Xaml.Automation.Peers.h"`。
- 若要 `MainPage.cpp`，加入 `using namespace winrt::Windows::UI::Xaml::Media;`。
- 若要 `MainPage.cpp`，加入 `using namespace winrt::Windows::UI::Xaml::Automation::Peers;`。

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>複製完成移植 **MainPage** 所需的 XAML 和樣式

針對 XAML，理想的情況是，您可以在 C# 和 C++/WinRT 專案中使用*相同的* XAML 標記。 剪貼簿範例是其中一個案例。

在其 `Styles.xaml` 檔案中，剪貼簿範例具有 XAML **ResourceDictionary** 的樣式，其會套用至應用程式 UI 上的按鈕、功能表和其他 UI 元素。 `Styles.xaml` 頁面會合併至 `App.xaml`。 接下來就是 UI 的標準 `MainPage.xaml` 起點，我們已經簡略地介紹過了。 我們現在可以在 C++/WinRT 版本的專案中，重複使用這三個 `.xaml` 檔案 (未變更)。

如同資產檔案，您可以選擇從應用程式的多個版本中參考相同的共用 XAML 檔案。 在此逐步解說中，為簡單起見，我們會將檔案複製到 C++/WinRT 專案中，並以這種方式加入檔案。

瀏覽至 `\Clipboard_sample\SharedContent\xaml` 資料夾、選取並複製 `App.xaml` 和 `MainPage.xaml`，然後將這兩個檔案貼到 C++/WinRT 專案的 `\Clipboard\Clipboard` 資料夾中，並在出現提示時，選擇取代檔案。

將新資料夾新增至 C++/WinRT 專案，緊接在專案節點底下，然後命名為 `Styles`。 瀏覽至 `\Clipboard_sample\SharedContent\xaml` 資料夾、選取並複製 `Styles.xaml`，然後將其貼到 C++/WinRT 專案的 `\Clipboard\Clipboard\Styles` 資料夾中。 以滑鼠右鍵按一下 `Styles` 資料夾 (在 C++/WinRT 專案的 [方案總管] 中) > [新增] >  > [現有項目...]，然後瀏覽至 `\Clipboard\Clipboard\Styles`。 在檔案選擇器中，選取 `Styles`，然後按一下 [新增]。

我們現在已經完成移植 **MainPage**，如果您依照這些步驟進行，則您的 C++/WinRT 專案現在將會建置並執行。

## <a name="consolidate-your-idl-files"></a>合併您的 `.idl` 檔案

除了 UI 的標準 `MainPage.xaml` 起點之外，剪貼簿範例還有五個其他案例特定的 XAML 頁面，以及其對應的程式碼背後置檔案。 我們會在專案的 C++/WinRT 版本中，以不變更的方式重複使用所有這些頁面的實際 XAML 標記。 我們將在接下來的幾個主要章節中探討如何移植程式碼後置。 但在那之前，我們先來討論 IDL。

將您的執行階段類別合併為單一 IDL 檔案是很有價值的 (請參閱[將執行階段類別分解成 Midl 檔案 (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl))。 接下來，我們會合併 `CopyFiles.idl`、`CopyImage.idl`、`CopyText.idl`、`HistoryAndRoaming.idl`和 `OtherScenarios.idl` 的內容，方法為將該 IDL 移到名為 `Project.idl` 的單一檔案 (然後刪除原始檔案)。

當這麼做時，也讓我們從這五個 XAML 頁面類型的每一個中移除自動產生的虛擬屬性 (`Int32 MyProperty;` 及其實作)。

首先，將新的 **Midl 檔案 (.idl)** 項目新增至 C++/WinRT 專案。 請命名為 `Project.idl`。 以下列程式碼取代整個 `Project.idl` 的內容。

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

如您所見，這只是個別 `.idl` 檔案的內容副本，全都在一個命名空間內，而 `MyProperty` 從每個執行階段類別中移除。

在 Visual Studio 的方案總管中，多重選取所有原始 IDL 檔案 (`CopyFiles.idl`、`CopyImage.idl`、`CopyText.idl`、`HistoryAndRoaming.idl` 和 `OtherScenarios.idl`)，然後**編輯** > **移除** (選擇對話方塊中的 [刪除])。

最後&mdash;，若要為五個 XAML 頁面類型的每一個完全移除 `.h` 和 `.cpp` 檔案中的 `MyProperty`&mdash;，請刪除 `int32_t MyProperty()` 存取子和 `void MyProperty(int32_t)` 更動子函數的宣告和定義。

順便一提，您可讓 XAML 檔案的名稱符合其所代表的類別名稱。 例如，如果您在 XAML 標記檔中有 `x:Class="MyNamespace.MyPage"`，則應該將該檔案命名為 `MyPage.xaml`。 雖然這不是技術需求，但不需要針對相同的成品使用不同的名稱，就能讓您的專案更容易了解和維護，而且更容易使用。

## <a name="copyfiles"></a>**CopyFiles**

在 C# 專案中，**COPYFILES** XAML 頁面類型是在 `CopyFiles.xaml` 和 `CopyFiles.xaml.cs` 原始程式碼檔案中實作。 讓我們再依序看一下 **CopyFiles** 的每個成員。

### <a name="rootpage"></a>**rootPage**

這是私人欄位。

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

在 C++/WinRT 中，我們可以定義它，並將它初始化，如下所示。

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

同樣地 (就像使用 **MainPage::current** 一般)，**CopyFiles::rootPage** 會宣告為類型 **SDKTemplate::MainPage**，這是預計類型，而不是實作類型。

### <a name="copyfiles-the-constructor"></a>**CopyFiles** (建構函式)

在 C++/WinRT 專案中，**CopyFiles** 類型已有一個建構函式，其中包含我們所需的程式碼 (它只會呼叫 **InitializeComponent**)。

### <a name="copybutton_click"></a>**CopyButton_Click**

C# **CopyButton_Click** 方法是事件處理常式，而且從其簽章中的 `async` 關鍵字，我們可以告訴方法執行非同步工作。 在 C++/WinRT 中，我們會將非同步方法實作為*協同程式*。 如需 C++/WinRT 中並行的簡介，以及何謂*協同程式* 的描述，請參閱[透過 C++/WinRT 的並行和非同步作業](./concurrency.md)。

在協同程式完成之後，一般都會想要排定進一步的工作，而且對於這種情況，協同程式會傳回一些可以等候的非同步物件類型，並選擇性地報告進度。 但這些考慮通常不適用於事件處理常式。 因此，當您有執行非同步作業的事件處理常式時，可以將它實作為傳回 **winrt::fire_and_forget** 的協同程式。 如需詳細資訊，請參閱[射後不理 (Fire-and-forget)](./concurrency-2.md#fire-and-forget)。

雖然「射後不理」協同程式的想法是您不在意何時完成，但仍會在背景繼續執行工作 (或暫停，等待繼續)。 您可以從 C# 實作中看到 **CopyButton_Click** 取決於 `this` 指標 (它會存取執行個體資料成員 `rootPage`)。 因此，我們必須確定 `this` 指標 (指向 **CopyFiles** 物件的指標) 的存留時間超過 **CopyButton_Click** 協同程式。 在類似此範例應用程式的情況下，使用者會在 UI 頁面之間導覽，因此我們無法直接控制這些頁面的存留期。 如果 **CopyFiles** 頁面遭到終結 (藉由離開它)，但 **CopyButton_Click** 仍在背景執行緒上進行，則無法安全存取 `rootPage`。 若要讓協同程式正確，必須取得 `this` 指標的強式參考，並在協同程式執行期間保留該參考。 如需詳細資訊，請參閱 [C++/WinRT 中的強式和弱式參考](./weak-references.md)。

如果您查看範例的 C++/WinRT 版本，則在 **CopyFiles::CopyButton_Click** 中，您會看到已在堆疊上完成簡單的宣告。

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

讓我們查看移植程式碼其他值得注意的層面。

在程式碼中，我們會具現化 [**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker) 物件，並在兩行之後我們會存取該物件的 [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) 屬性。 該屬性的傳回類別會實作字串的 **IVector**。 在該 **IVector** 上，我們會呼叫 [IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall) 方法。 有趣的層面是我們要傳遞至該方法的值，預期的是陣列。 以下是程式碼行。

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

我們所傳遞的值 (`{ L"*" }`) 是標準 C++ *初始設定式清單*。 在此情況下，其包含單一物件，但初始設定式清單可以包含任意數目的逗號分隔物件。 可讓您輕鬆地將初始設定式清單傳遞至方法的 C++/WinRT 片段，如[標準初始設定式清單](./std-cpp-data-types.md#standard-initializer-lists)中所述。

我們會在 C++/WinRT 中將 C# `await` 關鍵字移植到 `co_await`。 以下是程式碼中的範例。

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

接下來，請考慮這行的 C# 程式碼。

```csharp
dataPackage.SetStorageItems(storageItems);
```

C# 能夠以隱含方式將 *storageItems* 所表示的 **IReadOnlyList<StorageFile>** ，轉換成 [**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems) 所預期的 **IEnumerable<IStorageItem>** 。 但在 C++/WinRT 中，我們需要從 **IVectorView<StorageFile>** 明確地轉換為 **iiterable<IStorageItem>** 。 因此，我們有另一個 [ 範例 **，做為運作中的** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 函數。

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

在 C# 中使用 `null` 關鍵字 (例如，`Clipboard.SetContentWithOptions(dataPackage, null)`) 時，我們會在 C++/WinRT 中使用 `nullptr` (例如，`Clipboard::SetContentWithOptions(dataPackage, nullptr)`)。

### <a name="pastebutton_click"></a>**PasteButton_Click**

這是另一個採用射後不理協同程式形式的事件處理常式。 讓我們查看移植程式碼值得注意的層面。

在範例的 C# 版本中，我們會使用 `catch (Exception ex)`來捕捉例外狀況。 在移植的 C++/WinRT 程式碼中，您會看到運算式 `catch (winrt::hresult_error const& ex)`。 如需 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 及其使用方式的詳細資訊，請參閱 [使用 C++/WinRT 處理錯誤](./error-handling.md)。

測試 C# 物件是 `null` 還是 `if (storageItems != null)` 的範例。 在 C++/WinRT 中，我們可以依賴轉換運算子來 `bool`，這會在內部對 `nullptr` 進行測試。

以下是範例的移植 C++/WinRT 版本中，稍微簡化的程式碼片段版本。

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

從 **winrt::hstring** 建構 **std::wstring_view** ，例如說明呼叫 [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 函數的替代方法 (將 **winrt::hstring** 轉換成 C 樣式字串)。 這種替代方法的運作歸功於 **hstring** [對 **std::wstring_view** 的轉換運算子](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view)。

請考慮此 C# 片段。

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

為了將 C# `as` 關鍵字移植到C++/WinRT，到目前為止，我們看到 [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 函數使用了好幾次。 如果類型轉換失敗，則該函數會擲回例外狀況。 但是，如果我們想要轉換在失敗時傳回 `nullptr` (讓我們可以在程式碼中處理該條件)，則改用 [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 函數。

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>複製完成移植 **CopyFiles** 所需的 XAML

您現在可以從 C# 物件中選取 `CopyFiles.xaml` 檔案的整個內容，並將該內容貼入 C++/WinRT 專案中的 `CopyFiles.xaml` 檔案 (取代 C++/WinRT 專案中該檔案的現有內容)。

最後，編輯 `CopyFiles.h` 和 `.cpp` 並刪除虛擬 **ClickHandler** 函數，因為我們剛覆寫了對應的 XAML 標記。

我們現在已完成移植 **CopyFiles**，如果您依照這些步驟進行，則您的 C++/WinRT 專案現在將會建置並執行，而且 **CopyFiles** 案例將可運作。

## <a name="copyimage"></a>**CopyImage**

若要移植 **CopyImage** XAML 頁面類型，請遵循與 **CopyFiles** 相同的程序。 在移植 **CopyImage** 時，您會遇到 C# [*using 陳述式*](/dotnet/csharp/language-reference/keywords/using-statement)的使用，這可確保實作 [**IDisposable**](/dotnet/api/system.idisposable) 介面的物件正確地處置。

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

C++/WinRT 中的對等介面是 [**windows.foundation.iclosable**](/uwp/api/windows.foundation.iclosable)，其具有單一 **Close** 方法。 以下是 C++/WinRT 程式碼，相等於上述的 C# 程式碼。

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

C++/WinRT 物件會實作 **IClosable**，主要用於取得不具決定性最終處理的語言優點。 C++/WinRT 具決定性最終處理，因此我們在撰寫 C++/WinRT 時，通常不需要呼叫 **IClosable::Close**。 但有時候非常適合於呼叫它，而且這是其中一個時間。 在這裡，*imageStream* 識別碼是基礎 Windows 執行階段物件周圍的參考計數包裝函式 (在此案例中，指的是實作 [**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype) 的物件)。 雖然我們可以判斷 *imageStream* (其解構函式) 的完成項會在封閉範圍 (大括弧) 的結尾執行，但是無法確定完成項將會呼叫 **Close**。 這是因為我們已將 *imageStream* 傳遞至其他 API，而且它們可能仍會參與基礎 Windows 執行階段物件的參考計數。 因此，在此情況下，最好明確地呼叫 **Close**。 如需詳細資訊，請參閱[我需要在我使用的執行階段類別上呼叫 IClosable::Close 嗎？](./faq.md#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume)。

接下來，請考慮 C# 運算式 `(uint)(imageDecoder.OrientedPixelWidth * 0.5)`，您可以在 **OnDeferredImageRequestedHandler** 事件處理常式中找到此運算式。 該運算式會將 `uint` 乘以 `double`，因而產生 `double`。 然後，將該結果轉換為 `uint`。 在 C++/WinRT 中，我們 *可以*使用外表類似的 C 樣式轉換 (`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`)，但最好是讓它清楚明白我們想要的轉換類型，在此情況下，我們會使用 `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)` 來執行此動作。

**CopyImage. OnDeferredImageRequestedHandler** 的 C# 版本具有 `finally` 子句，但沒有 `catch` 子句。 我們已稍微在 C++/WinRT 版本中進一步執行，並實作 `catch` 子句，讓我們可以報告延遲轉譯是否成功。

移植此 XAML 頁面的其餘部分，並不會產生任何新的討論內容。 請記得刪除虛擬 **ClickHandler** 函數。 另外，就像 **CopyFiles** 一樣，移植中的最後一個步驟就是選取 `CopyImage.xaml` 的整個內容，並將它貼入 C++/WinRT 專案中的相同檔案。

## <a name="copytext"></a>**CopyText**

您可以使用我們已涵蓋的技術來移植 `CopyText.xaml` 和 `CopyText.xaml.cs`。

## <a name="historyandroaming"></a>**HistoryAndRoaming**

移植 **HistoryAndRoaming** XAML 頁面類型時，會引發一些興趣點。

首先，查看 C# 原始程式碼，然後透過 **OnHistoryEnabledChanged** 事件處理常式遵循來自 **OnNavigatedTo** 的控制流程，最後再查看非同步函數 **CheckHistoryAndRoaming** (這不是等待，因此基本上它是射後不理)。 因為 **CheckHistoryAndRoaming** 是非同步，所以我們必須在 C++/WinRT 中謹慎處理 `this` 指標的存留期。 如果您查看 `HistoryAndRoaming.cpp` 原始程式碼檔案中的實作，就可以看到結果。 第一，將委派附加至 **Clipboard::HistoryEnabledChanged** 和 **Clipboard::RoamingEnabledChanged** 事件時，我們只會對 **HistoryAndRoaming** 頁面物件採取弱式參考。 方法是建立委派，其對從 [**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)傳回的值具有相依性 ，而不是對 `this` 指標具有相依性。 這表示，委派本身 (最後會呼叫非同步程式碼) 不會讓 **HistoryAndRoaming** 頁保持運作，而是我們應該離開它。

第二，當我們最終到達射後不理的 **CheckHistoryAndRoaming** 協同程式時，要做的第一件事就是對 `this` 採取強式參考，以保證 **HistoryAndRoaming** 頁面至少會存留到協同程式最後完成為止。 如需上述兩個層面的詳細資訊，請參閱 [C++/WinRT 中的強式和弱式參考](./weak-references.md)。

在移植 **CheckHistoryAndRoaming** 時，我們發現另一個興趣點。 它包含用來更新 UI 的程式碼；因此，我們必須確定是在主要 UI 執行緒上執行該更新。 最初呼叫事件處理常式的執行緒是主要的 UI 執行緒。 但一般來說，非同步方法可以在任何的任意執行緒上執行和/或繼續。 在 C# 中，解決方案是呼叫 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync)，並從 lambda 函數內更新 UI。 在 C++/WinRT 中，我們可以使用 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 函數，搭配 `this` 指標的[**發送器**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) 來暫停協同程式，並在主要 UI 執行緒上立即繼續執行。

相關的運算式為 `co_await winrt::resume_foreground(Dispatcher());`。 或者，雖然較不清楚，但是也可以將其簡單表達為 `co_await Dispatcher();`。 較短的版本是承蒙 C++/WinRT 所提供的轉換運算子來達成。

移植此 XAML 頁面的其餘部分，並不會產生任何新的討論內容。 請記得刪除虛擬 **ClickHandler** 函數，並複製 XAML 標記。

## <a name="otherscenarios"></a>**OtherScenarios**

您可以使用我們已涵蓋的技術來移植 `OtherScenarios.xaml` 和 `OtherScenarios.xaml.cs`。

## <a name="conclusion"></a>結論

希望此逐步解說已提供您足夠的移植資訊和技術，讓您現在可以繼續進行，並將自己的 C# 應用程式移植到 C++/WinRT。 藉由重新整理程式，您可以繼續回頭參考 Cliboard 範例中舊版 (C#) 和新版 (C++/WinRT) 的原始程式碼，然後並排比較它們以查看對應關係。

## <a name="related-topics"></a>相關主題
* [從 C# 移到 C++/WinRT](./move-to-winrt-from-csharp.md)