---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: 封裝 UWP 應用程式
description: 若要發佈或銷售您的通用 Windows 平台 (UWP) 應用程式，您必須為其建立應用程式套件。
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: d5ed75cb79488eb994135dcfef74483ec078a32e
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58173024"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>使用 Visual studio 封裝 UWP app

若要銷售通用 Windows 平台 (UWP) app 或將其散發給其他使用者，您必須封裝應用程式。 如果不想透過 Microsoft Store 散發應用程式，您可以直接將應用程式套件側載至裝置或透過 [Web 安裝](installing-UWP-apps-web.md)散發。 本文描述使用 Visual Studio 設定、建立和測試 UWP app 套件的程序。 如需管理和部署線 (LOB) 應用程式的相關資訊，請查看[企業應用程式管理](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)。

在 Windows 10 中，您可以提交應用程式套件、 應用程式套件組合或若要完成的應用程式套件上傳檔案[合作夥伴中心](https://partner.microsoft.com/dashboard)。 這些選項，提交應用程式套件上傳檔案將會提供最佳體驗。

## <a name="types-of-app-packages"></a>應用程式套件類型

- **應用程式套件 （.appx 或.msix）**  
    檔案，包含可側載在裝置上的格式的應用程式。 Visual Studio 所建立的任何單一應用程式套件檔案**不**要提交到合作夥伴中心和應該使用的側載功能，而且僅限於測試目的。 如果您想要應用程式提交到合作夥伴中心，請使用 應用程式套件上傳檔案。  

- **應用程式配套 （.appxbundle 或.msixbundle）**  
    應用程式套件組合是一種套件，可以包含多個應用程式套件，每一個為了支援特定裝置架構而建置。 例如應用程式套件組合可以包含適用於 x86、x64 及 ARM 設定的三個不同的應用程式套件。 應該盡可能產生應用程式套件組合，因為它們允許您的應用程式用於最多種類的裝置。  

- **應用程式套件上傳檔案 （.appxupload 或.msixupload）**  
    單一檔案，可以包含多個應用程式套件或單一應用程式套件組合以支援各種處理器架構。 應用程式套件上傳檔案也包含符號檔要[分析應用程式效能](https://docs.microsoft.com/windows/uwp/publish/analytics)Microsoft Store 中發佈您的應用程式之後。 若要區分，用意在於將它提交至合作夥伴中心中，發行封裝您的應用程式，使用 Visual Studio，這個檔案，將會自動建立為您。

以下是準備與建立應用程式套件的步驟概觀：

1.  [封裝您的應用程式之前](#before-packaging-your-app)。 請遵循下列步驟以確保您的應用程式已準備好封裝以合作夥伴中心提交。
2.  [設定應用程式套件](#configure-an-app-package)。 請使用 Visual Studio 資訊清單設計工具來設定套件。 例如，新增磚影像，然後選擇您的應用程式支援的方向。
3.  [建立應用程式套件上傳檔案](#create-an-app-package-upload-file)。 使用 Visual Studio 應用程式套件精靈，接著使用 Windows 應用程式認證套件認證您的套件。
4.  [側載您的應用程式套件](#sideload-your-app-package)。 將您的應用程式側載到裝置後，您可以測試它是否如預期運作。

完成上述步驟之後，您已經準備好散發應用程式。 如果您有您不計畫銷售，因為它是僅供內部使用者的特定業務 (LOB) 應用程式時，您可以側載在任何 Windows 10 裝置上安裝此應用程式。

## <a name="before-packaging-your-app"></a>封裝您的應用程式之前

1.  **測試您的應用程式。** 封裝您的應用程式合作夥伴中心提交之前，請確定它如預期般在您打算支援的所有裝置系列上運作。 這些裝置系列可能包含桌上型電腦、行動裝置、Surface Hub、Xbox、IoT 裝置或其他等。 如需有關部署和測試您的應用程式使用 Visual Studio 的詳細資訊，請參閱[部署和偵錯 UWP 應用程式](../debug-test-perf/deploying-and-debugging-uwp-apps.md)。
2.  **最佳化您的應用程式。** 您可以使用 Visual Studio 的分析與偵錯工具來最佳化您的 UWP 應用程式的效能。 例如，UI 回應性時間軸工具、記憶體使用量工具及 CPU 使用量工具等。 如需這些工具的詳細資訊，請參閱[程式碼剖析功能之旅](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour)主題。
3.  **檢查.NET 原生相容性 (若為 VB 和C#應用程式)。** 在通用 Windows 平台中，有原生編譯器可改善您的應用程式的執行階段效能。 由於這項變更，您應該在此編譯環境中測試您的 app。 根據預設，**Release** 組建組態可啟用 .NET 原生工具鏈，因此請務必使用這個 **Release** 組態測試您的 app，確認您的 app 是否如預期般運作。 [偵錯 .NET Native Windows 通用 app](https://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx)詳細說明一些使用 .NET Native 可能會發生的常見偵錯問題。

## <a name="configure-an-app-package"></a>設定 app 套件

應用程式資訊清單檔案 (Package.appxmanifest) 是包含建立應用程式套件所需之屬性和設定的 XML 檔案。 例如，app 資訊清單檔案中的屬性描述做為您的應用程式磚的影像，以及當使用者旋轉裝置時您的應用程式支援的方向。

Visual Studio 資訊清單設計工具可讓您輕鬆更新資訊清單檔案而不需要編輯檔案的原始 XML。

**使用資訊清單設計工具設定套件**

1.  在 [方案總管] 中，展開您的 UWP app 的專案節點。
2.  按兩下 [Package.appxmanifest] 檔案。 如果資訊清單檔案已在 XML 程式碼檢視中開啟，Visual Studio 會提示您關閉檔案。
3.  現在您可以決定如何設定您的 app。 每個索引標籤均包含可設定有關您的 app 的資訊以及必要的詳細資訊的連結。  
    ![Visual Studio 中的資訊清單設計工具](images/packaging-screen1.jpg)

    在 [視覺資產] 索引標籤上確認您擁有 UWP app 需要的所有影像。

    從 [封裝] 索引標籤中，您可以輸入發佈資料。 從此處您可以選擇要用來登入您的 app 的憑證。 所有 UWP 應用程式都必須以憑證簽署。

    >[!IMPORTANT]
    >如果您要在 Microsoft Store 發佈應用程式，將會為您以受信任的憑證簽署應用程式。 這可讓使用者安裝及執行您的應用程式，而不需安裝相關聯的應用程式簽署憑證。

    如果您不要發佈應用程式，只是想要側載應用程式套件，首先需要信任套件。 憑證必須安裝在該使用者裝置上，才能信任該套件。 如需側載的詳細資訊，請參閱[啟用您的裝置以用於開發](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。

4.  在對您的 app 進行必要的編輯之後，請儲存 **Package.appxmanifest** 檔案。

如果在散發您的應用程式，透過 Microsoft Store 時，Visual Studio 可以與市集建立關聯您的套件。 若要這樣做，請以滑鼠右鍵按一下方案總管] 中的專案名稱，然後選擇 [**存放區**->**將應用程式建立關聯的存放區**。 您也可以執行這**建立應用程式套件**精靈 中下, 一節中所述。 建立應用程式關聯時，會自動更新資訊清單設計工具的 [封裝] 索引標籤中的一些欄位。

## <a name="create-an-app-package-upload-file"></a>建立應用程式套件上傳檔案

若要散發透過 Microsoft Store 應用程式，您必須建立應用程式套件 （.appx 或.msix）、 應用程式配套 （.appxbundle 或.msixbundle） 或應用程式封裝上傳檔案 （.appxupload 或.msixupload） 和[提交已封裝應用程式合作夥伴中心](https://docs.microsoft.com/windows/uwp/publish/app-submissions). 雖然可以提交到單獨的合作夥伴中心應用程式套件或應用程式套件組合，但我們建議您提交應用程式套件上傳檔案。 您可以使用，以建立應用程式套件上傳檔案**建立應用程式套件**精靈，Visual Studio 中，或者您可以建立一個以手動方式從現有的應用程式套件或應用程式套件組合。

>[!NOTE]
> 如果您想要以手動方式建立應用程式套件 （.appx 或.msix） 或應用程式配套 （.appxbundle 或.msixbundle），請參閱[MakeAppx.exe 工具建立應用程式套件](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

### <a name="to-create-your-app-package-upload-file-using-visual-studio"></a>若要建立您使用 Visual Studio 的應用程式套件上傳檔案

1.  在 [方案總管] 中，開啟您的 UWP app 專案的方案。
2.  在專案上按一下滑鼠右鍵，然後選擇 **\[Microsoft Store\]**->**\[建立應用程式套件\]**。 如果此選項停用或未顯示，請確定專案是通用 Windows 專案。  
    ![瀏覽至 建立應用程式套件的內容功能表](images/packaging-screen2.jpg)

    **\[建立應用程式套件\]** 精靈便會出現。

3.  選取 [**我想要建立套件以上傳到使用新的應用程式名稱的 Microsoft Store**中第一個對話方塊，然後按一下**下一步]**。  
    ![建立您的套件 對話方塊視窗中顯示](images/packaging-screen3.jpg)

    如果您已經有關聯您的專案與應用程式存放區中，您也可以建立相關聯的市集應用程式套件。 如果您選擇**我想要建立封裝以側載**，Visual Studio 將不會產生應用程式套件上傳 （.msixupload 或.appxupload） 檔案合作夥伴中心提交。 如果您只想側載您的應用程式以在內部裝置上執行或做為測試目的，您可以選取此選項。 如需側載的詳細資訊，請參閱[啟用您的裝置以用於開發](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
4.  在下一步 頁面上，使用您開發人員帳戶登入合作夥伴中心。 如果您還沒有開發人員帳戶，精靈會幫助您建立一個。
    ![建立應用程式封裝 視窗以顯示選取的應用程式名稱](images/packaging-screen4.jpg)
5.  從目前已註冊您的帳戶，應用程式清單中選取您封裝應用程式的名稱，或保留新的如果您有保留一個在合作夥伴中心。  
6.  確定您在 **\[選取並設定套件\]** 對話方塊中選取全部的三種架構設定 (x86、x64 及 ARM)，以確保 app 部署到最多種類的裝置。 在 [產生應用程式套件組合] 清單方塊中，選取 [一律]。 應用程式配套 （.appxbundle 或.msixbundle） 建議，透過單一的應用程式套件檔案因為它包含每種類型的處理器架構所設定的應用程式套件的集合。 當您選擇要產生應用程式套件組合時，應用程式套件組合會包含在最終的應用程式封裝上傳 （.appxupload 或.msixupload） 檔案以及偵錯和當機分析的資訊。 如果您不確定選擇哪些架構，或想要深入了解各種裝置所使用的架構，請查看[應用程式套件架構](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)。  
    ![使用封裝組態顯示建立應用程式封裝 視窗](images/packaging-screen5.jpg)
7.  包含完整的 PDB 符號檔才能[分析應用程式效能](https://docs.microsoft.com/windows/uwp/publish/analytics)從已發行您的應用程式之後，合作夥伴中心。 設定任何其他詳細資料，例如版本編號或套件輸出位置。
9.  按一下 **\[建立\]** 產生應用程式套件。 如果您選取其中一個**我想要建立套件以上傳到 Microsoft Store**步驟 3 中的選項，並建立合作夥伴中心提交套件，精靈會建立封裝上傳 （.appxupload 或.msixupload） 檔案。 如果您選取**我想要建立封裝以側載**在步驟 3 中，精靈會建立單一應用程式套件或應用程式套件組合，根據您在步驟 6 中的選取項目。
10. 當您的應用程式封裝成功時，您會看到這個對話方塊，您可以從指定的輸出位置來擷取您的應用程式套件上傳檔案。 此時，您可以[驗證您的應用程式套件，在本機電腦或遠端電腦上](#validate-your-app-package)。
    ![封裝建立完成視窗所顯示的驗證選項](images/packaging-screen6.jpg)

### <a name="to-create-your-app-package-upload-file-manually"></a>若要手動建立您的應用程式套件上傳檔案

1. 將下列檔案放在資料夾中：
    - 一或多個應用程式套件 （.msix 則為.appx） 或應用程式套件組合 （.msixbundle 或.appxbundle）。
    - .appxsym 檔案。 這是壓縮的.pdb 檔案，包含用於應用程式的公用符號[損毀分析](../publish/health-report.md)在合作夥伴中心。 您可以略過這個檔案，但如果您這樣做，可供您的應用程式沒有損毀分析或偵錯資訊。
2. 壓縮資料夾。
3. 將 zip 的資料夾中副檔名從.zip.msixupload 或.appxupload。

### <a name="validate-your-app-package"></a>驗證您的應用程式套件

驗證您的應用程式，再提交到合作夥伴中心為本機或遠端電腦上的認證。 您只能驗證您的應用程式套件的發行組建，而非偵錯組建。 如需有關應用程式提交至合作夥伴中心的詳細資訊，請參閱[提交應用程式時](https://docs.microsoft.com/windows/uwp/publish/app-submissions)。

**若要驗證您的應用程式套件在本機**

1. 在最終**套件建立完成**頁**建立應用程式套件**精靈 中，保留**本機**選取的選項，然後按一下**啟動Windows 應用程式認證套件**。 如需使用 Windows 應用程式認證套件測試應用程式的詳細資訊，請參閱[Windows 應用程式認證套件](https://msdn.microsoft.com/library/windows/apps/Mt186449)。

    Windows 應用程式認證套件會執行各種測試並傳回結果。 如需更具體的資訊，請參閱 [Windows 應用程式認證套件測試](https://msdn.microsoft.com/library/windows/apps/mt186450)。

    如果您有想要用於測試的遠端 Windows 10 裝置時，您必須手動安裝該裝置上的 Windows 應用程式認證套件。 下一節會帶您逐步完成下列步驟。 完成此動作之後，接著您可以選取 **\[遠端電腦\]**，按一下 **\[啟動 Windows 應用程式認證套件\]** 以連線到遠端裝置並執行驗證測試。

2. WACK 已完成，而您的應用程式已通過認證後，您就能夠應用程式提交到合作夥伴中心。 請確定您上傳的是正確的檔案。 檔案的預設位置可以找到您的解決方案的根資料夾中`\[AppName]\AppPackages`和它的副檔名為.appxupload 或.msixupload 檔案結尾。 名稱會是表單的`[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload`或`[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload`若所有選取的套件架構選擇應用程式套件組合。

**若要驗證遠端的 Windows 10 裝置上的應用程式套件**

1.  啟用 Windows 10 裝置，以進行開發[啟用您的裝置進行開發](https://msdn.microsoft.com/library/windows/apps/Dn706236)指示。
    >[!IMPORTANT]
    > 您無法驗證遠端 ARM 裝置上的應用程式套件，適用於 Windows 10。
2.  下載和安裝 Visual Studio 遠端工具。 這些工具可用來以遠端方式執行 Windows 應用程式認證套件。 您可以瀏覽[在遠端電腦上執行 UWP app](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor)，以取得關於這些工具的詳細資訊 (包括下載位置)。
3.  下載所需[Windows 應用程式認證套件](https://go.microsoft.com/fwlink/p/?LinkID=309666)然後在您的遠端 Windows 10 裝置上進行安裝。
4.  在精靈的 **\[套件建立完成\]** 頁面上，選擇 **\[遠端電腦\]** 選項按鈕，然後選擇 **\[測試連線\]** 按鈕旁的省略符號按鈕。
    >[!NOTE]
    > **遠端機器**選項按鈕已選取至少一個支援驗證的方案組態時，才提供使用。 如需使用 WACK 測試 app 的詳細資訊，請參閱 [Windows 應用程式認證套件](https://msdn.microsoft.com/library/windows/apps/Mt186449)。
5.  指定您的子網路內的裝置種類，或提供子網路以外的裝置的網域名稱伺服器 (DNS) 名稱或 IP 位址。
6.  如果您的裝置不需要您使用 Windows 認證登入，請在 **\[驗證模式\]** 清單中選擇 **\[無\]**。
7.  選擇 **\[選取\]** 按鈕，然後再選擇 **\[啟動 Windows 應用程式認證套件\]** 按鈕。 如果遠端工具在該裝置上執行，Visual Studio 會與裝置連線，接著執行驗證測試。 請參閱 [Windows 應用程式認證套件測試](https://msdn.microsoft.com/library/windows/apps/mt186450)。

## <a name="sideload-your-app-package"></a>側載您的應用程式套件

UWP 應用程式封裝、 應用程式未安裝到裝置，因為它們是使用傳統型應用程式。 一般而言，您從 Microsoft Store 下載 UWP app 時，也會同時將應用程式安裝到您的裝置。 應用程式不需要發佈到 Microsoft Store 即可安裝 (側載)。 這可讓您安裝和測試應用程式使用應用程式套件檔案所建立。 如果您不想在市集中銷售某個應用程式，例如企業營運 (LOB) 應用程式，您可以側載該應用程式，讓您公司中的其他使用者可以使用它。

您可以側載的目標裝置上的應用程式之前，您必須[啟用您的裝置進行開發](../get-started/enable-your-device-for-development.md)。

側載您的應用程式，在 Windows 10 行動裝置版裝置上，使用[WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md)工具。 為桌上型電腦、 膝上型電腦和平板電腦，請遵循下列指示。

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>側載您的應用程式封裝，在 Windows 10 年度更新版或更新版本

在 Windows 10 年度更新版 （Windows 10、 1607年版） 引進，可以只要按兩下應用程式套件檔案安裝應用程式套件。 若要使用這種情況，請瀏覽至您的應用程式套件或應用程式套件組合的檔案，然後按兩下。 [應用程式安裝程式](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root)會啟動，並提供基本的應用程式資訊，以及安裝 按鈕、 安裝進度列，以及任何相關的錯誤訊息。

![應用程式安裝程式顯示安裝稱為 Contoso 的範例應用程式](images/appinstaller-screen.png)

> [!NOTE]
> 應用程式安裝程式會假設應用程式由裝置信任。 如果要側載開發人員或企業應用程式，您必須在裝置的受信任的人或受信任的發行者憑證授權單位存放區安裝簽署憑證。 如果您不確定如何執行此動作，請參閱[安裝測試憑證](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)。

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>側載您的應用程式封裝在舊版 Windows

1.  複製要安裝至目標裝置的應用程式版本的資料夾。

    如果您已經建立應用程式套件組合，您將會有一個以版本號碼為依據的資料夾與一個 `*_Test` 資料夾。 例如下列兩個資料夾 (其中要安裝的版本是 1.0.2.0)：

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    如果您沒有應用程式套件組合，請複製正確架構的資料夾和對應的 `*_Test` 資料夾。 這兩個資料夾是 x64 架構應用程式套件及其 `*_Test` 資料夾的範例：

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  在目標裝置上，開啟 `*_Test` 資料夾。
3.  以滑鼠右鍵按一下 **Add-AppDevPackage.ps1** 檔案。 選擇 **\[用 PowerShell 執行\]**，並依照提示進行。  
    ![檔案總管瀏覽至所示的 PowerShell 指令碼](images/packaging-screen7.jpg)

    已安裝的應用程式套件，則 [PowerShell] 視窗會顯示下列訊息：**已成功安裝您的應用程式。**
    >[!TIP]
    > 若要開啟平板電腦上的捷徑功能表，觸控式的螢幕您想要以滑鼠右鍵按一下，按住直到出現完整的圓形，然後將手指的位置。 提起手指後，就會開啟捷徑功能表。

4.  按一下 [開始] 按鈕以依據名稱搜尋應用程式，然後啟動它。
