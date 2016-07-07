---
author: mcleanbyron
Description: "您可以鼓勵客戶從您的應用程式啟動意見反應中樞，來留下意見反應。"
title: "從您的應用程式啟動意見反應中樞"
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
translationtype: Human Translation
ms.sourcegitcommit: de85956c7c1d2a0ba509d61ee8928b412f057f8a
ms.openlocfilehash: ccda01d9bfa4ffdff2bbce5d6c60c78e026270e5

---

# 從您的應用程式啟動意見反應中樞

您可以鼓勵客戶將控制項 (例如按鈕) 新增到啟動意見反應中樞的通用 Windows 平台 (UWP) 應用程式，來留下意見反應。 意見反應中樞是預先安裝的應用程式，提供單一位置來收集 Windows 和已安裝應用程式的意見反應。 在 Windows 開發人員中心儀表板的 [意見反應報告](../publish/feedback-report.md) 中，收集透過意見反應中樞針對您的應用程式所提交的所有客戶意見反應，並向您呈現它們，因此，您可以在一份報告中查看客戶已提交的問題、建議和附議。

>
            **注意：**
            **意見反應**報告目前只提供給已加入[開發人員中心測試人員計畫](../publish/dev-center-insider-program.md)的開發人員帳戶。 

若要從您的應用程式啟動意見反應中樞，請使用 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) 所提供的 API。 建議您使用這個 API 從應用程式的 UI 元素啟動意見反應中樞，而應用程式遵循我們的設計指導方針。

>
            **注意** 意見反應中樞僅適用於執行 Windows 10 10.0.14271 版或更新版本的裝置。 建議您只在使用者裝置上可以使用意見反應中樞時，才在您的應用程式中顯示意見反應控制項。 本主題中的程式碼示範操作方法。

## 如何從您的應用程式啟動意見反應中樞

從您的應用程式啟動意見反應中樞：

1. 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。 除了用於啟動意見反應中樞的 API 之外，此 SDK 也會針對其他功能 (例如，在應用程式中使用 A/B 測試來執行實驗，以及顯示廣告) 提供 API。 如需這個 SDK 的詳細資訊，請參閱[利用 Store Engagement and Monetization SDK 讓您的應用程式獲利及吸引客戶](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 \[方案總管\] 中，於專案的 \[參考\] 節點上按一下滑鼠右鍵，然後按一下 \[加入參考\]。
4. 在 \[參考管理員\] 中，展開 \[通用 Windows\]，然後按一下 \[擴充功能\]。
5. 在 SDK 清單中，按一下 \[Microsoft Store Engagement SDK\] 旁邊的核取方塊，然後按一下 \[確定\]。
6. 在專案中，新增您想要向使用者顯示以啟動意見反應中樞的控制項 (例如按鈕)。 建議您設定控制項，如下所示︰
  * 將控制項中所顯示內容的字型設定為 **Segoe MDL2 Assets**。
  * 將控制項中的文字設定為十六進位 Unicode 字元碼 E939。 這是 **Segoe MDL2 Assets** 字型的建議意見反應圖示的字元碼。
  * 將控制項的可見度設定為隱藏。

    > 
            **注意** 意見反應中樞僅適用於執行 Windows 10 10.0.14271 版或更新版本的裝置。 建議您隱藏意見反應控制項 (預設值)，並且只有在使用者裝置上可以使用意見反應中樞時，才使用初始化程式碼來顯示意見反應控制項。 下一步示範操作方法。

  下列程式碼示範如上所述設定的 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) 的 XAML 定義。
  ```xml
  <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
  ```
7. 在裝載意見反應控制項之應用程式頁面的初始化程式碼中，使用 [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) 類別的 [IsSupported](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.issupported.aspx) 屬性判斷是否可以在使用者裝置上使用意見反應中樞。 如果這個屬性傳回 **true**，請將控制項設為可見。 下列程式碼示範 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) 的操作方法。
```CSharp
if (Microsoft.Services.Store.Engagement.Feedback.IsSupported)
{
        this.feedbackButton.Visibility = Visibility.Visible;
}
```
8. 在使用者按一下控制項時執行的事件處理常式中，呼叫 [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) 類別的靜態 [LaunchFeedbackAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.launchfeedbackasync.aspx) 方法，來啟動意見反應中樞應用程式。 這個方法有兩個多載︰一個不含參數，另一個接受索引鍵/值組的字典，而此字典包含您想要與意見反應產生關聯的中繼資料。 下列範例示範如何在 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) 的 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件處理常式中啟動意見反應中樞。
```CSharp
private async void feedbackButton_Click(object sender, RoutedEventArgs e)
{
        await Microsoft.Services.Store.Engagement.Feedback.LaunchFeedbackAsync();
}
```

## 意見反應 UI 的設計建議

若要啟動意見反應中樞，建議您在應用程式中新增 UI 元素 (例如按鈕)，而應用程式使用 Segoe MDL2 Assets 字型和字元碼 E939 來顯示下列標準意見反應圖示。

![]意見反應圖示](images/feedback_icon.PNG)

我們也建議您使用下列一個或多個位置選項來連結到您應用程式中的意見反應中樞。
* 
            **直接在應用程式列中**。 根據您的實作，您可能只想要使用圖示，或新增文字 (如下所示)。

  ![]意見反應圖示](images/feedback_appbar_placement.png)

* 
            **在應用程式的設定中**。 這是存取意見反應中樞的更精緻方式。 在下列範例中，意見反應連結會顯示為應用程式下的其中一個連結。

  ![]意見反應圖示](images/feedback_settings_placement.png)

* 
            **在事件驅動的飛出視窗中**。 當您想要在啟動到 Windows 意見反應中樞之前查詢您的客戶有關特定問題時，這十分有用。 例如，在您的應用程式使用特定功能之後，您可能會向客戶提示他們對該功能滿意度的特定問題。 如果客戶選擇回應，您的應用程式就會啟動意見反應中樞。


## 相關主題

* [意見反應報告](../publish/feedback-report.md)



<!--HONumber=Jun16_HO4-->


