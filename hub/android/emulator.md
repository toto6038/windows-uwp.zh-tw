---
title: 從 Windows 執行 Android 裝置或模擬器
description: 從 Windows 在 Android 裝置或模擬器上測試您的應用程式，並使用 hyper-v 和 Windows 虛擬程式平臺 (WHPX) 來啟用虛擬化。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、模擬器、虛擬裝置、裝置設定、啟用裝置、開發人員、設定、虛擬化、visual studio、hyper-v、intel、haxm、amd、Windows 程式管理平臺、WHPX
ms.date: 04/28/2020
ms.openlocfilehash: 57e1d8d62ea7b3918c5e52724c11febcb9f03d72
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161552"
---
# <a name="test-on-an-android-device-or-emulator"></a>在 Android 裝置或模擬器上進行測試

有幾種方式可在 Windows 電腦上使用真實裝置或模擬器來測試和偵測 Android 應用程式。 本指南概述了幾項建議。

## <a name="run-on-a-real-android-device"></a>在實際的 Android 裝置上執行

若要在實際的 Android 裝置上執行您的應用程式，您必須先啟用您的 Android 裝置以進行開發。 自4.2 版開始，Android 上的開發人員選項預設為隱藏，因此可能會因 Android 版本而異。

### <a name="enable-your-device-for-development"></a>啟用您的裝置以用於開發

針對執行最新版 Android 9.0 + 的裝置：

1. 使用 USB 纜線將您的裝置連接到您的 Windows 開發電腦。 您可能會收到安裝 USB 驅動程式的通知。
2. 開啟 Android 裝置上的 [ **設定** ] 畫面。
3. 選取 [ **關於電話**]。
4. 在**您現在為開發人員**之前，請先向下移動並點一下**組建編號**七次！ 是可見的。
5. 回到上一個畫面，選取 [ **System**]。
6. 選取 [ **Advanced**]、[向下移動]，然後按一下 [ **開發人員選項**]。
7. 在 [ **開發人員選項** ] 視窗中，向下滾動以尋找並啟用 **USB 調試**。

針對執行舊版 Android 的裝置，請參閱 [設定裝置以進行開發](/xamarin/android/get-started/installation/set-up-device-for-development)。

### <a name="run-your-app-on-the-device"></a>在裝置上執行您的應用程式

1. 在 [Android Studio] 工具列的 [回合設定] 下拉式功能表中，選取 **您的應用** 程式。

    ![Android Studio 執行設定] 功能表](../images/android-run-config-menu.png)

2. 從 [ **目標裝置** ] 下拉式功能表中，選取您要執行應用程式的裝置。

    ![Android Studio 目標裝置功能表](../images/android-target-device-menu.png)

3. 選取 [執行▷]。 這會在連線的裝置上啟動應用程式。

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>使用模擬器在虛擬 Android 裝置上執行您的應用程式

在 Windows 電腦上執行 Android 模擬器的第一件事是，不論您的 IDE (Android Studio、Visual Studio 等) ，都能藉由啟用虛擬化支援，大幅改善模擬器的效能。

### <a name="enable-virtualization-support"></a>啟用虛擬化支援

使用 Android 模擬器建立虛擬裝置之前，建議您開啟 Hyper-v 和 Windows 虛擬程式平臺 (WHPX) 功能，以啟用虛擬化。 這可讓您的電腦處理器大幅改善模擬器的執行速度。

> 若要執行 Hyper-v 和 Windows 虛擬程式平臺，您的電腦必須：
>
> * 有4GB 的記憶體可用
> * 使用具有第二層位址轉譯 (SLAT) 的64位 Intel 處理器或 AMD Ryzen CPU
> * 正在執行 Windows 10 build 1803 + ([檢查您的組建 #](ms-settings:about)) 
> * 已更新圖形驅動程式 (裝置管理員 > 顯示配接器 > 更新驅動程式) 
>
> 如果您的電腦不符合此準則，您可能可以執行 [INTEL HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) 或 [AMD 虛擬](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)程式。 如需詳細資訊，請參閱文章： [適用于模擬器效能的硬體加速](/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) 或 [Android Studio 模擬器檔](https://developer.android.com/studio/run/emulator)。

1. 開啟命令提示字元並輸入命令，確認電腦的硬體和軟體是否與 Hyper-v 相容： `systeminfo`

    ![命令提示字元中的 systeminfo hyper-v 需求](../images/systeminfo.png)

2. 在 [Windows 搜尋] 方塊中 (左下方) ，輸入「Windows 功能」。 從搜尋結果中選取 [ **開啟或關閉 Windows 功能** ]。

3. 出現 [ **Windows 功能** ] 清單之後，請進行滾動以找出 **hyper-v** (同時包含管理工具和平臺) 和 **Windows 虛擬程式平臺**，請確定已核取此方塊來啟用兩者，然後選取 **[確定]**。

4. 在收到提示時重新啟動您的電腦。

### <a name="emulator-for-native-development-with-android-studio"></a>使用 Android Studio 的原生開發模擬器

建立和測試原生 Android 應用程式時，建議 [使用 Android Studio](./native-android.md)。 當您的應用程式準備好進行測試之後，您可以透過下列方式建立並執行您的應用程式：

1. 在 [Android Studio] 工具列的 [回合設定] 下拉式功能表中，選取 **您的應用** 程式。

    ![Android Studio 執行設定] 功能表](../images/android-run-config-menu.png)

2. 從 [ **目標裝置** ] 下拉式功能表中，選取您要執行應用程式的裝置。

    ![Android Studio 目標裝置功能表](../images/android-target-device-menu.png)

3. 選取 [執行▷]。 這將會啟動 [Android Emulator](https://developer.android.com/studio/run/emulator)。

> [!TIP]
> 當您的應用程式安裝在模擬器裝置上之後，您就可以使用 [套用 [變更](https://developer.android.com/studio/run#apply-changes) ] 來部署特定的程式碼和資源變更，而不需要建立新的 APK。

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>使用 Visual Studio 的跨平臺開發模擬器

有許多適用于 Windows 電腦的 [Android 模擬器選項](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) 。 我們建議使用 Google 的 [android 模擬器](https://developer.android.com/studio/run/emulator)，因為它可讓您存取最新的 android OS 映射和 Google Play 服務。

### <a name="install-android-emulator-with-visual-studio"></a>使用 Visual Studio 安裝 Android 模擬器

1. 如果尚未安裝，請下載 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)。 使用 Visual Studio 安裝程式來 [修改您的工作負載](/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) ，並確保您具有 **.net 工作負載**的行動裝置開發。

2. 建立新專案。 [設定 Android Emulator](/xamarin/android/get-started/installation/android-emulator/)之後，您可以使用[Android 裝置管理員](/xamarin/android/get-started/installation/android-emulator/device-manager?pivots=windows&tabs=windows#requirements)來建立、複製、自訂和啟動各種 Android 虛擬裝置。 從 [工具] 功能表啟動 Android 裝置管理員： **tools**  >  **Android**  >  **Android 裝置管理員**。

3. Android 裝置管理員開啟後，選取 [ **+ 新增** ] 以建立新的裝置。

4. 您必須提供裝置的名稱，從下拉式功能表中選擇基礎裝置類型、選擇處理器和作業系統版本，以及虛擬裝置的其他數個變數。 如需詳細資訊，請 [Android 裝置管理員主畫面](/xamarin/android/get-started/installation/android-emulator/device-manager?pivots=windows&tabs=windows#main-screen)。

5. 在 [Visual Studio] 工具列中，選擇 [在應用程式) 啟動後，在模擬器中執行的應用程式] **Release** (附加至應用程式， (**停用調試**程式) 。 然後從裝置下拉式功能表中選擇虛擬裝置，然後選取 [ **播放** ] 按鈕▷，在模擬器中執行您的應用程式。

    ![Visual Studio 啟動 Android Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>其他資源

- [開發適用于 Android 的雙螢幕應用程式，並取得 Surface 雙核裝置 SDK](/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)