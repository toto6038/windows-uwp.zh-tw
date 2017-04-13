---
author: payzer
title: "裝置入口網站 Xbox 開發人員設定 API 參考"
description: "了解如何存取 Xbox 開發人員設定。"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.openlocfilehash: 43e4bb289d12439bbc0f6de347d187b067288d51
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developer-settings-api-reference"></a>開發人員設定 API 參考   
您可以使用這個 API 來存取適合用於開發的 Xbox One 設定。

## <a name="get-all-developer-settings-at-once"></a>同時取得所有的開發人員設定

**要求**

您可以使用下列要求，以透過單一要求取得所有開發人員設定。

方法      | 要求 URI
:------     | :-----
GET | /ext/settings
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
回應是包含所有設定的 Settings JSON 陣列。 每個設定物件包含下列欄位：   

Name - (字串) 設定的名稱。   
Value - (字串) 設定的值。   
RequiresReboot - ("Yes" | "No") 這個欄位會指示設定是否需要重新開機才會生效。
Category - (字串) 設定的類別

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
GET | /ext/settings/\&lt;設定名稱\&gt;
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
回應是 JSON 物件，包含下列欄位：   

Name - (字串) 設定的名稱。   
Value - (字串) 設定的值。   
RequiresReboot - ("Yes" | "No") 這個欄位會指示設定是否需要重新開機才會生效。
Category - (字串) 設定的類別

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
PUT | /ext/settings/\&lt;設定名稱\&gt;
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   
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

<br />
**可用裝置系列**

* Windows Xbox

