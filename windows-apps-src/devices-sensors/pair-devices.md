---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: 配對裝置
description: 有些裝置在使用之前需要先進行配對。 Windows.Devices.Enumeration 命名空間支援三種不同方式來配對裝置。
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 85d42e69b376e2f3f455e44eb1dce3d41e890971
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258642"
---
# <a name="pair-devices"></a>配對裝置



**重要 API**

- [**Windows. 列舉**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

有些裝置在使用之前需要先進行配對。 [  **Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 命名空間支援三種不同方式來配對裝置。

-   自動配對
-   基本配對
-   自訂配對

**提示**  某些裝置不需要配對，就能使用。 此內容將涵蓋於＜自動配對＞一節中。

 

## <a name="automatic-pairing"></a>自動配對


有時您想要在應用程式中使用某個裝置，但不在乎該裝置是否已經配對。 您只是想要能夠使用與裝置相關的功能。 例如，如果您的 app 只想要擷取網路攝影機中的影像，您不一定會對裝置本身感興趣，只會對影像擷取感興趣。 如果有適用您感興趣之裝置的裝置 API 可供使用，這個案例就會落在自動配對的範圍內。

在此情況下，您只需使用與裝置相關聯的 API，在必要時進行呼叫，並信任系統來儲存任何可能需要的配對。 有些裝置不需要依序配對，您就能使用它們的功能。 如果裝置需要進行配對，則裝置 API 將會在幕後處理配對動作，讓您不需將該功能整合到 app。 您的 app 不需要知道任何有關指定的裝置是否需要配對的資訊，但您仍能存取該裝置並使用它的功能。

## <a name="basic-pairing"></a>基本配對


當您的應用程式使用 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 來嘗試進行裝置配對時，就稱為基本配對。 在這個案例中，您會讓 Windows 嘗試進行配對程序並加以處理。 如果需要任何使用者互動，將會由 Windows 處理。 如果您需要配對某個裝置，而沒有可嘗試進行自動配對的相關裝置 API，您就會使用基本配對。 您只是想要能夠使用該裝置，而需要先將它進行配對。

若要嘗試基本配對，您需要先針對感興趣的裝置取得 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件。 一旦接收到該物件之後，您將會與 [**DeviceInformation.Pairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 屬性進行互動，也就是 [**DeviceInformationPairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 物件。 若要嘗試進行配對，只需呼叫 [**DeviceInformationPairing.PairAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.pairasync)。 您將需要 **await** 結果，才能讓 app 有時間嘗試完成配對動作。 隨即會傳回配對動作的結果，只要沒有傳回任何錯誤，裝置就會進行配對。

如果使用的是基本配對，也可以存取其他有關裝置配對狀態的資訊。 例如，您了解配對狀態 ([**IsPaired**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired))，以及裝置是否可配對 ([**CanPair**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair))。 這兩者均為 [**DeviceInformationPairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 物件的屬性。 如果使用的是自動配對，除非取得相關的 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件，否則可能無法存取此資訊。

## <a name="custom-pairing"></a>自訂配對


自訂配對可讓您的 app 參與配對程序。 這可讓您的 app 指定針對配對程序支援的 [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds)。 您也需負責建立您自己的使用者介面，以視需要與使用者進行互動。 當您想要讓 app 對配對程序的進行方式有多一點的影響，或是顯示您自己的配對使用者介面時，請使用自訂配對。

若要實作自訂配對，您將需要針對感興趣的裝置取得 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件，就像在基本配對時所做的一樣。 不過，您感興趣的特定屬性是 [**DeviceInformation.Pairing.Custom**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.custom)。 這將為您提供 [**DeviceInformationCustomPairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcustompairing) 物件。 所有的 [**DeviceInformationCustomPairing.PairAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairasync) 方法需要您包含 [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) 參數。 這表示使用者需採取才能嘗試將裝置配對的動作。 如需不同類型以及使用者需採取動作的詳細資訊，請參閱 **DevicePairingKinds** 參考頁面。 就像基本配對一樣，您將需要 **await** 結果，才能讓 app 有時間嘗試完成配對動作。 隨即會傳回配對動作的結果，只要沒有傳回任何錯誤，裝置就會進行配對。

若要支援自訂配對，您需要針對 [**PairingRequested**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested) 事件建立一個處理常式。 這個處理常式必須確定會負責所有不同的 [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds)，這可能會在自訂配對案例中使用。 要採取的適當動作將取決於 **DevicePairingKinds**，這可提供來做為事件引數的一部分。

請務必注意，自訂配對一律是系統層級的操作。 基於這個原因，當您在桌面或 Windows Phone 上進行操作時，在配對即將發生時，一律會向使用者顯示系統對話方塊。 這是因為這兩個平台擁有的使用者體驗需要取得使用者同意。 由於對話方塊是自動產生的，因此在這些平台上進行操作時，您不需要在選擇 [ConfirmOnly**的**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds)DevicePairingKinds 時建立自己的對話方塊。 針對另一個 **DevicePairingKinds**，您將需要根據特定的 **DevicePairingKinds** 值來執行一些特殊處理。 請參閱範例，以取得如何針對不同的 **DevicePairingKinds** 值來處理自訂配對的範例。

從 Windows 10 版本1903開始，支援新的**DevicePairingKinds** ， **ProvidePasswordCredential**。 此值表示應用程式必須向使用者要求使用者名稱和密碼，才能向配對的裝置進行驗證。 若要處理這種情況，請呼叫**PairingRequested**事件處理常式之事件引數的[**AcceptWithPasswordCredential**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs.acceptwithpasswordcredential?branch=release-19h1#Windows_Devices_Enumeration_DevicePairingRequestedEventArgs_AcceptWithPasswordCredential_Windows_Security_Credentials_PasswordCredential_)方法，以接受配對。 傳入[**PasswordCredential**](https://docs.microsoft.com/uwp/api/windows.security.credentials.passwordcredential)物件，將使用者名稱和密碼封裝為參數。 請注意，遠端裝置的使用者名稱和密碼與本機登入使用者的認證不同，通常不會相同。

## <a name="unpairing"></a>取消配對


取消配對裝置只會與上述的基本或自訂配對案例有關。 如果使用的是自動配對，則您的 app 仍對裝置的配對狀態一無所知，因而不需將它取消配對。 如果選擇取消配對裝置，此程序與您是否實作基本或自訂配對完全相同。 這是因為不需要在取消配對過程中提供其他資訊或互動。

取消配對裝置的第一個步驟是針對想要取消配對的裝置取得 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件。 接著，需要擷取 [**DeviceInformation.Pairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 屬性並呼叫 [**DeviceInformationPairing.UnpairAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.unpairasync)。 就像配對一樣，您會想要 **await** 結果。 隨即會傳回取消配對動作的結果，只要沒有傳回任何錯誤，裝置就會進行取消配對。

## <a name="sample"></a>範例


若要下載示範如何使用 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 的範例，可按一下[這裡](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)。

 

 
