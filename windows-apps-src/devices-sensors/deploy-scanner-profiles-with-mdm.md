---
title: 使用 MDM 部署條碼掃描器設定檔
description: 可以使用 MDM 伺服器來部署條碼掃描器設定檔。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dbcaa683e2c7a2bb18d88fcba03e10fa951d4459
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626153"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署條碼掃描器設定檔

**附註**  這項功能需要 Windows 10 行動裝置版或更新版本。

可以使用 MDM 伺服器來部署條碼掃描器設定檔。 若要部署設定檔，使用*OemProfile*中[EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)放置到\\資料\\SharedData\\OEM\\公用\\設定檔資料夾。 然後驅動程式製造商就可以使用這些掃描器設定檔來設定未透過 API 介面公開的設定。

Microsoft 沒有定義掃描器設定檔的詳細資料或實作方式。

## <a name="related-topics"></a>相關主題
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [條碼掃描器的裝置支援](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)