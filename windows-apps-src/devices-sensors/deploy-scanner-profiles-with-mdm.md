---
title: 使用 MDM 部署條碼掃描器設定檔
description: 瞭解如何使用 EnterpriseExtFileSystem 設定服務提供者 (CSP) ，將條碼掃描器設定檔部署至行動裝置管理 (MDM) 伺服器。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de3a43386e37c9bb997340c35c8f16977871916d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304720"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署條碼掃描器設定檔

**注意**   這項功能需要 Windows 10 行動裝置版或更新版本。

可以使用 MDM 伺服器來部署條碼掃描器設定檔。 若要部署設定檔，請使用[ENTERPRISEEXTFILESYSTEM CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)中的*OemProfile* ，將它們放入 \\ 資料 \\ SharedData \\ OEM \\ 公用 \\ 設定檔資料夾。 然後驅動程式製造商就可以使用這些掃描器設定檔來設定未透過 API 介面公開的設定。

Microsoft 沒有定義掃描器設定檔的詳細資料或實作方式。

## <a name="related-topics"></a>相關主題
- [EnterpriseExtFileSystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [條碼掃描器裝置支援](./pos-device-support.md#barcode-scanner)