---
Description: 了解如何在您已在 Windows 應用程式封裝中的預先封裝的桌面應用程式中新增新式體驗 Windows 10 使用者。
title: 將封裝的桌面應用程式現代化
ms.date: 04/22/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6d71233bc7b96af9d9b261406d6b149f36f65f29
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985288"
---
# <a name="features-that-require-package-identity"></a>需要封裝識別碼的功能

如果您想要更新您的桌面應用程式與[現代 Windows 10 體驗](index.md)，許多功能都只封裝在 MSIX 封裝中的傳統型應用程式。

MSIX 是針對所有 Windows 應用程式、 WPF、 Windows Forms 和 Win32 應用程式提供通用的封裝體驗的新式 Windows 應用程式封裝格式。 封裝您的傳統型 Windows 應用程式，可讓您將動態磚和通知等現代 Windows 10 體驗整合到您的應用程式。 它也會取得您存取強大的安裝和更新的體驗、 受管理的安全性模型與彈性的功能系統，支援 Microsoft Store、 企業管理和許多自訂發佈模型。 如需詳細資訊，請參閱 <<c0> [ 桌面應用程式封裝](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)MSIX 文件中。

如果您封裝傳統型應用程式時，您可以使用 UWP Api 需要封裝識別碼、 封裝擴充功能和 UWP 應用程式封裝中的元件。 如需詳細資訊，請參閱下列文章。

## <a name="use-uwp-apis-that-require-package-identity"></a>使用 UWP Api 需要的套件識別

某些 UWP Api 需要[封裝身分識別](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)用於傳統型應用程式。 （包括封裝資訊清單） MSIX 套件有提供此身分識別。

如需詳細資訊，請參閱 <<c0> [ 這份 Api](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>整合套件擴充功能

如果您的應用程式需要與系統整合 (例如： 建立防火牆規則)、 描述您的應用程式的套件資訊清單中的這些項目，以及系統將會完成其餘部分。 針對大部分的工作，您完全不需要撰寫任何程式碼。 位元的 XML 資訊清單中，您可以執行動作像是啟動處理序，當使用者登入時，將您的應用程式整合到 [檔案總管] 中，新增您的應用程式的列印目標出現在其他應用程式清單。

如需詳細資訊，請參閱 <<c0> [ 傳統型應用程式整合套件擴充功能](desktop-to-uwp-extensions.md)。

## <a name="extend-with-uwp-components"></a>使用 UWP 元件，擴充

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 一般情況下，您應該先判斷是否可以新增您的經驗，包括[增強](desktop-to-uwp-enhance.md)UWP Api 與您現有的桌面應用程式。 如果您有使用 UWP 元件，來達到體驗，然後您可以將 UWP 專案新增至您的解決方案和使用桌面應用程式和 UWP 元件之間進行通訊的應用程式服務。

如需詳細資訊，請參閱 <<c0> [ 延伸您的傳統型應用程式與 UWP 元件](desktop-to-uwp-extend.md)。

## <a name="distribute"></a>散佈

您可以將您的應用程式散發發行 Microsoft Store 或側載到其他系統。

請參閱[散發傳統型應用程式封裝的](desktop-to-uwp-distribute.md)。
