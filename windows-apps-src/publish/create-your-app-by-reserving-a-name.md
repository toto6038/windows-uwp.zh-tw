---
author: jnHs
Description: The first step in creating a new app in your Windows Dev Center dashboard is reserving an app name. See how to reserve app names and find suggestions for choosing a great name for your app.
title: 透過保留名稱建立您的 App
keywords: windows 10, uwp, 名稱保留, app 名稱, 應用程式名稱, 名稱, 產品名稱, 命名, 保留名稱, 標題
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 83f2ab8a27810635b569d44961ff532ce3240e28
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2018
ms.locfileid: "4175981"
---
# <a name="create-your-app-by-reserving-a-name"></a>透過保留名稱建立您的應用程式

在您[Windows 開發人員中心儀表板](https://partner.microsoft.com/dashboard)中建立新的應用程式的第一個步驟即為保留應用程式名稱。 每個保留名稱 (有時稱為您應用程式的*標題*) 必須在 Microsoft Store 中是唯一的。

即使您尚未開始建置應用程式，也能保留名稱給您的應用程式。 我們建議您儘速執行這個動作，好讓其他人無法使用該名稱。 請注意，您需要在 3 個月內提交應用程式，該名稱才能保留給您使用。

[上傳應用程式套件](upload-app-packages.md)時，[**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 值必須符合您為應用程式保留的名稱。 如果您使用 Microsoft Visual Studio 建立應用程式套件，則系統將會幫您填入此屬性。

> [!IMPORTANT]
> 您可以保留其他名稱用於應用程式，以及您可以選擇使用其中一個已發佈您的應用程式的版本中而不是您保留您先在儀表板中建立您的應用程式時的一個。 不過，請注意，在某些您的應用程式的[身分識別詳細資料](view-app-identity-details.md)，例如**套件系列名稱 (PFN)** 會使用您在這裡輸入的名字。 這些值可能會看到某些使用者，而且不能變更，因此請確定您保留的名稱是適用於此用途。


## <a name="create-your-app-by-reserving-a-new-name"></a>透過保留新名稱建立您的應用程式

保留名稱是在儀表板中建立應用程式的第一個步驟。 

1.  從 **\[概觀\]** 頁面，按一下 **\[建立新的應用程式\]**。
2.  在文字方塊中，輸入要使用的名稱，然後選取 **\[檢查可用性\]**。 如果名稱可供使用，您將會看見綠色勾號 (如果輸入的名稱已被其他開發人員保留或使用，您將看見一則訊息，指出無法使用該名稱)。
3.  按一下 **\[保留產品名稱\]**。

現在已為您保留該名稱，您可以在準備好時，開始進行[提交](app-submissions.md)。 

> [!NOTE]
> 即使 Microsoft Store 中沒有看到任何列出的應用程式使用某個名稱，但您可能會發現無法保留該名稱。 這通常是因為其他開發人員已為其應用程式保留該名稱，但尚未提交應用程式。 如果您無法保留您具有商標權或其他合法權利的名稱，或者看到 Microsoft Store 有其他應用程式使用該名稱，請[連絡 Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777)。

保留名稱之後，您必須在三個月內提交該應用程式。 如果您沒有在三個月內提交應用程式，保留的名稱將會到期，而其他開發人員就能使用該名稱做為應用程式名稱。 如果您嘗試在名稱到期後送出應用程式，可能就會發生錯誤。

> [!NOTE]
> 如果您先前在舊版 Windows Phone 儀表板中建立 Windows Phone App，而且不曾為其保留名稱，則您必須這麼做才能為其上傳 .appx 套件，或是檢視 .appx 套件專屬的[應用程式身分識別詳細資料](view-app-identity-details.md)。 保留唯一的名稱也會防止任何人將該名稱保留給自己使用。 不過，如果不保留名稱，您仍然可以為 Windows Phone 8.x 客戶管理和提交應用程式。


## <a name="choosing-your-apps-name"></a>選擇應用程式的名稱

為應用程式選擇適合的名稱是一件非常重要的工作。 請挑選可抓住客戶興趣並吸引他們深入了解您應用程式的名稱。 以下是選擇最佳應用程式名稱的一些祕訣。

-   **簡潔有力。** 由於許多顯示應用程式名稱的地方，其空間有限，因此建議使用最簡潔有力的名稱。 雖然您的應用程式名稱最多可以有 256 個字元，但是客戶不一定能看到冗長名稱的結尾。
    > [!NOTE]
    > 實際顯示的字元數目在各個位置會有所不同，取決於分配的長度以及應用程式名稱中使用的字元類型。 例如，在 Windows 使用的 Segoe UI 字型中，10 個 "W" 字元所用的空間大約可容納 30 個 "I" 字元。 因為這種差異性，所以送出應用程式之前，請務必測試您的應用程式，確認其在磚上 (如果您選擇覆蓋應用程式名稱)、在搜尋結果中以及應用程式本身內會如何顯示名稱。 另請考量要使用哪種語言來提供您的應用程式。 請記住，東亞字元通常比拉丁字元更寬，因此可以顯示的字元數目較少。
-   **請獨特創新。** 務必讓您的應用程式名稱與眾不同，才不容易與現有的應用程式混淆。
-   **不要採用他人已註冊為商標的名稱** 請確定您有權使用您保留的名稱。 如果其他人已經將該名稱註冊為商標，他們將可報告侵權，而您將無法再使用該名稱。 如果在 app 發佈之後發生這種情況，您的 app 將會從市集移除。 然後，您將需要變更 app 的名稱，以及 app 與其內容中所有該名稱出現的地方，然後才能再次[提交您的應用程式](app-submissions.md)進行認證。
-   **避免在名稱結尾加上辨別資訊。** 如果將辨別多個應用程式的資訊加在名稱結尾，客戶可能會遺漏這些資訊，尤其是當名稱太長時，不同版本的應用程式名稱可能看起來都一樣。 如果無法避免這種情況，請使用不同的標誌和應用程式影像，這樣比較容易區分不同的應用程式。
-   **不要在您的名稱中包含 Emoji。** 您無法保留包含 Emoji 或其他不支援的字元的名稱。


## <a name="manage-additional-app-names"></a>管理其他的 app 名稱

您可以在 **\[管理應用程式名稱\]** 頁面的 **\[應用程式管理\]** 區段中，為 Windows 開發人員中心儀表板中的每個 app 新增及管理其他名稱。

在某些情況下，您可能想要保留多個名稱用於相同的 app，例如，當您想以多個語言提供 app 並且想要針對每一種語言使用不同的名稱時。 如果您想要完全變更 app 的名稱，您將需要保留其他名稱。

在此頁面上，您也可以刪除任何已保留但不再使用的名稱。

如需詳細資訊，請參閱[管理應用程式名稱](manage-app-names.md)。

 

 




