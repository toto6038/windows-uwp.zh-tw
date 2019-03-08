---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP 應用程式串流安裝
description: 通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, uwp, 串流安裝, uwp app 串流安裝
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608653"
---
# <a name="uwp-app-streaming-install"></a>UWP 應用程式串流安裝
通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。 

若要使用 UWP App 串流安裝，您必須將您應用程式的檔案分為數個區塊。 若要完成這項操作，您將需要建立一個內容群組對應，也就是您一個與您應用程式一同封裝的 XML 檔案，讓您可以設定檔案下載的優先度及順序。 請參閱下方的主題連結，以獲得詳細資訊。

如需將 UWP App 串流安裝新增至您 UWP app 的完整指南，請參閱這個[部落格系列文章](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。

| 主題 | 描述 | 
|-------|-------------|
| [建立並將轉換的來源內容群組對應](create-cgm.md) | 若要讓您的通用 Windows 平台 (UWP) 應用程式準備就緒以使用 UWP App 串流安裝，您需要建立內容群組對應。 此文章將協助您了解建立和轉換內容群組對應時所需要的詳細資訊，以及提供一些過程中的秘訣與技巧。 |