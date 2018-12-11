---
title: 啟動螢幕剪取
description: 本主題描述 ms-people screenclip 和 ms screensketch URI 配置。 您的應用程式可以使用這些 URI 配置啟動剪取與草圖應用程式或開啟新剪取。
ms.date: 8/1/2017
ms.topic: article
keywords: windows 10、 uwp、 uri、 剪取、 草圖
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a90772e01885a7361cd51b54fc6e5ea9930bfbd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8920557"
---
# <a name="launch-screen-snipping"></a>啟動螢幕剪取

**Ms screenclip:** 和**ms screensketch:** URI 配置可讓您以起始剪或編輯螢幕擷取畫面。

## <a name="open-a-new-snip-from-your-app"></a>從您的應用程式中開啟新剪取

**Ms screenclip:** URI 可讓您的應用程式會自動開啟，並開始新的剪取。 產生剪取會複製到使用者的剪貼簿，但不是會自動傳遞回開啟中應用程式。

**ms screenclip:** 採用下列參數：

| 參數 | 類型 | 必要 | 描述 |
| --- | --- | --- | --- |
| 來源 | 字串 | 否 | 任意方向字串，指出啟動 URI 的來源。 |
| delayInSeconds | 整數 | 否 | 整數值，從 1 到 30 美元。 指定的延遲，以完整秒之間的 URI 呼叫和剪開始時。 |

## <a name="launching-the-snip--sketch-app"></a>啟動剪取 & 草圖應用程式

**Ms screensketch:** URI 可讓您以程式設計方式啟動剪取與草圖應用程式，並在註解針對該應用程式中開啟特定的映像。

**ms screensketch:** 採用下列參數：

| 參數 | 類型 | 必要 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字串 | 否 | 識別要剪取與草圖應用程式中開啟的檔案權杖。 從[SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)擷取。 如果省略此參數，則不需要開啟的檔案來啟動 app。 |
| secondarySharedAccessToken | 字串 | 否 | 使用剪取的相關中繼資料中識別的 JSON 檔案的字串。 中繼資料可能會包含具有陣列的 x、 y 座標，和/或[userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) **clipPoints**欄位。 |
| 來源 | 字串 | 否 | 任意方向字串，指出啟動 URI 的來源。 |
| isTemporary | bool | 否 | 如果設定為 True，螢幕草圖會嘗試開啟它之後刪除的檔案。 |

下列範例會呼叫[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)方法，從使用者的應用程式傳送至剪取 & 草圖的影像。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

下列範例說明**secondaryFileAccessToken**參數的**ms 螢幕擷取畫面**所指定的檔案可能包含：

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
