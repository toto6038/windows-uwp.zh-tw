---
author: TylerMSFT
title: ms-tonepicker 配置
description: 本主題說明 ms-tonepicker URI 配置，以及如何使用它來顯示音調選擇器以選取音調、儲存音調，以及取得音調的易記名稱。
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 98747ece2160ef25c89dd921e8309d31b6459971
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663658"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>使用 ms-tonepicker URI 配置來選擇及儲存音調

本主題說明如何使用 **ms-tonepicker:** URI 配置。 此 URI 配置可用來：
- 判斷裝置上是否有提供音調選擇器。
- 顯示音調選擇器以列出可用的鈴聲、系統音效、簡訊鈴聲及鬧鐘音效；並取得代表使用者所選音效的音調語彙基元。
- 顯示音調儲存程式，此程式會以音效檔語彙基元作為輸入，然後將它儲存到裝置。 接著，便可透過音調選擇器使用已儲存的音調。 使用者也可以為音調提供易記名稱。
- 將音調傳換成其易記名稱。

## <a name="ms-tonepicker-uri-scheme-reference"></a>ms-tonepicker: URI 配置參考

這個 URI 配置不會透過 URI 配置字串傳遞引數，而是會透過 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) 傳遞引數。 所有字串皆有區分大小寫。

下列各節會指出應該傳遞哪些引數來完成指定的工作。

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>工作：判斷裝置上是否有提供音調選擇器。
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>工作：顯示音調選擇器

您可以傳遞來顯示音調選擇器的引數如下︰

| 參數 | 類型 | 必要 | 可能值 | 描述 |
|-----------|------|----------|-------|-------------|
| 動作 | 字串 | 是 | "PickRingtone" | 開啟音調選擇器。 |
| CurrentToneFilePath | 字串 | 否 | 現有的音調語彙基元。 | 要顯示為音調選擇器中目前音調的音調。 如果未設定此值，預設會選取清單中的第一個音調。<br>嚴格說來，這並不是一個檔案路徑。 您可以從音調選擇器所傳回的 `ToneToken` 值中取得適當的 `CurrenttoneFilePath` 值。  |
| TypeFilter | 字串 | 否 | "Ringtones"、"Notifications"、"Alarms"、"None" | 選取要將哪些音調新增到選擇器。 如果未指定任何篩選，則會顯示所有音調。 |

在 [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) 中傳回的值：

| 傳回值 | 類型 | 可能值 | 描述 |
|--------------|------|-------|-------------|
| Result | Int32 | 0 - 成功。 <br>1 - 已取消。 <br>7 - 無效的參數。 <br>8 - 沒有任何音調符合篩選條件。 <br>255 - 未實作指定的動作。 | 選擇器作業的結果。 |
| ToneToken | 字串 | 所選音調的語彙基元。 <br>如果使用者在選擇器中選取 **\[預設\]**，則此字串為空白。 | 這個語彙基元可以用在快顯通知承載中，或是指派為連絡人的鈴聲或簡訊鈴聲。 只有當 **Result** 為 0 時，才會在 ValueSet 中傳回此參數。 |
| DisplayName | 字串 | 所指定音調的易記名稱。 | 可對使用者顯示來代表所選的音調的字串。 只有當 **Result** 為 0 時，才會在 ValueSet 中傳回此參數。 |


**範例：開啟音調選擇器以便讓使用者選取音調**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>工作：顯示音調儲存程式

您可以傳遞來顯示音調儲存程式的引數如下︰

| 參數 | 類型 | 必要 | 可能值 | 描述 |
|-----------|------|----------|-------|-------------|
| 動作 | 字串 | 是 | "SaveRingtone" | 開啟選擇器以儲存鈴聲。 |
| ToneFileSharingToken | 字串 | 是 | 要儲存之鈴聲檔的 [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx) 檔案共用語彙基元。 | 將特定的音效檔儲存為鈴聲。 支援的檔案內容類型為 mpeg 音訊和 x-ms-wma 音訊。 |
| DisplayName | 字串 | 否 | 所指定音調的易記名稱。 | 設定儲存指定的鈴聲時要使用的顯示名稱。 |

在 [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) 中傳回的值：

| 傳回值 | 類型 | 可能值 | 描述 |
|--------------|------|-------|-------------|
| Result | Int32 | 0 - 成功。<br>1 - 被使用者取消。<br>2 - 無效的檔案。<br>3 - 無效的檔案內容類型。<br>4 - 檔案超出鈴聲大小上限 (在 Windows 10 中為 1 MB)。<br>5 - 檔案超出 40 秒的長度限制。<br>6 - 檔案受數位版權管理保護。<br>7 - 無效的參數。 | 選擇器作業的結果。 |

**範例：將本機音樂檔儲存為鈴聲。**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>工作：將音調傳換成其易記名稱。

您可以傳遞來取得音調易記名稱的引數如下︰

| 參數 | 類型 | 必要 | 可能值 | 描述 |
|-----------|------|----------|-------|-------------|
| 動作 | 字串 | 是 | "GetToneName" | 指出您想要取得音調的易記名稱。 |
| ToneToken | 字串 | 是 | 音調語彙基元 | 要從中取得顯示名稱的音調語彙基元。 |

在 [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) 中傳回的值：

| 傳回值 | 類型 | 可能值 | 描述 |
|--------------|------|-------|-------------|
| Result | Int32 | 0 - 選擇器作業成功。<br>7 - 不正確的參數 (例如未提供任何 ToneToken)。<br>9 - 讀取所指定語彙基元的名稱時發生錯誤。<br>10 - 找不到指定的音調語彙基元。 | 選擇器作業的結果。
| DisplayName | 字串 | 音調的易記名稱。 | 傳回所選音調的顯示名稱。 只有當 **Result** 為 0 時，才會在 ValueSet 中傳回此參數。 |

**範例︰從 Contact.RingToneToken 擷取音調語彙基元，並在連絡人卡片中顯示其易記名稱。**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```
