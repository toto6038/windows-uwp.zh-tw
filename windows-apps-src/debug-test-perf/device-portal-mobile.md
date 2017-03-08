---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "行動裝置的 Device Portal"
description: "了解 Windows Device Portal 如何讓您從遠端設定並管理行動裝置。"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 54660777706fbfdc54b08da025c2f280f194c010
ms.lasthandoff: 02/07/2017

---
# <a name="device-portal-for-mobile"></a>行動裝置的 Device Portal

從 Windows 10 版本 1511 開始，即針對行動裝置系列提供額外的開發人員功能。 只有在裝置上啟用開發人員模式時，才能使用這些功能。

如需啟用開發人員模式的資訊，請參閱[啟用您的裝置以用於開發](../get-started/enable-your-device-for-development.md)。

![Device Portal 設定](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>在 Windows Phone 上設定 Device Portal

### <a name="turn-on-device-discovery-and-pairing"></a>開啟裝置探索和配對

若要連線到 Device Portal，您必須啟用裝置探索和 Device Portal。 這可讓您將手機與電腦或其他 Windows 10 裝置配對。 這兩個裝置都必須透過有線或無線連線連接到網路的同一個子網路，或者必須透過 USB 來連接它們。

第一次連線到 Device Portal 時，系統會要求您提供 6 個字元且區分大小寫的安全性驗證碼。 這可確保您具備手機的存取權，並讓您保持安全免於受到攻擊者入侵。 按下手機上的 [配對] 按鈕，即會產生並顯示驗證碼，接著請在瀏覽器的文字方塊中輸入這 6 個字元。

![開發人員模式裝置探索設定](images/device-portal/mob-dev-mode-pairing.png)

您可以從 3 種連接到 Device Portal 的方式中進行選擇：USB、本機主機，以及透過區域網路 (包括 VPN 和網際網路共用)。

**連接到 Device Portal**

1. 在瀏覽器中，針對您使用的連接類型輸入位址 (如下所示)。

    - USB： `http://127.0.0.1:10080`

    當手機透過 USB 連線連接到電腦時，請使用這個位址。 這兩個裝置都必須具備 Windows 10 版本 1511或更新版本。
    
    - 本機主機： `http://127.0.0.1`

    使用這個位址來在手機上適用於 Windows 10 行動裝置版的 Microsoft Edge 中於本機檢視 Device Portal。
    
    - 區域網路： `https://<The IP address or hostname of the phone>`

    使用這個位址來透過區域網路連線。

    手機的 IP 位址會顯示於手機上的 Device Portal 設定中。 HTTPS 需要進行驗證和安全通訊。 主機名稱 (可在 [設定] &gt; [系統] &gt; [關於] 中編輯) 也可以用來在區域網路上存取 Device Portal (例如 http://Phone360)。這對於經常會變更網路或 IP 位址，或是需要與他人共用的裝置來說相當有用。 

2. 按下手機上的 [配對] 按鈕，來產生並顯示所需的安全性驗證碼

3. 在您瀏覽器的 Device Portal 密碼方塊中，輸入 6 個字元的安全性驗證碼。

4. (選擇性) 選取瀏覽器中的 [記住我的電腦] 方塊，以便日後記住這個配對。

以下是 Windows Phone 上開發人員設定頁面的 [Device Portal] 區段。

![Device Portal 設定](images/device-portal/mob-dev-mode-portal.png)

如果您是在受保護的環境中使用 Device Portal (例如測試實驗室)，您可以在此環境中信任區域網路上的每個人、裝置上沒有個人資訊，而且有獨特的需求，則您可以停用驗證。 這會啟用未加密的通訊，並允許具有您手機 IP 位址的任何人對它進行控制。

## <a name="tool-notes"></a>工具附註

## <a name="device-portal-pages"></a>Device Portal 頁面
### <a name="processes"></a>處理程序

終止任意程序的能力並不包含在 Windows Mobile Device Portal 中。 

行動裝置上的 Device Portal 提供標準的頁面集。 如需詳細描述，請參閱 [Windows Device Portal 概觀](device-portal.md)。

- 應用程式管理員
- App 檔案總管 (隔離的存放裝置總管)
- 處理程序
- 效能圖表
- Windows 事件追蹤 (ETW)
- 效能追蹤 (WPR) 
- 裝置
- 網路功能
