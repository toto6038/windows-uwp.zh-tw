---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) 為物件關聯式對應程式，可讓您使用網域特定物件處理關聯式資料。"
title: "Entity Framework Core 與 SQLite 搭配用於 C# App"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, SQLite, C#, EF, Entity Framework
ms.openlocfilehash: 015030774c7d148d3a9be757c80de827987347a9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="entity-framework-core-with-sqlite-for-c-apps"></a>Entity Framework Core 與 SQLite 搭配用於 C# App

\[ 已更新 Windows10 上的 UWP app。 如需 Windows8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entity Framework (EF) 是一種關聯對應程式，可讓您搭配使用網域特定物件的關聯資料使用。 本文說明如何在通用 Windows 應用程式中搭配 SQLite 資料庫來使用 Entity Framework Core。

原本專供 .NET 開發人員使用的 Entity Framework Core 可以透過網域特定物件，在通用 Windows 平台 (UWP) 上搭配 SQLite 用來儲存和操作關聯式資料。 您可以從 .NET 應用程式將 EF 程式碼移轉到 UWP app，並預期它可以搭配連接字串的適當變更使用。

目前 EF 僅支援 UWP 上的 SQLite。 在[開始使用通用 Windows 平台頁面](http://go.microsoft.com/fwlink/p/?LinkId=735013)上，有安裝 Entity Framework Core 和建立模型的詳細逐步解說。 其涵蓋下列主題：

-   先決條件
-   建立新專案
-   安裝 Entity Framework
-   建立您的模型
-   建立您的資料庫
-   使用您的模型
