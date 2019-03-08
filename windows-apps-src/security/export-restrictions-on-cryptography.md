---
title: 密碼編譯的出口限制
description: 使用這項資訊判斷您的 app 使用密碼編譯的方式，是否會使它無法被刊登於 Microsoft Store 中。
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10 uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 6d445e5164d542a7e10f136a5fb238c575f35c2d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656013"
---
# <a name="export-restrictions-on-cryptography"></a>密碼編譯的出口限制



使用這項資訊判斷您的 app 使用密碼編譯的方式，是否會使它無法被刊登於 Microsoft Store 中。

美國商務部的工業安全局規定使用特定加密類型的技術出口事項。 Microsoft Store 列出的所有應用程式必須遵守這些法令規定，因為這些應用程式檔案可能會儲存在美國。 即使 app 開發人員從其他國家/地區上傳並在美國以外的地方發行的 app 也必須遵守這些法規。 因此，將 app 送出到 Microsoft Store 時，所有 app 開發人員必須確定他們的 app 中沒有包含這些法規管制使用的任何技術。

> **附註**  此處提供的資訊提供一些指引，但您必須負責為應用程式開發人員正藉此確定您的應用程式遵守所有適用的法令與法規 Microsoft Store 中發佈應用程式。

 

如需詳細資訊，關於美國部門的商務和機構的產業和安全性，請參閱[關於局的產業和安全性](https://go.microsoft.com/fwlink/p/?LinkID=245644)。

如需規範技術 (包含加密) 出口的出口管制條例 (EAR) 詳細資訊，請參閱 [EAR 管制使用密碼編譯的項目](https://go.microsoft.com/fwlink/p/?LinkID=245645)。

## <a name="governed-uses"></a>受規範的使用方式

首先，請判斷您的 app 是否是使用受出口管制條例規範的密碼編譯類型。 問題包含以下清單中顯示的範例；但請記住，這份清單未詳列每種可能的密碼編譯應用方式。

> **重要**  考慮不只您的應用程式，但也所有的軟體程式庫、 公用程式和作業系統元件，其中包含您的應用程式，或連結至您所撰寫的程式碼。

-   數位簽章的任何用途，例如驗證或完整性檢查
-   加密您的應用程式使用或存取的任何資料或檔案
-   金鑰管理、憑證管理或是與公開金鑰基礎結構互動的任何項目
-   使用安全通訊管道，例如 NTLM、Kerberos、安全通訊端層 (SSL) 或是傳輸層安全性 (TLS)
-   密碼或其他形式的資訊安全性加密
-   複製保護或數位版權管理 (DRM)
-   防毒保護

如需密碼編譯 app 的最新完整清單，請參閱[使用加密之項目的 EAR 控制項](https://go.microsoft.com/fwlink/p/?LinkID=245645)。

## <a name="non-restricted-uses"></a>非限制的使用方式

請注意，某些密碼編譯的應用方式不會受到限制。 不受限制的工作如下：

-   密碼加密
-   複製保護
-   Authentication
-   數位版權管理
-   使用數位簽章

如需密碼編譯 app 的最新完整清單，請參閱[使用加密之項目的 EAR 控制項](https://go.microsoft.com/fwlink/p/?LinkID=245645)。

如果您的 app 會呼叫、支援、包含或使用密碼編譯或加密來進行未列在此清單中的任何工作，則必須提供出口管制分類號碼 (ECCN)。

如果您沒有 ECCN，請參閱 [ECCN 問答集](https://go.microsoft.com/fwlink/p/?LinkID=245646)。
