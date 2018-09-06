---
author: jnHs
Description: If your developer account has been granted the appropriate permissions, you can generate and download preinstall packages so that an OEM can include your app in their OS image.
title: 針對 OEM 產生預先安裝套件
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 598a73b291d5f8b3c004f1e9adeddf0b92b841ab
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3403081"
---
# <a name="generate-preinstall-packages-for-oems"></a>針對 OEM 產生預先安裝套件

如果您的開發人員帳戶已被授與適當權限，您就可以產生並下載預先安裝套件，讓 OEM 能夠使用該預先安裝套件將您的 App 包含在其 OS 映像中。 只有在 OEM 贊助的開發人員帳戶上才會啟用預先安裝權限。


## <a name="important-preinstall-policy--limitations"></a>重要的預先安裝原則與限制

預先安裝 app 必須透過 Windows 開發人員中心認證，才能擁有最新的市集授權，app 才能連線到市集接收 app 更新。

預先安裝的所有 app 在所有市場都必須維持免費。


## <a name="generating-preinstall-packages"></a>產生預先安裝套件

在使用預先安裝權限啟用帳戶之後，請完成以下步驟：

1.  在儀表板中，瀏覽至要預先安裝的 app。
2.  在左瀏覽功能表中，展開 **\[應用程式管理\]**，然後選取 **\[目前的套件\]**。
3.  在 **\[為作業系統要求預先安裝封裝\]** 區段中，選取 **\[啟用可下載的套件\]**。
4.  在確認對話方塊中，選取 **\[啟用\]**。
5.  尋找您要下載的套件，然後選取適當的 **\[產生套件\]** 連結。

    > [!NOTE]
    > 預先安裝套件的產生時間將會隨著已選取套件的大小而有所不同。 您可以離開此頁面，並在稍後返回，或在產生套件時讓頁面保留開啟。

6.  套件產生之後，將會出現 **\[下載套件\]** 連結。 選取此連結可下載 .zip 檔案。

接著，您可以將此 .zip 檔案提供給 OEM，以便包含在其作業系統映像中。


## <a name="support"></a>支援

如果您對於產生預先安裝套件有其他疑問，請寄送電子郵件到 <partnerops@microsoft.com>。

 

 




