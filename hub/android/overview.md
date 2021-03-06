---
title: Windows 上的 Android 開發
description: 協助您在 Windows 上開始開發 Android 的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows 上的 android、xamarin、回應原生、cordova、ionic、phonegap、c + + android 遊戲、windows defender、模擬器
ms.date: 04/28/2020
ms.openlocfilehash: 315c32eb6219deafe2817ba8df36b4f9d1cb1dae
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254291"
---
# <a name="overview-of-android-development-on-windows"></a>Windows 上的 Android 開發概觀

使用 Windows 作業系統開發 Android 裝置應用程式有多個路徑。 這些路徑分為三種主要類型： **[原生 Android 開發](#native-android)**、 **[跨平臺開發](#cross-platform)** 和 **[Android 遊戲開發](#game-development)**。 本總覽將協助您決定開發 Android 應用程式時所遵循的開發路徑，然後提供 [後續步驟](#next-steps) 來協助您開始使用 Windows 進行開發：

- [原生 Android](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md) \(英文\)
- [Cordova、Ionic 或 PhoneGap](pwa.md)
- [適用于遊戲開發的 c/c + +](native-android.md#use-c-or-c-for-android-game-development)

此外，本指南也會提供使用 Windows 的秘訣：

- [在 Android 裝置或模擬器上進行測試](emulator.md)
- [更新 Windows Defender 設定以來改善效能](defender-settings.md)
- [開發適用于 Android 的雙螢幕應用程式，並取得 Surface 雙核裝置 SDK](/dual-screen/android/)

## <a name="native-android"></a>原生 Android

[Windows 上的原生 Android 開發](./native-android.md) 表示您的應用程式僅以 Android 為目標， (不是 IOS 或 Windows 裝置) 。 您可以使用 [Android Studio](https://developer.android.com/studio/install#windows) 或 [Visual Studio](https://visualstudio.microsoft.com/vs/android/) ，在專為 Android 作業系統設計的生態系統內進行開發。 效能將會針對 Android 裝置優化，使用者介面的外觀和操作將會與裝置上的其他原生應用程式一致，而使用者裝置的任何功能或功能將會相當簡單，可供存取及使用。 以原生格式開發您的應用程式，可協助它只「感覺正確」，因為它會遵循專為 Android 裝置所建立的所有互動模式和使用者經驗標準。

## <a name="cross-platform"></a>跨平台

跨平臺架構提供單一程式碼基底，在 Android、iOS 和 Windows 裝置之間可 (大部分) 共用。 使用跨平臺架構可協助您的應用程式跨裝置平臺維護相同的外觀、操作和體驗，以及從自動推出的更新和修正中獲益。 應用程式是在共用程式碼基底中開發的，而不需要瞭解各種裝置特定的程式碼語言，通常是以一種語言來開發。

雖然跨平臺架構的外觀和感覺盡可能接近原生應用程式，但它們永遠不會與原生開發的應用程式緊密整合，而且可能會降低速度和效能降低。 此外，用來建立跨平臺應用程式的工具可能不會有每個不同裝置平臺所提供的所有功能，可能需要因應措施。

程式碼基底通常是由 **UI 程式碼** 所組成，用來建立使用者介面，例如頁面、按鈕控制項、標籤、清單等，以及用來呼叫 web 服務、存取資料庫、叫用硬體功能和管理狀態的 **邏輯程式碼**。 平均來說，90% 的可重複使用，不過通常需要針對每個裝置平臺自訂程式碼。 這項一般化主要取決於您所建立的應用程式類型，但提供了一些希望能協助您進行決策的內容。  

## <a name="choosing-a-cross-platform-framework"></a>選擇跨平臺架構

[Xamarin Native (Xamarin. Android) ](xamarin-android.md)

- UI 程式碼：具有 Android 設計工具的 XML 和材質主題
- 邏輯程式碼： c # 或 F#
- 仍能利用一些原生的 Android 元素，但適用于其他平臺 (iOS、Windows) 的程式碼基底。
- 只有邏輯程式碼會跨平臺共用，而不是 UI 程式碼。
- 適用于具有裝置特定使用者介面的更複雜應用程式。

[Xamarin 表單 (Xamarin) ](xamarin-forms.md)

- UI 程式碼：使用 Visual Studio) 的 XAML 和 .NET (
- 邏輯程式碼： C#
- 在 Android、iOS 和 Windows 裝置應用程式之間共用大約60–90% 的邏輯和 UI 程式碼。 
- 使用常用的使用者控制項，例如按鈕、標籤、專案、ListView、StackLayout、行事曆、TabbedPage 等等。建立按鈕和 Xamarin 表單，將會找出如何使用系結程式庫從 c # 呼叫 JAVA 或 Swift 程式碼，來呼叫每個平臺的原生按鈕。
- 適用于簡單的應用程式，例如內部或企業營運 (LOB) 應用程式、原型或 Mvp。 使用簡單的使用者介面，可以看起來有點標準或泛型的任何應用程式。

[React Native](react-native.md) \(英文\)

- UI 程式碼： JavaScript
- 邏輯程式碼： JavaScript
- 回應原生的目標不是撰寫程式碼一次，而是在任何平臺上執行，而是一次 () 和寫入的回應方式。
- 此社區新增了像是博覽會的工具，並建立回應原生應用程式，以協助在不使用 Xcode 或 Android Studio 的情況下建立應用程式。
- 類似于 Xamarin (c # ) ，回應原生 (JavaScript) 呼叫原生 UI 元素 (而不需要撰寫 JAVA/Kotlin 或 Swift) 。

[漸進式 Web 應用程式 (PWA)](pwa.md)

- UI 程式碼： HTML、CSS、JavaScript
- 邏輯程式碼： JavaScript
- Pwa 是以標準模式建立的 web 應用程式，可讓他們充分利用 web 和原生應用程式功能。 您可以在不使用架構的情況下建立它們，但要考慮的一些熱門架構是 [Ionic](https://ionicframework.com/docs/intro) 和 [PhoneGap](https://phonegap.com/about/)。
- Pwa 可以安裝在 (Android、iOS 或 Windows) 的裝置上，並可在不受服務工作者的合併下離線工作。
- Pwa 可在沒有使用 web URL 的應用程式存放區的情況下進行散發與安裝。 Microsoft Store 和 Google Play 商店允許列出 Pwa，Apple Store 目前沒有，不過仍可安裝在任何執行12.2 或更新版本的 iOS 裝置上。
- 若要深入瞭解，請參閱此 Pwa 的 MDN [簡介](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) 。

## <a name="game-development"></a>遊戲開發

Android 的遊戲開發通常是開發標準 Android 應用程式的唯一方式，因為遊戲通常會使用自訂轉譯邏輯，通常是以 OpenGL 或 Vulkan 撰寫。 基於這個理由，以及支援遊戲開發的許多 C 程式庫，開發人員通常會使用 [C/c + + 搭配 Visual Studio](/cpp/cross-platform/?view=vs-2019)，以及 Android [原生開發工具組 (NDK) ](/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)，以建立適用于 android 的遊戲。 [開始使用 C/c + + 進行遊戲開發](native-android.md#use-c-or-c-for-android-game-development)。

開發 Android 遊戲的另一個常見路徑是使用遊戲引擎。 有許多免費的開放原始碼引擎可供使用，例如 [Unity 與 Visual Studio](/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019)、 [Unreal Engine](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html)、 [MonoGame with xamarin](/xamarin/graphics-games/monogame/introduction/)、 [UrhoSharp with](/xamarin/graphics-games/urhosharp/introduction)Xamarin、 [SkiaSharp With xamarin](/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) COCOONJS、應用程式遊戲套件、融合、Corona SDK、科科2d 等等。

## <a name="next-steps"></a>下一步

- [開始在 Windows 上使用原生 Android 開發](native-android.md)
- [開始使用 Xamarin 進行 Android 開發](xamarin-android.md)
- [開始使用 Xamarin 進行 Android 開發](xamarin-forms.md)
- [開始使用回應原生開發 Android](react-native.md)
- [開始開發適用于 Android 的 PWA](pwa.md)
- [開發適用于 Android 的雙螢幕應用程式，並取得 Surface 雙核裝置 SDK](/dual-screen/android/)
- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)
- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)