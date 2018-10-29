---
author: stevewhims
description: 使用 Visual Studio
title: 使用 Visual Studio
ms.assetid: 7FBB50A2-6D22-4082-B333-5153DADDDE9A
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8219f1600297dfa60345fe8130e8954558b8ac61
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5746214"
---
# <a name="getting-started-getting-around-in-visual-studio"></a>開始使用：使用 Visual Studio


## <a name="getting-around-in-microsoft-visual-studio"></a>使用 Microsoft Visual Studio

現在讓我們回到先前建立的專案，並看看如何使用 Microsoft Visual Studio 整合式開發環境 (IDE)。

如果您是位 Xcode 開發人員，則應該很熟悉下面的預設檢視，左窗格內有原始程式檔，中央窗格內有編輯器 (UI 或原始程式碼)，右窗格內則有控制項及其屬性。

![Xcode 開發環境](images/ios-to-uwp/xcode-ide.png)

Microsoft Visual Studio 看起來非常類似，但預設檢視的控制項在 \[**工具箱**\] 的左邊。 原始檔位於 \[**方案總管**\] 右側，而屬性則位於 \[**方案總管**\] 窗格底下的 \[**屬性**\]，如下所示：

![Visual Studio 開發環境](images/ios-to-uwp/vs-ide.png)

如果您覺得有點怪怪的，您會很高興知道您可以在 Visual Studio 中重新排列這些窗格，您可以將來源檔案放在畫面左側，將工具箱放在右側。 事實上，您可以按一下並拖曳任何窗格的標題列來將它重新定位，在您放開滑鼠後，Visual Studio 將會顯示一個灰色方塊，告訴您窗格的停駐位置。 許多窗格在其標題列中也會有小型圖釘圖示。 這可讓您依現狀釘選面板，就地鎖定。 取消釘選窗格，而且您可以摺疊窗格以節省空間：這對小型監視器非常有用。 如果您將窗格弄亂 (別擔心，我們都有過這種經驗)，從 \[**視窗**\] 功能表中選取 \[**重設視窗配置**\] 以還原順序。

## <a name="adding-controls-setting-their-properties-and-responding-to-events"></a>新增控制項、設定其屬性以及回應事件

現在可將一些控制項新增到您的專案。 然後變更某些屬性，以及撰寫一些程式碼以回應其中一個控制項的事件。

若要在 Xcode 中新增控制項，您要開啟所需的 .xib 檔案或腳本，然後拖放控制項，例如 **Round Rect Button** 或 **Label**，如下所示。

![在 Xcode 中設計 UI](images/ios-to-uwp/xcode-add-button-label.png)

讓我們在 Visual Studio 執行類似的步驟。 在 \[**工具箱**\] 中，拖曳 \[**Button**\] 控制項，然後將它放到 MainPage.xaml 檔案的設計表面。

對 **TextBlock** 控制項執行相同的動作，看起來就像這樣：

![在 Visual Studio 中設計 UI](images/ios-to-uwp/vs-add-button-label.png)

Visual Studio 與 Xcode 不一樣，後者會將配置及繫結資訊隱藏在 .xib 或腳本檔案內部，而前者會鼓勵您使用豐富、可編輯、宣告式且類似 XML 架構的語言來編輯用來儲存這些詳細資料的 XAML 檔案。 如需有關 Extensible Application Markup Language (XAML) 的詳細資訊，請參閱 [XAML 概觀](https://msdn.microsoft.com/library/windows/apps/mt185595)。 現在，您只需知道 \[**設計**\] 窗格中顯示的所有物件都是在 \[**XAML**\] 窗格中定義。 \[**XAML**\] 窗格能夠視需要進行更精確的控制。隨著您了解得越多，也就可以更快地手動開發使用者介面程式碼。 不過，在目前這個階段，讓我們先只專注於 \[**設計**\] 和 \[**屬性**\] 窗格。

讓我們來變更按鈕的詳細資料。 稍後您將會了解，若要在 Xcode 中變更按鈕的名稱，您會在其屬性面板中，變更 \[**標題**\] 欄位中的值。

使用 Visual Studio 時，您會執行非常類似的動作。 在 \[**設計**\] 窗格中，點選按鈕讓它成為焦點。 接著，在 \[**屬性**\] 窗格中，將 \[**內容**\] 方塊的值從「Button」變更為「Press Me」。 接下來，藉由將 **\[名稱\]** 值從 [&lt;No Name&gt;] 變更為 [myButton] 來更新按鈕控制項的名稱，如下所示：

![Visual Studio 中的按鈕屬性視窗](images/ios-to-uwp/vs-button-properties.png)

現在，我們來撰寫一些程式碼，將 **TextBlock** 控制項的內容變更為「Hello, World!」。 這會在使用者點選按鈕之後發生。

在 Xcode 中，您可撰寫程式碼來產生事件與控制項的關聯，然後再將該程式碼與控制項建立關聯，通常可透過按住 Control 鍵並拖曳按鈕到原始程式碼的方式進行，如下所示：

![在 Xcode 中撰寫以關聯按鈕和事件](images/ios-to-uwp/xcode-add-button-event.png)

```swift
// Swift implementation.

@IBAction func buttonPressed(sender: UIButton) {
    
}
```

Visual Studio 很相似。 \[**屬性**\] 右上角有一個閃電按鈕。 這裡列出了與選取控制項關聯的可能事件，如下所示：

![Visual Studio 中的按鈕事件清單](images/ios-to-uwp/vs-button-event.png)

若要為按鈕的 Click 事件新增程式碼，請先在 \[**設計**\] 窗格中選取按鈕。 然後按一下閃電按鈕，並按兩下 \[**Click**\] 名稱旁的空白方塊。 Visual Studio 便會將「myButton\_Click」事件新增到 \[**Click**\] 方塊，然後在 MainPage.xaml.cs 檔案中新增並顯示對應的事件處理常式，如下所示。

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{

}
```

現在連接 \[**TextBlock**\] 控制項。 在 Xcode 中，您可以透過按住 Control 鍵並拖曳按鈕到原始程式碼的方式，來將控制項與其定義建立關聯，如下所示。

![在 Xcode 中撰寫以關聯標籤及其定義](images/ios-to-uwp/xcode-add-button-reference.png)

```swift
// Swift implentation.

@IBOutlet weak var myLabel : UILabel
```

在 Visual Studio 中，您不需要與控制項建立關聯，因為我們已經為您做好了。 不過我們還是變更一下部分的屬性：

1.  點選 MainPage.xaml 檔案索引標籤。
2.  在 \[**設計**\] 窗格中，點選 \[**TextBlock**\] 控制項。
3.  在 \[**屬性**\] 窗格中，點選扳手按鈕以顯示其屬性。
4.  在 **\[名稱\]** 方塊中，將 &lt;No Name&gt; 變更為 myLabel。

![Visual Studio 中的標籤屬性視窗](images/ios-to-uwp/vs-label-properties.png)

現在要新增一些程式碼到按鈕的 Click 事件。 若要這樣做，請點選 MainPage.xaml.cs 檔案，然後新增下列程式碼到 myButton\_Click 事件處理常式。

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    myLabel.Text = "Hello, World!";
}
```

這與您在 Swift 中的撰寫內容相似：

```swift
@IBAction func buttonPressed(sender: UIButton) {
    myLabel.text = "Hello, World!"
}
```

最後，若要執行 app，請選取 \[**偵錯**\] 功能表，然後再選取 **開始偵錯** \(或是直接按下 F5\)。 在 app 啟動之後，按一下 \[Press Me\] 按鈕，就會看到標籤的內容從「TextBlock」變更為「Hello, World!」，如下圖中所示。

![第一個逐步解說的執行結果：Hello, World!](images/ios-to-uwp/vs-hello-world.png)

若要退出 app，請返回 Visual Studio，點選\ [**偵錯**\] 功能表，然後點選 **停止偵錯** \(或是直接按下 SHIFT + F5\)。 請注意，Visual Studio 可讓您在許多不同的裝置上試用 app，以查看它在每台裝置上的執行方式。

## <a name="next-step"></a>下一步

[開始使用：常用控制項](getting-started-common-controls.md)

