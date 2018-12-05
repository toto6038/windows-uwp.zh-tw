---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: 建立 "Hello, world" 應用程式 (JS)
description: 本教學課程會教您如何使用 JavaScript 和 HTML 來建立簡單與 \#0034;Hello，world & \#0034;目標為 windows 10 上通用 Windows 平台 (UWP) 應用程式。
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c5b99c95167940c1ae51dbe96a3e43dc6fb0af34
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8701804"
---
# <a name="create-a-hello-world-app-js"></a>建立 "Hello, world" 應用程式 (JS)

本教學課程會教您如何使用 JavaScript 和 HTML 來建立簡單的"Hello，world"應用程式目標為 windows 10 上通用 Windows 平台 (UWP)。 在 Microsoft Visual Studio 中的單一專案，您可以建置在任何 windows 10 裝置執行的應用程式。

> [!NOTE]
> 本教學課程使用 Visual Studio Community 2017。 如果您使用不同版本的 Visual Studio，它的外觀可能會略有不同。


您將在此處了解如何：

-   建立新的**Visual Studio 2017**專案目標為**windows 10**和**UWP**。
-   新增 HTML 和 JavaScript 內容
-   在本機桌面上使用 Visual Studio 執行專案

## <a name="before-you-start"></a>開始之前...

-   [什麼是 UWP app？](universal-application-platform-guide.md)。
-   若要完成這個教學課程，您需要 windows 10 與 Visual Studio2017。 [開始設定](get-set-up.md)。
-   我們亦假設您使用的是 Visual Studio 中預設的視窗配置。 如果您變更預設配置，您可以使用 **\[視窗\]** 功能表中的 **\[重設視窗配置\]** 命令來重設它。

## <a name="step-1-create-a-new-project-in-visual-studio"></a>步驟 1：在 Visual Studio 中建立新專案

1.  啟動 Visual Studio 2017。

2.  從 **\[檔案\]** 功能表中，選取 **\[新增\] > \[專案\]** 來開啟 *\[新增專案\]* 對話方塊。

3.  從左邊的範本清單中，開啟 **\[已安裝\] > \[範本\] > \[JavaScript\]**，然後選擇 **\[Windows 通用\]** 來查看 UWP 專案範本的清單。

    (如果您沒有看到任何「通用」範本，表示您可能遺失用於建立 UWP app 的元件。 您可以重複安裝程序並新增 UWP 支援，方法是按一下 **\[新增專案\]** 對話方塊上的 *\[開啟 Visual Studio 安裝程式\]*。 請參閱[開始設定](get-set-up.md)

4.  選擇 **\[空白應用程式 (通用 Windows)\]** 範本，然後輸入 "HelloWorld" 作為 **\[名稱\]**。 選取 **\[確定\]**。

    ![\[新增專案\] 視窗](images/win10-js-01.png)

> [!NOTE]
> 如果這是您第一次使用 Visual Studio，您可能會看到 \[設定\] 對話方塊要求您啟用 **\[開發人員模式\]**。 開發人員模式是啟用某些功能的特殊設定，例如直接執行 app 的權限，而非只執行來自於 Microsoft Store。 如需詳細資訊，請閱讀[啟用您的裝置以進行開發](enable-your-device-for-development.md)。 若要繼續使用此指南，請選取 **\[開發人員模式\]**，按一下 **\[是\]**，並關閉對話方塊。

 ![啟用開發人員模式對話方塊](images/win10-cs-00.png)

5.  \[目標版本\]/\[最小版本\] 對話方塊隨即出現。 在這個教學課程，使用預設設定即可，因此請選取 **\[確定\]** 來建立專案。

    ![[方案總管] 視窗](images/win10-cs-02.png)

6.  當您的新專案開啟時，其檔案會顯示在右邊的 **\[方案總管\]** 窗格中。 您可能需要選擇 **\[方案總管\]** 索引標籤 (而不是 **\[屬性\]** 索引標籤)，才能看到您的檔案。

    ![[方案總管] 視窗](images/win10-js-02.png)

雖然 **\[空白應用程式 (通用 Windows)\]** 是最基本的範本，但它仍然包含許多檔案。 這些檔案對於所有使用 JavaScript 的 UWP app 都是必要的。 您在 Visual Studio 中建立的每個專案都包含這些檔案。


### <a name="whats-in-the-files"></a>檔案提供哪些內容？

若要檢視和編輯您專案中的檔案，請在 **\[方案總管\]** 中按兩下該檔案。 

*default.css*

-  app 使用的預設樣式表。

*main.js*

- 預設 JavaScript 檔案。 index.html 檔案中參考到這個檔案。

*index.html*

- app 的網頁，當 app 時會載入並顯示。

*一組標誌影像*
-   Assets/Square150x150Logo.scale-200.png 代表 [開始] 選單中您的 App。
-   Assets/StoreLogo.png 代表 Microsoft Store 中您的 App。
-   Assets/SplashScreen.scale-200.png 是您 App 啟動時顯示的啟動顯示畫面。

## <a name="step-2-adding-a-button"></a>步驟 2：新增按鈕

按一下 *index.html*，在編輯器中選取它，並將包含的 HTML 變更如下：

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

看起來應該像這樣：

 ![專案的 HTML](images/win10-js-03.png)

這個 HTML 參考 *main.js*，其中包含我們的 JavaScript，然後新增一行文字和單一按鈕至網頁內文。 提供按鈕 *ID*，讓 JavaScript 可以參照它。


## <a name="step-3-adding-some-javascript"></a>步驟 3：加入一些 JavaScript

現在我們將新增 JavaScript。 按一下 *main.js* 選取它，並新增下列程式碼：

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

看起來應該像這樣：

 ![專案的 JavaScript](images/win10-js-04.png)

這個 JavaScript 宣告兩個函式。 *window.onload* 函式在 *index.html* 顯示時自動呼叫。 它尋找按鈕（使用我們所宣告的 ID），然後新增 onclick 處理常式：按一下按鈕時所呼叫的方法。

第二個函式 *sayHello()* 會建立並顯示對話方塊。 這非常類似於 *Alert()* 函式，您可能會從先前的 JavaScript 開發知道。


## <a name="step-4-run-the-app"></a>步驟 4：執行 App！

現在您可以按下 F5 來執行 App。 App 會載入，網頁將會顯示。 按一下按鈕，訊息對話方塊會出現。

 ![執行專案](images/win10-js-05.png)



## <a name="summary"></a>摘要


恭喜，您已經完成的 JavaScript 應用程式針對 windows 10 和 UWP ！ 這是非常簡單的範例，但是，您現在可以開始加入您最愛的 JavaScript 程式庫和架構來建立您自己的應用程式。 因為它是 UWP app，您可以將其發佈至 Microsoft Store。 如需如何新增協力廠商架構的範例，請參閱這些專案：

* [以 JavaScript 和 CreateJS 撰寫的適用於 Microsoft Store 的簡單 2D UWP 遊戲](get-started-tutorial-game-js2d.md)
* [以 JavaScript 和 threeJS 撰寫的適用於 Microsoft Store 的 3D UWP 遊戲](get-started-tutorial-game-js3d.md)


