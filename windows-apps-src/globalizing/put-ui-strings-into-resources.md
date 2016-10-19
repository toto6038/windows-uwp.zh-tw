---
author: DelfCo
Description: "將 UI 的字串資源放入資源檔。 接著您就可以從程式碼或標記中參照這些字串。"
title: "將 UI 字串放入資源"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: e404eceb4aad562474cff264bb992a3d71a3bed4

---

# 將 UI 字串放入資源





**重要 API**

-   [**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)
-   [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)

將 UI 的字串資源放入資源檔。 接著您就可以從程式碼或標記中參照這些字串。

本主題說明將數個語言的字串資源新增到您的通用 Windows 應用程式，以及如何短暫測試的步驟。

## <span id="put_strings_into_resource_files__instead_of_putting_them_directly_in_code_or_markup."></span><span id="PUT_STRINGS_INTO_RESOURCE_FILES__INSTEAD_OF_PUTTING_THEM_DIRECTLY_IN_CODE_OR_MARKUP."></span>將字串放入資源檔中，而不是直接放入程式碼或標記當中。


1.  在 Visual Studio 中開啟您的方案 (或建立新的方案)。

2.  在 Visual Studio 中開啟 package.appxmanifest，移至 [應用程式]**** 索引標籤，並且 (針對此範例) 將預設語言設定為 [en-US]。 如果您的方案中有多個 package.appxmanifest 檔案，請針對每個項目執行這個動作。
    <br>**注意：**這樣會指定專案的預設語言。 如果使用者的慣用語言或顯示語言不符合應用程式提供的語言資源，就會使用預設語言資源。
3.  建立資料夾以包含資源檔案。
    1.  在 [方案總管] 中，用滑鼠右鍵按一下 [專案] (如果您的方案包含多個專案則為 [共用的專案]) 並選取 [新增]**** &gt; [新資料夾]****。
    2.  將新資料夾命名為 "Strings"。
    3.  如果在 [方案總管] 中沒有看到新資料夾，請保持選取該專案，同時從 Microsoft Visual Studio 功能表中，選取 [專案]**** &gt; [顯示所有檔案]****。

4.  建立適用於英文 (美國) 的子資料夾和資源檔案。
    1.  在 Strings 資料夾上按一下滑鼠右鍵，然後在下面新增資料夾。 將它命名為 "en-US"。 資源檔案會放入針對 [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) 語言標籤命名的資料夾中。 如需語言限定詞與通用語言標籤清單的詳細資料，請參閱[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。
    2.  在 [en-US] 資料夾上按一下滑鼠右鍵，然後選取 [加入]**** &gt; [新增項目]****。
    3.  **XAML：**選取 [資源檔 (.resw)]。
        <br>**HTML：**選取 [資源檔 (.resjson)]。

    4.  按一下 [新增]****。 這會新增預設名稱為 "Resources.resw" (針對 **XAML**) 或 "resources.rejson" (針對 **HTML**) 的資源檔案。 建議您使用此預設檔名。 應用程式可將其資源分割到其他檔案，但您必須謹慎小心以正確地參照這些資源 (請參閱[如何載入字串資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323))。
    5.  **僅 XAML：**如果您的 .resx 檔案只包含先前 .NET 專案的字串資源，請選取 [加入]**** &gt; [現有項目]****，然後新增 .resx 檔案，再將它重新命名為 .resw。
    6.  開啟檔案並使用編輯器新增這些資源：

        **XAML：**

        Strings/en-US/Resources.resw ![新增資源，英文](images/addresource-en-us.png) 在這個範例中，"Greeting.Text" 和 "Farewell" 識別要顯示的字串。 "Greeting.Width" 會識別 "Greeting" 字串的 Width 屬性。 註解是為負責將字串當地語系化成其他語言的譯者提供特殊指示的好位置。

        **HTML：**

        新檔案包含預設內容。 以下列內容取代預設內容 (可能會和預設內容相似)：

        Strings/en-US/resources.resjson

        ```        json
        {
                "greeting"              : "Hello",
                "_greeting.comment"     : "A welcome greeting",

                "farewell"              : "Goodbye",
                "_farewell.comment"     : "A goodbye"
        }
        ```

        這是嚴格的 JavaScript 物件標記法 (JSON) 語法，每個名稱/值組後面都必須放置逗號，最後一個除外。 在這個範例中，"greeting" 和 "farewell" 會識別將要顯示的字串。 另一組 ("\_greeting.comment" 與 "\_farewell.comment") 則是說明字串的註解。 註解是為負責將字串當地語系化成其他語言的譯者提供特殊指示的好位置。

## <span id="associate_controls_to_resources."></span><span id="ASSOCIATE_CONTROLS_TO_RESOURCES."></span>關聯控制項與資源。


**僅 XAML：**

您必須將每個需要當地語系化文字的控制項與 .resw 檔案產生關聯。 完成這項動作的方式是在 XAML 元素上使用 **x:Uid** 屬性，就像這樣：

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

對於資源名稱，您必須提供 **Uid** 屬性值並指定哪個屬性要取得翻譯字串 (在此例中為 Text 屬性)。 您可以為不同的語言指定其他屬性/值，例如 Greeting.Width，但是請小心這類的配置相關屬性。 您應該努力讓控制項根據裝置的螢幕來動態配置。

請注意，附加屬性在 resw 檔案 (例如 AutomationPeer.Name) 中以不同的方式來處理。 您需要明確寫出命名空間，如下所示：

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <span id="add_string_resource_identifiers_to_code_and_markup."></span><span id="ADD_STRING_RESOURCE_IDENTIFIERS_TO_CODE_AND_MARKUP."></span>將字串資源識別碼新增到程式碼和標記。


**XAML：**

在您的程式碼中，您可以動態參考字串：

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```

**HTML：**

1.  將適用於 JavaScript 的 Windows Library 的參考新增到您的 HTML 檔案 (如果尚未新增)。

    **注意：**以下程式碼顯示當您在 Visual Studio 建立新的 [空白應用程式 (通用 Windows)]**** JavaScript 專案時，所產生的 Windows 專案之 default.html 檔案的 HTML。 請注意，檔案已包含 WinJS 的參考。

    ```    HTML
    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>
    ```

2.  在隨 HTML 檔案提供的 JavaScript 程式碼中，當載入 HTML 時呼叫 [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864) 函式。

    ```    JavaScript
    WinJS.Application.onloaded = function(){
        WinJS.Resources.processAll();
    }
    ```
    
    如果有其他 HTML 載入至 [**WinJS.UI.Pages.PageControl**](https://msdn.microsoft.com/library/windows/apps/jj126158) 物件，請在頁面控制項的 [**IPageControlMembers.ready**](https://msdn.microsoft.com/library/windows/apps/hh770590) 方法中呼叫 [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)(*element*)，其中 *element* 是要載入的 HTML 元素 (及其子元素)。 這個範例是以[應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=227301)的案例 6 為根據：

    ```    JavaScript
    var output;
    
    var page = WinJS.UI.Pages.define("/html/scenario6.html", {
        ready: function (element, options) {
            output = element.querySelector('#output');
            WinJS.Resources.processAll(output); // Refetch string resources
            WinJS.Resources.addEventListener("contextchanged", refresh, false);
        }
        unload: function () { 
            WinJS.Resources.removeEventListener("contextchanged", refresh, false); 
        } 
    });
    ```

3.  在 HTML 中，使用資源檔中的資源識別碼 ('greeting' 和 'farewell') 參考字串資源。
    ```    HTML
    <h2><span data-win-res="{textContent: 'greeting';}"></span></h2>
    <h2><span data-win-res="{textContent: 'farewell'}"></span></h2>
    ```

4.  參考屬性的字串資源。

    ```    HTML
    <div data-win-res="{attributes: {'aria-label'; : 'String1'}}" >
    ```

    HTML 取代項目的 data-win-res 屬性的一般模式是 data-win-res="{*屬性名稱1*: '*資源識別碼1*', *屬性名稱2*: '*資源識別碼2*'}"。

    **注意：**如果字串不包含任何標記，請盡可能將資源繫結到 textContent 屬性，而不是 innerHTML。 取代 textContent 屬性的速度會比 innerHTML 快許多。

5.  在 JavaScript 中參考字串資源。
    <span codelanguage="JavaScript"></span>
    ```    JavaScript
    var el = element.querySelector('#header');
    var res = WinJS.Resources.getString('greeting');
    el.textContent = res.value;
    el.setAttribute('lang', res.lang);
    ```

## <span id="add_folders_and_resource_files_for_two_additional_languages."></span><span id="ADD_FOLDERS_AND_RESOURCE_FILES_FOR_TWO_ADDITIONAL_LANGUAGES."></span>針對兩個其他語言新增資料夾與資源檔案。


1.  在適用於德文的 Strings 資料夾之下，建立另一個資料夾。 將資料夾命名為 "de-DE"，用以代表德文 (德國)。
2.  在 de-DE 資料夾中，建立另一個資源檔案，然後新增下列內容：

    **XAML：**

    strings/de-DE/Resources.resw

    ![新增資源, 德文](images/addresource-de-de.png)

    **HTML：**

    strings/de-DE/resources.resjson

    ```    json
    {
        "greeting"              : "Hallo",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Auf Wiedersehen",
        "_farewell.comment"     : "A goodbye."
    }
    ```

3.  再建立一個名為 "fr-FR" 的資料夾，用以代表法文 (法國)。 建立新的資源檔案，然後新增下列內容：

    **XAML：**

    strings/fr-FR/Resources.resw ![新增資源，法文](images/addresource-fr-fr.png)
    **HTML：**

    strings/fr-FR/resources.resjson

    ```    json
    {
        "greeting"              : "Bonjour",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Au revoir",
        "_farewell.comment"     : "A goodbye."
    }
    ```

## <span id="build_and_run_the_app."></span><span id="BUILD_AND_RUN_THE_APP."></span>建置並執行應用程式。


針對您的預設顯示語言來測試應用程式。

1.  按 F5 來建置並執行應用程式。
2.  請注意，greeting 與 farewell 將以使用者偏好的語言顯示。
3.  結束應用程式。

針對其他語言來測試應用程式。

1.  顯示您裝置上的 [設定]****。
2.  選取 [時間與語言]****。
3.  選取 [地區及語言]**** (或者選取手機或手機模擬器上的 [語言]****)。
4.  請注意，當您執行應用程式時所顯示的語言是列在最上方的語言，也就是英文、德文或法文。 如果列在最上方的語言不是這三種語言之一，應用程式將切換回清單上支援的下一個語言。
5.  如果您的電腦上沒有上述三種語言，可以按一下 [新增語言]****，然後將遺漏的語言新增至清單。
6.  若要測試另一種語言的應用程式，在清單中選取該語言並按一下 [設為預設值]**** (或者在手機或手機模擬器上，在清單中長按該語言然後點選 [上移]**** 直到它在頂端顯示為止)。 然後執行該應用程式。

## <span id="related_topics"></span>相關主題


* [如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [如何載入字串資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 






<!--HONumber=Aug16_HO3-->


