---
title: Xbox Live 設定組態
description: 說明如何設定 Xbox Live 的安裝程式，以及在合作夥伴中心。
ms.assetid: ''
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live、 Xbox、 遊戲、 uwp、 windows 10、windows 一個 Xbox、 Xbox Live 設定合作夥伴中心
ms.openlocfilehash: 9a846a4b7f0069216e92eb123b33d9fc0f7f67c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612823"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>在合作夥伴中心設定 Xbox Live 的安裝程式

您可以使用[合作夥伴中心](https://developer.microsoft.com/dashboard)設定一組初始的 Xbox Live 的屬性，用您的遊戲與相關聯。 新增組態，執行下列動作：

1. 瀏覽至**Xbox Live 的安裝程式**一節以取得您的標題，位於**Services** > **Xbox Live** > **Xbox Live 的安裝程式**.
2. 在此頁面上，您可以設定標題的名稱、 預設地區設定、 產品類型、 裝置系列和禁運日期。 在您完成設定您的組態，請按一下**儲存**將變更送出 按鈕。

## <a name="title-names"></a>標題名稱
上即可**新增當地語系化標題**，您可以輸入您的產品的名稱，並選取要將其以當地語系化語言。 請注意，這裡的標題名稱應該對應至您所提交的 [內容] 頁面上定義當地語系化的產品名稱。 預設為 英文 (EN-US)。

![在合作夥伴中心內的 [新增當地語系化的標題] 對話方塊的影像](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>預設的地區設定
此選項可讓您設定要用來設定您的所有字串 Xbox Live 服務組態中的預設語言。 比方說，如果您將預設的地區設定設定為西班牙文 (ES-ES)，而且您想要設定的成就，然後最少的成就名稱和描述會一定要以西班牙文。 換句話說，您無法將此選項設定為西班牙文，但只提供英文版的成就資訊。 中的預設地區設定相同的版本，必須提供所有在 Xbox Live 服務組態。 根據預設，預設的地區設定設為 英文 (EN-US)。
> [!NOTE]
> 此外，所有字串中可都當地語系化的都當地語系化字串頁面。  

![選取下拉式清單，以在合作夥伴中心內選擇您的預設地區設定的映像](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>產品類型
下拉式選單中，可讓您變更的產品類型。 它會預設為類型**遊戲**。 所做的選擇會影響可供您的 Xbox Live 功能。 您有三個選項可供選擇：
1. 應用程式 
2. Game 
3. 遊戲的示範 

![選取下拉式清單選擇您的產品類型在合作夥伴中心內的影像](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>裝置系列
此設定可讓您選擇的裝置您的標題可以存取 Xbox Live 類型。 根據預設，會啟用所有裝置系列。 您可以檢查，讓他們的裝置。

![選取核取方塊，以在合作夥伴中心內選取的裝置系列的映像](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>禁運日期
當您的 Xbox Live 設定上線公開時，將會決定您所選取的日期。 請務必請注意，即使您為零售版發行您的變更就不會即時除非達到禁運日期。 若要更進一步地說明：
1. 如果您選取在未來的日期時，所做的變更會變成公開上市在該日期。
2. 如果您選取一個日期在過去，所做的變更會變成在公開上市，只要您將變更發佈至零售版。

按一下日期時間選擇器，並展開可讓您選取的精確日期和時間。 一旦您按一下**確定**，禁運日期會設定。

![在合作夥伴中心設定禁運日期的映像](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>[進階設定] 下

您可以按一下**顯示的選項**來設定**多點出席**。 存在多個點可讓相同的使用者登入 Xbox Live 從多個裝置在相同的時間。 Xbox Live 功能，例如，成就與多人會有有限存取。 因此，適用於遊戲不建議您使用此選項。 啟用此選項，核取方塊。
