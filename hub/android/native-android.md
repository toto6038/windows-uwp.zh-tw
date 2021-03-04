---
title: Windows 上的原生 Android 開發
description: 如何在 Windows 上開始開發 Android 原生應用程式的逐步指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、android studio、visual studio、c + + android 遊戲、windows defender、模擬器、虛擬裝置、安裝、java、kotlin
ms.date: 04/28/2020
ms.openlocfilehash: c08aac8968ecc16ec548fadd4bee7d83b74ea719
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823182"
---
# <a name="get-started-with-native-android-development-on-windows"></a>開始在 Windows 上使用原生 Android 開發

本指南將協助您開始使用 Windows 建立原生 Android 應用程式。 如果您想要使用跨平臺解決方案，請參閱 [Windows 上的 Android 開發總覽](./overview.md) ，以取得一些選項的簡短摘要。

建立原生 Android 應用程式的最簡單方式是使用 Android Studio 搭配 [JAVA 或 Kotlin](#java-or-kotlin)，不過，如果您有特定用途，也可以 [使用 c 或 c + + 進行 android 開發](#use-c-or-c-for-android-game-development) 。 Android Studio SDK 工具會將您的程式碼、資料和資源檔編譯為封存的 Android 套件 apk 檔案。 其中一個 APK 檔案包含 Android 應用程式的所有內容，而且是 Android 裝置用來安裝應用程式的檔案。

## <a name="install-android-studio"></a>安裝 Android Studio

Android Studio 是 Google Android 作業系統的官方整合式開發環境。 [下載適用于 Windows 的 Android Studio 最新版本](https://developer.android.com/studio)。

- 如果您已下載 .exe 檔案 (建議的) ，請按兩下以啟動它。
- 如果您已下載 .zip 檔案，請將 ZIP 檔案解壓縮、將 android studio 資料夾複製到 [Program Files] 資料夾，然後開啟 android studio > bin 資料夾，然後啟動64位電腦的 studio64.exe () 或 studio.exe 32 位電腦 (的) 。

遵循 Android Studio 中的安裝程式，並安裝所建議的任何 SDK 套件。 當有新工具和其他 api 可供使用時，Android Studio 會透過快顯視窗通知您，或選取 [說明]   >  **檢查更新** 來檢查是否有更新。

## <a name="create-a-new-project"></a>建立新專案

選取 **[**  >  **新增** 新  >  **專案**]。

在 [ **選擇您的專案** ] 視窗中，您將能夠在這些範本之間進行選擇：

- **基本活動**：建立簡單的應用程式，其中包含應用程式行、浮動動作按鈕和兩個版面配置檔案：一個用於活動，另一個則用於分隔文字內容。

- **空白活動**：建立空白活動，以及包含範例文字內容的單一版面配置檔案。

- **下方導覽活動**：為活動建立標準底部導覽列。 請參閱 Google 的材質設計指導方針中的 [底部導覽元件](https://material.io/guidelines/components/bottom-navigation.html) 。

- 深入瞭解如何在 Android Studio 檔中 [選取活動範本](https://developer.android.com/studio/projects/templates#SelectTemplate) 。

範本通常用來將活動新增至新的和現有的應用程式模組。 例如，若要建立應用程式使用者的登入畫面，請使用 [Login 活動範本](https://developer.android.com/studio/projects/templates#LoginActivity)新增活動。

> [!NOTE]
> Android 作業系統是以 **元件** 的概念為基礎，並使用「 **活動** 」和「 **意圖** 」這些詞彙來定義互動。 **活動** 代表使用者可進行的單一焦點工作。 **活動** 會提供一個視窗，讓您使用以 **View** 類別為基礎的類別來建立使用者介面。 Android 作業系統的 **活動** 生命週期是由一組六個回呼所定義： `onCreate()` 、 `onStart()` 、 `onResume()` 、 `onPause()` 、 `onStop()` 和 `onDestroy()` 。 活動元件會使用 **意圖** 物件與另一個元件互動。 意圖會定義活動，以啟動或描述要執行的動作類型 (而且系統會為您選取適當的活動，這甚至可以來自不同的應用程式) 。 深入瞭解 Android 檔中的 [活動](https://developer.android.com/reference/android/app/Activity)、 [活動生命週期](https://developer.android.com/guide/components/activities/activity-lifecycle)和 [意圖](https://developer.android.com/reference/android/content/Intent.html) 。

### <a name="java-or-kotlin"></a>JAVA 或 Kotlin

**JAVA** 變成1991的語言，是由 Sun Microsystems 所開發，但現在是由 Oracle 所擁有。 它已成為最受歡迎且功能強大的程式設計語言之一，其中有全球最大的支援群體。 JAVA 是以類別為基礎且物件導向，其設計目的是要盡可能地有最少的實作相依性。 語法類似 C 和 c + +，但其低層級的設施比任何一個更低。

**Kotlin** 已先宣佈為新的開放原始碼語言，JetBrains 于2011中，並已在 Android Studio 中作為 JAVA 的替代方案，自2017起提供。 在5月2019日，Google 宣佈 Kotlin 是 Android 應用程式開發人員慣用的語言，因此即使是較新的語言，它也有強大的支援小組，並已識別為最快速成長的程式設計語言之一。 Kotlin 是跨平臺、靜態型別，而且是設計來與 JAVA 完全互通。

JAVA 較廣泛用於更廣泛的應用程式，並提供一些 Kotlin 沒有的功能，例如，已檢查的例外狀況、非類別的基本類型、靜態成員、非私用欄位、萬用字元類型和三元運算子。 Kotlin 是特別針對 Android 設計及建議。 此外，它也提供 JAVA 不提供的一些功能，例如由型別系統所控制的 null 參考、沒有原始型別、不變的陣列、適當的函式類型 (而不是 JAVA 的 SAM 轉換) 、使用不含萬用字元的-網站變異數、智慧型轉換等等。 Kotlin 檔提供 [更深入的探討 JAVA 比較](https://kotlinlang.org/docs/reference/comparison-to-java.html)。

### <a name="minimum-api-level"></a>最低 API 層級

您必須決定應用程式的最低 API 層級。 這會決定您的應用程式將支援的 Android 版本。 較低的 API 層級較舊，因此通常支援更多裝置，但較高的 API 層級較新，且因此提供更多功能。

![Android Studio 最小 API 選取畫面](../images/android-minimum-api-selection.png)

選取 [ **協助我選擇** ] 連結以開啟比較圖表，其中顯示與平臺版本發行相關聯的裝置支援分佈和主要功能。

![Android Studio 最小 API 比較畫面](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>即時應用程式支援和 Androidx 構件

您可能會注意到 **支援即時應用程式** 的核取方塊，另一個則是在您的專案建立選項中 **使用 androidx 構件** 。 不會檢查 *即時應用程式的支援* ，而且會核取 [ *androidx* ] 做為建議的預設值。

Google Play **即時應用程式** 提供一種方式，讓使用者不需要先安裝，即可試用應用程式或遊戲。 這些即時應用程式可以跨 Play 商店、Google Search、社交網路，以及您共用連結的任何位置來呈現。 藉由勾選 [ **支援即時應用程式** ] 方塊，您會要求 Android Studio 將 Google Play 即時開發 SDK 納入您的專案。 若要深入瞭解 [Google Play 即時應用程式](https://developer.android.com/topic/google-play-instant) ，以及如何 [建立立即啟用的應用程式](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)套件組合，請參閱 Android 檔。

**AndroidX 構件** 代表 android 支援程式庫的新版本，並提供跨 Android 版本的回溯相容性。 AndroidX 會提供一致的命名空間，從所有可用封裝的字串 AndroidX 開始。

> [!NOTE]
> AndroidX 現在是預設程式庫。 若要取消核取此方塊並使用先前的支援程式庫，必須移除最新 Android Q SDK。 如需相關指示，請參閱取消核取 [在 StackOverflow [使用 Androidx 構件](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) ]，但請先注意先前的支援程式庫套件已對應到對應的 Androidx. * 套件。 如需所有舊類別和組建成品的完整對應至新的類別，請參閱 [遷移至 AndroidX](https://developer.android.com/jetpack/androidx/migrate)。

## <a name="project-files"></a>專案檔

Android Studio [ **專案** ] 視窗包含下列檔案 (請確定已從下拉式功能表中選取 [android 視圖]) ：

**應用程式 > java > >myfirstapp > >mainactivity**

應用程式的主要活動和進入點。 當您建立並執行您的應用程式時，系統會啟動此活動的實例，並載入其版面配置。

**應用程式 > res > 版面配置 > activity_main.xml**

定義活動使用者介面配置的 XML 檔案 (UI) 。 它包含文字 "Hello World" 的 TextView 元素

**應用程式 > 資訊清單 > AndroidManifest.xml**

描述應用程式的基本特性及其每個元件的資訊清單檔。

**Gradle 腳本 > build. Gradle**

有兩個檔案的名稱如下：「專案：我的第一個應用程式」、適用于整個專案，以及「模組：應用程式」，適用于每個應用程式模組。 新專案一開始只會有一個模組。 使用模組的組建檔案來控制 Gradle 外掛程式如何建立您的應用程式。 若要深入瞭解，請參閱 Android 檔中的 [設定組建](https://developer.android.com/studio/build/index) 文章。

## <a name="use-c-or-c-for-android-game-development"></a>使用 C 或 c + + 進行 Android 遊戲開發

Android 作業系統是設計來支援以 JAVA 或 Kotlin 撰寫的應用程式，並可從系統架構中內嵌的工具獲益。 許多系統功能（例如 Android UI 和意圖處理）只能透過 JAVA 介面公開。 儘管有一些相關的挑戰，但您可能會想要透過 **Android 原生開發工具組，使用 C 或 c + + 程式碼 (NDK)** 。 遊戲開發是一個例子，因為遊戲通常會使用以 OpenGL 或 Vulkan 撰寫的自訂轉譯邏輯，並受益于以遊戲開發為焦點的豐富 C 程式庫。 使用 C 或 c + + 也 *可以* 協助您從裝置中捏出額外的效能，以達到低延遲或執行密集運算的應用程式，例如物理模擬。 但 NDK **並非適用于大部分的新手 Android 程式設計人員** 。 除非您有使用 NDK 的特定用途，否則建議您不要使用 JAVA、Kotlin 或其中一個 [跨平臺](./overview.md)架構。

若要使用 C/c + + 支援來建立新專案：

- 在 Android Studio wizard 的 [ **選擇您的專案** ] 區段中，選取 [ *原生 c + +*] 專案類型。 選取 **[下一步]**，完成其餘欄位，然後再次選取 **[下一步]** 。

- 在嚮導的 [ **自訂 c + + 支援** ] 區段中，您可以使用 **c + + 標準** 欄位自訂您的專案。 您可以使用下拉式清單來選取您想要使用的 c + + 標準化。 選取 [ **工具鏈預設值** ] 會使用預設的 CMake 設定。 選取 [完成]。

- Android Studio 建立新專案之後，您可以在 [**專案**] 窗格中找到 **cpp** 資料夾，其中包含原生原始程式檔、標頭、CMake 或 ndk 組建的組建腳本，以及屬於專案一部分的預建程式庫。 您也可以在資料夾中找到範例 c + + 原始檔，它會提供簡單的函式， `native-lib.cpp` 以傳回 `src/main/cpp/` `stringFromJNI()` 字串 "Hello From c + +"。 此外，您會 [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html) 在模組的根目錄中，找到建立原生程式庫所需的 CMake 組建腳本。

若要深入瞭解，請參閱 Android 檔主題： [將 C 和 c + + 程式碼新增至您的專案](https://developer.android.com/studio/projects/add-native-code)。 如需範例，請參閱 GitHub 上的 [ANDROID NDK 範例與 Android Studio c + + 整合](https://github.com/android/ndk-samples) 存放庫。 若要在 Android 上編譯及執行 c + + 遊戲，請使用 [Google Play 遊戲服務 API](https://developers.google.com/games/services/cpp/gettingStartedAndroid)。

## <a name="design-guidelines"></a>設計指導方針

裝置使用者預期應用程式會以特定方式尋找和行為 .。。無論是以輕量方式或使用語音控制，使用者都能保有您的應用程式應該看起來和使用方式的特定期望。 這些期望必須保持一致，才能減少混淆和挫折。 Android 提供這些平臺和裝置期望的指南，其中結合了適用于視覺效果和導覽模式的 Google 材質設計基礎，以及相容性、效能和安全性的品質指導方針。

若要深入瞭解，請 [參閱 Android 設計檔](https://developer.android.com/design)。

### <a name="fluent-design-system-for-android"></a>適用于 Android 的流暢設計系統

Microsoft 也提供設計指引，目的是要在整個 Microsoft 行動應用程式組合中提供順暢的體驗。

[適用于 android 的流暢設計系統](https://www.microsoft.com/design/fluent/#/android) 設計和建立原生 android 的自訂應用程式，但仍可獨特地流暢。

- [Sketch 工具組](https://aka.ms/fluenttoolkits/android/sketch)
- [Figma 工具組](https://aka.ms/fluenttoolkits/android/figma)
- [Android 字型](https://fonts.google.com/specimen/Roboto)
- [Android 使用者介面指導方針](https://developer.android.com/design/)
- [Android 應用程式圖示的指導方針](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>其他資源

- [Android 應用程式基本概念](https://developer.android.com/guide/components/fundamentals)

- [開發適用于 Android 的雙螢幕應用程式，並取得 Surface 雙核裝置 SDK](/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)

- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)