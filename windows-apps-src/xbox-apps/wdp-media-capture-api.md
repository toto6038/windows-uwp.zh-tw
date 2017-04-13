---
author: WilliamsJason
title: "媒體擷取 API 參考"
description: "了解如何以程式設計方式存取媒體擷取 API。"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.openlocfilehash: 9236b0cd9ac658a34283e54ba70b7e70d19c6bb3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="media-capture-api-reference"></a>媒體擷取 API 參考 #

**要求**

您可以使用下列要求格式擷取代表目前畫面的 PNG 檔案。

| 方法        | 要求 URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**URI 參數**

您可以在要求 URI 上指定下列其他參數：


| URI 參數      | 說明     | 
| ------------------ |-----------------|
| download (選擇性)| 指出 HTTP 回應標頭是否應該設定為指示主機瀏覽器應該以附件形式下載螢幕擷取畫面 (而不是在瀏覽器中呈現) 的布林值。  |
<br>

**要求標頭**

* 無

**要求主體**

* 無

###<a name="response"></a>回應###

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼   | 描述     | 
| ------------------ |-----------------|
| 200                | 螢幕擷取畫面要求成功且已傳回擷取 |
| 5XX                | 未預期失敗的錯誤代碼 |
<br>

**可用裝置系列**

* Windows Xbox

