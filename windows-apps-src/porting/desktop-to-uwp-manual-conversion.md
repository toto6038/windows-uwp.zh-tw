---
author: awkoren
Description: "說明如何將 Windows 傳統型應用程式 (例如，Win32、WPF 及 Windows Forms) 手動轉換為通用 Windows 平台 (UWP) 應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: "將 Windows 傳統型應用程式手動轉換為通用 Windows 平台 (UWP) App"
translationtype: Human Translation
ms.sourcegitcommit: ee697323af75f13c0d36914f65ba70f544d046ff
ms.openlocfilehash: f55f3bd6479cdf076c51cf574b07bfb5ce3a805c

---

# <a name="manually-convert-your-app-to-uwp-using-the-desktop-bridge"></a>使用傳統型橋接器將您的 App 手動轉換為 UWP

使用 [Desktop App Converter (DAC)](desktop-to-uwp-run-desktop-app-converter.md) 相當簡便且自動化，如果您對於安裝程式所做的一切有任何不確定性，這相當有用。 但如果您的 app 是使用 xcopy 所安裝，或者您已熟悉 app 的安裝程式對於系統所做的變更，您可能想要手動建立應用程式套件和資訊清單。 本文包含開始使用的步驟。 其中也會說明如何將無背板資產新增至您的 app，而 DAC 並未涵蓋此項。 

以下是如何開始使用的方式：

## <a name="create-a-manifest-by-hand"></a>手動建立資訊清單

您的 _appxmanifest.xml_ 檔案必須具有下列內容 (基本內容)。 將已格式化的預留位置 (例如 \*\*\*THIS\*\*\*) 變更為適用於應用程式的實際值。

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

## <a name="add-unplated-assets"></a>新增無背板資產

以下是如何針對您的 app，設定要顯示於工作列上的 44x44 資產。

1. 取得正確的 44x44 影像，並將它們複製到包含您的影像 (也就是資產) 的資料夾。

2. 針對每個 44x44 影像，在相同的資料夾中建立一個複本，然後將 *.targetsize-44_altform-unplated* 附加至檔案名稱。 每個圖示應該會有兩個複本，每個均以特定方式命名。 例如，完成處理程序之後，您的資產資料夾可能包含 *MYAPP_44x44.png* 和 *MYAPP_44x44.targetsize-44_altform-unplated.png* (注意︰前者是 VisualElements 屬性 *Square44x44Logo* 下方 appxmanifest 中所參考的圖示)。 

3.  在 AppXManifest 中，針對您要修正的每個圖示，將 BackgroundColor 設為透明。 這個屬性可以在每個應用程式的 VisualElements 下方找到。

4.  開啟 CMD、將目錄變更為套件的根資料夾，然後執行命令 ```makepri createconfig /cf priconfig.xml /dq en-US``` 來建立 priconfig.xml 檔案。

5.  使用 CMD、留在套件的根資料夾中，然後使用命令 ```makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``` 來建立 resources.pri 檔案。 例如，應用程式的命令可能看起來像這樣：```makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml```。 

6.  使用下一個步驟中的指示封裝您的 AppX，以查看結果。

## <a name="run-the-makeappx-tool"></a>執行 MakeAppX 工具

使用[應用程式封裝工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)，為您的專案產生 AppX。 MakeAppx.exe 隨附於 Windows&nbsp;10 SDK。 

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

## <a name="sign-your-appx-package"></a>簽署您的 AppX 套件

Add-AppxPackage Cmdlet 要求部署的應用程式套件 (.appx) 必須進行簽署。 使用 [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx) (其隨附於 Microsoft Windows&nbsp;10 SDK) 來簽署 .appx 套件。

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




<!--HONumber=Dec16_HO1-->


