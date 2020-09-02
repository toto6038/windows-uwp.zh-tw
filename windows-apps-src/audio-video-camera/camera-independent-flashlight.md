---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: 本文章說明如何存取和使用裝置的燈光 (如果有的話)。 燈光功能分別從裝置的相機和閃光燈功能來管理。
title: 相機獨立閃光燈
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 281cede94ee587cc86509a9f32ed34857a5ae620
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362921"
---
# <a name="camera-independent-flashlight"></a>相機獨立閃光燈



本文章說明如何存取和使用裝置的燈光 (如果有的話)。 燈光功能分別從裝置的相機和閃光燈功能來管理。 除了取得燈光的參考及調整其設定以外，本文也說明如何在不使用燈光時正確地釋出燈光資源，以及如何偵測燈光的可用性何時變更以免另一個 App 正在使用它。

## <a name="get-the-devices-default-lamp"></a>取得裝置的預設燈光

若要取得裝置的預設燈光裝置，請呼叫 [**Lamp.GetDefaultAsync**](/uwp/api/windows.devices.lights.lamp.getdefaultasync)。 在 [**Windows.Devices.Lights**](/uwp/api/Windows.Devices.Lights) 命名空間中可找到燈光 API。 請務必先為此命名空間新增 using 指示詞，再嘗試存取這些 API。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLightsNamespace":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetDeclareLamp":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetGetDefaultLamp":::

如果傳回的物件是 **null**，則裝置不支援 **Lamp** API。 即使裝置上有實際配備燈光，但有些裝置可能不支援 **Lamp** API。

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>取得使用燈光選取器字串的特定燈光

有些裝置可能有一個以上的燈光。 若要取得裝置上可用燈光的清單，請呼叫 [**GetDeviceSelector**](/uwp/api/windows.devices.lights.lamp.getdeviceselector) 以取得裝置選取器字串。 此選取器字串可接著傳遞到 [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 中。 這個方法用來列舉許多不同種類的裝置，而選取器字串可讓方法知道只要傳回燈光裝置。 從 **FindAllAsync** 傳回的 [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 物件是 [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件的集合，代表裝置上可用的燈光。 選取清單中的其中一個物件，然後將 [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 屬性傳遞至 [**Lamp.FromIdAsync**](/uwp/api/windows.devices.lights.lamp.fromidasync) 以取得對要求之燈光的參考。 這個範例使用來自 **System.Linq** 命名空間的 **GetFirstOrDefault** 延伸方法來選取 **DeviceInformation** 物件，其中 [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) 屬性的值為 **Back**，該值會選取裝置機殼背面的燈光 (如果有的話)。

請注意，在 [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) 命名空間中可找到 [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) API。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetGetLampWithSelectionString":::

## <a name="adjust-lamp-settings"></a>調整燈光設定

擁有 [**Lamp**](/uwp/api/Windows.Devices.Lights.Lamp) 類別的執行個體後，請將 [**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled) 屬性設定為 **true** 以開啟燈光。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsOn":::

將 [**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled) 屬性設定為 **false**，以關閉燈光。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsOff":::

部分裝置具有支援色彩值的燈光。 檢查 [**IsColorSettable**](/uwp/api/windows.devices.lights.lamp.iscolorsettable) 屬性以查看燈光是否支援色彩。 如果此值為 **true**，您可以使用 [**Color**](/uwp/api/windows.devices.lights.lamp.color) 屬性設定燈光的色彩。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsColor":::

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>登錄以在燈光可用性變更時收到通知

燈光存取權會授予最新的 App 以要求存取權。 因此，如果另一個 App 已啟動並要求您的 App 目前所使用的燈光資源，則在其他 App 釋出資源前，您的 App 將無法再控制燈光。 若要在燈光的可用性變更時收到通知，請登錄 [**Lamp.AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged) 事件的處理常式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetAvailabilityChanged":::

在此事件的處理常式中，檢查 [**LampAvailabilityChanged.IsAvailable**](/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable) 屬性，以判斷燈光是否可用。 在此範例中，用於開啟和關閉燈光的切換開關會根據燈光可用性來啟用或停用。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetAvailabilityChangedHandler":::

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>不使用燈光資源時的適當處置

當您不再使用燈光時，您應該將它停用並呼叫 [**Lamp.Close**](/uwp/api/windows.devices.lights.lamp.close) 以釋出資源，讓其他 App 可存取此燈光。 如果您使用 C#，此屬性會對應至 **Dispose** 方法。 如果您已登錄 [**AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged)，您應在處置燈光資源時取消登錄此處理常式。 您的程式碼中處置燈光資源的適當位置取決於您的 App。 若要將燈光存取的範圍限制為單一頁面，請釋出 [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) 事件中的資源。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetDisposeLamp":::

## <a name="related-topics"></a>相關主題
- [媒體播放](media-playback.md)

 
