---
title: 裝置入口網站控制器 API 參考
description: 了解如何取得附加實體控制器數目，並以程式設計方式將它們關閉。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b5061f9193d78d4ff23f5fa707b0bea67a10f98
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929833"
---
# <a name="controller-api-reference"></a>控制器 API 參考   
您可以使用此 REST API 取得附加實體控制器數目，並以程式設計方式將它們關閉。

## <a name="determine-the-number-of-attached-physical-controllers"></a>決定附加實體控制器數目

**要求**

您可以使用下列要求檢查裝置上附加實體控制器的數目。

方法      | 要求 URI
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   

- 無

**回應**   

- 指定附加實體控制器數目的 JSON 數目屬性 ConnectedControllerCount。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>拔除 devkit 上所有的實體控制器

**要求**

您可以使用下列要求，拔除裝置上的所有實體控制器。

方法      | 要求 URI
:------     | :-----
DELETE | /ext/remoteinput/controllers
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   

- 無

**回應**   

- 無 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 中斷連接控制器的要求成功。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox
