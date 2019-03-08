---
title: 客戶資料庫應用程式結構
description: 檢閱結構的客戶資料庫教學課程中，以及為什麼它有建構其用法。
keywords: 企業版、 教學課程、 客戶、 資料、 crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656613"
---
# <a name="customer-database-app-structure"></a>客戶資料庫應用程式結構

複雜的特定業務應用程式通常會有許多頁面和功能很大的多行程式碼。 基於這個原因，它是重要的不可或缺的您設計您的應用程式周圍的可預測的結構。 有數個應用程式設計模式是適用於企業應用程式，但它們皆已內建周圍，旨在讓您更容易了解及使用大規模的應用程式。

雖然[客戶資料庫教學課程](customer-database-tutorial.md)呈現單一頁面應用程式為了簡單起見，它會實作 Model View ViewModel (MVVM) 應用程式設計模式，展示這些概念的動作。 如同名稱所暗示，MVVM 設計模式就會將核心應用程式邏輯分成三個類別：

* 模型是包含應用程式的資料類別。
* 檢視是頁面的任何指定的 UI。
* Viewmodel 提供應用程式邏輯。 這可能包括處理使用者動作，從 檢視和/或管理與模型互動。

雖然此應用程式並不完美和 architypical MVVM 的範例中，它會顯示作用中的主要的關注點分離原則。 [簽出的應用程式。](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>應用程式結構

開啟應用程式之後，請先檢查**方案總管 中。** 某些您看到的內容有應該很熟悉，如果您先前曾經使用 UWP 應用程式之前，但您也會看到資料夾的集合，保留應用程式的元件組件。

![在 [方案總管] 中開始點的應用程式](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>檢視

所有應用程式的 UI 定義在 [Views] 資料夾中。 因為在本教學課程現在是單一頁面應用程式，這表示有只有一個檢視- **CustomerListPage**。 它具有 XAML UI 的標記和程式碼後置 xaml.cs-這兩個檔案組成一個檢視。 您會將 UI 項目**CustomerListPage.xaml**。

> [!NOTE]
> 您可能會注意到此應用程式沒有 MainPage。 這是因為我們在指定**App.xaml.cs**應用程式應該啟動**CustomerListPage**時啟動。

### <a name="viewmodels"></a>ViewModels

雖然此應用程式只有一個檢視，有兩個的 Viewmodel。 為什麼會這樣？

**CustomerListPageViewModel.cs**是標準的 ViewModel MVVM 模式中。 它是應用程式之頁面的基本邏輯是，和網頁就會使用在本教學課程中大部分位置。 使用者所採取的每個 UI 動作會透過檢視來處理此 ViewModel。

**CustomerViewModel.cs**，不過，不與任何特定的檢視相關聯。 相反地，它將與包含個別客戶的模型中的資料關聯 （已編輯的屬性） 的程式設計概念。

### <a name="models"></a>模型

此應用程式包含三種模型，其中儲存應用程式的資料，並提供與存放庫互動的介面。 這些都是重要的應用程式，它們不是您會直接在本教學課程中編輯的項目。

最重要的是**Customer.cs**，其中描述您將在本教學課程中使用客戶資料結構。

> [!NOTE]
> 本教學課程會忽略*電子郵件*並*Phone*客戶物件的屬性。 如果您想要超越項目會顯示在這裡，將這兩個屬性新增至應用程式的 UI 是很好的起點。

### <a name="repository"></a>存放庫

存放庫資料夾包含建構及本機 SQLite 資料庫互動的類別。 教學課程中，SQLite 資料庫會以-為。 雖然您會將程式碼中**CustomerListPageViewModel.cs**呼叫這些類別所定義的方法，您不需要進行任何變更，以便設定它們。

如需有關在 UWP，SQLite[請參閱這篇文章](../data-access/sqlite-databases.md)。

如果您嘗試本教學課程的 「 繼續 」 一節，這是您將在其中建立類別以連接到遠端的其餘部分資料庫。 它也會實作**ICustomerRepository**介面區段中定義模型，但是它看起來會非常不同於其 SQLite 對應項目。

### <a name="other-elements"></a>其他項目

因為是一般的 UWP 應用程式，在定義應用程式的啟動行為**App.xaml.cs**類別。 此處的程式碼大多是任何 UWP 應用程式的預設程式碼。 不過，我們已進行了幾個小變更：

* 我們已經指定應用程式應顯示**CustomerListPage**啟動。
* 我們建立了儲存機制物件，會保留我們正在使用的資料來源。
* 我們已新增**SQLiteDatabase**方法，它會初始化本機資料庫，並將它設定為指定的存放庫。

如果您嘗試 「 繼續 」 一節，您將新增類似的方法，來初始化 REST 存放庫物件。 因為我們已經有獨立的優先考量，而使用 SQLite 與 REST 作業的相同定義的介面，這會是唯一的現有程式碼，以您要將變更為應用程式中使用而不是 SQLite 的其餘部分。

## <a name="next-steps"></a>後續步驟

如果您已經完成教學課程中，則您可以看看[完整的範例應用程式](https://github.com/Microsoft/Windows-appsample-customers-orders-database)來了解這些功能的大規模實作。

否則，既然您知道的所有項目是其所在的為何，您應該[返回本教學課程](customer-database-tutorial.md)以及使用我們所討論的結構。