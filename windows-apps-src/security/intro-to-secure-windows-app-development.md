---
title: 安全開發 Windows app 的簡介
description: 這篇簡介文章將協助應用程式設計人員和開發人員，能更了解各種能快速建立安全的通用 Windows 平台 (UWP) 應用程式的 Windows 10 平台功能。
ms.assetid: 6AFF9D09-77C2-4811-BB1A-BBF4A6FF511E
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 1e48d0b21d588ef7b4913e16b75f9d21c5d5209f
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "4019306"
---
# <a name="intro-to-secure-windows-app-development"></a>安全開發 Windows 應用程式的簡介




這篇簡介文章將協助應用程式設計人員和開發人員，能更了解各種能快速建立安全的通用 Windows 平台 (UWP) 應用程式的 Windows 10 平台功能。 文章會詳細說明如何在每個階段 (驗證、傳輸中資料及靜態資料) 中使用可用的 Windows 安全性功能。 如需深入了解每個主題，請檢視包含在每個章節中的額外資源。

## <a name="1-introduction"></a>1 簡介


開發安全的應用程式是種挑戰。 今日的世界充滿各種行動、社交、雲端，及複雜的企業應用程式，而且瞬息萬變，使得客戶期待應用程式會有更快的上市及更新速度。 而客戶也會使用許多不同類型的裝置，更增添了應用程式開發的複雜性。 如果您要建置適用於 Windows 10 通用 Windows 平台 (UWP) 的應用程式，這些裝置可能包含傳統的桌上型電腦、筆記型電腦、平板電腦及行動裝置，再加上種類日益增加的新裝置，包括物聯網、Xbox One、Microsoft Surface Hub 及 HoloLens。 身為開發人員，您必須確保自己的應用程式能在所有平台或相關裝置之間安全地傳輸及儲存資料。

以下是幾個利用 Windows 10 安全性功能的好處。

-   您在所有支援 Windows 10 的裝置上將擁有標準化的安全性，方法是使用一致的安全性元件和技術 API。
-   如果您之前使用自訂的程式碼來處理這些安全性案例，現在您需要編寫、測試及維護的程式碼將會減少。
-   您的應用程式會更加穩定、安全，原因是您將使用作業系統來控制應用程式存取自己的資源，以及本機或遠端系統資源的方式。

在驗證期間，系統會驗證要求存取特定服務的使用者身分識別。 Windows Hello 是 Windows 10 的元件，可協助您在 Windows 應用程式中建立更安全的驗證機制。 您可以藉由這個元件來使用個人識別碼 (PIN) 或生物特徵辨識技術 (例如使用者的指紋、臉部或虹膜)，以便為您的應用程式實行多重要素驗證。

傳輸中資料就是指連線，以及透過該連線傳輸的訊息。 舉例來說，當您使用 Web 服務來擷取遠端伺服器上的資料時， 使用安全通訊端層 (SSL) 和安全超文字傳輸通訊協定 (HTTPS) 可確保連線的安全。 避免第三方存取這些訊息，或是避免未經授權的應用程式使用 Web 服務進行通訊，是保護傳輸中資料的重要關鍵。

最後，靜態資料與存放在記憶體中或儲存媒體上的資料有關。 Windows 10 有種應用程式模型，能防止應用程式之間未經授權的資料存取，以及提供加密 API 來進一步保護裝置上的資料。 而稱為「認證保險箱」的功能在搭配可防止應用程式存取裝置上資訊的作業系統之後，就能用來將使用者認證安全地儲存在裝置上。

## <a name="2-authentication-factors"></a>2 驗證因素


為了要保護資料，要求存取資料的使用者必須要經過身分識別，並獲得授權來存取該使用者要求的資料。 識別使用者身分的程序稱為「驗證」，而決定給予某資源存取權限的過程則叫「授權」。 這兩者是關係密切的作業，且對於使用者來說可能難以分辨。 它們可以是相當簡單或複雜的作業，完全取決於許多因素：例如，資料是存放在單一伺服器上，還是分散到許多系統中。 提供驗證及授權服務的伺服器稱為「識別提供者」。

使用者為了要向特定的服務和/或應用程式驗證自己的身分，會使用由您知道的事情、您擁有的東西，及/或您扮演的角色來組成的認證。 而這些資訊每個都叫做驗證因素。

-   **使用者知道的資訊**通常是密碼，但也可以是個人識別碼 (PIN) 或「祕密」的問題與答案組。
-   **使用者擁有的物品**大多數是硬體記憶體裝置，例如內含使用者專屬驗證資料的 USB 隨身碟。
-   **使用者本身的特質**通常包含使用者的指紋，但有些因素越來越受歡迎，例如使用者的語音、臉部、眼睛特徵，或是行為模式。 當這些因素的度量儲存成資料之後，就稱為生物特徵辨識技術。

使用者建立的密碼本身就是驗證因素，但這通常不夠，因為任何知道密碼的人，都能冒充擁有該密碼的使用者。 智慧卡能提供更高層級的安全性，但是它可能會遭竊、遺失，或放錯地方。 能利用指紋或眼部掃描來驗證使用者身分的系統，或許可提供最高層級和最便利的安全性，但這需要昂貴和專用的硬體 (例如，臉部辨識用的 Intel RealSense 攝影機)，並非所有使用者都能負擔。

設計系統所使用的驗證方法，對於資料安全性來說是個複雜且重要的部分。 一般而言，您使用於驗證的因素越多，系統就越安全。 然而，驗證必須是便於使用的。 使用者通常會一天登入系統許多次，因此驗證程序必須快速。 您選擇的驗證類型是安全性和易於使用性之間的取捨；單一因素驗證最不安全但最易於使用，而多重要素驗證會隨因素的增加而更加安全，但也更加複雜。

## <a name="21-single-factor-authentication"></a>2.1 單一因素驗證


這種形式的驗證是根據單一使用者認證來進行的。 這認證通常是密碼，但也可能是個人識別碼 (PIN)。

以下是單一因素驗證的程序。

-   使用者將自己的使用者名稱和密碼提供給識別提供者。 識別提供者就是驗證使用者的身分識別的伺服器程序。
-   識別提供者會檢查該使用者名稱及密碼，是否與儲存在系統中的使用者名稱及密碼相同。 在大多數的情況下，密碼會經過加密來提供額外的安全性，讓他人無法讀取。
-   識別提供者會傳回指出驗證是否成功的驗證狀態。
-   如果成功，就會開始資料交換。 如果不成功，使用者就必須重新驗證身分。

![單一因素驗證](images/secure-sfa.png)

這種驗證方法是目前各種服務最常使用的。 但在只用這種方法來驗證時，它也是最不安全的驗證形式。 密碼的複雜性需求、「秘密問題」，以及定期的密碼變更都能讓密碼更安全，但這會讓使用者承受比較大的負擔，且使用者並非能有效對抗駭客的對象。

使用密碼的挑戰在於，成功猜出密碼，比入侵擁有不只一個因素的系統還要容易。 如果駭客向某家網路小店購買內含使用者帳戶及雜湊密碼的資料庫，他們就能在其他網站上使用這些密碼。 使用者通常會一直重複使用相同的帳戶，因為複雜的密碼是很難記住的。 對於 IT 部門來說，管理密碼也有其複雜性，像是必須提供密碼重設機制、需要經常更新密碼，以及必須將密碼儲存在安全的方法。

儘管單一因素驗證有許多缺點，但它給予使用者認證的控制權。 使用者可以建立及修改密碼，而且只要鍵盤就能進行驗證程序。 這是單一因素驗證和多重要素驗證的主要差異。

## <a name="211-web-authentication-broker"></a>2.1.1 Web 驗證代理人


如同先前所討論的，對於 IT 部門來說，使用密碼驗證的挑戰之一，在於管理使用者名稱/密碼資料庫、重設機制等等的經常費用提高了。目前有種越來越受歡迎的選擇，就是仰賴第三方識別提供者來提供藉由 OAuth (開放的驗證標準) 驗證的程序。

IT 部門只要使用 OAuth，就能有效地把維護使用者名稱及密碼資料庫、密碼重設功能等複雜工作「外包」給第三方識別提供者，例如 Facebook、Twitter 或 Microsoft。

使用者在這些平台上可以完全掌控自己的身分識別，但在使用者經過驗證，且在該使用者的同意下，應用程式可要求識別提供者提供權杖，用來授權給已經過驗證的使用者。

Windows 10 的 Web 驗證代理人提供一組 API 和基礎結構，讓應用程式來使用驗證及授權通訊協定 (例如 OAuth 和 OpenID)。 應用程式可透過 [**WebAuthenticationBroker**](https://msdn.microsoft.com/library/windows/apps/br227025) API 來啟動驗證作業，以傳回 [**WebAuthenticationResult**](https://msdn.microsoft.com/library/windows/apps/br227038)。 下圖為通訊流程的概觀。

![WAB 工作流程](images/secure-wab.png)

app 會擔任代理人，透過 app 中的 [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 向識別提供者啟動驗證作業。 識別提供者在驗證使用者的身分之後，會將權杖傳回給應用程式，讓應用程式能用來向識別提供者要求該使用者的相關資訊。 基於安全性考量，應用程式必須先向識別提供者註冊，才可以向識別提供者進行代理驗證程序。 每個識別提供者的註冊步驟都不相同。

以下是呼叫 [**WebAuthenticationBroker**](https://msdn.microsoft.com/library/windows/apps/br227025) API 來與識別提供者進行通訊的一般工作流程。

-   建立要傳送給識別提供者的要求字串。 每個 Web 服務的字串數目和每個字串中的資訊都不相同，但它通常包含 2 個 URI 字串，且每個字串都有 1 個 URL：一個是驗證要求的傳送目的地，另一個是授權完成之後使用者將被重新導向的目的地。
-   呼叫會利用要求字串傳遞的 [**WebAuthenticationBroker.AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066)，並等待識別提供者的回應。
-   收到回應時，呼叫 [**WebAuthenticationResult.ResponseStatus**](https://msdn.microsoft.com/library/windows/apps/br227041) 以取得狀態。
-   如果通訊成功，則處理識別提供者傳回的回應字串。 如果失敗，則處理錯誤。

如果通訊成功，則處理識別提供者傳回的回應字串。 如果失敗，則處理錯誤。

下列為這個處理程序的 C# 程式碼範例。 如需資訊和詳細的逐步解說，請參閱 [WebAuthenticationBroker](web-authentication-broker.md)。 如需完整的程式碼範例，請查看 [GitHub 上的 WebAuthenticationBroker 範例](http://go.microsoft.com/fwlink/p/?LinkId=620622)。

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>";
string endURL = "http://<AppEndPoint>";

var startURI = new System.Uri(startURL);
var endURI = new System.Uri(endURL);

try
{
    WebAuthenticationResult webAuthenticationResult = 
        await WebAuthenticationBroker.AuthenticateAsync( 
            WebAuthenticationOptions.None, startURI, endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case WebAuthenticationStatus.Success:
            // Successful authentication. 
            break;
        case WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            break;
        default:
            // Other error.
        break;
    }
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and
    // Network Unavailable errors here. 
}
```

## <a name="22-multi-factor-authentication"></a>2.2 多重要素驗證


多重要素驗證使用一個以上的驗證因素， 通常是「您知道的事情」(例如密碼) 與「您擁有的東西」(例如行動電話或智慧卡) 的組合。 就算攻擊者發現使用者的密碼，在沒有裝置或智慧卡的情況下，攻擊者仍然無法存取帳戶。 而且如果攻擊者取得裝置或智慧卡，在沒有密碼的情況下，該裝置或智慧卡也沒有任何用處。 因此，多重要素驗證比單一因素驗證更加安全，但也更加複雜。

使用多重要素驗證的服務，通常會讓使用者選擇如何讓服務接收第二個認證。 舉例來說，使用這種驗證類型的服務，通常會利用簡訊把驗證碼傳送到使用者的行動電話中。

-   使用者將自己的使用者名稱和密碼提供給識別提供者。
-   識別提供者會驗證使用者名稱和密碼 (就跟單一因素驗證的程序一樣)，然後尋找儲存在系統中的使用者行動電話號碼。
-   伺服器會將內含系統所產生驗證碼的簡訊傳送到使用者的行動電話。
-   使用者透過系統提供給使用者的表單，將驗證碼提供給識別提供者。
-   伺服器會傳回驗證狀態，指出這兩種認證是否都已驗證成功。
-   如果成功，就會開始資料交換。 如果不成功，使用者就必須重新驗證。

![雙因素驗證](images/secure-tfa.png)

如您所見，這個程序也與單一因素驗證不同，因為第二個使用者認證是傳送給使用者，而不是由使用者建立或提供。 因此使用者對於必要的認證並沒有完全的控制權。 把智慧卡當做第二個認證時亦是如此：是由組織負責製作智慧卡，然後提供給使用者。

## <a name="221-azure-active-directory"></a>2.2.1 Azure Active Directory


Azure Active Directory (Azure AD) 是雲端式身分識別及存取管理服務，可做為單一因素驗證或多重要素驗證的識別提供者。 Azure AD 驗證可以單獨使用，或是搭配驗證碼來使用。

雖然 Azure AD 也可以實行單一因素驗證，但企業通常需要安全性更高的多重要素驗證。 在多重要素驗證的設定中，利用 Azure AD 帳戶來驗證的使用者，可以選擇把驗證碼當做簡訊傳送到自己的行動電話，或是傳送到 Azure Authenticator 行動應用程式。

此外，Azure AD 可以做為 OAuth 提供者，為標準使用者提供跨多重平台之應用程式的驗證及授權機制。 如需深入了解，請參閱 [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 和 [在 Azure 的多重要素驗證](https://azure.microsoft.com/services/multi-factor-authentication/)。

## <a name="24-windows-hello"></a>2.4 Windows Hello


在 Windows 10 中，方便的多重要素驗證機制已經內建在作業系統中。 Windows Hello 就是 Windows 10 內建的新生物特徵辨識登入系統。 由於 Windows Hello 是直接內建在作業系統中，因此能夠讓使用者利用臉部或指紋辨識來解除使用者裝置的鎖定。 Windows 安全的認證存放區會保護裝置上的生物特徵辨識資料。

Windows Hello 可為裝置提供健全的方式來辨識個別使用者，這也就是使用者與所要求服務或資料項目之間路徑的第一個部分。 裝置在成功辨識使用者之後，仍然需要驗證使用者的身分，才能決定是否要授與所要求資源的存取權。 Windows Hello 也會提供與 Windows 完全整合的增強式雙因素驗證 (2FA)，利用特定裝置和生物特徵辨識技術或 PIN 的組合，來取代可重複使用的密碼。 PIN 是使用者在註冊 Microsoft 帳戶時所指定的。

不過，Windows Hello 不只是用來取代傳統的 2FA 系統。 這在概念上類似智慧卡：驗證是使用密碼編譯基本類型而不是字串比較來執行，而使用者的金鑰內容在防竄改的硬體內部是安全的。 Microsoft Hello 也不需要智慧卡部署所需的額外基礎結構元件。 特別的是，您並不需要公開金鑰基礎結構 (PKI) 來管理憑證 (如果您目前沒有 PKI)。 Windows Hello 結合智慧卡的主要優點，有虛擬智慧卡的部署彈性及實體智慧卡的強大安全性，卻沒有兩者的任何缺點。

使用者必須先讓裝置向 Windows Hello 註冊，才能使用該裝置進行驗證。 Windows Hello 會使用非對稱式 (公開/私密金鑰) 加密，讓其中一方使用公開金鑰來為資料加密，而另一方使用私密金鑰來解密。 就 Windows Hello 來說，它會建立公開/私密金鑰組，並將私密金鑰寫入裝置的信賴平台模組 (TPM) 晶片中。 裝置註冊之後，UWP app 就能呼叫系統 API 來擷取使用者的公開金鑰，讓系統能用來讓使用者在伺服器上註冊。

應用程式的註冊工作流程可能如下：

![Windows Hello 註冊](images/secure-passport.png)

您所收集註冊資訊中的身分辨識資訊，可能比這簡單案例中所收集的多出許多。 舉例來說，如果您的應用程式存取某個受保護的服務 (例如銀行服務)，您必須要求身分識別證明及其他項目，做為註冊程序的一部分。 當所有條件都符合之後，這位使用者的公開金鑰將會儲存在後端，讓該使用者下次使用服務時可用來進行驗證。

如需 Windows Hello 的詳細資訊，請參閱 [Windows Hello 指南](https://msdn.microsoft.com/library/mt589441)和[Windows Hello 開發人員指南](microsoft-passport.md)。

## <a name="3-data-in-flight-security-methods"></a>3 傳輸中資料的安全性方法


傳輸中資料的安全性方法，適用於連線到網路的裝置之間傳輸的資料。 資料可能是在私人公司內部網路的高安全性環境中的不同系統之間傳輸，或是在網路上的非安全環境中的用戶端和 Web 服務之間傳輸。 Windows 10 應用程式透過自己的網路 API 支援 SSL 等標準，並搭配 Azure API Management 等技術來運作，讓開發人員可確保自己的應用程式有適當的安全性層級。

## <a name="31-remote-system-authentication"></a>3.1 遠端系統驗證


一般而言，會與遠端電腦系統進行通訊的案例有兩種。

-   本機伺服器透過直接連線來驗證使用者。 例如，當伺服器和使用者都在公司的內部網路中時。
-   Web 服務透過網際網路進行通訊。

Web 服務通訊的安全性需求，會比在直接連線案例中的需求還要高，因為資料不再是安全網路的一部分，且惡意攻擊者想要攔截資料的可能性也比較高。 由於會有各種類型的裝置存取服務，這些服務可能會建置為 RESTful 服務 (而非 WCF 等服務)，這代表服務的驗證與授權也有了新的挑戰。 我們將討論兩種安全的遠端系統通訊的需求。

第一種需求是訊息機密性：用戶端與 Web 服務之間傳遞的資訊 (例如使用者的身分識別及其他個人資訊)，在傳輸過程中不得被第三方讀取。 這通常是透過為傳送訊息所用的連線加密，以及為訊息本身加密來達成。 在私密/公開金鑰加密方面，公開金鑰是所有人都能使用的，且會用來為要傳送給特定收件者的訊息加密。 而私密金鑰是只有收件者才能擁有的，且會用來為訊息解密。

第二種需求是訊息完整性：用戶端與 Web 服務必須能確認自己收到的訊息是由另一方有意傳送的，且訊息在傳輸過程中沒有遭到竄改。 這可透過以數位簽章簽署訊息，以及使用憑證驗證來達成。

## <a name="32-ssl-connections"></a>3.2 SSL 連線


Web 服務為了要建立及維護與用戶端之間的安全連線，可以使用安全超文字傳輸通訊協定 (HTTPS) 所支援的安全通訊端層 (SSL)。 SSL 藉由支援公開金鑰加密和伺服器憑證，來提供訊息機密性和完整性。 傳輸層安全性 (TLS) 已取代 SSL，但 TLS 通常還是會被稱為 SSL。

當用戶端要求存取伺服器上的資源時，SSL 會啟動與伺服器之間的交涉程序， 這就稱為 SSL 信號交換。 而加密層級、公開和私密金鑰組，以及用戶端與伺服器憑證中的身分識別資訊，是雙方同意在 SSL 連線期間所有通訊的基礎。 伺服器也可能會在這個階段要求用戶端進行驗證。 連線建立之後，所有訊息都會使用交涉的公開金鑰加密，直到連線關閉為止。

## <a name="321-ssl-pinning"></a>3.2.1 SSL 關聯


雖然 SSL 可使用加密和憑證來提供訊息機密性，卻完全不會檢查正在與用戶端進行通訊的伺服器，是否為正確的伺服器。 未經授權的第三方可能會模仿伺服器的行為，進而攔截用戶端所傳輸的機密資料。 為避免發生這種情況，用戶端會使用稱為 SSL 關聯的技術，來確認伺服器上的憑證是否為用戶端預期和信任的憑證。

在應用程式中使用 SSL 關聯的方式有幾種，但每種都各有利弊。 最簡單的方式，就是透過應用程式套件資訊清單中的憑證宣告。 這個宣告可讓應用程式套件安裝數位憑證，並指定對這些憑證的專有信任。 這會讓應用程式只會與憑證鏈結中有對應憑證的伺服器建立 SSL 連線。 這種機制也啟用安全的自我簽署憑證，因為並不需要依靠第三方來擔任受信任的公開憑證授權單位。

![SSL 資訊清單](images/secure-ssl-manifest.png)

如需提高對驗證邏輯的控制，您可以使用 API 來驗證伺服器為回應 HTTPS 要求所傳回的憑證。 請注意，這個方法需要傳送要求及檢查回應，因此請務必在實際傳送要求中的機密資訊之前，先新增這個驗證方法。

下列 C# 程式碼說明 SSL 關聯的方法。 **ValidateSSLRoot** 方法會使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別來執行 HTTP 要求。 在用戶端傳送回應後，它會使用 [**RequestMessage.TransportInformation.ServerIntermediateCertificates**](https://msdn.microsoft.com/library/windows/apps/dn279681) 集合來檢查伺服器傳回的憑證。 然後，用戶端可以使用憑證鏈結中的指紋，來驗證整個憑證鏈結。 這個方法不會在伺服器憑證過期及更新時，要求更新應用程式中的憑證指紋。

```cs
private async Task ValidateSSLRoot()
{
    // Send a get request to Bing
    var httpClient = new HttpClient();
    var bingUri = new Uri("https://www.bing.com");
    HttpResponseMessage response = 
        await httpClient.GetAsync(bingUri);

    // Get the list of certificates that were used to
    // validate the server's identity
    IReadOnlyList<Certificate> serverCertificates = response.RequestMessage.TransportInformation.ServerIntermediateCertificates;
  
    // Perform validation
    if (!ValidateCertificates(serverCertificates))
    {
        // Close connection as chain is not valid
        return;
    }
    // Validation passed, continue with connection to service
}

private bool ValidateCertificates(IReadOnlyList<Certificate> certs)
{
    // In this example, we iterate through the certificates
    // and check that the chain contains
    // one specific certificate we are expecting
    foreach (var cert in certs)
    {
        byte[] thumbprint = cert.GetHashValue();

        // Check if the thumbprint matches whatever you 
        // are expecting
        var expected = new byte[] { 212, 222, 32, 208, 94, 102, 
            252, 83, 254, 26, 80, 136, 44, 120, 219, 40, 82, 202, 
            228, 116 };

        // ThumbprintMatches does the byte[] comparison 
        if (ThumbprintMatches(thumbprint, expected))
        {
            return true;
        }
    }
    return false;
}
```

## <a name="33-publishing-and-securing-access-to-rest-apis"></a>3.3 發佈 REST API 和保護 REST API 的存取


為確保 Web 服務的存取已經過授權，服務必須在每次 API 呼叫時要求驗證。 當 Web 服務在網路上公開時，能夠控制效能及範圍也是值得考量的地方。 Azure API Management 是能協助您在網路上公開 API 的服務，它依照三個層級來提供功能。

API 的**發行者/系統管理員**可輕鬆地透過 Azure API Management 的發行者入口網站來設定 API。 他們可以在這裡建立 API 組，以及管理哪些人有哪些 API 的存取權。

想存取這些 API 的**開發人員**，可以透過開發人員入口網站提出要求，入口網站可能會立刻提供存取權，或是要求發行者/系統管理員來核准。 開發人員也可以在開發人員入口網站檢視 API 文件和範例程式碼，以便能迅速開始使用 Web 服務所提供的 API。

這些開發人員所建立，然後透過 Azure API Management 提供的 Proxy 來存取 API 的 **app**。 Proxy 提供一層保護，隱藏 API 在發行者/系統管理員的伺服器上實際的端點，它同時也包含額外的邏輯 (例如 API 轉譯)，來確保已公開的 API 在針對某個 API 的呼叫被重新導向到另一個 API 時，能夠保持一致。 它也能使用 IP 篩選功能封鎖來自特定 IP 網域或網域組的 API 呼叫。 Azure API Management 也利用稱為 API 金鑰的公開金鑰組來驗證和授權每個 API 呼叫，以便保護自己的 Web 服務。 當授權失敗時，系統就會封鎖對於 API 及其支援功能的存取。

Azure API Management 也能減少針對某個服務的 API 呼叫數量 (稱為節流的程序)，以便讓 Web 服務的效能最佳化。 如要深入了解，請參閱 [Azure API Management](https://azure.microsoft.com/services/api-management/) 和 [2015 年 AzureCon 中的 Azure API Management](https://channel9.msdn.com/events/Microsoft-Azure/AzureCon-2015/ACON313)。

## <a name="4-data-at-rest-security-methods"></a>4 靜態資料的安全性方法


當資料抵達裝置後，我們將這些資料稱為「靜態資料」。 這種資料必須以安全的方式儲存在裝置上，讓未經授權的使用者或應用程式無法存取這些資訊。 Windows 10 中的應用程式模型會執行許多工作，來確保任何應用程式所儲存的資料只有該應用程式才能存取，同時在需要時提供 API 來共用資料。 還有其他 API 可用來確保資料能夠加密，且認證可以安全地儲存。

## <a name="41-windows-app-model"></a>4.1 Windows 應用程式模型


Windows 過去從來沒有為應用程式下過定義。 它之前最常被稱為可執行檔 (.exe)，且從未包含安裝、儲存體狀態、執行長度、版本設定、作業系統整合，或是 app 之間的通訊。 通用 Windows 平台模型為應用程式模型下了定義，其定義涵蓋安裝、執行階段環境、資源管理、更新、資料模型，以及解除安裝。

Windows 10 應用程式在容器中執行，這代表它們的預設權限有限 (使用者可以要求及授與額外的權限)。 舉例來說，如果應用程式要存取系統中的檔案，則必須使用 [**Windows.Storage.Pickers**](https://msdn.microsoft.com/library/windows/apps/br207928) 命名空間的檔案選擇器 ，來讓使用者挑選檔案 (而非啟用直接存取檔案的權限)。 我們再舉一個範例，如果應用程式要存取使用者的位置資料，它必須讓裝置的定位功能需求受到宣告，讓使用者在下載應用程式時，提示使用者該應用程式需要存取使用者的位置資訊。 除此之外，應用程式第一次要存取使用者的位置時，系統會顯示額外的同意提示，要求存取該資料的權限。

請注意，這個應用程式模型就像是應用程式的「監獄」，代表應用程式無法接觸外界，但它並非無法從外界接觸的「城堡」(擁有系統管理員權限的應用程式當然仍舊可以向內接觸)。 Windows 10 中的 Device Guard，可讓組織/IT 人員指定可執行的 (Win32) app，能進一步協助限制這項存取權。

應用程式模型也會管理應用程式的週期。 它預設會限制應用程式的背景執行，例如在應用程式一進入背景時，就暫停應用程式的處理程序 (但會先給應用程式短暫的時間在程式碼中提及自己遭到暫停)，並凍結應用程式的記憶體。 作業系統不提供讓應用程式要求執行特定背景工作的機制，無論該工作是已經排定、是各種事件 (例如網際網路/藍牙連線、電源變更等) 所觸發，還是屬於特定的案例 (例如播放音樂或使用 GPS 追蹤功能)。

當裝置上的記憶體資源不足時，Windows 會終止應用程式以釋放記憶體空間。 這個週期模型會強制應用程式在暫停時保存資料，因為從暫停到終止之間並沒有任何額外的時間。

如需詳細資訊，請參閱[這是通用的：了解 Windows 10 應用程式的週期](https://visualstudiomagazine.com/articles/2015/09/01/its-universal.aspx)。

## <a name="42-stored-credential-protection"></a>4.2 已儲存認證的保護


會存取已驗證服務的 Windows 應用程式，通常會提供讓使用者將認證儲存在本機裝置上的選項。 這對使用者而言很方便；當他們提供使用者名稱與密碼之後，應用程式就會在後續啟動時自動使用這些資訊。 但如果讓攻擊者取得這項已儲存的資料，可能會帶來安全上的問題，因此 Windows 10 提供讓 Windows 應用程式把使用者認證儲存在安全的認證保險箱中的能力。 應用程式會呼叫認證保險箱 API 來儲存及擷取認證保險箱中的認證，而不是將認證儲存在應用程式的儲存體容器中。 認證保險箱是由作業系統所管理的，但只有儲存認證的的應用程式才能存取，這為認證儲存提供了安全管理的解決方案。

當使用者提供要儲存的認證時，app 會使用 [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 命名空間中的 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 物件，來取得認證保險箱的參照。 它接著會建立包含 Windows app 識別碼和使用者名稱及密碼的 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 物件。 該物件會傳遞給 [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) 方法，以便在保險箱儲存認證。 下列 C# 程式碼範例示範如何完成此作業。

```cs
var vault = new PasswordVault();
vault.Add(new PasswordCredential("My App", username, password));
```

在下列 C# 程式碼範例中，應用程式會呼叫 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 物件的 [**FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083) 方法，以要求對應到該應用程式的所有認證。 如果傳回多個認證，應用程式會提示使用者輸入自己的使用者名稱。 如果保險箱內沒有認證，應用程式會提示使用者輸入認證。 然後，使用者便會使用認證登入伺服器。

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    PasswordCredential loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }
    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}

private PasswordCredential GetCredentialFromLocker()
{
    PasswordCredential credential = null;

    var vault = new PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);

    if (credentialList.Count == 1)
    {
        credential = credentialList[0];
    }
    else if (credentialList.Count > 0)
    {
        // When there are multiple usernames,
        // retrieve the default username. If one doesn't
        // exist, then display UI to have the user select
        // a default username.
        defaultUserName = GetDefaultUserNameUI();

        credential = vault.Retrieve(resourceName, defaultUserName);
    }
    return credential;
}
```

如需詳細資訊，請參閱[認證保險箱](credential-locker.md)。

## <a name="43-stored-data-protection"></a>4.3 已儲存資料的保護


當您在處理已儲存的資料 (通常稱為靜態資料) 時，為這些資料加密即可防止未經授權的使用者存取已儲存的資料。 為資料加密的兩個常見機制，就是使用對稱金鑰或使用非對稱金鑰。 不過，資料加密無法確保資料從傳送到儲存的這段時間中不會遭到修改。 換句話說，這種方法無法確保資料完整性。 使用訊息驗證碼、雜湊及數位簽章，是解決這種問題的常見技術。

## <a name="431-data-encryption"></a>4.3.1 資料加密


使用對稱式加密時，寄件者與收件者都有相同的金鑰，並使用該金鑰來為資料加密及解密。 這個方法的挑戰在於安全地分享金鑰，讓雙方都知道自己有該金鑰。

而其中一個解決方法，就是使用公開/私密金鑰的非對稱式加密。 公開金鑰可自由地與任何想要為訊息加密的使用者分享。 私密金鑰則會永遠保密，只有您可以使用它來為資料解密。 有種允許尋找公開金鑰的常見技術，就是使用數位憑證 (或是簡單稱為憑證)。 憑證除了包含使用者或伺服器的相關資訊 (例如名稱、簽發者、電子郵件地址和國家/地區) 之外，也包含公開金鑰的相關資訊。

Windows app 開發人員可以使用 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 和 [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 類別，在自己的 UWP app 中使用對稱式和非對稱式加密。 此外， [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 類別可以用來加密和解密資料、簽入內容和確認數位簽章。 App 也可以使用 [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空間中的 [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) 類別來加密和解密儲存的本機資料。

## <a name="432-detecting-message-tampering-macs-hashes-and-signatures"></a>4.3.2 偵測訊息竄改 (MAC、雜湊以及簽章)


MAC 是使用對稱金鑰 (稱為祕密金鑰) 產生的程式碼 (或標記)，或做為 MAC 加密演算法輸入的訊息。 寄件者及收件者要同意祕密金鑰及演算法，才會開始訊息傳輸。

MAC 會確認訊息，就像這樣。

-   寄件者使用祕密金鑰做為 MAC 演算法的輸入，以衍生 MAC 標記。
-   寄件者將 MAC 標記及訊息傳送給收件者。
-   收件者使用祕密金鑰與訊息做為 MAC 演算法的輸入，以衍生 MAC 標記。
-   收件者比較其 MAC 標記與寄件者的 MAC 標記。 如果這兩者相同，我們便知道該訊息並未遭到竄改。

![mac 驗證](images/secure-macs.png)

Windows 應用程式可以實行 MAC 訊息驗證，方法是呼叫 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 類別來產生金鑰，以及呼叫 [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 類別來執行 MAC 加密演算法。

## <a name="433-using-hashes"></a>4.3.3 使用雜湊


雜湊函數是使用任意長度的資料區塊的密碼編譯演算法，會傳回一個固定大小位元字串，稱為雜湊值。 有一整個系列的雜湊函數可以執行這項操作。

雜湊值可用來取代上述訊息傳輸案例中的 MAC。 寄件者會傳送雜湊值和一則訊息，收件者從寄件者的雜湊值和訊息衍生其雜湊值，並比較兩個雜湊值。 Windows 10 上執行的 app 可以呼叫 [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 類別來列舉可用的雜湊演算法，並執行其中一個演算法。 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 類別代表雜湊值。 [**CryptographicHash.GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) 方法可以用來重複雜湊不同的資料，而不用在每次使用時都要重新建立物件。 **CryptographicHash** 類別的 Append 方法會將新資料加入要雜湊的緩衝區。 下列 C# 程式碼範例將示範這整個過程。

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the
    // hashing algorithm to use.
    string strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    string strMsg1 = "This is message 1";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    string strMsg2 = "This is message 2";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    string strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    string strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
}
```

## <a name="434-digital-signatures"></a>4.3.4 數位簽章


數位簽署的儲存訊息的資料完整性是以類似 MAC 驗證的方式驗證。 以下是數位簽章工作流程運作的方式。

-   寄件者使用訊息做為雜湊演算法的輸入，以衍生雜湊值 (也稱為摘要)。
-   寄件者使用他們的私密金鑰加密摘要。
-   寄件者會傳送訊息、加密的摘要，以及所使用的雜湊演算法的名稱。
-   收件者使用公開金鑰來解密其收到的加密摘要。 然後使用雜湊演算法雜湊訊息，以建立自己的摘要。 最後，收件者會比較這兩個摘要 (其收到和解密的，以及其所建立的)。 只有在這兩者相符時，收件者才能確定訊息是由私密金鑰的擁有人傳送的，因此對方的確是他們聲稱的人，且訊息在傳輸過程中並未遭到修改。

![數位簽章](images/secure-digital-signatures.png)

雜湊演算法的速度非常快，即使是處理大型訊息時也能迅速衍生雜湊值。 產生的雜湊值長度不一，可能會比完整訊息還要短，因此最佳做法是使用公開金鑰和私密金鑰來只為摘要加密及解密，而非為完整的訊息。

如需詳細資訊，請參閱[數位簽章](https://msdn.microsoft.com/library/windows/desktop/aa381977)、[MAC、雜湊以及簽章](macs-hashes-and-signatures.md)，以及[密碼編譯](cryptography.md)。

## <a name="5-summary"></a>5 總結


Windows 10 的通用 Windows 平台提供數種方式，讓您能利用作業系統的功能來建立更安全的應用程式。 在不同的驗證案例中 (例如單一因素驗證、多重要素驗證，或利用 OAuth 識別提供者的代理驗證)，API 可用來減少在驗證方面最常見的挑戰。 Windows Hello 提供新的生物特徵識別登入系統來辨識使用者，以及主動防禦規避適當身分識別程序的攻擊。 它也會提供多層按鍵及不可以洩露或在信賴平台模組外部使用的憑證。 此外，透過選用的證明識別金鑰和憑證，能進一步提高安全性層級。

如要保護傳輸中的資料，可使用 API 來透過 SSL 與遠端系統安全地通訊，它還提供利用 SSL 關聯來驗證伺服器真確性的可能性。 Azure API Management 能協助您以受控制的方式安全地發佈 API，因為它使用能提供額外的 API 端點混淆的 Proxy，為在網路上公開 API 提供強大的設定選項。 您可以使用 API 金鑰來保護這些 API 的存取權，以及調整 API 呼叫來控制效能。

當資料抵達裝置時，Windows 應用程式模型在應用程式該如何安裝、更新及存取資料方面提供更大的控制權，同時它還能防止其他應用程式以未經授權的方式存取這些資料。 由作業系統管理的認證保險箱能為使用者認證提供安全的儲存空間，而其他資料可在裝置上透過使用由通用 Windows 平台提供的加密和雜湊 API 來保護。

## <a name="6-resources"></a>6 資源


### <a name="61-how-to-articles"></a>6.1 操作說明文章

-   [驗證和使用者識別](authentication-and-user-identity.md)
-   [Windows Hello](microsoft-passport.md)
-   [認證保險箱](credential-locker.md)
-   [Web 驗證代理人](web-authentication-broker.md)
-   [指紋生物識別技術](fingerprint-biometrics.md)
-   [智慧卡](smart-cards.md)
-   [共用的憑證](share-certificates.md)
-   [密碼編譯](cryptography.md)
-   [憑證](certificates.md)
-   [密碼編譯金鑰](cryptographic-keys.md)
-   [資料保護](data-protection.md)
-   [MAC、雜湊以及簽章](macs-hashes-and-signatures.md)
-   [密碼編譯的出口限制](export-restrictions-on-cryptography.md)
-   [常見的密碼編譯工作](common-cryptography-tasks.md)

### <a name="62-code-samples"></a>6.2 程式碼範例

-   [認證保險箱](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/PasswordVault)
-   [認證選擇器](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CredentialPicker)
-   [利用 Azure 登入功能鎖定裝置](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DeviceLockdownAzureLogin)
-   [企業資料保護](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/EnterpriseDataProtection)
-   [KeyCredentialManager](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/KeyCredentialManager)
-   [智慧卡](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SmartCard)
-   [Web 帳戶管理](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/WebAccountManagement)
-   [WebAuthenticationBroker](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/WebAuthenticationBroker)

### <a name="63-api-reference"></a>6.3 API 參考

-   [**Windows.Security.Authentication.OnlineId**](https://msdn.microsoft.com/library/windows/apps/hh701371)
-   [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044)
-   [**Windows.Security.Authentication.Web.Core**](https://msdn.microsoft.com/library/windows/apps/dn921967)
-   [**Windows.Security.Authentication.Web.Provider**](https://msdn.microsoft.com/library/windows/apps/dn921965)
-   [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)
-   [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)
-   [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356)
-   [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404)
-   [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476)
-   [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547)
-   [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585)
-   [**Windows.Security.ExchangeActiveSyncProvisioning**](https://msdn.microsoft.com/library/windows/apps/hh701506)
-   [**Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)
