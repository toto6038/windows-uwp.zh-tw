---
title: 設定您的 Xbox 開發主控台
description: 了解如何設定支援的 Xbox Live 開發您的開發 xbox。
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 479be2401e0c54801645ad1c0d91b11b7ffb6869
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649013"
---
# <a name="configure-your-xbox-development-console"></a>設定您的 Xbox 開發主控台

若要設定您的開發主控台：
- 取得您的識別碼
- 設定您的沙箱上開發套件
- 使用開發帳戶登入

## <a name="get-your-ids"></a>取得您的識別碼
若要啟用沙箱和 Xbox Live 服務，您必須取得數個識別碼，以設定您的開發套件和程式的標題。 這些可以透過相同的程序。

請遵循[Xbox Live 服務組態](../xbox-live-service-configuration.md)以取得您的識別碼

## <a name="set-your-sandbox-on-your-development-kits"></a>設定您的沙箱上開發套件
您不能啟動您的開發套件，而不需要設定您的沙箱識別碼。 若要這樣做，您可以使用 「 Xbox 一個管理員 」 在您的電腦上安裝的 XDK，或者您可以開啟 XDK 命令視窗，並使用 Configuration (xbconfig.exe) 命令，如下所示：

請檢查您目前的沙箱。 在命令提示字元中輸入 xbconfig sandboxid。

如果它不如預期，請變更您的沙箱識別碼。輸入 xbconfig sandboxid =<your sandbox id>在命令提示字元。

重新啟動您在命令提示字元中使用重新開機 (xbreboot.exe) 的主控台。

請確認正確重設您的沙箱。 在命令提示字元中輸入 xbconfig sandboxid。

## <a name="sign-in-with-a-development-account"></a>使用開發帳戶登入

您可以建立開發帳戶用來登入[Xbox 開發人員入口網站 (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts)或[合作夥伴中心](https://partner.microsoft.com/dashboard)
