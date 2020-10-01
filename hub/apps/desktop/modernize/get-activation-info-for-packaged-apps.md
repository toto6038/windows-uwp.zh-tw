---
description: 了解如何取得已封裝 .NET 和原生 C++/Win32 應用程式的特定種類啟用資訊
title: 取得已封裝應用程式的啟用資訊
ms.date: 09/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 5593b59a542f014e482c590b8d534804c836a1a7
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216721"
---
# <a name="get-activation-info-for-packaged-apps"></a>取得已封裝應用程式的啟用資訊

從 Windows 10 版本 1809 開始，封裝的傳統型應用程式可以呼叫 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法，在啟動期間擷取特定種類的啟用資訊。 例如，您可以呼叫此方法，開啟檔案、按一下互動式快顯通知或使用通訊協定，以取得與應用程式啟用相關的資訊。 從 Windows 10 版本 2004 開始，在使用[疏鬆套件](./grant-identity-to-nonpackaged-apps.md)的應用程式中也支援這項功能。

> [!NOTE]
> 除了使用本文所述的 [AppInstance. GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法來擷取特定類型的啟用資訊以外，您也可以藉由定義 COM 類別來擷取背景工作的啟用資訊。 如需詳細資訊，請參閱[建立並註冊 winmain COM 背景工作](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task)。

## <a name="code-example"></a>程式碼範例

下列程式碼範例示範如何從 Windows Forms 應用程式中的 **Main** 函式呼叫 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法。 針對您應用程式支援的每個啟用類型，將 `args` 傳回值轉換為對應的事件引數類型。 在此程式碼範例中，假設 `Handlexxx` 方法是您已在其他地方定義的專用啟用處理常式碼。

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>支援的啟用類型

您可以使用 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法，從下表所列的一組受支援事件引數物件中擷取啟用資訊。 其中某些啟用類型需要使用封裝資訊清單中的封裝擴充功能。

只有 Windows 10 版本 2004 和更新版本上才支援 [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) 啟用資訊。 Windows 10 版本 1809 和更新版本上支援所有其他啟用資訊類型。

| 事件引數類型 | 封裝擴充功能 | 相關文件 | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [讓您的傳統型應用程式成為共用目標](./desktop-to-uwp-extend.md#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [使用通訊協定啟動您的應用程式](./desktop-to-uwp-extensions.md#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [來自傳統型應用程式的快顯通知](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps)。 |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [在使用者登入 Windows 時執行可執行檔](./desktop-to-uwp-extensions.md#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [將已封裝的應用程式和一組檔案類型建立關聯](./desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | 無 | [處理啟用和執行語音命令](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | 無 |  |