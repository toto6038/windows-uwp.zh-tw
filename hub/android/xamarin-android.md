---
title: 使用 Xamarin 建立簡單的 Android 應用程式
description: 如何開始使用 Xamarin 撰寫 Android 應用程式
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin、教學課程、xaml
ms.date: 04/28/2020
ms.openlocfilehash: c731b5f96243333e4a4ad150de499ac9459113bc
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255202"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>開始使用 Xamarin 進行 Android 開發

本指南將協助您開始在 Windows 上使用 Xamarin，以建立可在 Android 裝置上執行的跨平臺應用程式。

在本文中，您將使用 Xamarin. Android 和 Visual Studio 2019 建立簡單的 Android 應用程式。

## <a name="requirements"></a>需求

若要使用本教學課程，您需要下列各項：

- Windows 10
- [Visual Studio 2019：社區、專業或企業](https://visualstudio.microsoft.com/downloads/)（請參閱附注）
- Visual Studio 2019 的「使用 .NET 進行行動開發」工作負載

> [!NOTE]
> 本指南將與 Visual Studio 2017 或2019搭配使用。 如果您使用 Visual Studio 2017，某些指示可能會因為 Visual Studio 的兩個版本之間的 UI 差異而不正確。

您也會有 Android 手機或設定的模擬器，以在其中執行您的應用程式。 請參閱設定[Android 模擬器](emulator.md)。

## <a name="create-a-new-xamarinandroid-project"></a>建立新的 Xamarin.Android 專案

啟動 Visual Studio。 選取 [檔案] > [新增 > 專案]，以建立新的專案。

在 [新增專案] 對話方塊中，選取 [ **Android 應用程式（Xamarin）** ] 範本，然後按 **[下一步]**。

將專案命名為**TimeChangerAndroid** ，然後按一下 [**建立**]。

在 [新增跨平臺應用程式] 對話方塊中，選取 [**空白應用程式**]。 在 [**最低 Android 版本**] 中，選取 [ **Android 5.0 （棒糖）**]。 按一下 [確定]  。

Xamarin 會使用名為**TimeChangerAndroid**的單一專案來建立新的方案。

## <a name="create-a-ui-with-xaml"></a>使用 XAML 建立 UI

在專案的**Resources\layout**目錄中，開啟**activity_main .xml**。 此檔案中的 XML 會定義使用者開啟 TimeChanger 時將看到的第一個畫面。

TimeChanger 的 UI 很簡單。 它會顯示目前的時間，並具有按鈕來調整一小時的增量時間。 它會使用垂直`LinearLayout`對齊按鈕上方的時間和水準`LinearLayout`並排排列按鈕。 內容會在畫面中置中，其方式是將**android：重力**屬性設定為`LinearLayout`垂直**中央**。

將**activity_main**的內容取代為下列程式碼。

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

此時，您可以執行**TimeChangerAndroid** ，並查看您已建立的 UI。 在下一節中，您將在 UI 中新增功能，以顯示目前的時間，並啟用按鈕來執行動作。

## <a name="add-logic-code-with-c"></a>使用 C 新增邏輯程式碼#

開啟 **MainActivity.cs**。 此檔案包含程式碼後置邏輯，其會將功能加入至 UI。

### <a name="set-the-current-time"></a>設定目前的時間

首先，取得將會顯示時間`TextView`的參考。 使用**FindViewById**來搜尋所有 UI 元素，其中包含正確的**android： id** （在上一個步驟的`"@+id/timeDisplay"` xml 中設定為）。 這是`TextView`會顯示目前時間的。

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Ui 控制項必須在 UI 執行緒上更新。 從另一個執行緒所做的變更，可能無法在控制項于螢幕上顯示時適當地更新。 由於不保證此程式碼一律會在 UI 執行緒上執行，因此請使用**RunOnUiThread**方法，以確保任何更新都能正確顯示。 以下是完整`UpdateTimeLabel`的方法。

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

此時，在 TimeChangerAndroid 啟動之後，目前的時間會精確到最多一秒。 標籤必須定期更新，以保持正確的時間。 **計時器**物件會定期呼叫會以目前時間更新標籤的回呼方法。

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>建立按鈕的 Click 事件處理常式

[向上] 和 [向下] 按鈕必須執行 [遞增] 或 [遞減] HourOffset 屬性，並呼叫 UpdateTimeLabel。

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>將向上和向下按鈕連接到其對應的事件處理常式

若要將按鈕與對應的事件處理常式產生關聯，請先使用 FindViewById 來依識別碼尋找按鈕。 一旦有了按鈕物件的參考之後，您就可以將事件處理常式加入其`Click`事件中。

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>已完成的 MainActivity.cs 檔案

當您完成時，MainActivity.cs 看起來應該像這樣：

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

## <a name="run-your-app"></a>執行您的應用程式

若要執行應用程式，請按**F5**或按一下 [Debug] > 開始進行偵錯工具。 視[偵錯工具的設定](emulator.md)方式而定，您的應用程式將會在裝置或模擬器中啟動。

## <a name="related-links"></a>相關連結

- [在 Android 裝置或模擬器上進行測試](emulator.md)。
- [使用 Xamarin 建立 Android 範例應用程式](xamarin-forms.md)
