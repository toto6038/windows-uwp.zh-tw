---
title: 程式設計社會服務
description: 提供如何使用 Xbox Live 社交管理員 API 的程式碼範例。
ms.assetid: 101d059a-e03f-472c-8300-800aa5730ee2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 社交 manager、 範例
ms.localizationpriority: medium
ms.openlocfilehash: 5039d9ed205cadfee3b2e64527a1f58f1624accc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592053"
---
# <a name="programming-social-services"></a>程式設計社會服務

> [!NOTE]
> 這篇文章會示範進階的 API 使用方式。  做為起點，請看看[社交管理員 API 的簡介](../intro-to-social-manager.md)可大幅簡化開發。  請讓您知道是否您發現不支援的案例在身分證管理員 中的補西牆。

下列程式碼範例示範如何擷取與 Xbox Live 的社交關係。 它會產生一份系統上的所有使用者，並擷取第一個。 接下來，它會擷取所有與該使用者的社交的關聯性。 最後，它會顯示每個這些關聯性的公用屬性。

```cpp
XboxLiveContext^ xboxLiveContext = NULL;

// An XboxLiveContext for a user should be created only once, or you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_SocialService_GetSocialRelationshipsAsync()
{
    // Generate a list of users on the system.
    // Create an XboxLiveContext from user 0 (the first one returned).
    // This user's authentication token will be used for service requests.
    XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

    // Asynchronously retrieve all social relationships from that context.
    auto asyncOp = xboxLiveContext->SocialService->GetSocialRelationshipsAsync();

    create_task( asyncOp )
    .then([](XboxSocialRelationshipResult^ result)
    {
        // For each social relationship of the specified user...
        for( XboxSocialRelationship^ xboxSocialRelationship : result->Items )
        {
            // ...display the public properties of the relationship.
            LogComment( xboxSocialRelationship->XboxUserId );
            LogComment( xboxSocialRelationship->IsFavorite.ToString() );
        }

    })
    .wait();
}
```
