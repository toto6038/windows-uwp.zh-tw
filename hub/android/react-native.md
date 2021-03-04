---
title: 在 Windows 上回應原生的 Android 開發
description: 逐步指南說明如何在 Windows 上開始使用回應原生，以建立可在 Android 裝置上運作的跨平臺應用程式。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、回應原生、模擬器、博覽會、metro 搭配程式、終端機
ms.date: 04/28/2020
ms.openlocfilehash: 82bf078d6c29c8968ce3e0cc19ce4d6f803e6d71
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823222"
---
# <a name="get-started-developing-for-android-using-react-native"></a>開始使用回應原生開發 Android

本指南將協助您開始使用 Windows 上的回應原生，以建立可在 Android 裝置上運作的跨平臺應用程式。

## <a name="overview"></a>概觀

回應原生是 Facebook 所建立的 [開放原始](https://github.com/facebook/react-native) 碼行動應用程式架構。 它是用來開發適用于 Android、iOS、Web 和 UWP 的應用程式 (Windows) 提供原生 UI 控制項和原生平臺的完整存取權。 使用回應原生需要瞭解 JavaScript 的基本概念。

## <a name="get-started-with-react-native-by-installing-required-tools"></a>藉由安裝必要的工具，開始使用回應原生

1. [安裝 Visual Studio Code](https://code.visualstudio.com) (或選擇) 的程式碼編輯器。

2. [安裝適用于 Windows 的 Android Studio](https://developer.android.com/studio)。 Android Studio 預設會安裝最新的 Android SDK。 回應原生需要 Android 6.0 (Marshmallow) SDK 或更高版本。 我們建議使用最新的 SDK。

3. 建立 JAVA SDK 和 Android SDK 的環境變數路徑：
    - 在 [Windows 搜尋] 功能表中，輸入：「編輯系統內容變數」，這會開啟 [ **系統屬性** ] 視窗。
    - 選擇 [**環境變數**]，然後選擇 [**使用者變數**] 下的 [**新增]。**
    - 輸入變數名稱和值 (路徑) 。 JAVA 和 Android Sdk 的預設路徑如下所示。 如果您已選擇要安裝 JAVA 和 Android Sdk 的特定位置，請務必適當地更新變數路徑。
        - JAVA_HOME： C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME： C:\Users\username\AppData\Local\Android\Sdk

    ![新增環境變數路徑的螢幕擷取畫面](../images/add-environmental-variable-path.png)

4. [安裝適用于 Windows 的 NodeJS](https://nodejs.org/en/) 如果您將使用多個專案和 NodeJS 版本，您可能會想要考慮使用 [Node 版本管理員 (nvm) For Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 。 建議您為新專案安裝最新的 LTS 版本。

> [!NOTE]
> 您也可能想要考慮安裝和使用 [Windows 終端](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) 機， (CLI) ，以及 [Git 進行版本控制](https://git-scm.com/downloads)，以使用您慣用的命令列介面。 [JAVA JDK](https://www.oracle.com/java/technologies/javase-downloads.html)隨附于 android studio 2.2 +，但如果您需要與 android studio 分開更新 JDK，請使用[Windows x64 安裝程式](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)。

## <a name="create-a-new-project-with-react-native"></a>建立具有回應原生的新專案

1. 您可以使用 npm，從 Windows 命令提示字元、PowerShell、 [Windows 終端](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)機或 VS Code 中的整合式終端機，在 VS Code (View > 整合式終端) 中，安裝「[博覽會 CLI](https://docs.expo.io/versions/latest/)命令列公用程式」。

    ```powershell
    npm install -g expo-cli
    ```

2. 使用博覽會來建立可在 iOS、Android 和 web 上執行的回應原生應用程式。 然後，您必須在專案範本之間進行選擇，包括 **空白**、 **空白 (TypeScript)**、 **使用** 「回應導覽」 (範例畫面) 、「 **基本**」或「 **基本」 (TypeScript)**。

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > 如果您使用 `npx create-react-native-app` 的是，它仍可運作，但博覽會-CLI init 有 [一些額外的優點](https://github.com/react-native-community/discussions-and-proposals/issues/23)。

3. 開啟新的「我的新應用程式」目錄：

    ```powershell
    cd my-new-app
    ```

4. 若要執行您的專案，請輸入下列命令。 這會在您的預設網際網路瀏覽器中，開啟顯示節點 Metro 搭配程式的 localhost 視窗。 它也會在您的命令列和 Metro 搭配程式瀏覽器視窗中顯示 QR 代碼。 * 您也可以使用命令： `npm start` 或 `npm run android` 。

     ```powershell
    expo start
    ```

    ![瀏覽器中 Metro 搭配程式的螢幕擷取畫面](../images/metro-bundler.png)

5. 若要查看您在 Android 裝置上執行的專案，您必須先在 Android 裝置上 [安裝具有 Google Play 商店的博覽會用戶端應用程式](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) 。 安裝了「展覽用戶端」應用程式之後，請在裝置上開啟該應用程式，然後選取 [ **掃描 QR 代碼**]。 註冊 QR 代碼之後，您就可以在您的裝置上，以及在瀏覽器的 localhost 上執行的 Metro 搭配程式視窗中看到套件組建。

6. 若要查看在 Android 模擬器上執行的專案，您必須先開啟 Android Studio，然後建立並啟動虛擬裝置。 **工具**  > **AVD 管理員**  > **[+ 建立虛擬裝置 ...](https://developer.android.com/studio/run/managing-avds#createavd)** 建立虛擬裝置之後，請選取 [Android 虛擬裝置管理員] 的 [**動作**] 欄下的 [啟動] 按鈕▷，以開始模擬裝置。 虛擬裝置開啟後，請返回在網際網路瀏覽器視窗中執行的 Metro 搭配程式視窗，然後從左側資料行選取 [在 Android 裝置/模擬器上執行]。 您應該會看到一個快顯視窗，讓您知道 Metro 搭配程式「正在嘗試開啟模擬器 ...」然後，查看在您模擬的 Android 裝置中開啟的博覽會用戶端應用程式，一旦完成下載 JavaScript 套件組合，您就會看到您的回應原生應用程式顯示。  (如果您遇到問題，請 [檢查 [博覽會 Android 模擬器](https://docs.expo.io/workflow/android-studio-emulator/)] 檔。 ) 

7. 開啟您的回應原生專案以開始使用您的應用程式。 您應該會在執行的應用程式中，看到透過您裝置上或在 Android 模擬器中執行的應用程式自動更新的變更。

8. 請嘗試變更登陸網頁檢視文字，例如： "Hello World！"。 您可以在您選擇的 IDE 中進行這項作業。  (建議採用 VS Code 或 Android Studio。根據您選擇的範本，) 登陸頁面檔案將會有所不同。 它可能是 `App.js` 、 `App.tsx` 或 `HomeScreen.js` 。

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. 嘗試新增映射。 首先，您必須在應用程式中建立與 "android" 和 "ios" 資料夾相同層級的資料夾，讓我們將其稱為「映射」。 例如，將影像放在該資料夾中 `your-image.png` 。 使用下列格式來參考您的影像，並以高度和寬度為其設定樣式。

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
> 如果您想要新增回應原生應用程式的支援，使其以 Windows 10 應用程式的形式執行，請參閱 [開始使用 windows 檔的原生回應](https://microsoft.github.io/react-native-windows/docs/getting-started) 。

## <a name="additional-resources"></a>其他資源

- [開發適用于 Android 的雙螢幕應用程式，並取得 Surface 雙核裝置 SDK](/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)

- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)