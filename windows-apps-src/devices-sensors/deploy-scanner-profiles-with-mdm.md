---
title: "使用 MDM 部署條碼掃描器設定檔"
author: PatrickFarley
description: "可以使用 MDM 伺服器來部署條碼掃描器設定檔。"
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: a63a09e64b6e2b935963a3f49ed7cbc6b82bdcef
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2017
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署條碼掃描器設定檔

**注意**：此功能需要 Windows 10 行動裝置版或更新版本。

可以使用 MDM 伺服器來部署條碼掃描器設定檔。 若要部署設定檔，請使用 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) 中的 *OemProfile* 將這些設定檔置入 \\Data\\SharedData\\OEM\\Public\\Profile 資料夾。 然後驅動程式製造商就可以使用這些掃描器設定檔來設定未透過 API 介面公開的設定。

Microsoft 沒有定義掃描器設定檔的詳細資料或實作方式。

## <a name="related-topics"></a>相關主題
[條碼掃描器](barcode-scanner.md)

[EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)