---
title: Windows 上的 Android 開發 React Native
description: 開始使用 Windows 上的 Xamarin 原生開發 Android 應用程式。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android，windows，回應原生、模擬器、展覽、metro 搭配程式、終端機
ms.date: 04/28/2020
ms.openlocfilehash: db49e3ed12fee8e7ced7680e305a84afb89f32a2
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255232"
---
# <a name="get-started-developing-for-android-using-react-native"></a>使用 React Native 開始針對 Android 進行開發

本指南將協助您開始使用 Windows 上的 React Native 來建立可在 Android 裝置上工作的跨平臺應用程式。

## <a name="overview"></a>概觀

React Native 是 Facebook 所建立的[開放原始](https://github.com/facebook/react-native)碼行動應用程式架構。 它可用來開發適用于 Android、iOS、Web 和 UWP （Windows）的應用程式，以提供原生 UI 控制項和原生平臺的完整存取權。 使用 React Native 需要瞭解 JavaScript 基本概念。

## <a name="get-started-with-react-native-by-installing-required-tools"></a>安裝必要的工具以開始使用 React Native

1. [安裝 Visual Studio Code](https://code.visualstudio.com) （或您選擇的程式碼編輯器）。

2. [安裝適用于 Windows 的 Android Studio](https://developer.android.com/studio)。 Android Studio 預設會安裝最新的 Android SDK。 React Native 需要 Android 6.0 （Marshmallow） SDK 或更新版本。 我們建議使用最新的 SDK。

3. 建立 JAVA SDK 和 Android SDK 的環境變數路徑：
    - 在 Windows [搜尋] 功能表中，輸入：「編輯系統內容變數」，這會開啟 [**系統屬性**] 視窗。
    - 選擇 [**環境變數 ...** ]，然後選擇 [**使用者變數**] 底下的 [**新增]。**
    - 輸入變數名稱和值（路徑）。 JAVA 和 Android Sdk 的預設路徑如下所示。 如果您已選擇要安裝 JAVA 和 Android Sdk 的特定位置，請務必據以更新變數路徑。
        - JAVA_HOME： C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME： C:\Users\username\AppData\Local\Android\Sdk

    ![新增環境變數路徑的螢幕擷取畫面](../images/add-environmental-variable-path.png)

4. [安裝適用于 Windows 的 NodeJS](https://nodejs.org/en/)如果您將使用多個專案和版本的 NodeJS，您可能會想要考慮使用[適用于 Windows 的 Node 版本管理員（nvm）](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 。 我們建議您為新專案安裝最新的 LTS 版本。

> [!NOTE]
> 您可能也想要考慮安裝和使用[Windows 終端](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)機，以使用您慣用的命令列介面（CLI），以及[Git 進行版本控制](https://git-scm.com/downloads)。 [JAVA JDK](https://www.oracle.com/java/technologies/javase-downloads.html)會與 Android Studio v1.0 + 封裝在一起，但如果您需要與 Android Studio 分開更新 JDK，請使用[Windows x64 安裝程式](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)。

## <a name="create-a-new-project-with-react-native"></a>使用 React Native 建立新的專案

1. 使用 npm，從 Windows 命令提示字元、PowerShell、 [Windows 終端](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)機或 VS Code 中的整合式終端機（View > 整合式終端機）安裝 [[博覽會 CLI](https://docs.expo.io/versions/latest/)命令列公用程式]。

    ```powershell
    npm install -g expo-cli
    ```

2. 使用 [博覽會] 建立在 iOS、Android 和 web 上執行的 React Native 應用程式。 接著，您必須在專案範本之間選擇，包括**空白**、**空白（TypeScript）**、索引標籤（使用 [回應-流覽 **]** 的範例畫面）、**最小**或**最小（TypeScript）**。

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > 如果您習慣使用`npx create-react-native-app`，這仍然可以正常執行，但博覽會-CLI init 還有[一些額外的好處](https://github.com/react-native-community/discussions-and-proposals/issues/23)。

3. 開啟新的「我的新應用程式」目錄：

    ```powershell
    cd my-new-app
    ```

4. 若要執行您的專案，請輸入下列命令。 這會在預設的網際網路瀏覽器中開啟 localhost 視窗，顯示節點 Metro 搭配程式。 它也會在您的命令列和 Metro 搭配程式瀏覽器視窗中顯示 QR 代碼。 * 您也可以使用命令： `npm start`或`npm run android` 。

     ```powershell
    expo start
    ```

    ![瀏覽器中 Metro 搭配程式的螢幕擷取畫面](../images/metro-bundler.png)

5. 若要查看在 Android 裝置上執行的專案，您必須先使用 Android 裝置上[的 Google Play 商店安裝 [博覽會用戶端應用程式](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US)]。 安裝了博覽會用戶端應用程式之後，請在您的裝置上開啟它，然後選取 [**掃描 QR 代碼**]。 註冊 QR 代碼之後，您就能夠在您的裝置上，以及在您瀏覽器的 localhost 上執行的 Metro 搭配程式視窗中，看到套件組建。

6. 若要查看在 Android 模擬器上執行的專案，您必須先開啟 Android Studio，然後建立並啟動虛擬裝置。 **工具** > **AVD 管理員** > **[+ 建立虛擬裝置](https://developer.android.com/studio/run/managing-avds#createavd)**...。建立虛擬裝置之後，請選取 [Android 虛擬 Device Manager 的 [**動作**] 資料行底下的 [啟動] 按鈕▷，以開始模擬裝置。 虛擬裝置開啟之後，返回您在網際網路瀏覽器視窗中執行的 Metro 搭配程式視窗，然後從左欄中選取 [在 Android 裝置/模擬器上執行]。 您應該會看到快顯視窗，讓您知道 Metro 搭配程式是「正在嘗試開啟模擬器 ...」然後，查看在您的模擬 Android 裝置中開啟的 [使用者] 用戶端應用程式，完成下載 JavaScript 配套後，您會看到顯示 React Native 應用程式。 （如果您遇到問題，請[查看 [博覽會 Android 模擬器](https://docs.expo.io/workflow/android-studio-emulator/)] 檔）。

7. 開啟您的 React Native 專案，開始處理您的應用程式。 您應該會在應用程式中看到透過您裝置上的博覽會用戶端或 Android Emulator 中自動更新的變更。

8. 請嘗試變更登陸網頁檢視文字，以說出： "Hello World！"。 您可以在您選擇的 IDE 中執行此動作。 （建議 VS Code 或 Android Studio）。登陸頁面檔案會根據您選擇的範本而有所不同。 它可以是`App.js`、 `App.tsx`或`HomeScreen.js`。

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. 嘗試新增影像。 首先，您必須在應用程式中建立與「android」和「ios」資料夾相同層級的資料夾，讓我們將它稱為「映射」。 例如，將影像放在該`your-image.png`資料夾中。 使用下列格式來參考您的影像，並使用高度和寬度來設定其樣式。

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> 如果您想要新增 React Native 應用程式的支援，使其以 Windows 10 應用程式的形式執行，請參閱[開始使用適用于 windows 的 React Native](https://microsoft.github.io/react-native-windows/docs/getting-started)檔。

## <a name="additional-resources"></a>其他資源

- [開發適用于 Android 的雙畫面應用程式，並取得 Surface 雙核裝置 SDK](https://docs.microsoft.com/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)

- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)
