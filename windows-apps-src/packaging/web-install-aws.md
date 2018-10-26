---
author: laurenhughes
title: 在 AWS 上裝載 UWP 應用程式套件以進行 Web 安裝
description: 教學課程，來驗證應用程式安裝應用程式安裝程式應用程式透過設定 AWS 網頁伺服器
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10，Windows 10，UWP，應用程式安裝程式，AppInstaller，側載，相關設定，選用套件，AWS
ms.localizationpriority: medium
ms.openlocfilehash: f24abac93e2444a3c9f454c8883902e5db4d31be
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5548832"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>在 AWS 上裝載 UWP 應用程式套件以進行 Web 安裝

應用程式安裝程式可讓開發人員和 IT 專業人員透過在他們自己的內容傳遞網路 (CDN) 上，散發 Windows 10 應用程式。 這對於不想要或不需要發佈其應用程式至 Microsoft Store ，但仍然想要利用 Windows 10 封裝與部署平台的企業而言，是很實用的。

本主題概述設定 Amazon Web 服務 (AWS) 網站，以裝載 UWP 應用程式套件，以及如何使用應用程式安裝程式安裝應用程式套件的步驟。

## <a name="setup"></a>設定

為成功遵循本教學課程，您需要：
 
1. AWS 訂閱 
2. 網頁
3. UWP app 套件 - 您將散發的應用程式套件

選用：在 GitHub 上的[入門專案](https://github.com/AppInstaller/MySampleWebApp)。 如果您沒有要使用的應用程式套件或網頁，但仍然想要了解如何使用這項功能，這會很有幫助。

本教學課程會說明如何安裝網頁，並在 AWS 上的裝載套件一點。 這將會需要 AWS 訂閱。 根據您的作業的縮放比例，您可以使用免費的會員資格遵循本教學課程。 

## <a name="step-1---aws-membership"></a>步驟 1-AWS 會員資格
若要取得 AWS 會員資格，請瀏覽[AWS 帳戶詳細資料頁面](https://aws.amazon.com/free/)。 基於本教學課程的目的，您可以使用免費的會員資格。

## <a name="step-2---create-an-amazon-s3-bucket"></a>步驟 2-建立 Amazon S3 桶

Amazon Simple Storage Service (S3) 是 AWS 提供收集、 儲存和分析資料。 S3 值區是裝載 UWP 應用程式套件和網頁散布的便利方式。 

使用您的認證下, 登入 AWS 之後`Services`尋找`S3`。 

選取**建立桶**，並輸入您網站的 [**桶名稱**。 依照對話方塊提示設定屬性和權限。 若要確保您的 UWP app，從您的網站散發，啟用**讀取**與**寫入**權限您桶並選取**公用讀取存取權授與此桶**。

![設定 Amazon S3 桶的權限](images/aws-permissions.png) 

檢閱摘要]，請確定已選取的選項會反映。 按一下 [**建立桶**完成此步驟。 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>步驟 3-UWP 應用程式套件和網頁上傳至 S3 桶

其中一個您已建立 Amazon S3 桶，您將會看到它在 Amazon S3 檢視中。 以下是我們示範桶的外觀的範例：

![Amazon S3 桶檢視](images/aws-post-create.png)

我們已經準備好要上傳應用程式套件和我們希望我們 Amazon S3 桶中裝載的網頁。 

按一下新建立的桶上傳的內容。 桶是目前空的因為任何項目已上傳尚未。 按一下 [**上傳**] 按鈕，然後選取應用程式套件和網頁您想要上傳的檔案。

> [!NOTE]
> 如果您沒有可使用的應用程式套件，您可以使用 GitHub 上屬於所提供[入門專案](https://github.com/AppInstaller/MySampleWebApp)存放庫一部分的應用程式套件。 簽署套件的憑證 (MySampleApp.cer) 也是在 GitHub 上的範例。 您必須在安裝應用程式之前安裝憑證到您的裝置。

![上傳應用程式套件](images/aws-upload-package.png)

類似於建立 Amazon S3 桶的權限，桶中的內容也必須有**讀取**、**寫入**和**公用唯讀存取權授與此物件**的權限。

如果您想要測試上傳網頁，但沒有的話，您可以使用範例 html 頁面 (default.html) 從[起始專案](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)。

> [!IMPORTANT]
> 您上傳網頁之前，請確認您網頁中的應用程式套件參考正確無誤。 

若要取得應用程式套件參考，第一次上傳應用程式套件和應用程式套件 URL 複製。 編輯 html 網頁，以反映正確的應用程式封裝路徑。 如需詳細資訊的程式碼範例，請參閱。 

選取已上傳應用程式套件檔案來取得應用程式套件的參考連結，它應該是類似於此範例中：

![已上傳的套件的路徑](images/aws-package-path.png)

**複製**到應用程式連結封裝，並在您的網頁中新增參考。 

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
上傳您 Amazon S3 桶的 html 檔案。 請記住設定允許**讀取**和**寫入**存取權限。

## <a name="step-4---test"></a>步驟 4-測試

一旦網頁上傳到您 Amazon S3 桶，透過選取已上傳的 html 檔案以取得網頁的連結。

您可以使用連結來開啟的網頁。 由於我們設定權限授與應用程式套件和網頁的公開存取權，網頁連結的任何人都無法存取它並安裝您應用程式安裝程式使用的 UWP 應用程式套件。 請注意，應用程式安裝程式的 Windows 10 平台的一部分。 身為開發人員，您不需要將任何額外的程式碼或功能新增到您的應用程式啟用的應用程式安裝程式使用。 

## <a name="troubleshooting"></a>疑難排解

### <a name="app-installer-fails-to-install"></a>應用程式安裝程式無法安裝 

如果裝置上未安裝簽署應用程式套件的憑證，應用程式安裝將會失敗。 若要修正這個問題，您必須在安裝應用程式之前安裝憑證。 如果您裝載公開散發的應用程式套件，建議您簽署您的應用程式套件中的憑證授權單位的憑證。 


