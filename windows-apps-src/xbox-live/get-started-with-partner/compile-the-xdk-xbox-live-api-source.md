---
title: 編譯 XDK Xbox Live API 來源
description: 了解如何編譯出貨與 Xbox Developer Kit (XDK) 中的 Xbox Live API 來源。
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 xdk
ms.localizationpriority: medium
ms.openlocfilehash: 9a98af637c8c60449cd2005c4fc6f83f9b0719cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660993"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>編譯 Xbox Developer Kit (XDK) Xbox Live API 來源

Xbox 開發人員 Kit (XDK) 包含用於建置 Microsoft.Xbox.Services.dll (XSAPI) 的來源。 開發人員可以遵循這些指示來更新其專案以使用本機組建的 dll。

您可能想要 XSAPI 自行建置如果：
1. 如果您想要偵錯的問題來了解錯誤程式碼來自何處。
1. 如果我們提供來源程式碼修正問題，就能將 QFE 修補程式。

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>若要自行編譯 XDK c + + XSAPI 專案

<ol>
  <li> 取得 Microsoft.Xbox.Services 來源。 若要這樣做，擷取所有的檔案從"%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox 服務 API\8.0\SourceDist\Xbox.Services.zip 」 到可寫入的資料夾之外"C:\Program Files (x86)"，或者您可以複製的來源 <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 如果您的專案參考預先建置的 DLL，您要移除的參考</li>
    <ul>
      <li> 適用於 Visual Studio 2012:選擇 [專案-> 參考...]在 Visual Studio。 如果 Xbox 服務 API 列出做為參考，請選取它，然後按一下 「 移除的參考 」。 按一下 [確定]，並儲存專案檔。</li>
      <li> Visual Studio 2015 或 2017年:選擇 「 Project]-> [新增參考...」 在 Visual Studio。 如果勾選 Xbox 服務 API，請將它取消選取。 按一下 [確定]，並儲存專案檔。</li>
    </ul>
  <li> 如果您要建置使用 XDK，選擇 「 檔案]-> [新增]-> [現有專案...」 在 Visual Studio，將下列兩個專案加入至您的應用程式方案。 Vcxproj 檔會位於您解壓縮至來源的資料夾。</li>
Visual Studio 2017: <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
Visual Studio 2015: <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
適用於 Visual Studio 2012: <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> 加入原始碼專案，做為參考透過選擇 專案-> 參考...然後選取 加入參考。 在 方案-> 專案，檢查的項目，如上述這兩個專案，然後按一下 確定。</li>
    <li> 加入您的專案中的 props 檔案，依序按一下 「 檢視-> 其他 Windows-> 屬性管理員 」，您的專案上按一下滑鼠右鍵，選取 「 加入現有屬性工作表 」，然後最後選取 SDK sourch 根目錄中的 xsapi.staticlib.props 檔案。  如果您的建置系統不支援 props 檔案，您必須手動加入的前置處理器定義和 %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\ 所示的程式庫Xbox.Services.API.Cpp.props</li>
    <li> 將 services.h 檔案加入您的應用程式來源，以滑鼠右鍵按一下專案的 新增-> 現有項目，然後選擇 {SDK root}\Include\xsapi\services.h 原始檔案</li>
    <li> 請確定應用程式專案和 Xbox 的服務專案的 「 輸出資料夾 」 相同。 此設定可在 Visual Studio 專案屬性]-> [組態屬性]-> [一般]-> [輸出目錄。</li>
    <li> 重建您的 Visual Studio 方案</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>若要自行編譯 XDK WinRT XSAPI 專案

<ol>
  <li> 取得 Microsoft.Xbox.Services 來源。 若要這樣做，您會擷取所有檔案從"%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox 服務 API\8.0\SourceDist\Xbox.Services.zip 」 到可寫入的資料夾之外"C:\Program Files (x86)"，或者您可以複製的來源 <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 如果您的專案參考預先建置的 DLL，您要移除的參考</li>
    <ul>
      <li> 適用於 Visual Studio 2012:選擇 [專案-> 參考...]在 Visual Studio。 如果 Xbox 服務 API 列出做為參考，請選取它，然後按一下 「 移除的參考 」。 按一下 [確定]，並儲存專案檔。</li>
      <li> Visual Studio 2015 或 2017年:選擇 「 Project]-> [新增參考...」 在 Visual Studio。 如果勾選 Xbox 服務 API，請將它取消選取。 按一下 [確定]，並儲存專案檔。</li>
    </ul>
  <li> 如果您要建置使用 XDK，選擇 「 檔案]-> [新增]-> [現有專案...」 在 Visual Studio，將下列兩個專案加入至您的應用程式方案。 Vcxproj 檔會位於您解壓縮至來源的資料夾。  Visual Studio 2015，專案將會自動升級為 VS2015 格式。</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> 在 Visual Studio 中新增參考：</li>
    <ul>
      <li> 適用於 Visual Studio 2012:選擇 [專案-> 參考...]然後在 Visual Studio 中選取 [加入參考]。 在方案]-> [專案，chceck 上述這兩個專案的項目，並按一下 [確定]。</li>
      <li> Visual Studio 2015 或 2017年:選擇 「 Project]-> [新增參考...」 在 Visual Studio。 在專案中，請檢查上述這兩個專案的項目並按一下 [確定]。</li>
    </ul>
  <li> 請確定應用程式專案和 Xbox 的服務專案的 「 輸出資料夾 」 相同。 此設定可在 Visual Studio 專案屬性]-> [組態屬性]-> [一般]-> [輸出目錄。</li>
  <li> 重建您的 Visual Studio 方案</li>
</ol>
