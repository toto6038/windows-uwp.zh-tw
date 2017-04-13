---
author: payzer
title: "裝置入口網站 Xbox 開發人員開發套件更新原則 API 參考"
description: "深入了解如何以程式設計方式設定您主機的更新原則。"
ms.openlocfilehash: f9313d3c8b93ba13074c547f1f63c9f3204f0f58
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
注意：此 API 會隨附在下一個開發人員預覽中。

# <a name="system-update-policy-api-reference"></a>系統更新原則 API 參考   
您可以使用這個 API 查看哪個更新原則已套用至您的主機，並將更新原則變更為新的原則。

重要︰嘗試呼叫這個 API 時，大多數的主機都會收到「拒絕存取」回應。 這是因為不是所有開發主機都具備變更其更新原則的能力。

這個 API 會影響處於開發人員模式之主機的更新原則，而非零售主機。

## <a name="get-the-console-update-policy"></a>取得主機更新原則

**要求**

您可以使用下列要求來取得您主機的更新原則。

方法      | 要求 URI
:------     | :-----
GET | /ext/update/policy
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
回應是 JSON 陣列，包含主機的系統更新群組成員資格。 每個物件都有下列欄位︰   

識別碼 - (字串) 更新群組的識別碼。   
易記名稱 - (字串) 更新群組的顯示名稱。   
說明 - (字串) 更新群組的說明。
IsDevKitGroup - (true | false) 指出更新群組是否適用於開發人員組建。
ResourceSetID - (字串) 忽略 - 由系統更新基礎結構所使用。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="set-a-consoles-system-update-policy"></a>設定主機的系統更新原則
您可以使用此 API 切換主機的系統更新群組成員資格。

注意：主機一次只能位於一個系統更新群組中。

**要求**

您可以使用下列要求設定主機的系統更新群組成員資格。

方法      | 要求 URI
:------     | :-----
POST | /ext/update/policy
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   
要求主體是 JSON 物件，包含下列欄位：   
GroupIdToJoin - (字串) 您希望主機加入的系統更新群組識別碼。  
GroupIdToLeave - (字串) 您希望主機離開的系統更新群組識別碼。

所有的欄位都是必要的。

可能的 GroupID 是︰   
* 無更新 - "b2dbed33-2845-44cc-a7a1-4a9afb29d8d9"   
* 最新的實際執行修復 - "7432ae99-8c09-48dd-99f9-a2697499e240"   
* 最新的預覽修復 - "a8153054-1a1b-47cc-acc9-9aed90c1f8db"    

**回應**   

- 無

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox

