---
title: Web 服務
description: 了解如何建立您的應用程式的 Web 服務
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 web 服務
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600013"
---
# <a name="set-up-web-services-in-udc"></a>設定 UDC 中的 Web 服務

> [!WARNING]
> 下列文章是專為ID@Xbox和受管理的協力廠商開發人員只是因為 Web 服務設定的限制。 信賴憑證者的合作對象帳戶層級授與權限，才可供開發人員網路服務設定。 如果您沒有帳戶層級權限連絡以取得協助您開發的帳戶管理員 （補西牆） 的控制項。

如果他們想要自訂其應用程式/標題與 Xbox Live 服務進行互動的方式，發行者可以建立 Web 服務。 Web 服務是 「 發行者 」 層級設定，而且可以設定單一登入所擁有 「 發行者 」 在沙箱內的任何項目所呼叫。

若要定義 Web 服務的原因：

1. Xbox Live 使用者-讓您的 web 服務，來為 Xbox Live 的使用者提供單一登入，請提供單一登入，必須設定為信賴憑證者的合作對象的 Xbox Live。 設定如此一來，到 Xbox Live 驗證的使用者將會自動進行驗證到您的服務而不必重新輸入一組不同的認證。
2. 進行服務對服務呼叫從您的服務到 Xbox Live 服務-如果您的產品將會使用您的 web 服務的其中一個進行至 Xbox Live 服務的呼叫，直接或代表個別使用者，您將需要商業夥伴憑證。

1. ## <a name="create-a-web-service"></a>建立 Web 服務

1. 移至[夥伴中心儀表板](https://partner.microsoft.com/dashboard/windows/overview)  
2. 按一下 [存取] 頁面的右上角的齒輪形狀圖示**設定**下拉式清單。
3. 在下拉式清單中，選取**開發人員設定**。
4. 在左側導覽列中，展開 選擇**Xbox Live** ，然後選取**Web 服務**。

![web services gif ](../../images/dev-center/web-services/web-services.gif)

5. 在 [Web 服務] 頁面中，按一下**新的 Web 服務**。
6. 輸入 Web 服務名稱，然後選擇所需的存取類型。  
    * 遙測存取可讓您擷取任何您的遊戲的遊戲的遙測資料的服務。
    * 應用程式通道存取可供以程式設計的方式發行供透過 OneGuide twist 的主控台上的應用程式通道的授權單位擁有服務的媒體提供者。
7. 按一下 [儲存]

此時，您已定義的服務和 Xbox Live 並知道服務存在。 根據建立 web 服務的原因，您必須設定信賴憑證者 Parties(Single Sing-On) 或商業夥伴 Certificates(Service-to-service calls)。  

## <a name="configure-relying-party"></a>設定信賴憑證者的合作對象

Web 服務必須設定為信賴憑證者的合作對象的 Xbox live 以提供單一登入體驗給 Xbox Live 使用者 – Xbox live 驗證的使用者將會自動驗證 web 服務而不必重新輸入將組不同的認證。 若要達成此目的，則必須建立 Xbox 服務與 web 服務之間的信任。 一組宣告 （例如玩家代號、 裝置類型，標題識別碼） 可做為信賴憑證者的合作對象組態的一部分來強制執行此信任。 這是 Xbox Live 和 web 服務來協助自動驗證使用者間交換資訊。

### <a name="create-a-relying-party"></a>建立信賴憑證者的合作對象

1. 前往合作夥伴中心儀表板  
2. 按一下 [存取] 頁面的右上角的齒輪形狀圖示**設定**下拉式清單。
3. 在下拉式清單中，選取**開發人員設定**。
4. 在左側導覽列中，展開 選擇**Xbox Live** ，然後選取**信賴憑證者的合作對象**。
5. 在 [信賴憑證者的合作對象] 頁面中，按一下**新信賴憑證者的合作對象**。
6. 輸入的格式如下的信賴憑證者合作對象的 URI: *example.com*。
7. 選取要用來確保安全性的信賴憑證者的合作對象服務的加密類型。
8. 如果您選取與上一個步驟中的共用金鑰的對稱式加密，請按一下**產生新金鑰**取得新的共用的金鑰。 請依照指示來安全地儲存金鑰畫面上。
9. 請輸入**權杖的存留時間**以小時為單位。
10. 底下**宣告**，下拉式清單中提供的信賴憑證者的合作對象服務可以使用以用於驗證的宣告清單。 選取您想要使用的所有宣告。 選取的宣告會出現下方的下拉式清單中。 依預設，會在該區域內填入一些標準的宣告。
11. 按一下 **儲存**完成時。  

## <a name="configure-a-business-partner-certificate"></a>設定商務夥伴憑證

如果您的產品將使用其中一項 web 服務呼叫到 Xbox Live 服務，直接或代表個別使用者，您將需要商業夥伴憑證。

### <a name="generate-a-business-partner-certificate"></a>產生商業夥伴憑證

已成功建立 Web 服務之後，以繼續進行下列步驟。  

1. 在 Web 服務 頁面中，尋找您想要使用的商務夥伴憑證產生關聯的 web 服務。
2. 選取 **產生憑證**對所選的 web 服務的連結。
3. 按一下 **顯示選項**旁邊產生新的憑證。 這會顯示應該從 PowerShell 執行系統管理員權限的命令。
4. 執行所有命令一之後其他應該成功提供 Base64 編碼的 blob。 這是公用金鑰。 從 PowerShell 複製公開金鑰，並將它貼在 CSP Blob 的預留位置。
5. 按一下 **下載**，並遵循頁面上的繫結的憑證。
    1. 使用相同的電腦，您用來產生公開金鑰。
    2. 在 PowerShell 中執行此命令： *mmc.exe*
    3. 選取檔案，並選取 新增/移除嵌入式管理單元。
    4. 選取憑證，然後選取 [新增]。 請務必選取 憑證 嵌入式管理單元中的 電腦帳戶，按一下 完成 然後按一下 確定。
    5. 開啟 Personal\Certificate 存放區。
    6. 以滑鼠右鍵按一下 憑證 並選取 所有工作，都選取 匯入。
    7. 選取您從 UDC 下載的憑證。
    8. 以滑鼠右鍵按一下 UI 中的憑證已匯入之後，選取 所有工作並都選取 匯出。
    9. 遵循 「 匯出精靈 」，並務必選取以匯出包含憑證的私密金鑰。
    10. 完成匯出精靈 」。