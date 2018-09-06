---
title: 認證保險箱
description: 本文說明通用 Windows 平台 (UWP) 應用程式可以如何使用認證保險箱，來安全地儲存和擷取使用者認證，並透過使用者的 Microsoft 帳戶在裝置之間進行漫遊。
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: c6412f28e60ed0401fb96098fd38128a37491c8d
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3390432"
---
# <a name="credential-locker"></a>認證保險箱




本文說明通用 Windows 平台 (UWP) 應用程式可以如何使用認證保險箱，來安全地儲存和擷取使用者認證，並透過使用者的 Microsoft 帳戶在裝置之間進行漫遊。

例如，您有一個 app 會連線至服務以存取受保護的資源 (如媒體檔案或社交網路)。 而您的服務需要每個使用者的登入資訊。 您在 app 中已經建立能取得使用者的使用者名稱及密碼的 UI，之後會使用這些資訊將使用者登入服務。 使用認證保險箱 API 時，您可以儲存並輕鬆地擷取使用者的使用者名稱與密碼，並在使用者下次開啟 app 時，無論他們使用什麼裝置都可以將他們自動登入。

儲存在 CredentialLocker 中的使用者認證*不會*過期，*不受* [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) 影響，也*不會*像傳統漫遊資料在一段時間無作用之後遭到清除。 不過，您最多只能在 CredentialLocker 中針對每個 app 儲存 20 個認證。

網域帳戶的認證保險箱運作方式有些不同。 如果有隨您的 Microsoft 帳戶儲存的認證，而您將該帳戶與網域帳戶 (例如您的工作帳戶) 關聯，則您的認證會漫遊至該網域帳戶。 但使用該網域帳戶登入時所新增的任何新認證則不會漫遊。 這樣可確保網域的私密認證不會暴露到網域外。

## <a name="storing-user-credentials"></a>儲存使用者認證


1.  使用 [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 命名空間中的 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 物件取得認證保險箱的參考。
2.  建立一個包含您的應用程式識別碼、使用者名稱及密碼的 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 物件，並將它傳送到 [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) 方法以在保險箱新增認證。

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>擷取使用者認證


在具備 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 物件的參考後，您有幾個選項可從認證保險箱擷取使用者認證。

-   您可以使用 [**PasswordVault.RetrieveAll**](https://msdn.microsoft.com/library/windows/apps/br227088) 方法來擷取保險箱中使用者為您的應用程式提供的所有認證。

-   如果您知道所儲存認證的使用者名稱，您可以使用 [**PasswordVault.FindAllByUserName**](https://msdn.microsoft.com/library/windows/apps/br227084) 方法來擷取該使用者名稱的所有認證。

-   如果您知道所儲存認證的資源名稱，您可以使用 [**PasswordVault.FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083) 方法來擷取該資源名稱的所有認證。

-   最後，如果您知道某個認證的使用者名稱與資源名稱，您可以使用 [**PasswordVault.Retrieve**](https://msdn.microsoft.com/library/windows/apps/br227087) 方法來擷取該認證。

請看以下範例，在此範例中，我們將資源名稱以全域方式儲存在應用程式中，如果我們找到使用者的認證，就會自動將使用者登入。 如果我們找到同一位使用者有多個認證，則會要求使用者選取登入時要使用的預設認證。

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    var loginCredential = GetCredentialFromLocker();

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


private Windows.Security.Credentials.PasswordCredential GetCredentialFromLocker()
{
    Windows.Security.Credentials.PasswordCredential credential = null;

    var vault = new Windows.Security.Credentials.PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);
    if (credentialList.Count > 0)
    {
        if (credentialList.Count == 1)
        {
            credential = credentialList[0];
        }
        else
        {
            // When there are multiple usernames,
            // retrieve the default username. If one doesn't
            // exist, then display UI to have the user select
            // a default username.

            defaultUserName = GetDefaultUserNameUI();

            credential = vault.Retrieve(resourceName, defaultUserName);
        }
    }

    return credential;
}
```

## <a name="deleting-user-credentials"></a>刪除使用者認證


刪除儲存在認證保險箱的使用者認證也是一個快速的雙步驟程序。

1.  使用 [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 命名空間中的 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 物件取得認證保險箱的參考。

2.  將您想刪除的認證傳送到 [**PasswordVault.Remove**](https://msdn.microsoft.com/library/windows/apps/hh701242) 方法。

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>最佳做法


僅使用認證保險箱來儲存密碼，不要儲存較大的資料 blob。

只在符合下列條件時才會將密碼儲存至認證保險箱：

-   使用者已成功登入過。
-   使用者已選擇儲存密碼。

請勿使用應用程式資料或漫遊設定儲存純文字形式的認證。
