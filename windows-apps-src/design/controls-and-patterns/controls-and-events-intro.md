---
description: 您可以利用控制項 (例如按鈕、文字方塊以及下拉式方塊) 為自己的 app 建立 UI，以顯示資料和取得使用者輸入。 以下說明如何將控制項新增到您的 app。
title: 控制項和模式的簡介
ms.assetid: 64740BF2-CAA1-419E-85D1-42EE7E15F1A5
label: Intro to controls and patterns
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: fe0081a1558bfce9bd00eaaa0a7e4268320ce91b
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804762"
---
# <a name="intro-to-controls-and-patterns"></a>控制項和模式的簡介

在 Windows 應用程式開發中，「控制項」  是顯示內容或啟用互動的 UI 元素。 您可以利用控制項 (例如按鈕、文字方塊以及下拉式方塊) 為自己的 app 建立 UI，以顯示資料和取得使用者輸入。

> **重要 API**：[Windows.UI.Xaml.Controls 命名空間](/uwp/api/windows.ui.xaml.controls)

「模式」  是可修改控制項或結合數個控制項以創造新項目的方法。 例如， [清單/詳細資料](list-details.md) 模式是您可以使用 [SplitView](split-view.md) 控制項進行應用程式流覽的一種方式。 同樣地，您可以自訂 [NavigationView](navigationview.md) 控制項的範本來實作索引標籤模式。

在許多情況下，您可以直接使用控制項。 但 XAML 控制項將功能和結構與外觀分開處理，因此您可以依據需求做出不同程度的修改。 在[樣式](../style/index.md)一節中，您會了解如何使用 [XAML 樣式](xaml-styles.md)與[控制項範本](control-templates.md)來修改控制項。

在本節中，我們會針對您可用來建置 app 之 UI 的每個 XAML 控制項提供指導方針。 做為開始，這篇文章會說明如何將控制項新增至 app。 在 app 使用控制項有 3 個主要步驟：

- 將控制項新增到 app UI。
- 設定控制項上的屬性，例如寬度、高度或前景色彩。
- 在控制項的事件處理常式中新增程式碼，使其執行某些功能。 

## <a name="add-a-control"></a>新增控制項
您可以使用數種方式將控制項新增到應用程式：
 
- 使用設計工具，例如 Blend for Visual Studio 或 Microsoft Visual Studio Extensible Application Markup Language (XAML) 設計工具。 
- 在 Visual Studio XAML 編輯器中將控制項新增到 XAML 標記。 
- 在程式碼中新增控制項。 您在程式碼中新增的控制項會在應用程式執行時顯示，但不會顯示在 Visual Studio XAML 設計工具中。

在 Visual Studio 中的應用程式新增和操作控制項時，您可以使用程式的許多功能，包括 [工具箱]、XAML 設計工具、XAML 編輯器以及 [屬性] 視窗。 

Visual Studio [工具箱] 顯示許多控制項，可以讓您用於自己的應用程式。 若要將控制項新增至您的應用程式，請在 [工具箱] 中按兩下該控制項。 例如，當您按兩下 TextBox 控制項，這個 XAML 就會新增到 [XAML] 檢視。 

```xaml
<TextBox HorizontalAlignment="Left" Text="TextBox" VerticalAlignment="Top"/>
```

您也可以將控制項從 [工具箱] 拖曳到 XAML 設計工具。

## <a name="set-the-name-of-a-control"></a>設定控制項的名稱

若要在程式碼中處理控制項，您需設定其 [x:Name](../../xaml-platform/x-name-attribute.md) 屬性，然後在您的程式碼中以名稱參照該控制項。 您可以在 Visual Studio [屬性] 視窗或 XAML 中設定名稱。 以下說明如何使用 [屬性] 視窗頂端的 [名稱] 文字方塊來變更目前所選控制項的名稱。

命名控制項
1. 選取要命名的元素。
2. 在 [屬性] 面板中，在 [名稱] 文字方塊中輸入名稱。
3. 按 Enter 確認名稱。

![Visual Studio 設計工具中的 [名稱] 屬性](images/add-controls-control-name-designer.png)

以下說明如何在 XAML 編輯器中，透過變更 x:Name 屬性設定控制項的名稱。

```xaml
<Button x:Name="Button1" Content="Button"/>
```

## <a name="set-the-control-properties"></a>設定控制項屬性 

您可以使用屬性 (Property) 來指定控制項的外觀、內容以及其他屬性 (Attribute)。 當您使用設計工具新增控制項時，Visual Studio 可能會幫您設定控制大小、位置和內容的一些屬性。 在 [設計] 檢視中選取和操作控制項，就可以變更部分屬性 (例如 Width、Height 或 Margin)。 這個圖例顯示 [設計] 檢視中的部分調整大小工具。 

![Visual Studio 設計工具中的調整大小工具](images/add-controls-resizing-designer.png)

您可以讓控制項自動變更大小和位置。 在這種情況下，您可以重設 Visual Studio 為您設定的大小和位置屬性。

重設屬性
1. 在 [屬性] 面板中，按一下屬性值旁邊的屬性標記。 屬性功能表隨即開啟。
2. 在屬性功能表中，按一下 [重設]。

![Visual Studio 屬性重設功能表選項](images/add-controls-property-reset.png)

您可以在 [屬性] 視窗、XAML 或在程式碼中設定控制項屬性。 例如，若要變更按鈕的前景色彩，您可以設定控制項的 [前景] 屬性。 這個圖例說明如何使用 [屬性] 視窗的色彩選擇器來設定 [前景] 屬性。 

![Visual Studio 設計工具中的色彩選擇器](images/add-controls-foreground-designer.png)

以下說明如何在 XAML 編輯器中設定 Foreground 屬性。 請注意，開啟的 Visual Studio IntelliSense 視窗可協助您使用語法。 

![XAML 中的 Intellisense 第 1 部分](images/add-controls-foreground-xaml.png)

![XAML 中的 Intellisense 第 2 部分](images/add-controls-foreground-xaml-2.png)

以下是設定 Foreground 屬性之後產生的 XAML。 

```xaml
<Button x:Name="Button1" Content="Button" 
        HorizontalAlignment="Left" VerticalAlignment="Top"
        Foreground="Beige"/>
```

以下說明如何在程式碼中設定 Foreground 屬性。 

```csharp
Button1.Foreground = new SolidColorBrush(Windows.UI.Colors.Beige);
```
```cppwinrt
Button1().Foreground(Media::SolidColorBrush(Windows::UI::Colors::Beige()));
```

## <a name="create-an-event-handler"></a>建立事件處理常式 

每個控制項都有事件可讓您回應使用者的動作，或是應用程式的其他變更。 例如，Button 控制項有一個 Click 事件，會在使用者按一下 Button 時引發。 您可以建立方法來處理事件，該方法稱為事件處理常式。 您可以在 [屬性] 視窗、XAML 或程式碼中建立控制項事件和事件處理常式方法的關聯。 如需事件的詳細資訊，請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)。

若要建立事件處理常式，請選取控制項，然後按一下 [屬性] 視窗頂端的 [事件] 索引標籤。 [屬性] 視窗會列出該控制項可用的所有事件。 以下是 Button 的一些事件。

![Visual Studio 事件清單](images/add-controls-add-event-designer.png)

若要以預設名稱建立事件處理常式，請在 [屬性] 視窗中按兩下事件名稱旁邊的文字方塊。 若要以自訂名稱建立事件處理常式，請在文字方塊中輸入您選擇的名稱，然後按 Enter。 這時會建立事件處理常式，並在程式碼編輯器中開啟程式碼後置檔案。 事件處理常式方法有 2 個參數。 第一個參數是 `sender`，這是附加處理常式之物件的參照。 `sender` 參數是 **Object** 類型。 如果您打算檢查或變更 `sender` 物件本身的狀態，通常需將 `sender` 轉換成更精準的類型。 依據您自己的 app 設計，您會想要一個能夠依據處理程式附加位置來放心轉換 `sender` 的類型。 第二個值是事件資料，通常以 `e` 或 `args` 參數的形式顯示在簽章中。

以下程式碼用來處理 Button (名為 `Button1`) 的 Click 事件。 當您按一下按鈕，按下之 Button 的 Foreground 屬性會設成藍色。 

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Button b = (Button)sender;
    b.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
}
```
```cppwinrt
#MainPage.h
struct MainPage : MainPageT<MainPage>
    {
        MainPage();
        ...
        void Button1_Click(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
    };
    
#MainPage.cpp
void MainPage::Button1_Click(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        auto b{ sender.as<winrt::Windows::UI::Xaml::Controls::Button>() };
        b.Foreground(Media::SolidColorBrush(Windows::UI::Colors::Blue()));
    }
```

您也可以在 XAML 中關聯事件處理常式。 在 XAML 編輯器中，輸入您想要處理的事件名稱。 當您開始輸入時，Visual Studio 會顯示 IntelliSense 視窗。 指定事件之後，您可以在 IntelliSense 視窗中按兩下 [`<New Event Handler>`]，建立以預設名稱命名的新事件處理常式，或從清單中選取現有的事件處理常式。 

以下是出現的 IntelliSense 視窗。 它可協助您建立新的事件處理常式，或選取現有的事件處理常式。

![click 事件的 Intellisense](images/add-controls-add-event-xaml.png)

這個範例說明如何在 XAML 建立 Click 事件和名為 Button_Click 之事件處理常式的關聯。 

```xaml
<Button Name="Button1" Content="Button" Click="Button_Click"/>
```

您也可以在程式碼後置中讓事件與其事件處理常式產生關聯。 以下說明如何在程式碼中關聯事件處理常式。

```csharp
Button1.Click += new RoutedEventHandler(Button_Click);
```
```cppwinrt
Button1().Click({ this, &MainPage::Button1_Click });
```

## <a name="related-topics"></a>相關主題

-   [依功能排序的控制項索引](./index.md)
-   [Windows.UI.Xaml.Controls 命名空間](/uwp/api/windows.ui.xaml.controls)
-   [配置](../layout/index.md)
-   [樣式](../style/index.md)
-   [可用性](../usability/index.md)