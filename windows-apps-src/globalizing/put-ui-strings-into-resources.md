---
author: DelfCo
Description: "將 UI 的字串資源放入資源檔。 接著您就可以從程式碼或標記中參照這些字串。"
title: "將 UI 字串放入資源"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: dff1e6df480a1ffc0e2441c78b942ccd0ee2126c

---

# <a name="put-ui-strings-into-resources"></a>將 UI 字串放入資源
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

將 UI 的字串資源放入資源檔。 接著您就可以從程式碼或標記中參照這些字串。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)</li>
<li>[**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)</li>
</ul>
</div>


本主題說明將數個語言的字串資源新增到您的通用 Windows app，以及如何短暫測試的步驟。

## <a name="put-strings-into-resource-files-instead-of-putting-them-directly-in-code-or-markup"></a>將字串放入資源檔中，而不是直接放入程式碼或標記當中。


1.  在 Visual Studio 中開啟您的方案 (或建立新的方案)。

2.  在 Visual Studio 中開啟 package.appxmanifest，移至 **[應用程式]** 索引標籤，並且 (針對此範例) 將預設語言設定為 [en-US]。 如果您的方案中有多個 package.appxmanifest 檔案，請針對每個項目執行這個動作。
    <br>**注意：**這樣會指定專案的預設語言。 如果使用者的慣用語言或顯示語言不符合應用程式提供的語言資源，就會使用預設語言資源。
3.  建立資料夾以包含資源檔案。
    1.  在 [方案總管] 中，用滑鼠右鍵按一下 [專案] (如果您的方案包含多個專案則為 [共用的專案]) 並選取 **[新增]** &gt; **[新資料夾]**。
    2.  將新資料夾命名為 "Strings"。
    3.  如果在 [方案總管] 中沒有看到新資料夾，請保持選取該專案，同時從 Microsoft Visual Studio 功能表中，選取 **[專案]** &gt; **[顯示所有檔案]**。

4.  建立適用於英文 (美國) 的子資料夾和資源檔案。
    1.  在 Strings 資料夾上按一下滑鼠右鍵，然後在下面新增資料夾。 將它命名為 "en-US"。 資源檔案會放入針對 [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) 語言標籤命名的資料夾中。 如需語言限定詞與通用語言標籤清單的詳細資料，請參閱[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。
    2.  在 [en-US] 資料夾上按一下滑鼠右鍵，然後選取 **[加入]** &gt; **[新增項目]**。
    3.  選取 [資源檔 (.resw)]。

    4.  按一下 **[新增]**。 這會新增預設名稱為 "Resources.resw" 的資源檔案。 建議您使用此預設檔名。 應用程式可將其資源分割到其他檔案，但您必須謹慎小心以正確地參照這些資源 (請參閱[如何載入字串資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323))。
    5.  如果您的 .resx 檔案只包含先前 .NET 專案的字串資源，請選取 **[加入]** &gt; **[現有項目]**，然後新增 .resx 檔案，再將它重新命名為 .resw。
    6.  開啟檔案並使用編輯器新增這些資源：


        Strings/en-US/Resources.resw ![新增資源，英文](images/addresource-en-us.png) 在這個範例中，"Greeting.Text" 和 "Farewell" 會識別要顯示的字串。 "Greeting.Width" 會識別 "Greeting" 字串的 Width 屬性。 註解是為負責將字串當地語系化成其他語言的譯者提供特殊指示的好位置。

## <a name="associate-controls-to-resources"></a>建立控制項與資源的關聯。

您必須將每個需要當地語系化文字的控制項與 .resw 檔案產生關聯。 完成這項動作的方式是在 XAML 元素上使用 **x:Uid** 屬性，就像這樣：

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

對於資源名稱，您必須提供 **Uid** 屬性值並指定哪個屬性要取得翻譯字串 (在此例中為 Text 屬性)。 您可以為不同的語言指定其他屬性/值，例如 Greeting.Width，但是請小心這類的配置相關屬性。 您應該努力讓控制項根據裝置的螢幕來動態配置。

請注意，附加屬性在 resw 檔案 (例如 AutomationPeer.Name) 中以不同的方式來處理。 您需要明確寫出命名空間，如下所示：

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <a name="add-string-resource-identifiers-to-code-and-markup"></a>將字串資源識別碼新增到程式碼和標記。

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


## <a name="add-folders-and-resource-files-for-two-additional-languages"></a>針對兩個其他語言新增資料夾與資源檔案。


1.  在適用於德文的 Strings 資料夾之下，建立另一個資料夾。 將資料夾命名為 "de-DE"，用以代表德文 (德國)。
2.  在 de-DE 資料夾中，建立另一個資源檔案，然後新增下列內容：

    strings/de-DE/Resources.resw

    ![新增資源，德文](images/addresource-de-de.png)


3.  再建立一個名為 "fr-FR" 的資料夾，用以代表法文 (法國)。 建立新的資源檔案，然後新增下列內容：

    strings/fr-FR/Resources.resw ![新增資源，法文](images/addresource-fr-fr.png)

## <a name="build-and-run-the-app"></a>建置並執行應用程式。


針對您的預設顯示語言來測試應用程式。

1.  按 F5 來建置並執行應用程式。
2.  請注意，greeting 與 farewell 將以使用者偏好的語言顯示。
3.  結束應用程式。

針對其他語言來測試應用程式。

1.  顯示您裝置上的 **[設定]**。
2.  選取 **[時間與語言]**。
3.  選取 **[地區及語言]** (或者選取手機或手機模擬器上的 **[語言]**)。
4.  請注意，當您執行應用程式時所顯示的語言是列在最上方的語言，也就是英文、德文或法文。 如果列在最上方的語言不是這三種語言之一，應用程式將切換回清單上支援的下一個語言。
5.  如果您的電腦上沒有上述三種語言，可以按一下 **[新增語言]**，然後將遺漏的語言新增至清單。
6.  若要測試另一種語言的應用程式，在清單中選取該語言並按一下 **[設為預設值]** (或者在手機或手機模擬器上，在清單中長按該語言然後點選 **[上移]** 直到它在頂端顯示為止)。 然後執行該應用程式。

## <a name="related-topics"></a>相關主題


* [如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [如何載入字串資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 






<!--HONumber=Dec16_HO2-->


