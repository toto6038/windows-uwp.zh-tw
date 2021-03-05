---
title: 使用 VS Code) Rust for Windows 的 RSS 讀取程式教學課程 (
description: 撰寫簡單應用程式的逐步解說，此應用程式會從 RSS 摘要下載 blog 文章的標題。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、microsoft、learning rust、windows 上適用于初學者的 rust、使用 vs code 的 rust、rust for windows
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 929c6a016294559a049eb329454cee59bb1c980e
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194478"
---
# <a name="rss-reader-tutorial-rust-for-windows-with-vs-code"></a>使用 VS Code) Rust for Windows 的 RSS 讀取程式教學課程 (

上一個主題介紹 [windows 的 Rust，以及 windows 的](rust-for-windows.md)擁有。

現在讓我們來撰寫簡單的應用程式，以從真正簡單的新聞訂閱 (RSS) 摘要中下載 blog 文章的標題，來試用 Windows Rust。

1. 啟動命令提示字元 (VS、或任何) 的 **X64 Native Tools 命令提示** 字元 `cmd.exe` ，以及要 `cd` 保留 Rust 專案的資料夾。

2. 然後，透過貨物，建立名為 *rss_reader* 的新 Rust 專案，然後 `cd` 加入至專案的新建立資料夾。

   ```console
   cargo new rss_reader
   cd rss_reader
   ```

3. 現在透過 &mdash; 貨物 &mdash; ，我們將建立名為 *bindings* 的新子專案。 如您在下列命令中所見，這個新的專案是一個程式庫，它會做為系結至我們想要呼叫之 Windows Api 的媒體。 在組建階段，系結 *程式庫子* 專案將會 *建立在 Rust (中* ，這是二進位或程式庫) 的詞彙。 我們會從 *rss_reader* 專案中取用該專案，如我們所見。

   ```console
   cargo new --lib bindings
   ```

   建立 *連結區的系* 結表示當我們建立 *rss_reader* 時 *，系* 結將能夠快取我們匯入之任何系結的結果。

4. 然後，在 VS Code 中開啟 *rss_reader* 專案。

   ```console
   code .
   ```

5. 讓我們先處理系結 *程式庫。*

   在 VS Code 的 [Explorer] 中，開啟檔案 `bindings`  >  `Cargo.toml` 。

   ![在建立專案之後，于 VS Code 中開啟貨物點 >gopkg.toml 檔案](../../images/rust-rss-reader-1.png)

   檔案 `Cargo.toml` 是描述 Rust 專案的文字檔，包括它所擁有的任何相依性。

   現在 `dependencies` 區段是空的。 現在，請編輯該區段 (並且將 `[build-dependencies]` 區段) ，讓它看起來像這樣。

   ```toml
   # bindings\Cargo.toml
   ...

   [dependencies]
   windows="0.3.1"

   [build-dependencies]
   windows="0.3.1"
   ```

   我們剛為系結 *程式庫和* 其組建腳本新增了 *windows* 建立元件的相依性。 這可讓貨物下載、建立和快取 Windows 支援作為套件。 將版本號碼設定為任何最新版本， &mdash; 您就可以在 [windows](https://crates.io/crates/windows)建立的網頁上看到該版本號碼。

6. 現在，我們可以加入組建腳本本身;這是我們將會產生最終所依賴之系結的地方。 在 VS Code 中，以滑鼠右鍵 *按一下 [系* 結] 資料夾，然後按一下 [ **新增** 檔案]。 輸入名稱 *build.rs*，然後按 **enter**。 編輯 `build.rs` 外觀如下。

   ```rust
   // bindings\build.rs
   fn main() { 
       windows::build!(windows::web::syndication::SyndicationClient);
   }
   ```

   宏負責解析檔案形式的任何相依性 `windows::build!` `.winmd` ，並直接從中繼資料產生所選類型的系結。 我們 *可能* 會要求 (的整個命名空間， `windows::web::syndication::*` 而不是 `windows::web::syndication::SyndicationClient`) 。 但在這裡，我們會要求只針對 [**SyndicationClient**](/uwp/api/windows.web.syndication.syndicationclient) 類型產生系結。 如此一來，您就可以輕鬆或盡可能地匯入，並避免針對您永遠不需要的專案產生和編譯器代碼。
   
   此外， *build* 宏會自動尋找明確列出的)  (s 類型的所有相依性。 在這裡， **SyndicationClient** 包含的方法需要型別為 [**Uri**](/uwp/api/windows.foundation.uri)的參數。 因此， *build* 宏包含 **Uri** 的定義，因此您可以呼叫該方法。 其他類型是 **windows** 元件本身的一部分。 例如， **windows：： Result** 是由 *windows* 產生者所定義，因此一律可供使用。 您將會發現，您需要從 [**Windows Foundation**](/uwp/api/windows.foundation) 命名空間進行的大部分動作都會自動包含在內。

7. 開啟 `bindings`  >  `src`  >  `lib.rs` 原始程式碼檔。 若要包含在上一個步驟中產生的系結，請使用下列程式碼取代您將在中找到的預設程式碼 `lib.rs` 。

   ```rust
   // bindings\src\lib.rs
   ::windows::include_bindings!();
   ```

   `windows::include_bindings!`宏包含組建腳本在上一個步驟中產生的原始程式碼。 現在，每當您需要存取其他 Api 時，只要在組建腳本中列出它們， (`build.rs`) 。

8. 現在讓我們來執行主要 *rss_reader* 專案。 首先，開啟 `Cargo.toml` 專案根目錄中的檔案，並在內部系結中新增下列相依性。 

   ```toml
   # Cargo.toml
   ...

   [dependencies] 
   bindings = { path = "bindings" }
   ```

9. 最後，開啟 *rss_reader* 專案的 `src`  >  `main.rs` 原始程式碼檔。 有一個簡單的程式碼可輸出 *Hello，world！* 回應。 將此程式碼加入至的開頭 `main.rs` 。

   ```rust
   // src\main.rs
   use bindings::{ 
       windows::foundation::Uri,
       windows::web::syndication::SyndicationClient,
       windows::Result,
   };

   fn main() {
       println!("Hello, world!");
   }
   ```

   宣告會 `use` 縮短我們將使用之類型的路徑。 我們稍早提到的是 **Uri** 型別。 和 **windows：： Result** 可協助我們進行錯誤傳播和簡潔的錯誤處理。

10. 若要建立新的 [**Uri**](/uwp/api/windows.foundation.uri)，請將此程式碼新增至 **main** 函數。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;

       Ok(())
   }
   ```

   請注意，我們使用的是 **windows：： Result** 做為 **main** 函式的傳回型別。 這可讓事情變得更容易，因為處理作業系統 (作業系統) Api 的錯誤很常見。

   您可以在建立 **Uri** 的程式程式碼結尾看到問號運算子。 為了節省輸入，我們要利用 Rust 的錯誤傳播和最少運算邏輯。 這表示我們不需要針對這個簡單的範例，進行一堆手動錯誤處理。

11. 若要下載此 RSS 摘要，我們將建立新的 **SyndicationClient** 物件。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;

       Ok(())
   }
   ```

   **新** 函式的 Rust 等同于預設的函式。

12. 現在，我們可以使用 **SyndicationClient** 物件來取出摘要。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       Ok(())
   }
   ```

由於 [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) 是非同步 API，因此我們可以使用封鎖 **get** 函式 (如上所示) 。 另外，我們也可以在 `await` 函式內使用運算子 `async` ， (以合作方式等候結果) ，就像在 c # 或 c + + 中所做的一樣。

13. 現在我們可以逐一查看結果專案，讓我們只列印出標題。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       for item in feed.items()? {
           println!("{}", item.title()?.text()?);
       }

       Ok(())
   }
   ```

14. 現在讓我們來確認是否可以建立和執行，方法是按一下 [**執行**  >  **執行但不 (調試** 程式]，或按 **Ctrl + F5**) 。 此外，也有內嵌在文字編輯器內的 **Debug** 和 **Run** 命令。

   ![內嵌于文字編輯器中的 Debug 和 Run 命令](../../images/rust-rss-reader-2.png)

   在 [ **終端** 機] 窗格中，您可以看到貨物成功下載和編譯 **windows** 製作者、快取結果，然後使用它們來讓後續組建在較短的時間內完成。 然後，它會建立範例並加以執行，並顯示一份 blog 貼文標題清單。

   ![Blog 貼文標題清單](../../images/rust-rss-reader-3.png)

就像是針對 Windows 編寫 Rust 一樣簡單。 不過，在幕後，有很多人都在建立工具，讓 Rust 可以剖析以 `.winmd` [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/) (通用語言基礎結構的檔案，或在編譯時期使用 CLI) ，同時在執行時間使用以 COM 為基礎的應用程式二進位介面 (ABI) ，同時考慮安全性和效率。

## <a name="related"></a>相關參考

* [適用于 Windows 的 Rust 和 windows 包裝箱](rust-for-windows.md)
* [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/)
