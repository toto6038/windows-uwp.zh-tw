---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: 本文示範如何利用只會出現在行動裝置上的特殊相機 UI 功能。
title: 適用於行動裝置的相機 UI 功能
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e1076c0632299ff79a8ca2fc226865d0ff3ebef
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362891"
---
# <a name="camera-ui-features-for-mobile-devices"></a>適用於行動裝置的相機 UI 功能

本文示範如何利用只會出現在行動裝置上的特殊相機 UI 功能。 

## <a name="add-the-mobile-extension-to-your-project"></a>將行動裝置擴充功能新增到您的專案 

若要使用這些功能，您必須將 Microsoft Mobile Extension SDK for Universal App Platform 的參照新增到您的專案。

**新增硬體相機按鈕支援的行動擴充功能 SDK 的參照**

1.  在 [方案總管]**** 中，以滑鼠右鍵按一下 [參考]****，然後選取 [新增參考]****。

2.  展開 **\[Windows 通用\]** 節點，然後選取 **\[擴充功能\]**。

3.  選取 **\[Microsoft Mobile Extension SDK for Universal App Platform\]** 核取方塊。

## <a name="hide-the-status-bar"></a>隱藏狀態列

行動裝置的 [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) 控制項可為使用者提供有關裝置的狀態資訊。 此控制項會佔掉螢幕上可能會干擾媒體擷取 UI 的空間。 您可以呼叫 [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) 以隱藏狀態列，但是您必須在條件性區塊中進行此呼叫，而此種區塊中您會使用 [**ApiInformation.IsTypePresent**](/uwp/api/windows.foundation.metadata.apiinformation.istypepresent) 方法來判斷 API 是否可用。 這個方法只會在支援狀態列的行動裝置上傳回 true。 您應該在 app 啟動時或在您開始從相機進行預覽時隱藏狀態列。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHideStatusBar":::

當 app 關閉或使用者離開 app 的媒體擷取頁面時，您可讓此控制項再次顯現。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetShowStatusBar":::

## <a name="use-the-hardware-camera-button"></a>使用硬體相機按鈕

相較於螢幕上的控制項，有些行動裝置有某些使用者偏好的專用硬體相機按鈕。 若要在按下硬體相機按鈕時收到通知，請登錄 [**HardwareButtons.CameraPressed**](/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed) 事件的處理常式。 因為此 API 只能用於行動裝置，所以您必須再次使用 **IsTypePresent** 來確定目前的裝置支援此 API，再嘗試進行存取。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPhoneUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterCameraButtonHandler":::

在 **CameraPressed** 事件的處理常式中，您可以起始相片擷取。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCameraPressed":::

當 app 關閉或使用者離開 app 的媒體擷取頁面時，請取消登錄硬體按鈕處理常式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUnregisterCameraButtonHandler":::

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
