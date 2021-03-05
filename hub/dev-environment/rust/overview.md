---
title: 使用 Rust 在 Windows 上進行開發的總覽
description: 針對有興趣使用 Rust 在 Windows 上進行開發的初學者概述。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、microsoft、learning rust、windows 上適用于初學者的 rust、使用 vs code 的 rust
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 7d3d2cbe392fe54a82af982a23b866a745e993ae
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194479"
---
# <a name="overview-of-developing-on-windows-with-rust"></a>使用 Rust 在 Windows 上進行開發的總覽

開始使用 [Rust](https://www.rust-lang.org/)並不難。 如果您是想要使用 Windows 10 學習 Rust 的新手，建議您遵循本逐步指南的每一項詳細資料。 它會顯示要安裝的內容，以及如何設定您的開發環境。

> [!TIP]
> 如果您已經在 Rust 上銷售，而且您已設定好 Rust 環境，而且您只想要開始呼叫 Windows Api，則可隨時跳至 windows 的 [Rust，以及 Windows 安裝程式](rust-for-windows.md) 主題。

## <a name="what-is-rust"></a>什麼是 Rust？

Rust 是一種系統程式設計語言，因此用來撰寫 (的系統，例如作業系統) 。 但它也可以用於效能和可信度很重要的應用程式。 Rust 語言語法相當於 c + + 的功能，可提供與新式 c + + 相當的效能，而針對許多經驗豐富的開發人員，Rust 在編譯和執行時間模型、類型系統和決定性的最終情況下，會遇到所有正確的注意事項。

此外，Rust 的設計是圍繞保證記憶體安全的保證，而不需要垃圾收集。

那麼，為什麼我們選擇 Rust 來取得最新的 Windows 語言投影？ 其中一個因素是，堆疊溢 [位的年度開發人員問卷](https://insights.stackoverflow.com/survey) 將 Rust 視為最愛的程式設計語言，在年度之後。 雖然您可能會發現語言有一段陡的學習曲線，但在 hump 後，就不會落在愛。

此外，Microsoft 是 [Rust Foundation](https://foundation.rust-lang.org/)的成立成員。 基礎是一種獨立的非收益組織，具有新的方法來維持和成長大型、participatory 的開放原始碼生態系統。

## <a name="the-pieces-of-the-rust-development-toolsetecosystem"></a>Rust 開發工具組/生態系統的各項

在本節中，我們將介紹一些 Rust 工具和詞彙。 您可以回頭回頭重新整理任何描述。

* 建立 *器是編譯和連結的 Rust* 單位。 建立者可以存在於原始程式碼表單中，並從該處以二進位 excutable 的形式，將它處理成二進位檔的建立者，以提供簡短) 的二進位 (*二進位* 檔，或二進位程式庫 (連結 *庫* 以進行簡短的) 。
* Rust 專案稱為 *封裝*。 封裝包含一或多個建立，以及 `Cargo.toml` 描述如何建立這些建立的檔案。
* `rustup` 是 Rust 工具鏈的安裝程式和更新程式。
* *貨物* 是 Rust 封裝管理工具的名稱。
* `rustc` 是 Rust 的編譯器。 大部分的情況下，您不會直接叫用，而 `rustc` 是透過貨物間接叫用。
* *crates.io* (`https://crates.io/`) 是 Rust 社區的包裝箱登錄。

## <a name="setting-up-your-development-environment"></a>設定開發環境

在下一個主題中，我們將瞭解如何 [在 Windows 上設定開發環境以進行 Rust](setup.md)。

## <a name="related"></a>相關參考

* [Rust 網站](https://www.rust-lang.org/)
* [適用于 Windows 的 Rust 和 windows 包裝箱](rust-for-windows.md)
* [Stack 溢位年度開發人員問卷](https://insights.stackoverflow.com/survey)
* [Rust 基礎](https://foundation.rust-lang.org/)
* [crates.io](https://crates.io/)
* [在 Windows 上設定開發環境以進行 Rust](setup.md)
