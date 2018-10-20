---
title: 密碼編譯
description: 本文提供通用 Windows 平台 (UWP) 應用程式所提供的密碼編譯功能概觀。 如需特定工作的詳細資訊，請參閱本文結尾的表格。
ms.assetid: 9C213036-47FD-4AA4-99E0-84006BE63F47
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: aa01cc3d70db7a94667e944d1a1739e911f94b0c
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "5157469"
---
# <a name="cryptography"></a>密碼編譯




本文提供通用 Windows 平台 (UWP) 應用程式所提供的密碼編譯功能概觀。 如需特定工作的詳細資訊，請參閱本文結尾的表格。

## <a name="terminology"></a>詞彙


下列為密碼編譯和公開金鑰基礎結構 (PKI) 中常用的詞彙。

| 詞彙                        | 說明                                                                                                                                                                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 加密                  | 使用密碼編譯演算法和金鑰來轉換資料的程序。 轉換的資料只能使用相同的演算法和相同 (對稱) 或相關 (公開) 金鑰來還原。 |
| 解密                  | 將加密資料還原為原始形式的程序。                                                                                                                                         |
| 純文字                   | 原本是指未加密的文字訊息。 現在則是指任何未加密的資料。                                                                                                         |
| 密碼文字                  | 原本是指已加密因而無法讀取的文字訊息。 現在則是指任何加密的資料。                                                                                  |
| 雜湊                     | 將可變長度資料轉換成固定長度 (值一般比較小) 的程序。 透過比較雜湊，您可以合理地相信二個或多個資料是相同的。            |
| 簽章                   | 數位資料的加密雜湊，通常被用來驗證資料傳送者或確定資料在傳送時未被竄改。                                               |
| 演算法                   | 加密資料的逐步程序。                                                                                                                                                         |
| 按鍵                         | 隨機或虛擬隨機的數字，可用來做為密碼編譯演算法的輸入項目，以加密和解密資料。                                                                                               |
| 對稱金鑰密碼編譯  | 一種密碼編譯方法，使用相同的金鑰進行加密和解密。 這也稱為祕密金鑰密碼編譯。                                                                                      |
| 非對稱金鑰密碼編譯 | 一種密碼編譯方法，使用不同但數學上相關的金鑰進行加密和解密。 這也稱為公開金鑰密碼編譯。                                                          |
| 編碼                    | 為數位訊息 (包括憑證) 進行編碼以便在網路上傳輸的程序。                                                                                                     |
| 演算法提供者          | 實作密碼編譯演算法的 DLL。                                                                                                                                                      |
| 金鑰儲存提供者        | 儲存金鑰資料的容器。 金鑰目前可以儲存於軟體、智慧卡或信賴平台模組 (TPM)。                                                                   |
| X.509 憑證           | 一種數位文件，通常由憑證授權單位發出，用於向其他相關各方來驗證個人、系統或實體的身分。                                            |

 
## <a name="namespaces"></a>命名空間

下列是應用程式中可供使用的命名空間。

### <a name="windowssecuritycryptography"></a>Windows.Security.Cryptography

包含 CryptographicBuffer 類別和靜態方法，讓您能夠：

-   將資料轉換為字串和從字串轉換
-   將資料轉換為位元組陣列和從位元組陣列轉換
-   加密訊息以進行網路傳輸
-   傳輸之後將訊息解密

### <a name="windowssecuritycryptographycertificates"></a>Windows.Security.Cryptography.Certificates

包含類別、介面及列舉類型，讓您能夠：

-   建立憑證要求
-   安裝憑證回應
-   匯入 PFX 檔案中的憑證
-   指定和抓取憑證要求屬性

### <a name="windowssecuritycryptographycore"></a>Windows.Security.Cryptography.Core

包含類別和列舉類型，讓您能夠：

-   加密和解密資料
-   進行資料拼湊
-   簽署資料並驗證簽章
-   建立、匯入和匯出金鑰
-   使用非對稱金鑰演算法提供者
-   使用對稱金鑰演算法提供者
-   使用雜湊演算法提供者
-   使用電腦驗證代碼 (MAC) 演算法提供者
-   使用金鑰衍生演算法提供者

### <a name="windowssecuritycryptographydataprotection"></a>Windows.Security.Cryptography.DataProtection

包含類別，讓您能夠：

-   非同步加密和解密靜態資料
-   非同步加密和解密資料流

## <a name="crypto-and-pki-application-capabilities"></a>密碼編譯與 PKI 應用程式功能


適用於應用程式的簡化應用程式程式設計介面會啟用下列密碼編譯和公開金鑰基礎結構 (PKI) 功能：

### <a name="cryptography-support"></a>密碼編譯支援

您可以執行下列密碼編譯工作。 如需詳細資訊，請參閱 [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547) 命名空間。

-   建立對稱金鑰
-   執行對稱式加密
-   建立非對稱金鑰
-   執行非對稱式加密
-   衍生密碼式金鑰
-   建立訊息驗證碼 (MAC)
-   雜湊內容
-   數位簽署內容

SDK 也針對以密碼為基礎的資料保護提供簡化的介面。 您可以用它來執行下列工作。 如需詳細資訊，請參閱 [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空間。

-   靜態資料的非同步保護
-   資料流的非同步保護

### <a name="encoding-support"></a>編碼支援

應用程式可以編碼密碼編譯的資料，以便在網路上傳送，也可以解碼從網路來源收到的資料。 如需詳細資訊，請參閱 [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) 命名空間可用的靜態方法。

### <a name="pki-support"></a>PKI 支援

應用程式可以執行下列 PKI 工作。 如需詳細資訊，請參閱 [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476) 命名空間。

-   建立憑證
-   建立自我簽署憑證
-   安裝憑證回應
-   以 PFX 格式匯入憑證
-   使用智慧卡憑證和金鑰 (sharedUserCertificates 功能組)
-   使用來自使用者 MY 存放區的憑證 (sharedUserCertificates 功能組)

此外，您可以使用資訊清單來執行下列動作：

-   指定每個應用程式信任的根憑證
-   指定每個應用程式對等信任的憑證
-   明確停用來自系統信任的繼承
-   指定憑證選取準則
    -   僅硬體憑證
    -   透過一組指定簽發者鏈結的憑證
    -   從 app 存放區自動選取憑證

## <a name="detailed-articles"></a>詳細說明的文章


下列文章提供有關安全性案例的詳細資料：

| 主題                                                                         | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [憑證](certificates.md)                                               | 本文討論 UWP app 中的憑證使用方式。 數位憑證用於公開金鑰密碼編譯，將公開金鑰繫結至個人、電腦或組織。 這種繫結身分常被用來在實體之間互相驗證。 例如，憑證通常是用來向使用者驗證網頁伺服器，或是向網頁伺服器驗證使用者。 您可以建立憑證要求並安裝或匯入已發出的憑證。 您也可以在憑證階層中註冊憑證。 |
| [密碼編譯金鑰](cryptographic-keys.md)                                   | 本文說明如何使用標準金鑰衍生函式來衍生金鑰，以及如何使用對稱和非對稱金鑰來加密內容。                                                                                                                                                                                                                                                                                                                                                                             |
| [資料保護](data-protection.md)                                         | 本文說明如何使用 [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空間中的 [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) 類別，來加密和解密 UWP app 中的數位資料。                                                                                                                                                                                                                  |
| [MAC、雜湊以及簽章](macs-hashes-and-signatures.md)               | 本文討論如何在 UWP app 中使用訊息驗證碼 (MAC)、雜湊及簽章來偵測訊息是否遭竄改。                                                                                                                                                                                                                                                                                                                                                                                |
| [密碼編譯的出口限制](export-restrictions-on-cryptography.md) | 使用這項資訊判斷您的 app 使用密碼編譯的方式，是否會使它無法被刊登於 Microsoft Store 中。                                                                                                                                                                                                                                                                                                                                                                                            |
| [常見的密碼編譯工作](common-cryptography-tasks.md)                     | 下列文章提供常見的 UWP 密碼編譯工作範例程式碼，例如建立隨機數字、比較緩衝區、在字串與二進位資料間轉換、複製到位元組陣列和從位元組陣列中複製，以及編碼和解碼資料。                                                                                                                                                                                                                                                                                    |

 