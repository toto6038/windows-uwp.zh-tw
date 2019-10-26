---
Description: 使用 UTF-8 字元編碼，以達到 web 應用程式與其他 * nix 平臺（Unix、Linux 和變體）之間的最佳相容性、將當地語系化錯誤降至最低，並減少測試負擔。
title: 使用 Windows UTF-8 字碼頁
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: be3aade0289911f878d960fb62bde49b8ef840a8
ms.sourcegitcommit: 3a06cf3f8bd00e5e6eac3b38ee7e3c7cf4bc5197
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2019
ms.locfileid: "72888740"
---
# <a name="use-the-utf-8-code-page"></a>使用 UTF-8 字碼頁

使用[utf-8](http://www.utf-8.com/)字元編碼，以達到 web 應用程式與其他 * nix 平臺（Unix、Linux 和變體）之間的最佳相容性、將當地語系化錯誤降至最低，並減少測試負擔。

UTF-8 是國際化的通用字碼頁，並支援使用1-6 位元組可變寬度編碼的所有 Unicode 程式碼點。 它是在 web 上使用 pervasively，而且是 * nix 架構平臺的預設值。

## <a name="-a-vs--w-apis"></a>-A 與-W Api
  
Win32 Api 通常支援-A 和-W 變體。

-變數會辨識系統上設定的 ANSI 字碼頁，並支援 `char*`，而-W 變體會以 UTF-16 運作並支援 `WCHAR`。

到目前為止，Windows 已強調「Unicode」-W variant over-A Api。 不過，最新的版本已使用 ANSI 字碼頁和-Api 做為對應用程式引進 UTF-8 支援的方法。 如果 ANSI 字碼頁是針對 UTF-8 所設定，則 Api 會以 UTF-8 運作。 此模型的優點是可支援使用建立的現有程式碼，而不需要變更任何程式碼。

## <a name="set-a-process-code-page-to-utf-8"></a>將處理常式字碼頁設定為 UTF-8

從 Windows 1903 版（可能是2019更新），您可以在 package.appxmanifest.xml 中使用已封裝應用程式的 ActiveCodePage 屬性，或針對未封裝應用程式的融合資訊清單，強制程式使用 UTF-8 做為進程程式字碼頁面。

您可以在舊版 Windows 組建上宣告此屬性和目標/執行，但您必須照常處理舊版字碼頁偵測和轉換。 使用 Windows 1903 版的最低目標版本時，程式字碼頁一律為 UTF-8，因此可以避免使用舊版字碼頁偵測和轉換。

## <a name="examples"></a>範例

**已封裝應用程式的 Appx 資訊清單：**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**未封裝之 Win32 應用程式的融合資訊清單：**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> 從命令列將資訊清單新增至現有的可執行檔，並 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>字碼頁轉換

當 Windows 在 UTF-16 （`WCHAR`）中以原生方式運作時，您可能需要將 UTF-8 資料轉換成 UTF-16 （反之亦然），才能與 Windows Api 互通。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)和[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte)可讓您在 utf-8 和 utf-16 （`WCHAR`）之間轉換（以及其他字碼頁）。 當舊版 WIN32 API 可能只瞭解 `WCHAR`時，這特別有用。 這些函式可讓您將 UTF-8 輸入轉換成 `WCHAR` 以傳入-W API，並在必要時將任何結果轉換回來。
當使用這些函式時，`CodePage` 設定為 `CP_UTF8`時，請使用 `0` 或 `MB_ERR_INVALID_CHARS`的 `dwFlags`，否則會發生 `ERROR_INVALID_FLAGS`。

注意：只有在 Windows 1903 版（可能是2019更新）或更新版本上執行，且上述 ActiveCodePage 屬性設定為 UTF-8 時，`CP_ACP` 等同于 `CP_UTF8`。 否則，它會接受舊版系統字碼頁。 我們建議您明確使用 `CP_UTF8`。

## <a name="related-topics"></a>相關主題

- [字碼頁](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [字碼頁識別碼](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
