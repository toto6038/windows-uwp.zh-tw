---
author: jnHs
Description: "了解您的應用程式套件是如何提供給您的客戶，以及如何管理特定套件案例。"
title: "應用程式套件管理指導方針"
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 54f6d6c786eb0787a441628452d26e46f353b3d8
ms.lasthandoff: 02/07/2017

---

# <a name="guidance-for-app-package-management"></a>應用程式套件管理指導方針


了解您的應用程式套件是如何提供給您的客戶，以及如何管理特定套件案例。

-   [作業系統版本和套件發佈](#os-versions-and-package-distribution)
-   [將適用於 Windows 10 的套件新增至先前發佈的 app](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [維護 Windows Phone 8.1 的套件相容性](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [從市集移除 App](#removing-an-app-from-the-store)
-   [移除先前支援之裝置系列的套件](#removing-packages-for-a-previously-supported-device-family)

## <a name="os-versions-and-package-distribution"></a>作業系統版本和套件發佈


不同的作業系統上可以執行不同類型的套件。 如果有一個以上的套件可以在客戶的裝置上執行，則 Windows 市集將提供最佳絕配。

一般而言，較新的作業系統版本可以針對相同的裝置系列，執行以之前作業系統版本為目標的套件。 不過，如果 app 不包含以目前作業系統版本為目標的套件，則客戶只會取得那些套件。

例如，Windows 10 裝置可以執行所有先前支援的作業系統版本 (每個裝置系列)。 Windows 10 桌面裝置可以執行針對 Windows 8.1 或 Windows 8 建置的 app；Windows 10 行動裝置可以執行針對 Windows Phone 8.1、Windows Phone 8 和 Windows Phone 7.x 所建置的 app。

下列範例針對包含以不同作業系統版本為目標之套件的 app，說明各種的案例。 在某些情況下，您套件的特定限制可能不允許它們在此處所列的每一個作業系統版本和裝置類型上執行 (例如，架構必須適當)，但這些範例應可協助您了解哪些作業系統版本可以執行特定套件。

### <a name="example-app-1"></a>範例 app 1

| 套件的目標作業系統 | 將取得此套件的作業系統 |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Windows 10 桌上型電腦裝置、Windows 8.1      |
| Windows Phone 8.1                   | Windows 10 行動裝置、Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

在 app 範例 1，app 還沒有特別針對 Windows 10 裝置建置的通用 Windows 平台 (UWP) 套件，但 Windows 10 的客戶仍可取得 app。 這些客戶會根據其裝置類型取得最佳的套件。

### <a name="example-app-2"></a>範例 app 2

| 套件的目標作業系統  | 將取得此套件的作業系統 |
|--------------------------------------|----------------------------------------------|
| Windows 10 (通用裝置系列) | Windows 10 (所有裝置系列)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x、Windows Phone 8           |

在範例 app 2 中，沒有可在 Windows 8 上執行的套件。 在所有其他作業系統版本上執行的客戶可以取得 app。

### <a name="example-app-3"></a>範例 app 3

| 套件的目標作業系統 | 將取得此套件的作業系統                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (桌面裝置系列)  | Windows 10 桌面裝置                                    |
| Windows Phone 8                     | Windows 10 行動裝置、Windows Phone 8、Windows Phone 8.1 |

在範例 app 3 中，因為沒有以行動裝置系列為目標的 UWP 套件，所以 Windows 10 行動裝置的客戶將會取得 Windows Phone 8 套件。 如果此 app 後來新增以行動裝置系列 (或通用裝置系列) 為目標的套件，則該套用即可供 Windows 10 行動裝置的客戶，而非 Windows Phone 8 套件的客戶使用。

也請注意，這個範例 app 不包含任何可在 Windows 7.x 上執行的套件。

### <a name="example-app-4"></a>範例 app 4

| 套件的目標作業系統  | 將取得此套件的作業系統 |
|--------------------------------------|----------------------------------------------|
| Windows 10 (通用裝置系列) | Windows 10 (所有裝置系列)             |

在範例 app 4 中，任何執行 Windows 10 的裝置均可取得 app，但舊版作業系統的客戶無法使用該 app。 因為 UWP 套件以通用裝置系列為目標，所以可供桌面和行動 Windows 10 裝置使用。

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>將適用於 Windows 10 的套件新增至先前發佈的 app


如果您在市集中有一個 app 並想要針對 Windows 10 更新您的 app，請在 [[套件](upload-app-packages.md)] 步驟期間建立新的提交並新增您的 UWP .appxupload 套件。 在您的 app 通過認證程序後，客戶如果在升級為 Windows 10 之前就已經擁有您的 app，就可以從市集以更新的形式取得您的 UWP 套件。 Windows 10 上的客戶也可以透過全新取得的方式獲取 UWP 套件。

> **重要**  一旦 Windows 10 客戶取得您的 UWP 套件後，您將無法讓該客戶回復到使用任何之前作業系統版本的套件。 在將您的 UWP 套件新增到提交之前，請務必在 Windows 10 上徹底測試您的 UWP 套件。

您可以同時更新任何其他套件或對此提交進行其他變更 (例如，您可能要對舊版作業系統的客戶顯示[建立特定平台的介紹](create-platform-specific-descriptions.md))。 如果您想要，也能讓其他項目保持相同。

> **注意**  針對相同的應用程式，您的 Windows 10 套件版本號碼必須高於您正在發佈的 Windows 8、Windows 8.1 和/或 Windows Phone 8.1 套件 (或是適用於您先前發佈之作業系統版本的套件) 的版本號碼。 如需 Windows 10 版本編號的詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。

一旦新的提交完成認證程序，即可使用 UWP 套件，以及您已提供給尚未採用 Windows 10 客戶使用的其他套件。

如需針對市集封裝 UWP app 詳細資訊，請參閱[封裝適用於 Windows 10 的通用 Windows app](http://go.microsoft.com/fwlink/p/?LinkId=620193 )。

> **重要**  請記住，如果您提供以通用裝置系列為目標的套件，則在任何舊版作業系統 (Windows Phone 8、Windows 8.1 等) 上已擁有您的應用程式，而後升級至 Windows 10 的客戶，將會被更新至 Windows 10 通用套件。
> 
> 即使您已在提交的[定價和可用性](set-app-pricing-and-availability.md#windows-10-device-families)步驟中排除特定裝置系列，也會發生這種情況，因為**裝置系列**選項僅適用於全新取得的情況。 如果您不想每一位先前客戶取得您的新 Windows 10 套件，請務必更新您的 appx 資訊清單中的 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 元素，只包含您想要支援的特定裝置系列。
> 
> 例如，假設您只想要讓已升級至 Windows 10 的 Windows 8 和 Windows 8.1 客戶取得您的 UWP app，而且您想要 Windows Phone 8.1 和更早版本的客戶保留您先前提供的套件 (針對 Windows Phone 8 或 Windows Phone 8.1)。 若要這麼做，您必須更新您 appx 資訊清單中的 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)，確定它只包含 **Windows.Desktop** (針對桌面裝置系列)，而不是保留 Microsoft Visual Studio 預設在 appx 資訊清單中包含的 **Windows.Universal** 值 (針對通用裝置系列)。 請勿提交任何針對通用或行動裝置系列的 UWP 套件 (**Windows.Universal** 或 **Windows.Universal**)。 如此一來，您的 Windows 10 行動客戶將不會取得任何 UWP 套件。
> 
> 如需裝置系列的詳細資訊，請參閱[通用 Windows 平台 (UWP) app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>維護 Windows Phone 8.1 的套件相容性


在更新先前針對 Windows Phone 8.1 發佈的 app 時，會套用某些套件類型的特定要求：

-   在 app 擁有已發佈的 Windows Phone 8.1 套件後，所有後續的更新也必須包含 Windows Phone 8.1 套件。
-   在 app 擁有已發佈的 Windows Phone 8.1 XAP 後，後續的更新必須具有 Windows Phone 8.1 XAP、Windows Phone 8.1 appx 或 Windows Phone 8.1 appxbundle。
-   當 app 擁有已發佈的 Windows Phone 8.1 .appx 時，後續的更新必須具有 Windows Phone 8.1 .appx 或 Windows Phone 8.1 .appxbundle。 換句話說，不允許 Windows Phone 8.1 XAP。 這也適用於包含 Windows Phone 8.1.appx 的 .appxupload。
-   在 app 擁有已發佈的 Windows Phone 8.1 .appxbundle 後，後續的更新必須具有 Windows Phone 8.1 .appxbundle。 換句話說，不允許 Windows Phone 8.1 XAP 或 Windows Phone 8.1 .appx。 這也適用於包含 Windows Phone 8.1 .appxbundle 的 .appxupload。

未遵循這些規則將導致套件上傳錯誤，而使您無法完成提交。

## <a name="removing-an-app-from-the-store"></a>從市集移除 App


有時，您可能想要完全停止為客戶提供某個 app，有效地「取消發佈」該 app。 若要執行此動作，請按一下 [App 概觀] 頁面上的 [停止提供 App]****。 在您確認想要停止提供該 app 之後，該 app 在數小時內便無法在市集中看見，而所有的新客戶都將無法透過任何方法 (包括促銷碼) 來取得它。

> **重要**  這將會覆寫您已在提交中選取的所有[配送和可見性](set-app-pricing-and-availability.md#distribution-and-visibility)設定。

請注意，任何已擁有此 app 的客戶仍可使用它 (甚至可以在您於稍後提交新的套件時取得更新)。

停止提供該 app 之後，您仍能在儀表板中看見它。 如果您決定再次為客戶提供該 app，就可以按一下 [App 概觀] 頁面上的 [提供 App]****。 當您確認之後，該 app 即可在數小時內提供給新的客戶使用 (除非受到您在最新提交中的設定所限制)。

> **注意**  如果您想保持應用程式的可用性，但不想繼續提供給特定作業系統版本上的客戶，您可以建立新的提交，並針對您想要在其上防止新取得的作業系統版本移除所有套件。 例如，如果您先前已有適用於 Windows Phone 8、Windows Phone 8.1 及 Windows 10 的套件，而您不想持續提供該 app 給 Windows Phone 8 上的客戶，則可從提交中移除 Windows Phone 8 套件。 發佈更新之後，就不會有任何 Windows Phone 8 上的新客戶能夠擷取該 app (但已經擁有該 app 的客戶仍能繼續使用)。 該 app 仍然可供 Windows Phone 8.1 和 Windows 10 上的新客戶使用。

## <a name="removing-packages-for-a-previously-supported-device-family"></a>移除先前支援之裝置系列的套件


如果您要移除您 app 先前支援之特定裝置系列的所有套件，系統會先提示以確認您確實要這樣做，您才能在 [套件]**** 頁面上儲存變更。

當您發佈移除 app 先前所支援裝置系列之套件的提交時，使用該裝置系列的新客戶將無法取得您的 app。 此後您還是可以發佈其他更新，以再次針對該裝置系列提供套件。

請注意，即使您移除支援特定裝置系列的所有套件，任何已安裝該 app 的現有客戶仍能夠使用它，且他們會取得您之後所提供的任何更新。

 

 





