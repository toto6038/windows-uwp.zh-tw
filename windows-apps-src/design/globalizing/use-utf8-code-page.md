---
description: 使用 UTF-8 字元編碼，在 web 應用程式與其他以 \* nix 為基礎的平臺之間取得最佳相容性 (Unix、Linux 和變化) 、將當地語系化錯誤降至最低，並降低測試額外負荷。
title: 使用 Windows UTF-8 字碼頁
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 8aefe7bc45bc7c41347fe8fc4b8192347c3e4354
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034301"
---
# <a name="use-the-utf-8-code-page"></a>使用 UTF-8 字碼頁

使用 [utf-8](http://www.utf-8.com/) 字元編碼，在 web 應用程式與其他以 \* nix 為基礎的平臺之間取得最佳相容性 (Unix、Linux 和變化) 、將當地語系化錯誤降至最低，並降低測試額外負荷。

UTF-8 是國際化的通用字碼頁，而且能夠將整個 Unicode 字元集編碼。 它是在網路上 pervasively 使用，而且是 * nix 架構平臺的預設值。

> [!NOTE]
> 編碼的字元需要1到4個位元組。 UTF-8 編碼支援較長的位元組序列，最多6個位元組，但 Unicode 6.0 (U + 10FFFF) 的最大程式碼點只需要4個位元組。

## <a name="-a-vs--w-apis"></a>-A 與-W Api
  
Win32 Api 通常支援-A 和-W 變體。

-Variant 會辨識系統上設定的 ANSI 字碼頁，並支援 `char*` ，而-W 變數則會以 utf-16 和支援來運作 `WCHAR` 。

在最近，Windows 已強調「Unicode」-W 變異的 Api。 不過，最新的版本已使用 ANSI 字碼頁和-Api 作為將 UTF-8 支援引入應用程式的方法。 如果 ANSI 字碼頁設定為 UTF-8，則 Api 會在 UTF-8 中運作。 此模型的優點是，不需要變更任何程式碼，就能支援以 Api 建立的現有程式碼。

## <a name="set-a-process-code-page-to-utf-8"></a>將處理常式字碼頁設定為 UTF-8

從 Windows 版本 1903 (2019 更新) ，您可以在 package.appxmanifest 中使用已封裝應用程式的 ActiveCodePage 屬性，或針對未封裝的應用程式使用融合資訊清單，以強制處理常式使用 UTF-8 作為進程字碼頁面。

您可以宣告此屬性並在舊版 Windows 組建上執行，但是您必須如往常般處理舊版字碼頁偵測和轉換。 使用最低目標版本的 Windows 1903 版，程式字碼頁面一律為 UTF-8，因此可以避免舊版字碼頁的偵測和轉換。

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

**未封裝的 Win32 應用程式的融合資訊清單：**

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
> 從命令列將資訊清單新增至現有的可執行檔 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>字碼頁轉換

當 Windows 在 UTF-16 () 中以原生方式運作時 `WCHAR` ，您可能需要將 utf-8 資料轉換為 utf-16 (或反之亦然) 以與 Windows api 互通。

[MultiByteToWideChar](/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) 和 [WideCharToMultiByte](/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) 可讓您在 utf-8 和 utf-16 (`WCHAR`)  (和其他字碼頁) 之間進行轉換。 這在舊版 WIN32 API 可能只瞭解時特別有用 `WCHAR` 。 這些函式可讓您將 UTF-8 輸入轉換成 `WCHAR` 以傳遞至 W API，然後在必要時將任何結果轉換回來。
使用將這些函 `CodePage` 式設定為時 `CP_UTF8` ，使用 `dwFlags` 或的 `0` `MB_ERR_INVALID_CHARS` ，否則 `ERROR_INVALID_FLAGS` 會發生。

> [!NOTE]
> `CP_ACP` 等同于 `CP_UTF8` 在 Windows 1903 版上執行 (2019 更新) 或更新版本，且上述 ActiveCodePage 屬性設定為 utf-8。 否則，它會接受舊版系統字碼頁。 我們建議您 `CP_UTF8` 明確地使用。

## <a name="related-topics"></a>相關主題

- [字碼頁](/windows/desktop/Intl/code-pages)
- [字碼頁識別碼](/windows/desktop/Intl/code-page-identifiers)
