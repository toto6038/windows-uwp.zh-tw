---
author: QuinnRadich
title: 啟動螢幕剪取
description: 本主題描述 ms-people screenclip 和 ms screensketch URI 配置。 您的應用程式可以使用這些 URI 配置啟動剪取與草圖應用程式或開啟新剪取。
ms.author: quradic
ms.date: 8/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，uri、 剪取草圖
ms.localizationpriority: medium
ms.openlocfilehash: e18662125ef72051a289b3f1d0f3dc09b452d256
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "4947399"
---
# <a name="launch-screen-snipping"></a>啟動螢幕剪取

**Ms screenclip:** 及**ms screensketch:** URI 配置可讓您以起始剪或編輯螢幕擷取畫面。

## <a name="open-a-new-snip-from-your-app"></a>從您的應用程式中開啟新剪取

**Ms screenclip:** URI 可讓您的應用程式會自動開啟，並開始新的剪取。 產生剪取會複製到使用者的剪貼簿，但不是會自動傳遞回開啟中應用程式。

**ms screenclip:** 採用下列參數：

| 參數 | 類型 | 必要 | 描述 |
| --- | --- | --- | --- |
| 來源 | 字串 | 否 | 任意方向的字串，指出啟動 URI 的來源。 |
| delayInSeconds | 整數 | 否 | 從 1 到 30 整數值。 指定的延遲，以完整秒之間的 URI 呼叫和剪開始時。 |

## <a name="launching-the-snip--sketch-app"></a>啟動剪取 & 草圖應用程式

**Ms screensketch:** URI 可讓您以程式設計方式啟動剪取與草圖應用程式，並在該應用程式的註解中開啟特定的映像。

**ms screensketch:** 採用下列參數：

| 參數 | 類型 | 必要 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字串 | 否 | 識別要剪取與草圖應用程式中開啟的檔案權杖。 從[SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)擷取。 如果省略此參數，則不需要開啟的檔案來啟動 app。 |
| 來源 | 字串 | 否 | 任意方向的字串，指出啟動 URI 的來源。 |
| isTemporary | bool | 否 | 如果設定為 True，螢幕繪圖將會嘗試開啟它之後刪除的檔案。 |

下列範例會呼叫[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)方法，從使用者的應用程式傳送至剪取 & 草圖的影像。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```