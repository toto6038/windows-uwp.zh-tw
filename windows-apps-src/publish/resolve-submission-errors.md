---
Description: 如果在提交您的 app 到市集後發生錯誤，您必須先解決這些問題才能繼續進行認證程序。
title: 解決提交錯誤
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: df7c1bbbc77374b8afb4272e1d9618c8294a4b6e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591853"
---
# <a name="resolve-submission-errors"></a>解決提交錯誤

如果在提交您的 app 到市集後發生錯誤，您必須先解決這些問題才能繼續進行[認證程序](the-app-certification-process.md)。 錯誤訊息會指出問題是什麼，以及您為了修正這個問題可能需要執行的動作。 以下是一些可協助您解決這些錯誤的額外資訊。

## <a name="uwp-apps"></a>UWP 應用程式

如果您提交的 UWP 應用程式，您可能會在前置處理，如果您套件的檔案不是 Visual Studio 所產生的存放區的.msixupload 或.appxupload 檔案期間看到錯誤。 確定您遵循的步驟[封裝 UWP 應用程式與 Visual Studio](../packaging/packaging-uwp-apps.md)上建立您的應用程式套件檔案，並只上傳的.msixupload 或.appxupload 檔案時[封裝](upload-app-packages.md)提交頁面不是.msix/appx 或.msixbundle/appxbundle。

如果出現編譯錯誤，請確認您可以成功在釋出模式中建置您的應用程式。 如需詳細資訊，請參閱 [.NET 原生內部編譯器錯誤](https://go.microsoft.com/fwlink/p/?LinkID=613098)。

## <a name="desktop-application"></a>桌面應用程式

如果您計劃提交包含 Win32 與 UWP 的二進位檔的封裝，請確定您使用 Visual Studio 2017 Update 4 中提供 Windows 封裝專案來建立該套件。 如果您使用 UWP 專案範本建立封裝，您可能無法提交的封裝到市集或側載到其他電腦上。 即使封裝已成功發行，它可能會非預期的方式，在使用者的電腦上的行為。 如需詳細資訊，請參閱 <<c0> [ 使用 Visual Studio （傳統型橋接器） 封裝的應用程式]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)。

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8.x 和更早版本

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新建立的產品不能包含封裝目標設為 Windows Phone 8.x 或更早版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

在前置處理期間，當偵測到 Windows Phone 套件有問題時，您可能會看到**錯誤 2001**。 在大部分的情況下，您必須重建您的 app 套件來更正錯誤。 一旦完成，在您再次按一下 [提交至市集] 前，請先在提交的 [[套件](upload-app-packages.md)] 頁面上使用新套件取代舊套件。

造成此錯誤的問題有幾種。 請檢閱下列清單，判斷可能適用於您套件的問題。

-   **在封裝中的一或多個組件已經過模糊處理不正確：** 若要執行模糊化，使用不同的工具，或移除模糊化。 編譯處理序最佳化模糊處理的組件，但是有些組件偶爾會以不受支援的方式使用修改 MSIL 的某個工具來模糊處理而導致錯誤。
-   **應用程式中的一個或多個方法的大小超過 256 KB 的 IL:** 違反的方法重構為較小的函式。 組件中方法的 MSIL 大小可以使用 ILDASM 工具決定。
-   **一或多個組件的強式名稱簽章驗證失敗：** 強式名稱簽署已使用不同的組件中繼資料中必須要有的金鑰執行時，通常就會發生此錯誤。 使用正確的金鑰簽署或移除強式名稱簽署。
-   **封裝包含混合模式 （使用 managed 和原生程式碼） 組件：** 在 Windows Phone 上不支援混合模式組件。 從套件中移除混合模式組件，然後重新提交 app。
-   **Windows Phone 8.1 XAP 或 appx/appxbundle 組件不會是有效項目：** 請確定您的.winmd 檔案具有至少一個公用進入點。 如有需要，您可以使用任何解編程式應用程式檢閱程式碼，並檢查公用的進入點。

提交您的 app 後可能看到的另一個錯誤是**錯誤 1300**。 這發生在一或多個組件 (或整個套件) 已先行編譯時。 若要修正此問題，請先在 Microsoft Visual Studio 中重建 App 套件，然後提交新產生的套件。

## <a name="nameidentity-errors"></a>名稱/身分識別錯誤

是否您看到錯誤指出**中封裝的名稱不是其中一個保留的應用程式名稱。請保留應用程式名稱及/或更新您的套件具有正確的應用程式名稱，此語言**，可能是因為您在套件中輸入的名稱不正確。 如果您使用應用程式名稱，您還沒有保留在合作夥伴中心，也會發生此錯誤。 您通常可以依照下列步驟解決這個錯誤：

- 移至您 App 的 [應用程式身分識別](view-app-identity-details.md) 頁面 (在 **應用程式管理** 下)，確認您的 App 是否有指派的身分識別。 如果沒有，您會看到建立選項。 您必須保留您的應用程式名稱，才能建立身分識別。 請確定這是您在您的套件中使用的名稱。
- 如果您的 App 已經有身分識別，您可能仍需要保留您想在您的套件中使用的名稱。 在 **應用程式管理** 下，按一下 [管理應用程式名稱](manage-app-names.md)。 輸入您想要使用的名稱，然後按一下 [保留應用程式名稱]。

> [!IMPORTANT]
>  如果您想要使用的名稱無法使用，表示另一個 App 可能已經保留該名稱。 如果您的 App 已經使用該名稱發行，或如果您認為您有權使用該名稱，請[連絡客戶支援](https://go.microsoft.com/fwlink/p/?LinkId=331509)。  

 

 




