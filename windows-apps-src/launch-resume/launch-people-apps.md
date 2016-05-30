---
author: mcleblanc
title: 啟動連絡人 app
description: 本主題描述 ms-people URI 配置。 您的 app 可以使用此 URI 配置來啟動連絡人 app，以執行特定動作。
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
---

# 啟動連絡人 app


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主題描述 **ms-people:** URI 配置。 您的 app 可以使用此 URI 配置來啟動連絡人 app，以執行特定動作。

## ms-people: URI 配置參考


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
**注意**  
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
**注意**  
<p>這些參數區分大小寫。</p>
<p>參數的順序並不重要。</p>
<p>如果有多個相符項目，我們會傳回第一個相符的連絡人。</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">啟動到連絡人 app 內的儲存連絡人頁面，以提供的電話號碼或電子郵件地址與儲存指定的連絡人。
<div class="alert">
**注意**  
<p>這些參數區分大小寫。</p>
<p>參數的順序並不重要。</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

 

## ms-people:search: 參數參考


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">說明</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**SearchString**</td>
<td align="left"><p>選用。</p>
<p>連絡人搜尋資訊的搜尋字串。</p>
<p>電話號碼或連絡人名稱。</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

 

## ms-people:viewcontact: 參數參考


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">說明</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**ContactId**</td>
<td align="left"><p>選用。</p>
<p>連絡人的連絡人識別碼。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>選用。</p>
<p>連絡人的電話號碼。</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**Email**</td>
<td align="left"><p>選用。</p>
<p>連絡人的電子郵件。</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>選用。</p>
<p>連絡人的名稱。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**Contact**</td>
<td align="left"><p>選用。</p>
<p>Contact 物件。</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

 

## ms-people:savetocontact: 參數參考


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">說明</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>選用。</p>
<p>連絡人的電話號碼。</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**Email**</td>
<td align="left"><p>選用。</p>
<p>連絡人的電子郵件。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>選用。</p>
<p>連絡人的名稱。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 





<!--HONumber=May16_HO2-->


