---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: 了解如何指派值的 **AdControl** 屬性。
title: XAML 屬性範例

---

# XAML 屬性範例


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

下列 XAML 範例示範如何指派值的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 屬性。 如果未設定屬性，**AdControl** 會使用預設值來建立與 App 的使用者經驗一致的廣告。

這些值為範例。 您會在您的程式碼中設定適合您 App 的這些函式和屬性的值。

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->


