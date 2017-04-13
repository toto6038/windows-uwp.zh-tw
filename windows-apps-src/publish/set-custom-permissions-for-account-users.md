---
author: jnHs
Description: "設定帳戶使用者的自訂權限。"
title: "設定帳戶使用者的自訂權限"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 2ce4ddc5240281618fefa16587067c4ad9382b2e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-custom-permissions-for-account-users"></a>設定帳戶使用者的自訂權限

當您新增使用者到您的帳戶時，您可以提供他們[標準角色](manage-account-users.md#roles-and-permissions)，或是選擇自訂他們的權限，為使用者提供適當層級的存取權。 其中的某些權限適用於整個帳戶，某些權限的授與可針對所有產品或限制為特定產品。 

新增或編輯使用者帳戶時，若要使用自訂權限而非標準角色，請按一下 **\[角色\]** 區段中的 **\[自訂權限\]**。 

> **注意** 不論您是新增使用者、群組或 Azure AD 應用程式，都適用相同的權限。

若要啟用使用者的權限，請切換方塊到適當的設定。 

![存取權設定指南](images/permission_key.png)

- **無存取權**：使用者將不具有指示的權限。
- **唯讀**：使用者有權檢視指示區域相關的功能，但無法進行變更。
- **讀/寫**：使用者有權進行區域相關的變更和檢視。
- **混合式**︰您無法直接選取這個選項，但如果您允許該權限有存取權組合，則會顯示**混合式**指示器。 例如，如果您針對**所有產品**的**定價和可用性**授與**唯讀**存取權，然後針對特定產品授與**定價和可用性**的**讀/寫**存取權，則**所有產品**的**定價和可用性**指示器會顯示為「混合式」。 如果某些產品具有**無存取權** 的權限，但其他產品具有**讀/寫**和/或**唯讀**存取權，則情況一樣。

針對某些權限 (例如和檢視分析資料相關的權限)，您只能授與**唯讀**存取權。 請注意，在目前的實作中，有些權限沒有區別**唯讀**和**讀/寫**存取權。 請檢閱每個權限的詳細資料，以了解**唯讀**和**讀/寫**存取權賦予的特定能力。

下表說明每個權限相關的特定詳細資料。

## <a name="account-level-permissions"></a>帳戶層級權限

本節中的權限不限特定產品。 授與這些權限的存取權可讓使用者在整個帳戶擁有該項權限。

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">權限名稱</th>
    <th align="left">唯讀</th>
    <th align="left">讀/寫</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    **帳戶設定**                    </td><td align="left">  可檢視 **\[帳戶設定\]** 區段中的所有頁面，包括[連絡資訊](managing-your-profile.md)。       </td><td align="left">  可檢視 **\[帳戶設定\]** 區段中的所有頁面。 可變更[連絡資訊](managing-your-profile.md)和其他頁面，但不能變更支付帳戶或稅金設定檔 (除非另外授與該權限)。            </td></tr>
<tr><td align="left">    **帳戶使用者**                       </td><td align="left">  可檢視已加入 **\[管理使用者\]** 區段中的帳戶的使用者。          </td><td align="left">  可在 **\[管理使用者\]** 區段中新增使用者到帳戶和變更現有使用者。             </td></tr>
<tr><td align="left">    **帳戶層級的廣告績效報告** </td><td align="left">  可檢視帳戶層級的[廣告績效報告](advertising-performance-report.md#account-level-advertising-performance-report)。 (無法檢視個別產品的廣告績效報告，除非另外授與該項權限。)       </td><td align="left">  不適用   </td></tr>
<tr><td align="left">    **廣告活動**                        </td><td align="left">  可檢視帳戶中建立的[廣告活動](create-an-ad-campaign-for-your-app.md)。      </td><td align="left">  可建立、管理和檢視帳戶中建立的[廣告活動](create-an-ad-campaign-for-your-app.md)。          </td></tr>
<tr><td align="left">    **廣告流量分配**                        </td><td align="left">  可檢視帳戶中所有產品的[廣告流量分配設定](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx)。    </td><td align="left">  可檢視和變更帳戶中所有產品的[廣告流量分配設定](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx)。        </td></tr>
<tr><td align="left">    **廣告流量分配報告**                </td><td align="left">  可檢視帳戶中所有產品的[廣告流量分配報告](ad-mediation-report.md)。    </td><td align="left">  不適用    </td></tr>
<tr><td align="left">    **廣告績效報告**              </td><td align="left">  可檢視帳戶中所有產品的[廣告績效報告](advertising-performance-report.md)。 (無法檢視帳戶層級的[廣告績效報告](advertising-performance-report.md#account-level-advertising-performance-report)，除非另外授與該項權限。)       </td><td align="left">  可檢視帳戶中所有產品的[廣告績效報告](advertising-performance-report.md)。 (無法檢視帳戶層級的[廣告績效報告](advertising-performance-report.md#account-level-advertising-performance-report)，除非另外授與該項權限。)         </td></tr>
<tr><td align="left">    **廣告單位**                            </td><td align="left">  可檢視已為帳戶建立的[廣告單位](monetize-with-ads.md)。    </td><td align="left">  可建立、管理及檢視帳戶的[廣告單位](monetize-with-ads.md)。             </td></tr>
<tr><td align="left">    **聯盟廣告**                       </td><td align="left">  可檢視帳戶中所有產品的[聯盟廣告](about-affiliate-ads.md)使用量。    </td><td align="left">  可管理和檢視帳戶中所有產品的[聯盟廣告](about-affiliate-ads.md)使用量。                </td></tr>
<tr><td align="left">    **聯盟績效報告**      </td><td align="left">  可檢視帳戶中所有產品的[聯盟績效報告](affiliates-performance-report.md)。   </td><td align="left">  不適用   </td></tr>
<tr><td align="left">    **App 安裝廣告報告**             </td><td align="left">  可檢視帳戶中所有產品的 [App 安裝廣告報告](app-install-ads-reports.md)。           </td><td align="left">  不適用   </td></tr>
<tr><td align="left">    **社群廣告**                       </td><td align="left">  可檢視帳戶中所有產品的[社群廣告](about-community-ads.md)使用量。          </td><td align="left">  可建立、管理和檢視帳戶中所有產品的免費[社群廣告](about-community-ads.md)使用量。               </td></tr>
<tr><td align="left">    **連絡資訊**                        </td><td align="left">  可檢視 \[帳戶設定\] 區段中的[連絡資訊](managing-your-profile.md)。        </td><td align="left">  可編輯和檢視 \[帳戶設定\] 區段中的[連絡資訊](managing-your-profile.md)。            </td></tr>
<tr><td align="left">    **COPPA 規範**                    </td><td align="left">  可檢視帳戶中所有產品的 [COPPA 規範](monetize-with-ads.md#coppa-compliance)選取項目 (指示產品的目標對象是否為 13 歲以下的兒童)。                                            </td><td align="left">  可編輯和檢視帳戶中所有產品的 [COPPA 規範](monetize-with-ads.md#coppa-compliance)選取項目 (指示產品的目標對象是否為 13 歲以下的兒童)。         </td></tr>
<tr><td align="left">    **客戶群組**                     </td><td align="left">  可檢視 **\[客戶\]** 區段中的[客戶群組](create-customer-groups.md) (區隔和正式發行前小眾測試版群組)。      </td><td align="left">  可建立、編輯和檢視 **\[客戶\]** 區段中的[客戶群組](create-customer-groups.md) (區隔和正式發行前小眾測試版群組)。       </td></tr>
<tr><td align="left">    **新的 app**                            </td><td align="left">  可檢視新的 app 建立頁面，但實際上不能在帳戶中建立新的 app。    </td><td align="left">  可透過保留新的 app 名稱以在帳戶中[建立新的 app](create-your-app-by-reserving-a-name.md)，並可建立提交內容，將 app 提交到市集。     </td></tr>
<tr><td align="left">    **新的套件組合**&nbsp;*                       </td><td align="left">  可檢視新的套件組合建立頁面，但實際上不能在帳戶中建立新的套件組合。     </td><td align="left">  可建立新的產品套件組合。          </td></tr>
<tr><td align="left">    **合作夥伴服務**&nbsp;*                  </td><td align="left">  可檢視安裝到服務以擷取 XTokens 的憑證。     </td><td align="left">  可管理和檢視安裝到服務以擷取 XTokens 的憑證。       </td></tr>
<tr><td align="left">    **支付帳戶**                      </td><td align="left">  可檢視 **\[帳戶設定\]** 中的[支付帳戶資訊](setting-up-your-payout-account-and-tax-forms.md#payout-account)。     </td><td align="left">  可編輯和檢視 **\[帳戶設定\]** 中的[支付帳戶資訊](setting-up-your-payout-account-and-tax-forms.md#payout-account)。       </td></tr>
<tr><td align="left">    **支付摘要**                      </td><td align="left">  可檢視[支付摘要](payout-summary.md)以存取和下載支付報告資訊。       </td><td align="left">  可檢視[支付摘要](payout-summary.md)以存取和下載支付報告資訊。   </td></tr>
<tr><td align="left">    **信賴憑證者**&nbsp;*                   </td><td align="left">  可檢視信賴憑證者以擷取 XTokens。    </td><td align="left">  可管理和檢視信賴憑證者以擷取 XTokens。     </td></tr>
<tr><td align="left">    **沙箱**&nbsp;*                         </td><td align="left">  可存取 **\[沙箱\]** 頁面，和檢視帳戶中的沙箱與這些沙箱適用的任何設定。 除非被授與適當的產品層級權限，否則無法檢視每個沙箱的產品和提交內容。 </td><td align="left">  可存取 **\[沙箱\]** 頁面及檢視和管理帳戶中的沙箱，包括建立和刪除沙箱及管理其設定。 除非被授與適當的產品層級權限，否則無法檢視每個沙箱的產品和提交內容。    </td></tr>
<tr><td align="left">    **稅金設定檔**                         </td><td align="left">  可檢視 **\[帳戶設定\]** 中的[稅金設定檔資訊和表單](setting-up-your-payout-account-and-tax-forms.md#tax-forms)。     </td><td align="left">  可在 **\[帳戶設定\]** 中填寫稅單並更新[稅金設定檔資訊](setting-up-your-payout-account-and-tax-forms.md#tax-forms)。     </td></tr>
<tr><td align="left">    **測試帳戶**&nbsp;*                     </td><td align="left">  可檢視用於測試 Xbox Live 設定的帳戶。      </td><td align="left">  可建立、管理和檢視用於測試 Xbox Live 設定的帳戶。      </td></tr>
<tr><td align="left">    **Xbox 裝置**                        </td><td align="left">  可在 **\[帳戶設定\]** 區段中檢視為帳戶啟用的 Xbox 開發主機。       </td><td align="left">  可在 **\[帳戶設定\]** 區段中新增、移除和檢視為帳戶啟用的 Xbox 開發主機 。     </td></tr>
    </tbody>
    </table>

\ * 標示星號 (*) 的權限會授與並非適用所有帳戶之功能的存取權。 如果您的帳戶尚未啟用這些功能，您對於這些權限所做的選擇不會有任何影響。   

## <a name="product-level-permissions"></a>產品層級的權限

本節中的權限可授與帳戶中的所有產品，或可自訂為只允許一或多個特定產品的權限。 這些權限分成四種類別︰**分析**、**創造營收**、**發佈**和 **Xbox Live**。 您可以展開這些類別，以檢視每個類別中的個別權限。 

若要為帳戶中的所有產品授與權限，請在標示 **\[所有產品\]** 的列中針對該權限做選擇 (切換方塊以指示**唯讀**、**讀/寫**或**無存取權**)。 
 
> **提示**：對 **\[所有產品\]** 做的選擇將適用於目前帳戶中的每個產品，以及未來在帳戶建立的任何產品。

在 **\[所有產品\]** 列下方，您會看到帳戶中的每個產品列在不同的列中。 若只要授與特定產品的權限，請在該產品的列中針對該權限做選擇。

除了 **\[所有附加元件\]** 列外，每個附加元件會列在其父產品下方的個別列中。 針對 **\[所有附加元件\]** 所做的選擇將適用於該產品所有目前的附加元件，以及未來為該產品建立的任何附加元件。

請注意，附加元件無法設定一部分的權限。 這是因為它們不適用於附加元件 (例如**客戶回函**權限)，或因為在父產品層級授與的權限適用於該產品的所有附加元件 (例如**促銷碼**)。 但請注意，附加元件適用的任何權限都必須個別設定；附加元件不會繼承為父產品所做的選擇。 例如，如果您想允許使用者選擇附加元件的定價和可用性，則無論是否已授與父產品的**定價和可用性**權限，您都必須啟用附加元件 (或**所有附加元件**) 的**定價和可用性**權限。 

### <a name="analytics"></a>分析

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀/寫</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **下載數**     </td><td>    可檢視產品的[下載數](acquisitions-report.md)和[附加元件下載數](add-on-acquisitions-report.md)報告。        </td><td>    不適用    </td><td>    不適用 (父項產品的設定包含附加元件下載數報告)        </td><td>    不適用                         </td></tr>
    <tr><td align="left">    **使用量** </td><td>    可檢視產品的[使用量報告](usage-report.md)。     </td><td>    不適用       </td><td>    N/A     </td><td>    不適用         </td></tr>
    <tr><td align="left">    **健康情況** </td><td>    可檢視產品的[健康情況報告](health-report.md)。    </td><td>    不適用     </td><td>    N/A     </td><td>    不適用         </td></tr>
    <tr><td align="left">    **客戶回函**    </td><td>    可檢視產品的[分級](ratings-report.md)、[評論](reviews-report.md)及[回函](feedback-report.md)報告。       </td><td>    不適用 (以回應回函或評論，必須授與**連絡客戶**權限)   </td><td>    不適用     </td><td>    不適用         </td></tr>
    <tr><td align="left">    **Xbox 分析** </td><td>    可檢視產品的 Xbox 分析報告。 (注意︰這份報告尚未提供。)    </td><td>    不適用   </td><td>    N/A       </td><td>    不適用          </td></tr>
    <tr><td align="left">    **即時**   </td><td>    可檢視產品的即時報告。       </td><td>    不適用   </td><td>    N/A     </td><td>    不適用                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>創造營收

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀/寫</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **連絡客戶**  </td><td>    只要一併授與**客戶回函**權限，即可檢視[對客戶回函的回應](respond-to-customer-feedback.md)和[對客戶評論的回應](respond-to-customer-reviews.md)。 也可檢視已為產品建立的[特定對象的通知](send-push-notifications-to-your-apps-customers.md)。    </td><td>    只要一併授與**客戶回函**權限，即可[回應客戶回函](respond-to-customer-feedback.md)和[回應客戶評論](respond-to-customer-reviews.md)。 也可為產品[建立和傳送特定對象的通知](send-push-notifications-to-your-apps-customers.md)。                   </td><td>    不適用         </td><td>    不適用                          </td></tr>
    <tr><td align="left">    **實驗**</td><td>    可檢視 [實驗 (A/B 測試)](../monetize/run-app-experiments-with-a-b-testing.md) 及檢視產品的實驗資料。   </td><td>    可建立、管理及檢視產品的 [實驗 (A/B 測試)](../monetize/run-app-experiments-with-a-b-testing.md) 及檢視實驗資料。     </td><td>    不適用  </td><td>    不適用                 </td></tr>
    <tr><td align="left">    **促銷碼**     </td><td>    可檢視產品和其附加元件的[促銷碼](generate-promotional-codes.md)訂單與使用量資訊，並可檢視使用量資訊。         </td><td>    可檢視、管理及建立產品及其附加元件的[促銷碼](generate-promotional-codes.md)訂單，並可檢視使用量資訊。          </td><td>    不適用 (父產品的設定適用於所有附加元件)     </td><td>    不適用 (父產品的設定適用於所有附加元件)     </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>發佈 

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀/寫</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **定價和可用性**  </td><td>    可檢視產品提交的[定價和可用性](set-app-pricing-and-availability.md)頁面。     </td><td>    可檢視和編輯產品提交的[定價和可用性](set-app-pricing-and-availability.md)頁面。 </td><td>    可檢視附加元件提交的[定價和可用性](set-add-on-pricing-and-availability.md)頁面。   </td><td>    可檢視和編輯附加元件提交的[定價和可用性](set-add-on-pricing-and-availability.md)頁面。          </td></tr>
    <tr><td align="left">    **屬性**   </td><td>    可檢視產品提交的[屬性](enter-app-properties.md)頁面。      </td><td>    可檢視和編輯產品提交的[屬性](enter-app-properties.md)頁面。       </td><td>    可檢視附加元件提交的[屬性](enter-add-on-properties.md)頁面。     </td><td>    可檢視和編輯附加元件提交的[屬性](enter-add-on-properties.md)頁面。               </td></tr>
    <tr><td align="left">    **年齡分級**    </td><td>    可檢視產品提交的[年齡分級](age-ratings.md)頁面。       </td><td>    可檢視和編輯產品提交的[年齡分級](age-ratings.md)頁面。    </td><td>    * 可檢視附加元件提交的 \[年齡分級\] 頁面。          </td><td>    * 可檢視和編輯附加元件提交的 \[年齡分級\] 頁面。       </td></tr>
    <tr><td align="left">    **套件**        </td><td>    可檢視產品提交的[套件](upload-app-packages.md)頁面。  </td><td>    可檢視和編輯產品提交的[套件](upload-app-packages.md)頁面，包括上傳套件。     </td><td>    * 可檢視附加元件提交的裝置系列銷售對象和套件 (如果適用)。   </td><td>    * 可檢視和編輯附加元件提交的裝置系列銷售對象，包括上傳套件 (如果適用)。             </td></tr>
    <tr><td align="left">    **市集清單**  </td><td>    可檢視產品提交的[市集清單頁面](create-app-store-listings.md)。  </td><td>    可檢視和編輯產品提交的[市集清單頁面](create-app-store-listings.md)，並可新增不同語言的市集清單。     </td><td>    可檢視附加元件提交的[市集清單頁面](create-add-on-store-listings.md)。            </td><td>    可檢視和編輯附加元件提交的[市集清單頁面](create-add-on-store-listings.md)，並可新增不同語言的市集清單。                 </td></tr>
    <tr><td align="left">    **市集提交**     </td><td>    若此權限設為唯讀，則不會授與任何存取權。           </td><td>    可將產品提交到市集和檢視認證報告。 包含新的與更新的提交。 </td><td>若此權限設為唯讀，則不會授與任何存取權。     </td><td>    可將附加元件提交到市集和檢視認證報告。 包含新的與更新的提交。</td></tr>
    <tr><td align="left">    **建立新的提交**       </td><td>    若此權限設為唯讀，則不會授與任何存取權。        </td><td>    可為產品建立新的[提交](app-submissions.md)。  </td><td>    若此權限設為唯讀，則不會授與任何存取權。   </td><td>    可為附加元件建立新的[提交](add-on-submissions.md)。        </td></tr>
    <tr><td align="left">    **新的附加元件**    </td><td>    若此權限設為唯讀，則不會授與任何存取權。 </td><td>    可為產品[建立新的附加元件](set-your-add-on-product-id.md)。 </td><td>    不適用    </td><td>    不適用        </td></tr>
    <tr><td align="left">    **名稱保留值**   </td><td>    可檢視產品的[管理 app 名稱](manage-app-names.md)頁面。</td><td>    可檢視和編輯產品的[管理 app 名稱](manage-app-names.md)頁面，包括保留其他名稱及刪除保留名稱。 </td><td>   * 可檢視附加元件的保留名稱。    </td><td>   * 可檢視和編輯附加元件的保留名稱。          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀/寫</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Xbox 服務設定**&nbsp;\*    </td><td>    可檢視產品中與成就、多人遊戲、排行榜和其他 Xbox Live 設定相關的設定。  </td><td>    可檢視和編輯產品中與成就、多人遊戲、排行榜和其他 Xbox Live 設定相關的設定。  </td><td>    不適用     </td><td>    不適用                      </td></tr>
    <tr><td align="left">    **App 頻道**&nbsp;\*</td><td>    不適用  </td><td>    可將宣傳影片頻道發佈到 Xbox 主機，以透過OneGuide 檢視。  </td><td>  不適用 </td><td> 不適用 </td></tr>
</tbody>
</table>

\ * 標示星號 (*) 的權限會授與並非適用所有帳戶之功能的存取權。 如果您的帳戶尚未啟用這些功能，您對於這些權限所做的選擇不會有任何影響。  