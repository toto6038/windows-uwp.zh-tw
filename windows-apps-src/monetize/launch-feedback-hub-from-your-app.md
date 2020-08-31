---
Description: 您可以鼓勵客戶從您的應用程式啟動意見反應中樞，來留下意見反應。
title: 從您的應用程式啟動意見反應中樞
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 意見反應中樞, 啟動
ms.localizationpriority: medium
ms.openlocfilehash: efdc4a4b39f71b26658e3fbaf57287098b23e4be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158492"
---
# <a name="launch-feedback-hub-from-your-app"></a>從您的應用程式啟動意見反應中樞

您可以鼓勵客戶將控制項 (例如按鈕) 新增到啟動意見反應中樞的通用 Windows 平台 (UWP) 應用程式，來留下意見反應。 意見反應中樞是預先安裝的應用程式，提供單一位置來收集 Windows 和已安裝應用程式的意見反應。 透過意見反應中樞提交給您的應用程式的所有客戶意見反應都會收集，並顯示在合作夥伴中心的 [意見反應報告](../publish/feedback-report.md) 中，讓您可以看到客戶在一份報告中提交的問題、建議和 upvotes。

若要從您的應用程式啟動意見反應中樞，請使用 [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) 所提供的 API。 建議您使用這個 API，在遵循我們設計指導方針的應用程式中，從 UI 元素啟動「意見反應中樞」。

> [!NOTE]
> 「意見反應中樞」僅適用於執行以傳統型和行動[裝置系列](../get-started/universal-application-platform-guide.md)為基礎之 Windows 10 OS 10.0.14271 版或更新版本的裝置。 建議您只在使用者裝置上可以使用「意見反應中樞」的情況下，才在您的應用程式中顯示意見反應控制項。 本主題中的程式碼示範操作方法。

## <a name="how-to-launch-feedback-hub-from-your-app"></a>如何從您的應用程式啟動意見反應中樞

從您的應用程式啟動「意見反應中樞」：

1. [安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在方案總管中，以滑鼠右鍵按一下專案的 [ **參考** ] 節點，然後按一下 [ **加入參考**]。
4. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。
5. 在 Sdk 清單中，按一下 [ **Microsoft Engagement 架構** ] 旁的核取方塊，然後按一下 **[確定]**。
6. 在專案中，新增您想要向使用者顯示以啟動意見反應中樞的控制項 (例如按鈕)。 建議您設定控制項，如下所示︰
  * 將控制項中所顯示內容的字型設定為 **Segoe MDL2 Assets**。
  * 將控制項中的文字設定為十六進位 Unicode 字元碼 E939。 這是 **Segoe MDL2 Assets** 字型的建議意見反應圖示的字元碼。
  * 將控制項的可見度設定為隱藏。
    > [!NOTE]
    > 建議您預設隱藏意見反應控制項，而只在使用者裝置上可以使用「意見反應中樞」時，才在初始化程式碼中顯示此控制項。 下一步示範操作方法。

    下列程式碼示範如上所述設定的 [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) 的 XAML 定義。

    ```XML
    <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
    ```

7. 在裝載意見反應控制項之應用程式頁面的初始化程式碼中，使用 [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 類別的靜態 [IsSupported](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.issupported) 方法來判斷是否可以在使用者裝置上使用「意見反應中樞」。 「意見反應中樞」僅適用於執行以傳統型和行動[裝置系列](../get-started/universal-application-platform-guide.md)為基礎之 Windows 10 OS 10.0.14271 版或更新版本的裝置。

    如果這個屬性傳回 **true**，請將控制項設為可見。 下列程式碼示範適用於 [Button](/uwp/api/windows.ui.xaml.controls.button) 的操作方法。

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#ToggleFeedbackVisibility)]
      > [!NOTE]
      > 雖然目前 Xbox 裝置上不支援「意見反應中樞」，但在執行 Windows 10 10.0.14271 版或更新版本的 Xbox 裝置上，**IsSupported** 屬性目前會傳回 **true**。 這是已知的問題，在未來的 Microsoft Store Services SDK 版本中將會修正此問題。  

8. 在使用者按一下控制項時會執行的事件處理常式中，取得 [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 物件，並呼叫 [LaunchAsync](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.launchasync) 方法來啟動「意見反應中樞」App。 這個方法有兩個多載︰一個不含參數，另一個接受索引鍵/值組的字典，其中包含您想要與意見反應產生關聯的中繼資料。 下列範例示範如何在 [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) 的 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件處理常式中啟動意見反應中樞。

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#FeedbackButtonClick)]

## <a name="design-recommendations-for-your-feedback-ui"></a>意見反應 UI 的設計建議

若要啟動意見反應中樞，建議您在應用程式中新增 UI 元素 (例如按鈕)，而應用程式使用 Segoe MDL2 Assets 字型和字元碼 E939 來顯示下列標準意見反應圖示。

![意見反應圖示](images/feedback_icon.PNG)

我們也建議您使用下列一個或多個位置選項來連結到您應用程式中的意見反應中樞。
* **直接在應用程式列中**： 根據您的實作，您可能只想要使用圖示，或新增文字 (如下所示)。

  ![意見反應圖示](images/feedback_appbar_placement.png)

* **在應用程式的設定中**： 這是存取意見反應中樞的更精緻方式。 在下列範例中，意見反應連結會顯示為應用程式下的其中一個連結。

  ![意見反應圖示](images/feedback_settings_placement.png)

* **在事件驅動的飛出視窗中**： 當您想要在啟動到 Windows 意見反應中樞之前查詢您的客戶有關特定問題時，這十分有用。 例如，在您的應用程式使用特定功能之後，您可能會向客戶提示他們對該功能滿意度的特定問題。 如果客戶選擇回應，您的應用程式就會啟動意見反應中樞。


## <a name="related-topics"></a>相關主題

* [意見反應報告](../publish/feedback-report.md)