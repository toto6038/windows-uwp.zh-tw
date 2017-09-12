---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: "封裝 UWP 應用程式"
description: "若要發佈或銷售您的通用 Windows 平台 (UWP) 應用程式，您必須為其建立應用程式套件。"
ms.author: lahugh
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c6aa08c8c2e62bfed388000944de5520cbe58501
ms.sourcegitcommit: 63c815f8c6665872987b5410cabf324f2b7e3c7c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2017
---
# <a name="package-a-uwp-app-with-visual-studio"></a>使用 Visual studio 封裝 UWP app

\[ 已更新 Windows10 上的 UWP app。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

若要銷售您的通用 Windows 平台 (UWP) app 或將其提供給其他使用者，您必須為其建立 UWP app 套件。 如果不想透過市集發佈應用程式，您可以直接將應用程式套件側載至裝置。 本文描述使用 Visual Studio 設定、建立和測試 UWP app 套件的程序。 如需有關側載企業和特定業務 (LOB) 應用程式的相關資訊，請參閱[在 Windows 10 中側載 LOB 應用程式](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10)。

在 Windows 10 中，您可以上傳應用程式套件 (.appx)、應用程式套件組合 (.appxbundle) 或完整的上傳檔案 (.appxupload)。 .Appxupload 檔案是同時包含應用程式套件或套件組合以及其他重要偵錯資訊的資料夾。 強烈建議您提交 .appxupload 檔案。 將您的應用程式套件提交至市集後，您的應用程式就可供在 Windows 10 裝置上安裝及執行。 

以下是準備與建立應用程式套件的步驟概觀。

1.  [封裝您的應用程式之前](#before-packaging-your-app)。 請依照這些步驟執行，確認您的應用程式已可封裝以提交至市集。
2.  [設定應用程式套件](#configure-an-app-package)。 請使用資訊清單設計工具來設定套件。 例如，新增磚影像，然後選擇您的應用程式支援的方向。
3.  [建立應用程式套件](#create-an-app-package)。 使用 Visual Studio 應用程式套件精靈來建立應用程式套件。 然後使用 Windows 應用程式認證套件來認證您的套件。
4.  [側載您的應用程式套件](#sideload-your-app-package)。 將您的應用程式側載到裝置後，您可以測試它是否正常運作。

完成上述步驟之後，您已經準備好將您的應用程式發佈至市集。 如果您有僅供內部使用而不打算銷售的特定業務 (LOB) 應用程式，您可以側載這個應用程式，以將它安裝到任何 Windows 10 裝置上。

## <a name="before-packaging-your-app"></a>封裝您的應用程式之前

1.  **測試您的應用程式。** 封裝您的應用程式以提交到市集之前，請確定它在您計畫支援的所有裝置系列上可如預期般運作。 這些裝置系列可能包含桌上型電腦、行動裝置、Surface Hub、Xbox、IoT 裝置或其他等。
2.  **最佳化您的應用程式。** 您可以使用 Visual Studio 的分析與偵錯工具來最佳化您的 UWP 應用程式的效能。 例如，UI 回應性時間軸工具、記憶體使用量工具及 CPU 使用量工具等。 如需這些工具的詳細資訊，請參閱[執行診斷工具但不偵錯](https://msdn.microsoft.com/library/dn957936.aspx)。
3.  **檢查 .NET 原生的相容性 (適用於 VB 和 C# 應用程式)。** 在通用 Windows 平台中，有原生編譯器可改善您的應用程式的執行階段效能。 由於這項變更，強烈建議您在此編譯環境中測試您的 app。 根據預設，**Release** 組建組態可啟用 .NET 原生工具鏈，因此請務必使用這個 **Release** 組態測試您的 app，確認您的 app 是否如預期般運作。 [偵錯 .NET Native Windows 通用 app](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx)詳細說明一些使用 .NET Native 可能會發生的常見偵錯問題。

## <a name="configure-an-app-package"></a>設定應用程式套件

應用程式資訊清單檔案 (Package.appxmanifest) 是包含建立應用程式套件所需之屬性和設定的 XML 檔案。 例如，資訊清單檔案中的屬性描述做為您的應用程式磚的影像，以及當使用者旋轉裝置時您的應用程式支援的方向。

Visual Studio 資訊清單設計工具可讓您輕鬆更新資訊清單檔案而不需要編輯檔案的原始 XML。

**使用資訊清單設計工具設定套件**

1.  在 **\[方案總管\]** 中，展開您的 UWP app 的專案節點。
2.  按兩下 **\[Package.appxmanifest\]** 檔案。 如果資訊清單檔案已在 XML 程式碼檢視中開啟，Visual Studio 會提示您關閉檔案。
3.  現在您可以決定如何設定您的 app。 每個索引標籤均包含可設定有關您的 app 的資訊以及必要的詳細資訊的連結。<br/>
    ![Visual Studio 中的資訊清單設計工具](images/packaging-screen1.jpg)

    在 **\[視覺資產\]** 索引標籤上確認您擁有 UWP app 需要的所有影像。

    從 **\[封裝\]** 索引標籤中，您可以輸入發佈資料。 從此處您可以選擇要用來登入您的 app 的憑證。 所有 UWP 應用程式都必須以憑證簽署。 若要側載應用程式套件，您必須信任該套件。 憑證必須安裝在該裝置上才能信任該套件。 如需側載的詳細資訊，請參閱[啟用您的裝置以用於開發](https://msdn.microsoft.com/library/windows/apps/Dn706236)。

4.  在對您的 app 進行必要的編輯之後，請儲存您的檔案。

Visual Studio 可以將您的套件與市集關聯。 當您這樣做時，會自動更新資訊清單設計工具的 [封裝] 索引標籤中的一些欄位。

## <a name="create-an-app-package"></a>建立應用程式套件

若要透過市集發佈應用程式，您必須建立應用程式套件 (.appx)、應用程式套件組合 (.appxbundle) 或上傳套件 (.appxupload)。 您可以使用 **\[建立應用程式套件\]** 精靈來執行這項操作。 請依照下列步驟使用 Visual Studio 建立適合提交至市集的套件。

**建立應用程式套件**

1.  在 **\[方案總管\]** 中，開啟您的 UWP app 專案的方案。
2.  在專案上按一下滑鼠右鍵，然後選擇 **\[市集\]** -> **\[建立應用程式套件\]**。 如果此選項停用或未顯示，請確定專案是 UWP 專案。<br/>
    ![導覽至 [建立應用程式套件] 的操作功能表](images/packaging-screen2.jpg)

    **\[建立應用程式套件\]** 精靈便會出現。

3.  在詢問您是否要建置套件以上傳到 Windows 市集的第一個對話方塊中，選取 [是]，然後按一下 [下一步]。<br/>
    ![顯示的 [建立您的套件] 對話方塊視窗](images/packaging-screen3.jpg)

    如果您在此選擇 [否]，Visual Studio 將不會產生提交至市集所需的 .appxupload 套件。 如果您只想側載您的應用程式以在內部裝置上執行，您可以選取此選項。 如需側載的詳細資訊，請參閱[啟用您的裝置以用於開發](https://msdn.microsoft.com/library/windows/apps/Dn706236)。

4.  使用您的開發人員帳戶登入 Windows 開發人員中心。 (如果您還沒有開發人員帳戶，精靈會幫助您建立一個)。
5.  選取您套件的 App 名稱，或如果您還沒有在 Windows 開發人員中心入口網站保留一個名稱，則保留一個新的名稱。<br/>
    ![顯示選取 App 名稱的 [建立應用程式套件] 視窗](images/packaging-screen4.jpg)
6.  確定您在 **\[選取並設定套件\]** 對話方塊中選取全部的三種架構設定 (x86、x64 及 ARM)。 如此一來，您的 app 即可部署到最多種類的裝置。 在 **\[產生應用程式套件組合\]** 清單方塊中，選取 **\[一律\]**。 這會使市集提交程序更簡單，因為您只有一個檔案要上傳 (.appxupload)。 單一套件組合會包含部署到裝置的所有必要套件，並含有每個處理器架構，以及重要的偵錯和當機分析資訊。 若要深入了解各種裝置使用的套件架構，請參閱[應用程式套件架構](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)。<br/>
    ![顯示套件設定的 [建立應用程式套件] 視窗](images/packaging-screen5.jpg)
7.  強烈建議您包含完整的 PDB 符號檔案以從 Windows 開發人員中心獲得最佳的[損毀分析](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/)體驗。 您可以瀏覽[符號的偵錯](https://msdn.microsoft.com/library/windows/desktop/Ee416588)以深入了解符號的偵錯 。
8.  現在您可以設定詳細資訊以建立您的套件。 當您準備好發佈您的 app 時，將從輸出位置上傳套件。
9.  按一下 **\[建立\]** 以產生 appxupload 套件。
10. 現在，您會看到此對話方塊。<br/>
    ![顯示驗證選項的 [套件建立完成] 視窗](images/packaging-screen6.jpg)

    在您將 App 提交至市集之前，請先在本機或遠端電腦上驗證您的 App 以取得認證。 您只能驗證您的應用程式套件的發行組建，而非偵錯組建。

11. 若要在本機進行驗證，請將 **\[本機電腦\]** 保持在選取的選項，然後按一下 **\[啟動 Windows 應用程式認證套件\]**。 如需使用 Windows 應用程式認證套件測試應用程式的詳細資訊，請參閱 [Windows 應用程式認證套件](https://msdn.microsoft.com/library/windows/apps/Mt186449)。

    Windows 應用程式認證套件會執行各種測試並傳回結果。 如需更具體的資訊，請參閱 [Windows 應用程式認證套件測試](https://msdn.microsoft.com/library/windows/apps/mt186450)。

    如果您有想用來測試的遠端 Windows 10 裝置，您將需要在該裝置上手動安裝 Windows 應用程式認證套件。 下一節會帶您逐步完成下列步驟。 完成此動作之後，接著您可以選取 **\[遠端電腦\]**，按一下 **\[啟動 Windows 應用程式認證套件\]** 以連線到遠端裝置並執行驗證測試。

12. 完成 WACK 且您的 app 已通過驗證後，您可以準備將應用程式提交至市集。 請確定您上傳的是正確的檔案。 您可以在方案的根資料夾 \\\[AppName\]\\AppPackages 找到它，它會以 .appxupload 的副檔名結尾 (或是 .appx/.appxbundle 副檔名，如果您使用的是這個)。 名稱格式將為 \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload。

**在遠端 Windows 10 裝置上驗證您的應用程式套件。**

1.  依照[啟用您的裝置以進行開發](https://msdn.microsoft.com/library/windows/apps/Dn706236)的指示，啟用您的 Windows 10 裝置以進行開發。
    **重要**  您無法在遠端 ARM 裝置上驗證您的應用程式套件是否適用於 Windows 10。
2.  下載和安裝 Visual Studio 遠端工具。 這些工具可用來以遠端方式執行 Windows 應用程式認證套件。 您可以瀏覽[在遠端電腦上執行 Windows 市集應用程式](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor)，以取得關於這些工具的詳細資訊 (包括下載位置)。
3.  下載必要的 [Windows 應用程式認證套件](http://go.microsoft.com/fwlink/p/?LinkID=309666)，然後將它安裝在遠端的 Windows 10 裝置上。
4.  在精靈的 **\[套件建立完成\]** 頁面上，選擇 **\[遠端電腦\]** 選項按鈕，然後選擇 **\[測試連線\]** 按鈕旁的省略符號按鈕。
    **注意**：只有當您至少選取一個支援驗證的方案設定時，才可使用 **\[遠端電腦\]** 選項按鈕。 如需使用 WACK 測試 app 的詳細資訊，請參閱 [Windows 應用程式認證套件](https://msdn.microsoft.com/library/windows/apps/Mt186449)。
5.  指定您的子網路內的裝置種類，或提供子網路以外的裝置的網域名稱伺服器 (DNS) 名稱或 IP 位址。
6.  如果您的裝置不需要您使用 Windows 認證登入，請在 **\[驗證模式\]** 清單中選擇 **\[無\]**。
7.  選擇 **\[選取\]** 按鈕，然後再選擇 **\[啟動 Windows 應用程式認證套件\]** 按鈕。 如果遠端工具在該裝置上執行，Visual Studio 會與它連線，接著執行驗證測試。 請參閱 [Windows 應用程式認證套件測試](https://msdn.microsoft.com/library/windows/apps/mt186450)。

## <a name="sideload-your-app-package"></a>側載您的應用程式套件

Windows 10 年度更新版引進了新功能，只需按兩下應用程式套件檔案即可安裝應用程式套件。 若要使用此功能，只需瀏覽到您的應用程式套件 (.appx) 或應用程式套件組合 (.appxbundle) 檔案，然後按兩下。 應用程式安裝程式會啟動並提供基本的應用程式資訊，以及安裝按鈕、安裝進度列和任何相關的錯誤訊息。 

![應用程式安裝程式顯示安裝稱為 Contoso 的範例應用程式](images/appinstaller-screen.png)

> [!NOTE]
> 應用程式安裝程式假設應用程式已受到裝置的信任。 如果您是側載開發人員或企業應用程式，您必須在裝置的受信任的根憑證授權單位存放區安裝簽署憑證。 如果您不確定如何執行此動作，請參閱[安裝測試憑證](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)。

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>在舊版 Windows 上側載您的應用程式
使用 UWP app 套件時，因為它們是使用傳統型應用程式，所以應用程式不會安裝到裝置上。 一般而言，您從市集下載 UWP app 時，也會同時將應用程式安裝到您的裝置。 應用程式不需要提交到市集即可安裝 (側載)。 這可讓您安裝並使用您已建立的應用程式套件 (.appx) 來測試應用程式。 如果您不想在市集中銷售某個應用程式，例如企業營運 (LOB) 應用程式，您可以側載該應用程式，讓您公司中的其他使用者可以使用它。

下列清單提供您的應用程式的側載需求。

-   您必須[啟用您的裝置以進行開發](https://msdn.microsoft.com/library/windows/apps/Dn706236)。
-   若要在 Windows 10 行動裝置版裝置上側載您的應用程式，請使用 [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) 工具。

**側載應用程式到桌上型電腦、膝上型電腦或平板電腦**

1.  複製要安裝至目標裝置的應用程式版本的資料夾。

    如果您已經建立應用程式套件組合，您將會有一個以版本號碼為依據的資料夾與一個 `*\_Test` 資料夾。 例如下列兩個資料夾 (其中要安裝的版本是 1.0.2.0)：

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test`

    如果您沒有應用程式套件組合，請複製正確架構的資料夾和對應的 `*\_Test` 資料夾。 這兩個資料夾是 x64 架構應用程式套件及其 `*\_Test` 資料夾的範例：

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test`

2.  在目標裝置上，開啟 `*\_Test` 資料夾。
3.  以滑鼠右鍵按一下 **Add-AppDevPackage.ps1** 檔案。 選擇 **\[用 PowerShell 執行\]**，並依照提示進行。<br/>
    ![顯示瀏覽到 PowerShell 指令碼的檔案總管](images/packaging-screen7.jpg)

    安裝完應用程式套件時，PowerShell 視窗會顯示此訊息：**您的應用程式已順利安裝。**

    **提示**：若要在平板電腦上開啟捷徑功能表，請觸碰螢幕上您想要以滑鼠右鍵按一下的位置，暫停直到完整的圓形顯示，然後提起手指。 提起手指後，就會開啟捷徑功能表。
4.  按一下 [開始] 按鈕以依據名稱搜尋應用程式，然後啟動它。
