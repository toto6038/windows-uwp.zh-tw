---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP app 串流安裝
description: 通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10，uwp，串流安裝，uwp app 串流安裝
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8466978"
---
# <a name="uwp-app-streaming-install"></a>UWP app 串流安裝
通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。 

若要使用 UWP App 串流安裝，您將需要將您的應用程式檔案分節。 若要這樣做，您將會建立內容群組對應，也就是一個 XML 檔案，會與您的應用程式封裝，可讓您將設定下載優先順序和順序。 請參閱下列連結，如需詳細資訊的主題。

如上將 UWP 應用程式串流安裝加入您的 UWP 應用程式的完整指南，請參閱此[部落格系統](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。

| 主題 | 描述 | 
|-------|-------------|
| [建立和轉換來源內容群組對應](create-cgm.md) | 若要讓您的通用 Windows 平台 (UWP) 應用程式準備就緒以使用 UWP App 串流安裝，您需要建立內容群組對應。 此文章將協助您了解建立和轉換內容群組對應時所需要的詳細資訊，以及提供一些過程中的秘訣與技巧。 |