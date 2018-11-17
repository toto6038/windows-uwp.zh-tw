---
author: stevewhims
Description: There are several URI (Uniform Resource Identifier) schemes that you can use to refer to files that come from your app's package, your app's data folders, or the cloud. You can also use a URI scheme to refer to strings loaded from your app's Resources Files (.resw).
title: URI 配置
template: detail.hbs
ms.author: stwhi
ms.date: 10/16/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 75ba42674ca1ea460698fcce6e67bb3528589797
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7160098"
---
# <a name="uri-schemes"></a>URI 配置

有幾個可供您用來參考您的應用程式套件、您的 App 資料的資料夾或雲端之檔案的 URI (統一資源識別項) 配置。 您也可以使用 URI 配置參考從應用程式資源檔案 (.resw) 載入的字串。 您可以在程式碼、XAML 標記、應用程式套件資訊清單或磚及快顯通知範本中使用這些 URI 配置。

## <a name="common-features-of-the-uri-schemes"></a>URI 配置的一般功能

本主題說明的所有配置都遵循正規化及資源擷取的一般 URI 配置規則。 如需了解 URI 的一般語法，請參閱 [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444)。

所有的 URI 配置都會根據 [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444) 將階層式組件定義為 URI 的授權單位及路徑元件。

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

這表示，URI 基本上會有三個元件。 緊接在 URI *配置*的兩個斜線後面的是稱為*授權單位*的元件 (可空白)。 再緊接其後的是*路徑*。 以 URI `http://www.contoso.com/welcome.png` 為例，其配置是「`http://`」、授權單位是「`www.contoso.com`」，以及路徑是「`/welcome.png`」。 另舉一個範例 URI `ms-appx:///logo.png`，其中授權單位元件為空白，並接受預設值。

本主題所述 URI 的配置特定處理會忽略片段元件。 進行資源擷取和比較時，片段元件並無任何影響。 不過，特定實作以上的層級可能會解譯片段來擷取次要資源。

正規化所有 IRI 元件之後，會進行位元組對位元組的比較。

## <a name="case-insensitivity-and-normalization"></a>不區分大小寫和正規化

本主題說明的所有 URI 配置都遵循配置正規化及資源擷取的一般 URI 規則 (RFC 3986)。 這些標準化形式的 URI 會維持大小寫，並對 RFC 3986 非保留字元進行百分比解碼。

就本主題所述的所有 URI 配置而言，*配置*、*授權單位*和*路徑*依照標準不區分大小寫，或是由其他系統以不區分大小寫的方式來處理。 **注意**：該規則的唯一例外是 `ms-resource` 的*授權單位*，這會區分大小寫。

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx 和 ms-appx-web

使用 `ms-appx` 或 `ms-appx-web` URI 配置來參考應用程式套件中的檔案 (請參閱[封裝應用程式](../packaging/index.md))。 應用程式套件中的檔案通常是靜態影像、資料、程式碼及配置檔案。 `ms-appx-web` 配置將相同的檔案當做 `ms-appx` 來存取，但會在網頁區間中進行。 如需詳細資訊，請參閱[從 XAML 標記和程式碼參考影像或其他資產](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)。

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>配置名稱 (ms-appx 和 ms-appx-web)

URI 配置名稱是字串 "ms-appx" 或 "ms-appx-web"。

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>授權單位 (ms-appx 和 ms-appx-web)

授權單位是封裝資訊清單中定義的套件識別名稱。 因此受限於套件識別名稱中所允許之字元集的 URI 和 IRI (國際化資源識別項) 形式。 套件名稱必須是在目前執行中應用程式的套件相依性關係圖上，其中一個套件的名稱。

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

如果授權單位中出現任何其他字元，則擷取和比較會失敗。 授權單位的預設值是目前執行中應用程式的套件。

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>使用者資訊和連接埠 (ms-appx 和 ms-appx-web)

與其他常用配置不同，`ms-appx` 配置不會定義使用者資訊或連接埠元件。 由於不允許 "@" and ":" 做為有效授權單位值，如果有包含，查閱會失敗。 下列各項會失敗。

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>路徑 (ms-appx 和 ms-appx-web)

路徑元件符合一般 RFC 3986 語法，並且支援在 IRI 中使用非 ASCII 字元。 路徑元件定義檔案的邏輯或實體檔案路徑。 該檔案位於與授權單位指定應用程式之應用程式套件安裝位置相關聯的資料夾中。

如果路徑參考實體路徑及檔案名稱，則擷取該實體檔案資產。 但如果不找到這樣的實體檔案，則在執行階段使用內容交涉來決定擷取期間傳回的實際資源。 這項決定以應用程式、作業系統和使用者設定 (例如語言、顯示縮放比例、佈景主題、高對比及其他執行階段內容) 為根據。 例如，決定要擷取的實際資源值時，可以將應用程式語言、系統顯示設定與使用者高對比設定的組合列入考慮。

```xml
ms-appx:///images/logo.png
```

上述 URI 可能會實際擷取目前應用程式套件中具有下列實體檔案名稱的檔案。

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

當然也可以直接以其完整名稱參考這個相同的實體檔案來進行擷取。

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

`ms-appx(-web)` 的路徑元件和一般 URI 一樣會區分大小寫。 不過，當存取資源的基礎檔案系統不區分大小寫時 (例如 NTFS)，則是以不區分大小寫的方式進行擷取。

標準化形式的 URI 會維持大小寫，並對 RFC 3986 非保留字元進行百分比解碼 ("%" 符號後面加上兩位數十六進位表示)。 路徑中的字元「?」、「#」、「/」、「*」和「"」(雙引號字元) 必須以百分比編碼來表示檔案名稱或資料夾名稱等資料。 所有百分比編碼字元都會在擷取前進行解碼。 因此，若要擷取名稱為 Hello#World.html 的檔案，請使用這個 URI。

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>查詢 (ms-appx 和 ms-appx-web)

進行資源擷取時會忽略查詢參數。 標準化形式的查詢參數會維持大小寫。 進行比較時會忽略查詢參數。

## <a name="ms-appdata"></a>ms-appdata

使用 `ms-appdata` URI 配置來參考來自應用程式本機、漫遊以及暫存資料之資料夾的檔案。 如需有關這些應用程式資料之資料夾的詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](../design/app-settings/store-and-retrieve-app-data.md)。

`ms-appdata` URI 配置不會執行 [ms-appx 和 ms-appx-web](#ms-appx-and-ms-appx-web) 所做的執行階段內容交涉。 但是您可以回應 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 的內容，並在 URI 中使用完整實體檔案名稱，從應用程式資料載入適當的資產。

### <a name="scheme-name-ms-appdata"></a>配置名稱 (ms-appdata)

URI 配置名稱是字串 "ms-appdata"。

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>授權單位 (ms-appdata)

授權單位是封裝資訊清單中定義的套件識別名稱。 因此受限於套件識別名稱中所允許之字元集的 URI 和 IRI (國際化資源識別項) 形式。 套件名稱必須是目前執行中應用程式套件的名稱。

```xml
ms-appdata://Contoso.MyApp/
```

如果授權單位中出現任何其他字元，則擷取和比較會失敗。 授權單位的預設值是目前執行中應用程式的套件。

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>使用者資訊和連接埠 (ms-appdata)

與其他常用配置不同，`ms-appdata` 配置不會定義使用者資訊或連接埠元件。 由於不允許 "@" and ":" 做為有效授權單位值，如果有包含，查閱會失敗。 下列各項會失敗。

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>路徑 (ms-appdata)

路徑元件符合一般 RFC 3986 語法，並且支援在 IRI 中使用非 ASCII 字元。 在 [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) 位置中，有三個保留作本機、漫遊及暫時狀態儲存區的資料夾。 `ms-appdata` 配置允許存取這些位置中的檔案及資料夾。 路徑元件的第一個區段必須以下列方式指定特定資料夾。 因此「路徑空白」形式的「階層組件」不合法。

本機資料夾。

```xml
ms-appdata:///local/
```

暫存資料夾。

```xml
ms-appdata:///temp/
```

漫遊資料夾。

```xml
ms-appdata:///roaming/
```

`ms-appdata` 的路徑元件和一般 URI 一樣會區分大小寫。 不過，當存取資源的基礎檔案系統不區分大小寫時 (例如 NTFS)，則是以不區分大小寫的方式進行擷取。

標準化形式的 URI 會維持大小寫，並對 RFC 3986 非保留字元進行百分比解碼 ("%" 符號後面加上兩位數十六進位表示)。 路徑中的字元「?」、「#」、「/」、「*」和「"」(雙引號字元) 必須以百分比編碼來表示檔案名稱或資料夾名稱等資料。 所有百分比編碼字元都會在擷取前進行解碼。 因此，若要擷取名稱為 Hello#World.html 的本機檔案，請使用這個 URI。

```xml
ms-appdata://local/Hello%23World.html
```

資源的擷取以及最上層路徑區段的識別是在點正規化之後進行處理 (「.././b/c」)。 因此，URI 無法透過以點替代其中一個保留資料夾的方式來表示其本身。 因此，不允許下列 URI。

```xml
ms-appdata:///local/../hello/logo.png
```

但允許這個 URI (即使很冗長)。

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>查詢 (ms-appdata)

進行資源擷取時會忽略查詢參數。 標準化形式的查詢參數會維持大小寫。 進行比較時會忽略查詢參數。

## <a name="ms-resource"></a>ms-resource

使用 `ms-resource` URI 配置參考從應用程式資源檔案 (.resw) 載入的字串。 如需資源檔案的範例及詳細資訊，請參閱[將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md)。

### <a name="scheme-name-ms-resource"></a>配置名稱 (ms-resource)

URI 配置名稱是字串 "ms-resource"。

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>授權單位 (ms-resource)

授權單位是封裝資源索引 (PRI) 中定義的最上層資源對應，通常對應至封裝資訊清單中的定義的套件識別名稱。 請參閱[封裝應用程式](../packaging/index.md)。 因此受限於套件識別名稱中所允許之字元集的 URI 和 IRI (國際化資源識別項) 形式。 套件名稱必須是在目前執行中應用程式的套件相依性關係圖上，其中一個套件的名稱。

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

如果授權單位中出現任何其他字元，則擷取和比較會失敗。 授權單位的預設值是目前執行中應用程式的區分大小寫套件名稱。

```xml
ms-resource:///
```

授權單位會區分大小寫，而且標準化形式也會維持大小寫。 不過，查閱資源時是以不區分大小寫的方式進行。

### <a name="user-info-and-port-ms-resource"></a>使用者資訊和連接埠 (ms-resource)

與其他常用配置不同，`ms-resource` 配置不會定義使用者資訊或連接埠元件。 由於不允許 "@" and ":" 做為有效授權單位值，如果有包含，查閱會失敗。 下列各項會失敗。

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>路徑 (ms-resource)

路徑會識別 [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) 樹狀子目錄的階層位置 (請參閱[資源管理系統](https://msdn.microsoft.com/library/windows/apps/jj552947)) 以及其中的 [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live)。 這通常對應至資源檔案 (.resw) 的檔名 (不含副檔名)，以及其中字串資源的識別碼。

如需範例及詳細資訊，請參閱[將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md)和[對語言、縮放比例及高對比的磚和快顯通知支援](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。

`ms-resource` 的路徑元件和一般 URI 一樣會區分大小寫。 不過，基礎擷取會[CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628)與*ignoreCase*設為`true`。

標準化形式的 URI 會維持大小寫，並對 RFC 3986 非保留字元進行百分比解碼 ("%" 符號後面加上兩位數十六進位表示)。 路徑中的字元「?」、「#」、「/」、「*」和「"」(雙引號字元) 必須以百分比編碼來表示檔案名稱或資料夾名稱等資料。 所有百分比編碼字元都會在擷取前進行解碼。 因此，若要從資源檔案擷取字串資源名稱為`Hello#World.resw`，使用這個 URI。

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>查詢 (ms-resource)

進行資源擷取時會忽略查詢參數。 標準化形式的查詢參數會維持大小寫。 進行比較時會忽略查詢參數。 查詢參數是以區分大小寫的方式進行比較。

層級在此 URI 剖析以上之特定元件的開發人員可能會選擇依照自認合適的方式使用查詢參數。

## <a name="related-topics"></a>相關主題

* [統一資源識別項 (URI)：一般語法](http://go.microsoft.com/fwlink/p/?LinkId=263444)
* [封裝應用程式](../packaging/index.md)
* [從 XAML 標記和程式碼參考影像或其他資產](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [儲存和擷取設定及其他應用程式資料](../design/app-settings/store-and-retrieve-app-data.md)
* [將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md)
* [資源管理系統](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [對語言、縮放比例及高對比的磚和快顯通知支援](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)