---
Description: 無論您的 app 是否免費，都可以直接從 app 內銷售內容、其他 app 或新的 app 功能 (例如解除鎖定遊戲的下一個關卡)。 以下示範如何在 app 中啟用這些產品。
title: 啟用 app 內產品購買
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
關鍵字︰App 內的購買選項
關鍵字：App 內購買
關鍵字：App 內產品
關鍵字：如何支援應用程式內
關鍵字：App 內購買程式碼範例
關鍵字：App 內的購買選項程式碼範例
---

# 啟用 app 內產品購買

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

無論您的 app 是否免費，都可以直接從 app 內銷售內容、其他 app 或新的 app 功能 (例如解除鎖定遊戲的下一個關卡)。 以下示範如何在 app 中啟用這些產品。

**注意：**試用版的應用程式無法提供 app 內產品。 使用試用版 app 的客戶只有在購買 app 的完整版本後，才能購買 app 內產品。

## 先決條件

-   要新增功能讓客戶購買的 Windows 應用程式。
-   初次撰寫並測試新應用程式內產品的程式碼時，您必須使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 物件，而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 物件。 如此一來，您就可以利用對授權伺服器進行模擬呼叫來驗證授權邏輯，而不是呼叫使用中的伺服器。 若要這樣做，您必須自訂 %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中名為 "WindowsStoreProxy.xml" 的檔案。 Microsoft Visual Studio 模擬器會在您第一次執行您的 app 時建立這個檔案，或者您也可以在執行階段載入自訂的檔案。 如需詳細資訊，請參閱 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)。
-   本主題也會參照[市集範例](http://go.microsoft.com/fwlink/p/?LinkID=627610) (英文) 中提供的程式碼範例。 這個範例非常適合用來體驗實機操作針對通用 Windows 平台 (UWP) app 提供的不同貨幣選項。

## 步驟 1：初始化應用程式的授權資訊

在您的 app 進行初始化時，請透過初始化 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 或 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 來為 app 取得 [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) 物件，以啟用購買 app 內產品的功能。

```CSharp
void AppInit()
{
    // some app initialization functions 

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## 步驟 2：在您的應用程式新增 App 內購買選項

針對您想要透過應用程式內產品提供的每項功能，建立一個購買選項，然後新增至您的 app。

**重要事項：**在將您的應用程式送出至市集之前，您必須先把要呈現給客戶的所有應用程式內產品新增至應用程式。 如果您想要稍後再新增 app 內產品，您就必須更新應用程式，然後重新送出新版本。

1.  **建立應用程式內的購買選項權杖**

    您可以依權杖識別您應用程式中的每個應用程式內產品。 這個權杖是您定義的字串，並在應用程式和市集中用來識別特定的應用程式內產品。 請指定一個既唯一 (對您的應用程式來說) 又有意義的名稱，以便在撰寫程式碼時，可以快速地識別其正確功能。 以下是一些名稱的範例：

    -   "SpaceMissionLevel4"
    -   "ContosoCloudSave"
    -   "RainbowThemePack".

2.  **以條件性區塊來撰寫功能的程式碼**

    針對與應用程式內產品關聯的每個功能，您必須將其程式碼放到條件性區塊中來測試，以查看客戶是否擁有可使用該功能的授權。

    下列範例示範如何在授權專屬的條件性區塊中，撰寫 **featureName** 產品功能的程式碼。 **featureName** 字串是可在應用程式內唯一識別這個產品的權杖，同時也可用來在市集中識別此產品。

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive) 
        {
            // the customer can access this feature
        } 
        else
        {
            // the customer can' t access this feature
        }
    ```

3.  **新增此功能的購買 UI**

    您的應用程式也必須提供購買機制，讓客戶能夠購買應用程式內產品所提供的產品或功能。 客戶無法透過市集以購買完整應用程式的方式來購買這些產品或功能。

    以下是如何測試以查看客戶是否已經擁有應用程式內產品，如果沒有，就顯示購買對話方塊來讓客戶購買。 請以您為購買對話方塊自訂的程式碼取代 "show the purchase dialog" 註解 (例如提供一個含有親切提醒「購買此應用程式！」 按鈕的頁面)。

    ```    CSharp
    void BuyFeature1() {
            if (!licenseInformation.ProductLicenses["featureName"].IsActive)
            {
                try
                    {
                    // The customer doesn't own this feature, so 
                    // show the purchase dialog.
                                    
                    await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);
                    //Check the license state to determine if the in-app purchase was successful.
                }
                catch (Exception)
                {
                    // The in-app purchase was not completed because 
                    // an error occurred.
                }
            } 
            else
            {
                // The customer already owns this feature.
            }
        }
    ```

## 步驟 3：將測試程式碼變更成最後呼叫

這個步驟很簡單：在您 App 的程式碼中，將 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 的每個參考變更成 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)。 您不再需要提供 WindowsStoreProxy.xml 檔案，因此可以將該檔案從您 App 的路徑中移除 (雖然您可能想要加以儲存，以供在下個步驟中設定應用程式內的購買選項時參考)。

## 步驟 4：在市集中設定應用程式內產品購買選項

在開發人員中心儀表板中，定義應用程式內產品的產品識別碼、類型、價格，以及其他屬性。 請確定在測試時，將它設定為與 WindowsStoreProxy.xml 相同的設定。 如需詳細資訊，請參閱 [IAP 提交](https://msdn.microsoft.com/library/windows/apps/mt148551)。

## 備註

如果您想要為客戶提供消費性 app 內產品選項 (可購買、耗盡，並在想要時再次購買的項目)，請前往[啟用消費性 app 內產品購買](enable-consumable-in-app-product-purchases.md)主題。

如果您需要使用收據來確認使用者已進行 app 內購買，請務必檢閱[使用收據來驗證產品購買](use-receipts-to-verify-product-purchases.md)。

## 相關主題


* [啟用消費性 app 內產品購買](enable-consumable-in-app-product-purchases.md)
* [管理大型的 app 內產品型錄](manage-a-large-catalog-of-in-app-products.md)
* [使用收據來驗證產品購買](use-receipts-to-verify-product-purchases.md)
* [市集範例 (示範試用版和 app 內購買)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
 

 






<!--HONumber=Mar16_HO1-->


