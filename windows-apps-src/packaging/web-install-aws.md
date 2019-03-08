---
title: 在 AWS 上裝載 UWP 應用程式套件以進行 Web 安裝
description: 教學課程中設定 AWS web 伺服器來驗證透過應用程式安裝程式應用程式的應用程式安裝
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10，Windows 10 UWP 應用程式安裝程式，AppInstaller，側載，相關的設定 （選擇性） 套件，AWS
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628053"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>在 AWS 上裝載 UWP 應用程式套件以進行 Web 安裝

應用程式安裝程式可讓開發人員和 IT 專業人員透過在他們自己的內容傳遞網路 (CDN) 上，散發 Windows 10 應用程式。 這對於不想要或不需要發佈其應用程式至 Microsoft Store ，但仍然想要利用 Windows 10 封裝與部署平台的企業而言，是很實用的。

本主題概述設定裝載 UWP 應用程式套件，將 Amazon Web Services (AWS) 網站，以及如何使用應用程式安裝程式應用程式來安裝應用程式套件的步驟。

## <a name="setup"></a>設定

為成功遵循本教學課程，您需要：
 
1. AWS 訂用帳戶 
2. Web 網頁
3. UWP app 套件 - 您將散發的應用程式套件

選擇性：[入門專案](https://github.com/AppInstaller/MySampleWebApp)GitHub 上。 如果您沒有要使用的應用程式套件或網頁，但仍然想要了解如何使用這項功能，這會很有幫助。

本教學課程將介紹如何設定網頁和 AWS 上的主應用程式封裝。 這將需要的 AWS 訂用帳戶。 根據作業的標尺，您可以使用其可用的成員資格来遵循此教學課程。 

## <a name="step-1---aws-membership"></a>步驟 1-AWS 成員資格
若要取得使用 AWS 成員資格，請瀏覽[AWS 帳戶詳細資料頁面](https://aws.amazon.com/free/)。 基於本教學課程的目的，您可以使用免費的會員資格。

## <a name="step-2---create-an-amazon-s3-bucket"></a>步驟 2-建立 Amazon S3 貯體

Amazon Simple Storage Service (S3) 是 AWS 供應項目收集、 儲存和分析資料。 S3 貯體是便利的方式來裝載 UWP 應用程式套件和發佈的網頁。 

使用您的認證，在登入 AWS 之後`Services`尋找`S3`。 

選取 **建立貯體**，然後輸入**貯體名稱**為您的網站。 請遵循對話方塊會提示您輸入設定屬性和權限。 若要確保可以發佈在網站中的 UWP 應用程式，啟用**讀取**並**撰寫**貯體，然後選取的權限**授與到這個貯體的公用讀取權限**.

![設定 Amazon S3 貯體中的 權限](images/aws-permissions.png) 

檢閱摘要，請確定選取的選項會反映。 按一下 **建立貯體**來完成此步驟。 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>步驟 3-上傳至 S3 貯體的 UWP 應用程式套件和網頁

您已建立一個 Amazon S3 貯體，您將能夠在您的 Amazon S3 檢視中看到它。 以下是我們的示範貯體的外觀的範例：

![Amazon S3 貯體檢視](images/aws-post-create.png)

我們現在已準備好上傳的應用程式套件和我們想要裝載在我們的 Amazon S3 貯體中的網頁。 

按一下新建立的貯體，即可上傳內容。 貯體是目前空的因為任何具有尚未上傳。 按一下 **上傳**按鈕，然後選取 應用程式套件與您要上傳的網頁檔案。

> [!NOTE]
> 如果您沒有可使用的應用程式套件，您可以使用 GitHub 上屬於所提供[入門專案](https://github.com/AppInstaller/MySampleWebApp)存放庫一部分的應用程式套件。 簽署套件的憑證 (MySampleApp.cer) 也是在 GitHub 上的範例。 您必須在安裝應用程式之前安裝憑證到您的裝置。

![上傳應用程式套件](images/aws-upload-package.png)

類似於建立 Amazon S3 貯體的權限，貯體中的內容也必須擁有**讀取**，**撰寫**，並**授與此物件的公用讀取權限**權限。

如果您想要測試上傳 web 網頁，但還沒有，您可以使用範例 html 頁面 (default.html) 從[入門專案](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)。

> [!IMPORTANT]
> 您上傳的網頁之前，請確認您的網頁中的應用程式套件參考正確無誤。 

若要取得應用程式套件參考，請先將應用程式套件上傳並複製應用程式封裝 URL。 編輯 html 網頁，以反映正確的應用程式封裝路徑。 請參閱程式碼範例，如需詳細資訊。 

選取已上傳的應用程式套件檔案，以取得應用程式套件的參考連結，它應該類似此範例：

![上傳的封裝路徑](images/aws-package-path.png)

**複製**連結至應用程式封裝，然後在您的網頁中加入參考。 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
將 html 檔案上傳您的 Amazon S3 貯體。 請記得設定權限以允許**讀取**並**撰寫**存取。

## <a name="step-4---test"></a>步驟 4： 測試

網頁上傳到您的 Amazon S3 貯體，來取得 web 網頁連結選取已上傳的 html 檔案。

您可以使用連結來開啟網頁。 因為我們設定要授與公用存取權的應用程式套件和網頁的權限時，web 網頁連結的任何人都可以存取它，並安裝您使用應用程式安裝程式的 UWP 應用程式封裝。 請注意，應用程式安裝程式的 Windows 10 平台的一部分。 身為開發人員，您不需要任何額外的程式碼或功能加入您的應用程式，若要啟用的應用程式安裝程式。 

## <a name="troubleshooting"></a>疑難排解

### <a name="app-installer-fails-to-install"></a>應用程式安裝程式安裝失敗 

如果應用程式封裝以簽署的憑證未安裝在裝置上，應用程式安裝將會失敗。 若要修正這個問題，您必須在安裝應用程式之前安裝憑證。 如果您裝載公開散發的應用程式套件，建議您登入您的應用程式套件，使用從憑證授權單位憑證。 


