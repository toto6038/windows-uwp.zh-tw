---
author: muhsinking
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: 列舉裝置
description: 列舉命名空間可讓您尋找內部連接到系統、外部連接，或者可透過無線或網路通訊協定偵測到的裝置。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: df6082665136442c03273dea4132417b0fd7033c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975168"
---
# <a name="enumerate-devices"></a>列舉裝置


## <a name="samples"></a>範例

列舉所有可用裝置最簡單的方法是使用 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 命令拍攝快照 (會在後面的章節中近一步說明)。

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

若要下載示範 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 更進階使用方式的範例，可按一下[這裡](http://go.microsoft.com/fwlink/?LinkID=620536)。

## <a name="enumeration-apis"></a>列舉 API

列舉命名空間可讓您尋找內部連接到系統、外部連接，或者可透過無線或網路通訊協定偵測到的裝置。 您用來列舉可能裝置的 API 是 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 命名空間。 使用這些 API 的部份原因如下。

-   尋找要與您的 app 連線的裝置。
-   取得與系統連線或可由系統搜索的裝置相關資訊。
-   當新增裝置、連線、中斷連線、變更線上狀態或變更其他屬性時，讓 app 可以收到通知。
-   當裝置連接、中斷連接、變更線上狀態或變更其他屬性時，讓 app 可以收到背景觸發程序。

這些 API 可透過下列任何通訊協定和匯流排來列舉裝置，前提是執行 app 的個別裝置和系統必須支援該技術。 這不是完整清單，而且特定裝置可能支援其他通訊協定。

-   實際連線的匯流排。 這包含 PCI 和 USB。 例如，您可以在[**裝置管理員**] 中看到的任何項目。
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   數位生活網路聯盟 (DLNA)
-   [**探索並啟動 (DIAL)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**DNS 服務探索 (DNS-SD)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [裝置上的 Web 服務 (WSD)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [藍牙](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**Wi-Fi Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**服務點**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

在許多情況下，您並不需要擔心列舉 API 的使用。 這是因為許多使用裝置的 API 會自動選取適當的預設裝置，或提供更順暢的列舉 API。 例如，[**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) 會自動使用預設的音訊轉譯器裝置。 只要您的 app 可以使用預設的裝置，就沒有必要在應用程式中使用列舉 API。 列舉 API 提供一般的彈性方式，可讓您探索並連接到可用的裝置。 本主題提供列舉裝置的相關資訊，並說明四個列舉裝置的常用方式。

-   使用 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) UI
-   列舉系統目前可探索的裝置快照
-   列舉目前可探索且監看變更的裝置
-   列舉背景工作中目前可探索且監看變更的裝置

## <a name="deviceinformation-objects"></a>DeviceInformation 物件


使用列舉 API，您會經常需要使用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件。 這些物件包含大部分關於裝置的可用資訊。 下表說明您會感興趣的一些 **DeviceInformation** 屬性。 如需完整清單，請參閱 **DeviceInformation** 的參考頁面。

| 屬性                         | 註解                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | 這是裝置的唯一識別碼，並以字串變數的方式提供。 在大部分情況下，這是一個您會從一種方法傳遞到另一種方法，來指出您感興趣之特定裝置的不透明值。 您也可以在關閉並重新開啟您的 app 之後使用這個屬性和 **DeviceInformation.Kind** 屬性。 這將能確保您可以復原並重複使用相同的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件。 |
| **DeviceInformation.Kind**       | 這表示 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件所代表的裝置物件類型。 這不是裝置類別或裝置類型。 單一裝置可由數個不同類型的不同 **DeviceInformation** 物件來呈現。 這個屬性的可能值及其彼此關聯的方式都列於 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 中。                           |
| **DeviceInformation.Properties** | 這個屬性包涵蓋針對 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件要求的資訊。 您可以輕鬆地將最常見的屬性當成 **DeviceInformation** 物件的屬性進行參照，例如 [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name)。 如需詳細資訊，請參閱[裝置資訊屬性](device-information-properties.md)。                                                                |

 

## <a name="devicepicker-ui"></a>DevicePicker UI


[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 是 Windows 所提供的控制項，可用來建立讓使用者能夠從清單中選取裝置的小型 UI。 您可以透過幾種方式來自訂 [**DevicePicker**] 視窗。

-   您可以控制顯示在 UI 中的裝置，方法是將 [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors.aspx)、[**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses.aspx)，或兩者新增至 [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter)。 在大部分情況下，您只需要新增一個選取器或類別，但如果您需要不止一個，您可以新增多個選取器或類別。 如果您新增多個選取器或類別，它們會使用 OR 邏輯函式結合在一起。
-   您可以指定想要為裝置擷取的屬性。 您可以藉由將屬性新增至 [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties) 來執行此操作。
-   您可以使用 [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance) 來變更 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 的外觀。
-   您可以指定 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 顯示時的大小和位置。

顯示 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 時，UI 的內容會在新增、移除或更新裝置時自動更新。

**注意：** 不能指定使用[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 。 如果您想要擁有具備特定 **DeviceInformationKind** 的裝置，就需要建置 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 並提供自己的 UI。

 

如果您想要使用傳播媒體內容和 DIAL，它們也會各自提供自己的選擇器。 這些選擇器分別是 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) 和 [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783)。

## <a name="enumerate-a-snapshot-of-devices"></a>列舉裝置的快照


在某些情況下，[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 不會適合您的需求，而您需要更多彈性。 或許您想要建置自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置。 在這些情況下，您可以列舉裝置的快照。 這包含仔細尋找目前已與系統連接或配對的裝置。 不過，您需要注意這個方法只會查看可使用的裝置快照，所以您找不到在列舉清單之後連接的裝置。 如果裝置已更新或移除，您也不會收到通知。 另一個需要注意的潛在缺點是，這個方法在整個列舉完成之前，將不會顯示任何結果。 基於這個原因，當您對 **AssociationEndpoint**、**AssociationEndpointContainer** 或 **AssociationEndpointService** 物件感興趣時，便不應該使用這個方法，因為它們是透過網路或無線通訊協定找到的物件。 這最多可能需要 30 秒才能完成。 在這個案例中，您應該使用 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 物件來列舉可能的裝置。

若要列舉裝置的快照，您可以使用 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 方法。 這個方法會等待，直到整個列舉處理程序完成，並以一個 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx) 物件形式傳回所有結果為止。 這個方法也同時會多載以提供您數個選項，讓您可以篩選結果並將結果限制在您感興趣的裝置。 您可以藉由提供 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) 或傳入裝置選取器來執行這個動作。 裝置選取器是一個 AQS 字串，可指定您想要列舉的裝置。 如需詳細資訊，請參閱[建置裝置選取器](build-a-device-selector.md)。

以下提供裝置列舉快照的範例：



除了限制結果之外，您也可以指定想要為裝置擷取的屬性。 如果這樣做，指定的屬性就能在集合中傳回的每個 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件的屬性包中使用。 請務必注意，並非所有屬性都可供所有裝置類型使用。 若要查看哪些屬性可供哪些裝置類型使用，請參閱[裝置資訊屬性](device-information-properties.md)。



## <a name="enumerate-and-watch-devices"></a>列舉並監看裝置


有一個可用來列舉裝置、功能更強大且彈性的方法是建立 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)。 這個選項可在您列舉裝置時提供最大的彈性。 它讓您能夠列舉目前存在的裝置，也可以在新增或移除符合您裝置選取器的裝置，或進行該裝置的屬性變更時接收通知。 當您建立 **DeviceWatcher** 時，您將會提供裝置選取器。 如需裝置選取器的詳細資訊，請參閱[建置裝置選取器](build-a-device-selector.md)。 建立監看員之後，您將會收到下列針對所有符合您所提供準則之裝置的通知。

-   在加入新裝置時的新增通知。
-   在您感興趣的屬性變更時的更新通知。
-   當裝置不再可用或不再符合您的篩選時的移除通知。

在使用 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 的多數情況下，您會維護一份裝置清單，並在您的監看員收到您正在監看之裝置的更新時，新增、移除或更新清單中的項目。 當您收到更新通知時，更新的資訊將會以 [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationupdate.aspx) 物件供使用。 若要更新您的裝置清單，請先尋找已變更的適當 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)。 然後呼叫該物件的 [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update) 方法，提供 **DeviceInformationUpdate** 物件。 這個便利的函式將會自動更新您的 **DeviceInformation** 物件。

由於 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 會在裝置連接及變更時傳送通知，您應該在對 **AssociationEndpoint**、**AssociationEndpointContainer** 或 **AssociationEndpointService** 物件感興趣時使用這個列舉裝置的方法，因為它們會透過網路或無線通訊協定加以列舉。

若要建立 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)，請使用其中一種 [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx) 方法。 這些方法已多載以讓您指定感興趣的裝置。 您可以藉由提供 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) 或傳入裝置選取器來執行這個動作。 裝置選取器是一個 AQS 字串，可指定您想要列舉的裝置。 如需詳細資訊，請參閱[建置裝置選取器](build-a-device-selector.md)。 您也可以指定想要為裝置擷取以及感興趣的屬性。 如果這樣做，指定的屬性就能在集合中傳回的每個 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件的屬性包中使用。 請務必注意，並非所有屬性都可供所有裝置類型使用。 若要查看哪些屬性可供哪些裝置類型使用，請參閱[裝置資訊屬性](device-information-properties.md)

## <a name="watch-devices-as-a-background-task"></a>以背景工作的方式監看裝置


以背景工作的方式監看裝置很類似上述建立 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 的方式。 事實上，您仍然需要先建立如上節所述的標準 **DeviceWatcher** 物件。 一旦建立該物件之後，您會呼叫 [**GetBackgroundTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx)，而不是 [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start)。 當您呼叫 **GetBackgroundTrigger** 時，必須指定您所感興趣的通知：新增、移除或更新。 您無法在沒有要求新增的情況下要求更新或移除。 一旦登錄觸發程序之後，**DeviceWatcher** 就會立即開始在背景中執行。 自此之後，每當收到適用於您應用程式且符合您準則的新通知時，將會觸發背景工作，並提供自從上次觸發您的應用程式之後所做的最新變更。

**重要** [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)觸發您的應用程式第一次便是在監看員達到**EnumerationCompleted**狀態。 這表示它將包含所有初始結果。 當它在未來的任何時候觸發您的應用程式時，將只包含自從上次觸發之後所發生的新增、更新及移除通知。 這與前景 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 物件有些微差異，因為初始結果不會一次傳入一個，並只會在達到 **EnumerationCompleted** 之後以套件組合形式傳遞。

 

某些無線通訊協定在背景進行掃描時，可能會和在前景進行掃描時擁有不一樣的行為。它們也有可能完全不支援在背景中掃描。 有三種與背景掃瞄相關的可能性。 下表列出這些可能性，以及這可能會對您的應用程式產生的效果。 例如，藍牙和 Wi-Fi Direct 不支援背景掃描，因而可推測出它們不支援 [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)。

| 行為                                  | 影響                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| 背景中的相同行為               | 無                                                                                                                                    |
| 可能在背景中的唯一被動掃描 | 裝置在等待被動掃瞄執行時，可能需要較長的時間來探索。                                                           |
| 不支援背景掃描            | [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) 將不會偵測到任何裝置，並且將不會報告任何更新。 |

 

如果您的 [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) 包含不支援以背景工作方式進行掃瞄的通訊協定，則觸發程序仍可運作。 不過，您將無法透過該通訊協定取得任何更新或結果。 通常仍會偵測到其他通訊協定或裝置的更新。

## <a name="using-deviceinformationkind"></a>使用 DeviceInformationKind


在大部分情況下，您不需要擔心 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)。 這是因為您正在使用之裝置 API 所傳回的裝置選取器，通常能保證您會取得可與其 API 搭配使用的正確裝置物件類型。 不過，某些情況下，雖然您想要取得裝置的 **DeviceInformation**，卻沒有對應的裝置 API 可提供裝置選取器。 在這些情況下，您便需要建置自己的選取器。 例如，裝置上的 Web 服務沒有專用的 API，但您可以探索這些裝置，並使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 來取得相關資訊，然後利用通訊端 API 來使用它們。

如果您正在建置自己的裝置選取器來列舉裝置物件，請務必了解 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)。 如需所有可能的類型及其彼此關聯的方式，請參閱 **DeviceInformationKind** 參考頁面中的說明。 **DeviceInformationKind** 最常見的用法之一，便是指定您在搭配裝置選取器送出查詢時正在搜尋的裝置類型。 如此可確保您只會列舉符合所提供之 **DeviceInformationKind** 的裝置。 例如，您可以尋找 **DeviceInterface** 物件，然後執行查詢來取得父項 **Device** 物件的資訊。 該父項物件可能包含其他資訊。

需要特別注意的是，[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件屬性包中的可用屬性會根據裝置的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 而不同。 某些屬性僅供特定類型使用。 如需哪些屬性可供哪些類型使用的詳細資訊，請參閱[裝置資訊屬性](device-information-properties.md)。 因此，在上述範例中，搜尋父項 **Device** 將讓您能夠存取更多無法從 **DeviceInterface** 裝置物件取得的資訊。 因此，當您建立 AQS 篩選字串時，請務必確認所要求的屬性適用於您正在列舉的 **DeviceInformationKind** 物件。 如需建置篩選器的詳細資訊，請參閱[建置裝置選取器](build-a-device-selector.md)。

列舉 **AssociationEndpoint**、**AssociationEndpointContainer** 或 **AssociationEndpointService** 物件時，您是透過網路或網路通訊協定來列舉。 在這些情況下，建議您不要使用 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx)，並改用 [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx)。 這是因為在網路上搜尋，通常會造成 [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) 產生之前無法逾時達 10 秒 (或以上) 的搜尋操作。 **FindAllAsync** 不會完成其操作，直到觸發 **EnumerationCompleted** 為止。 如果您正在使用 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)，不論何時呼叫 **EnumerationCompleted**，您都會取得接近即時的結果。

## <a name="save-a-device-for-later-use"></a>儲存裝置以供稍後使用


任何 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件皆是由下列兩個資訊的組合來唯一識別：[**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 和 [**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)。 如果您保留這兩個資訊，當遺失 **DeviceInformation** 物件時，就可以將此資訊提供給 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/br225425.aspx) 來重新建立該物件。 如果您這樣做，就可以儲存與您 app 整合之裝置的使用者喜好設定。


 

 




