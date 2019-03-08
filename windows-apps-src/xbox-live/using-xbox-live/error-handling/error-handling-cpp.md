---
title: C++ API 錯誤處理
description: 了解如何在 Xbox Live 服務呼叫，使用 c + + Api 時，處理錯誤。
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，錯誤處理
ms.localizationpriority: medium
ms.openlocfilehash: 90fd816f8d44b27c1df0ded9bee6473f642478a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636523"
---
# <a name="c-api-error-handling"></a>C++ API 錯誤處理

在 c + + API，而不是擲回例外狀況，大部分的呼叫會傳回適當 xbox_live_result < payload_type >。

## <a name="xboxliveresult-structure"></a>xbox_live_result structure
xbox_live_result 有 3 個項目：
1. 藉此作業，傳回的錯誤
2. 用於偵錯用途的特定錯誤訊息和
3. 結果 （可以是空白如果發生錯誤） 的承載 」

您可以取得更多有關 xbox_live_result 為以及哪些錯誤代碼是 Xbox Live 的文件中。

結構如下：

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**err** -會傳回錯誤。  將會發生任何錯誤的 NULL 參考。  此行為與 c + + STL 錯誤沒有，您可以藉由呼叫 value （） 取得的基本值。  呼叫 message （） 會取得字串表示。  因此，如果錯誤碼代表 「 無效的引數 」，然後```err().message()```會是 「 無效的引數 」 的文字。

**err_message** -將有關錯誤的詳細說明。  比方說如果**err**是 「 無效的引數 」，則**err_message**會詳述在哪一個引數無效。

**裝載**-傳回感興趣的項目。  例如，請考慮```xbox_live_result<achievement>```您可能會從呼叫 get_achievement 取得。  在此範例中，承載會成就本身 （如果沒有錯誤）。

## <a name="example"></a>範例

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>使用 xbox_live_error_condition 測試針對廣泛的錯誤類別目錄
在上述範例中，我們測試錯誤碼 403 錯誤，以及所謂```xbox_live_error_condition::auth```。

 當使用 xbox_live_result err() 函式，其中可以個別測試對錯誤代碼。  例如，400 類別錯誤您可能會有個別的測試和控制項的流程：

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* etc

但這通常是不是您想執行，而且您想要測試的其中一個錯誤類別。  因此，您可以測試使用的列舉中可用的錯誤類別```xbox_live_error_condition```類別。  我們會對許多錯誤代碼的測試自動化的等號比較運算子的實作多載。  除了```auth```，有類別，諸如```rta```和```http```。  完整清單可在*errors.h*或是在*xblsdk_cpp.chm*。

如需說明此範例和 c + + Xbox 服務 api 的一些其他功能的影片，請查看我們 XFest 溝通[Xfest 2015 影片](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx)下方*XSAPI:C + +，任何例外狀況 ！*
