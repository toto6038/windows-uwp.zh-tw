---
author: normesta
Description: "執行您的已封裝應用程式來查看外觀，而不需要簽署。 然後，設定中斷點和逐步執行程式碼。 當您準備好在生產環境測試您的應用程式時，請簽署您的應用程式，然後再安裝它。"
Search.Product: eADQiWindows 10XVcnh
title: "執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)"
ms.author: normesta
ms.date: 06/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: c160fecc530a6366de48f4f2ecc24df2463c0469
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)

執行您的已封裝應用程式來查看外觀，而不需要簽署。 然後，設定中斷點和逐步執行程式碼。 當您準備好在生產環境測試您的應用程式時，請簽署您的應用程式，然後再安裝它。 此主題將會告訴您如何執行每個項目。

<span id="run-app" />
## <a name="run-your-app"></a>執行您的應用程式

您可以在本機執行您的應用程式並進行測試，而不需要取得憑證和進行簽署。

若您是使用 UWP 專案中 Visual Studio 建立套件，您只需將封裝專案設定為啟始專案，然後按下 CTRL+F5 即可啟動您的應用程式。

若您是使用 Desktop App Converter 或手動封裝您的應用程式，請開啟 Windows PowerShell 命令提示字元，並從您輸出資料夾的 **PacakgeFiles** 子資料夾中執行此 Cmdlet︰

```
Add-AppxPackage –Register AppxManifest.xml
```
若要啟動您的應用程式，請在 Windows \[開始\] 功能表中找到您的應用程式。

![在 \[開始\] 功能表中的已封裝應用程式](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> 封裝的應用程式總是會以互動使用者的身分執行，您安裝已封裝應用程式的任何磁碟機都必須格式化為 NTFS 格式。

## <a name="debug-your-app"></a>偵錯您的應用程式

每次偵錯您的應用程式時在對話方塊中選取您的套件，或是安裝擴充功能，就不需要在偵錯應用程式時每次啟動工作階段時都需要選取套件。

### <a name="debug-your-app-by-selecting-the-package"></a>透過選取套件來偵錯您的應用程式

此選項僅需最短的安裝時間，但您必須在每次開始偵錯工作階段時執行一道額外的步驟。


1. 請務必至少啟動您的已封裝應用程式一次，讓它安裝在您的本機電腦上。

   請參閱上述[執行您的應用程式](#run-app)一節。

2. 啟動 Visual Studio。

   若您想要以提升權限偵錯您的應用程式，您可以利用**以系統管理員身分執行**選項來啟動 Visual Studio。

3. 在 Visual Studio 中，選擇 **\[偵錯\]**->**\[其他偵錯目標\]**->**\[偵錯已安裝的應用程式套件\]**。

4. 在 **\[已安裝的應用程式套件\]** 清單中，選取您的應用程式套件，然後選擇 **\[附加\]** 按鈕。


### <a name="debug-your-app-without-having-to-select-the-package"></a>在不需要選取套件的情況下偵錯您的應用程式

此選項需要花費最多的安裝時間，但您不需要在每次啟動應用程式時選取已安裝的套件。 您需要先安裝 [Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/) 才能使用這種方式。

1. 首先，安裝[傳統型橋接器偵錯專案](http://go.microsoft.com/fwlink/?LinkId=797871)。

2. 啟動 Visual Studio，並開啟傳統型應用程式專案。

6. 將 **\[傳統型橋接器偵錯\]** 專案新增至您的方案。

   您可以在已安裝的範本內的 **\[其他專案類型\]** 群組中找到專案範本。

    ![alt](images/desktop-to-uwp/debug-2.png)

    **\[傳統型橋接器偵錯\]** 專案將會出現在您的方案中。

    ![alt](images/desktop-to-uwp/debug-3.png)

7. 開啟 **\[傳統型橋接器偵錯\]** 專案的屬性頁面。

8. 將 **\[套件配置\]** 欄位設定為您套件資訊清單檔案 (AppxManifest.xml) 所在的位置，然後從 **\[開始磚\]** 下拉式清單中選擇您應用程式的可執行檔。

     ![alt](images/desktop-to-uwp/debug-4.png)

8. 在程式碼編輯器中開啟 AppXPackageFileList.xml 檔案。

9. 取消註解 XML 區塊並為這些元素新增數值︰

   **MyProjectOutputPath**︰您傳統型應用程式之偵錯資料夾的相對路徑。

   **LayoutFile**︰位於您傳統型應用程式之偵錯資料夾中的可執行檔。

   **PackagePath**︰在轉換過程中複製到您 Windows 應用程式套件資料夾，您傳統型應用程式之可執行檔的完整名稱。

    這裡提供一個範例：

    ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
     <MyProjectOutputPath>..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    </PropertyGroup>
    <ItemGroup>
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.exe">
        <PackagePath>$(PackageLayout)\MyDesktopApp.exe</PackagePath>
      </LayoutFile>
    </ItemGroup>
  </Project>
    ```

  若您的應用程式會消耗您方案中別的專案所產生的 dll 檔案，而您想要逐步執行這些 dll 中所含的程式碼，您可以針對這些 dll 檔案個別建立一個 **LayoutFile** 元素。

  ```XML
  ...
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.Models.dll">
      <PackagePath>$(PackageLayout)\MyDesktopApp.Models.dll</PackagePath>
      </LayoutFile>
  ...
  ```

10. 將封裝專案設定為啟始專案。  

    ![alt](images/desktop-to-uwp/debug-5.png)

11. 在您的傳統型應用程式程式碼中設定中斷點，然後啟動偵錯工具。

  ![偵錯按鈕](images/desktop-to-uwp/debugger-button.png)

  Visual Studio 會將您在 XML 檔案中指定的可執行檔和 dll 檔案複製到您的 Windows 應用程式套件，並啟動偵錯工具。

#### <a name="handle-multiple-build-configurations"></a>處理多個組建設定

若您已經定義多個組建設定 (例如︰發行版本和偵錯版本)，您可以修改 AppXPackageFileList.xml 檔案以只複製那些您在啟動偵錯工具時，於 Visual Studio 中選擇之組建設定所需要的檔案。

這裡提供一個範例。

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'Debug'">..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'Release'"> ..\MyDesktopApp\bin\Release</MyProjectOutputPath>
</PropertyGroup>
```

#### <a name="debug-uwp-enhancements-to-your-app"></a>偵錯針對您應用程式所做的 UWP 美化

您可能會想要使用一些現代化體驗 (例如動態磚等) 來美化您的應用程式。 若您想要進行這項工作，您可以使用條件式編譯，以針對特定組建設定啟用程式碼路徑。

1. 首先，在 Visual Studio 中，定義一個組建設定，並將其命名為例如「DesktopUWP」。

2. 在您專案之專案屬性內的 **\[組建\]** 索引標籤，將該名稱新增至 **\[條件式編譯符號\]** 欄位。

     ![alt](images/desktop-to-uwp/debug-8.png)

3. 新增條件式程式碼區塊。 這段程式碼只會為 **DesktopUWP** 組建設定進行編譯。

    ```csharp
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

### <a name="debug-the-entire-app-lifecycle"></a>偵錯整個應用程式的生命週期

在某些情況下，您可能會想對偵錯處理程序進行更細微的控制，包括能夠在啟動應用程式之前進行偵錯。

您可以使用 [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 取得對應用程式生命週期的完整掌控權，包括暫止、繼續，與終止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 隨附於 Windows SDK。


### <a name="modify-your-app-in-between-debug-sessions"></a>在偵錯工作階段之間修改您的應用程式

若您變更您的應用程式以修正問題，請使用 MakeAppx 工具重新封裝。 請參閱[執行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

## <a name="test-your-app"></a>測試您的應用程式

當您準備好在逼真的設定下測試您的應用程式以準備散布時，我們建議您簽署您的應用程式並安裝它。

若您是使用 Visual Studio 來封裝應用程式，您可以執行指令碼來簽署您的應用程式，然後再安裝它。 請參閱[側載您的套件](../packaging/packaging-uwp-apps.md#sideload-your-app-package)。

如果您是使用 Desktop App Converter 封裝您的應用程式，您可以使用 ``sign`` 參數來自動使用產生的憑證簽署應用程式。 您必須先安裝該憑證，然後再安裝應用程式。 請參閱[執行封裝後的應用程式](desktop-to-uwp-run-desktop-app-converter.md#run-app)。   

您也可以手動簽署您的應用程式。 作法如下

1. 建立憑證。 請參閱[建立憑證](../packaging/create-certificate-package-signing.md)。

2. 將該憑證安裝到您系統上的**「受信任的根」**或**「受信任的人」**憑證存放區。

3. 使用該憑證來簽署您的應用程式，請參閱[使用 SignTool 簽署應用程式套件](../packaging/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 請確定您憑證的發行者名稱符合您應用程式的發行者名稱。

### <a name="related-sample"></a>相關範例

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>針對 Windows 10 S 測試您的應用程式

在發行您的應用程式之前，請確保它可在執行 Windows 10 S 的裝置上正常運作。事實上，如果您計畫在 Windows 市集發行應用程式，則必須執行此步驟，因為這是市集需求。 無法在執行 Windows 10 S 的裝置上正確運作的應用程式，無法獲得認證。 

請參閱[針對 Windows 10 S 測試您的 Windows 應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器內執行另一個處理程序

您可以在指定應用程式套件的容器內叫用自訂處理序。 這對於測試案例非常有用 (例如，如果您擁有自訂的測試載入器，而且想要測試 app 的輸出)。 若要這樣做，請使用 ```Invoke-CommandInDesktopPackage``` PowerShell Cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>後續步驟

**尋找特定問題的解答**

我們的團隊會監視這些 [StackOverflow 標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供有關本文的意見反應**

使用下方的留言區塊。
