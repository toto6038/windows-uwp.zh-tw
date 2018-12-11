---
ms.assetid: ''
description: 顯示如何管理 UWP app 的遊戲廣播。
title: 管理遊戲廣播
ms.date: 09/27/2017
ms.topic: article
keywords: Windows 10, 遊戲, 廣播
ms.localizationpriority: medium
ms.openlocfilehash: c906551fd626dec726498ded9a7995007230504f
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8930489"
---
# <a name="manage-game-broadcasting"></a>管理遊戲廣播
本文顯示如何管理 UWP app 的遊戲廣播。 使用者必須使用內建到 Windows 的系統 UI 起始遊戲廣播，但是開始使用 Windows 10 版本 1709，App 可以啟動系統廣播 UI 並且可以在廣播開始和停止時收到通知。

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>將 UWP 的 Windows 桌面延伸新增到您的 App
管理 App 廣播的 API 位於**[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** 命名空間，不包含在通用 API 協定中。 若要存取 API，您必須利用下列步驟將 UWP 的 Windows 桌面延伸的參考新增到您的 App。

1. 在 Visual Studio 中，請在 **\[方案總管\]** 中展開 UWP 專案，以滑鼠右鍵按一下 **\[參考\]**，然後選取 **\[加入參考...\]**。 
2. 展開 **\[通用 Windows\]** 節點，然後選取 **\[延伸\]**。
3. 在延伸清單中，核取符合您專案的目標組建的 **\[UWP 的 Windows 桌面延伸\]** 項目旁的核取方塊。 若是 App 的廣播功能，版本必須是 1709 或以上。
4. 按一下 **\[確定\]**。

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>啟動系統 UI 可讓使用者起始廣播
有幾個原因您的 App 目前可能無法廣播，包括目前的裝置是否不符合廣播的硬體需求，或者其他 App 目前正在廣播中。 啟動系統 UI 之前，您可以查看您的 App 目前是否無法廣播。 首先，檢查廣播 API 在目前的裝置上是否可用。 API 在執行 Windows 10 版本 1709 之前的作業系統版本的裝置上無法使用。 與其檢查特定的作業系統版本，不如使用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 方法來查詢 *Windows.Media.AppBroadcasting.AppBroadcastingContract* 1.0 版。 如果有此協定，則廣播 API 可在裝置上使用。

接下來，取得 **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** 類別的執行個體，方法是在電腦上呼叫 Factory 方法 **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)**，其中一次有單一使用者登入。 在可多位使用者登入的 XBox 上，請改為呼叫 **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)**。 然後呼叫 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** 以取得您的 App 的廣播狀態。

**AppBroadcastingStatus** 類別的 **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** 屬性會告訴您 App 目前是否可以開始廣播。 如果不行，您可以檢查 **[Details](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** 屬性來判斷廣播無法使用的原因。 根據原因而定，您可能想要對使用者顯示狀態，或顯示啟用廣播的指示。

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

藉由呼叫 **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)** 來要求系統顯示 App 廣播 UI。

> [!NOTE] 
> 根據系統的目前狀態，**ShowBroadcastUI** 方法代表要求可能不會成功。 您的 App 不應該假設廣播已在呼叫此方法之後開始。 使用 **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 事件在廣播開始或停止收到通知。

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>廣播開始和停止時接收通知
透過初始化 **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** 類別的執行個體，以及註冊 **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 事件的處理常式，註冊在使用者使用系統 UI 開始或停止廣播您的 App 時收到通知。 如上一節所述，請務必在某些點使用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 來確認在嘗試使用廣播 API 之前其已存在裝置上。 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

在 **IsCurrentAppBroadcastingChanged** 事件的處理常式中，您可能想要更新您的 App UI 以反映目前的廣播狀態。

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>相關主題

* [遊戲](index.md)

 

 




