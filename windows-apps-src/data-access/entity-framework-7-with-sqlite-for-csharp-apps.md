---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) 是一種關聯對應程式，可讓您搭配使用網域特定物件的關聯資料使用。"
title: "針對 C# 應用程式搭配 SQLite 使用 Entity Framework 7"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, SQLite, C#, EF, Entity Framework
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2ab2a12f6c2bc2f0f8853b404afaf13bf80635b7
ms.lasthandoff: 02/07/2017

---

# <a name="entity-framework-core-1-with-sqlite-for-c-apps"></a>針對 C# app 搭配 SQLite 使用 Entity Framework Core 1

\[ 針對 Windows 10 上的 UWP 應用程式更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entity Framework (EF) 是一種關聯對應程式，可讓您搭配使用網域特定物件的關聯資料使用。 本文說明如何在通用 Windows app 中使用 Entity Framework Core 1 搭配 SQLite 資料庫。

Entity Framework Core 1 原本是專供 .NET 開發人員使用，可在通用 Windows 平台 (UWP) 上搭配 SQLite 來儲存和操作使用網域特定物件的關聯資料。 您可以從 .NET 應用程式將 EF 程式碼移轉到 UWP app，並預期它可以搭配連接字串的適當變更使用。

目前 EF 僅支援 UWP 上的 SQLite。 在[開始使用通用 Windows 平台頁面](http://go.microsoft.com/fwlink/p/?LinkId=735013)上，有安裝 Entity Framework Core 1 和建立模型的詳細逐步解說。 其涵蓋下列主題：

-   先決條件
-   建立新專案
-   安裝 Entity Framework
-   建立您的模型
-   建立您的資料庫
-   使用您的模型

