---
title: 啟動螢幕剪取
description: 本主題說明 screenclip 和 ms screensketch URI 配置。 您的應用程式可以使用這些 URI 配置來啟動剪取 & 草圖應用程式，或開啟新的剪取。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10、uwp、uri、剪取、草圖
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2d7471f414922eb1e4923079082ee6abfd8418bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167812"
---
# <a name="launch-screen-snipping"></a>啟動螢幕剪取

**Screenclip：** 和**ms screensketch：** URI 配置可讓您起始剪取或編輯螢幕擷取畫面。

## <a name="open-a-new-snip-from-your-app"></a>從您的應用程式開啟新的剪取

**Screenclip：** URI 可讓您的應用程式自動開啟並開始新的剪取。 產生的剪取會複製到使用者的剪貼簿，但不會自動傳回至開啟的應用程式。

**screenclip：** 接受下列參數：

| 參數 | 類型 | 必要 | 說明 |
| --- | --- | --- | --- |
| source | 字串 | 否 | 表示啟動 URI 之來源的自由格式字串。 |
| delayInSeconds | int | 否 | 從1到30的整數值。 指定 URI 呼叫和剪取開始之間的延遲（以完整秒為單位）。 |
| callbackformat | 字串 | 否 | 此參數無法使用。 |

## <a name="launching-the-snip--sketch-app"></a>啟動剪取 & 草圖應用程式

**Screensketch：** URI 可讓您以程式設計方式啟動剪取 & 草圖應用程式，並在該應用程式中開啟批註的特定影像。

**screensketch：** 接受下列參數：

| 參數 | 類型 | 必要 | 說明 |
| --- | --- | --- | --- |
| sharedAccessToken | 字串 | 否 | 權杖，識別要在剪取 & 草圖應用程式中開啟的檔案。 取自 [SharedStorageAccessManager. AddFile](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)。 如果省略此參數，將會在沒有開啟檔案的情況下啟動應用程式。 |
| secondarySharedAccessToken | 字串 | 否 | 識別 JSON 檔案的字串，其中包含剪取的相關中繼資料。 中繼資料可能包含 **clipPoints** 欄位，其中包含 x、y 座標和/或 [userActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity)的陣列。 |
| source | 字串 | 否 | 表示啟動 URI 之來源的自由格式字串。 |
| isTemporary | bool | 否 | 如果設定為 True，則在開啟檔案後，螢幕草圖會嘗試刪除該檔案。 |

下列範例會呼叫 [LaunchUriAsync](/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 方法，以從使用者的應用程式將影像傳送至剪取 & 草圖。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

下列範例說明**screensketch**的**secondarySharedAccessToken**參數所指定的檔案可能包含：

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
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```