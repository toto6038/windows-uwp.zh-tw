---
title: 在 UWP 應用程式中使用 MongoDB 資料庫
description: 在 UWP 應用程式中使用 MongoDB 資料庫。
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MongoDB, 資料庫
ms.localizationpriority: medium
ms.openlocfilehash: ea3e630a3bb0b8fc5f7f4168b0b946f115b97446
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "67714001"
---
# <a name="use-a-mongodb-database"></a>使用 MongoDB 資料庫
本文包含從 UWP 應用程式使用 MongoDB 資料庫所需的步驟。 其中也包含小型的程式碼片段，顯示您在程式碼中與資料庫互動的方式。

## <a name="set-up-your-solution"></a>設定您的解決方案

若要將您的應用程式直接連線到 MongoDB 資料庫，請確定您專案的最低版本目標為 Fall Creators Update (Build 16299)。  您可以在 UWP 專案的屬性頁面中找到該資訊。

![Visual Studio 中的 [目標屬性] 窗格影像，其中顯示設定為 Fall Creators Update 的目標和最小版本](images/min-version-fall-creators.png)

開啟 套件管理員主控台  (檢視 -> 其他 Windows-> 套件管理員主控台)。 使用 **Install-Package MongoDB.Driver** 命令來安裝 MongoDB 的驅動程式。 這可讓您以程式設計方式存取 MongoDB 資料庫。

## <a name="test-your-connection-using-sample-code"></a>使用範例程式碼測試您的連線
下列範例程式碼會從遠端 MongoDB 用戶端取得一個集合，然後將新文件新增至該集合。 接著，它會使用 MongoDB API 來擷取新的集合大小以及已插入的文件，並將其列印出來。請注意，IP 位址和資料庫名稱必須加以自訂。

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```
