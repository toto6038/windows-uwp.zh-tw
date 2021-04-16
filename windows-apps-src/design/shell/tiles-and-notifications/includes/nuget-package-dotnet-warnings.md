---
ms.openlocfilehash: e4f09efa1bd6d6884e9a32b46aff1bf586a16e60
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559397"
---
> [!IMPORTANT]
> 仍使用 packages.config 的 .NET Framework 桌面應用程式必須遷移至 PackageReference，否則將無法正確參考 Windows 10 Sdk。 在您的專案中，以滑鼠右鍵按一下 [參考]，然後按一下 [將 packages.config 遷移至 PackageReference]。
> 
> .NET Core 3.0 WPF 應用程式必須更新為 .NET Core 3.1，否則 Api 將不存在。
> 
> .NET 5 應用程式必須 [使用其中一個 Windows 10 tfm](https://docs.microsoft.com/dotnet/standard/frameworks#how-to-specify-a-target-framework)，否則將會遺失快顯的傳送和管理 api （例如） `Show()` 。 將您的 TFM 設定為 `net5.0-windows10.0.17763.0` 或更高。
