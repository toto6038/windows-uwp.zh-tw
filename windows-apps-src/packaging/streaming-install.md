---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP app 串流安裝
description: 通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 資料流安裝、 資料流的 uwp 應用程式安裝
ms.localizationpriority: medium
ms.openlocfilehash: 087226cad4bcf7ea0294d8878564c345d6cfb9d0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "304628"
---
# <a name="uwp-app-streaming-install"></a>UWP app 串流安裝
通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。 

若要使用 UWP 應用程式串流安裝您需要將您的應用程式的檔案分割成區段。 為達成此目的，您將建立內容的群組對應，也就是與您的應用程式封裝的 XML 檔案，讓您設定下載優先順序及順序。 請參閱如需詳細資訊連結下方的主題。

新增應用程式串流安裝 UWP UWP app 完整的指南，查看此[一系列部落格](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。

| 主題 | 描述 | 
|-------|-------------|
| [建立和轉換來源內容群組對應](create-cgm.md) | 若要讓您的通用 Windows 平台 (UWP) 應用程式準備就緒以使用 UWP App 串流安裝，您需要建立內容群組對應。 此文章將協助您了解建立和轉換內容群組對應時所需要的詳細資訊，以及提供一些過程中的秘訣與技巧。 |