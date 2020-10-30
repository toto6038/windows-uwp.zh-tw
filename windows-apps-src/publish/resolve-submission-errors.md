---
description: 如果在提交您的 app 到市集後發生錯誤，您必須先解決這些問題才能繼續進行認證程序。
title: 解決提交錯誤
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c7ce114be55b9a53ce3a2ad0beabbbd89377f559
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029956"
---
# <a name="resolve-submission-errors"></a>解決提交錯誤

如果在提交您的 app 到市集後發生錯誤，您必須先解決這些問題才能繼續進行[認證程序](the-app-certification-process.md)。 錯誤訊息會指出問題是什麼，以及您為了修正這個問題可能需要執行的動作。 以下是一些可協助您解決這些錯誤的額外資訊。

## <a name="uwp-apps"></a>UWP 應用程式

如果您要提交 UWP 應用程式，您可能會在前置處理期間看到錯誤，因為您的封裝檔案不是由存放區 Visual Studio 所產生的. msixupload 或 .appxupload 檔案。 建立應用程式套件檔案時，請務必遵循 [使用 Visual Studio 封裝 UWP 應用程式](/windows/msix/package/packaging-uwp-apps) 中的步驟，並只在提交的 [封裝](upload-app-packages.md) 頁面上傳 msixupload 或 .appxupload 檔案，而不是 msix/appx 或 msixbundle/.appxbundle。

如果出現編譯錯誤，請確認您可以成功在釋出模式中建置您的應用程式。 如需詳細資訊，請參閱 [.NET 原生內部編譯器錯誤](https://github.com/dotnet/core/blob/master/Documentation/ilcRepro.md)。

## <a name="desktop-application"></a>傳統型應用程式

如果您打算提交同時包含 Win32 和 UWP 二進位檔的套件，請務必使用 Visual Studio 2017 Update 4 和更新版本中提供的 Windows 封裝專案來建立該套件。 如果您使用 UWP 專案範本建立封裝，可能無法將該封裝提交到存放區，或將它側載到其他電腦上。 即使封裝成功發佈，但在使用者的電腦上可能會以非預期的方式表現。 如需詳細資訊，請參閱 [使用 Visual Studio (封裝應用程式傳統型橋接器) ]( /windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8.x 及更早版本

> [!IMPORTANT]
> 從2018年10月31日起，新建立的產品不能包含以 Windows Phone 8.x 或更早版本為目標的套件。 如需詳細資訊，請參閱這 [篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

在前置處理期間，當偵測到 Windows Phone 套件有問題時，您可能會看到 **錯誤 2001** 。 在大部分的情況下，您必須重建您的 app 套件來更正錯誤。 一旦完成，在您再次按一下 **\[提交至市集\]** 前，請先在提交的 [ [套件](upload-app-packages.md)] 頁面上使用新套件取代舊套件。

造成此錯誤的問題有幾種。 請檢閱下列清單，判斷可能適用於您套件的問題。

-   **套件中一或多個組件模糊處理錯誤：** 使用不同的工具來執行模糊處理，或移除模糊處理。 編譯處理序最佳化模糊處理的組件，但是有些組件偶爾會以不受支援的方式使用修改 MSIL 的某個工具來模糊處理而導致錯誤。
-   **App 中一或多個方法的大小超過 IL 的 256 KB：** 將有問題的方法重構為較小的函式。 組件中方法的 MSIL 大小可以使用 ILDASM 工具決定。
-   **一或多個組件的強式名稱簽章驗證失敗：** 這個錯誤的發生原因通常是使用與組件中繼資料庫不同的金鑰來簽署強式名稱。 使用正確的金鑰簽署或移除強式名稱簽署。
-   **套件包含混合模式 (包含 Managed 和原生程式碼) 組件：** Windows Phone 不支援混合模式的組件。 從套件中移除混合模式組件，然後重新提交 app。
-   **Windows Phone 8.1 XAP 或 appx/appxbundle 組件不正確：** 請確定您的 .winmd 檔案中有至少一個公用的進入點。 如有需要，您可以使用任何解編程式應用程式檢閱程式碼，並檢查公用的進入點。

提交您的 app 後可能看到的另一個錯誤是 **錯誤 1300** 。 這發生在一或多個組件 (或整個套件) 已先行編譯時。 若要修正此問題，請先在 Microsoft Visual Studio 中重建 App 套件，然後提交新產生的套件。

## <a name="nameidentity-errors"></a>名稱/身分識別錯誤

如果您看到 **「在套件中找到的名稱不是其中一個保留的應用程式名稱。請保留應用程式名稱和/或使用此語言的正確應用程式名稱更新您的套件」** 錯誤，可能是因為您在套件中輸入的名稱不正確。 如果您使用的應用程式名稱尚未保留在合作夥伴中心中，也可能會發生此錯誤。 您通常可以依照下列步驟解決這個錯誤：

- 移至您 App 的 [應用程式身分識別](view-app-identity-details.md) 頁面 (在 **應用程式管理** 下)，確認您的 App 是否有指派的身分識別。 如果沒有，您會看到建立選項。 您必須保留您的應用程式名稱，才能建立身分識別。 請確定這是您在您的套件中使用的名稱。
- 如果您的 App 已經有身分識別，您可能仍需要保留您想在您的套件中使用的名稱。 在 **應用程式管理** 下，按一下 [管理應用程式名稱](manage-app-names.md)。 輸入您想要使用的名稱，然後按一下 **\[保留應用程式名稱\]** 。

> [!IMPORTANT]
>  如果您想要使用的名稱無法使用，表示另一個 App 可能已經保留該名稱。 如果您的 App 已經使用該名稱發行，或如果您認為您有權使用該名稱，請[連絡客戶支援](https://support.microsoft.com/getsupport/hostpage.aspx?locale=EN-US&supportregion=EN-US&ccfcode=US&ln=EN-US&pesid=14654&oaspworkflow=start_1.0.0.0&tenant=store&supporttopic_L1=31762156&supporttopic_L2=31762179)。  

 

 
