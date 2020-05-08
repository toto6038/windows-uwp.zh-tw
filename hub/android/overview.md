---
title: Windows 上的 Android 開發概觀
description: 協助您開始在 Windows 上進行 Android 開發的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android on windows，xamarin，回應 native，cordova，ionic，phonegap，c + + android 遊戲，windows defender，模擬器
ms.date: 04/28/2020
ms.openlocfilehash: d43420f442fd5dfcb2b885fb0369964a113e9bac
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255242"
---
# <a name="overview-of-android-development-on-windows"></a>Windows 上的 Android 開發概觀

有多個路徑可用於使用 Windows 作業系統開發 Android 裝置應用程式。 這些路徑分為三種主要類型：**[原生 Android 開發](#native-android)**、**[跨平臺開發](#cross-platform)** 和**[Android 遊戲開發](#game-development)**。 本總覽將協助您決定開發 Android 應用程式所需遵循的開發路徑，然後提供[後續步驟](#next-steps)，協助您開始使用 Windows 來進行開發：

- [原生 Android](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova、Ionic 或 PhoneGap](pwa.md)
- [適用于遊戲開發的 c/c + +](native-android.md#use-c-or-c-for-android-game-development)

此外，本指南將提供使用 Windows 來執行下列動作的秘訣：

- [在 Android 裝置或模擬器上進行測試](emulator.md)
- [更新 Windows Defender 設定以來改善效能](defender-settings.md)
- [開發適用于 Android 的雙畫面應用程式，並取得 Surface 雙核裝置 SDK](https://docs.microsoft.com/dual-screen/android/)

## <a name="native-android"></a>原生 Android

[Windows 上的原生 Android 開發](./native-android.md)表示您的應用程式僅以 Android （而非 IOS 或 Windows 裝置）為目標。 您可以使用[Android Studio](https://developer.android.com/studio/install#windows)或[Visual Studio](https://visualstudio.microsoft.com/vs/android/) ，在專為 Android 作業系統設計的生態系統內進行開發。 效能將針對 Android 裝置優化，使用者介面外觀與風格會與裝置上的其他原生應用程式一致，而使用者裝置的任何功能或功能將會直接存取和使用。 以原生格式開發您的應用程式，可協助它只使用「感受 right」，因為它會遵循專為 Android 裝置所建立的所有互動模式和使用者經驗標準。

## <a name="cross-platform"></a>跨平台

跨平臺架構提供單一程式碼基底，可以（大部分）在 Android、iOS 和 Windows 裝置之間共用。 使用跨平臺架構，可協助您的應用程式在裝置平臺上維持相同的外觀、操作和體驗，以及從自動推出更新和修正程式中獲益。 應用程式不需要瞭解各種裝置專屬程式碼語言，而是在共用程式碼基底中開發，通常是以一種語言撰寫。

雖然跨平臺架構的目標是要盡可能接近原生應用程式，但它們絕對不會與原本開發的應用程式緊密整合，而且可能會降低速度和效能降低。 此外，用來建立跨平臺應用程式的工具可能不會有每個不同裝置平臺所提供的所有功能，可能需要因應措施。

基本程式通常是由 UI 程式**代碼**所組成，用於建立使用者介面（例如頁面、按鈕控制項、標籤、清單等），以及**邏輯程式碼**，用於呼叫 web 服務、存取資料庫、叫用硬體功能及管理狀態。 平均來說，90% 可以重複使用，不過通常需要針對每個裝置平臺自訂程式碼。 此一般化主要取決於您所建立的應用程式類型，但會提供一些內容，希望能夠協助您進行決策。  

## <a name="choosing-a-cross-platform-framework"></a>選擇跨平臺架構

[Xamarin Native （Xamarin）](xamarin-android.md)

- UI 程式碼：具有 Android Designer 的 XML 和材質主題
- 邏輯程式碼： c # 或 F#
- 仍然能夠使用一些原生 Android 元素，但適用于其他平臺（iOS、Windows）的程式碼基底。
- 只有邏輯程式碼會在平臺之間共用，而不是 UI 程式碼。
- 適用于具有裝置特定使用者介面的更複雜應用程式。

[Xamarin 表單（Xamarin）](xamarin-forms.md)

- UI 程式碼： XAML 和 .NET （含 Visual Studio）
- 邏輯程式碼： C#
- 在 Android、iOS 和 Windows 裝置應用程式之間，共用大約 60-90% 的邏輯和 UI 程式碼。 
- 使用常見的使用者控制項，例如按鈕、標籤、專案、ListView、StackLayout、行事曆、TabbedPage 等。建立按鈕和 Xamarin 表單將會瞭解如何使用系結程式庫從 c # 呼叫 JAVA 或 Swift 程式碼，以呼叫每個平臺的原生按鈕。
- 適用于簡單的應用程式，例如內部或企業營運（LOB）應用程式、原型或 Mvp。 任何可以看起來有點標準或一般的應用程式，都是利用簡單的使用者介面。

[React Native](react-native.md)

- UI 程式碼： JavaScript
- 邏輯程式碼： JavaScript
- React Native 的目標並不是撰寫程式碼一次，而是在任何平臺上執行，而是要學習一次（以回應方式）並隨時隨地寫入。
- 此社區已新增諸如博覽會等工具，並建立 React Native 應用程式，以協助不使用 Xcode 或 Android Studio 來建立應用程式的人員。
- 類似于 Xamarin （c #），React Native （JavaScript）會呼叫原生 UI 元素（而不需要撰寫 JAVA/Kotlin 或 Swift）。

[漸進式 Web 應用程式 (PWA)](pwa.md)

- UI 程式碼： HTML、CSS、JavaScript
- 邏輯程式碼： JavaScript
- Pwa 是以標準模式建立的 web 應用程式，可讓他們利用 web 和原生應用程式功能。 您可以在不使用架構的情況下建立它們，但是有幾個熱門的架構要考慮[Ionic](https://ionicframework.com/docs/intro)和[PhoneGap](https://phonegap.com/about/)。
- Pwa 可以安裝在裝置上（Android、iOS 或 Windows），而且會因為合併服務背景工作而離線工作。
- Pwa 可以在沒有使用 web URL 的應用程式存放區的情況下散發和安裝。 Microsoft Store 和 Google Play 商店允許列出 Pwa，Apple Store 目前不存在，但仍可安裝在任何執行12.2 或更新版本的 iOS 裝置上。
- 若要深入瞭解，請參閱此 Pwa on MDN[簡介](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction)。

## <a name="game-development"></a>遊戲開發

Android 遊戲開發通常與開發標準 Android 應用程式不同，因為遊戲通常會使用自訂轉譯邏輯，通常是以 OpenGL 或 Vulkan 的方式撰寫。 基於這個理由，以及因為支援遊戲開發的眾多 C 程式庫而提供，開發人員通常會搭配使用[C/c + + 與 Visual Studio](https://docs.microsoft.com/cpp/cross-platform/?view=vs-2019)以及 Android[原生開發工具組（NDK）](https://docs.microsoft.com/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)來建立 android 遊戲。 [開始使用 c/c + + 進行遊戲開發](native-android.md#use-c-or-c-for-android-game-development)。

開發 Android 遊戲的另一個常見路徑是使用遊戲引擎。 有許多免費的開放原始碼引擎可供使用，例如[具有 Visual Studio 的 Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019)、 [Unreal 引擎](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html)、[使用 xamarin 的 MonoGame](https://docs.microsoft.com/xamarin/graphics-games/monogame/introduction/)、搭配 xamarin 的 UrhoSharp、[使用](https://docs.microsoft.com/xamarin/graphics-games/urhosharp/introduction)Xamarin [SkiaSharp](https://docs.microsoft.com/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) 、COCOONJS、應用程式遊戲套件、融合、Corona SDK、科科斯及更多功能。

## <a name="next-steps"></a>後續步驟

- [Windows 上的原生 Android 開發入門](native-android.md)
- [開始使用 Xamarin 進行 Android 開發](xamarin-android.md)
- [開始使用 Xamarin 進行 Android 開發](xamarin-forms.md)
- [使用 React Native 開始針對 Android 進行開發](react-native.md)
- [開始開發適用于 Android 的 PWA](pwa.md)
- [開發適用于 Android 的雙畫面應用程式，並取得 Surface 雙核裝置 SDK](https://docs.microsoft.com/dual-screen/android/)
- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)
- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)
