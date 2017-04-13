---
author: mcleblanc
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: "資料存取"
description: "本節討論在私人資料庫的裝置上儲存資料，以及在通用 Windows 平台 (UWP) 應用程式中使用物件關聯對應。"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 資料, 資料庫, 關聯式, 表格, sqlite"
ms.openlocfilehash: 9e5873e7c7c5af9b3d13dcd850e19ff3dfd91dc7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="data-access"></a>資料存取

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節討論在私人資料庫的裝置上儲存資料，以及在通用 Windows 平台 (UWP) app 中使用物件關聯對應。

UWP SDK 中包含 SQLite。 Entity Framework Core 會在 UWP app 中使用 SQLite。 使用這些技術針對離線/間歇連線案例進行開發，以及跨 App 工作階段保存資料。

| 主題 | 描述|
|-------|------------|
| [Entity Framework Core 與 SQLite 搭配用於 C# App](entity-framework-7-with-sqlite-for-csharp-apps.md) | Entity Framework (EF) 為物件關聯式對應程式，可讓您使用網域特定物件處理關聯式資料。 本文說明如何在通用 Windows 應用程式中搭配 SQLite 資料庫來使用 Entity Framework Core。 |
| [SQLite 資料庫](sqlite-databases.md) | SQLite 是無伺服器的內嵌資料庫引擎。 本文說明如何使用包含在 SDK 中的 SQLite 程式庫、在通用 Windows app 中封裝您自己的 SQLite 程式庫，或是從來源建立 SQLite 程式庫。 |
