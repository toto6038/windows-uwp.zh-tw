---
description: 使用目前的 Mac 電腦開發適用於 Windows 的 app。
title: 設定 Mac 的 Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 55165e0369c6bda64c19dc384c5c2addf224b8ba
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259120"
---
# <a name="setting-up-your-mac-with-windows-10"></a>設定 Mac 的 Windows 10


使用目前的 Mac 電腦開發適用於 Windows 的 app。

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>在 Mac 上執行 Windows 和使用 Visual Studio

準備好開始開發通用 Windows 應用程式，但手邊沒有電腦？ 沒關係 — 您可以使用您的 Mac！ 您可以使用 Apple Boot Camp、Oracle VirtualBox、VMware 融合和平行桌面等熱門的協力廠商解決方案，在 Apple 電腦上安裝 Windows 10 和 Microsoft Visual Studio。

**請注意**  在磁片或 USB 快閃磁片磁碟機上，您將需要有 Windows 10 可開機映射。 如果您是 MSDN 訂閱者，可以從 MSDN 訂閱者下載中心下載安裝映像。 如果您不是訂閱者，則可以從[Microsoft Store](https://www.microsoft.com/store/apps)購買安裝程式。 您也可以從[這個位置](https://www.microsoft.com/software-download/windows10)下載，如果您已經執行 Windows 而且想要升級，這就非常好用。

執行 Windows 之後，您就可以從[windows 10 的開發人員下載](https://developer.microsoft.com/en-us/windows/downloads)安裝最新版本的 Visual Studio，並開始撰寫應用程式！

**請注意**  如果您打算使用 Visual Studio 裝置模擬器，就**必須**安裝64位（X64）版本的 Windows 10 專業版或更高的版本。 但是，某些舊版的 Mac 無法執行 64 位元的 Windows。 請透過這個 [Apple 支援頁面](https://support.apple.com/kb/HT5634)，向 Apple 確認您的硬體是否相容。

## <a name="apple-boot-camp"></a>Apple Boot Camp

開機 Camp 助理應用程式會預先安裝在每個最近的 Mac 上，並啟動它將引導您完成安裝 Windows 10 的過程。 您需要的只有一份 Windows 複本 (來自上述的來源)，以及至少 30 Gb 的可用磁碟空間。 安裝之後，您可以選擇開機進入 Mac OSX 或 Windows 10。 如需詳細資訊，請參閱 Apple 的 [Boot Camp 指示頁面](https://support.apple.com/HT201468)。

## <a name="parallels-desktop"></a>Parallels Desktop

使用 Parallels Desktop 11，您就可以同時執行 Windows 應用程式和現有的 Mac 應用程式，包括 Visual Studio 和 Cortana。 現在有適用於開發人員、包含額外功能 (包括改進偵錯，支援 Docker 和 Jenkins) 的專業版版本。 如需詳細資訊和免費的試用版，請參閱 [Parallels Desktop](https://www.parallels.com/download/desktop/)。

## <a name="vmware-fusion"></a>VMware Fusion

VMWare 的 Fusion 8 可讓您在 Mac 桌面上直接執行 Visual Studio。 現在有提供開發人員一些進階功能 (例如 vSphere 支援) 的專業版版本。 如需詳細資訊和免費的試用版，請參閱 [VMware Fusion](http://www.vmware.com/products/fusion/)。

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox 是一個在電腦上用來執行虛擬機器的免費應用程式，支援在 Mac 上執行 Windows。 雖然沒有什麼花俏的功能，但價格很吸引人。 如需詳細資訊，請參閱 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)。

