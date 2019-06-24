---
title: 相機條碼掃描器設定
description: 相機條碼掃描器啟用或停用
ms.date: 4/8/2019
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 7db48b610d5bdf256d6a44d4786b8642d984c0b0
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321541"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>啟用或停用隨附於 Windows 10 的軟體解碼器

在 Windows 10 版本 1803 中，安裝有軟體解碼器並且預設為啟用。  如果您不想使用相機條碼掃描器，或者如果您已經取得協力廠商的解碼器搭配 Windows.Devices.PointOfService.BarcodeScanner API，並且兩者都不想使用，您可以停用 Windows 隨附的軟體解碼器。

## <a name="enable-or-disable-using-the-system-registry"></a>使用系統登錄啟用或停用

隨附於 Windows 的軟體解碼器可以透過系統登錄啟用或停用，只要在 *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* 下新增登錄機碼 *InboxDecoder*，以及如以下所述設定 *Enable* 值。

| 值名稱  | 值類型 | 值 | 狀態 |
| ----------- | --------- | -------|--------|
| 啟用      | DWORD     | 1 (預設值)<br/>0 |  啟用隨附於 Windows 的軟體解碼器 <br/> 停用隨附於 Windows 的軟體解碼器 |

以下是範例登錄檔案，您可以用來**停用**隨附於 Windows 的軟體解碼器：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

使用此範例登錄檔來**啟用**隨附於 Windows 的軟體解碼器：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> 如果您未正確修改登錄，可能會發生嚴重問題。  若要增加保護，請在修改前備份登錄。  之後如果發生問題，您還可以還原登錄。  如需如何備份和還原登錄的詳細資訊，請按一下以下文章編號，以檢視 Microsoft 知識庫中的文章： <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows)如何備份及還原 Windows 中的登錄。

> [!NOTE]
> 內建於 Windows 10 的軟體解碼器由 [**Digimarc Corporation**](https://www.digimarc.com/) 提供。
