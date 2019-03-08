---
title: 多人遊戲的角色
description: 了解如何使用角色來定義玩家角色在 Xbox Live 的多人遊戲。
ms.date: 06/29/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、windows xbox one，多人遊戲，角色
ms.localizationpriority: medium
ms.openlocfilehash: ac5e7758bd8e068681d1c8dab2d47d11374c2616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662083"
---
# <a name="roles"></a>角色

某些遊戲的工作階段，您可能想要指定特定的成員擁有特定遊戲角色，例如支援、 medic、 突擊等。您也可以 wat 的播放程式將會填滿一個特定的遊戲角色保留遊戲的位置。 使用 Xbox Live 角色功能，服務可以追蹤哪一個播放程式會指派哪些遊戲的角色，並強制執行，就可以選取特定的遊戲角色的播放程式數目上限。

最常用的角色是判斷該遊戲的工作階段的遊戲的特定角色。 例如，您可以需要介於 1 到 2 的支援類別、 至少 1 個儲存槽/繁重類別，以及 5 個以上的突擊類別的遊戲模式。

在另一個可能的情況下，您可以指定遊戲工作階段可以有最多 8 spectators 和 1 announcer 剛好 4 的遊戲玩家。

您也可以使用角色來填入其餘的位置，透過其他方式，例如工作階段瀏覽時，保留給朋友的位置。

## <a name="role-types"></a>角色類型

角色類型代表一組角色定義。 每一個角色必須定義一部分的角色類型。 多人連線的工作階段的文件中定義的角色類型。

成員只能指派一個角色從指定的角色類型。 例如，如果 「 類別 」 的角色類型包含治療、 槽和損毀，則成員可以只能指派給這些角色的其中一個。

您可以定義多個角色類型和成員可以從每種角色類型指派一個角色。 在前述案例中，成員可能會選擇他角色，但可能也會指派警察局的刑事組領導者角色，如果警察局的刑事組領導者角色定義於個別的角色類型。

> **重要事項：** Xbox Live SDK 目前僅支援單一角色類型和每個成員的單一角色。

## <a name="role-type-properties"></a>角色型別屬性

當您定義的角色類型時，您必須指定角色類型的下列資訊：

* 角色型別的名稱。 名稱必須是小寫字母-數字，並不能超過 100 個字元。
* 如果角色類型中定義的角色是受管理的擁有者。
* 如果工作階段的存留期間，就可以變更的角色類型中定義的角色屬性。
* 角色定義的角色類型中所包含。

如果受管理的擁有者角色類型，這表示只有擁有者的工作階段中的成員可以將該類型的角色指派給成員。 如果角色類型不是受管理的擁有者，成員可以將角色指派給本身。

您只能指定角色類型是在已設定的 「 hasOwners 」 功能的工作階段管理的擁有者。

> Xbox Live SDK 目前不支援將角色指派給其他成員的擁有者。

## <a name="role-properties"></a>角色屬性

當您定義的角色時，您必須指定每個角色的下列資訊：

* 角色的名稱。 名稱必須是小寫字母-數字，並不能超過 100 個字元。
* 允許充當此角色的成員數目上限。 必須是小於或等於零。
* 的目標數目應該充當此角色的成員。 目標必須是小於或等於零，並小於或等於最大數目的成員可以擔任的角色。

當工作階段將成員指派角色時，該資訊會記錄在多人連線的工作階段的文件中的成員角色中。

服務會強制執行可以指派給角色，但不會強制使用的目標數目的成員數目上限。

## <a name="create-roles"></a>建立角色

角色和角色型別通常定義於[工作階段範本](service-configuration/session-templates.md)。 服務會支援角色和工作階段期間的角色類型定義，但 Xbox Live SDK 並不會。

### <a name="define-role-types-and-roles-in-a-session-template"></a>在工作階段範本中定義的角色類型和角色

當您在 Xbox Live 設定期間建立工作階段範本時，您可以定義角色類型和角色。

角色類型和角色資訊指定為在工作階段範本中，以下列格式的基底層級"roleTypes 」 項目：

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>擷取角色資訊的多人連線的工作階段

您可以取得有關角色的資訊類型、 角色和從多人遊戲工作階段，或從多人遊戲的搜尋控制代碼，將會指派給每個角色的成員數目。

在 Xbox Live SDK 中的角色類型和角色的相關資訊儲存在對應結構中。 將 c + + Api`unordered_map`類別和使用的 WinRT Api`IMapView`類別。

### <a name="get-the-role-information-from-a-search-handle"></a>取得從搜尋控制代碼的角色資訊

在 `multiplayer_search_handle_details`物件傳回的搜尋要求，您可以編製索引，以取得角色型別資訊`role_types`對應您感興趣之角色類型的名稱。

這會傳回`multiplayer_role_type`物件。 您可以取得角色編製索引`roles`對應，以傳回`multiplayer_role_info`物件。

`multiplayer_role_info`物件包含資訊的角色，包括`max_members_count`， `member_xbox_user_ids`， `members_count`，和`target_count`。

### <a name="get-the-role-information-from-a-search-handle"></a>取得從搜尋控制代碼的角色資訊

如從工作階段的使用者角色資訊流程的搜尋控制代碼，從取得資訊的流程類似，但使用一些不同的類別。

在 `multiplayer_session`物件，您可以藉由參考取得角色型別資訊`session_role_types`物件，它是`multiplayer_session_role_types`類別。 這個物件中，您也可以索引`role_types`對應您感興趣之角色類型的名稱。

這會傳回`multiplayer_role_type`物件。 您可以取得角色編製索引`roles`對應，以傳回`multiplayer_role_info`物件。

`multiplayer_role_info`物件包含資訊的角色，包括`max_members_count`， `member_xbox_user_ids`， `members_count`，和`target_count`。

## <a name="change-mutable-role-settings"></a>變更可變動的角色設定

如果角色類型指出某些角色設定可以變更 （可變的），您可以使用`multiplayer_role_type.set_roles()`方法來修改可變動的設定。 只有標示為工作階段擁有者的成員才可以變更角色設定。

## <a name="assign-a-role-to-a-member"></a>將角色指派給成員

目前，只有成員可以將自己在 Xbox Live SDK 中的角色指派。 在 `multiplayer_session`物件，您可以呼叫`set_current_user_role_info(role_type, role_name)`方法，以指定目前成員的角色類型和角色。

如果角色已滿，當您嘗試寫入服務的工作階段時，MPSD 將拒絕寫入。
