---
description: XBind 標記延伸允許在標記中使用函式。
title: x:Bind 中的函式
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 879be9591bae36a1dbcd485387fbb4ac7f502fea
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360077"
---
# <a name="functions-in-xbind"></a>x:Bind 中的函式

> [!NOTE]
> 針對如何在應用程式中以 **{x:Bind}** 使用資料繫結 (以及 **{x:Bind}** 和 **{Binding}** 的完整比較) 的相關資訊，請參閱[深入了解資料繫結](data-binding-in-depth.md)。

從 Windows 10 版本 1607 開始， **{x:Bind}** 支援使用函式作為繫結路徑的分葉步驟。 這可以：

- 使完成值轉換更為簡單
- 使繫結取決於多個參數

> [!NOTE]
> 若要搭配 **{x:Bind}** 使用函式，您 App 的最低目標 SDK 版本必須是 14393 或更新版本。 當您的 App 以舊版 Windows 10 為目標時，您將無法使用函式。 如需目標版本的相關詳細資訊，請參閱[版本調適型程式碼](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

在下列範例中，項目的背景與前景會繫結函式，以根據色彩參數來執行轉換

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

```xaml
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>函式的路徑

函式的路徑是以和其他屬性路徑相同的方式指定，且可以包含句點 (.)、索引子或轉換來找出該函式。

靜態函式可以使用 XMLNamespace:ClassName.MethodName 語法來指定。 例如，使用下列語法繫結至程式碼後置中的靜態函式。

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```

```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

您也可以直接在標記中使用系統函數來完成簡單的案例，例如日期格式設定、文字格式、文字串連等。例如：

```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

如果模式為 OneWay/TwoWay，則函式路徑上會執行變更偵測，且如果那些物件有變更，繫結將會重新評估。

繫結的函式必須︰

- 讓程式碼與中繼資料可以存取。因此 internal / private可在 C# 運作，但是 C++/CX 則需要公用 WinRT 方法
- 多載是根據引數的數量，而非類型，而它會嘗試針對第一個具有相同數量引述的多載進行比對
- 引數類型需要符合傳入的資料，我們不會進行縮小轉換
- 函式的傳回類型必須符合正在使用繫結的屬性類型

繫結引擎會回應以函數名稱引發的屬性變更通知，並視需要重新評估繫結。 例如：

```xaml
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```

```csharp
public class Person:INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> 您可以使用 x:Bind 中的函式，達到與 WPF 中的轉換器和 MultiBinding 所支援的相同案例。

## <a name="function-arguments"></a>函式引數

可以指定多個函式引數，以逗號 (,) 分隔

- 繫結路徑 - 和您直接繫結到該物件相同的語法。
  - 如果模式為 OneWay/TwoWay，則會執行變更偵測，並在物件變更時會重新評估繫結
- 以引號括住常數字串 - 需要引號來將它指定為字串。 上標三角 (^) 可以用來逸出字串中的引號
- 常數的數字 - 例如 123.456
- 布林值 - 指定為 "x:True" 或 "x:False"

### <a name="two-way-function-bindings"></a>雙向函式繫結

在雙向繫結案例中，必須針對繫結的相反方向指定第二個函式。 這是使用 **BindBack** 繫結屬性來完成。 在以下範例中，函式應該有一個引數是推回到模型所需的值。

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```
