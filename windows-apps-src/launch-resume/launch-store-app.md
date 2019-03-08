---
title: 啟動 Microsoft Store 應用程式
description: 本主題描述 ms-windows-store URI 配置。 您的應用程式可以使用此 URI 配置，來啟動 Microsoft Store 應用程式存放區中的特定頁面。
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cda37ee9964a3e7e02f4e4ce3829a8b55e823692
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660893"
---
# <a name="launch-the-microsoft-store-app"></a>啟動 Microsoft Store 應用程式



本主題描述**ms windows 市集：** URI 配置。 您的應用程式可以使用此 URI 配置，啟動 Microsoft Store 應用程式存放區中的特定頁面使用[ **LaunchUriAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701476)方法。

此範例顯示如何將 Microsoft Store 開啟到 \[遊戲\] 頁面：

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms windows 市集：URI 配置參考

<table>
<tr><th>描述</th><th></th><th>URI 配置</th></tr>
<tr><td>啟動市集的首頁。</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>啟動市集中的頂層類別。<p>注意：並非所有的使用者可以存取所有的縱向市場。</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">啟動產品的產品詳細資料頁面 (PDP)。 <p>存放區識別碼建議用於 Windows 10 上的客戶，並將所有的作業系統版本，但這麼做的先前方式運作 (例如：PFN) 都仍受支援。</p>
<p>這些值可在<a href="https://partner.microsoft.com/dashboard">合作夥伴中心</a>上<a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">應用程式身分識別</a>每個應用程式的應用程式管理 區段中的頁面。</p>
</td>
<td>
Store 識別碼 <p>(建議使用)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>套件系列名稱 (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>產品識別碼 (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>產品識別碼 (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">啟動產品的撰寫評論體驗。</td>
<td>Store 識別碼 <p>(建議使用)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>套件系列名稱 (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>產品識別碼 (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>產品識別碼 (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>啟動搜尋與副檔名相關聯之產品的作業。 </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>啟動搜尋與通訊協定相關聯之產品的作業。</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>啟動搜尋與一或多個標籤相關聯之產品的作業。 標籤應以逗號分隔。
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
啟動指定查詢的搜尋。 在查詢中可使用空格。
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>啟動對類別中的產品進行搜尋的作業。</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>啟動從指定的發行者搜尋產品的作業。 在名稱中可使用空格。
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>啟動下載和更新頁面。</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>啟動市集設定頁面。</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 
