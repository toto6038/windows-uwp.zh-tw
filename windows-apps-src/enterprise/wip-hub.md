---
author: normesta
Description: This is a hub topic covering the full developer picture of how Windows Information Protection (WIP) relates to files, buffers, clipboard, networking, background tasks, and data protection under lock.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows 資訊保護 (WIP)
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Windows 資訊保護, 企業資料, 企業資料保護, edp, 啟發式應用程式
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.localizationpriority: medium
ms.openlocfilehash: dec05e663e6ca7390dc3974b8a3cde2971b50426
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765884"
---
# <a name="windows-information-protection-wip"></a>Windows 資訊保護 (WIP)

__備註__ Windows 資訊保護 (WIP) 原則可以套用至 Windows10 (版本 1607)。

WIP 會強制執行組織所定義的原則，以保護屬於組織的資料。 如果您的應用程式包含在這些原則中，則應用程式產生的所有資料都會受到原則限制。 本主題可協助您建置能順利強制執行這些原則的應用程式，且使用者的個人資料不會受到任何影響。
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>首先，什麼是 WIP？

WIP 是桌上型電腦、膝上型電腦、平板電腦與手機上支援組織的行動裝置管理 (MDM) 及行動應用程式管理 (MAM) 系統的功能。

WIP 搭配 MDM 可讓組織更能控制其所管理裝置上的資料處理方式。 有時使用者會在工作中使用自身裝置，但未註冊組織的 MDM。  在這類情況下，組織可使用 MAM，針對使用者於裝置上所安裝的特定企業營運應用程式，更進一步地掌控組織資料在其上的處理方式。

系統管理員可透過使用 MDM 或 MAM，識別哪些應用程式可存取屬於組織的檔案，以及使用者是否可以複製這些檔案中的資料，並將資料貼到個人文件。

它的運作方式如下： 使用者在組織的行動裝置管理 (MDM) 系統註冊他們的裝置。 管理組織中的系統管理員使用 Microsoft Intune 或 System Center Configuration Manager (SCCM) 定義原則，接著部署到註冊的裝置。

若使用者不須註冊其裝置，系統管理員會使用其 MAM 系統定義並部署適用於特定應用程式的原則。 使用者安裝任何這類的應用程式時，即會收到相關聯的原則。

原則會識別可以存取企業資料的應用程式 (稱為原則的*允許清單*)。 這些應用程式可以在剪貼簿上或透過分享協定存取受保護的企業檔案、虛擬私人網路 (VPN) 和企業資料。 原則也定義了管理資料的規則。 例如，資料是否可以從企業擁有的檔案複製和貼到非企業擁有的檔案。

若使用者將裝置從組織的 MDM 系統取消註冊，或將組織 MAM 系統所識別的應用程式取消註冊，系統管理員可從裝置以遠端方式抹除企業資料。

![WIP 週期](images/wip-lifecycle.png)

> **深入瞭解 WIP** <br>
* [Windows 資訊保護簡介](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [使用 Windows 資訊保護 (WIP) 保護您的企業資料](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

如果您的應用程式在允許清單中，則應用程式產生的所有資料都會受到原則限制。 這表示如果系統管理員撤銷使用者的企業資料存取權，這些使用者就會失去應用程式產生的所有資料的存取權。

如果您的應用程式只是設計成供企業使用就無所謂。 但如果您的應用程式建立的資料對使用者來說是個人資料，您會想要*啟發*您的應用程式，以明智地分辨企業資料和個人資料。 我們稱這種類型的應用程式為*企業啟發式應用程式*，因為它可以順利地強制執行企業原則，又同時保留使用者個人資料的完整性。

## <a name="create-an-enterprise-enlightened-app"></a>建立企業啟發式應用程式

使用 WIP API 來啟發您的應用程式，然後將您的應用程式宣告為企業啟發式應用程式。

如果您的應用程式取向是組織和個人用途，請啟發您的應用程式。

如果您想順利地強制執行原則項目，請啟發您的應用程式。

例如，如果原則允許使用者將企業資料貼到個人文件，您可以設計成讓使用者貼上資料前不需回應同意對話方塊。 同樣地，您也可以顯示自訂的資訊對話方塊以回應這類事件。

如果您已經準備好啟發您的應用程式，請參閱以下指南之一︰

**您使用 C# 建置的通用 Windows 平台 (UWP) 應用程式**

[Windows 資訊保護 (WIP) 開發人員指南](wip-dev-guide.md)。

**如果是使用 C++ 建置的傳統型應用程式：**

[Windows 資訊保護 (WIP) 開發人員指南 (C++)](http://go.microsoft.com/fwlink/?LinkId=822192)。


## <a name="create-non-enlightened-enterprise-app"></a>建立非啟發式企業應用程式

若您要建立的企業營運 (LOB) 應用程式非為個人用途，您可能無須加以啟發。

### <a name="windows-desktop-apps"></a>Windows 傳統型應用程式
您無須啟發 Windows 傳統型應用程式，但您應測試應用程式以確保其在原則下正常運作。 例如，啟動您的應用程式、加以使用，接著將裝置從 MDM 取消註冊。 然後，確定應用程式可以再次啟動。 若應用程式作業關鍵的檔案已加密，則應用程式可能不會啟動。 此外，檢閱與您應用程式互動的檔案，確保應用程式不會不慎將使用者的個人檔案加密。 這包括中繼資料檔案及影像等等。

測試完應用程式後，將此旗標新增至資源檔或您的專案，然後重新編譯應用程式。

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
MDM 原則不需要旗標，但 MAM 原則需要。

### <a name="uwp-apps"></a>UWP 應用程式

若您要讓應用程式涵蓋於 MAM 原則中，您應加以啟發。 原則若部署至 MDM 下的裝置則不需要它，但若您將應用程式散發給組織消費者，則要判斷其將使用的原則管理系統種類則會變得極為困難。 若要確保您的應用程式在兩種原則管理系統 (MDM 及 MAM) 都可使用，您應加以啟發。






 
