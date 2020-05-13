---
Description: 在您的 Windows 應用程式中，接收、處理和管理指標的輸入資料，例如觸控、滑鼠、畫筆/手寫筆和觸控板。
title: 處理指標輸入
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: pen, mouse, touchpad, touch, pointer, input, user interaction, 畫筆, 滑鼠, 觸控板, 觸控 ,指標, 輸入, 使用者互動
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 609f6f1ad33c3736d9ac99ec40cbbfe7ade9500a
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234820"
---
# <a name="handle-pointer-input"></a>處理指標輸入

在您的 Windows 應用程式中，接收、處理和管理來自指標裝置（例如觸控、滑鼠、畫筆/手寫筆和觸控板）的輸入資料。

> [!Important]
> 只有在需求明確且定義清楚，而且沒有平台控制項支援的互動可以支援您的情況時，才能建立自訂互動。  
> 若您在您的 Windows 應用程式中自訂互動體驗，使用者會預期他們一致、直覺化且可搜尋到。 基於這些理由，我們建議您在[平台控制項](../controls-and-patterns/controls-by-function.md)支援的項目上為您的自訂互動建模。 平臺控制項提供完整的 Windows 應用程式使用者互動體驗，包括標準互動、動畫物理效果、視覺化意見反應和協助工具。 

## <a name="important-apis"></a>重要 API
- [Windows.Devices.Input](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
- [Windows. UI. Xaml. 輸入](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>指標
大多數互動體驗通常會牽涉到使用者透過使用輸入裝置指向想要互動的物件來識別該物件，例如觸控、滑鼠、畫筆/手寫筆及觸控板。 由於這些輸入裝置所提供的原始人性化介面裝置 (HID) 資料包含許多常用屬性，因此，會將內容升級並合併到整合的輸入堆疊，並公開為與裝置無關的指標資料。 然後您的 Windows 應用程式就可以取用此資料，而不必擔心所使用的輸入裝置。

> [!NOTE]
> 裝置特定的資訊也會視您應用程式的需求，從原始 HID 資料升級。

輸入堆疊上的每個輸入點 (或接觸點) 是利用 [**Pointer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.Pointer) 物件來表示，此物件是透過各種不同指標事件處理常式中的 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 參數來公開。 如果有多個手寫筆或是多點觸控輸入，就會將每個接觸點視為不同的單一輸入指標。

## <a name="pointer-events"></a>指標事件

指標事件會公開基本資訊，例如輸入裝置類型和偵測狀態 (位於範圍或接觸點內)，以及取得延伸資訊 (例如位置、壓力和接觸幾何)。 此外，也能取得特定的裝置屬性 (例如，使用者按下哪一個滑鼠按鈕，或者是否使用了畫筆橡皮擦的筆尖)。 如果您的應用程式必須區別輸入裝置及其功能，請參閱[識別輸入裝置](identify-input-devices.md)。

Windows 應用程式可以接聽下列指標事件：

> [!NOTE]
> 藉由在指標事件處理常式中的該項目上呼叫 [**CapturePointer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.capturepointer) 來輸入特定 UI 項目的限制指標。 當項目擷取指標時，只有該物件會接收到指標輸入事件，即使指標移動到物件的界限區域以外也一樣。 [**IsInContact**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isincontact) (按下滑鼠，接觸觸控或手寫筆) 必須為 true，**CapturePointer** 才會成功。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>這會在平台取消指標時發生。 此會在以下情況發生：</p>
<ul>
<li>在輸入表面的範圍內偵測到手寫筆時，即會取消觸控指標。</li>
<li>偵測作用中接觸點的時間不會超過 100 毫秒。</li>
<li>監視器/顯示器已變更 (解析度、設定、多重監視器設定)。</li>
<li>桌面已鎖定，或者使用者已登出。</li>
<li>同時發生的接觸點數目超過裝置支援的數目。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>在另一個 UI 元素擷取指標、指標被釋放，或另一個指標以程式設計方式被擷取時，即會發生此情況。</p>
<div class="alert">
<strong>注意</strong>   沒有對應的指標捕捉事件。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>當指標進入元素的界限區域時，即會發生此情況。 針對觸控、觸控板、滑鼠及手寫筆輸入，發生此情況的方式會稍有不同。</p>
<ul>
<li>觸控需要手指接觸點來引發此事件，不論是從元素上直接向下觸碰，或是移到元素的界限區域內均可。</li>
<li>滑鼠和觸控板一律會在螢幕上顯示一個游標，即使沒有按下滑鼠或觸控板按鈕，仍會引發此事件。</li>
<li>如同觸控，手寫筆會透過從元素上直接向下觸碰，或是移到元素的界限區域內來引發此事件。 但是，手寫筆也有暫留狀態 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)，當此狀態為 true 時，便會引發此事件。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>當指標離開元素的界限區域時，即會發生此情況。 針對觸控、觸控板、滑鼠及手寫筆輸入，發生此情況的方式會稍有不同。</p>
<ul>
<li>觸控需要手指接觸點，並且在將指標移出元素的界限區域時引發此事件。</li>
<li>滑鼠和觸控板一律會在螢幕上顯示一個游標，即使沒有按下滑鼠或觸控板按鈕，仍會引發此事件。</li>
<li>如同觸控，手寫筆會在移出元素的界限區域時引發此事件。 不過，手寫筆也有暫留狀態 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)，當此狀態從 true 變更為 false 時，就會引發此事件。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>當指標在元素的界限區域內變更座標、按鈕狀態、壓力、傾斜或接觸幾何時 (例如，寬度與高度)，就會發生此情況。 針對觸控、觸控板、滑鼠及手寫筆輸入，發生此情況的方式會稍有不同。</p>
<ul>
<li>觸控需要手指接觸點，而且只會在接觸點位於元素的界限區域內時引發此事件。</li>
<li>滑鼠和觸控板一律會在螢幕上顯示一個游標，即使沒有按下滑鼠或觸控板按鈕，仍會引發此事件。</li>
<li>如同觸控，手寫筆會在接觸點位於元素的界限區域內時引發此事件。 不過，手寫筆也有暫留狀態 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)，當此狀態為 True 並位於元素的界線區域內時，就會引發此事件。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>當指標指出元素界限區域內的按下動作 (例如，觸控向下、滑鼠向下、手寫筆向下或觸控板按鈕向下) 時，即會發生此情況。</p>
<p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.capturepointer">CapturePointer</a> 必須從此事件的處理常式中呼叫。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>當指標指出元素界限區域內的放開動作 (例如，觸控向上、滑鼠按鈕向上、手寫筆向上或觸控版按鈕向上)，或如果指標在界限區域外部被擷取時，即會發生此情況。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>發生於旋轉滑鼠滾輪時。</p>
<p>滑鼠輸入會與第一次偵測到滑鼠輸入時指派的單一指標相關聯。 按一下滑鼠按鈕 (左鍵、滾輪或右鍵) 會透過 <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved">PointerMoved</a> 事件建立指標與該按鈕的次要關聯。</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>指標事件範例

以下提供一些來自基本指標追蹤應用程式的程式碼片段，示範如何進行接聽，以及處理多個指標的事件並取得各種相關聯指標的屬性。

![指標應用程式 UI](images/pointers/pointers1.gif)

**從[指標輸入範例下載此範例（基本）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>建立 UI

在此範例中，我們使用[矩形](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.rectangle)（ `Target` ）做為物件耗用指標輸入。 當指標狀態變更時，目標的色彩就會變更。

每個指標的詳細資料都會顯示於浮動的 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 中，該區塊會與指標一起移動。 指標事件本身會在位於矩形右側的 [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 中回報。

這是適用於此範例中 UI 的 Extensible Application Markup Language (XAML)。 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>接聽指標事件

在大部分情況下，我們建議您透過事件處理常式的 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 來取得指標資訊。

如果事件引數未公開所需的指標詳細資料，您可以透過 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 的 [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) 與 [**GetIntermediatePoints**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) 方法，取得公開的延伸 [**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) 資訊。

下列程式碼會設定全域字典物件，用於追蹤每一個作用中的指標，並識別目標物件的各種指標事件接聽程式。

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>處理指標事件

接下來，將使用 UI 回饋來示範基本指標事件處理常式。

-   這個處理常式會管理 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 事件。 我們將事件新增到事件記錄檔、將指標新增到作用中的指標字典，並顯示指標詳細資料。

    > [!NOTE]
    > [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 和 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 事件不一定會成對出現。 您的應用程式應接聽和處理可能得出指標向下 (例如 [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)、[**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)、 以及 [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)) 的任何事件。      

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   這個處理常式會管理 [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered) 事件。 我們將事件新增到事件記錄檔、將指標新增到指標集合，並顯示指標詳細資料。

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   這個處理常式會管理 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) 事件。 我們將事件新增到事件記錄檔，並更新指標詳細資料。

    > [!Important]
    > 滑鼠輸入會與第一次偵測到滑鼠輸入時指派的單一指標相關聯。 按一下滑鼠按鈕 (左鍵、滾輪或右鍵) 會透過 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 事件建立指標與該按鈕的次要關聯。 只在放開相同的滑鼠按鈕時才會觸發 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 事件 (這個事件完成前，沒有其他按鈕可以與該指標關聯)。 由於這個專屬關聯的關係，其他滑鼠按鈕的按一下都會經由 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) 事件進行路由。     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   這個處理常式會管理 [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged) 事件。 我們將事件新增到事件記錄檔、將指標新增到指標陣列 (必要時)，並顯示指標詳細資料。

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   這個處理常式會管理終止與數位板接觸的 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 事件。 我們將事件新增到事件記錄檔、從指標集合移除指標，並更新指標詳細資料。

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   這個處理常式會管理與數位板保持接觸的 [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) 事件。 我們將事件新增到事件記錄檔、從指標陣列移除指標，並更新指標詳細資料。

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   這個處理常式會管理 [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled) 事件。 我們將事件新增到事件記錄檔、從指標陣列移除指標，並更新指標詳細資料。

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   這個處理常式會管理 [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost) 事件。 我們將事件新增到事件記錄檔、從指標陣列移除指標，並更新指標詳細資料。

    > [!NOTE]
    > [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost) 可能會取代 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 發生。 指標擷取可能會因為各種原因遺失，包括使用者互動、使用程式設計方式擷取另一個指標、呼叫 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 等。     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>取得指標屬性

如稍早所述，您必須透過 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 的 [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) 與 [**GetIntermediatePoints**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) 方法，從 [**Windows.UI.Input.PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) 物件取得最延伸的指標資訊。 下列程式碼片段顯示取得的方式。

-   首先，為每個指標建立一個新的 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   接著，我們提供一個方法，用來更新與該指標相關之現有 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 中的指標資訊。

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   最後，我們查詢幾個指標屬性。

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>主要指標
某些輸入裝置，例如觸控數位板或觸控板，可支援比一般單一滑鼠或手寫筆指標更多的指標 (在大多數的案例中為 Surface Hub，因為其支援兩個手寫筆輸入)。 

請使用 **[PointerPointerProperties](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties)** 類別的唯讀 **[IsPrimary](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** 屬性識別和區別單一主要指標 (主要指標在輸入序列期間永遠都是第一個偵測到的指標)。 

藉由識別主要指標，您可以使用它來模擬滑鼠或手寫筆輸入、自訂互動，或提供其他特定功能或 UI。

> [!NOTE]
> 當主要指標在輸入序列期間釋放、取消或遺失時，在新的輸入序列啟動前不會建立主要輸入指標 (輸入序列會在所有指標釋放、取消或遺失時結束)。

## <a name="primary-pointer-animation-example"></a>主要指標動畫範例

這些程式碼片段顯示提供特殊視覺回饋，以協助使用者在您應用程式中區別指標輸入的方式。

此特定應用程式使用色彩和動畫醒目提示主要指標。

![帶有動畫視覺回饋的指標應用程式](images/pointers/pointers-usercontrol-animation.gif)

**從[指標輸入範例下載此範例（使用動畫的 UserControl）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>視覺化回饋

我們會根據 XAML **[Ellipse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.ellipse)** 物件，定義一個 **[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)** 來醒目提示哪個指標正位於畫布上，並使用 **[Storyboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard)** 來為對應到主要指標的橢圓形建立動畫。

**以下是 XAML：**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

以下是其後置程式碼：
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>建立 UI
此範例中的 UI 會限制在輸入 **[Canvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)** 中，我們會在其中追蹤任何指標並轉譯指標指示器和主要指標動畫 (若適用的話)，搭配包含指標計數器和主要指標識別識別碼的標頭列。

以下是 MainPage.xaml：

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>處理指標事件

最後，我們會在 MainPage.xaml.cs 後置程式碼中定義我們的基本指標事件處理常式。 我們不會在此處重新產生程式碼，因為我們已經在先前的範例中涵蓋基礎內容，但您仍然可以從[指標輸入範例 (附帶動畫的 UserControl)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip) 下載可運作的範例。

## <a name="related-articles"></a>相關文章

### <a name="topic-samples"></a>主題範例

- [指標輸入範例 (基本)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
- [指標輸入範例 (附帶動畫的 UserControl)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

### <a name="other-samples"></a>其他範例

- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

### <a name="archive-samples"></a>封存範例

- [輸入：XAML 使用者輸入事件範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [輸入：裝置功能範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [輸入：操作和手勢範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [輸入：觸控點擊測試範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 捲軸、移動流覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [輸入：簡化的筆跡範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
