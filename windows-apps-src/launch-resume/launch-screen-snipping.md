---
title: 啟動螢幕剪取
description: 本主題描述 ms screenclip 和 ms screensketch 的 URI 配置。 您的應用程式可以使用這些 URI 配置，即可啟動 剪取 & 草圖應用程式，或開啟新的剪取。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10、 uwp、 uri、 剪取方式、 草圖
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595383"
---
# <a name="launch-screen-snipping"></a>啟動螢幕剪取

**Ms screenclip:** 和**ms screensketch:** URI 配置可以讓您啟動 剪取，或編輯螢幕擷取畫面。

## <a name="open-a-new-snip-from-your-app"></a>從您的應用程式中開啟新的剪取

**Ms screenclip:** URI 可讓您的應用程式會自動開啟，並開始新的剪取。 產生的剪取會複製到使用者的剪貼簿 中，但不是會自動傳回到開啟應用程式。

**ms screenclip:** 採用下列參數：

| 參數 | 類型 | 必要 | 描述 |
| --- | --- | --- | --- |
| 來源 | 字串 | 否 | 自由格式字串，以指出啟動 URI 的來源。 |
| delayInSeconds | 整數 | 否 | 從 1 到 30 的整數值。 指定的延遲，URI 呼叫和 剪取開始時之間的完整秒數。 |
| callbackformat | 字串 | 否 | 無法使用此參數。 |

## <a name="launching-the-snip--sketch-app"></a>啟動剪取 & 草圖應用程式

**Ms screensketch:** URI 可讓您以程式設計方式啟動剪取 & 草圖應用程式，並開啟應用程式隸屬於註釋中的特定映像。

**ms screensketch:** 採用下列參數：

| 參數 | 類型 | 必要 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字串 | 否 | 語彙基元，可識別要剪取 & 草圖應用程式中開啟的檔案。 擷取自[SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)。 如果省略這個參數，則會啟動應用程式，而不需要開啟的檔案。 |
| secondarySharedAccessToken | 字串 | 否 | 剪取的相關中繼資料中識別的 JSON 檔案的字串。 中繼資料可能包含**clipPoints**欄位的 x，y 座標陣列和/或[userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)。 |
| 來源 | 字串 | 否 | 自由格式字串，以指出啟動 URI 的來源。 |
| IsTemporary | bool | 否 | 如果設定為 True，畫面草圖會嘗試刪除檔案之後將它開啟。 |

下列範例會呼叫[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)剪取 & 草圖傳送映像，從使用者的應用程式的方法。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

下列範例會說明哪些所指定的檔案**secondarySharedAccessToken**的參數**ms screensketch**可能包含：

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
