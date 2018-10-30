---
title: 使用 MDM 部署條碼掃描器設定檔
author: PatrickFarley
description: 可以使用 MDM 伺服器來部署條碼掃描器設定檔。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cfd9692620273952483ec7da65a69b643cb5bf4f
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762373"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署條碼掃描器設定檔

**注意：** 此功能需要 windows 10 行動裝置版或更新版本。

可以使用 MDM 伺服器來部署條碼掃描器設定檔。 若要部署設定檔，請使用 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) 中的 *OemProfile* 將這些設定檔置入 \\Data\\SharedData\\OEM\\Public\\Profile 資料夾。 然後驅動程式製造商就可以使用這些掃描器設定檔來設定未透過 API 介面公開的設定。

Microsoft 沒有定義掃描器設定檔的詳細資料或實作方式。

## <a name="related-topics"></a>相關主題
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [條碼掃描器裝置支援](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)