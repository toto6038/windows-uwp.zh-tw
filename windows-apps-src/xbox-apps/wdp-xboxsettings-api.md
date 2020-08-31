---
title: 裝置入口網站 Xbox 開發人員設定 API 參考
description: 瞭解如何使用 Xbox 裝置入口網站 REST API 存取適用于開發的 Xbox One 設定。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 0aceb7afdce9cc76eab3ee330f0018fdc7ccd1bb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168982"
---
# <a name="developer-settings-api-reference"></a>開發人員設定 API 參考

您可以使用這個 API 來存取適合用於開發的 Xbox One 設定。

## <a name="get-all-developer-settings-at-once"></a>同時取得所有的開發人員設定

**要求**

您可以使用下列要求，以透過單一要求取得所有開發人員設定。

方法      | 要求 URI
:------     | :-----
GET | /ext/settings

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**   
回應是包含所有設定的 Settings JSON 陣列。 每個設定物件包含下列欄位：

* Name - (字串) 設定的名稱。
* Value - (字串) 設定的值。
* RequiresReboot - ("Yes" | "No") 這個欄位會指示設定是否需要重新開機才會生效。
* Disabled - ("Yes" | "No") 這個欄位會指示設定是否已停用，無法編輯。
* Category - (字串) 設定的類別。
* Type - ("Text" | "Number" | "Bool" | "Select") 這個欄位指示是設定的類型：文字輸入、布林值 ("true" 或 "false")、有最小值和最大值的數字，或有特定值清單的 select。

如果設定為數字：

* 最小值- (數位) 此欄位表示設定的最小數值。
* 最大值- (數位) 此欄位表示設定的最大數值。

如果設定為 select：

* OptionsVariable- ( "Yes" |"No" ) 此欄位指出如果有效的選項不需要重新開機，是否可以變更設定選項是否為變數。
* Options - JSON 陣列，包含有效 select 選項 (為字串)。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="get-settings-one-at-a-time"></a>一次取得一個設定

設定也可以個別擷取。

**要求**

您可以使用下列要求來取得個別設定的相關資訊。

方法      | 要求 URI
:------     | :-----
GET | /ext/settings/\<setting name\>

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**   
回應是 JSON 物件，包含下列欄位：

* Name - (字串) 設定的名稱。
* Value - (字串) 設定的值。
* RequiresReboot - ("Yes" | "No") 這個欄位會指示設定是否需要重新開機才會生效。
* Disabled - ("Yes" | "No") 這個欄位會指示設定是否已停用，無法編輯。
* Category - (字串) 設定的類別。
* Type - ("Text" | "Number" | "Bool" | "Select") 這個欄位指示是設定的類型：文字輸入、布林值 ("true" 或 "false")、有最小值和最大值的數字，或有特定值清單的 select。

如果設定為數字：

* 最小值- (數位) 此欄位表示設定的最小數值。
* 最大值- (數位) 此欄位表示設定的最大數值。

如果設定為 select：

* OptionsVariable- ( "Yes" |"No" ) 此欄位指出如果有效的選項不需要重新開機，是否可以變更設定選項是否為變數。
* Options - JSON 陣列，包含有效 select 選項 (為字串)。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="set-the-value-of-a-setting"></a>設定設定的值

您可以設定設定的值。

**要求**

您可以使用下列要求設定設定的值。

方法      | 要求 URI
:------     | :-----
PUT | /ext/settings/\<setting name\>

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**   
要求主體是 JSON 物件，包含下列欄位：   
Value - (字串) 設定的新值。

**回應**   

- 無

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

**可用裝置系列**

* Windows Xbox