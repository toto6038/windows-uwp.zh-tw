---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: 啟用您的裝置以用於開發
description: 了解如何藉由在 Visual Studio 中啟用開發人員模式，讓您的 Windows 10 裝置進行開發和偵錯。
keywords: 開始使用開發人員授權 Visual Studio, 開發人員授權啟用裝置
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011866"
---
# <a name="enable-your-device-for-development"></a>啟用您的裝置以用於開發

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>啟用開發人員模式、側載應用程式，以及存取其他開發人員功能

![啟用您的裝置以進行開發](images/developer-poster.png)

> [!IMPORTANT]
> 如果您不是在自己的電腦上建立自己的應用程式，就不需要啟用開發人員模式。 如果您要嘗試修正電腦的問題，請參閱 [Windows 說明](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10)。 如果您是第一次進行開發，最好下載所需的工具來[取得設定](get-set-up.md) 。

如果您使用電腦進行一般的日常活動，例如玩遊戲、瀏覽網頁、收發電子郵件或使用 Office 應用程式等等，您「不」必啟用開發人員模式，事實上您不應啟用。 此頁面上剩餘的資訊對您而言不重要，您可以放心地返回任何您原本進行的動作。 感謝您的瀏覽！

但是，如果您是第一次在電腦上使用 Visual Studio 撰寫軟體，您「將」需要在開發電腦上以及在任何要用來測試程式碼的裝置上，啟用開發人員模式。 當開發人員模式未啟用時開啟 UWP 專案會開啟 [開發人員專用] 設定頁面，或造成此對話方塊出現在 Visual Studio 中：

![Visual Studio 中顯示的 [啟用開發人員模式] 對話方塊](images/latestenabledialog.png)

當您看到這個對話方塊時，請按一下 [開發人員專用設定] 來開啟 [開發人員專用] 設定頁面。

> [!NOTE]
> 您可以隨時移至 [開發人員專用] 頁面來啟用或停用 [開發人員模式]：只要在工作列中的 Cortana 搜尋方塊中輸入「開發人員專用」即可。

## <a name="accessing-settings-for-developers"></a>存取開發人員專用設定

若要啟用開發人員模式，或存取其他設定：

1.  從 [開發人員專用] 設定對話方塊中，選擇您需要的存取層級。
2.  閱讀您所選設定的免責聲明，然後按一下 [是] 來接受變更。

> [!NOTE]
> 啟用 Developer 模式需要系統管理員權限。 如果您的裝置是由組織所擁有，此選項可能會停用。

### <a name="developer-mode-features"></a>開發人員模式功能

[開發人員模式] 取代了 Windows 8.1 對開發人員授權的需求。  除了側載功能，[開發人員模式] 設定還會啟用偵錯和其他部署選項。 這包括啟動 SSH 服務來允許此裝置成為部署目標。 若要停用此服務，您必須停用 [開發人員模式]。

當您在桌面上啟用開發人員模式時，系統會安裝一套功能，包括︰
- Windows 裝置入口網站。 只有在已開啟 [啟用裝置入口網站] 選項時，才會啟用裝置入口網站並為其設定防火牆規則。
- 為 SSH 服務安裝及設定允許遠端安裝應用程式的防火牆規則。 啟用 [裝置探索] 將會開啟 SSH 伺服器。

如需這些功能的詳細資訊，或如果您在安裝程式中遇到困難，請參閱 [開發人員模式的功能和調試](developer-mode-features-and-debugging.md)程式。

## <a name="see-also"></a>另請參閱

* [註冊 Windows 帳戶](sign-up.md)
* [開發人員模式功能和調試](developer-mode-features-and-debugging.md)。