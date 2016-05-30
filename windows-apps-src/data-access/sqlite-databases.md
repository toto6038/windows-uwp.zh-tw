---
author: mcleblanc
ms.assetid: 5A47301A-2291-4FC8-8BA7-55DB2A5C653F
title: SQLite 資料庫
description: SQLite 是無伺服器的內嵌資料庫引擎。 本文說明如何使用包含在 SDK 中的 SQLite 資料庫、在通用 Windows app 中封裝您自己的 SQLite 資料庫，或是從來源建立 SQLite 資料庫。
---
# SQLite 資料庫

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


SQLite 是無伺服器的內嵌資料庫引擎。 本文說明如何使用包含在 SDK 中的 SQLite 程式庫、在通用 Windows app 中封裝您自己的 SQLite 程式庫，或是從來源建立 SQLite 程式庫。

## SQLite 是什麼，使用的時機為何

SQLite 是開放原始碼、內嵌的無伺服器資料庫。 多年來，它已運用在許多平台和裝置上的資料儲存區的主要裝置端技術。 通用 Windows 平台 (UWP) 支援所有 Windows 10 裝置系列的本機儲存體適用的 SQLite 並建議使用。

SQLite 最適合 Windows 10 IoT 核心版 (IoT 核心版) 的手機 app 和內嵌應用程式，並且做為企業關聯式資料庫伺服器 (RDBS) 資料的快取。 它將滿足大部分的本機資料存取需求 (除非它們伴隨大量的並行寫入或不太可能發生在多數 app 的大型資料規模)。

在媒體播放與遊戲應用程式中，SQLite 也可當做一種檔案格式使用，以儲存可從 Web 伺服器依原狀下載的目錄或其他資產 (例如遊戲關卡)。

## 新增 SQLite 到 UWP 應用程式專案

有三種方法可以將 SQLite 新增到 UWP 專案。

1.  [使用 SDK SQLite](#using-the-sdk-sqlite)
2.  [在 app 套件中包含 SQLite](#including-sqlite-in-the-app-package)
3.  [從 Visual Studio 中的來源建置 SQLite](#building-sqlite-from-source-in-visual-studio)

### 使用 SDK SQLite

您可能想要使用包含在 UWP SDK 中的 SQLite 程式庫減少應用程式套件的大小，並依賴平台定期更新程式庫。 使用 SDK SQLite 可能也會提高效能，例如，若 SQLite 程式庫很可能已載入記憶體供系統元件使用，便可以更快啟動。

若要參考 SDK SQLite，請在專案中加入下列標頭。 標頭也包含平台中支援的 SQLite 版本。

`#include <winsqlite/winsqlite3.h>`

設定專案以連結到 winsqlite3.lib。 在 [方案總管]**** 中，以滑鼠右鍵按一下您的專案並選取 [屬性]****&gt;[連結器]****&gt;[輸入]****，然後將 winsqlite3.lib 新增到 [其他相依性]****。

### 2. 在應用程式套件中包含 SQLite

有時候，您可能想要封裝您自己的程式庫而不是使用 SDK 版本，例如，您可能想在您的跨平台用戶端使用特定版本的程式庫，該版本與 SDK 中包含的 SQLite 版本並不同。

從 SQLite.org 在通用 Windows 平台 Visual Studio 擴充功能安裝 SQLite 程式庫，或透過擴充功能和更新工具安裝 SQLite。

![擴充功能和更新畫面](./images/extensions-and-updates.png)

安裝擴充功能之後，在您的程式碼中參考下列標頭檔案。

`#include <sqlite3.h>`

### 3. 從 Visual Studio 中的來源建置 SQLite

有時候，您可能想編譯您自己的 SQLite 二進位檔以使用[各種編譯器選項](http://www.sqlite.org/compile.html)縮小檔案大小、調整程式庫效能，或為您的應用程式量身打造功能集。 SQLite 提供的選項有平台設定、設定預設參數值、設定大小限制、控制操作特性、啟用一般為關閉的功能、停用一般為開啟的功能、省略功能、啟用分析與偵錯及管理 Windows 上的記憶體配置行為。

*新增來源至 Visual Studio 專案*

SQLite 的原始程式碼可至 [SQLite.org 下載頁面](https://www.sqlite.org/download.html)下載。 將這個檔案新增到您想要在其中使用 SQLite 的應用程式的 Visual Studio 專案。

*設定 Preprocessors*

除了其他任何[編譯時間選項](http://www.sqlite.org/compile.html)之外，請一律使用 SQLITE_OS_WINRT 與 SQLITE\_API=\_\_declspec(dllexport)。

![SQLite 屬性頁畫面](./images/property-pages.png)

## 管理 SQLite 資料庫

使用 SQLite C API 可以建立、更新和刪除 SQLite 資料庫。 在 SQLite.org [SQLite C/C++ 介面簡介](http://www.sqlite.org/cintro.html) (英文) 頁面可以找到 SQLite C API 的詳細資訊。

若要充分了解 SQLite 的運作方式，從用來評估 SQL 陳述式的 SQL 資料庫的主要工作往回工作。 您必須記住兩個目標：

-   [資料庫連線控制代碼](https://www.sqlite.org/c3ref/sqlite3.html)
-   [準備好的陳述式物件](https://www.sqlite.org/c3ref/stmt.html)

有六個介面可用來在這些物件上執行資料庫作業：

-   [sqlite3\_open()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/open.html)
-   [sqlite3\_prepare()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/prepare.html)
-   [sqlite3\_step()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/step.html)
-   [sqlite3\_column()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/column_blob.html)
-   [sqlite3\_finalize()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/finalize.html)
-   [sqlite3\_close()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/close.html)

 

 






<!--HONumber=May16_HO2-->


