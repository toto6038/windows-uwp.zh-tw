---
title: 使用 Xamarin 建立簡單的 Android 應用程式
description: 如何開始使用 Xamarin 撰寫 Android 應用程式
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin、xaml、教學課程
ms.date: 04/28/2020
ms.openlocfilehash: a1426bfef9863227c1ac110bc295536786695df7
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255192"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>開始使用 Xamarin 進行 Android 開發

本指南將協助您開始在 Windows 上使用 Xamarin，以建立可在 Android 裝置上工作的跨平臺應用程式。

在本文中，您將使用 Xamarin. Forms 和 Visual Studio 2019 建立簡單的 Android 應用程式。

## <a name="requirements"></a>需求

若要使用本教學課程，您需要下列各項：

- Windows 10
- [Visual Studio 2019：社區、專業或企業](https://visualstudio.microsoft.com/downloads/)（請參閱附注）
- Visual Studio 2019 的「使用 .NET 進行行動開發」工作負載

> [!NOTE]
> 本指南將與 Visual Studio 2017 或2019搭配使用。 如果您使用 Visual Studio 2017，某些指示可能會因為 Visual Studio 的兩個版本之間的 UI 差異而不正確。

您也會有 Android 手機或設定的模擬器，以在其中執行您的應用程式。 請參閱[在 Android 裝置或模擬器上進行測試](emulator.md)。

## <a name="create-a-new-xamarinforms-project"></a>建立新的 Xamarin 專案

啟動 Visual Studio。 按一下 [檔案] > [新增 > 專案]，以建立新的專案。

在 [新增專案] 對話方塊中，選取 [行動**應用程式（Xamarin）** ] 範本，然後按 **[下一步]**。

將專案命名為**TimeChangerForms** ，然後按一下 [**建立**]。

在 [新增跨平臺應用程式] 對話方塊中，選取 [**空白**]。 在 [平臺] 區段中，核取 [ **Android** ] 並取消核取其他所有方塊。 按一下 [確定]  。

Xamarin 會建立包含兩個專案的新方案： **TimeChangerForms**和**TimeChangerForms** 。

## <a name="create-a-ui-with-xaml"></a>使用 XAML 建立 UI

展開 [ **TimeChangerForms** ] 專案，然後開啟**MainPage**。 此檔案中的 XAML 會定義使用者開啟 TimeChanger 時將看到的第一個畫面。

TimeChanger 的 UI 很簡單。 它會顯示目前的時間，並具有按鈕來調整一小時的增量時間。 它會使用垂直 StackLayout 來對齊按鈕上方的時間，而水準 StackLayout 則會並排排列按鈕。 將垂直 StackLayout 的**HorizontalOptions**和**VerticalOptions**設定為 **"CenterAndExpand"**，以將內容集中在畫面中。

將 MainPage 的內容取代為下列程式碼。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

此時，UI 已完成。 不過，TimeChangerForms 將不會建立，因為**UpButton_Clicked**和**DownButton_Clicked**的方法會在 XAML 中參考，但不會定義于任何位置。 即使應用程式已執行，仍不會顯示目前的時間。 在下一節中，您將會修正這些錯誤，並將功能新增至您的 UI。

## <a name="add-logic-code-with-c"></a>使用 C 新增邏輯程式碼#

在 [方案總管中，以滑鼠右鍵按一下 [MainPage]，然後按一下 [ **View Code**]。 這個檔案包含的程式碼後置會將功能加入至 UI。

### <a name="set-the-current-time"></a>設定目前的時間

這個檔案中的程式碼可以參考使用控制項的**x：Name**屬性值，在 XAML 中宣告的控制項。 在此情況下，會呼叫`time`顯示目前時間的標籤。

UI 控制項必須在主執行緒上更新。 從另一個執行緒所做的變更，可能無法在控制項于螢幕上顯示時適當地更新。 由於不保證此程式碼一律會在主執行緒上執行，因此請使用**BeginInvokeOnMainThread**方法來確定任何更新都正確顯示。 以下是完整的 UpdateTimeLabel 方法。

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>每秒更新一次目前的時間

此時，在 TimeChangerForms 啟動之後，目前的時間會精確到最多一秒。 標籤必須定期更新，以保持正確的時間。 **計時器**物件會定期呼叫會以目前時間更新標籤的回呼方法。

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>新增 HourOffset

[上移] 和 [下移] 按鈕會以一小時的增量來調整時間。 加入**HourOffset**屬性來追蹤目前的調整。

```csharp
public int HourOffset { get; private set; }
```

現在更新 UpdateTimeLabel 方法，以瞭解 HourOffset 屬性。

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>新增按鈕 Click 事件處理常式

[向上] 和 [向下] 按鈕必須執行 [遞增] 或 [遞減] HourOffset 屬性，並呼叫 UpdateTimeLabel。

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

當您完成時，MainPage.xaml.cs 看起來應該像這樣：

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>執行應用程式

若要執行應用程式，請按**F5**或按一下 [Debug] > 開始進行偵錯工具。 視[偵錯工具的設定](emulator.md)方式而定，您的應用程式將會在裝置或模擬器中啟動。

## <a name="related-links"></a>相關連結

- [在 Android 裝置或模擬器上進行測試](emulator.md)。

- [使用 Xamarin 建立 Android 範例應用程式](xamarin-android.md)
