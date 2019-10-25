---
Description: 您可以設定應用程式在存放區中可使用的精確日期和時間，讓您有更大的彈性，並能夠為不同市場自訂日期。
title: 設定精確的發行排程
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10，uwp，排程，發行日期，日期，啟動
ms.localizationpriority: medium
ms.openlocfilehash: ec9ee00aaa350fc48185cc6328674ac2d8f62ea5
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690358"
---
# <a name="configure-precise-release-scheduling"></a>設定精確的發行排程

[[定價與可用性](set-app-pricing-and-availability.md)] 頁面上的 [**排程**] 區段可讓您設定應用程式在存放區中的可用日期和時間，讓您有更大的彈性，並能夠自訂不同市場的日期。

> [!NOTE]
> 雖然本主題參考應用程式，但附加元件提交的發行排程會使用相同的流程。

您可以另外選擇設定產品在商店中不能再使用的日期。 請注意，這表示產品無法再透過搜尋或流覽在商店中找到，但任何具有直接連結的客戶都可以看到產品的商店清單。 只有當使用者已擁有產品或有[促銷代碼](generate-promotional-codes.md)，而且正在使用 Windows 10 裝置時，他們才能下載。

根據預設（除非您已在 [[可見度](choose-visibility-options.md#discoverability)] 區段的 [存放區] 選項中選取 [**讓此應用程式可供使用但無法**探索]，否則您的應用程式將會在客戶通過認證並完成發行時立即提供給客戶流程. 若要選擇其他日期，請選取 [**顯示選項**] 以展開此區段。

請注意，如果您已在 [[可見度](choose-visibility-options.md#discoverability)] 區段的 [存放區] 選項中選取其中一個 [**讓此應用程式可供使用但無法**探索]，您將無法在 [**排程**] 區段中設定日期，因為您的應用程式不會發行至客戶，所以沒有可設定的發行日期。

> [!IMPORTANT]
> 您在 [排程] 區段中指定的日期僅適用于 Windows 10 上的客戶。
>
>如果您先前發佈的應用程式支援較舊的 OS 版本，則您選取的任何「**停止**取得」日期都不會套用到這些客戶;他們仍然可以取得應用程式（除非您在 [[可見度](choose-visibility-options.md#discoverability)] 區段中以新的選取專案提交更新，或從**應用程式**的 [總覽] 頁面選取 [**讓應用程式無法使用**]）。


## <a name="base-schedule"></a>基本排程

您針對基本排程所做的選擇將會套用到您的應用程式可供使用的所有市場，除非您在稍後針對特定市場選取 [[自訂](#customize-the-schedule-for-specific-markets)]，以新增特定市場（或市場群組）的日期。

您會在這裡看到兩個選項： [**發行**] 和 [**停止**取得]。 

## <a name="release"></a>版本

在 [**發行**] 下拉式集中，您可以設定要在存放區中使用應用程式的時間。 這表示應用程式可透過搜尋或流覽在存放區中找到，而且客戶可以查看其商店清單並取得應用程式。

>[!NOTE]
> 當您的應用程式已發佈且已在商店中推出之後，您將無法再選取**發行**日期（因為應用程式已發行）。

以下是您可以為產品的**發行**排程設定的選項：
- **儘快：產品**會在認證併發布之後立即發行。 這是預設選項。
- **于**：產品將會在您選取的日期和時間發行。 另外還有兩個選項：
   - **UTC**：您選取的時間將會是國際標準時間（UTC）時間，因此應用程式會在所有位置的相同時間釋放。
   - **本機**：您選取的時間將會在與市場相關聯的每個時區中使用。 （請注意，針對包含多個時區的市場，將只會使用該市場中的一個時區。 針對美國，會使用東部時區。 完整的時間區域清單會顯示在此頁面的下方。）
- **未排程**：應用程式將無法在存放區中使用。 如果您選擇此選項，您可以在稍後建立新的提交，並選擇其中一個其他選項，讓應用程式在存放區中可供使用。


## <a name="stop-acquisition"></a>停止取得

在 [**停止**取得] 下拉式清單中，您可以設定要停止允許新客戶從商店取得它或探索其清單的日期和時間。 如果您想要精確地控制將應用程式不再提供給新客戶的時間，例如當您協調多個應用程式之間的可用性時，這會很有用。

根據預設，[**停止**取得] 會設為 [永不]。 若要變更此項，請在下拉式選單中選取 [ **at** ]，並指定日期和時間，如上所述。 在您選取的日期和時間，客戶將無法再取得應用程式。

請務必瞭解，此選項與選取 [**讓此應用程式可供探索，但無法**在[可見度](choose-visibility-options.md#discoverability)] 區段中使用，並選擇 [停止取得] 有相同的影響 **：任何具有直接連結的客戶都可以看到產品的商店列出，但只有在先前擁有產品或具有促銷代碼並使用 Windows 10 裝置時，才可以下載。** 若要完全停止提供應用程式給新客戶，請按一下 [應用程式總覽] 頁面中的 [**讓應用程式無法使用**]。 如需詳細資訊，請參閱[從存放區移除應用程式](guidance-for-app-package-management.md#removing-an-app-from-the-store)。

> [!TIP]
> 如果您選取要**停止**取得的日期，並在稍後決定要讓應用程式再次可用，您可以建立新的提交，並將**停止**取得變更回**永不**。 發行更新的提交之後，應用程式會再次變成可用。

## <a name="customize-the-schedule-for-specific-markets"></a>自訂特定市場的排程 

根據預設，您在上面選取的選項將會套用至您的應用程式所提供的所有市場。 若要自訂特定市場的價格，請按一下 [**針對特定市場自訂**]。 [**市場選擇**] 快顯視窗隨即出現，並列出您已選擇讓應用程式可供使用的所有市場。 如果您已排除[市場](define-pricing-and-market-selection.md)一節中的任何市場，將不會顯示這些市場。 

若要為一個市場新增排程，請選取它，然後按一下 [**儲存**]。 您接著會看到上述相同的**發行**和**停止**取得選項，但您所做的選擇只會套用到該市場。

若要新增適用于多個市場的排程，您將會建立一個*市場群組*。 若要這麼做，請選取您想要包含的市場，然後輸入該群組的名稱。 （此名稱僅供您參考，且不會對任何客戶顯示）。例如，如果您想要建立北美洲的市場群組，您可以選取 [**加拿大**]、[**墨西哥**] 和 [**美國**]，並將它命名**北美洲**或您選擇的其他名稱。 當您完成建立市場群組時，按一下 [**儲存**]。 您接著會看到上述相同的**發行**和**停止**取得選項，但您所做的選擇只會套用到該市場群組。

若要新增其他市場或其他市場群組的自訂排程，請按一下 [**自訂特定市場**]，然後重複這些步驟。 若要變更市場群組中所包含的市場，請選取其名稱。 若要移除市場群組（或個別市場）的自訂排程，請按一下 [**移除**]。

> [!NOTE]
> 市場不能屬於您在 [**排程**] 區段中使用的一個或多個市場群組。 

## <a name="global-time-zones"></a>全域時區

下表顯示每個市場中使用的特定時區，因此當您的提交使用當地時間時（例如，在上午9點發行），您可以找出每個市場的發行時間，特別適用于有多個時間 z 的市場。一種，例如加拿大。

<a name="market--time-zone"></a>市場：時區
==================
阿富汗：（UTC + 04：30）喀布爾阿爾巴尼亞：（UTC + 01：00）塞拉耶佛，斯高彼亞，華沙，札格雷布阿爾及利亞：（UTC + 01：00）塞拉耶佛，斯高彼亞，華沙，札格雷布美屬薩摩亞：（UTC + 13：00）薩摩亞安道爾：（UTC + 01：00）塞拉耶佛，斯高彼亞，華沙，札格雷布安哥拉：（UTC + 01：00）西部中央非洲安圭拉島：（UTC-04:00）大西洋時間（加拿大）南極洲：（UTC + 12：00）奧克蘭，威靈頓安地卡及巴布達：（UTC-04:00）大西洋中部時間（加拿大）阿根廷：（UTC-03:00） City，布宜諾斯艾利斯：（UTC + 04：00）阿布達比阿布達比，馬斯喀特 Aruba：（UTC-04:00）大西洋時間（加拿大）澳大利亞：（UTC + 10：00）坎培拉，墨爾本，悉尼奧地利：（UTC + 01：00）阿姆斯特丹，柏林，伯恩，羅馬，斯德哥爾摩，維也納亞塞拜然：（UTC + 04：00）巴庫巴哈馬，& 時間：（UTC-05:00）巴林：（UTC+ 04:00）阿布達比阿布達比，馬斯喀特孟加拉國：（UTC + 06：00）達卡巴巴多斯：（utc-04:00）大西洋時間（加拿大）白俄羅斯：（utc + 03：00）明斯克比利時：（UTC + 01：00）布魯塞爾，哥本哈根，馬德里，巴黎繁體中文：（UTC-06:00）中部時間（美國 & 加拿大）貝南：（UTC + 01：00）West Central 非洲百慕達：（UTC-04:00）大西洋時間（加拿大）不丹：（UTC + 06：00）達卡委內瑞拉共和國：（UTC-04:00）卡拉卡斯玻利維亞：（UTC-04:00）喬治城，拉巴斯，瑪瑙斯，San Juan 波奈，聖歇斯和沙巴：（UTC-04:00）大西洋時間（加拿大）波士尼亞和黑塞哥維那：（UTC + 01：00）塞拉耶佛，斯高彼亞，華沙，札格雷布博茨瓦納：（UTC + 01：00） West Central 非洲布威島：（UTC + 00：00）蒙羅維亞，雷克雅維克巴西：（UTC-03:00）巴西利亞英屬印度海運地區：（UTC + 06：00）達卡英屬維爾京群島：（UTC-04:00）大西洋時間（加拿大）汶萊：（UTC + 08：00） Irkutsk 保加利亞：（utc + 02：00）奇西瑙布吉納法索：（utc + 02：00）蒙羅維亞，皮托裡 CÃ́te：（UTC + 00：00）蒙羅維亞，雷克雅維克柬埔寨：（UTC + 07：00）曼谷，河內，雅加達喀麥隆：（UTC + 01：00） West Central 非洲加拿大：（UTC-05:00）印度東部時間（美國 & 加拿大）維德角：（UTC-01:00）維德角。
Cayman Islands: (UTC-05:00) Eastern Time (US & Canada) Central African Republic: (UTC+01:00) West Central Africa Chad: (UTC+01:00) West Central Africa Chile: (UTC-04:00) Santiago China: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Christmas Island: (UTC+07:00) Krasnoyarsk Cocos (Keeling) Islands: (UTC+06:30) Yangon (Rangoon) Colombia: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Comoros: (UTC+03:00) Nairobi Congo: (UTC+01:00) West Central Africa Congo (DRC): (UTC+01:00) West Central Africa Cook Islands: (UTC-10:00) Hawaii Costa Rica: (UTC-06:00) Central Time (US & Canada) Croatia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb CuraÃ§ao: (UTC-04:00) Cuiaba Cyprus: (UTC+02:00) Chisinau Czech Republic: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Denmark: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Djibouti: (UTC+03:00) Nairobi Dominica: (UTC-04:00) Atlantic Time (Canada) Dominican Republic: (UTC-04:00) Atlantic Time (Canada) Ecuador: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Egypt: (UTC+02:00) Chisinau El Salvador: (UTC-06:00) Central Time (US & Canada) Equatorial Guinea: (UTC+01:00) West Central Africa Eritrea: (UTC+03:00) Nairobi Estonia: (UTC+02:00) Chisinau Ethiopia: (UTC+03:00) Nairobi Falkland Islands (Islas Malvinas): (UTC-04:00) Santiago Faroe Islands: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Fiji: (UTC+12:00) Fiji Finland: (UTC+02:00) Helsinki, Kyiv, Riga, Sofia, Tallinn, Vilnius France: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris French Guiana: (UTC-03:00) Cayenne, Fortaleza French Polynesia: (UTC-10:00) Hawaii French Southern and Antarctic Lands: (UTC+05:00) Ashgabat, Tashkent Gabon: (UTC+01:00) West Central Africa Gambia, The: (UTC+00:00) Monrovia, Reykjavik Georgia: (UTC-05:00) Eastern Time (US & Canada) Germany: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Ghana: (UTC+00:00) Monrovia, Reykjavik Gibraltar: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Greece: (UTC+02:00) Athens, Bucharest Greenland: (UTC+00:00) Monrovia, Reykjavik Grenada: (UTC-04:00) Atlantic Time (Canada) Guadeloupe: (UTC-04:00) Atlantic Time (Canada) Guam: (UTC+10:00) Guam, Port Moresby Guatemala: (UTC-06:00) Central Time (US & Canada) Guernsey: (UTC+00:00) Monrovia, Reykjavik Guinea: (UTC+00:00) Monrovia, Reykjavik Guinea-Bissau: (UTC+00:00) Monrovia, Reykjavik Guyana: (UTC-04:00) Atlantic Time (Canada) Haiti: (UTC-05:00) Eastern Time (US & Canada) Heard Island and McDonald Islands: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Holy See (Vatican City): (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Honduras: (UTC-06:00) Central Time (US & Canada) Hong Kong SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Hungary: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Iceland: (UTC+00:00) Monrovia, Reykjavik India: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Indonesia: (UTC+07:00) Bangkok, Hanoi, Jakarta Iraq: (UTC+04:00) Abu Dhabi, Muscat Ireland: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Israel: (UTC+02:00) Jerusalem Italy: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Jamaica: (UTC-05:00) Eastern Time (US & Canada) Japan: (UTC+09:00) Osaka, Sapporo, Tokyo Jersey: (UTC+00:00) Monrovia, Reykjavik Jordan: (UTC+02:00) Chisinau Kazakhstan: (UTC+05:00) Ashgabat, Tashkent Kenya: (UTC+03:00) Nairobi Kiribati: (UTC+14:00) Kiritimati Island Korea: (UTC+09:00) Seoul Kuwait: (UTC+04:00) Abu Dhabi, Muscat Kyrgyzstan: (UTC+06:00) Astana Laos: (UTC+07:00) Bangkok, Hanoi, Jakarta Latvia: (UTC+02:00) Chisinau Lebanon: (UTC+02:00) Chisinau Lesotho: (UTC+02:00) Harare, Pretoria Liberia: (UTC+00:00) Monrovia, Reykjavik Libya: (UTC+02:00) Chisinau Liechtenstein: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Lithuania: (UTC+02:00) Chisinau Luxembourg: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Macao SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Macedonia, FYROM: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Madagascar: (UTC+03:00) Nairobi Malawi: (UTC+02:00) Harare, Pretoria Malaysia: (UTC+08:00) Kuala Lumpur, Singapore Maldives: (UTC+05:00) Ashgabat, Tashkent Mali: (UTC+00:00) Monrovia, Reykjavik Malta: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Man, Isle of: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Marshall Islands: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Martinique: (UTC-04:00) Atlantic Time (Canada) Mauritania: (UTC+00:00) Monrovia, Reykjavik Mauritius: (UTC+04:00) Port Louis Mayotte: (UTC+03:00) Nairobi Mexico: (UTC-06:00) Guadalajara, Mexico City, Monterrey Micronesia: (UTC+10:00) Guam, Port Moresby Moldova: (UTC+02:00) Chisinau Monaco: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Mongolia: (UTC+07:00) Krasnoyarsk Montenegro: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Montserrat: (UTC-04:00) Atlantic Time (Canada) Morocco: (UTC+01:00) Casablanca Mozambique: (UTC+02:00) Harare, Pretoria Myanmar: (UTC+06:30) Yangon (Rangoon) Namibia: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Nauru: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Nepal: (UTC+05:45) Kathmandu Netherlands: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna New Caledonia: (UTC+11:00) Solomon Is., New Caledonia New Zealand: (UTC+12:00) Auckland, Wellington Nicaragua: (UTC-06:00) Central Time (US & Canada) Niger: (UTC+01:00) West Central Africa Nigeria: (UTC+01:00) West Central Africa Niue: (UTC+13:00) Samoa Norfolk Island: (UTC+11:00) Solomon Is., New Caledonia Northern Mariana Islands: (UTC+10:00) Guam, Port Moresby Norway: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Oman: (UTC+04:00) Abu Dhabi, Muscat Pakistan: (UTC+05:00) Islamabad, Karachi Palau: (UTC+09:00) Osaka, Sapporo, Tokyo Palestinian Authority: (UTC+02:00) Chisinau Panama: (UTC-05:00) Eastern Time (US & Canada) Papua New Guinea: (UTC+10:00) Vladivostok Paraguay: (UTC-04:00) Asuncion Peru: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Philippines: (UTC+08:00) Kuala Lumpur, Singapore Pitcairn Islands: (UTC-08:00) Pacific Time (US & Canada) Poland: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Portugal: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Qatar: (UTC+04:00) Abu Dhabi, Muscat Reunion: (UTC+04:00) Port Louis Romania: (UTC+02:00) Chisinau ROW: (UTC-07:00) Mountain Time (US & Canada) Russia: (UTC+03:00) Moscow, St. Petersburg Rwanda: (UTC+02:00) Harare, Pretoria SÃ£o TomÃ© and PrÃ­ncipe: (UTC+00:00) Monrovia, Reykjavik Saint BarthÃ©lemy: (UTC+04:00) Yerevan Saint Helena, Ascension and Tristan da Cunha: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Saint Kitts and Nevis: (UTC-04:00) Atlantic Time (Canada) Saint Lucia: (UTC-04:00) Atlantic Time (Canada) Saint Martin (French Part): (UTC-04:00) Atlantic Time (Canada) Saint Pierre and Miquelon: (UTC-02:00) Mid-Atlantic - Old Saint Vincent and the Grenadines: (UTC-04:00) Atlantic Time (Canada) Samoa: (UTC+13:00) Samoa San Marino: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Saudi Arabia: (UTC+03:00) Kuwait, Riyadh Senegal: (UTC+00:00) Monrovia, Reykjavik Serbia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Seychelles: (UTC+04:00) Abu Dhabi, Muscat Sierra Leone: (UTC+00:00) Monrovia, Reykjavik Singapore: (UTC+08:00) Kuala Lumpur, Singapore Sint Maarten (Dutch Part): (UTC-04:00) Atlantic Time (Canada) Slovakia: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Slovenia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Solomon Islands: (UTC+11:00) Solomon Is., New Caledonia Somalia: (UTC+03:00) Nairobi South Africa: (UTC+02:00) Harare, Pretoria South Georgia and the South Sandwich Islands: (UTC-02:00) Mid-Atlantic - Old Spain: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Sri Lanka: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Suriname: (UTC-03:00) Cayenne, Fortaleza Svalbard and Jan Mayen: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Swaziland: (UTC+02:00) Harare, Pretoria Sweden: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Switzerland: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Taiwan: (UTC+08:00) Taipei Tajikistan: (UTC+05:00) Ashgabat, Tashkent Tanzania: (UTC+03:00) Nairobi Thailand: (UTC+07:00) Bangkok, Hanoi, Jakarta Timor-Leste: (UTC+09:00) Seoul Togo: (UTC+00:00) Monrovia, Reykjavik Tokelau: (UTC+13:00) Nuku'alofa Tonga: (UTC+13:00) Nuku'alofa Trinidad and Tobago: (UTC-04:00) Atlantic Time (Canada) Tunisia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Turkey: (UTC+03:00) Istanbul Turkmenistan: (UTC+05:00) Ashgabat, Tashkent Turks and Caicos Islands: (UTC-05:00) Eastern Time (US & Canada) Tuvalu: (UTC+12:00) Petropavlovsk-Kamchatsky - Old U.S. Minor Outlying Islands: (UTC+13:00) Samoa U.S. Virgin Islands: (UTC-04:00) Atlantic Time (Canada) Uganda: (UTC+03:00) Nairobi Ukraine: (UTC+02:00) Chisinau United Arab Emirates: (UTC+04:00) Abu Dhabi, Muscat United Kingdom: (UTC+00:00) Dublin, Edinburgh, Lisbon, London United States: (UTC-05:00) Eastern Time (US & Canada) Uruguay: (UTC-03:00) Brasilia Uzbekistan: (UTC+05:00) Ashgabat, Tashkent Vanuatu: (UTC+11:00) Solomon Is., New Caledonia Vietnam: (UTC+07:00) Bangkok, Hanoi, Jakarta Wallis and Futuna: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Western Sahara (Disputed): (UTC+00:00) Dublin, Edinburgh, Lisbon, London Yemen: (UTC+04:00) Abu Dhabi, Muscat Zambia: (UTC+02:00) Harare, Pretoria Zimbabwe: (UTC+02:00) Harare, Pretoria
