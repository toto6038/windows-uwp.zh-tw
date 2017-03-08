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
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0f7aff1c9e7354fcf92d24ed8acb88b41a23d377
ms.lasthandoff: 02/07/2017

---
# <a name="data-access"></a>資料存取

\[ 針對 Windows 10 上的 UWP 應用程式更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節討論在私人資料庫的裝置上儲存資料，以及在通用 Windows 平台 (UWP) app 中使用物件關聯對應。

UWP SDK 中包含 SQLite。 Entity Framework 7 可搭配 UWP app 中的 SQLite 使用。 使用這些技術針對離線/間歇性的連線案例開發，以及保存 app 工作階段的資料。

| 主題 | 說明|
|-------|------------|
| [針對 C# app 搭配 SQLite 使用 Entity Framework 7](entity-framework-7-with-sqlite-for-csharp-apps.md) | Entity Framework (EF) 是一種關聯對應程式，可讓您搭配使用網域特定物件的關聯資料使用。 本文說明如何在通用 Windows app 中使用 Entity Framework 7 搭配 SQLite 資料庫。 |
| [SQLite 資料庫](sqlite-databases.md) | SQLite 是無伺服器的內嵌資料庫引擎。 本文說明如何使用包含在 SDK 中的 SQLite 程式庫、在通用 Windows app 中封裝您自己的 SQLite 程式庫，或是從來源建立 SQLite 程式庫。 |

