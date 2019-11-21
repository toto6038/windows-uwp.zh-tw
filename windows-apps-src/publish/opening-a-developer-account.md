---
ms.assetid: 284EBA1F-BFB4-4CDA-9F05-4927CDACDAA7
title: 開啟開發人員帳戶
description: Here's an overview of how to register for a Windows developer account for Microsoft Store and other Microsoft programs in Partner Center.
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d00fcee11b8cf813144a6f8ea021dc40829056d2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259955"
---
# <a name="opening-a-developer-account"></a>開啟開發人員帳戶

This article describes how to register for a Windows developer account in [Partner Center](https://partner.microsoft.com/dashboard).

> [!NOTE]
> When you sign up for a developer account, we'll use the email address you provide in your contact info to send messages related to your account. At times, these may include information about our programs. If you choose to [opt out](https://account.microsoft.com/account/Account?ru=https%3A%2F%2Faccount.microsoft.com%2Fprofile%2Fcontact-info&destrt=profile-landing) of these informational emails, be aware that we'll still send you transactional messages (for example, to let you know that your app has passed certification or that a payment is on the way). These transactional emails are a necessary part of your account, and unless you close your account, you'll continue to receive them.

## <a name="the-account-signup-process"></a>帳戶註冊程序

> [!NOTE]
> In some cases, the screens and fields you see when you register for a developer account may vary slightly from what's outlined in the following steps. But the basic information and process will match what these steps describe.

1.  Go to the [registration page](https://developer.microsoft.com/store/register) and select **Sign up**.
2.  如果您尚未使用 Microsoft 帳戶登入，請立即登入，或建立新的 Microsoft 帳戶。 The Microsoft account you use here is what you'll use to sign in to your developer account.
3.  Select the [country/region](account-types-locations-and-fees.md#developer-account-and-app-submission-markets) where you live or where your business is located. 之後將無法變更此設定。
4.  選取您的[開發人員帳戶類型](account-types-locations-and-fees.md) (個人或公司)。 您稍後將無法變更此設定，因此請務必選擇正確的帳戶類型。
5.  Enter the **publisher display name** that you want to use (50 characters or fewer). 請謹慎選取此名稱，因為客戶會在瀏覽時看到此名稱，並透過此名稱認識您的 app。 若是公司帳戶，請輸入組織的註冊公司名稱或商號。 If you enter a name that someone else has already selected, or if someone else has the rights to use that name, we won't permit you to use it.

    > [!NOTE]
    > 請確定您有權使用在此輸入的名稱。 如果其他人已經擁有您挑選之名稱的商標或著作權，您的帳戶會被關閉。 See [App Developer Agreement](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) for more info. 如果其他人使用了您所擁有之商標或其他法定權利的發行者顯示名稱，請[連絡 Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html)。    

6.  輸入您要為開發人員帳戶使用的連絡資訊。

    > [!NOTE]
    > 我們將使用這個資訊與您連絡有關帳戶的事項。 For example, you'll receive an email confirmation after you complete your registration. After that, we'll send messages when we pay you or if you need to fix something with your account. We may also send informational emails as described earlier, unless you opt out of receiving non-transactional emails.

    If you're registering as a company, you'll also need to enter the name, email address, and phone number of the person who will approve your company's account.

7.  Select **Next** to move on to the **Payment** section.

8.  輸入付款資訊以支付一次性註冊費用。 如果您有涵蓋註冊費用的促銷碼，可以在此輸入。 Otherwise, provide your credit card info (or PayPal info in supported markets). 請注意，此購買無法使用預付卡。 When you're finished, select **Next** to move on to the **Review** screen.

9.  請檢閱您的帳戶資訊，並確認所有項目正確。 接著，請閱讀並接受[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)的條款及條件。 Check the box to indicate you've read these terms and accept them.

10.  Select **Finish** to confirm your registration. Your payment will be processed and we'll send a confirmation message to your email address.

After you've signed up, your account will go through verification. For individual accounts, we check to make sure another user isn't already using your publisher display name. 若是公司帳戶，這個程序需要較長的時間，因為我們必須確定您有權代替您的公司設定帳戶。 This verification can take from a few days to a couple of weeks, and it often includes a phone call to your company. 您可以在 **\[帳戶設定\]** 頁面查看您的驗證狀態。


## <a name="additional-guidelines-for-company-accounts"></a>公司帳戶的其他指導方針

> [!IMPORTANT]
> To allow multiple users to access your developer account, we recommend using Azure Active Directory (Azure AD) to assign roles to individual users instead of sharing access to the Microsoft account. Each user can then access the developer account by signing in to Partner Center with their individual Azure AD credentials. 如需詳細資訊，請參閱[管理帳戶使用者](manage-account-users.md)。

If you want to let multiple people access the company account by signing in with the Microsoft account that opened it (instead of as individual users added to the account), see the following guidelines:

-   Create the Microsoft account by using an email address that doesn't already belong to you or another individual, such as MyCompany_PartnerCenter@outlook.com. Don't use an email address at your company's domain, particularly if your company already uses Azure AD. As noted earlier, you can add additional users from your company's Azure AD service later.
-   Limit access to this Microsoft account to the least number of users possible.
-   Set up a corporate email distribution list that includes everyone who needs to access the developer account. Add this email address to the [security info associated with the Microsoft account](https://account.microsoft.com/security). This approach allows all the employees on the list to receive security codes that are sent to this alias. If setting up a distribution list isn't feasible, you can add an individual's email address to your security info. But, the owner of that email address will be the only person who can access and share the security code when prompted (such as when new security info is added to the account or when the account is accessed from a new device).
-   Add a company phone number to the Microsoft account's security info. Try to use a number that doesn't require an extension and that's accessible to key team members.
-   Encourage developers to use [trusted devices](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device) to sign in to your company's developer account. 所有主要小組成員都必須擁有這些信任的裝置的存取權。 This arrangement reduces the need for security codes to be sent when team members access the account. There's a limit to the number of codes that can be generated per account per week.
-   如果您需要允許從非信任的電腦存取帳戶，請將可存取的開發人員人數限制為最多 5 位。 在理想的情況下，這些開發人員應該從位於相同地理和網路位置的電腦存取帳戶。
-   Frequently review your company’s security info at https://account.microsoft.com/security to make sure it's all current.


## <a name="microsoft-account-security"></a>Microsoft account security

我們會使用您提供的安全性資訊，將您的 Microsoft 帳戶與多種形式的證明建立關聯，藉此提高帳戶的安全性等級。 這樣可以提高未經授權就存取您 Microsoft 帳戶 (以及開發人員帳戶) 的難度。 Also, if you ever forget your password or if someone else tries to access your account, we’ll be able to reach you to confirm ownership and/or re-establish appropriate control of your account.

You must have at least two email addresses or phone numbers on your Microsoft account. 建議您新增的越多越好。 請記住，部分安全性資訊需要經過確認才能生效。 Also, make sure to review your security info frequently and verify that it's up to date. You can manage your security info by going to https://account.microsoft.com/security and signing in with your Microsoft account. For more info, see [Account security info & verification codes](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes).

When you sign in to Partner Center through your Microsoft account, the system may prompt you to verify your identity by sending a security code, which you must enter to complete the sign-in process. We recommend designating PCs that you use frequently as *trusted devices*. When you sign in from a trusted device, you typically aren't prompted for a code, although you may occasionally be prompted in specific situations or if you haven’t signed in on that device in a long time. For more info, see [Add a trusted device to your Microsoft account](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device).


## <a name="closing-your-account"></a>關閉您的帳戶

Developer accounts don't expire, so there's no need to renew your account in order to keep it open. 如果您決定完全關閉您的帳戶，您可以連絡支援人員來執行此動作。

當您關閉您的帳戶時，請務必了解您已在 Microsoft Store 中發行的任何應用程式會發生什麼事：

-   Your app's current customers can still use the app. However, they can't make in-app purchases.
-   Even though the app is still available to customers who have previously acquired it, your app listing is removed from Microsoft Store. No new customers can acquire your app.
-   您的應用程式名稱將釋出，而其他開發人員可以使用該名稱。
-   If you have a balance due from previous app sales, you can request payment for that balance even if the amount due doesn't meet the standard payment threshold.
