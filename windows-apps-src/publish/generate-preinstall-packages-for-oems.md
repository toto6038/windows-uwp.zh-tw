---
Description: 如果您的開發人員帳戶已被授與適當權限，您就可以產生並下載預先安裝套件，讓 OEM 能夠使用該預先安裝套件將您的 App 包含在其 OS 映像中。
title: 針對 OEM 產生預先安裝套件
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1ab17adc80a643c04ac7793945486c3ff975fde5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643133"
---
# <a name="generate-preinstall-packages-for-oems"></a>針對 OEM 產生預先安裝套件

如果您的開發人員帳戶已被授與適當權限，您就可以產生並下載預先安裝套件，讓 OEM 能夠使用該預先安裝套件將您的 App 包含在其 OS 映像中。 只有在 OEM 贊助的開發人員帳戶上才會啟用預先安裝權限。


## <a name="important-preinstall-policy--limitations"></a>重要的預先安裝原則與限制

預先安裝的應用程式必須透過認證[合作夥伴中心](https://partner.microsoft.com/dashboard)擁有最新的存放區授權，使其能夠連線到市集，並收到應用程式更新。

預先安裝的所有 app 在所有市場都必須維持免費。


## <a name="generating-preinstall-packages"></a>產生預先安裝套件

在使用預先安裝權限啟用帳戶之後，請完成以下步驟：

1.  在合作夥伴中心，瀏覽至預先安裝的應用程式。
2.  在左瀏覽功能表中，展開 **\[應用程式管理\]**，然後選取 **\[目前的套件\]**。
3.  在 **\[為作業系統要求預先安裝封裝\]** 區段中，選取 **\[啟用可下載的套件\]**。
4.  在確認對話方塊中，選取 **\[啟用\]**。
5.  尋找您要下載的套件，然後選取適當的 **\[產生套件\]** 連結。

    > [!NOTE]
    > 預先安裝套件的產生時間將會隨著已選取套件的大小而有所不同。 您可以離開此頁面，並在稍後返回，或在產生套件時讓頁面保留開啟。

6.  套件產生之後，將會出現 **\[下載套件\]** 連結。 選取此連結可下載 .zip 檔案。

接著，您可以將此 .zip 檔案提供給 OEM，以便包含在其作業系統映像中。


## <a name="support"></a>支援

如果您還有其他產生預先安裝套件的相關問題，請寄電子郵件<partnerops@microsoft.com>。

 

 




