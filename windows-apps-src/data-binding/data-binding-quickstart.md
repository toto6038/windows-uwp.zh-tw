---
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: 資料繫結概觀
description: 本主題示範如何在通用 Windows 平台 (UWP) app 中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目的控制項繫結到項目集合。
ms.date: 10/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cppcx
ms.openlocfilehash: dfe17fc64fd3e97f7562a7feca760b3a5d918f2e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318004"
---
# <a name="data-binding-overview"></a>資料繫結概觀

本主題說明如何在通用 Windows 平台 (UWP) 應用程式中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目控制項繫結到項目集合。 此外，我們還會說明如何控制項目的呈現、根據選擇來實作詳細資料檢視、以及轉換資料以供顯示。 如需詳細資訊，請參閱[深入了解資料繫結](data-binding-in-depth.md)。

## <a name="prerequisites"></a>先決條件

這個主題假設您知道如何建立基本的 UWP app。 如需建立第一個 UWP 應用程式的指示，請參閱 [Windows 應用程式入門](https://docs.microsoft.com/windows/uwp/get-started/)。

## <a name="create-the-project"></a>建立專案

建立新的 [空白應用程式 (Windows 通用)]  專案。 將它命名為「快速入門」。

## <a name="binding-to-a-single-item"></a>繫結到單一項目

每個繫結是由繫結目標和繫結來源所組成。 通常，目標是控制項或其他 UI 元素的屬性，來源是類別執行個體 (資料模型或檢視模型) 的屬性。 這個範例示範如何將控制項繫結到單一項目。 目標是 **TextBlock** 的 **Text** 屬性。 來源是一個簡單類別 **Recording** 的執行個體，代表音訊錄製。 讓我們先看一下這個類別。

如果您使用C#或C++/CX，然後將新類別新增至您的專案，並將類別命名為**錄製**。

如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然後新增**Midl 檔 (.idl)** 項目加入專案中，如中所示C++/WinRT 下方的程式碼範例清單。 使用這些新檔案的內容取代[MIDL 3.0](/uwp/midl-3/intro)顯示在清單中，程式碼建置專案以產生`Recording.h`及`.cpp`並`RecordingViewModel.h`和`.cpp`，然後將程式碼新增至產生的檔案若要比對的清單。 如需這些產生的檔案的詳細資訊，以及如何將它們複製到您的專案，請參閱 < [XAML 控制項，繫結至C++/WinRT 屬性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }
        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }
        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }
    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cppwinrt
// Recording.idl
namespace Quickstart
{
    runtimeclass Recording
    {
        Recording(String artistName, String compositionName, Windows.Globalization.Calendar releaseDateTime);
        String ArtistName{ get; };
        String CompositionName{ get; };
        Windows.Globalization.Calendar ReleaseDateTime{ get; };
        String OneLineSummary{ get; };
    }
}

// RecordingViewModel.idl
import "Recording.idl";

namespace Quickstart
{
    runtimeclass RecordingViewModel
    {
        RecordingViewModel();
        Quickstart.Recording DefaultRecording{ get; };
    }
}

// Recording.h
// Add these fields:
...
#include <sstream>
...
private:
    std::wstring m_artistName;
    std::wstring m_compositionName;
    Windows::Globalization::Calendar m_releaseDateTime;
...

// Recording.cpp
// Implement like this:
...
Recording::Recording(hstring const& artistName, hstring const& compositionName, Windows::Globalization::Calendar const& releaseDateTime) :
    m_artistName{ artistName.c_str() },
    m_compositionName{ compositionName.c_str() },
    m_releaseDateTime{ releaseDateTime } {}

hstring Recording::ArtistName(){ return hstring{ m_artistName }; }
hstring Recording::CompositionName(){ return hstring{ m_compositionName }; }
Windows::Globalization::Calendar Recording::ReleaseDateTime(){ return m_releaseDateTime; }

hstring Recording::OneLineSummary()
{
    std::wstringstream wstringstream;
    wstringstream << m_compositionName.c_str();
    wstringstream << L" by " << m_artistName.c_str();
    wstringstream << L", released: " << m_releaseDateTime.MonthAsNumericString().c_str();
    wstringstream << L"/" << m_releaseDateTime.DayAsString().c_str();
    wstringstream << L"/" << m_releaseDateTime.YearAsString().c_str();
    return hstring{ wstringstream.str().c_str() };
}
...

// RecordingViewModel.h
// Add this field:
...
#include "Recording.h"
...
private:
    Quickstart::Recording m_defaultRecording{ nullptr };
...

// RecordingViewModel.cpp
// Implement like this:
...
Quickstart::Recording RecordingViewModel::DefaultRecording()
{
    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Year(1761);
    releaseDateTime.Month(1);
    releaseDateTime.Day(1);
    m_defaultRecording = winrt::make<Recording>(L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime);
    return m_defaultRecording;
}
...
```

```cppcx
// Recording.h
#include <sstream>
namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;
    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}
        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }
        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }
        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }
        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c_str());
            }
        }
    };
    public ref class RecordingViewModel sealed
    {
    private:
        Recording ^ defaultRecording;
    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Year = 1761;
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }
        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}

// Recording.cpp
#include "pch.h"
#include "Recording.h"
```

接著，從代表標記頁面的類別中公開繫結來源類別。 作法是將 **RecordingViewModel** 類型的屬性加入到 **MainPage**。

如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，則第一次更新`MainPage.idl`。 建置專案，以重新產生`MainPage.h`和`.cpp`，並在這些產生的檔案中將變更合併至您的專案中的項目。

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }
        public RecordingViewModel ViewModel{ get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
// Add this property:
import "RecordingViewModel.idl";
...
RecordingViewModel ViewModel{ get; };
...

// MainPage.h
// Add this property and this field:
...
#include "RecordingViewModel.h"
...
    Quickstart::RecordingViewModel ViewModel();

private:
    Quickstart::RecordingViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();
    m_viewModel = winrt::make<RecordingViewModel>();
}
Quickstart::RecordingViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

```cppcx
// MainPage.h
...
#include "Recording.h"

namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel ^ viewModel;
    public:
        MainPage();

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();
    this->viewModel = ref new RecordingViewModel();
}
```

最後一步是將 **TextBlock** 繫結到 **ViewModel.DefaultRecording.OneLiner** 屬性。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
    </Grid>
</Page>
```

如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，則您必須移除**MainPage::ClickHandler**函式，以建置專案的順序。

結果如下。

![繫結文字方塊](images/xaml-databinding0.png)

## <a name="binding-to-a-collection-of-items"></a>繫結到項目集合

常見的一個情況是繫結到商業物件的集合。 在 C# 和 Visual Basic 中，[**ObservableCollection&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) 泛型類別是適用於資料繫結的集合選擇，因為它實作 [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN) 和 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN) 介面。 當加入或移除項目，或清單本身的屬性變更時，這些介面提供變更通知給繫結。 如果您希望繫結控制項隨著集合中物件屬性的變更一起更新，那麼商業物件也應該實作 **INotifyPropertyChanged**。 如需詳細資訊，請參閱[深入了解資料繫結](data-binding-in-depth.md)。

如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然後您可以深入了解可觀察的集合中的繫結[XAML 控制項中項目，繫結至C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)。 如果您閱讀該主題第一次，然後的目的C++/如下所示的 WinRT 程式碼清單將會更清楚。

下一個範例將 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 繫結到 `Recording` 物件的集合。 首先讓我們將集合加入到檢視模型。 將這些新成員加入到 **RecordingViewModel** 類別。

```csharp
public class RecordingViewModel
{
    ...
    private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
    public ObservableCollection<Recording> Recordings{ get{ return this.recordings; } }
    public RecordingViewModel()
    {
        this.recordings.Add(new Recording(){ ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
        this.recordings.Add(new Recording(){ ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
        this.recordings.Add(new Recording(){ ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
    }
}
```

```cppwinrt
// RecordingViewModel.idl
// Add this property:
...
#include <winrt/Windows.Foundation.Collections.h>
...
Windows.Foundation.Collections.IVector<IInspectable> Recordings{ get; };
...

// RecordingViewModel.h
// Change the constructor declaration, and add this property and this field:
...
    RecordingViewModel();
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> Recordings();

private:
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_recordings;
...

// RecordingViewModel.cpp
// Update/add implementations like this:
...
RecordingViewModel::RecordingViewModel()
{
    std::vector<Windows::Foundation::IInspectable> recordings;

    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Month(7); releaseDateTime.Day(8); releaseDateTime.Year(1748);
    recordings.push_back(winrt::make<Recording>(L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(11); releaseDateTime.Day(2); releaseDateTime.Year(1805);
    recordings.push_back(winrt::make<Recording>(L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(3); releaseDateTime.Day(12); releaseDateTime.Year(1737);
    recordings.push_back(winrt::make<Recording>(L"George Frideric Handel", L"Serse", releaseDateTime));

    m_recordings = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>(std::move(recordings));
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> RecordingViewModel::Recordings() { return m_recordings; }
...
```

```cppcx
// Recording.h
...
public ref class RecordingViewModel sealed
{
private:
    ...
    Windows::Foundation::Collections::IVector<Recording^>^ recordings;
public:
    RecordingViewModel()
    {
        ...
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1748;
        releaseDateTime->Month = 7;
        releaseDateTime->Day = 8;
        Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1805;
        releaseDateTime->Month = 2;
        releaseDateTime->Day = 11;
        recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1737;
        releaseDateTime->Month = 12;
        releaseDateTime->Day = 3;
        recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
        this->Recordings->Append(recording);
    }
    ...
    property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
    {
        Windows::Foundation::Collections::IVector<Recording^>^ get()
        {
            if (this->recordings == nullptr)
            {
                this->recordings = ref new Platform::Collections::Vector<Recording^>();
            }
            return this->recordings;
        };
    }
};
```

然後將 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 繫結到 **ViewModel.Recordings** 屬性。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

我們還未提供資料範本給 **Recording** 類別，因此 UI 架構所能做的只是針對 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 中的每個項目，呼叫 [**ToString**](https://docs.microsoft.com/dotnet/api/system.object.tostring?redirectedfrom=MSDN#System_Object_ToString)。 **ToString** 的預設實作是傳回類型名稱。

![繫結清單檢視](images/xaml-databinding1.png)

若要解決此問題，我們可以是覆寫[ **ToString** ](https://docs.microsoft.com/dotnet/api/system.object.tostring?redirectedfrom=MSDN#System_Object_ToString)傳回的值**OneLineSummary**，或者我們可以提供資料範本。 更常用的解決方案，而更有彈性的一個資料範本選項。 您可以使用內容控制項的 [**ContentTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) 屬性或項目控制項的 [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 屬性來指定資料範本。 以下是為 **Recording** 設計資料範本的兩種方式，同時提供結果的插圖。

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <TextBlock Text="{x:Bind OneLineSummary}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![繫結清單檢視](images/xaml-databinding2.png)

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Margin="6">
                <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                <StackPanel>
                    <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                    <TextBlock Text="{x:Bind CompositionName}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![繫結清單檢視](images/xaml-databinding3.png)

如需 XAML 語法的詳細資訊，請參閱[使用 XAML 建立 UI](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui)。 如需控制項配置的詳細資訊，請參閱[使用 XAML 定義配置](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml)。

## <a name="adding-a-details-view"></a>新增詳細資料檢視

您可以選擇在 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 項目中顯示 **Recording** 物件的所有詳細資料。 但這會佔用大量空間。 相反地，您可以在項目中顯示剛好足夠識別它的資料，然後當使用者做出選擇時，您可以在另一個稱為詳細資料檢視的 UI 中，顯示選定項目的所有詳細資料。 這種安排也稱為主要/詳細資料檢視，或清單/詳細資料檢視。

有兩種作法。 您可以將詳細資料檢視繫結到 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 的 [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 屬性。 或者您可以使用[ **CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)，在此情況下您繫結兩者**ListView**和 詳細資料檢視以**CollectionViewSource**（這麼做讓會為處理的目前選取的項目您）。 這兩種技術會如下所示，兩者都提供相同的結果 （如圖所示）。

> [!NOTE]
> 本主題到目前為止，我們只使用 [{x:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，但以下我們將說明的兩種技巧需要更有彈性 (但效能較低) 的 [{Binding} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。

如果您使用C++/WinRT 或視覺效果C++元件擴充功能 (C++/CX) 然後，若要使用[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組，您必須新增[ **BindableAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)屬性設定為您想要繫結至任何執行階段類別。 若要使用[{x： 繫結}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，您不需要該屬性。

> [!IMPORTANT]
> 如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然後在[ **BindableAttribute** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)屬性時才可以使用您已安裝 Windows SDK 版本 10.0.17763.0 (Windows 10版本 1809年），或更新版本。 如果沒有該屬性中，您必須實作[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)並[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)為了能夠使用的介面[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組。

首先是 [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 技術。

```csharp
// No code changes necessary for C#.
```

```cppwinrt
// Recording.idl
// Add this attribute:
...
[Windows.UI.Xaml.Data.Bindable]
runtimeclass Recording
...
```

```cppcx
[Windows::UI::Xaml::Data::Bindable]
public ref class Recording sealed
{
    ...
};
```

其他只需變更標記。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

使用 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 技術時，請先新增 **CollectionViewSource** 做為頁面資源。

```xml
<Page.Resources>
    <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
</Page.Resources>
```

然後，將 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) (不再需要命名) 和詳細資料檢視上的繫結調整為使用 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。 請注意，將詳細資料檢視直接繫結到 **CollectionViewSource** 時，就意味著您想要繫結至在集合本身找不到路徑之繫結中的目前項目。 不需要指定 **CurrentItem** 屬性做為繫結的路徑 (但如果情況模稜兩可，您可以這樣做)。

```xml
...
<ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">
...
<StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
...
```

以下是各種情況的相同結果。

> [!NOTE]
> 如果您使用C++，則您的 UI 看起來與下圖完全相同： 呈現**ReleaseDateTime**屬性皆不同。 請參閱下的一節，如需詳細資訊。

![繫結清單檢視](images/xaml-databinding4.png)

## <a name="formatting-or-converting-data-values-for-display"></a>格式化或轉換資料值以供顯示

沒有使用上述的轉譯問題。 **ReleaseDateTime**屬性不是只是一個日期，就[ **DateTime** ](/uwp/api/windows.foundation.datetime) (如果您使用C++，則[ **的行事曆**](/uwp/api/windows.globalization.calendar)). 因此，在C#，它會顯示更多有效位數，不需要。 然後在C++所呈現的型別名稱。 一個解決方案是要將字串屬性，加入**錄製**傳回的對等的類別`this.ReleaseDateTime.ToString("d")`。 命名該屬性**ReleaseDate** ，它會傳回日期，並不日期和時間表示。 命名為 **ReleaseDateAsString** 進一步表示傳回字串。

更有彈性的解決辦法是使用所謂的「值轉換器」。 以下是如何撰寫您自己的值轉換器的範例。 如果您使用C#，然後新增下列程式碼，以您`Recording.cs`原始程式碼檔。 如果您使用C++/WinRT，然後新增**Midl 檔 (.idl)** 項目加入專案，名為中所示C++/WinRT 程式碼範例清單下，建置專案以產生`StringFormatter.h`並`.cpp`，將那些檔案新增以您的專案，然後將程式碼清單貼到它們。 也加入`#include "StringFormatter.h"`至`MainPage.h`。

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// StringFormatter.idl
namespace Quickstart
{
    runtimeclass StringFormatter : [default] Windows.UI.Xaml.Data.IValueConverter
    {
        StringFormatter();
    }
}

// StringFormatter.h
#pragma once

#include "StringFormatter.g.h"
#include <sstream>

namespace winrt::Quickstart::implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter>
    {
        StringFormatter() = default;

        Windows::Foundation::IInspectable Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
        Windows::Foundation::IInspectable ConvertBack(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
    };
}

namespace winrt::Quickstart::factory_implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter, implementation::StringFormatter>
    {
    };
}

// StringFormatter.cpp
#include "pch.h"
#include "StringFormatter.h"
#include "StringFormatter.g.cpp"

namespace winrt::Quickstart::implementation
{
    Windows::Foundation::IInspectable StringFormatter::Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar valueAsCalendar{ value.as<Windows::Globalization::Calendar>() };

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar.MonthAsNumericString().c_str();
        wstringstream << L"/" << valueAsCalendar.DayAsString().c_str();
        wstringstream << L"/" << valueAsCalendar.YearAsString().c_str();
        return winrt::box_value(hstring{ wstringstream.str().c_str() });
    }

    Windows::Foundation::IInspectable StringFormatter::ConvertBack(Windows::Foundation::IInspectable const& /* value */, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        throw hresult_not_implemented();
    }
}
```

```cppcx
...
public ref class StringFormatter sealed : Windows::UI::Xaml::Data::IValueConverter
{
public:
    virtual Platform::Object^ Convert(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar^ valueAsCalendar = dynamic_cast<Windows::Globalization::Calendar^>(value);

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar->MonthAsNumericString()->Data();
        wstringstream << L"/" << valueAsCalendar->DayAsString()->Data();
        wstringstream << L"/" << valueAsCalendar->YearAsString()->Data();
        return ref new Platform::String(wstringstream.str().c_str());
    }

    // No need to implement converting back on a one-way binding
    virtual Platform::Object^ ConvertBack(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        throw ref new Platform::NotImplementedException();
    }
};
...
```

> [注意 ！]針對C++/WinRT 程式碼清單上方，在`StringFormatter.idl`，我們會使用[預設屬性](https://docs.microsoft.com/windows/desktop/midl/default)宣告**IValueConverter**做為預設介面。 在清單中， **StringFormatter**都只有建構函式，以及沒有任何方法，所以沒有預設的介面會為它產生。 `default`屬性是如果您不會新增至執行個體成員的最佳**StringFormatter**，因為沒有 QueryInterface 才能呼叫**IValueConverter**方法。 或者，您可以在其中提示預設值**IStringFormatter**介面產生，這麼做之後，執行階段加上附註類別本身，而[default_interface 屬性](https://docs.microsoft.com/uwp/midl-3/predefined-attributes#the-default_interface-attribute)。 選項是如果您新增執行個體成員，以達到最佳**StringFormatter**呼叫的頻率的方法超過**IValueConverter**是，因為則將需要沒有 QueryInterface 呼叫執行個體成員。

我們可以加入的執行個體**StringFormatter**做為頁面資源並使用它的繫結中**TextBlock**以顯示**ReleaseDateTime**屬性。

```xml
<Page.Resources>
    <local:StringFormatter x:Key="StringFormatterValueConverter"/>
</Page.Resources>
...
<TextBlock Text="{Binding ReleaseDateTime,
    Converter={StaticResource StringFormatterValueConverter},
    ConverterParameter=Released: \{0:d\}}"/>
...
```

您可以看到上面，格式化彈性我們使用標記來將格式字串傳遞至轉換器的轉換子參數透過。 本主題中，只有 所示的程式碼範例C#值轉換器會使用該參數。 但您可以輕鬆地傳遞C++-樣式格式字串做為轉換子參數，並使用，在您使用的格式設定的值轉換器函式這類**wprintf**或是**swprintf**。

結果如下。

![顯示自訂格式的日期](images/xaml-databinding5.png)

> [!NOTE]
> 從 Windows 10 1607年版中，XAML 架構會提供內建的可見性布林值來轉換子。 轉換器 maps **，則為 true**要**Visibility.Visible**列舉值和**false**至**Visibility.Collapsed**讓您可以繫結布林值，而不需建立轉換子的可見性屬性。 若要使用內建轉換器，您 App 的最低目標 SDK 版本必須為 14393 或更新版本。 當您的 App 是以舊版 Windows 10 為目標時，您就無法使用它。 如需版本為目標的詳細資訊，請參閱[調適性版本的程式碼](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="see-also"></a>另請參閱
* [資料繫結](index.md)
