---
Description: The Microsoft Store enforces certain rules related to version numbers, which work somewhat differently in different OS versions.
title: 套件版本編號
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7a8ce14094733ef5598c510198f4268744cb581e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795015"
---
# <a name="package-version-numbering"></a>套件版本編號

您提供的每個套件必須具有版本號碼 (做為 app 資訊清單之 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素的 **Version** 屬性中的值)。 Microsoft Store 會強制執行某些與版本號碼相關的規則，在不同的作業系統版本中的運作方式有些不同。

> [!NOTE]
> 本主題是關於 「 套件 」，但是除非另行註明，相同的規則適用於.msix/.appx 和.msixbundle/.appxbundle 檔案兩者的版本號碼。


## <a name="version-numbering-for-windows10-packages"></a>Windows 10 套件的版本編號

> [!IMPORTANT]
> 針對 Windows 10 (UWP) 套件的版本號碼的最後一個 （第四個） 區段保留給市集使用，並當您建置您的套件 （但是市集可能會變更此區段中的值） 時，必須保留為 0。 其他區段必須設為介於 0 和 65535 （除了第一個區段，不能為 0） 之間的整數。

當從您發佈的提交中選擇 UWP 套件，Microsoft Store 將永遠使用適用於客戶的 Windows 10 裝置的最高版本套件。 這可讓您有更大的彈性，並讓您可以控制對於特定裝置類型的客戶提供哪個套件。 重要的是，您可以任何順序提交這些套件；您並不受限於在每個後續提交提供較高版本的套件。

您可以提供多個 UWP 套件具有相同版本號碼。 不過，因為市集用於每個套件的完整身分識別必須是唯一的，所以共用版本號碼的套件不能同時具有相同的架構。 如需詳細資訊，請參閱 [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)。

當您提供使用相同版本號碼的多個 UWP 套件時，架構 （依照順序 x64、 x86、 ARM、 中性） 將用於決定哪一個等級較高 （時在市集中決定哪一個套件提供給客戶的裝置）。 當排名使用相同版本號碼的 app 套件組合時，會考量套件組合中最高架構等級：包含 x64 套件的 app 套件組合比只包含 x86 套件的套件組合具有較高的等級。

給予您許多彈性讓 app 隨著時間進化。 您可以上傳並提交新增支援適用於您先前未支援的 Windows 10 裝置使用較低的版本號碼的新套件，您可以新增具有較嚴格相依性，以充分利用硬體或作業系統功能，或您的較高版本套件可以新增較高版本的套件，做為部分或所有現有客戶的更新基底。

下列範例說明如何管理版本編號以透過多重提交將想要的套件傳遞給客戶。

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>範例：透過多重提交移至單一套件

Windows 10 可讓您撰寫程式碼基底可在任何地方執行。 如此一來，開始新的跨平台專案就更容易。 不過，基於許多原因，您可能不想要合併現有的程式碼基底以立即建立單一專案。

您可以使用套件版本規則逐漸將您的客戶移至單一套件適用於通用裝置系列，同時提供一些中期更新的特定裝置系列 （包括利用 windows 10 Api）。 下列範例說明如何套用相同的規則以一致的方式透過一系列的相同的應用程式的提交。

| 提交 | 內容                                                  | 客戶體驗                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   套件版本：1.1.10.0 <br> -   裝置系列：Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.1.0.0 <br> -   裝置系列：Windows.Mobile, minVersion 10.0.10240.0     | -   Windows 10 傳統型組建 10.0.10240.0 和更新版本的裝置會取得 1.1.10.0 <br> -   Windows 10 行動裝置組建 10.0.10240.0 和更新版本的裝置會取得 1.1.0.0 <br> -   其他裝置系列無法購買及安裝應用程式 |
| 2          | -   套件版本：1.1.10.0 <br> -   裝置系列：Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.1.0.0 <br> -   裝置系列：Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.0.0.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10240.0    | -   Windows 10 傳統型組建 10.0.10240.0 和更新版本的裝置會取得 1.1.10.0 <br> -   Windows 10 行動裝置組建 10.0.10240.0 和更新版本的裝置會取得 1.1.0.0 <br> -   其他 (非傳統型、非行動) 裝置系列在被引入時將會取得 1.0.0.0 <br> -   已安裝應用程式的傳統型和行動裝置不會看到任何更新 (因為它們已經有最佳的可用版本—1.1.10.0 和 1.1.0.0 都高於 1.0.0.0) |
| 3          | -   套件版本：1.1.10.0 <br> -   裝置系列：Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.1.5.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   套件版本：1.0.0.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10240.0    | -   Windows 10 傳統型組建 10.0.10240.0 和更新版本的裝置會取得 1.1.10.0 <br> -   Windows 10 行動裝置組建 10.0.10250.0 和更新版本的裝置會取得 1.1.5.0 <br> -   Windows 10 行動裝置組建 &gt;=10.0.10240.0 和 &lt; 10.010250.0 的裝置會取得 1.1.0.0 
| 4          | -   套件版本：2.0.0.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10240.0   | -   在 Windows 10 組建 10.0.10240.0 版和更高版本上所有裝置系列的所有客戶會取得套件 2.0.0.0 | 

> [!NOTE]
>  在所有情況下，客戶裝置會收到符合資格的最高可能版本號碼的套件。 例如，在上述的第三個提交中，所有傳統型裝置將會收到 v1.1.10.0，即使他們有作業系統版本 10.0.10250.0 或更高版本，因此也可以接受 v1.1.5.0。 因為 1.1.10.0 是它們可用的最高版本號碼，所以這是它們會取得的套件。

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>針對新的擷取使用版本編號回復到先前發佈的套件

如果您保留您的套件的複本，您必須選擇復原您的應用程式封裝在市集中到舊版的 windows 10 套件如果您應該發現有版本問題。 這是暫時的方式，您需要時間來修正這個問題時限制對您的客戶的中斷。

若要這樣做，建立新的[提交](app-submissions.md)。 移除有問題的套件，並上傳您想要在市集中提供的舊套件。 已經收到您復原的套件的客戶仍然有問題套件 (因為您的舊版套件會有較早的版本號碼)。 但這會阻止任何人取得問題套件，同時讓 app 仍然可以在市集中取得。

若要修正這個問題的客戶已收到有問題的套件，您可以提交像您可以具有比損壞的套件更高的版本號碼的新 windows 10 套件。 提交進行認證程序之後，所有客戶都會更新為新套件，因為它將會有較高的版本號碼。


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>和 Windows Phone 8.1 套件的版本編號 Windows8.1 （和較舊版本）

> [!IMPORTANT]
> 截至 2018 年 10 月 31 剛建立的產品不能包含目標為 Windows 8.x/Windows 套件 Phone 8.x 或更舊版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

對於以 Windows Phone 8.1 為目標的 .appx 套件，新提交套件的版本號碼必須一律高於最後提交 (或任何先前的提交) 中包含的套件的版本號碼。

對於 Windows8 與 Windows8.1 為目標的.appx 套件，每個架構會套用相同規則： 新的提交中套件的版本號碼必須一律是更高的最後發佈到市集的相同架構的套件。

此外，Windows8.1 套件的版本號碼必須一律是大於任何 Windows8 為相同的應用程式套件的版本號碼。 換句話說，您提交的任何 Windows8 套件的版本號碼必須是相同的 app 送出的任何 Windows8.1 套件的版本號碼較低。

> [!NOTE]
> 如果您的應用程式也有 windows 10 套件，windows 10 套件的版本號碼必須高於的任何 Windows8、 Windows8.1，和/或 Windows Phone 8.1 套件。 如需詳細資訊，請參閱[新增適用於 windows 10 至先前發佈的 app 套件](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app)。

以下是一些 Windows8 和 Windows8.1 為目標的套件的不同版本號碼更新案例中發生情形的範例。

| 市集中您 app 的版本  | 您上傳這個版本 | 在 Microsoft Store 中提供新的版本之後，將會安裝於新的擷取中 | 在 Microsoft Store 中提供新的版本之後，將會更新 (如果客戶已經擁有該 app) |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| 無                                     | x86, v1.0.0.0               | x86 和 x64 電腦上都是 x86, v1.0.0.0                                                | 無。 |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | 客戶的架構為 v1.0.0.0                                                   | 無。 版本號碼相同。 |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | x86 的客戶為 v1.0.0.0 <br> x64 的客戶為 v1.0.0.1                 | 在 x86 電腦上執行應用程式的客戶不顯示任何資訊。 <br> 針對在 x64 電腦上執行 app 的客戶，v1.0.0.0 會更新為 v1.0.0.1。 <br> **注意：** 如果 x86 x64 執行的應用程式版本的電腦，應用程式不會更新成 x64 版本除非客戶解除安裝再重新安裝。 |
| 無                                     | 中性, v1.0.0.1           | 所有電腦上都是中性, v1.0.0.1                                                         | 無。 |
| 中性, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | 客戶電腦的架構為 v1.0.0.0。          | 無。 擁有中性, v1.0.0.1 版本 app 的客戶將繼續使用。 |
| 中性, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | 客戶電腦的架構為 v1.0.0.1。 | 執行中性, v1.0.0.1 版本應用程式的客戶不顯示任何資訊。 <br> 執行為電腦特定架構建置的 v1.0.0.0 應用程式的客戶，v1.0.0.0 將更新為 v1.0.0.1。 |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | 客戶電腦的架構為 v1.0.0.2。  | 對於執行為電腦特定架構建置的 v1.0.0.1 應用程式的客戶，v1.0.0.1 將更新為 v1.0.0.2。 |

> [!NOTE]
> 不同於 .appx 套件，在判斷要為特定客戶提供哪一個套件時，不會考慮任何 .xap 套件中的版本號碼。 若要將客戶從一個 .xap 套件更新為較新的套件，請務必在新的提交中移除較舊的 .xap。
