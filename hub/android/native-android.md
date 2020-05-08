---
title: Windows 上的原生 Android 開發
description: 開始在 Windows 上開發 Android 原生應用程式。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、android studio、visual studio、c + + android 遊戲、windows defender、模擬器、虛擬裝置、安裝、java、kotlin
ms.date: 04/28/2020
ms.openlocfilehash: 31cdd5124c659b5d35a7e8192db1e87821b891f4
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255222"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Windows 上的原生 Android 開發入門

本指南可讓您開始使用 Windows 建立原生 Android 應用程式。 如果您偏好使用跨平臺解決方案，請參閱[Windows 上的 Android 開發總覽](./overview.md)，以取得一些選項的簡短摘要。

建立原生 Android 應用程式最簡單的方式，就是使用[JAVA 或 Kotlin](#java-or-kotlin)的 Android Studio，但如果您有特定目的，也可以[使用 c 或 c + + 進行 Android 開發](#use-c-or-c-for-android-game-development)。 Android Studio SDK 工具會將您的程式碼、資料和資源檔編譯成封存的 Android 套件 apk 檔案。 一個 APK 檔案包含 Android 應用程式的所有內容，而是 Android 裝置用來安裝應用程式的檔案。

## <a name="install-android-studio"></a>安裝 Android Studio

Android Studio 是 Google Android 作業系統的官方整合式開發環境。 [下載適用于 Windows 的最新 Android Studio 版本](https://developer.android.com/studio)。

- 如果您已下載 .exe 檔案（建議選項），請按兩下以啟動它。
- 如果您已下載 .zip 檔案，請將 ZIP 檔案解壓縮，將 android-studio 資料夾複製到 Program Files 資料夾，然後開啟 android-studio > bin 資料夾並啟動 studio64 （適用于64位電腦）或 studio （適用于32位電腦）。

依照 Android Studio 中的安裝程式，並安裝它所建議的任何 SDK 套件。 當有新的工具和其他 api 可供使用時，Android Studio 會透過快顯視窗通知您，**或選取** > [說明] [**檢查更新**] 來檢查更新。

## <a name="create-a-new-project"></a>建立新專案

 > 選取 **[** 檔案] [**新增** > ] [**新增專案**]。

在 [**選擇您的專案**] 視窗中，您將能夠在這些範本之間進行選擇：

- **基本活動**：建立簡單的應用程式，其中包含應用程式行、浮動動作按鈕和兩個設定檔案：一個用於活動，另一個用來分隔文字內容。

- **空白活動**：建立空白活動和含有範例文字內容的單一配置檔案。

- **下方導覽活動**：建立活動的標準下方導覽列。 請參閱 Google 的材料設計指導方針中的[底部流覽元件](https://material.io/guidelines/components/bottom-navigation.html)。

- 深入瞭解如何在 Android Studio 檔中[選取活動範本](https://developer.android.com/studio/projects/templates#SelectTemplate)。

範本通常用來將活動新增至新的和現有的應用程式模組。 例如，若要為應用程式的使用者建立登入畫面，請使用 [登入[活動] 範本](https://developer.android.com/studio/projects/templates#LoginActivity)來新增活動。

> [!NOTE]
> Android 作業系統是以**元件**的概念為基礎，並使用「**活動**」和「**意圖**」詞彙來定義互動。 **活動**代表使用者可以執行的單一焦點工作。 **活動**會提供一個視窗，讓您使用以**View**類別為基礎的類別來建立使用者介面。 Android 作業系統中的**活動**有一個生命週期，由六個回呼的集合所`onCreate()`定義：、 `onStart()`、 `onResume()`、 `onPause()` `onStop()`、和。 `onDestroy()` 活動元件會使用**意圖**物件與其他人互動。 意圖會定義要啟動的活動，或描述要執行的動作類型（而且系統會為您選取適當的活動，甚至是來自不同的應用程式）。 深入瞭解 Android 檔中的[活動](https://developer.android.com/reference/android/app/Activity)、[活動生命週期](https://developer.android.com/guide/components/activities/activity-lifecycle)和[意圖](https://developer.android.com/reference/android/content/Intent.html)。

### <a name="java-or-kotlin"></a>JAVA 或 Kotlin

**JAVA**成為1991中的語言，由 Sun Microsystems 的開發，但現在是由 Oracle 所擁有。 它已成為其中一項最受歡迎且功能強大的程式設計語言，其中包含全球最大的支援團體之一。 JAVA 是以類別為基礎和麵向物件，其設計目的是要盡可能少執行相依性。 語法類似于 C 和 c + +，但其低層級的設施比其中之一少。

**Kotlin**第一次宣佈為 JetBrains 在2011中的新開放原始碼語言，並已納入做為 Android Studio 自2017之後的 JAVA 替代方案。 在5月2019日，Google 宣佈 Kotlin，因為它是 Android 應用程式開發人員的慣用語言，所以即使是較新的語言，它也具有強大的支援小組，並已識別為最快速成長的程式設計語言之一。 Kotlin 是跨平臺、靜態型別，設計成可與 JAVA 完全互通。

JAVA 較廣泛地用於更廣泛的應用程式，並提供一些 Kotlin 沒有的功能，例如已檢查的例外狀況、不是類別、靜態成員、非私用欄位、萬用字元類型和三元運算子的基本類型。 Kotlin 是專為和建議的 Android 所設計。 它也提供一些 JAVA 不適用的功能，例如由型別系統所控制的 null 參考、沒有原始類型、不變數組、適當的函式類型（相對於 JAVA 的 SAM 轉換）、使用不含萬用字元的網站變異數、智慧型轉換等等。 Kotlin 檔提供[更深入的探討與 JAVA 的比較](https://kotlinlang.org/docs/reference/comparison-to-java.html)。

### <a name="minimum-api-level"></a>最低 API 層級

您將需要決定應用程式的最低 API 層級。 這會決定您的應用程式將支援的 Android 版本。 較低的 API 層級較舊，因此通常會支援更多的裝置，但較高的 API 層級較新，而因此提供更多功能。

![Android Studio 最低 API 選取畫面](../images/android-minimum-api-selection.png)

選取 [**協助我選擇**] 連結以開啟比較圖，其中顯示與平臺版本發行相關聯的裝置支援散發和主要功能。

![Android Studio 最低 API 比較畫面](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>即時應用程式支援和 Androidx 構件

您可能會注意到**支援立即應用程式**的核取方塊，以及另一種在專案建立選項中**使用 androidx**成品。 不檢查*即時應用程式支援*，並將*androidx*檢查為建議的預設值。

Google Play**即時應用程式**可讓使用者嘗試應用程式或遊戲，而不需要先進行安裝。 這些即時應用程式可以在 Play Store、Google 搜尋、社交網路，以及您共用連結的任何位置呈現。 藉由勾選 [**支援即時應用程式**] 方塊，您會要求 Android Studio 包含 Google Play 的即時開發 SDK 與您的專案。 若要深入瞭解[Google Play 的即時應用程式](https://developer.android.com/topic/google-play-instant)，以及如何[建立立即啟用的應用程式](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)套件組合，請參閱 Android 檔。

**AndroidX 構件**代表新版本的 android 支援程式庫，並提供跨 Android 版本的回溯相容性。 AndroidX 提供一致的命名空間，開頭為所有可用封裝的字串 AndroidX。

> [!NOTE]
> AndroidX 現在是預設程式庫。 若要取消核取此方塊，並使用先前的支援程式庫，您必須移除最新 Android Q SDK。 如需相關指示，請參閱取消核取 [在 StackOverflow 上[使用 Androidx](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts)成品]，但請注意，先前的支援程式庫套件已對應到相對應的 Androidx. * 套件。 如需所有舊類別和組建成品的完整對應，請參閱[遷移至 AndroidX](https://developer.android.com/jetpack/androidx/migrate)。

## <a name="project-files"></a>專案檔

[Android Studio**專案**] 視窗包含下列檔案（請確定已從下拉式功能表中選取 Android 視圖）：

**應用程式 > java > com。範例 myfirstapp > MainActivity**

應用程式的主要活動和進入點。 當您建立並執行應用程式時，系統會啟動此活動的實例並載入其版面配置。

**應用程式 > res > 版面配置 > activity_main .xml**

定義活動之使用者介面（UI）版面配置的 XML 檔案。 它包含具有文字 "Hello World" 的 TextView 元素

**應用程式 > 資訊清單 > Androidmanifest.xml**

資訊清單檔案，描述應用程式及其每個元件的基本特性。

**Gradle 腳本 > 組建. Gradle**

有兩個檔案具有下列名稱：「專案：我的第一個應用程式」、適用于整個專案，以及「模組：應用程式」（針對每個應用程式模組）。 新的專案一開始只會有一個模組。 使用模組的 build. file 來控制 Gradle 外掛程式如何建立您的應用程式。 若要深入瞭解，請參閱 Android 檔[設定您的組建](https://developer.android.com/studio/build/index)文章。

## <a name="use-c-or-c-for-android-game-development"></a>針對 Android 遊戲開發使用 C 或 c + +

Android 作業系統是設計用來支援以 JAVA 或 Kotlin 撰寫的應用程式，並受益于內嵌于系統架構中的工具。 許多系統功能（例如 Android UI 和意圖處理）只會透過 JAVA 介面來公開。 雖然有一些相關的挑戰，但您還是可以透過**Android 原生開發工具組（NDK）來使用 C 或 c + + 程式碼**。 遊戲開發是一個例子，因為遊戲通常會使用以 OpenGL 或 Vulkan 撰寫的自訂轉譯邏輯，並受益于著重于遊戲開發的豐富 C 程式庫。 使用 C 或 c *+ + 也可協助*您從裝置中降低額外的效能，以達到低延遲或執行密集運算應用程式，例如物理模擬。 不過，NDK**不適合大部分的新手 Android 程式設計人員**。 除非您有使用 NDK 的特定用途，否則我們建議您堅持使用 JAVA、Kotlin 或其中一個[跨平臺](./overview.md)架構。

若要建立具有 C/c + + 支援的新專案：

- 在 [Android Studio wizard] 的 [**選擇您的專案**] 區段中，選取 [*原生 c + +*] 專案類型。 選取 **[下一步]**，完成其餘欄位，然後再次選取 **[下一步]** 。

- 在 wizard 的 [**自訂 c + + 支援**] 區段中，您可以使用 [ **c + + 標準**] 欄位自訂專案。 使用下拉式清單來選取您想要使用的 c + + 標準化。 選取 [**工具鏈預設值**] 會使用預設的 CMake 設定。 選取 [完成]  。

- 一旦 Android Studio 建立新的專案，您就可以在 [**專案**] 窗格中找到一個**cpp**資料夾，其中包含原生原始程式檔、標頭、CMake 或 ndk 組建的組建腳本，以及屬於專案一部分的預建程式庫。 您也可以在`native-lib.cpp` `src/main/cpp/`資料夾中找到範例 c + + 原始檔，此檔案提供了`stringFromJNI()`傳回字串 "Hello from c + +" 的簡單函式。 此外，您還可以在模組的根目錄中[`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html)找到建立原生程式庫所需的 CMake 組建腳本。

若要深入瞭解，請參閱 Android 檔主題：[將 C 和 c + + 程式碼加入至您的專案](https://developer.android.com/studio/projects/add-native-code)。 如需範例，請參閱 GitHub 上的[ANDROID NDK 範例與 Android Studio c + + 整合](https://github.com/android/ndk-samples)存放庫。 若要在 Android 上編譯和執行 c + + 遊戲，請使用[Google Play 的遊戲服務 API](https://developers.google.com/games/services/cpp/gettingStartedAndroid)。

## <a name="design-guidelines"></a>設計指導方針

裝置使用者預期應用程式會以某種方式來查看和行為 .。。不論是輕量或使用語音控制項，使用者都將保有應用程式外觀的特定期望，以及如何使用它。 這些期望應該保持一致，以便減少混淆和挫折。 Android 提供這些平臺和裝置期望的指南，其中結合了適用于視覺效果和導覽模式的 Google 材質設計基礎，以及相容性、效能和安全性的品質指導方針。

若要深入瞭解，請[參閱 Android 設計檔](https://developer.android.com/design)。

### <a name="fluent-design-system-for-android"></a>適用于 Android 的流暢設計系統

Microsoft 也提供設計指引，其目標是要在整個 Microsoft 行動應用程式組合中提供順暢的體驗。

[針對 android 設計提供流暢的設計系統](https://www.microsoft.com/design/fluent/#/android)，並建立原生 android 的自訂應用程式，但仍是唯一流暢的。

- [Sketch 工具組](https://aka.ms/fluenttoolkits/android/sketch)
- [Figma 工具組](https://aka.ms/fluenttoolkits/android/figma)
- [Android 字型](https://fonts.google.com/specimen/Roboto)
- [Android 使用者介面指導方針](https://developer.android.com/design/)
- [Android 應用程式圖示的指導方針](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>其他資源

- [Android 應用程式基本概念](https://developer.android.com/guide/components/fundamentals)

- [開發適用于 Android 的雙畫面應用程式，並取得 Surface 雙核裝置 SDK](https://docs.microsoft.com/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)

- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)
