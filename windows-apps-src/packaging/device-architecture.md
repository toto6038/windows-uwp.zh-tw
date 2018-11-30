---
title: 應用程式套件架構
description: 深入了解在建置 UWP app 套件時您應使用哪種處理器架構。
ms.date: 7/13/2017
ms.topic: article
keywords: Windows 10, uwp, 封裝, 架構, 套件設定
ms.localizationpriority: medium
ms.openlocfilehash: 15b1b8531390cf1b83c15e0aec1d371266417a87
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8205772"
---
# <a name="app-package-architectures"></a>應用程式套件架構

應用程式套件是設定來執行特定的處理器架構。 透過選取架構，您可指定您的應用程式要在哪些裝置上執行。 通用 Windows 平台 (UWP) 應用程式可以設定為在下列架構上執行：
- x86
- x64
- ARM

**強烈**建議您將您的應用程式套件建置為以所有架構為目標。 如果取消選取裝置架構，將會限制可執行您應用程式的裝置，從而限制可以使用您應用程式的人數！

## <a name="windows-10-devices-and-architectures"></a>Windows 10 裝置與架構

> [!div class="mx-tableFixed"]
| UWP 架構 | 桌上型電腦 (x86)      | 桌上型電腦 (x64)      | 桌上型電腦 (ARM)      | 行動裝置             | HoloLens           | Xbox               | IoT 核心版 (裝置相依) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |
 

讓我們更詳細討論這些架構。 

### <a name="x86"></a>x86
選擇 x86 對於應用程式套件通常是最安全的設定，因為它幾乎可在每個裝置上執行。 在某些裝置上，使用 x86 設定的應用程式套件不會執行，例如 Xbox 或某些 IoT 核心版裝置。 不過對於電腦而言，x86 套件是最安全的選項，可涵蓋最大範圍的裝置部署。 極大量的 Windows 10 裝置仍繼續執行 Windows 的 x86 版本。 

### <a name="x64"></a>x64
此設定比 x86 設定更少用到。 應注意的是，此設定是保留給使用 64 位元版本 Windows 10 的桌上型電腦、[Xbox 上的 UWP 應用程式](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation)，以及 Intel Joule 上的 Windows 10 IoT 核心版。

### <a name="arm"></a>ARM
ARM 上的 Windows 10 設定包含桌上型電腦、行動裝置和某些 IoT 核心版裝置 (Rasperry Pi 2、Raspberry Pi 3 和 DragonBoard)。 ARM 上的 Windows 10 桌上型電腦是 Windows 系列的新成員，因此如果您是 UWP app 開發人員，您應該提交 ARM 套件至市集，以便為這些電腦提供最佳使用體驗。 

請查看此 //Build 討論，獲得 [ARM 上的 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) 示範並了解其運作方式。 

如需 IoT 特定主題的詳細資訊，請參閱[使用 Visual Studio 部署應用程式](https://developer.microsoft.com/windows/iot/Docs/AppDeployment) (英文)。
