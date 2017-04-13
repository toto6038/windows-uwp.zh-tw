---
author: Mtoepke
title: "停用 Xbox One 開發人員模式"
description: "停用開發人員模式的方法。"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 244124dd-d80a-4a72-91db-1c9c2fbc7c3c
ms.openlocfilehash: 46e9e013336f19aedafdb21e0417c878c7c7e8a1
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xbox-one-developer-mode-deactivation"></a>停用 Xbox One 開發人員模式

* [切換至零售模式](#switch-to-retail-mode)
* [使用「啟用開發人員模式」App 停用您的主機](#deactivate-your-console-using-the-dev-mode-activation-app)  
* [重設您的主機](#reset-your-console)
* [使用 Windows 開發人員中心停用您的主機](#deactivate-your-console-using-windows-dev-center)

如果您決定不再想要將主機用於開發，請使用下列步驟來停用開發人員模式。

## <a name="switch-to-retail-mode"></a>切換至零售模式
首先，請將您的 Xbox One 主機切換回零售模式。

1. 開啟 **\[開發人員首頁\]**。
2. 按一下 **\[離開開發人員模式\]**。  您的主機將重新啟動為零售模式。  

   ![離開開發人員模式](images/deactivation-leave-dev-mode.png)

接下來，使用下列其中一種方法停用您的主機。

## <a name="deactivate-your-console-using-the-dev-mode-activation-app"></a>使用「啟用開發人員模式」App 停用您的主機

在您的主機上停用開發人員模式的偏好方式，是使用「啟用開發人員模式」app。 

1. 瀏覽至 **\[我的遊戲與 app\]** >  **\[App\]**。
  
   ![啟用步驟 3](images/activation-step-3.png)    
   
2.  開啟「啟用開發人員模式」App。    
3.  按一下 **\[停用\]**。
  
![停用主機](images/deactivation-app.png)

## <a name="reset-your-console"></a>重設您的主機

您也可以透過重設主機來停用開發人員模式。  

> [!NOTE]
> 當您重設主機時，所有本機存檔資料都會遺失。

如需重設主機，請執行下列步驟：

1.  移至 **\[我的遊戲與 app\]**。  
2.  選取 **\[App\]**，然後選取 **\[設定\]**。  
3.  移至左窗格中的 **\[系統\]**，然後選取右窗格中的 **\[主機資訊和更新\]**。  
4.  移至 **\[主機資訊和更新\]**。  
   
    ![主機資訊和更新](images/deactivation-console-info-updates.png)  
    
5.  按一下 **\[重設主機\]**。
    
    ![重設主機](images/deactivation-reset-console.png)
    
6.  接下來，按一下 **\[重設並移除所有項目\]**。 此選項會將主機重設為原始的零售狀態。  您所有的 App、遊戲及本機存檔資料都會被刪除。 請注意，若選擇另一個 **\[重設並保留我的遊戲與 app\]** 選項，會無法將您的主機從開發人員計畫中移除。  
   
    ![重設並移除所有項目](images/deactivation-reset-remove.png)

## <a name="deactivate-your-console-using-windows-dev-center"></a>使用 Windows 開發人員中心停用您的主機

如果基於任何原因，使您無法存取您的主機，您也可以透過 Windows 開發人員中心來停用主機上的開發人員模式。

1. 移至 [developer.microsoft.com/xboxdevices](https://developer.microsoft.com/xboxdevices)。    
2. 以您的開發人員中心帳戶登入開發人員中心。    
3. 在主機清單上，藉由比對序號、主機 ID 或裝置識別碼來尋找您想要停用的主機。  
4. 按一下 **\[停用\]**。  
  
![使用開發人員中心進行停用](images/deactivation-devcenter.png)

如果您之前並未將 Xbox One 主機切換回零售模式，請立即執行。

1. 啟動 **\[開發人員首頁\]**。
2. 按一下 **\[離開開發人員模式\]**。  您的主機將會重新啟動為零售模式。

![啟用步驟 13](images/deactivation-leave-dev-mode.png)

## <a name="see-also"></a>另請參閱
- [啟用 Xbox One 開發人員模式](devkit-activation.md)
- [Xbox One 上的 UWP](index.md)
