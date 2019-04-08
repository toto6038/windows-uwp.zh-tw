---
title: 啟動連絡人應用程式
description: 本主題描述 ms-people URI 配置。 您的應用程式可以使用此 URI 配置來啟動連絡人應用程式，以執行特定動作。
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ab10acab42ab3f03121a7c5a462cb651b0f3f31b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595563"
---
# <a name="launch-the-people-app"></a>啟動連絡人應用程式

本主題描述**ms 人：** URI 配置。 您的應用程式可以使用此 URI 配置來啟動連絡人應用程式，以執行特定動作。

## <a name="ms-people-uri-scheme-reference"></a>ms-people:URI 配置參考

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">結果</th>
<th align="left">URI 配置</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">可讓其他 app 啟動連絡人 app 主頁面。</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">可讓其他 app 啟動連絡人 app 設定頁面。</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">可讓其他 app 提供將會啟動含有搜尋結果頁面之連絡人 app 的搜尋字串。
<div class="alert">
<p>這些參數區分大小寫。</p>
<p>如果您未正確輸入語法或遺失搜尋字串值，預設行為將是傳回未經篩選的完整連絡人清單。</p>
</div>
<div>
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">如果找到連絡人，就會啟動到現有的連絡人卡片。 如果找不到連絡人，就會啟動到暫時連絡人卡片。 如果未提供輸入參數，我們將會啟動含有連絡人清單的連絡人 app。
<div class="alert">
<p>這些參數區分大小寫。</p>
<p>參數的順序並不重要。</p>
<p>如果有多個相符項目，我們會傳回第一個相符的連絡人。</p>
</div>
<div> 
</div></td>
<td align="left">ms-人： viewcontact 嗎？ContactId =&lt;contactid&gt;&amp;AggregatedId =&lt;aggid&gt;&amp;PhoneNumber = &lt;phonenum&gt;&amp;電子郵件 =&lt;電子郵件&gt; &amp;ContactName =&lt;名稱&gt;&amp;連絡人 =&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">啟動到連絡人 app 內的儲存連絡人頁面，以提供的電話號碼或電子郵件地址與儲存指定的連絡人。
<div class="alert">
<p>這些參數區分大小寫。</p>
<p>參數的順序並不重要。</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
<tr class="even">
<td align="left">啟動至 \[連絡人\] 應用程式 中的 \[新增連絡人\] 頁面，以儲存指定的連絡人。
<div class="alert"><p>請使用 <a href="https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a> 來開啟 \[儲存新的連絡人\] 頁面。 使用 <strong>LaunchUriAsync</strong> 只會啟動 [連絡人] App 主頁面。</p>
<p>這些參數區分大小寫。</p>
<p>參數的順序並不重要。</p>
<p>您可以使用任何支援的參數組合。</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savecontacttask?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesearch-parameter-reference"></a>ms-people:search: 參數參考

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">描述</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>選用。</p>
<p>連絡人搜尋資訊的搜尋字串。</p>
<p>電話號碼或連絡人名稱。</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peopleviewcontact-parameter-reference"></a>ms-people:viewcontact: 參數參考

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">描述</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的連絡人識別碼。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>電話號碼</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的電話號碼。</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>電子郵件</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的電子郵件。</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>連絡人姓名</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的名稱。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>請連絡</b></td>
<td align="left"><p>選用。</p>
<p>Contact 物件。</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavetocontact-parameter-reference"></a>ms-people:savetocontact: 參數參考

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">描述</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>電話號碼</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的電話號碼。</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>電子郵件</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的電子郵件。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>連絡人姓名</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的名稱。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>ms-people:savecontacttask: 參數參考

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">描述</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>公司</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的公司名稱。</p></td>

</tr>
<tr class="even">
<td align="left"><b>firstName</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的名字。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>選用。</p>
<p>住家地址的縣市。</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>選用。</p>
<p>住家地址的國家/地區。</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>選用。</p>
<p>住家地址的州/省。</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>選用。</p>
<p>住家地址的街道。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>選用。</p>
<p>住家地址的郵遞區號。</p></td>

</tr>
<tr class="even">
<td align="left"><b>住家電話</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的住家電話號碼。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的職稱。</p></td>
</tr>

<tr class="even">
<td align="left"><b>lastName</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的姓氏。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>middleName</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的中間名。</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的行動電話號碼。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>暱稱</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的暱稱。</p></td>
</tr>

<tr class="even">
<td align="left"><b>附註</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的附註</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的其他電子郵件。</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的個人電子郵件。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>後置詞</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的尾碼。</p></td>
</tr>

<tr class="even">
<td align="left"><b>標題</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的頭銜。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>網站</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的網站。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>選用。</p>
<p>工作地址的縣市。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>選用。</p>
<p>工作地址的國家/地區。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>選用。</p>
<p>工作位址的州/省。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>選用。</p>
<p>工作地址的街道。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>選用。</p>
<p>工作地址的郵遞區號。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>工作電子郵件地</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的工作電子郵件。</p></td>
</tr>

<tr class="even">
<td align="left"><b>分隔符號</b></td>
<td align="left"><p>選用。</p>
<p>連絡人的工作電話號碼。</p></td>
</tr>
</tbody>
</table>
