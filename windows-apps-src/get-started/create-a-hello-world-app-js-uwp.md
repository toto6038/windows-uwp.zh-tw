---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: 建立 "Hello, world" 應用程式 (JS)
description: 本教學課程會教您如何使用 JavaScript 和 HTML 來建立目標是 Windows 10 上通用 Windows 平台 (UWP) 的簡單 &\#0034;Hello, world&\#0034; app。
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3f4ab5b177539bc286ce24d480cd949d43a51e17
ms.sourcegitcommit: bd41fb6f59dfbd7021b14ff749b8b0f83f883c0f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963627"
---
# <a name="create-a-hello-world-app-js"></a>建立 "Hello, world" 應用程式 (JS)

本教學課程會教您如何使用 JavaScript 和 HTML 來建立目標是 Windows 10 上的通用 Windows 平台 (UWP) 的簡單 "Hello, world" 應用程式。 只要使用 Microsoft Visual Studio 中的單一專案，您便可以建置可在任何 Windows 10 裝置上執行的 App。

> [!NOTE]
> 本教學課程使用 Visual Studio Community 2017。 如果您使用不同版本的 Visual Studio，它的外觀可能會略有不同。

> [!WARNING]
> Visual Studio 2019 不支援 Javascript UWP 應用程式開發。 您必須使用 Visual Studio 2017 來開發 Javascript UWP 應用程式。

您將在此處了解如何：

-   建立目標是 **Windows 10** 和 **UWP** 的新 **Visual Studio 2017** 專案。
-   新增 HTML 和 JavaScript 內容
-   在本機桌面上使用 Visual Studio 執行專案

## <a name="before-you-start"></a>開始之前...

-   [什麼是 UWP 應用程式？](universal-application-platform-guide.md)。
-   若要完成這個教學課程，您需要 Windows 10 與 Visual Studio。 [開始設定](get-set-up.md)。
-   我們亦假設您使用的是 Visual Studio 中預設的視窗配置。 如果您變更預設配置，您可以使用 [視窗]  功能表中的 [重設視窗配置]  命令來重設它。

## <a name="step-1-create-a-new-project-in-visual-studio"></a>步驟 1：在 Visual Studio 中建立新專案。

1.  啟動 Visual Studio 2017。

2.  從 [檔案]  功能表，選取 [新增] > [專案...]  來開啟 [建立新專案]  對話方塊。

3.  選取 [空白應用程式 (通用 Windows) JavaScript]  ，然後選取 [下一步]  。

    (如果您沒有看到任何「通用」範本，表示您可能遺失用於建立 UWP 應用程式的元件。 您可以重複安裝程序並加入 UWP 支援，方法是按一下 [建立新專案]  對話方塊上的 [開啟 Visual Studio 安裝程式]  。 請參閱[開始設定](get-set-up.md)

4.  在 [設定新專案]  對話方塊中，輸入 "HelloWorld" 作為**專案名稱**，然後選取 [建立]  。

> [!NOTE]
> 如果這是您第一次使用 Visual Studio，您可能會看到 [設定] 對話方塊要求您啟用 [開發人員模式]  。 開發人員模式是啟用某些功能的特殊設定，例如，直接執行應用程式的權限，而非只執行來自 Microsoft Store 的。 如需詳細資訊，請閱讀[啟用您的裝置以進行開發](enable-your-device-for-development.md)。 若要繼續使用此指南，請選取 [開發人員模式]  ，按一下 [是]  ，並關閉對話方塊。

 ![啟用開發人員模式對話方塊](images/win10-cs-00.png)

5.  [目標版本/最小版本] 對話方塊隨即出現。 在這個教學課程中使用預設設定即可，因此請選取 [確定]  來建立專案。

    ![[方案總管] 視窗](images/win10-cs-02.png)

6.  當您的新專案開啟時，其檔案會顯示在右邊的 [方案總管]  窗格中。 您可能需要選擇 [方案總管]  索引標籤 (而不是 [屬性]  索引標籤)，才能看到您的檔案。

    ![[方案總管] 視窗](images/win10-js-02.png)

雖然 [空白應用程式 (通用 Windows)]  是最基本的範本，但它仍然包含許多檔案。 這些檔案對於所有使用 JavaScript 的 UWP app 都是必要的。 您在 Visual Studio 中建立的每個專案都包含這些檔案。


### <a name="whats-in-the-files"></a>檔案提供哪些內容？

若要檢視和編輯您專案中的檔案，請在 [方案總管]  中按兩下該檔案。

*default.css*

-  應用程式使用的預設樣式表。

*main.js*

- 預設 JavaScript 檔案。 index.html 檔案中參考到這個檔案。

*index.html*

- 應用程式的網頁，當應用程式啟動時會載入並顯示。

*一組標誌影像*
-   Assets/Square150x150Logo.scale-200.png 代表 [開始] 功能表中您的 App。
-   Assets/StoreLogo.png 代表 Microsoft Store 中您的應用程式。
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

這個 HTML 參考 *main.js*，其中包含我們的 JavaScript，然後新增一行文字和單一按鈕至網頁內文。 提供按鈕 *ID*，讓 JavaScript 可以參考它。


## <a name="step-3-adding-some-javascript"></a>步驟 3：新增一些 JavaScript

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

這個 JavaScript 宣告兩個函式。 *window.onload* 函式在 *index.html* 顯示時自動呼叫。 它尋找按鈕 (使用我們所宣告的 ID)，然後新增 onclick 處理常式：按一下按鈕時所呼叫的方法。

第二個函式 *sayHello()* 會建立並顯示對話方塊。 這非常類似於 *Alert()* 函式，您可能會從先前的 JavaScript 開發知道。


## <a name="step-4-run-the-app"></a>步驟 4：執行應用程式！

現在您可以按下 F5 來執行應用程式。 應用程式會載入，網頁將會顯示。 按一下按鈕，訊息對話方塊會出現。

 ![執行專案](images/win10-js-05.png)



## <a name="summary"></a>摘要


恭喜！您已經建立適用於 Windows 10 和 UWP 的 JavaScript 應用程式！ 這是非常簡單的範例，但是，您現在可以開始新增您最愛的 JavaScript 程式庫和架構來建立您自己的應用程式。 因為它是 UWP 應用程式，您可以將它發佈至 Microsoft Store。 如需如何新增協力廠商架構的範例，請參閱這些專案：

* [以 JavaScript 和 CreateJS 撰寫且適用於 Microsoft Store 的簡單 2D UWP 遊戲](get-started-tutorial-game-js2d.md)
* [以 JavaScript 和 threeJS 撰寫的適用於 Microsoft Store 的 3D UWP 遊戲](get-started-tutorial-game-js3d.md)
