---
author: jnHs
Description: "若要在適用於 Windows Phone 8.1 和較舊版本的 app 中使用地圖服務，您需要在 app 程式碼中包含地圖服務應用程式識別碼和權杖。 您可以在 \\[服務\\] 區段中的 \\[地圖\\] 頁面的開發人員中心儀表板取得這個權杖。"
title: "使用地圖服務"
ms.assetid: E5EE6B56-B86F-4D62-B16A-F023FE98EFAB
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 1657c092d09761b7e1db82252295356384faf68a
ms.lasthandoff: 02/07/2017

---

# <a name="use-map-services"></a>使用地圖服務


若要在適用於 Windows Phone 8.1 和較舊版本的 app 中使用地圖服務，您需要在 app 程式碼中包含地圖服務應用程式識別碼和權杖。 您可以在 [服務]**** 區段中的 [地圖]**** 頁面的開發人員中心儀表板取得這個權杖。

> **注意** 若要在目標為其他作業系統的 app 中使用地圖服務，請瀏覽 [Bing 地圖開發人員中心](http://go.microsoft.com/fwlink/p/?LinkId=614880)。 如需詳細資訊，請參閱[要求地圖驗證金鑰](https://msdn.microsoft.com/library/windows/apps/mt219694)。

一旦您[保留 app 名稱](create-your-app-by-reserving-a-name.md)之後，請尋找左方導覽功能表中的 [服務]**** 區段，然後將它展開以顯示 [地圖]**** 頁面。 當您按一下 [取得權杖]**** 時，將會產生 **ApplicationID** 和 **AuthenticationToken** 並顯示在此頁面。

> **注意** 目前您不需要完成提交 app 的作業。 在您要求權杖和識別碼之後，這項資訊會儲存在此頁面。 您可以隨時返回此頁面以存取這項資訊。

您也必須確定在封裝和提交您的 app 之前，將 **ApplicationID** 與 **AuthenticationToken** 加入至您的程式碼。 如需詳細資訊，請參閱[如何新增地圖控制項到頁面 (Windows Phone 8.1)](http://go.microsoft.com/fwlink/p/?LinkId=614882)。

 

 





