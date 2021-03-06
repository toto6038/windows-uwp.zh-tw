---
title: 使用 Xamarin 建立 Android 應用程式
description: 如何使用 Xamarin 開始撰寫 Android 應用程式
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin、教學課程、xaml
ms.date: 04/28/2020
ms.openlocfilehash: f14947564f08872ec1849d0d0b2f14218fb10322
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254299"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>開始使用 Xamarin 進行 Android 開發

本指南將協助您開始在 Windows 上使用 Xamarin，以建立可在 Android 裝置上運作的跨平臺應用程式。

在本文中，您將使用 Xamarin. Android 和 Visual Studio 2019 建立簡單的 Android 應用程式。

## <a name="requirements"></a>規格需求

若要使用本教學課程，您需要下列各項：

- Windows 10
- [Visual Studio 2019：社區、專業或企業](https://visualstudio.microsoft.com/downloads/) (請參閱附注) 
- Visual Studio 2019 的「使用 .NET 進行行動開發」工作負載

> [!NOTE]
> 本指南將適用于 Visual Studio 2017 或2019。 如果您使用 Visual Studio 2017，某些指令可能會因為兩個 Visual Studio 版本之間的 UI 差異而不正確。

您也會有 Android 手機或已設定的模擬器，可在其中執行您的應用程式。 請參閱設定 [Android 模擬器](emulator.md)。

## <a name="create-a-new-xamarinandroid-project"></a>建立新的 Xamarin.Android 專案

啟動 Visual Studio。 選取 [檔案 > 新的 > 專案]，以建立新專案。

在 [新增專案] 對話方塊中，選取 [ **Xamarin) 範本 (的 Android 應用程式** ，然後按 **[下一步]**。

將專案命名為 **TimeChangerAndroid** ，然後按一下 [ **建立**]。

在 [新的跨平臺應用程式] 對話方塊中，選取 [ **空白應用程式**]。 在 [ **最小 Android 版本**] 中，選取 [ **Android 5.0 (棒)**。 按一下 [確定]  。

Xamarin 會使用名為 **TimeChangerAndroid** 的單一專案來建立新的方案。

## <a name="create-a-ui-with-xaml"></a>使用 XAML 建立 UI

在專案的 **Resources\layout** 目錄中，開啟 **activity_main.xml**。 此檔案中的 XML 會定義使用者開啟 TimeChanger 時所看到的第一個畫面。

TimeChanger 的 UI 很簡單。 它會顯示目前的時間，並有按鈕可調整以一小時為單位的增量時間。 它會使用垂直 `LinearLayout` 對齊按鈕上方的時間，並使用水準 `LinearLayout` 來並排排列按鈕。 將 [ **android：重力** ] 屬性設定為 [置中] 的垂直位置，以將內容 **置** 中在畫面上 `LinearLayout` 。

以下列程式碼取代 **activity_main.xml** 的內容。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

現在您可以執行 **TimeChangerAndroid** ，並查看您所建立的 UI。 在下一節中，您會將功能加入至 UI，以顯示目前的時間，並啟用按鈕以執行動作。

## <a name="add-logic-code-with-c"></a>使用 C 新增邏輯程式碼#

開啟 **MainActivity.cs**。 此檔案包含可將功能新增至 UI 的程式碼後端邏輯。

### <a name="set-the-current-time"></a>設定目前的時間

首先，取得 `TextView` 將顯示時間的參考。 使用 **FindViewById** 來搜尋具有正確 **android： id** (的所有 UI 元素，此專案是在 `"@+id/timeDisplay"` 上一個步驟) 的 xml 中設定為。 這是 `TextView` 將會顯示目前時間的。

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Ui 控制項必須在 UI 執行緒上更新。 從另一個執行緒所做的變更，可能無法適當地更新控制項在畫面上顯示的內容。 因為不保證此程式碼永遠會在 UI 執行緒上執行，所以請使用 **RunOnUiThread** 方法來確定是否有任何更新正確顯示。 以下是完整的 `UpdateTimeLabel` 方法。

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>每秒更新一次目前的時間

目前，在 TimeChangerAndroid 啟動之後，目前的時間會是正確的，最多隻會有一秒的時間。 標籤必須定期更新，才能保持精確的時間。 **計時器** 物件會定期呼叫回呼方法，以使用目前的時間來更新標籤。

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>建立按鈕 Click 事件處理常式

所有的向上和向下按鈕都需要遞增或遞減 HourOffset 屬性，然後呼叫 UpdateTimeLabel。

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>將向上和向下按鈕連接至其對應的事件處理常式

若要將按鈕與其對應的事件處理常式產生關聯，請先使用 FindViewById 依識別碼尋找按鈕。 一旦有了 button 物件的參考之後，就可以將事件處理常式加入其 `Click` 事件中。

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>完成的 MainActivity.cs 檔案

當您完成時，MainActivity.cs 應該看起來像這樣：

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>執行應用程式

若要執行應用程式，請按 **F5** 或按一下 [Debug > 開始偵錯工具]。 視 [偵錯工具的設定](emulator.md)方式而定，您的應用程式將會在裝置上或模擬器中啟動。

## <a name="related-links"></a>相關連結

- [在 Android 裝置或模擬器上進行測試](emulator.md)。
- [使用 Xamarin 建立 Android 範例應用程式](xamarin-forms.md)
