---
Description: 使用 utf-8 字元編碼的 web 應用程式之間的最佳相容性和 * nix 基礎平台 （Unix、 Linux 和變體），當地語系化的錯誤降至最低並減少額外負荷測試。
title: 使用 Windows utf-8 字碼頁
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: a9386b31d16796c68d41a27ab48a5b2c9a9a342b
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141810"
---
# <a name="use-the-utf-8-code-page"></a>使用 UTF-8 字碼頁

使用[utf-8](http://www.utf-8.com/)字元編碼之間的 web 應用程式和其他最佳相容性 * nix 基礎平台 （Unix、 Linux 和變體），當地語系化的錯誤降至最低並減少額外負荷測試。

Utf-8 是國際化的通用的字碼頁，並支援所有的 Unicode 字碼指標，使用 1 至 4 位元組變動寬度的編碼方式。 它在 web 上，pervasively 使用，而且是預設值 * nix 為基礎的平台。

## <a name="-a-vs--w-apis"></a>-W Api 與-A
  
Win32 Api 通常會支援-和-W 變體。

-A 變體辨識設定上的系統和支援的 ANSI 字碼頁`char*`，而-W 的 variant 操作 8、UTF-16 和支援`WCHAR`。

直到最近，Windows 便-Api 透過強調"Unicode"-W 變體。 不過，最新版本已使用的 ANSI 字碼頁，而-導入 utf-8 做為 Api 應用程式支援。 如果設定的 ANSI 字碼頁 utf-8，-Api 操作 utf-8。 此模型的好處是能夠支援使用-A Api 而不需要變更任何程式碼所建置的現有程式碼。

## <a name="set-a-process-code-page-to-utf-8"></a>處理程序的字碼頁設定為 utf-8

從 Windows 版 1903 （可能 2019年更新），您可以使用 ActiveCodePage 屬性中，appxmanifest 已封裝應用程式，或未封裝的應用程式，fusion 資訊清單來強制執行的程序的處理程序的字碼頁為使用 utf-8。

您可以宣告這個屬性和目標/執行舊版 Windows 上建置，但您必須如往常般處理舊版程式碼頁偵測和轉換。 最小的目標版本的 Windows 版本 1903年，程序的字碼頁一律為 utf-8 可以避免舊版程式碼頁面偵測和轉換。

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

**解除封裝的 Win32 應用程式的融合資訊清單：**

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
> 加入現有的可執行檔中的資訊清單，從命令列 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>字碼頁轉換

與 Windows 原生方式在 utf-16 (`WCHAR`)，您可能需要為 utf-16 （反之亦然） 的 utf-8 資料轉換成與 Windows Api 交互操作。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)並[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte)可讓您將轉換 utf-8 與 utf-16 之間 (`WCHAR`) （和其他字碼頁）。 這會特別有用，在舊版 Win32 API 可能只了解`WCHAR`。 這些函式可讓您轉換至 utf-8 輸入`WCHAR`傳入 -W API，並只有在必要時回復，然後將轉換的任何結果。
使用這些函式時`CodePage`設為`CP_UTF8`，使用`dwFlags`任一`0`或是`MB_ERR_INVALID_CHARS`，否則為`ERROR_INVALID_FLAGS`，就會發生。

注意︰`CP_ACP`等同於`CP_UTF8`前提 （可能 2019年更新） 的 Windows 版本 1903年上執行或更新版本，且上述 ActiveCodePage 屬性設定為 utf-8。 否則，它會接受舊有系統字碼頁。 我們建議使用`CP_UTF8`明確。

## <a name="related-topics"></a>相關主題

- [字碼頁](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [字碼頁識別項](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
