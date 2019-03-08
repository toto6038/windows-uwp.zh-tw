---
title: XboxServices.config
description: 描述關聯的 Xbox Live 設定至您的 UWP 遊戲的 XboxServices.config 檔案。
ms.date: 03/29/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 服務組態、 xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606683"
---
# <a name="xboxservicesconfig-file-description"></a>XboxServices.config 檔案描述

當您開發 Xbox Live 啟用 UWP 遊戲，您的專案必須包含 XboxServices.config 檔案。  此檔案可讓 Xbox Live SDK 以將您的遊戲與應用程式合作夥伴中心和 Xbox Live 服務組態產生關聯。 此檔案包含的 JSON 物件的詳細資料資訊，例如服務的組態 ID、 title 識別碼等。

如果您使用 Unity 來設計使用外掛程式的 Xbox Live 的 Xbox Live 創作者計劃的遊戲，這個檔案會自動為您建立 Xbox Live 關聯精靈。

## <a name="xboxservicesconfig-fields"></a>XboxServices.config fields

>[!NOTE]
> Xbox Live 關聯精靈所建立的這個檔案可包含額外的欄位，如下所述，這項，但它們不會使用服務。

組態檔中的 JSON 物件中，會定義下列欄位：

欄位 | 描述
--- | ---
PrimaryServiceConfigId  |  Xbox Live 服務組態的識別碼 (SCID)。 在[合作夥伴中心](https://partner.microsoft.com/dashboard)，您可以找到此值**Xbox Live**頁面 （針對創作者計劃） 或**Xbox Live 設定**頁面 （適用於完整 Xbox Live 的遊戲），在**Services**一節以取得您的應用程式。
TitleId  |  您的應用程式十進位標題識別碼。 在[合作夥伴中心](https://partner.microsoft.com/dashboard)，您可以找到此值**Xbox Live**頁面 （針對創作者計劃） 或**Xbox Live 設定**頁面 （適用於完整 Xbox Live 的遊戲），在**Services**一節以取得您的應用程式。
XboxLiveCreatorsTitle  |  若為"true"，表示應用程式的 Xbox Live 創作者計劃應用程式。 否則即為"false"。
領域  |  **（選擇性）** 定義的應用程式使用的功能範圍。 如進一步說明，請參閱下方內容。

### <a name="scope-field"></a>範圍 欄位

**範圍**欄位是選擇性欄位，您可以使用來表示您的遊戲所使用的功能。


如果**領域**未指定欄位，則範圍會設定為預設值的值而定，這個值**XboxLiveCreatorsTitle**欄位中下, 表中所述：

XboxLiveCreatorsTitle value | 預設範圍值
--- | ---
"true"  |  "xbl.signin xbl.friends"
"false"  |  "xboxlive.signin"



下列清單描述的有效值**範圍**欄位。

範圍值 | 描述
--- | ---
xbl.signin  | 包含登入的創作者計劃遊戲的功能。 所需的創作者計劃的遊戲。
xbl.friends | 包含的朋友和社交排行榜創作者計劃之遊戲的功能。
xboxlive.signin | 包含登入存取 Xbox Live 的完整功能的遊戲的功能。 非-創作者計劃遊戲的必要項。

目前，若要指定的唯一理由**範圍**欄位，就是您要進行的 Xbox Live 創作者計劃遊戲，並且不需要存取的朋友清單或社交排行榜 （僅限於您的朋友排行榜） 您的遊戲。 如果發生這種情況，您可以將下列一行加入 XboxServices.config 檔案：

```
  "Scope" : "xbl.signin"
```

加入這一行可防止 UWP 應用程式要求存取 friend 清單，當您第一次啟動應用程式的權限。

## <a name="example-xboxservicesconfig-file"></a>範例 XboxServices.config 檔案

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
