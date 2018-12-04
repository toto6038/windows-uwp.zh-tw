---
title: 密碼編譯金鑰
description: 本文說明如何使用標準金鑰衍生函式來衍生金鑰，以及如何使用對稱和非對稱金鑰來加密內容。
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 2b74eccd5f6138e5a9d670aa3a0a93239813cf4d
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8475267"
---
# <a name="cryptographic-keys"></a>密碼編譯金鑰




本文說明如何使用標準金鑰衍生函式來衍生金鑰，以及如何使用對稱和非對稱金鑰來加密內容。 

## <a name="symmetric-keys"></a>對稱金鑰


對稱金鑰加密又稱為祕密金鑰加密，在加密和解密時需要使用同一個金鑰。 您可以使用 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 類別來指定對稱演算法以及建立或匯入金鑰。 您可以透過使用演算法和金鑰，在 [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 類別上使用靜態方法來加密和解密資料。

對稱金鑰加密通常會使用區塊密碼和區塊密碼模式。 區塊密碼是一種對稱式加密功能，可以在固定大小的區塊上操作。 如果您想要加密的訊息長度大於區塊長度，則必須使用區塊密碼模式。 區塊密碼模式是使用區塊密碼建置的對稱式加密功能。 它會將純文字加密成一連串固定大小的區塊。 下列是應用程式支援的模式：

-   ECB (電子密碼書) 模式會獨立加密訊息的每一個區塊。 這不被視為安全的加密模式。
-   CBC (密碼區塊鏈結) 模式使用先前的密碼文字區塊來模糊處理目前的區塊。 您必須判斷要在第一個區塊使用的值。 這個值稱為初始化向量 (IV)。
-   CCM (CBC-MAC 計數器) 模式會結合 CBC 區塊密碼模式和訊息驗證碼 (MAC)。
-   GCM (Galois 計數器模式) 模式會結合計數器加密模式和 Galois 驗證模式。

一些像是 CBC 的模式會要求您針對第一個加密文字區塊使用初始化向量 (IV)。 以下為常見的初始化向量。 呼叫 [**CryptographicEngine.Encrypt**](https://msdn.microsoft.com/library/windows/apps/br241494) 時，您可以指定 IV。 在大部分情況下，絕對不能使用相同金鑰重複使用 IV，這點很重要。

-   「固定」針對要加密的所有訊息使用相同的 IV。 但這樣會造成資訊外洩，因此不建議使用。
-   「計數器」會讓每個區塊的 IV 遞增。
-   「隨機」會建立虛擬隨機的 IV。 您可以使用 [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392) 來建立 IV。
-   「Nonce 產生」會針對每個要加密的訊息使用唯一數字。 Nonce 通常是一個修改過的訊息或者交易識別碼。 Nonce 不需要保密，不過在相同金鑰之下切勿重複使用它。

多數模式都會規定純文字長度必須是區塊大小的整數倍數。 通常您需要填補其他純文字才能取得適當的長度。

區塊密碼可以加密固定大小的資料區塊，而串流密碼是對稱加密功能，結合純文字位元和虛擬隨機位元串流 (稱為金鑰串流) 來產生加密文字。 部分像是輸出回饋模式 (OTF) 以及計數器模式 (CTR) 的區塊密碼模式，可以有效地將區塊密碼轉換成串流密碼。 不過，像 RC4 這樣的實際串流密碼，它的運作通常可以達到比區塊密碼模式更高的速度。

下列範例示範如何使用 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 類別來建立對稱金鑰，並使用它來加密和解密資料。

## <a name="asymmetric-keys"></a>非對稱金鑰


非對稱金鑰密碼編譯，也稱為公開金鑰密碼編譯，使用一個公開金鑰和一個私密金鑰來執行加密和解密。 兩個金鑰是不同的，但是使用相關的演算方式。 一般會保密私密金鑰，並用來解密資料，而公開金鑰則會發佈給相關的使用者並用來解密資料。 非對稱式密碼編譯對於簽署資料也很有用。

因為非對稱式密碼編譯比對稱式密碼編譯慢很多，所以很少使用它來直接加密大量資料。 通常是改用下列方式來加密金鑰。

-   Alice 要求 Bob 只傳送加密的電子郵件給她。
-   Alice 建立一個私密/公開金鑰組，保密她的私密金鑰並發佈她的公開金鑰。
-   Bob 有一封電子郵件要傳送給 Alice。
-   Bob 建立一個對稱金鑰。
-   Bob 使用這個新的對稱金鑰來加密要傳送給 Alice 的電子郵件。
-   Bob 使用 Alice 的公開金鑰來加密他的對稱金鑰。
-   Bob 將加密的電子郵件及經過加密的對稱金鑰傳送給 Alice (以封住的方式)。
-   Alice 使用她的私密金鑰 (從私密/公開金鑰組) 來解密 Bob 的對稱金鑰。
-   Alice 使用 Bob 的對稱金鑰來解密電子郵件。

您可以使用 [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 物件指定非對稱式演算法或簽章演算法，以建立或匯入一個暫時金鑰組，或是匯入金鑰組的公開金鑰部分。

## <a name="deriving-keys"></a>衍生金鑰


您通常需要從共用密碼來衍生其他金鑰。 您可以使用 [**KeyDerivationAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241518) 類別，以及 [**KeyDerivationParameters**](https://msdn.microsoft.com/library/windows/apps/br241524) 類別中下列專用方法的其中一種來衍生金鑰。

| 物件                                                                            | 說明                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](https://msdn.microsoft.com/library/windows/apps/br241525)    | 建立用於密碼型金鑰衍生函數 2 (PBKDF2) 的 KeyDerivationParameters 物件。                                 |
| [**BuildForSP800108**](https://msdn.microsoft.com/library/windows/apps/br241526)  | 建立用於計數器模式、雜湊訊息驗證碼 (HMAC) 金鑰衍生函數的 KeyDerivationParameters 物件。 |
| [**BuildForSP80056a**](https://msdn.microsoft.com/library/windows/apps/br241527)  | 建立用於 SP800-56A 金鑰衍生函數的 KeyDerivationParameters 物件。                                                 |

 
