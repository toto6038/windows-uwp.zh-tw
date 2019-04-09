---
title: 裝置入口網站 Xbox 開發人員設定 API 參考
description: 了解如何存取 Xbox 開發人員設定。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 54a15be26adf0da97105f15f3a44f26ee7bfc96d
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240036"
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

- None

**要求標頭**

- None

**要求本文**

- None

**回應**   
回應是包含所有設定的 Settings JSON 陣列。 每個設定物件包含下列欄位：

* Name - (字串) 設定的名稱。
* Value - (字串) 設定的值。
* RequiresReboot - ("Yes" | "No") 這個欄位會指示設定是否需要重新開機才會生效。
* Disabled - ("Yes" | "No") 這個欄位會指示設定是否已停用，無法編輯。
* Category - (字串) 設定的類別。
* Type - ("Text" | "Number" | "Bool" | "Select") 這個欄位指示是設定的類型：文字輸入、布林值 ("true" 或 "false")、有最小值和最大值的數字，或有特定值清單的 select。

如果設定的數字：

* 最小值為 （數字） 此欄位表示設定的最小數值。
* 最大-（數字） 此欄位代表設定的最大的數字值。

如果設定為選取項目：

* OptionsVariable-("Yes"|[否]） 這個欄位會指示設定選項是否為變數，如果有效的選項可以變更而重新開機。
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
GET | /ext/settings/\<設定名稱\>

**URI 參數**

- None

**要求標頭**

- None

**要求本文**

- None

**回應**   
回應是 JSON 物件，包含下列欄位：

* Name - (字串) 設定的名稱。
* Value - (字串) 設定的值。
* RequiresReboot - ("Yes" | "No") 這個欄位會指示設定是否需要重新開機才會生效。
* Disabled - ("Yes" | "No") 這個欄位會指示設定是否已停用，無法編輯。
* Category - (字串) 設定的類別。
* Type - ("Text" | "Number" | "Bool" | "Select") 這個欄位指示是設定的類型：文字輸入、布林值 ("true" 或 "false")、有最小值和最大值的數字，或有特定值清單的 select。

如果設定的數字：

* 最小值為 （數字） 此欄位表示設定的最小數值。
* 最大-（數字） 此欄位代表設定的最大的數字值。

如果設定為選取項目：

* OptionsVariable-("Yes"|[否]） 這個欄位會指示設定選項是否為變數，如果有效的選項可以變更而重新開機。
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
PUT | /ext/settings/\<設定名稱\>

**URI 參數**

- None

**要求標頭**

- None

**要求本文**   
要求主體是 JSON 物件，包含下列欄位：   
Value - (字串) 設定的新值。

**回應**   

- None

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

**可用裝置系列**

* Windows Xbox