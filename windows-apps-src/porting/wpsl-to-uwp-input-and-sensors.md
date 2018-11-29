---
description: 與裝置本身及其感應器整合的程式碼牽涉到從使用者輸入和輸出到使用者。
title: I/O、 裝置與 app 模型的移植到 UWP 的 WindowsPhone Silverlight'
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6ef1814443b3831e514eafb3f5a0c58b7703126b
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8197832"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>I/O、 裝置和應用程式模型的移植 WindowsPhone Silverlight 到 UWP


前一個主題是[移植 XAML 與 UI](wpsl-to-uwp-porting-xaml-and-ui.md)。

與裝置本身及其感應器整合的程式碼牽涉到從使用者輸入和輸出到使用者。 它也可能涉及資料處理。 但是這個程式碼通常不被認為是 UI 層或資料層。 這個程式碼包含與震動控制器、加速計、陀螺儀、麥克風和喇叭 (與語音辨識和合成交集)、(地理) 位置以及輸入形式 (例如觸控、滑鼠、鍵盤及手寫筆) 的整合。

## <a name="application-lifecycle-process-lifetime-management"></a>應用程式週期 (處理程序生命週期管理)

WindowsPhone Silverlight 應用程式包含可儲存和還原其應用程式狀態及其檢視狀態以支援被標記並進行後續重新啟用程式碼。 應用程式生命週期的通用 Windows 平台 (UWP) 應用程式極為擁有與 WindowsPhone Silverlight 應用程式，因為它們兩者設計最大化可用的資源相同目標使用者選擇在中有何種應用程式任何時刻前景。 您會發現您的程式碼會相當容易地適應新系統。

**注意：** 按下硬體 [**上一頁**] 按鈕會自動終止 WindowsPhone Silverlight app。 按下行動裝置上的硬體 **\[返回\]** 按鈕 *「不會」* 自動終止 UWP app。 相反地，UWP app 會暫停，然後可能會被終止。 但這些細節對適當地回應應用程式週期事件的應用程式來說是透明的。

「防反彈空檔」是當應用程式變成非使用中，而系統即將引發暫停事件之前的一段時間。 UWP 應用程式沒有防反彈空檔；當 app 變成非使用中時，隨即會引發暫停事件。

如需詳細資訊，請參閱[應用程式週期](https://msdn.microsoft.com/library/windows/apps/mt243287)。

## <a name="camera"></a>相機

WindowsPhone Silverlight 相機擷取程式碼會使用**Microsoft.Devices.Camera**、 **Microsoft.Devices.PhotoCamera**或**Microsoft.Phone.Tasks.CameraCaptureTask**類別。 若要將該程式碼移植到通用 Windows 平台 (UWP)，您可以使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 類別。 在 [**CapturePhotoToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700836) 主題中有提供程式碼範例。 該方法可讓您將相片擷取到存放檔案中，而它需要 app 套件資訊清單中已設定「麥克風」與「網路攝影機」[**裝置功能**](https://msdn.microsoft.com/library/windows/apps/dn934747)。

另一個選項是 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 類別，這同樣需要「麥克風」與「網路攝影機」[**裝置功能**](https://msdn.microsoft.com/library/windows/apps/dn934747)。

UWP app 不支援鏡頭 app。

## <a name="detecting-the-platform-your-app-is-running-on"></a>偵測執行您 app 的平台

思考 app 目標的變更與 windows 10 的方式。 新的概念性模型是針對通用 Windows 平台 (UWP) 設計應用程式，然後在所有 Windows 裝置上執行。 接下來可以決定要啟用的特定裝置系列專屬功能。 如有需要，app 也有選項可供限制其特別針對一或多個裝置系列進行設計。 如需有哪些裝置系列以及如何決定要針對哪個裝置系列進行設計的詳細資訊，請參閱 [UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

**注意：** 建議您不要使用作業系統或裝置系列來偵測功能是否存在。 若要判斷特定作業系統或裝置系列功能是否存在，識別目前的作業系統或裝置系列通常不是最佳的方式。 不要偵測作業系統或裝置系列 (與版本號碼)，而是要測試功能本身是否存在 (請參閱[條件式編譯與調適型程式碼](wpsl-to-uwp-porting-to-a-uwp-project.md))。 如果您必須要求特定的作業系統或裝置系列，請務必將其當做最低的支援版本，而不是專門針對那一個版本來設計測試。

有數項建議技術可用來針對不同的裝置量身打造您的應用程式 UI。 繼續使用自動調整大小元素與動態版面配置面板。 在 XAML 標記中，繼續使用以有效像素 (先前稱為檢視像素) 為單位的大小，讓您的 UI 可隨不同的解析度與縮放比例調整 (請參閱[檢視/有效像素、檢視距離與縮放比例](wpsl-to-uwp-porting-xaml-and-ui.md))。 還有使用 Visual State Manager 的調適型觸發程序與 Setter 讓您的 UI 可隨視窗大小調整 (請參閱 [UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631))。

不過，如果有無法避免的情況，使您不得不偵測裝置系列時，才能那樣做。 在這個範例中，我們使用 [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) 類別來瀏覽到專為行動裝置系列量身訂做的適當頁面，然後確保其會切換回預設頁面。

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

您的應用程式也能從作用中的資源選擇因素來判斷其正在哪個裝置系列上執行。 下列範例說明如何以命令方式來判斷，而 [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 主題還會根據裝置系列因素，針對載入裝置系列特定資源中的類別說明較常見的使用案例。

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

另請參閱[條件式編譯與調適型程式碼](wpsl-to-uwp-porting-to-a-uwp-project.md)。

## <a name="device-status"></a>裝置狀態

WindowsPhone Silverlight 應用程式可以使用**Microsoft.Phone.Info.DeviceStatus**類別來取得應用程式執行所在裝置的相關資訊。 雖然 UWP 沒有與 **Microsoft.Phone.Info** 命名空間直接對等的命名空間，但是以下是一些屬性和事件，可供您在 UWP app 中用來取代對 **DeviceStatus** 類別成員的呼叫。

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ApplicationCurrentMemoryUsage** 和 **ApplicationCurrentMemoryUsageLimit** 屬性 | [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/dn633832) 和 [**AppMemoryUsageLimit**](https://msdn.microsoft.com/library/windows/apps/dn633836) 屬性                                                                                                                                    |
| **ApplicationPeakMemoryUsage** 屬性                                                 | 使用 Visual Studio 中的記憶體分析工具。 如需詳細資訊，請參閱[分析記憶體使用狀況](http://msdn.microsoft.com/library/windows/apps/dn645469.aspx)。                                                                                                                                                                          |
| **DeviceFirmwareVersion** 屬性                                                      | [**EasClientDeviceInformation.SystemFirmwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608144) 屬性 (僅電腦裝置系列)                                                                                                                                                                             |
| **DeviceHardwareVersion** 屬性                                                      | [**EasClientDeviceInformation.SystemHardwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608145) 屬性 (僅電腦裝置系列)                                                                                                                                                                             |
| **DeviceManufacturer** 屬性                                                         | [**EasClientDeviceInformation.SystemManufacturer**](https://msdn.microsoft.com/library/windows/apps/hh701398) 屬性 (僅電腦裝置系列)                                                                                                                                                                                |
| **DeviceName** 屬性                                                                 | [**EasClientDeviceInformation.SystemProductName**](https://msdn.microsoft.com/library/windows/apps/hh701401) 屬性 (僅電腦裝置系列)                                                                                                                                                                                 |
| **DeviceTotalMemory** 屬性                                                          | 沒有對等項目                                                                                                                                                                                                                                                                                                                      |
| **IsKeyboardDeployed** 屬性                                                         | 沒有對等項目。 這個屬性會提供行動裝置硬體鍵盤 (並不常用) 的相關資訊。                                                                                                                                                                                                        |
| **IsKeyboardPresent** 屬性                                                          | 沒有對等項目。 這個屬性會提供行動裝置硬體鍵盤 (並不常用) 的相關資訊。                                                                                                                                                                                                        |
| **KeyboardDeployedChanged** 事件                                                       | 沒有對等項目。 這個屬性會提供行動裝置硬體鍵盤 (並不常用) 的相關資訊。                                                                                                                                                                                                        |
| **PowerSource** 屬性                                                                | 沒有對等項目                                                                                                                                                                                                                                                                                                                      |
| **PowerSourceChanged** 事件                                                            | 處理 [**RemainingChargePercentChanged**](https://msdn.microsoft.com/library/windows/apps/jj207240) 事件 (僅行動裝置系列)。 當 [**RemainingChargePercent**](https://msdn.microsoft.com/library/windows/apps/jj207239) 屬性 (僅行動裝置系列) 的值減少 1% 時，便會引發該事件。 |

## <a name="location"></a>位置

當宣告定位功能在其應用程式套件資訊清單中的應用程式執行於 windows 10 時，系統會提示使用者同意。 如果您的應用程式顯示其自有的自訂同意提示，或如果它提供切換開關，則建議您移除這些，使系統只會提示使用者一次。

## <a name="orientation"></a>方向

UWP app 中，與 **PhoneApplicationPage.SupportedOrientations** 和 **Orientation** 屬性對等的項目是 app 套件資訊清單中的 [**uap:InitialRotationPreference**](https://msdn.microsoft.com/library/windows/apps/dn934798) 元素。 選取 **\[應用程式\]** 索引標籤 (如果尚未選取)，然後選取 **\[支援的旋轉\]** 下的一或多個核取方塊，以記錄您的喜好設定。

不過，我們鼓勵您將 UWP app 的 UI 設計成不論什麼裝置方向和螢幕大小，都能賞心悅目。 在下下一個主題[針對尺寸與使用者體驗移植](wpsl-to-uwp-form-factors-and-ux.md)中，會有更多的相關資訊。

下一個主題是[移植商務與資料層](wpsl-to-uwp-business-and-data.md)。

