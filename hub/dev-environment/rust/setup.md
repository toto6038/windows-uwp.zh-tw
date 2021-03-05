---
title: 在 Windows 上設定開發環境以進行 Rust
description: 為想要使用 Rust 在 Windows 上進行開發的初學者，設定您的開發環境。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、microsoft、learning rust、windows 上適用于初學者的 rust、使用 vs code 的 rust
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 5aac8dd9b9f760f6e1ed49ff0246e44c400d72c4
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194480"
---
# <a name="set-up-your-dev-environment-on-windows-for-rust"></a>在 Windows 上設定開發環境以進行 Rust

在 [使用 Rust 主題開發 Windows 的簡介](overview.md) 中，我們引進了 Rust，並討論它是什麼，以及其主要的移動元件。 在本主題中，我們將設定開發環境。

建議您在 Windows 上進行 Rust 開發。 但是，如果您打算在 Linux 上進行本機編譯和測試，則在適用于 Linux 的 Windows 子系統上使用 Rust 進行開發 [ (WSL) ](/windows/wsl/about) 也是選項。

## <a name="install-visual-studio-recommended-or-the-microsoft-c-build-tools"></a>安裝 Visual Studio (建議的) 或 Microsoft c + + Build Tools

在 Windows 上，Rust 需要特定的 c + + build tools。

您可以下載 [Microsoft c + + Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)，或 (建議的) 您可能只想要安裝 [microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)。

> [!NOTE]
> 我們將使用 Visual Studio Code 作為 Rust 的整合式開發環境 (IDE) ，而不是 Visual Studio。 但是您仍然可以安裝 Visual Studio，而不需要支付費用。 有提供 &mdash; 免費的學生、開放原始碼參與者及個人版的社區版。

安裝 Visual Studio 時，有數個 Windows 工作負載建議您選取 &mdash; **.net 桌面開發**、**使用 c + + 進行桌面開發**，以及 **通用 Windows 平臺開發**。 您可能不認為您需要全部三個，但有可能是因為在需要時才會發生某些相依性，所以我們認為這三個都是比較簡單的選擇。

新的 Rust 專案預設為使用 Git。 此外，也請將 **適用于 Windows** 的個別元件 Git 新增至混合 (使用 [搜尋] 方塊依名稱) 搜尋。

![.NET 桌面開發、使用 c + + 的桌面開發和通用 Windows 平臺開發](../../images/rust-vs-workloads.png)

## <a name="install-rust"></a>安裝 Rust

接下來， [從 Rust 網站安裝 Rust](https://www.rust-lang.org/tools/install)。 網站會偵測到您正在執行 Windows，並提供您 Windows 的64和32位安裝程式，以及將 `rustup` Rust 安裝至 [適用于 Linux 的 Windows 子系統 (WSL) ](/windows/wsl/about)的指示。

> [!TIP]
> Rust 在 Windows 上的運作方式很好;因此，您不需要前往 WSL route (，除非您打算在 Linux) 上進行本機編譯和測試。 由於您有 Windows，建議您只執行 `rustup` 64 位 windows 的安裝程式。 然後您就可以使用 Rust 來撰寫 *適用于 Windows 的* 應用程式。

當 Rust 安裝程式完成時，您就可以開始使用 Rust 進行程式設計。 您還沒有方便的 IDE (我們會在下一節 &mdash; [安裝 Visual Studio Code](#install-visual-studio-code)) 。 您還沒有設定呼叫 Windows Api。 但是，您可以 (VS 或任何) 的 **X64 Native Tools 命令提示** 字元中啟動命令提示字元 `cmd.exe` ，而且可能會發出命令 `cargo --version` 。 如果您看到列印的版本號碼，則會確認 Rust 已正確安裝。

如果您想知道 `cargo` 上述關鍵字的用法， *貨物* 是 Rust 開發環境中的工具名稱，可管理和建立您的專案 (更正確、 *封裝*) 及其相依性。

如果您真的想要深入探討一些程式設計 (即使沒有 IDE) 的便利性，也可以閱讀 [Hello，World！](https://doc.rust-lang.org/book/ch01-02-hello-world.html) Rust 網站上的 Rust 程式設計語言書籍章節。

## <a name="install-visual-studio-code"></a>安裝 Visual Studio Code

藉由使用 Visual Studio Code (VS Code) 做為您的文字編輯器/整合式開發環境 (IDE) ，您可以利用語言服務，例如程式碼完成、語法醒目提示、格式設定和偵錯工具。

VS Code 也包含 [內建終端](https://code.visualstudio.com/docs/editor/integrated-terminal) 機，可讓您發出命令列引數 (來發出命令給貨物，例如) 。

1. 首先，請下載並安裝 [適用于 Windows 的 Visual Studio Code](https://code.visualstudio.com)。

2. 安裝 VS Code 之後，請安裝 **rust 分析器***擴充* 功能。 您可以 [從 Visual Studio Marketplace 安裝 rust 分析器擴充功能](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)，也可以開啟 VS Code，並在 [延伸模組] 功能表中搜尋 **rust-analyzer** (Ctrl + Shift + X) 。

3. 如需偵錯工具支援，請安裝 **CodeLLDB** 延伸模組。 您可以 [從 Visual Studio Marketplace 安裝 CodeLLDB 延伸](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)模組，也可以開啟 VS Code，並在 [延伸模組] 功能表中搜尋 **CodeLLDB** (Ctrl + Shift + X) 。

   > [!NOTE]
   > **CodeLLDB** 延伸模組支援的替代方法是 Microsoft **C/c + +** 擴充功能。 **C/c + +** 擴充功能與 **CodeLLDB** 所做的一樣，也不會與 IDE 整合。 但是 **C/c + +** 延伸模組提供了絕佳的調試資訊。 因此，您可能會想要在需要的情況下使用。
   >
   > 您可以 [從 Visual Studio Marketplace 安裝 C/c + + 延伸](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)模組，也可以開啟 VS Code，並在 [延伸模組] 功能表中搜尋 **C/c + +** (Ctrl + Shift + X) 。

4. 如果您想要在 VS Code 中開啟終端機，請選取 [**視圖**  >  **終端** 機]，或使用 [倒引號] 字元) 中的快捷方式 **Ctrl + '** (。 預設終端機為 PowerShell。

## <a name="hello-world-tutorial-rust-with-vs-code"></a>Hello, world! 使用 VS Code (Rust 的教學課程) 

讓我們使用簡單的「Hello，world！」來 Rust 旋轉 應用程式。

1. 首先， (VS、或任何) 的 **X64 Native Tools 命令提示** 字元，以及要 `cmd.exe` `cd` 保留 Rust 專案的資料夾中，啟動命令提示字元。

2. 然後使用下列命令，要求貨物為您建立新的 Rust 專案。

   ```console
   cargo new first_rust_project
   ```

   您傳遞給命令的引數 `cargo new` 是您想要貨物建立的專案名稱。 在這裡，專案名稱是 *first_rust_project*。 建議您使用蛇案例來命名您的 Rust 專案， (其中的單字為小寫，而且每個空格都會取代為底線) 。

   貨物會使用您提供的名稱為您建立專案。 事實上，貨物的新專案包含一個非常簡單的應用程式的原始程式碼，可輸出 *Hello，world！* 訊息，我們將會看到。 除了建立 *first_rust_project* 專案之外，貨物還建立了一個名為 *first_rust_project* 的資料夾，並將專案的原始程式碼檔案放在那裡。

3. 現在 `cd` 到該資料夾，然後從該資料夾的內容中啟動 VS Code。

   ```console
   cd first_rust_project
   code .
   ```

4. 在 VS Code 的 Explorer 中開啟檔案，此檔案 `src`  >  `main.rs` 是 Rust 原始程式碼檔，其中包含應用程式的進入點 (名為 **main**) 的函式。 它看起來像下面這樣。

   ```rust
   // main.rs
   fn main() {
     println!("Hello, world!");
   }
   ```

   > [!NOTE]
   > 當您在 VS Code 中開啟第一個檔案時 `.rs` ，您會收到通知，指出未安裝某些 Rust 元件，並詢問您是否要安裝這些元件。 按一下 **[是]**，VS Code 將會安裝 Rust 語言伺服器。

   您可以從 glancing 中的程式碼來分辨，那就 `main.rs` 是函式定義，而它會列印字串 "Hello，world！"。  如需有關此語法的詳細資訊，請參閱 Rust 網站上的 [Rust 程式剖析](https://doc.rust-lang.org/book/ch01-02-hello-world.html#anatomy-of-a-rust-program) 。

5. 現在，讓我們試著在偵錯工具下執行應用程式。 將中斷點放在第2行，然後按一下 [**執行**  >  **開始調試** 程式] (或按 **F5**) 。 此外，也有內嵌在文字編輯器內的 **Debug** 和 **Run** 命令。

   > [!NOTE]
   > 當您第一次在偵錯工具下執行應用程式時，您會看到一個對話方塊，指出「無法啟動偵錯工具，因為未提供任何啟動設定」。 按一下 **[確定]** 以查看第二個對話方塊，指出在此工作區中偵測到 [貨物. >gopkg.toml]。 您要產生其目標的啟動設定嗎？」。 按一下 [是]  。 然後關閉檔案上的 launch.js，然後再次開始重新調試。

6. 如您所見，偵錯工具會在第2行中斷。 按 **F5** 繼續，應用程式就會執行到完成。 在 **終端** 機窗格中，您會看到預期的輸出 "Hello，world！"。

## <a name="rust-for-windows"></a>適用于 Windows 的 Rust

您不僅可以 *在 windows 上* 使用 Rust，也可以使用 Rust 撰寫 *適用于 windows 的* 應用程式。 您可以透過 *windows* 包裝箱來呼叫過去、目前和未來的任何 windows API。 在 Rust for Windows 中還有更多關於該程式碼範例和程式碼範例的詳細資料， [以及 windows 包裝箱](rust-for-windows.md) 主題。

## <a name="related"></a>相關參考

* [適用于 Windows 的 Rust 和 windows 包裝箱](rust-for-windows.md)
* [適用於 Linux 的 Windows 子系統 (WSL)](/windows/wsl/about)
* [Microsoft c + + Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
* [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [適用于 Windows 的 Visual Studio Code](https://code.visualstudio.com)
* [rust-分析器擴充功能](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)
* [CodeLLDB 延伸模組](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
* [C/C++ 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
