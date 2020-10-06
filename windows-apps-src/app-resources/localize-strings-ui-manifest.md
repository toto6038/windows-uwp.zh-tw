---
Description: 如果希望應用程式支援不同的顯示語言，而且您的程式碼、XAML 標記或應用程式套件資訊清單也含有字串常值時，請將這些字串移入資源檔案 (.resw)。 您可以接著針對應用程式支援的每一種語言建立該資源檔案的翻譯複本。
title: 當地語系化您的 UI 及應用程式套件資訊清單中的字串
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 411ecb63189084ba83f9971ded2bbe02d899aabd
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762863"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>當地語系化您的 UI 及應用程式套件資訊清單中的字串

如需有關將您的應用程式當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../design/globalizing/globalizing-portal.md)。

如果希望應用程式支援不同的顯示語言，而且您的程式碼、XAML 標記或應用程式套件資訊清單也含有字串常值時，請將這些字串移入資源檔案 (.resw)。 您可以接著針對應用程式支援的每一種語言建立該資源檔案的翻譯複本。

硬式編碼的字串常值可以出現在命令式程式碼或 XAML 標記中，例如為 **TextBlock** 的 **Text** 屬性。 它們也可以出現在您的應用程式套件資訊清單來源檔案 (`Package.appxmanifest` 檔案) 中，例如做為 Visual Studio 資訊清單設計工具的 \[應用程式\] 索引標籤上 \[顯示名稱\] 的值。 將這些字串移入資源檔案 (.resw)，並使用資源識別碼的參照取代您的 App 和資訊清單中的硬式編碼字串常值。

不像影像資源，影像資源檔案中只包含一個影像資源，字串資源檔案中包含*多個*字串資源。 字串資源檔案是資源檔案 (.resw)，並且您通常是在專案的 \Strings 資料夾中建立這種資源檔案。 如需有關如何在資源檔案 (.resw) 的名稱中使用限定詞的背景資訊，請參閱[針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)。

## <a name="store-strings-in-a-resources-file"></a>將字串儲存在資源檔中

1. 設定您的 App 預設語言。
    1. 在 Visual Studio 中開啟您的解決方案，然後開啟 `Package.appxmanifest`。
    2. 在 \[應用程式\] 索引標籤上，確認預設語言有適當地設定 (例如，「en」或「en-US」)。 剩餘的步驟會假設您已設定預設語言為「en-US」。
    <br>**注意**  您至少需要為此預設語言提供當地語系化的字串資源。 這些是在找不到使用者慣用語言或顯示語言設定的較佳相符項，將會載入的資源。
2. 建立預設語言的資源檔案 (.resw)。
    1. 在您的專案節點下，建立新的資料夾，並將其命名為「Strings」。
    2. 在 `Strings` 下，建立新的子資料夾，並命名為「en-US」。
    3. 在 `en-US` 下，建立新的資源檔案 (.resw)，並確認其命名為「Resources.resw」。
    <br>**注意**  如果您有想要移植 ( .resx) 的 .NET 資源檔，請參閱[移植 XAML 和 UI](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)。
3. 開啟 `Resources.resw`，並新增這些字串資源。

    `Strings/en-US/Resources.resw`

    ![> E N U S > Resources. .resw 檔案的 [新增資源] 資料表的螢幕擷取畫面。](images/addresource-en-us.png)

    在此範例中，「Greeting」是您可以從您的標記參照的字串資源識別碼，如以下所示。 對於識別碼「Greeting」，會提供 Text 屬性字串，並提供 Width 屬性字串。 「Greeting.Text」是屬性識別碼的範例，因為它對應 UI 項目的屬性。 您同時也可以，例如在 \[名稱\] 欄位中新增「Greeting.Foreground」，並將其 Value 設為「Red」。 「Farewell」識別碼是簡單的字串資源識別碼；不具備子屬性，它可以從命令式程式碼載入，如以下所示。 Comment 欄位是提供任何特殊指示給翻譯人員的很好位置。

    在此範例中，既然我們有命名為「Farewell」的簡單字串資源識別碼項目，我們就無法同時*也*有依據該相同識別碼的屬性識別碼。 因此，新增「Farewell.Text」會使得建置 `Resources.resw` 時造成重複輸入錯誤。

    資源識別碼不區分大小寫，在每個資源檔案中都必須是唯一的。 請務必使用有意義的資源識別碼，將其他內容提供給翻譯工具。 請勿在傳送字串資源進行翻譯之後變更資源識別碼。 當地語系化團隊會使用資源識別碼，來追蹤資源中的新增、刪除及更新。 資源識別碼中的變更 (又稱為「資源識別碼轉換」) 需要重新翻譯字串，因為會出現像是字串已刪除或加入其他字串的情況。

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>從 XAML 參考字串資源識別碼

您可以使用 [x: Uid 指示詞](../xaml-platform/x-uid-directive.md)關聯標記中的控制項或其他項目與字串資源識別碼。

```xaml
<TextBlock x:Uid="Greeting"/>
```

在執行階段，會載入 `\Strings\en-US\Resources.resw`(因為現在這是專案中唯一的資源檔案)。 **TextBlock** 上的 **X: Uid**指示詞會造成發生查閱，以尋找包含字串資源識別碼「Greeting」的 `Resources.resw` 內的屬性識別碼。 會找到「Greeting.Text」和「Greeting.Width」屬性識別碼，並將其值套用到 **TextBlock**，覆寫標記中在本機設定的任何值。 「Greeting.Foreground」值也會套用，如果您已經新增了此值。 但只會使用屬性識別碼來設定 XAML 標記項目上的屬性，所以在此 TextBlock 上設定 **x: Uid** 為「Farewell」不會有任何影響。 `Resources.resw`*包含字串*資源識別碼 "離職"，但它不包含它的屬性識別碼。

指派字串資源識別碼給 XAML 項目時，請確定該識別碼的*所有*屬性識別碼適用於 XAML 項目。 例如，如果您在 **TextBlock** 上設定 `x:Uid="Greeting"`，則「Greeting.Text」將因為 **TextBlock** 類型有 Text 屬性而進行解析。 但是如果您在 **Button** 上設定 `x:Uid="Greeting"`，則「Greeting.Text」將會因為**Button** 類型沒有 Text 屬性造成執行階段錯誤。 該案例的一個解決方案是撰寫命名為「ButtonGreeting.Content」的屬性識別碼，並在 **Button** 上設定 `x:Uid="ButtonGreeting"`。

不從資源檔案設定 **Width**，您可能會想要允許控制項動態依照內容調整大小。

**注意**  針對[附加屬性](../xaml-platform/attached-properties-overview.md)，您需要在 .resw 檔案的 [名稱] 資料行中有特殊的語法。 例如，若要設定「Greeting」識別碼的 [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) 附加屬性的值，這就是您要在 \[名稱\] 欄位中輸入的內容。

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>參閱程式碼的字串資源識別碼

您可以明確地載入依據簡單字串資源識別碼的字串資源。

> [!NOTE]
> 如果您已呼叫任何*可能*會在背景/背景工作執行緒中執行的 **GetForCurrentView** 方法，請使用 `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` 測試來守護這個呼叫。 從背景/背景工作執行緒呼叫 **GetForCurrentView** 會造成例外狀況「無法在沒有 CoreWindow 的執行緒上建立 *&lt;typename&gt;。*」

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

您可以使用類別庫 (通用 Windows) 或 [Windows 執行階段媒體櫃 (通用 Windows)](../winrt-components/index.md) 專案中的此相同程式碼。 在執行階段，會載入裝載媒體櫃的 App 的資源。 建議從裝載媒體櫃的 App 載入資源，因為 App 可能有較大程度當地語系化。 如果媒體櫃確實需要提供資源，則它應該提供其裝載的 App 選項以取代那些做為輸入的資源。

如果資源名稱是分割 (其包含 "." 個字元) ，則以正斜線取代點 ( "/" ) 字元在資源名稱中。 例如，屬性識別碼包含點;因此您必須進行這項 substition，才能從程式碼載入其中一個。

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

如果不確定，您可以使用 [MakePri.exe](makepri-exe-command-options.md) 傾印您應用程式的 PRI 檔案。 每個資源 `uri` 都會顯示在傾印檔案中。

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>請參閱應用程式套件資訊清單的字串資源識別碼

1. 開啟您的應用程式套件資訊清單原始程式檔 (檔案 `Package.appxmanifest`) ，在預設情況下，您的應用程式 `Display name` 會以字串常值表示。

   ![Package.appxmanifest 檔案的螢幕擷取畫面，其中顯示 [應用程式] 索引標籤，並將 [顯示名稱] 設定為 [艾德作品] 週期。](images/display-name-before.png)

2. 若要製作這個字串的當地語系化版本，請開啟 `Resources.resw` 並加入名稱為「AppDisplayName」和值為「Adventure Works Cycles」的新字串資源。

3. 以剛建立的字串資源識別碼 (「AppDisplayName」) 的參照，取代顯示名稱字串常值。 若要這樣做，您可以使用 `ms-resource` URI (統一資源識別項) 配置。

   ![Package.appxmanifest 檔案的螢幕擷取畫面，其中顯示 [應用程式] 索引標籤，並將 [顯示名稱] 設定為 M S 資源應用程式顯示名稱。](images/display-name-after.png)

4. 對於您想要當地語系化的資訊清單中的每個字串重複此程序。 例如，您的 App 簡短名稱 (您可以設定為顯示在 [開始] 畫面的 App 磚上)。 如需可當地語系化的應用程式套件資訊清單中的所有項目清單，請參閱[可當地語系化的資訊清單項目](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)。

## <a name="localize-the-string-resources"></a>當地語系化字串資源

1. 為其他語言製作一份資源檔案 (.resw) 的複本。
    1. 在「Strings」下，對於德文 (德國) 建立新的子資料夾並命名為「de-DE」。
   <br>**注意**  針對資料夾名稱，您可以使用任何[BCP-47 語言標記](https://tools.ietf.org/html/bcp47)。 如需語言限定詞的詳細資訊和常見語言標記的清單，請參閱[針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)。
   2. 在 `Strings/de-DE` 資料夾中製作一份 `Strings/en-US/Resources.resw` 的複本。
2. 翻譯字串。
    1. 開啟 `Strings/de-DE/Resources.resw` 並翻譯 \[值\] 欄位中的值。 您不需要翻譯註解。

    `Strings/de-DE/Resources.resw`

    ![新增資源，德文](images/addresource-de-de.png)

如有需要，您可以對其他語言重複步驟 1 到 2。

`Strings/fr-FR/Resources.resw`

![新增資源，法文](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>測試應用程式

針對您的預設顯示語言來測試 App。 然後，您可以在 [**設定**時間] 中變更顯示語言  >  **& 語言**  >  **區域 & 語言**  >  **語言**，然後重新測試您的應用程式。 查看 UI 和命令介面 (例如您的標題列&mdash;這是您的顯示名稱&mdash;，以及您的磚上的簡短名稱) 中的字串。

**注意** 如果找到符合顯示語言設定的資料夾名稱，則該資料夾中的資源檔案已載入。 否則，會發生遞補，以您的 App 預設語言的資源結束。

## <a name="factoring-strings-into-multiple-resources-files"></a>將字串分解到多個資源檔案

您可以將所有字串保存在單一資源檔案 (resw)，或者您可以將它們分解為跨多個資源檔案。 例如，您可能想要在一個資源檔案中保存您的錯誤訊息，另一個資源檔案中保存應用程式套件資訊清單字串，第三個保存您的 UI 字串。 在該案例中，您的資料夾結構看起來會像這樣。

![解決方案面板的螢幕擷取畫面，其中顯示 [艾德作品] > 字串] 資料夾，其中包含德文、U S 英文和法文地區設定資料夾和檔案。](images/manifest-resources.png)

若要將字串資源識別碼限定範圍為參照特定檔案，您只要在識別碼之前新增 `/<resources-file-name>/`。 以下標記範例假設 `ErrorMessages.resw` 包含的資源其名稱為「PasswordTooWeak.Text」，其值描述錯誤。

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

您只需要為資源檔案*而非* `Resources.resw`，在字串資源識別碼之前新增 `/<resources-file-name>/`。 這是因為「Resources.resw」是預設檔案名稱，因此您若略過檔案的名稱 (如我們在本主題中的稍早範例) 時做此假設。

以下程式碼範例假設 `ErrorMessages.resw` 包含的資源其名稱為「MismatchedPasswords」，其值描述錯誤。

> [!NOTE]
> 如果您已呼叫任何*可能*會在背景/背景工作執行緒中執行的 **GetForCurrentView** 方法，請使用 `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` 測試來守護這個呼叫。 從背景/背景工作執行緒呼叫 **GetForCurrentView** 會造成例外狀況「無法在沒有 CoreWindow 的執行緒上建立 *&lt;typename&gt;。*」

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

如果您已將您的「AppDisplayName」資源移出 `Resources.resw` 到 `ManifestResources.resw`，然後到您要變更 `ms-resource:AppDisplayName` 為 `ms-resource:/ManifestResources/AppDisplayName` 的應用程式套件資訊清單中。

如果有分割的資源檔名稱 (它會包含 "." 個字元) ，則在您參考該名稱時，請保留名稱中的點。 **請勿** 以正斜線 ( "/" ) 字元來取代點，就像您對資源名稱所做的一樣。

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

如果不確定，您可以使用 [MakePri.exe](makepri-exe-command-options.md) 傾印您應用程式的 PRI 檔案。 每個資源 `uri` 都會顯示在傾印檔案中。

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>載入特定語言或其他內容的字串

預設 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (從 [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView) 取得) 包含每個限定詞名稱表示預設執行階段內容的限定詞值 (也就是說，適用於目前使用者及電腦的設定值)。 資源檔案 (.resw) 會&mdash;根據其名稱中的限定詞&mdash;比對該執行階段內容中的限定詞值。

但是，您有時可能會想要讓 App 覆寫系統設定，並明確指定要在尋找所需載入的相符資源檔案時使用的語言、縮放比例或其他限定詞值。 例如，您可能想讓使用者可以為工具提示或錯誤訊息選取其他語言。

您可以這樣做，方式為建構新的 **ResourceContext** (而不使用預設的)、覆寫其值，然後在您的字串查閱中使用該內容物件。

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

如上述程式碼範例使用 **QualifierValues** 適用於任何限定詞。 對於特殊案例的語言，您可以改為如此做。

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

為了在全域層級達到相同效果。您*可以*覆寫預設 **ResourceContext** 中的限定詞值。 但我們建議您改為呼叫 [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)。 您可以透過呼叫 **SetGlobalQualifierValue** 將值設定一次，然後這些值就會在每次您用來進行查閱時，於預設 **ResourceContext** 中生效。

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

某些限定詞有系統資料提供者。 所以，不呼叫 **SetGlobalQualifierValue** 的話，您可以改為透過其 API 調整提供者。 例如，此程式碼顯示如何設定 [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)。

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>更新字串以回應限定詞值變更事件

您的執行中 App 可以回應預設 **ResourceContext** 中受影響限定詞值的系統設定變更。 任何這些系統設定都會叫用 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 上的 [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 事件。

在回應這個事件上，您可以從預設 **ResourceContext** 重新載入字串。

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>從類別庫或 Windows 執行階段程式庫載入字串

參照之類別庫 (通用 Windows) 或[Windows 執行階段媒體櫃 (通用 Windows)](../winrt-components/index.md) 的字串資源，通常加入到在建置程序期間包含它們的套件的子資料夾。 這類字串的資源識別碼通常採用表單 *LibraryName/ResourcesFileName/ResourceIdentifier*。

類別庫可以為它自己的資源取得 ResourceLoader。 例如，下列程式碼說明程式庫或參考它的應用程式如何取得程式庫字串資源的 ResourceLoader。

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

針對 Windows 執行階段程式庫 (通用 Windows) ，如果預設命名空間已分割 (它包含 "." 字元) ，則在資源對應名稱中使用點。

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

您不需要為類別庫 (通用 Windows) 。 如果不確定，您可以指定 [MakePri.exe 命令列選項](makepri-exe-command-options.md) 來傾印元件或程式庫的 PRI 檔案。 每個資源 `uri` 都會顯示在傾印檔案中。

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>從其他套件載入字串

應用程式套件的資源是透過封裝本身的最上層 [**windows.applicationmodel.resources.core.resourcemap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) （可從目前的 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)存取）進行管理和存取。 在每個套件內，各種元件可以有自己的 ResourceMap 樹狀子目錄，您可以透過 [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live) 存取。

架構套件可以存取自己具有絕對資源識別碼 URI 的資源。 另請參閱 [URI 配置](uri-schemes.md)。

## <a name="loading-strings-in-non-packaged-applications"></a>在非封裝的應用程式中載入字串

從 Windows 版本 1903 (2019 更新) ，未封裝的應用程式也可以利用資源管理系統。

只要建立 UWP 使用者控制項/程式庫，並 [將任何字串儲存在資源檔中](#store-strings-in-a-resources-file)即可。 然後，您可以 [從 XAML 參考字串資源識別碼](#refer-to-a-string-resource-identifier-from-xaml)、 [從程式碼中參考字串資源識別碼](#refer-to-a-string-resource-identifier-from-code)，或從 [類別庫或 Windows 執行階段程式庫載入字串](#load-strings-from-a-class-library-or-a-windows-runtime-library)。

若要在非封裝的應用程式中使用資源，您應該執行幾項作業：

1. 從程式碼解析資源時，請使用 [GetForViewIndependentUse](/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) 而非 [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) ，因為未封裝的案例中沒有 *目前的觀點* 。 如果您在非封裝案例中呼叫 [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) ，就會發生下列例外狀況： *可能無法在沒有 CoreWindow 的執行緒上建立資源內容。*
1. 使用 [MakePri.exe](./compile-resources-manually-with-makepri.md) 手動產生您的應用程式資源 pri 檔案。
    - `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /of resources.pri`執行
    - &lt;Priconfig.default.xml &gt; 必須省略「封裝」 &lt; 區段， &gt; 以便將所有資源配套到單一資源的 pri 檔案中。 如果使用[createconfig](./makepri-exe-command-options.md#createconfig-command)所建立的預設[MakePri.exe 設定檔](./makepri-exe-configuration.md)，您必須在 &lt; 建立後手動刪除「封裝」 &gt; 區段。
    - &lt;Priconfig.default.xml &gt; 必須包含所有必要的索引子，以將您專案中的所有資源合併為單一資源的 pri 檔案。 [Createconfig](./makepri-exe-command-options.md#createconfig-command)所建立的預設[MakePri.exe 設定檔](./makepri-exe-configuration.md)包含所有索引子。
    - 如果您未使用預設設定，請確定已啟用 PRI 索引子 (查看如何執行這項作業的預設設定) ，以合併位於專案根目錄內的 UWP 專案參考、NuGet 參考等等的 PRIs。
        > [!NOTE]
        > 藉由省略 `/IndexName` ，而且專案沒有應用程式資訊清單，PRI 檔案的 IndexName/root 命名空間會自動設定為 *應用程式*，而執行時間會瞭解非封裝的應用程式， (這會移除先前在封裝識別碼) 上的永久相依性。 指定資源 Uri 時，會省略根命名空間的 ms 資源：////參考會將 *應用程式* 推斷為非封裝應用程式的根命名空間 (或者您可以明確地指定 *應用程式* ，如同在 ms 資源：//Application/) 。
1. 將 PRI 檔案複製到 .exe 的組建輸出目錄
1. 執行 .exe 
    > [!NOTE]
    > 當您根據非封裝應用程式中的語言來解析資源時，資源管理系統會使用系統顯示語言，而不是使用者慣用語言清單。 使用者慣用語言清單僅用於 UWP 應用程式。

> [!Important]
> 只要修改資源，您就必須手動重建 PRI 檔案。 我們建議使用可處理 [MakePri.exe](./compile-resources-manually-with-makepri.md) 命令的後置組建腳本，並將資源的 pri 輸出複製到 .exe 目錄。

## <a name="important-apis"></a>重要 API
* [ApplicationModel.Resources.ResourceLoader](/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>相關主題
* [移植 XAML 與 UI](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [x:Uid 指示詞](../xaml-platform/x-uid-directive.md)
* [附加屬性](../xaml-platform/attached-properties-overview.md)
* [可當地語系化的資訊清單項目](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [BCP-47 語言標記](https://tools.ietf.org/html/bcp47)
* [針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)
* [如何載入字串資源](/previous-versions/windows/apps/hh965323(v=win.10))