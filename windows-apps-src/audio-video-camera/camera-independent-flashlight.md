---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
本文章說明如何存取和使用裝置的燈光 (如果有的話)。 燈光功能分別從裝置的相機和閃燈功能進行管理。
相機獨立閃光燈
---

# 相機獨立閃光燈

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文章說明如何存取和使用裝置的燈光 (如果有的話)。 燈光功能分別從裝置的相機和閃燈功能進行管理。 除了取得燈光的參考及調整其設定以外，本文也說明如何在不使用燈光時正確地釋出燈光資源，以及如何偵測燈光的可用性何時變更以免另一個 App 正在使用它。

## 取得裝置的預設燈光

若要取得裝置的預設燈光裝置，請呼叫 [**Lamp.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/dn894327)。 在 [**Windows.Devices.Lights**](https://msdn.microsoft.com/library/windows/apps/dn894331) 命名空間中可找到燈光 API。 請務必先為此命名空間新增 using 指示詞，再存取這些 API。

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

如果傳回的物件是 **null**，則裝置不支援 **Lamp** API。 即使裝置上實際有燈光出現，但有些裝置可能不支援 **Lamp** API。

## 取得使用燈光選取器字串的特定燈光

有些裝置可能有一個以上的燈光。 若要取得裝置上可用的燈光清單，請呼叫 [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894328) 以取得裝置選取器字串。 此裝置選取器字串可接著傳遞到 [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)中。 這個方法用來列舉許多不同種類的裝置，而選取器字串可讓方法知道只要傳回燈光裝置。 從 **FindAllAsync** 傳回的 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 物件是 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 物件的集合，代表裝置上可用的燈光。 選取清單中的其中一個物件，然後將 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 屬性傳遞至 [**Lamp.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894326) 以取得所要求燈光的參考。 這個範例使用來自 **System.Linq** 命名空間的 **GetFirstOrDefault** 延伸方法來選取 **DeviceInformation** 物件，其中 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 屬性的值為 **Back**，該值會選取裝置機殼背面的燈光 (如果有的話)。

請注意，在 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空間中可找到 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) API。

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## 調整燈光設定

一旦擁有 [**Lamp**](https://msdn.microsoft.com/library/windows/apps/dn894310) 類別的執行個體，請將 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) 屬性設定為 **true**，以開啟燈光。

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

將 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) 屬性設定為 **false**，以關閉燈光。

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

部分裝置具有支援色彩值的燈光。 檢查 [**IsColorSettable**](https://msdn.microsoft.com/library/windows/apps/dn894329) 屬性以查看燈光是否支援色彩。 如果此值為 **true**，您可以使用 [**Color**](https://msdn.microsoft.com/library/windows/apps/dn894322) 屬性設定燈光的色彩。

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## 登錄在燈光可用性變更時收到通知

燈光存取權會授予最新的 App 以要求存取權。 因此，如果另一個 App 已啟動並要求您的 App 目前所使用的燈光資源，則在其他 App 釋出資源前，您的 App 將無法再控制燈光。 若要在燈光的可用性變更時收到通知，請登錄 [**Lamp.AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317) 事件的處理常式。

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

在此事件的處理常式中，檢查 [**LampAvailabilityChanged.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/dn894315) 屬性，以判斷燈光是否可用。 在此範例中，用於開啟和關閉燈光的切換開關會根據燈光可用性來啟用或停用。

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## 不使用燈光資源時的適當處置

當您不再使用燈光時，您應該將它停用並呼叫 [**Lamp.Close**](https://msdn.microsoft.com/library/windows/apps/dn894320) 以釋出資源，讓其他 App 可存取此燈光。 如果您使用 C#，此屬性會對應至 **Dispose** 方法。 如果您已登錄 [**AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317)，您應在處置燈光資源時取消登錄此處理常式。 您的程式碼中處置燈光資源的適當位置取決於您的 App。 若要將燈光存取的範圍限制為單一頁面，請釋出 [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509) 事件中的資源。

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

 

 






<!--HONumber=Mar16_HO1-->


