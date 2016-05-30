---
author: DelfCo
Description: '本主題說明如何在企業資料保護 (EFP) 案例中建立網路連線之前，先建立受保護的對話內容。'
MS-HAID: 'dev\_networking.tagging\_network\_connections\_with\_edp\_identity'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 使用 EDP 身份識別標記網路連線
---

# 使用 EDP 身份識別標記網路連線

__注意__：企業資料保護 (EDP) 原則無法套用於 Windows 10 1511 版 (組建 10586) 或更早版本。

本主題說明如何在企業資料保護 (EFP) 案例中建立網路連線之前，先建立受保護的對話內容。 如需 EDP 如何與檔案、串流、剪貼簿、網路、背景工作及鎖定時的資料保護產生關係的完整開發人員說明，請參閱[企業資料保護 (EDP)](../enterprise/edp-hub.md)。

**注意**：[企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵蓋許多 EDP 案例。



## 先決條件


-   **設定 EDP**

    請參閱[設定您的電腦使用 EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力於建置企業開明 app**

    可自主確保企業資料受企業管控的 app，稱之為企業開明 app。 開明應用程式比非開明應用程式的功能更強、更有智慧、更有彈性且更值得信任。 您的應用程式會透過宣告限制的 **enterpriseDataPolicy** 功能，以向系統宣告它是開明應用程式。 但是，比起設定功能，還有更多開明的行為。 若要深入了解，請參閱[企業開明 app](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) app 的非同步程式設計**

    若要了解如何使用 C# 或 Visual Basic 撰寫非同步的應用程式，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 撰寫非同步的應用程式，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 透過網路存取企業資源


在這個案例中，適用的郵件 app 正與一組由企業和個人信箱混合組成的信箱同步。 app 會將使用者身份識別傳遞給呼叫，以 [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) 來建立受保護的對話內容。 這會標記之後在含有該身份識別之相同對話上建立的所有網路連線，並允許存取由企業原則控制存取的企業網路資源。

這裡的「企業」是指使用者身份識別所屬的企業。 [
            **CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) 會傳回不受原則強制執行影響的 [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029) 物件。 一般而言，如果應用程式預期處理混合的資源，它可以選擇針對所有身份識別呼叫 **CreateCurrentThreadNetworkContext**。 在抓取網路資源之後，應用程式會在 **ThreadNetworkContext** 上呼叫 **Dispose** 以清除來自目前對話的任何身份識別標記。 您用於處置內容物件的模式將視您的程式設計語言而定。

如果身份識別不明，應用程式可以使用 [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) API 從資源的網路存取權查詢受企業原則管理的身份識別。

**注意**：就像您在程式碼範例中看到的一樣，[**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) 的正確使用模式是維持最小的效果範圍。 您應該設定企業網路內容、建立網路連線、還原內容，使用連線，然後關閉那些連線。 下列程式碼範例說明詳細資料。 在執行緒上設定企業網路內容時，務必不要在該執行緒上建立檔案。 無論您的目的是否是要讓檔案僅限個人使用，這樣做會導致檔案自動加密。 這是我們建議您儘早還原內容的原因之一。



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```

**注意**：本文適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。



## 相關主題


[企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 命名空間**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=May16_HO2-->


