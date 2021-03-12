---
description: 瞭解如何從其他類型的未封裝應用程式傳送本機快顯通知，並處理使用者按一下快顯通知。
title: 從其他類型的未封裝應用程式傳送本機快顯通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from other types of unpackaged apps
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10，傳送快顯通知、通知、傳送通知、快顯通知、操作說明、快速入門、使用者入門、程式碼範例、逐步解說、其他類型的應用程式、未封裝
ms.localizationpriority: medium
ms.openlocfilehash: 71c0facc0cd77383ac6682f4934334a7356eb0e8
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231540"
---
# <a name="send-a-local-toast-notification-from-other-types-of-unpackaged-apps"></a>從其他類型的未封裝應用程式傳送本機快顯通知

如果您要開發的應用程式未使用 MSIX/UWP 或稀疏簽署的套件，而且不是 c # 或 c + +，您可以使用此頁面！

快顯通知是一則訊息，App 可以建構此訊息，並於使用者目前未在 App 內時，傳遞給使用者。 本快速入門會逐步引導您完成建立、交付和顯示 Windows 10 快顯通知的步驟。 這些快速入門會使用本機通知，這是最簡單的要執行的通知。

> [!IMPORTANT]
> 如果您要撰寫 c # 應用程式，請參閱 [c # 檔](send-local-toast.md)。 如果您要撰寫 c + + 應用程式，請參閱 [c + + UWP](send-local-toast-cpp-uwp.md) 或 [c + + WRL](send-local-toast-desktop-cpp-wrl.md) 檔。



## <a name="step-1-register-your-app-in-the-registry"></a>步驟1：在登錄中註冊您的應用程式

您必須先在登錄中註冊應用程式的資訊，包括識別您應用程式的唯一 AUMID、應用程式的顯示名稱、圖示，以及 COM 啟動程式的 GUID。

```xml
<registryKey keyName="HKEY_LOCAL_MACHINE\Software\Classes\AppUserModelId\<YOUR_AUMID>">
    <registryValue
        name="DisplayName"
        value="My App"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconUri"
        value="C:\icon.png"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconBackgroundColor"
        value="AARRGGBB"
        valueType="REG_SZ" />
    <registryValue
        name="CustomActivator"
        value="{YOUR COM ACTIVATOR GUID HERE}"
        valueType="REG_SZ" />
</registryKey>
```

## <a name="step-2-set-up-your-com-activator"></a>步驟2：設定您的 COM 啟動項

即使您的應用程式未執行，也可以在任何時間點按一下通知。 因此，通知啟用會透過 COM 啟動項來處理。 您的 COM 類別必須執行 `INotificationActivationCallback` 介面。 COM 類別的 GUID 必須符合您在登錄 CustomActivator 值中所指定的 GUID。

```cpp
struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};
```



## <a name="step-3-send-a-toast"></a>步驟3：傳送快顯通知

在 Windows 10 中，您的快顯通知內容是使用賦予通知外觀極大彈性的調適性語言進行描述。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。

我們將從以文字為基礎的簡單通知開始。 使用通知程式庫) 來建立通知內容 (，並顯示通知！

> [!IMPORTANT]
> 您必須在傳送通知時使用先前的 AUMID，以便從您的應用程式中顯示通知。

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```cpp
// Construct the toast template
XmlDocument doc;
doc.LoadXml(L"<toast>\
    <visual>\
        <binding template=\"ToastGeneric\">\
            <text></text>\
            <text></text>\
        </binding>\
    </visual>\
</toast>");

// Populate with text and values
doc.SelectSingleNode(L"//text[1]").InnerText(L"Andrew sent you a picture");
doc.SelectSingleNode(L"//text[2]").InnerText(L"Check this out, The Enchantments in Washington!");

// Construct the notification
ToastNotification notif{ doc };

// And send it! Use the AUMID you specified earlier.
ToastNotificationManager::CreateToastNotifier(L"MyPublisher.MyApp").Show(notif);
```


## <a name="step-4-handling-activation"></a>步驟4：處理啟用

當您按一下通知時，將會啟用您的 COM 啟動項。


## <a name="more-details"></a>其他詳細資訊

### <a name="aumid-restrictions"></a>AUMID 限制

AUMID 長度最多為129個字元。 如果 AUMID 的長度超過129個字元，則排程的快顯通知將無法運作-您會在新增排程通知時收到下列例外狀況： *傳遞至系統呼叫的資料區域太小。 (0x8007007A)*。