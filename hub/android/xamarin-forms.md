---
title: 使用 Xamarin 建立簡單的 Android 應用程式
description: 逐步指南說明如何在 Windows 上開始使用 Xamarin 來建立可在 Android 裝置上運作的跨平臺應用程式。
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin、xaml、教學課程
ms.date: 04/28/2020
ms.openlocfilehash: b1364d8ac19176ec25ee6d45664c7e765accfe1e
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823192"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>開始使用 Xamarin 進行 Android 開發

本指南將協助您開始使用 Windows 上的 Xamarin，建立可在 Android 裝置上運作的跨平臺應用程式。

在本文中，您將使用 Xamarin 和 Visual Studio 2019 建立簡單的 Android 應用程式。

## <a name="requirements"></a>規格需求

若要使用本教學課程，您需要下列各項：

- Windows 10
- [Visual Studio 2019：社區、專業或企業](https://visualstudio.microsoft.com/downloads/) (請參閱附注) 
- Visual Studio 2019 的「使用 .NET 進行行動開發」工作負載

> [!NOTE]
> 本指南將適用于 Visual Studio 2017 或2019。 如果您使用 Visual Studio 2017，某些指令可能會因為兩個 Visual Studio 版本之間的 UI 差異而不正確。

您也會有 Android 手機或已設定的模擬器，可在其中執行您的應用程式。 請參閱 [在 Android 裝置或模擬器上進行測試](emulator.md)。

## <a name="create-a-new-xamarinforms-project"></a>建立新的 Xamarin. Forms 專案

啟動 Visual Studio。 按一下 [檔案] > 新增 > 專案]，以建立新專案。

在 [新增專案] 對話方塊中，選取 ([Xamarin]) 範本的行動 **應用程式** ，然後按 **[下一步]**。

將專案命名為 **TimeChangerForms** ，然後按一下 [ **建立**]。

在 [新的跨平臺應用程式] 對話方塊中，選取 [ **空白**]。 在 [平臺] 區段中，檢查 **Android** 並取消核取其他所有方塊。 按一下 [確定]  。

Xamarin 將會建立具有兩個專案的新方案： **TimeChangerForms** 和 **TimeChangerForms。**

## <a name="create-a-ui-with-xaml"></a>使用 XAML 建立 UI

展開 [ **TimeChangerForms** ] 專案，然後開啟 [ **MainPage**]。 此檔案中的 XAML 會定義使用者開啟 TimeChanger 時所看到的第一個畫面。

TimeChanger 的 UI 很簡單。 它會顯示目前的時間，並有按鈕可調整以一小時為單位的增量時間。 它會使用垂直 StackLayout 來對齊按鈕上方的時間，並使用水準 StackLayout 來並排排列按鈕。 將垂直 StackLayout 的 **HorizontalOptions** 和 **VerticalOptions** 設定為 **"CenterAndExpand"**，以在畫面中將內容置中。

以下列程式碼取代 MainPage 的內容。

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

到目前為止，UI 已完成。 不過，TimeChangerForms 將不會建立，因為 **UpButton_Clicked** 和 **DownButton_Clicked** 方法會在 XAML 中參考，但是不會在任何地方定義。 即使應用程式已執行，目前的時間也不會顯示。 在下一節中，您將修正這些錯誤，並將功能新增至您的 UI。

## <a name="add-logic-code-with-c"></a>使用 C 新增邏輯程式碼#

在 [方案 Explorer] 中，以滑鼠右鍵按一下 [MainPage]，然後按一下 [ **視圖程式碼**]。 此檔案包含會將功能加入至 UI 的程式碼後接。

### <a name="set-the-current-time"></a>設定目前的時間

這個檔案中的程式碼可以參考使用控制項 **x：Name** 屬性值在 XAML 中宣告的控制項。 在此情況下，會呼叫顯示目前時間的標籤 `time` 。

您必須在主執行緒上更新 UI 控制項。 從另一個執行緒所做的變更，可能無法適當地更新控制項在畫面上顯示的內容。 因為不保證此程式碼永遠會在主執行緒上執行，所以請使用 **BeginInvokeOnMainThread** 方法來確定是否有任何更新正確顯示。 以下是完整的 UpdateTimeLabel 方法。

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

目前，在 TimeChangerForms 啟動之後，目前的時間會是正確的，最多隻會有一秒的時間。 標籤必須定期更新，才能保持精確的時間。 **計時器** 物件會定期呼叫回呼方法，以使用目前的時間來更新標籤。

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>新增 HourOffset

向上和向下按鈕會以一小時的增量來調整時間。 加入 **HourOffset** 屬性以追蹤目前的調整。

```csharp
public int HourOffset { get; private set; }
```

現在，更新 UpdateTimeLabel 方法以留意 HourOffset 屬性。

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>新增按鈕 Click 事件處理常式

所有的向上和向下按鈕都需要遞增或遞減 HourOffset 屬性，然後呼叫 UpdateTimeLabel。

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

當您完成時，MainPage.xaml.cs 應該看起來像這樣：

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

若要執行應用程式，請按 **F5** 或按一下 [Debug > 開始偵錯工具]。 視 [偵錯工具的設定](emulator.md)方式而定，您的應用程式將會在裝置上或模擬器中啟動。

## <a name="related-links"></a>相關連結

- [在 Android 裝置或模擬器上進行測試](emulator.md)。

- [使用 Xamarin 建立 Android 範例應用程式](xamarin-android.md)
