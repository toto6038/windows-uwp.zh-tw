---
author: jnHs
Description: "管理您的 IAP 以允許您同時變更多個 IAP，而不是個別送出每個更新。"
title: "大量管理 IAP"
ms.sourcegitcommit: 475371dd55aa111f3743c03dc1600e8cfdbeb5b0
ms.openlocfilehash: ae4d4ed33b9bd10a2b01b336c942ad3212de6533


---

# 大量管理 IAP

> **重要** 此功能目前僅提供給已加入[開發人員中心測試人員計畫](dev-center-insider-program.md)的開發人員帳戶。 此功能實作可能會在它提供給所有開發人員使用前有所變更。 這份初步文件提供一些有關功能運作方式的基本資訊。

管理您的 IAP 以允許您同時變更多個 IAP，而不是個別送出每個更新。 您可以按一下 [管理大量的 IAP]****，從您 App 的概觀頁面存取此功能。

## 匯出目前的 IAP 資訊

若要開始，您需要下載 .csv 範本檔案。 如果您已經建立 IAP，此檔案將會包含它們的相關資訊。 如果沒有，它將會是空白的檔案，您可以用來輸入新 IAP 的資訊。 

若要產生並下載這個範本檔案，請按一下 [匯出 IAP]****，並將 .csv 檔案儲存到您的電腦。

.csv 檔案包含下列欄位： 

| 欄位名稱               | 說明                            | 必要？      |
|---------------------------|----------------------------------|----------------------|
| 產品識別碼    |  IAP 的[產品識別碼](set-your-iap-product-id.md#product-id)。  | 是。 IAP 發佈之後無法變更。 |
| 動作 |您匯入範本時想要套用的動作。 支援的值為 **Submit** (提交新的 IAP 或更新之前發佈的 IAP) 以及 **CreateDraft** (儲存變更而不提交至 [市集])。 |    是 |
| 產品類型  | IAP 的[產品類型](set-your-iap-product-id.md#product-type)。 支援的值為 **Consumable** 或 **Durable**。 | 是。 IAP 發佈之後無法變更。 |
| 產品存留期  | 針對 Durable IAP，這會是**永久** (適用於永遠不會過期的產品) 或設定的持續時間。 可接受的持續時間值為：**1 天、3 天、5 天、7 天、14 天、30 天、60 天、90 天、180 天、365 天**   | 是 (如果產品類型是 [耐久品]) |
| 內容類型  | IAP 的[內容類型](enter-iap-properties.md#content-type)。 針對大部分的 IAP，這應該是 **ElectronicSoftwareDownload**。 其他可接受的值為：**ElectronicBooks、ElectronicMagazineSingleIssue、ElectronicNewspaperSingleIssue、MusicDownload、MusicStreaming、OnlineDataStorageServices、VideoDownload、VideoStreaming、SoftwareAsAService** | 是 |
| 標記   | 在您的應用程式實作中使用的選用[標記](enter-iap-properties.md#tag)資訊。 | 否 |
| 基本價格    | 您想要提供 IAP 的[價格區間](set-iap-pricing-and-availability.md#base-price)。 必須是**免費**或格式為 **0.99USD** 的有效價格區間。 |   是 |
| 發行日期  | 您想要發佈 IAP 的日期。 可接受的值是**即時**、**手動**，或符合 [ISO 8601 標準](http://go.microsoft.com/fwlink/p/?LinkId=817237)的日期字串 。 | 是 |
| 標題    | 客戶會看到的 IAP 名稱，前面接著語言代碼以及分號。 例如，若要使用英文/美國的「範例標題」，您可以輸入*en-us;範例標題*。 其他語言的其他標題可以使用分號分隔。 每個標題的字元數必須在 100 個字元以下。     | 是 |
|說明   | 向客戶顯示的選擇性其他資訊，前面為語言地區設定程式碼以及分號。 例如，若要使用英文/美國的「這是範例」說明，您可以輸入 *en-us;這是範例*。 其他語言的其他標題可以使用分號分隔。 每個說明的字元數必須在 200 個字元以下。    | 否 |
| 市場 | 您想要提供 IAP 的一或多個[市場](define-pricing-and-market-selection.md#windows-store-consumer-markets)。 以分號分隔每個市場。 | 是 |
|關鍵字 | 在您的應用程式實作中使用的選擇性[關鍵字](enter-iap-properties.md#keywords)資訊。 | 否 |

## 匯入 IAP

在您可以匯入變更之前，您需要使用您想要進行的變更更新下載的.csv 檔案。

若要變更您已經發行的 IAP，請在試算表複本中更新您想要變更的值。 您可以移除您不想要更新的任何 IAP 資料列，或保留原狀。 請注意，如果該 IAP 已經有進行中的提交，您將無法使用 .csv 檔案進行變更。

> **重要** 提交您已經發佈的 IAP 更新時，您無法變更**產品識別碼**和**產品類型**欄位。

若要提交新的 IAP，請新增一列並輸入您新 IAP 的資訊。 請務必輸入所有必要資訊。 

當您完成所有變更時，請儲存 .csv 檔案 (使用相同的檔案名稱)，然後將它拖曳到指定的欄位來上傳檔案 (或按一下 [瀏覽您的檔案]****) 將會顯示變更的摘要，以及必須在提交之前修正的任何錯誤。 在您已驗證資訊正確無誤之後，您可以按一下 [提交至市集]****。 每個 IAP 會使用您已提供的資訊進行提交程序。




<!--HONumber=Jun16_HO4-->


