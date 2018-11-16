---
author: TerryWarwick
title: 主控預覽相機條碼掃描器
description: 了解如何在您的應用程式中主控預覽相機條碼掃描器
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 9684db2495e974c23d81b21e9a4a2e764d390255
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103552"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>在您的應用程式中主控預覽相機條碼掃描器
## <a name="step-1-setup-your-camera-preview"></a>步驟 1：設定您的相機預覽
在為相機條碼掃描器新增預覽到您的應用程式的第一個步驟，可遵循[顯示相機預覽]](../audio-video-camera/simple-camera-preview-access.md)主題中的指示來完成。  一旦您完成此步驟後，返回本主題以進行相機條碼掃描器的特定修改。

## <a name="step-2-update-capability-declarations"></a>步驟 2：更新功能宣告
若要防止使用者接收同意麥克風的提示，您可以從您的 App 資訊清單中所列出的功能排除此項。

1. 在 Microsoft Visual Studio 中，按兩下 **\[方案總管\]** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2. 選取 **\[功能\]** 索引標籤。
3. 取消選取 **\[麥克風\]** 方塊

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>步驟 3：使用媒體擷取的指示詞新增其他項目

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>步驟 4：設定您的 MediaCapture 初始化設定
下列範例初始化 [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)。 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>步驟 5：關聯 MediaCapture 物件與相機條碼掃描器
使用下列項目取代現有的 mediaCapture.InitializeAsync() in *StartPreviewAsync()*：

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> 如需有關在應用程式中主控相機預覽的主題，請參閱[顯示相機預覽](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest)。
