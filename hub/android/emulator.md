---
title: 從 Windows 執行 Android 裝置或模擬器
description: 從 Windows 在 Android 裝置或模擬器上測試您的應用程式，並使用 hyper-v 和 Windows 虛擬機器平臺（WHPX）啟用虛擬化。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android，windows，模擬器，虛擬裝置，裝置設定，啟用裝置，開發人員，設定，虛擬化，visual studio，hyper-v，intel，haxm，amd，Windows 虛擬程式平臺，WHPX
ms.date: 04/28/2020
ms.openlocfilehash: c651661d573695902368ffa595ce5d3014791a9a
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255152"
---
# <a name="test-on-an-android-device-or-emulator"></a>在 Android 裝置或模擬器上進行測試

有數種方式可以使用 Windows 電腦上的實際裝置或模擬器來測試和偵測 Android 應用程式。 我們已在本指南中概述一些建議。

## <a name="run-on-a-real-android-device"></a>在實際的 Android 裝置上執行

若要在實際的 Android 裝置上執行您的應用程式，您必須先啟用 Android 裝置以進行開發。 自4.2 版起，預設會隱藏 Android 上的開發人員選項，並根據 Android 版本來加以啟用。

### <a name="enable-your-device-for-development"></a>啟用您的裝置以進行開發

針對執行最新版本 Android 9.0 + 的裝置：

1. 使用 USB 纜線將您的裝置連線到 Windows 開發電腦。 您可能會收到安裝 USB 驅動程式的通知。
2. 在您的 Android 裝置上開啟 [**設定**] 畫面。
3. 選取 [**關於電話**]。
4. 在**您現在為開發人員**之前，請先前往底部，再按 [**組建編號**] 七次。 為可見。
5. 回到上一個畫面，選取 [**系統**]。
6. 選取 [ **Advanced**]、[向下]，然後按一下 [**開發人員選項**]。
7. 在 [**開發人員選項**] 視窗中，向下流覽以尋找並啟用**USB 調試**。

針對執行舊版 Android 的裝置，請參閱[設定裝置以進行開發](https://docs.microsoft.com/xamarin/android/get-started/installation/set-up-device-for-development)。

### <a name="run-your-app-on-the-device"></a>在裝置上執行您的應用程式

1. 在 [Android Studio] 工具列中，**從 [回合**設定] 下拉式功能表中選取您的應用程式。

    ![Android Studio 執行設定] 功能表](../images/android-run-config-menu.png)

2. 從 [**目標裝置**] 下拉式功能表中，選取您想要在其中執行應用程式的裝置。

    ![Android Studio 目標裝置功能表](../images/android-target-device-menu.png)

3. 選取 [執行▷]。 這會在您連線的裝置上啟動應用程式。

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>使用模擬器在虛擬 Android 裝置上執行應用程式

在 Windows 電腦上執行 Android 模擬器的第一件事是，不論您的 IDE （Android Studio、Visual Studio 等等）為何，都能藉由啟用虛擬化支援來大幅改善模擬器效能。

### <a name="enable-virtualization-support"></a>啟用虛擬化支援

使用 Android 模擬器建立虛擬裝置之前，建議您開啟 Hyper-v 和 Windows 虛擬機器平臺（WHPX）功能來啟用虛擬化。 這可讓您電腦的處理器大幅提升模擬器的執行速度。

> 若要執行 Hyper-v 和 Windows 虛擬程式平臺，您的電腦必須：
>
> * 有4GB 的可用記憶體
> * 具有64位 Intel 處理器或 AMD Ryzen CPU，並具有第二層位址轉譯（SLAT）
> * 正在執行 Windows 10 build 1803 + （[檢查您的組建 #](ms-settings:about)）
> * 已更新圖形驅動程式（Device Manager > 顯示介面卡 > 更新驅動程式）
>
> 如果您的電腦不符合此準則，您可以執行[INTEL HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows)或[AMD 虛擬機器](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)。 如需詳細資訊，請參閱下列文章：[適用于模擬器效能的硬體加速](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration)或[Android Studio 模擬器檔](https://developer.android.com/studio/run/emulator)。

1. 開啟命令提示字元並輸入命令，確認電腦的硬體和軟體與 Hyper-v 相容：`systeminfo`

    ![命令提示字元中 systeminfo 的 hyper-v 需求](../images/systeminfo.png)

2. 在 Windows 搜尋方塊（左下方）中，輸入「Windows 功能」。 從搜尋結果中選取 [**開啟或關閉 Windows 功能**]。

3. [ **Windows 功能**] 清單出現之後，請滾動至 [尋找**hyper-v** ] （包括 [管理工具] 和 [平臺]）和 [ **windows 虛擬機器平臺**]，確定已核取此方塊以啟用兩者，然後選取 **[確定]**。

4. 出現提示時，重新啟動您的電腦。

### <a name="emulator-for-native-development-with-android-studio"></a>使用 Android Studio 進行原生開發的模擬器

在建立和測試原生 Android 應用程式時，我們建議[使用 Android Studio](./native-android.md)。 當您的應用程式準備好進行測試之後，您可以透過下列方式建立並執行您的應用程式：

1. 在 [Android Studio] 工具列中，**從 [回合**設定] 下拉式功能表中選取您的應用程式。

    ![Android Studio 執行設定] 功能表](../images/android-run-config-menu.png)

2. 從 [**目標裝置**] 下拉式功能表中，選取您想要在其中執行應用程式的裝置。

    ![Android Studio 目標裝置功能表](../images/android-target-device-menu.png)

3. 選取 [執行▷]。 這會啟動[Android Emulator](https://developer.android.com/studio/run/emulator)。

> [!TIP]
> 當您的應用程式安裝在模擬器裝置上之後，您就可以使用 [套用[變更](https://developer.android.com/studio/run#apply-changes)] 來部署特定的程式碼和資源變更，而不需要建立新的 APK。

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>使用 Visual Studio 進行跨平臺開發的模擬器

Windows 電腦有許多[Android 模擬器選項](https://www.androidauthority.com/best-android-emulators-for-pc-655308/)可供使用。 我們建議使用 Google 的[android 模擬器](https://developer.android.com/studio/run/emulator)，因為它可讓您存取最新的 android OS 映射和 Google Play 服務。

### <a name="install-android-emulator-with-visual-studio"></a>使用 Visual Studio 安裝 Android 模擬器

1. 如果您尚未安裝，請下載[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)。 使用 Visual Studio 安裝程式來[修改您的工作負載](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads)，並確定您已**使用 .net 工作負載**進行行動裝置開發。

2. 建立新專案。 [設定好 Android Emulator](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/)之後，您就可以使用[Android Device Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#requirements)來建立、複製、自訂和啟動各種 Android 虛擬裝置。 從 [工具] 功能表使用 [**工具** > ] [**Android** > **Android Device Manager**] 啟動 Android Device Manager。

3. Android Device Manager 開啟之後，請選取 [ **+ 新增**] 來建立新的裝置。

4. 您必須為裝置提供名稱，從下拉式功能表中選擇 [基底裝置類型]、選擇 [處理器] 和 [作業系統版本]，以及針對虛擬裝置使用數個其他變數。 如需詳細資訊，請[Android Device Manager 主畫面](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#main-screen)。

5. 在 [Visual Studio] 工具列中，選擇 [ **Debug** ] （在應用程式啟動後附加至模擬器內執行的應用程式進程）或 [**發行**] 模式（停用偵錯工具）。 然後從 [裝置] 下拉式功能表中選擇虛擬裝置，然後選取 [**播放**] 按鈕▷，在模擬器中執行您的應用程式。

    ![Visual Studio 啟動 Android Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>其他資源

- [開發適用于 Android 的雙畫面應用程式，並取得 Surface 雙核裝置 SDK](https://docs.microsoft.com/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)
