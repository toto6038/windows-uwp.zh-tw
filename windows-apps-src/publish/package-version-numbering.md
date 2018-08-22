---
author: jnHs
Description: The Microsoft Store enforces certain rules related to version numbers, which work somewhat differently in different OS versions.
title: 套件版本編號
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.author: wdg-dev-content
ms.date: 5/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9a16339e0918f8291f7b1cc7a3a6dfef3ccf375d
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2795076"
---
# <a name="package-version-numbering"></a>套件版本編號

您提供的每個套件必須具有版本號碼 (做為 app 資訊清單之 **Package/Identity** 元素的 **Version** 屬性中的值)。 Microsoft Store 會強制執行某些與版本號碼相關的規則，在不同的作業系統版本中的運作方式有些不同。

> [!NOTE]
> 本主題是關於「套件」，但是除非另行註明，相同的規則適用於 .appx 和 .appxbundle 檔案兩者的版本號碼。


## <a name="version-numbering-for-windows-10-packages"></a>Windows 10 套件的版本編號

> [!IMPORTANT]
> Windows 10 套件的最後一個 （第四個）] 區段中的版本號碼的僅限存放區使用並建置您的套件 （雖然存放區可能會變更此區段中的值） 時必須保留為 0。

從您發佈的提交中選擇 Windows 10 套件時，Microsoft Store 將永遠使用適用於客戶裝置的最高版本套件。 這可讓您有更大的彈性，並讓您可以控制對於特定裝置類型的客戶提供哪個套件。 重要的是，您可以任何順序提交這些套件；您並不受限於在每個後續提交提供較高版本的套件。

> [!TIP]
> 如果您的應用程式也會有 Windows 8、 Windows 8.1 和/或 Windows Phone 8.1 套件，必須一律高於任何這些套件中的版本號碼任何 Windows 10 套件的版本號碼。 如需詳細資訊，請參閱[將適用於 Windows 10 的套件新增至先前發佈的 app](https://docs.microsoft.com/en-us/windows/uwp/publish/guidance-for-app-package-management#adding-packages-for-windows-10-to-a-previously-published-app)。

您可以提供多個 Windows 10 套件具有相同的版本號碼。 不過，因為市集用於每個套件的完整身分識別必須是唯一的，所以共用版本號碼的套件不能同時具有相同的架構。 如需詳細資訊，請參閱 [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)。

當您提供使用相同的版本號碼的多個 Windows 10 套件時，（順序 x64、 x86，ARM、 中性） 架構將用來決定哪一種是較高的排名 （當存放區決定要提供給客戶的裝置哪一個套件）。 當排名使用相同版本號碼的 app 套件組合時，會考量套件組合中最高架構等級：包含 x64 套件的 app 套件組合比只包含 x86 套件的套件組合具有較高的等級。

給予您許多彈性讓 app 隨著時間進化。 您可以上傳並提交使用較低版本號碼的新套件，為您先前未支援的價格實惠裝置新增支援，您可以新增具有較嚴格相依性的較高版本套件，以利用硬體或作業系統功能，或者您可以新增較高版本的套件，做為部分或所有現有客戶的更新。

下列範例說明如何管理版本編號以透過多重提交將想要的套件傳遞給客戶。

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>範例：透過多重提交移至單一套件

Windows 10 可讓您撰寫一個可在任何地方執行的程式碼基底。 如此一來，開始新的跨平台專案就更容易。 不過，基於許多原因，您可能不想要合併現有的程式碼基底以立即建立單一專案。

您可以使用套件版本規則，在針對特定裝置系列 (包括利用 Windows 10 API 的裝置系列) 提供一些中期更新的同時，逐漸將您的客戶移至通用裝置套件的單一套件。 下列範例說明如何一致地套用相同的規則。

| 提交 | 內容                                                  | 客戶體驗                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   套件版本：1.1.10.0 <br> -   裝置系列：Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.1.0.0 <br> -   裝置系列：Windows.Mobile, minVersion 10.0.10240.0     | -   Windows 10 傳統型組建 10.0.10240.0 和更新版本的裝置會取得 1.1.10.0 <br> -   Windows 10 行動裝置組建 10.0.10240.0 和更新版本的裝置會取得 1.1.0.0 <br> -   其他裝置系列無法購買及安裝應用程式 |
| 2          | -   套件版本：1.1.10.0 <br> -   裝置系列：Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.1.0.0 <br> -   裝置系列：Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.0.0.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10240.0    | -   Windows 10 傳統型組建 10.0.10240.0 和更新版本的裝置會取得 1.1.10.0 <br> -   Windows 10 行動裝置組建 10.0.10240.0 和更新版本的裝置會取得 1.1.0.0 <br> -   其他 (非傳統型、非行動) 裝置系列在被引入時將會取得 1.0.0.0 <br> -   已安裝應用程式的傳統型和行動裝置不會看到任何更新 (因為它們已經有最佳的可用版本—1.1.10.0 和 1.1.0.0 都高於 1.0.0.0) |
| 3          | -   套件版本：1.1.10.0 <br> -   裝置系列：Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   套件版本：1.1.5.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   套件版本：1.0.0.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10240.0    | -   Windows 10 傳統型組建 10.0.10240.0 和更新版本的裝置會取得 1.1.10.0 <br> -   Windows 10 行動裝置組建 10.0.10250.0 和更新版本的裝置會取得 1.1.5.0 <br> -   Windows 10 行動裝置組建 &gt;=10.0.10240.0 和 &lt; 10.010250.0 的裝置會取得 1.1.0.0 
| 4          | -   套件版本：2.0.0.0 <br> -   裝置系列：Windows.Universal, minVersion 10.0.10240.0   | -   在 Windows 10 組建 10.0.10240.0 版和更高版本上所有裝置系列的所有客戶會取得套件 2.0.0.0 | 

> [!NOTE]
>  在所有情況下，客戶裝置會收到符合資格的最高可能版本號碼的套件。 例如，在上述的第三個提交中，所有傳統型裝置將會收到 v1.1.10.0，即使他們有作業系統版本 10.0.10250.0 或更高版本，因此也可以接受 v1.1.5.0。 因為 1.1.10.0 是它們可用的最高版本號碼，所以這是它們會取得的套件。

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>針對新的擷取使用版本編號回復到先前發佈的套件

如果您保留複本，您將能夠回復您的應用程式套件存放區中舊版的 Windows 10 套件如果您發現版本的問題。 這是暫時的方式，在您修正問題時限制對客戶的中斷。

若要這樣做，建立新的[送出](app-submissions.md)。 移除有問題的套件，並上傳您想要在市集中提供的舊套件。 已經收到您復原的套件的客戶仍然有問題套件 (因為您的舊版套件會有較早的版本號碼)。 但這會阻止任何人取得問題套件，同時讓 app 仍然可以在市集中取得。

若要修正客戶已收到有問題的套件的問題，您可以立即提交具有比損壞的套件更高版本號碼的新 Windows 10 套件。 提交進行認證程序之後，所有客戶都會更新為新套件，因為它將會有較高的版本號碼。


## <a name="version-numbering-for-windows-81-and-earlier-and-windows-phone-81-packages"></a>Windows 8.1 (和舊版) 和 Windows Phone 8.1 套件的版本編號

對於以 Windows Phone 8.1 為目標的 .appx 套件，新提交套件的版本號碼必須一律高於最後提交 (或任何先前的提交) 中包含的套件的版本號碼。

對於以Windows 8 和 Windows 8.1 為目標的 .appx 套件，每個架構會套用相同規則：新提交套件的版本號碼必須一律高於最後針對相同架構發佈至 Microsoft Store 之套件的版本號碼。

此外，Windows 8.1 套件的版本編號必須永遠高於同一個 app 的任何 Windows 8 套件版本編號。 換句話說，您提交的任何 Windows 8 套件版本編號必須低於您為相同 app 提交的任何 Windows 8.1 套件版本編號。

> [!NOTE]
> 如果您的應用程式也有 Windows 10 套件，必須高於以外的任何 Windows 8、 Windows 8.1 和/或 Windows Phone 8.1 套件 Windows 10 套件的版本號碼。 如需詳細資訊，請參閱[將適用於 Windows 10 的套件新增至先前發佈的 app](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app)。

以下是一些 Windows 8 和 Windows 8.1 的不同版本號碼更新案例中發生情形的範例。

| 市集中您 app 的版本  | 您上傳這個版本 | 在 Microsoft Store 中提供新的版本之後，將會安裝於新的擷取中 | 在 Microsoft Store 中提供新的版本之後，將會更新 (如果客戶已經擁有該 app) |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| 無                                     | x86, v1.0.0.0               | x86 和 x64 電腦上都是 x86, v1.0.0.0                                                | 無。 |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | 客戶的架構為 v1.0.0.0                                                   | 無。 版本號碼相同。 |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | x86 的客戶為 v1.0.0.0 <br> x64 的客戶為 v1.0.0.1                 | 在 x86 電腦上執行應用程式的客戶不顯示任何資訊。 <br> 針對在 x64 電腦上執行 app 的客戶，v1.0.0.0 會更新為 v1.0.0.1。 <br> **注意**  如果應用程式的 x86 版本是在 x64 電腦上執行，則除非客戶先將應用程式解除安裝再重新安裝，否則應用程式不會更新成 x64 版本。 |
| 無                                     | 中性, v1.0.0.1           | 所有電腦上都是中性, v1.0.0.1                                                         | 無。 |
| 中性, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | 客戶電腦的架構為 v1.0.0.0。          | 無。 擁有中性, v1.0.0.1 版本 app 的客戶將繼續使用。 |
| 中性, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | 客戶電腦的架構為 v1.0.0.1。 | 執行中性, v1.0.0.1 版本應用程式的客戶不顯示任何資訊。 <br> 執行為電腦特定架構建置的 v1.0.0.0 應用程式的客戶，v1.0.0.0 將更新為 v1.0.0.1。 |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | 客戶電腦的架構為 v1.0.0.2。  | 對於執行為電腦特定架構建置的 v1.0.0.1 應用程式的客戶，v1.0.0.1 將更新為 v1.0.0.2。 |

> [!NOTE]
> 不同於 .appx 套件，在判斷要為特定客戶提供哪一個套件時，不會考慮任何 .xap 套件中的版本號碼。 若要將客戶從一個 .xap 套件更新為較新的套件，請務必在新的提交中移除較舊的 .xap。
