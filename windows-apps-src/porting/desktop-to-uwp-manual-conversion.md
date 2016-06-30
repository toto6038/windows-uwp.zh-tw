---
Description: "說明如何將 Windows 傳統型應用程式 (例如，Win32、WPF 及 Windows Forms) 手動轉換為通用 Windows 平台 (UWP) 應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: "將 Windows 傳統型應用程式手動轉換為通用 Windows 平台 (UWP) 應用程式"
ms.sourcegitcommit: 606d5237cb67cb4439704f81b180c3c48cc1556f
ms.openlocfilehash: be7599403e78c8db7ba91fe5a7c25b1c1a1ab644

---

# 將您的 Windows 傳統型應用程式手動轉換為通用 Windows 平台 (UWP) 應用程式

\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

使用轉換器相當簡便且自動化，如果您對於安裝程式所做的一切有任何不確定，這相當有用。 但如果您的應用程式是使用 xcopy 所安裝，或者您已熟悉應用程式的安裝程式對於系統所做的變更，您可以選擇手動建立應用程式套件和資訊清單。

以下是手動建立 Centennial 套件的步驟︰

## 手動建立資訊清單。

您的 _appxmanifest.xml_ 檔案必須具有下列內容 (基本內容)。 將已格式化的預留位置 (例如 ***THIS***) 變更為適用於應用程式的實際值。

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
    ```

## 執行 MakeAppX 工具

使用[應用程式封裝工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)，為您的專案產生 AppX。 MakeAppx.exe 隨附於 Windows 10 SDK。 

若要執行 MakeAppx，必須先確定您已建立資訊清單檔案，如上方所述。 

接著，建立對應檔案。 檔案的開頭應該是 **[Files]**，然後列出磁碟上的每個原始程式檔，之後接著它們在套件中的目的地路徑。 以下是範例： 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

最後，執行下列命令： 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## 簽署您的 AppX 套件

Add-AppxPackage Cmdlet 要求部署的應用程式套件 (.appx) 必須進行簽署。 使用 [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx) (其隨附於 Microsoft Windows 10 SDK) 來簽署 .appx 套件。

用法範例： 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

當您執行 MakeCert.exe，且系統要求您輸入密碼時，請選取 \[無\]。 如需憑證和簽章的詳細資訊，請參閱下列各項︰ 

- [做法︰建立開發時要使用的暫時憑證](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe (簽署工具)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Jun16_HO4-->


