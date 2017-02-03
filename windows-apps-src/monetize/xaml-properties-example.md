---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "了解如何指派值的 **AdControl** 屬性。"
title: "AdControl XAML 屬性範例"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: f33e4048a1a9aa68ecc627d81ca15858027720c1

---

# <a name="adcontrol-xaml-properties-example"></a>AdControl XAML 屬性範例

下列 XAML 範例示範如何指派值的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 屬性。 如果未設定屬性，**AdControl** 會使用預設值來建立與應用程式的使用者經驗一致的廣告。

這些值為範例。 您會在您的程式碼中設定適合您應用程式的這些函式和屬性的值。

> [!div class="tabbedCodeSnippets"]
``` xml
<UI:AdControl Width="300",
    Height="250",
    AdUnitId="10865270",
    ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
    IsAutoRefreshEnabled="false",
    AdRefreshed="OnAdRefresh",
    ErrorOcurred="OnAdError",
    IsEngagedChanged="OnAdEngagedChanged" />
```

## <a name="related-topics"></a>相關主題

* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [GitHub 上的廣告範例](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


