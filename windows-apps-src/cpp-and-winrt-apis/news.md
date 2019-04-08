---
description: C++/WinRT 的新聞和變更。
title: 什麼是新的 C + /cli WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 新聞、 什麼的 new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639953"
---
# <a name="whats-new-in-cwinrt"></a>什麼是新的 C + /cli WinRT

下表包含新聞和變為[C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Windows SDK 的最新的正式推出版本，這是 10.0.17763.0 (Windows 10 版本 1809年)。 這些變更也可能會出現在較新的 SDK Insider Preview 版本。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>新聞和變更，請在 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年)

| 新增或變更的功能 | 其他資訊 |
| - | - |
| **重大變更**。 針對其進行編譯，C + + /cli WinRT 不會相依於 Windows SDK 標頭。 | 請參閱[從 Windows SDK 標頭檔的隔離](#isolation-from-windows-sdk-header-files)底下。 |
| Visual Studio 專案系統格式已變更。 | 請參閱[如何將目標重定您 C + + /cli 至較新版的 Windows sdk 的 WinRT 專案](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)底下。 |
| 有新的函式和基底類別可協助您將集合物件傳遞至 Windows 執行階段函式，或實作您自己的集合屬性和集合型別。 | 請參閱[集合，使用 C + + /cli WinRT](collections.md)。 |
| 您可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension)標記延伸使用您 C + + /cli WinRT 執行階段類別。 | 如需詳細資訊，以及程式碼範例，請參閱[資料繫結概觀](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 取消協同程式的支援可讓您註冊的取消回呼。 | 如需詳細資訊，以及程式碼範例，請參閱[取消非同步作業，並取消回呼](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)。 |
| 在建立指向成員函式的委派時，您可以建立強式或目前物件的弱式參考 (而非原始*這*指標) 註冊處理常式的所在之處。 | 如需詳細資訊，以及程式碼範例，請參閱**如果您使用的成員函式為委派**一節中的子區段[安全地存取*這*與事件處理委派指標](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| 已修正 bug，所發現的 Visual Studio 的 c + + 標準改良的一致性。 LLVM 和 Clang 工具鏈也更好地用來驗證 C + + /cli WinRT 的標準一致性。 | 將不會再遇到問題中所述[為什麼無法我新增的專案編譯？我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

其他變更。

- **重大變更**。 [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi)現在會傳回`void*`而不是`HSTRING`。 您可以使用`static_cast<HSTRING>(get_abi(my_hstring));`取得 HSTRING。
- **重大變更**。 [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi)現在會傳回`void**`而不是`HSTRING*`。 您可以使用`reinterpret_cast<HSTRING*>(put_abi(my_hstring));`取得 HSTRING *。
- **重大變更**。 HRESULT 現在投射為**winrt::hresult**。 如果您需要 （或進行類型檢查，以支援類型特性） 的 HRESULT，則您可以`static_cast` **winrt::hresult**。 否則**winrt::hresult**將 HRESULT，只要您包括`unknwn.h`之前您所包含的任何 C + + /cli WinRT 標頭。
- **重大變更**。 GUID 現在投射為**winrt::guid**。 對於您所實作的 Api，您必須使用**winrt::guid** GUID 參數。 否則，請**winrt::hresult** ，只要您加入，將轉換成 GUID，`unknwn.h`之前您所包含的任何 C + + /cli WinRT 標頭。
- **重大變更**。 [ **Winrt::handle_type 建構函式**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor)未經強化，以便明確 （現在很難撰寫不正確的程式碼與它）。 如果您要指派的未經處理的控制代碼值，請呼叫[ **handle_type::attach 函式**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function)改。
- **重大變更**。 簽章**WINRT_CanUnloadNow**並**WINRT_GetActivationFactory**已變更。 您不得宣告這些函式。 相反地，包含`winrt/base.h`(這是自動包含在內，如果您包含任何 C + + /cli WinRT Windows 命名空間的標頭檔) 包含這些函式的宣告。
- 針對[ **winrt::clock 結構**](/uwp/cpp-ref-for-winrt/clock)， **from_FILETIME/to_FILETIME**取代為**from_file_time/to_file_time**。
- 預期的 Api **IBuffer**已簡化參數。 足夠的 Api 集合或陣列，則偏好使用的大部分 Api，雖然已依賴**IBuffer** ，它需要更輕鬆地使用 c + + 的這類 Api。 此更新可直接存取的資料，背後**IBuffer**使用 c + + 標準程式庫容器所使用的相同資料命名慣例的實作。 這也可避免碰撞的慣例以大寫字母開頭的中繼資料名稱。
- 改善程式碼產生： 若要減少程式碼大小的各種改進改善內嵌 （inline），並處理站快取最佳化。
- 移除不必要遞迴。 當命令列參照至資料夾，而不是特定`.winmd`，則`cppwinrt.exe`工具不會再以遞迴方式搜尋`.winmd`檔案。 `cppwinrt.exe`工具現在也會處理重複的項目更智慧的方式，讓您更有彈性，為使用者錯誤，而且為不良形成`.winmd`檔案。
- 強化的智慧型指標。 之前，無法撤銷時事件 revokers 移動-指派新值。 這有助於找出的問題所在的智慧型指標類別不可靠地處理自我指派;在進行 root 破解[ **winrt::com_ptr 結構範本**](/uwp/cpp-ref-for-winrt/com-ptr)。 **winrt::com_ptr**已修正，並修正來處理事件 revokers 移動語意正確以便指派時就會撤銷。

> [!IMPORTANT]
> 已對重要的變更[C + + WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)，兩者都在版本 1.0.181002.2，版本 1.0.190128.4 然後再。 如需詳細資訊的這些變更，以及它們如何影響您現有的專案中， [Visual Studio 支援 C + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)和[VSIX 擴充功能的舊版](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="isolation-from-windows-sdk-header-files"></a>從 Windows SDK 標頭檔的隔離

這可能是您的程式碼的重大變更。

針對其進行編譯，C + + /cli WinRT 不再取決於 Windows SDK 中的標頭檔。 C 執行階段程式庫 (CRT) 和 c + + 標準範本庫 (STL) 中的標頭檔也不包含任何 Windows SDK 標頭。 而且可以改善標準相容性、 可避免不小心的相依性，並可大幅減少您必須防範的巨集的數目。

表示此獨立性，C + + /cli WinRT 現在更多的可攜式] 和 [標準的相容性，而且它以便促進它變成跨編譯器及跨平台程式庫的可能性。 這也表示，C + + /cli WinRT 標頭不受造成負面影響的巨集。

如果您先前所保留 C + /cli 要包含在您的專案中的任何 Windows 標頭，則您現在必須將它們包含您自己的 WinRT。 是，在任何情況下，永遠的最佳做法是明確包含的標頭，您需要，而且不將它留給將它們納入您的另一個程式庫。

目前，唯一的例外狀況為 Windows SDK 標頭檔案隔離是內建函式和數字。 沒有任何已知的問題，這些最後一個剩餘的相依性。

在專案中，您可以重新啟用的 Windows SDK 標頭的 interop 如果您需要。 您可能會比方說，要實作 COM 介面 (立[ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509))。 例如，包含`unknwn.h`之前您所包含的任何 C + + /cli WinRT 標頭。 這麼做會導致 C + + /cli WinRT 啟用各種不同的攔截程序來支援傳統的 COM 介面的基底文件庫。 如需程式碼範例，請參閱 <<c0> [ 作者 COM 元件，使用 C + + /cli WinRT](author-coclasses.md)。 同樣地，明確地包含宣告類型和/或您想要呼叫的函式的任何其他 Windows SDK 標頭。

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何將目標重定您 C + + /cli WinRT 專案至較新版的 Windows sdk

重定目標的專案可能會導致最少的編譯器和連結器問題的方法也是最費力的。 該方法牽涉到建立新的專案 （您所選擇的 Windows SDK 版本為目標），並接著透過從您的舊，將檔案複製到新的專案。 會有的舊版區段`.vcxproj`和`.vcxproj.filters`檔案，您可以直接複製超過儲存您在 Visual Studio 中新增檔案。

不過，有兩種方式可重定目標的 Visual Studio 中的專案。

- 移至 專案屬性**一般** \> **Windows SDK 版本**，然後選取**所有組態**並**所有平台**。 設定**Windows SDK 版本**，做為目標的版本。
- 在 **方案總管 中**，以滑鼠右鍵按一下專案節點，按一下**重定目標的專案**，選擇您想要的目標，然後按一下 版本**確定**。

如果您遇到任何編譯器或連結器錯誤之後使用下列兩種方法，則您可以嘗試清除方案 (**建置** > **清除方案**及/或以手動方式刪除所有暫存資料夾和檔案） 然後再嘗試再次建置。

如果 c + + 編譯器會產生 「*錯誤 C2039:'IUnknown': 不是成員 '\`全域命名空間'*"，然後新增`#include <unknwn.h>`頂端您`pch.h`檔案 (之前您所包含的任何 C + + /cli WinRT 標頭)。

您可能也需要新增`#include <hstring.h>`之後。

如果 c + + 連結器會產生"*錯誤 LNK2019： 無法解析的外部符號_WINRT_CanUnloadNow@0函式中參考_VSDesignerCanUnloadNow@0* "，則您可以解析，加上`#define _VSDESIGNER_DONT_LOAD_AS_DLL`到您`pch.h`檔案。
