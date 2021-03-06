---
description: 本文會逐步引導您使用 C# 建立適用於 WinUI 3 的 XAML 樣板化控制項。
title: 使用 C# 製作的適用於 WinUI 3 應用程式的樣板化 XAML 控制項
ms.date: 03/05/2021
ms.topic: article
keywords: windows 10, uwp, 自訂控制項, 樣板化控制項, winui
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 3caadca2c6aae1ecceed534f9d9597f126310f3d
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254622"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-c"></a>使用 C# 製作的適用於 WinUI 3 應用程式的樣板化 XAML 控制項

本文會逐步引導您使用 C# 建立適用於 WinUI 3 的樣板化 XAML 控制項。 樣板化控制項繼承自 **Microsoft.UI.Xaml.Controls.Control**，並且具有可使用 XAML 控制項範本自訂的視覺效果結構和視覺效果行為。

在遵循本文中的步驟之前，您應該確定您的開發環境已設定為建立 WinUI 3 應用程式。 如需設定資訊，請參閱[開始使用適用於桌面應用程式的 WinUI 3](./get-started-winui3-for-desktop.md)。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>建立空白應用程式 (BgLabelControlApp)

請先在 Microsoft Visual Studio 中，建立新的專案。 在 [建立新專案] 對話方塊中，選取 [空白應用程式 (UWP 中的 WinUI)] 專案範本，並務必選取 C# 語言版本。 將專案名稱設定為 "BgLabelControlApp"，讓檔案名稱與下列範例中的程式碼一致。 將 [目標版本] 設定為 Windows 10 版本 1903 (組建 18362)，並將 [最低版本] 設定為 Windows 10 版本 1803 (組建 17134)。 本逐步解說也適用於使用 **已封裝的空白應用程式 (WinUI in Desktop)** 專案範本所建立的桌面應用程式，只是請務必執行 **BgLabelControlApp (Desktop)** 專案中的所有步驟。

![空白應用程式專案範本](images/winui-csharp-new-project-uwp.png)

## <a name="add-a-templated-control-to-your-app"></a>將樣板化控制項新增至您的應用程式

若要新增樣板化控制項，請按一下工具列中的 [專案] 功能表，或以滑鼠右鍵按一下 [方案總管] 中的專案，然後選取 [新增項目]。 在 [Visual C# -> WinUI] 下選取 [自訂控制項 (WinUI)] 範本。 將新控制項命名為 "BgLabelControl"，然後按一下 [新增]。 

## <a name="update-the-custom-control-c-file"></a>更新自訂控制項 c # 檔案

在 c # 檔案中 BgLabelControl.cs，請注意，此函式會定義控制項的 **DefaultStyleKey** 屬性。 此索引鍵會識別當控制項的取用者未明確指定範本時，將會使用的預設範本。 索引鍵值是控制項的「類型」。 我們會在稍後實作泛型範本檔案時，看到此索引鍵在使用中。

```csharp
public BgLabelControl()
{
    this.DefaultStyleKey = typeof(BgLabelControl);
}
```

我們的樣板化控制項會有一個文字標籤，可在程式碼、XAML 中或透過資料繫結以程式設計方式設定。 為了讓系統將控制項標籤的文字保持在最新狀態，必須以 [DependencyPropety](/uwp/api/Windows.UI.Xaml.DependencyProperty) 的形式實作。 若要這麼做，請先宣告字串屬性，並將其稱為 **Label**。 我們不使用支援變數，而是藉由呼叫 [GetValue](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 和 [SetValue](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 來設定和取得相依性屬性的值。 這些方法是由 [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject) 提供，**Microsoft.UI.Xaml.Controls.Control** 會繼承。

```csharp
public string Label
{
    get => (string)GetValue(LabelProperty);
    set => SetValue(LabelProperty, value);
}
```
接下來，藉由呼叫 [DependencyProperty.Register](/uwp/api/windows.ui.xaml.dependencyproperty.register)，宣告相依性屬性並且向系統註冊。 這個方法會指定 **Label** 屬性的名稱和類型、屬性擁有者的類型、**BgLabelControl** 類別，以及屬性的預設值。

```csharp
DependencyProperty LabelProperty = DependencyProperty.Register(
    nameof(Label), 
    typeof(string),
    typeof(BgLabelControl), 
    new PropertyMetadata(default(string)));
```

這兩個步驟都是實作相依性屬性的必要步驟，但是在此範例中，我們將為 **OnLabelChanged** 事件新增選擇性的處理常式。 每當更新屬性值時，系統就會引發這個事件。 在此情況下，我們會檢查新的標籤文字是否為空字串，並據以更新類別變數。

```csharp
public bool HasLabelValue { get; set; }

private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    {
        BgLabelControl labelControl = d as BgLabelControl; //null checks omitted
        String s = e.NewValue as String; //null checks omitted
        if (s == String.Empty)
        {
            labelControl.HasLabelValue = false;
        }
        else
        {
            labelControl.HasLabelValue = true;
        }
    }
}
```
如需相依性屬性運作方式的詳細資訊，請參閱[相依性屬性概觀](/windows/uwp/xaml-platform/dependency-properties-overview)。

## <a name="define-the-default-style-for-bglabelcontrol"></a>定義 BgLabelControl 的預設樣式
如果控制項的使用者未明確設定樣式，樣板化控制項就必須提供預設樣式範本。 在這個步驟中，我們將修改控制項的一般範本檔案。

當您將 **自訂控制項 (WinUI)** 新增至您的應用程式時，就會產生一般範本檔案。 檔案的名稱為 "Generic"，並且會在 [方案瀏覽器] 的 [ **主題** ] 資料夾中產生。 需要資料夾和檔案名，XAML 架構才能找到樣板化控制項的預設樣式。 刪除 Generic.xaml 的預設內容，並且貼到下列標記中。



```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

在此範例中，您可以看到 **樣式** 元素的 **TargetType** 屬性已設定為 **BgLabelControlApp** 命名空間內的 **BgLabelControl** 類型。 此類型的值與我們在控制項建構函式中針對 **DefaultStyleKey** 屬性所指定的值相同，其會將此內容識別為控制項的預設樣式。

控制項範本中 **TextBlock** 的 **Text** 屬性，會繫結至控制項的 **Label** 相依性屬性。 屬性會使用 [TemplateBinding](/windows/uwp/xaml-platform/templatebinding-markup-extension) 標記延伸來繫結。 這個範例也會將 **Grid** 背景繫結至 **Background** 相依性屬性，此屬性繼承自 **Control** 類別。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>將 BgLabelControl 的執行個體新增至主要 UI 頁面

開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 緊接在 (**StackPanel** 內的) **Button** 元素之後，新增下列標記。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

建置並執行應用程式，您會看到樣板化控制項，其中包含我們指定的背景色彩和標籤。

![樣板化控制項結果](images/winui-templated-control-result.png)


