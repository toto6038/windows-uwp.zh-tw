---
Description: 探索桌面 Win32 應用程式傳送快顯通知所需的不同選項
title: 傳統型應用程式的快顯通知
label: Toast notifications from desktop apps
template: detail.hbs
ms.date: 05/01/2018
ms.topic: article
keywords: windows 10，uwp，win32，桌面，快顯通知，桌面橋接器，msix，sparse 封裝，傳送快顯通知，com 伺服器，com 啟動程式，com，假 com，無 com，不含 com 的選項
ms.localizationpriority: medium
ms.openlocfilehash: b84120a592a1c2f5f18c6b6121568cbf126a582e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234545"
---
# <a name="toast-notifications-from-desktop-apps"></a>傳統型應用程式的快顯通知

桌面應用程式（包括封裝的[MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview)應用程式、使用[稀疏套件](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)來取得套件識別的應用程式，以及傳統的非封裝 Win32 應用程式）都可以傳送互動式快顯通知，就像 Windows 應用程式一樣。 不過，傳統型應用程式有幾個不同選項，因為不同的啟用配置。

在本文中，我們列出您可在Windows 10 上傳送快顯通知的選項。 每個選項完全支援...

* 保存在控制中心中
* 可從快顯和控制中心內啟用
* 可在 EXE 未執行時啟用

## <a name="all-options"></a>所有選項

下表顯示您的傳統型應用程式內支援快顯通知的選項，以及對應的支援功能。 您可以使用表格來選取最適合您情形的選項。<br/><br/>

| 選項 | 視覺效果 | 動作 | 輸入 | 在處理程序中啟用 |
| -- | -- | -- | -- | -- |
| [COM 啟動器](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [No COM / Stub CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>偏好的選項 - COM 啟動器

這是適用于桌面應用程式的慣用選項，並支援所有通知功能。 不用擔心「COM 啟動器」，我們有程式庫[適用於 C#](send-local-toast-desktop.md) 和 [C++ 應用程式](send-local-toast-desktop-cpp-wrl.md)，可讓這非常簡單，即使您從未撰寫過 COM 伺服器。<br/><br/>

| 視覺效果 | 動作 | 輸入 | 在處理程序中啟用 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

透過 COM 啟動器選項時，您可以在您的 App 中使用下列通知範本和啟用類型。<br/><br/>

| 範本和啟用類型 | MSIX/sparse 封裝 | 傳統型 Win32 |
| -- | -- | -- |
| ToastGeneric 前景 | ✔️ | ✔️ |
| ToastGeneric 背景 | ✔️ | ✔️ |
| ToastGeneric 通訊協定 | ✔️ | ✔️ |
| 舊版範本 | ✔️ | ❌ |

> [!NOTE]
> 如果您將 COM 啟動項新增至現有的 MSIX/sparse 封裝應用程式，前景/背景和舊版通知啟動現在會啟用您的 COM activator，而不是您的命令列。

若要瞭解如何使用此選項，請參閱[從 Desktop c # 應用程式傳送本機](send-local-toast-desktop.md)快顯通知或[從 Desktop c + + WRL 應用程式傳送本機](send-local-toast-desktop-cpp-wrl.md)快顯通知。


## <a name="alternative-option---no-com--stub-clsid"></a>其他選項 - No COM / Stub CLSID

這是您若無法實作 COM 啟動器時的替代選項。 不過，您將會犧牲幾個功能，例如輸入支援 (快顯通知上的文字方塊) 和處理程序的啟用。<br/><br/>

| 視覺效果 | 動作 | 輸入 | 在處理程序中啟用 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

使用此選項，您若支援傳統型 Win32，您會更受限於您可使用的通知範本和啟用類型，如下所示。<br/><br/>

| 範本和啟用類型 | MSIX/sparse 封裝 | 傳統型 Win32 |
| -- | -- | -- |
| ToastGeneric 前景 | ✔️ | ❌ |
| ToastGeneric 背景 | ✔️ | ❌ |
| ToastGeneric 通訊協定 | ✔️ | ✔️ |
| 舊版範本 | ✔️ | ❌ |

針對使用[稀疏套件](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)的封裝[MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview)應用程式和應用程式，只要傳送像是 UWP 應用程式的快顯通知就可以了。 當使用者按下您的快顯通知時，您的 App 將會以您在快顯通知中指定的啟動引述啟動命令列。

對於傳統型 Win32 應用程式，請設定 AUMID，讓您可以傳送快顯通知，然後在您的快速鍵也指定 CLSID。 這可以是任何隨機 GUID。 切勿新增 COM 伺服器/啟動器。 您將新增的「stub」COM CLSID，這會導致控制中心保存通知。 請注意，您只可以使用通訊協定啟用的快顯通知，因為 stub CLSID 將會中斷任何其他快顯通知的啟用。 因此，您必須更新您的 App 為支援通訊協定啟用，而且讓快顯通知通訊協定啟動您自己的 App。


## <a name="resources"></a>資源

* [從傳統型 C# 應用程式傳送本機快顯通知](send-local-toast-desktop.md)
* [從傳統型 C++ WRL 應用程式傳送本機快顯通知](send-local-toast-desktop-cpp-wrl.md)
* [快顯通知內容文件](adaptive-interactive-toasts.md)
