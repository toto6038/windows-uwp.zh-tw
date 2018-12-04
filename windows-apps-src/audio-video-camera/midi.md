---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: 本文章示範如何列舉 MIDI (樂器數位介面) 裝置，並且從通用 Windows app 傳送及接收 MIDI 訊息。
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cb210621b74fef5128456d06a7cdf047752f45f5
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8483488"
---
# <a name="midi"></a>MIDI



本文章示範如何列舉 MIDI (樂器數位介面) 裝置，並且從通用 Windows app 傳送及接收 MIDI 訊息。 Windows 10 支援透過 USB （類別規範和最專屬驅動程式），透過藍牙 LE MIDI MIDI (Windows 10 年度更新版及更新版本)，並透過自由地使用第三方產品，透過乙太網路 MIDI 和路由 MIDI。

## <a name="enumerate-midi-devices"></a>列舉 MIDI 裝置

列舉和使用 MIDI 裝置之前，請將下列命名空間新增至您的專案。

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

將 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) 控制項新增至您的 XAML 頁面，讓使用者選取其中一個要附加至系統的 MIDI 輸入裝置。 新增另一個以列出 MIDI 輸出裝置。

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

[**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 方法 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 類別是用來列舉 Windows 可辨識的許多不同類型的裝置。 若要指定您只想要方法可尋找 MIDI 輸入裝置，請使用 [**MidiInPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894779) 傳回的選取器字串。 **FindAllAsync** 會傳回 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395)，包含向系統登錄之每個 MIDI 輸入裝置的 **DeviceInformation**。 如果傳回的集合不包含任何項目，則沒有可用的 MIDI 輸入裝置。 如果集合中有項目，循環顯示 **DeviceInformation** 物件並且將每個裝置的名稱新增至 MIDI 輸入裝置 **ListBox**。

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

列舉 MIDI 輸出裝置的運作方式與列舉輸入裝置的方式完全相同，不同的是您應該指定在呼叫 **FindAllAsync** 時 [**MidiOutPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894845) 傳回的選取器字串。

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>建立裝置監控程式協助程式類別

[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空間提供 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446)，它可以在系統中新增或移除裝置時，或裝置的資訊更新時，通知您的 app。 因為已啟用 MIDI 的 app 通常會想要輸入和輸出裝置，這個範例會建立實作 **DeviceWatcher** 的協助程式類別，以便相同的程式碼可以用於 MIDI 輸入和 MIDI 輸出裝置，而不需要重複。

將新的類別新增至您的專案做為裝置監控程式。 在此範例中，類別名為 **MyMidiDeviceWatcher**。 在本節中的其餘程式碼是用來實作協助程式類別。

將部分成員變數新增至類別：

-   [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) 物件，該物件會監視裝置變更。
-   裝置選取器字串，它將針對一個執行個體包含 MIDI 輸入連接埠選取器字串，針對另一個執行個體包含 MIDI 輸出連接埠選取器字串。
-   [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) 控制項，該控制項會填入可用裝置的名稱。
-   [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)，這是從 UI 執行緒以外的執行緒更新 UI 的必要項目。

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

新增 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 屬性，該屬性是用來存取來自協助程式類別外部的目前裝置清單。

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

在類別建構函式中，呼叫者傳入 MIDI 裝置選取器字串，用來列出裝置的 **ListBox**，和更新 UI 所需的 **Dispatcher**。

呼叫 [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) 以建立 **DeviceWatcher** 類別的新執行個體，傳入 MIDI 裝置選取器字串。

針對監控程式的事件處理常式登錄處理常式。

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

**DeviceWatcher** 具有下列事件：

-   [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450) - 在新的裝置新增至系統時引發。
-   [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453) - 在從系統移除裝置時引發。
-   [**Updated**](https://msdn.microsoft.com/library/windows/apps/br225458) - 在與現有裝置相關聯的資訊更新時引發。
-   [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) - 在監控程式已完成其要求裝置類型的列舉時引發。

在每個事件的事件處理常式中，會呼叫協助程式方法 **UpdateDevices**，以使用目前的裝置清單更新 **ListBox**。 因為 **UpdateDevices** 更新 UI 元素與這些事件處理常式不是在 UI 執行緒上呼叫，每個呼叫必須包裝於對 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的呼叫中，這會使指定的程式碼在 UI 執行緒上執行。

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

**UpdateDevices** 協助程式方法會呼叫 [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)，以及使用本文先前所述的已傳回裝置的名稱更新 **ListBox**。

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

使用 **DeviceWatcher** 物件的 [**Start**](https://msdn.microsoft.com/library/windows/apps/br225454) 方法，新增方法以啟動監控程式，以及使用 [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456) 方法，停止監控程式。

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

提供解構函式以取消註冊監控程式事件處理常式，並且將裝置監控程式設為 Null。

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>建立 MIDI 連接埠以傳送和接收訊息

在您的頁面背後的程式碼中，宣告成員變數來保存 **MyMidiDeviceWatcher** 協助程式類別的兩個執行個體，一個用於輸入裝置，另一個用於輸出裝置。

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

建立監控程式協助程式類別的新執行個體，傳入裝置選取器字串、要填入的 **ListBox** 和可以透過頁面的 **Dispatcher** 屬性存取的 **CoreDispatcher** 物件。 然後，呼叫方法以啟動每個物件的 **DeviceWatcher**。

在啟動每個 **DeviceWatcher** 之後不久，它會完成列舉連接到系統的目前裝置，並且引發其 [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) 事件，這會導致每個 **ListBox** 以目前的 MIDI 裝置進行更新。

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

當使用者在 MIDI 輸入 **ListBox** 中選取項目時，會引發 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件。 在這個事件的處理常式中，存取協助程式類別的 **DeviceInformationCollection** 屬性以取得目前的裝置清單。 如果清單中有項目，選取具有對應至 **ListBox** 控制項的 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/br209768) 之索引的 **DeviceInformation** 物件。

建立 [**MidiInPort**](https://msdn.microsoft.com/library/windows/apps/dn894770) 物件，代表選取的輸入裝置，方法是呼叫 [**MidiInPort.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894776)，傳入選取的裝置的 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 屬性。

登錄 [**MessageReceived**](https://msdn.microsoft.com/library/windows/apps/dn894781) 事件的處理常式，它會在每當透過指定的裝置接收 MIDI 訊息時引發。

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

當呼叫 **MessageReceived** 處理常式時，訊息會包含在 **MidiMessageReceivedEventArgs** 的 [**Message**](https://msdn.microsoft.com/library/windows/apps/dn894783) 屬性中。 訊息物件的 [**Type**](https://msdn.microsoft.com/library/windows/apps/dn894726) 是來自 [**MidiMessageType**](https://msdn.microsoft.com/library/windows/apps/dn894786) 列舉的值，指出已收到之訊息的類型。 訊息的資料取決於訊息類型。 這個範例會查看訊息是否為訊息的附註，若是如此，會輸出 midi 通道、附註和訊息的速度。

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

輸出裝置 **ListBox** 的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 處理常式的運作方式與輸入裝置的處理常式運作方式相同，不同的是不會註冊事件處理常式。

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

建立輸出裝置之後，您可以傳送訊息，方法是針對您要傳送的訊息類型建立新的 [**IMidiMessage**](https://msdn.microsoft.com/library/windows/apps/dn911508)。 在此範例中，訊息是 [**NoteOnMessage**](https://msdn.microsoft.com/library/windows/apps/dn894817)。 會呼叫 [**IMidiOutPort**](https://msdn.microsoft.com/library/windows/apps/dn894727) 物件的 [**SendMessage**](https://msdn.microsoft.com/library/windows/apps/dn894730) 方法以傳送訊息。

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

當您的 app 停用時，請務必清除您的 app 資源。 取消註冊事件處理常式並且將 MIDI 輸入連接埠和輸出連接埠物件設為 Null。 停止裝置監控程式並且將它們設為 Null。

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>使用內建的 Windows General MIDI 合成

當您使用上述的技術列舉輸出 MIDI 裝置時，您的 app 將會探索名為「Microsoft GS Wavetable Synth」的 MIDI 裝置。 這是您可以用來從 app 播放的內建 General MIDI 合成器。 不過，除非您已經在專案中包含內建合成的 SDK 擴充功能，否則嘗試在此裝置建立 MIDI 輸出將會失敗。

**在 app 專案中包含 General MIDI Synth SDK 擴充功能**

1.  在 **\[方案總管\]** 中您的專案底下，以滑鼠右鍵按一下 **\[參考\]**，然後選取 **\[加入參考\]**。
2.  展開 **\[Universal Windows\]** 節點。
3.  選取 [**擴充功能**]。
4.  從擴充功能清單選取 **\[Microsoft General MIDI DLS for Universal Windows Apps\]**。
    > [!NOTE] 
    > 如果擴充功能有多個版本，請務必選取符合您 app 之目標的版本。 您可以在專案的 [屬性]、**\[應用程式\]** 索引標籤上查看設為 app 目標的 SDK 版本。

 

 




