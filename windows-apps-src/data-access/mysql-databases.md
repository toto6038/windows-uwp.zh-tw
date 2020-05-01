---
title: 在 UWP 應用程式中使用 MySQL 資料庫
description: 在 UWP 應用程式中使用 MySQL 資料庫。
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MySQL, 資料庫
ms.localizationpriority: medium
ms.openlocfilehash: bfed9c0a0c4198095b9be48fe71832bdfca67718
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "67713785"
---
# <a name="use-a-mysql-database"></a>使用 MySQL 資料庫
本文包含從 UWP 應用程式使用 MySQL 資料庫所需的步驟。 其中也包含小型的程式碼片段，顯示您在程式碼中與資料庫互動的方式。

## <a name="set-up-your-solution"></a>設定您的解決方案

若要將您的應用程式直接連線到 MySQL 資料庫，請確定您專案的最低版本目標為 Fall Creators Update (Build 16299)。  您可以在 UWP 專案的屬性頁面中找到該資訊。

![Visual Studio 中的 [目標屬性] 窗格影像，其中顯示設定為 Fall Creators Update 的目標和最小版本](images/min-version-fall-creators.png)

開啟 套件管理員主控台  (檢視 -> 其他 Windows-> 套件管理員主控台)。 使用 **Install-Package MySql.Data** 命令來安裝 MySQL 的驅動程式。 這可讓您以程式設計方式存取 MySQL 資料庫。

## <a name="test-your-connection-using-sample-code"></a>使用範例程式碼測試您的連線
以下是連線到遠端 MySQL 資料庫並從中讀取資料的範例。 請注意，IP 位址、認證和資料庫名稱都必須加以自訂。

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```
