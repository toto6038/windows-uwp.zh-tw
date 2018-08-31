---
title: MAC、雜湊以及簽章
description: 本文討論如何在通用 Windows 平台 (UWP) app 中使用訊息驗證碼 (MAC)、雜湊及簽章來偵測訊息是否遭竄改。
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: e7b345e520b848a3637a44fa3c3b26172c7afef0
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "3229180"
---
# <a name="macs-hashes-and-signatures"></a>MAC、雜湊以及簽章




本文討論如何在通用 Windows 平台 (UWP) app 中使用訊息驗證碼 (MAC)、雜湊及簽章來偵測訊息是否遭竄改。

## <a name="message-authentication-codes-macs"></a>訊息驗證碼 (MAC)


加密有助於防止未授權的人員讀取訊息，但加密無法防止該人員篡改訊息。 即使竄改不會產生任何不良影響且無意義，被竄改的訊息還是會產生實際成本。 訊息驗證碼 (MAC) 有助於防止訊息遭篡改。 例如，請考慮下列情況：

-   Bob 與 Alice 共用祕密金鑰並同意使用 MAC 函數。
-   Bob 建立訊息，然後將訊息與祕密金鑰輸入 MAC 函數來擷取 MAC 值。
-   Bob 傳送 \[未加密\] 訊息和 MAC 值給網路另一邊的 Alice。
-   Alice 使用祕密金鑰與訊息做為 MAC 函數的輸入。 然後將產生的 MAC 值與 Bob 傳送的 MAC 值相比較。 若結果相同，表示訊息在傳輸時未遭到變更。

請注意，竊聽 Bob 與 Alice 之間交談的第三者 Eve，無法有效操縱該訊息。 Eve 無法存取私密金鑰，因此也無法建立 MAC 值來讓 Alice 以為遭竄改的訊息是合法訊息。

建立訊息驗證碼只可確保原始訊息不被竄改，而且還可透過使用共用的祕密金鑰，確保訊息雜湊是由可存取私密金鑰的人員所簽署。

您可以使用 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 來列舉可用的 MAC 演算法以及產生對稱金鑰。 您可以在 [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 類別使用靜態方法，執行必要的加密來建立 MAC 值。

數位簽章是等同於私密金鑰訊息驗證碼 (MAC) 的公開金鑰。 雖然 MAC 使用私密金鑰讓訊息收件者驗證訊息在傳輸期間未遭竄改，但簽章使用的是私密/公開金鑰組。

下列範例程式碼說明如何使用 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 類別來建立雜湊訊息驗證碼 (HMAC)。

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>雜湊


密碼編譯雜湊函數使用任意長度的資料區塊並傳回固定大小位元字串。 簽章資料時通常是使用雜湊函數。 由於大部分的公開金鑰簽章作業需要大量的運作，因此通常簽署 (加密) 郵件雜湊比簽署原始郵件更有效率。 下列程序代表雖然簡化但卻常用的案例：

-   Bob 與 Alice 共用祕密金鑰並同意使用 MAC 函數。
-   Bob 建立訊息，然後將訊息與祕密金鑰輸入 MAC 函數來擷取 MAC 值。
-   Bob 傳送 \[未加密\] 訊息和 MAC 值給網路另一邊的 Alice。
-   Alice 使用祕密金鑰與訊息做為 MAC 函數的輸入。 然後將產生的 MAC 值與 Bob 傳送的 MAC 值相比較。 若結果相同，表示訊息在傳輸時未遭到變更。

請注意，Alice 傳送的是未加密的郵件。 僅加密了雜湊。 這個程序僅確保原始郵件未經過修改，而且，使用 Alice 的公開金鑰，郵件雜湊會由具有 Alice 私密金鑰存取權的人來簽署，可能就是 Alice 本人。

您可以使用 [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 類別來列舉可用的雜湊演算法及建立 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 值。

數位簽章是等同於私密金鑰訊息驗證碼 (MAC) 的公開金鑰。 相對而言，MAC 使用私密金鑰讓郵件收件者驗證郵件在傳輸期間沒有被修改，而簽章則是使用私密/公開金鑰組。

[**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 物件可以用來重複雜湊不同的資料，而不用在每次使用時都要重新建立物件。 [**Append**](https://msdn.microsoft.com/library/windows/apps/br241499) 方法會將新資料加入要雜湊的緩衝區。 [**GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) 方法會進行資料拼湊並重設物件以供下次使用。 如下列範例所示。

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>數位簽章


數位簽章是等同於私密金鑰訊息驗證碼 (MAC) 的公開金鑰。 相對而言，MAC 使用私密金鑰讓郵件收件者驗證郵件在傳輸期間沒有被修改，而簽章則是使用私密/公開金鑰組。

不過，由於大部分的公開金鑰簽章作業需要大量的運作，因此通常簽署 (加密) 郵件雜湊比簽署原始郵件更有效率。 寄件者建立郵件雜湊，簽署它，然後傳送簽章及 (未加密) 郵件兩者。 收件者透過郵件計算雜湊，解密簽章，然後將解密的簽章與雜湊值進行比較。 如果這兩者相符，收件者即可確定郵件確實是來自寄件者，而且傳輸期間未經過修改。

簽署僅確保原始郵件未經過修改，而且，使用寄件者的公開金鑰，郵件雜湊會由具有私密金鑰存取權的人來簽署。

您可以使用 [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 物件來列舉可用的簽章演算法及產生或匯入金鑰組。 您可以在 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 類別上使用靜態方法來簽署郵件或驗證簽章。