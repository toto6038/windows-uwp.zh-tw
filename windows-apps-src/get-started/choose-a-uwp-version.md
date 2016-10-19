---
author: QuinnRadich
title: "選擇 UWP 版本"
description: "在 Microsoft Visual Studio 中撰寫 UWP app 時，您可以選擇要以哪個版本為目標。 了解不同 UWP 版本之間的差異，以及如何在新的及現有的專案中設定您的選項。"
redirect_url: ../updates-and-versions/choose-a-uwp-version/
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f

---

# 選擇 UWP 版本

**此頁面已移至 ../updates-and-versions/choose-a-uwp-version/**

在 Microsoft Visual Studio 中撰寫 UWP app 時，您可以選擇要以哪個版本為目標。 了解不同 UWP 版本之間的差異，以及如何在新的及現有的專案中設定您的選項。

## 每個 UWP 版本有何差異？

每個 Windows 10 後繼版本都可使用適用於 UWP 的新 API 和已變更的 API。 如需有關哪個版本中新增了哪些功能的特定資訊，請參閱 [Windows 10 提供給開發人員的新功能](../whats-new/windows-10-version-1607.md)。

如需列舉出所有裝置系列及其版本和所有 API 協定及其版本的參考主題，請參閱[裝置系列](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx)和 [API 協定](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx)。

## 選擇要針對您 App 使用的版本

在 Visual Studio 中的 [新增通用 Windows 專案]**** 對話方塊中，您可以為 [目標版本]**** 和 [最小版本]**** 選擇版本。

* **目標版本**。 這會設定您專案檔中的 *TargetPlatformVersion* 設定。 它也會決定您應用程式套件資訊清單中 *TargetDeviceFamily@MaxVersionTested* 屬性的值。 您選擇的值會指定您專案的目標 UWP 平台版本，也因此會決定您 App 可用的一組 API，所以我們建議您選擇可能的最新版本。 如需有關您應用程式套件資訊清單的詳細資訊，以及一些有關手動設定 TargetDeviceFamily 指導方針，請參閱 [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903)。
* **最小版本**。 這會設定您專案檔中的 *TargetPlatformMinVersion* 設定。 它也會決定您應用程式套件資訊清單中 *TargetDeviceFamily@MinVersion* 屬性的值。 您選擇的值會指定您專案可搭配運作的 UWP 平台最小版本。

請注意，您宣告的是您的 App 可以在從 [最小版本]**** 到 [目標版本]**** 範圍內的任何 Windows 版本上運作。 如果這兩者是相同版本，您就不需要採取任何特別的動作。 如果兩者不同，則以下是一些需要注意的事項。

* 在您的程式碼中，您可以自由地 (也就是沒有條件檢查) 呼叫存在於 [最小版本]**** 所指定版本中的任何 API。
* [目標版本]**** 的值可用來識別所有用來編譯您專案的參考 (協定 winmds)。 但這些參考將可讓您在編譯您的程式碼時，使用不一定存在於您所宣告支援之裝置上的 API 呼叫 (透過 [最小版本]****) 來進行編譯。 因此，呼叫在 [最小版本]**** 之後引進的 API 時，將需要透過調適型程式碼來呼叫。 如需有關調適型程式碼的詳細資訊，請參閱[通用 Windows 平台 (UWP) app 指南](universal-application-platform-guide.md)。


<!--HONumber=Aug16_HO5-->


