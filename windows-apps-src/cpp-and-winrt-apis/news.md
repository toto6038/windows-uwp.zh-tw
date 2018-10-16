---
author: stevewhims
description: 新聞和變更至 C + + /winrt。
title: 有何新在 C + + /winrt
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 新聞，什麼的、 新
ms.localizationpriority: medium
ms.openlocfilehash: bc6be28e112dfdd14b3585bd88ba066fbeae382d
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4682028"
---
# <a name="whats-new-in-cwinrt"></a>有何新在 C + + /winrt

下表包含新聞和變更為[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)中的最新通常可以使用 Windows SDK 版本，這是 10.0.17763.0 (Windows 10，版本 1809年)。 這些變更可能也會出現在較新的 SDK Insider Preview 版本。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>新聞和變更，於 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年)

| 新增或變更的功能 | 其他資訊 |
| - | - |
| **新變更**。 針對它進行編譯，C + + /winrt 不取決於 Windows SDK 標頭。 | 請參閱下方的[Windows SDK 標頭檔案從隔離](#isolation-from-windows-sdk-header-files)。 |
| 已變更 Visual Studio 專案系統格式。 | 請參閱[How 重定您 C + + /winrt 專案，以更新版本的 Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)下方。 |
| 有新的函式和基底類別，可協助您將集合物件傳遞至 Windows 執行階段函式，或來實作您自己的集合屬性和集合類型。 | 請參閱[集合使用 C + + /winrt](collections.md)。 |
| 您可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組與您 C + + /winrt 執行階段類別。 | 如需詳細資訊和程式碼範例，請參閱[資料繫結概觀](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 取消協同程式的支援，可讓您登錄取消回呼。 | 如需詳細資訊和程式碼範例，請參閱[取消非同步作業，並取消回呼](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)。 |
| 在建立時指向成員函式的委派，您可以建立強式或弱式參考 （而不是原始*這個*指標） 會註冊處理常式的位置的某個點目前的物件。 | 如需詳細資訊和程式碼範例，請參閱[安全地存取*此*指標事件處理委派周圍](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)一節中的**如果您使用成員函式做為委派**子區段。 |
| 錯誤被固定的已發現的標準 c + + 的 Visual Studio 的更高一致性。 LLVM 和 Clang toolchain 也更清楚地運用驗證 C + + /winrt 的標準一致性。 | 您將不會再遇到這個問題[中所述的為什麼不會我的新專案編譯嗎？我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

其他變更。

- **新變更**。 [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi)現在會傳回`void*`而不是`HSTRING`。 您可以使用`static_cast<HSTRING>(get_abi(my_hstring));`若要取得 HSTRING。
- **新變更**。 [**winrt::put_abi(winrt::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi)現在會傳回`void**`而不是`HSTRING*`。 您可以使用`reinterpret_cast<HSTRING*>(put_abi(my_hstring));`若要取得 HSTRING *。
- **新變更**。 HRESULT 現在**winrt::hresult**為投影。 如果您需要 HRESULT （或進行輸入檢查，以支援類型 traits （特性）），則您可以`static_cast` **winrt::hresult**。 否則， **winrt::hresult**將轉換成 HRESULT，只要您包含`unknwn.h`之前包含任何 C + + /winrt 標頭。
- **新變更**。 做為**winrt::guid**現在投影 GUID。 適用於您實作的 Api，您必須使用**winrt::guid** GUID 參數。 否則， **winrt::hresult**將轉換為 GUID，只要您包含`unknwn.h`之前包含任何 C + + /winrt 標頭。
- **新變更**。 [**Winrt::handle_type 建構函式**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor)具有已強化進行明確 （它現在是很難撰寫不正確的程式碼與它）。 如果您需要將原始的控制代碼值指派，請改為呼叫[**handle_type::attach 函式**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function)。
- **新變更**。 **WINRT_GetActivationFactory** **WINRT_CanUnloadNow**以及簽章已經有所變更。 您我宣告這些函式。 相反地，包括`winrt/base.h`(這會自動包含，如果您包含任何 C + + /winrt Windows 命名空間標頭檔案) 以包含這些函式的宣告。
- [**Winrt::clock 結構**](/uwp/cpp-ref-for-winrt/clock)，如**from_FILETIME/to_FILETIME**是已取代為**from_file_time/to_file_time**。
- 簡化預期**IBuffer**參數的 Api。 雖然大部分的 Api 偏好集合或陣列，足夠 Api 會依賴**IBuffer** ，它會使用這類 Api，從 c + + 的工作變得更容易。 此更新提供直接存取使用 c + + 標準程式庫容器所使用的相同資料命名慣例**IBuffer**實作背後的資料。 這也可以避免與相衝突 signature 開頭的大寫字母的中繼資料名稱。
- 已改善程式碼產生： 各種不同的改進功能，以減少程式碼大小、 改善內嵌，並最佳化原廠快取。
- 移除不必要的遞迴。 當命令列指的是資料夾，而不是特定的`.winmd`、`cppwinrt.exe`工具不會再搜尋適用於以遞迴方式`.winmd`檔案。 `cppwinrt.exe`工具現在也會處理重複的項目更有效地使它更有彈性的使用者錯誤，且為不良形成`.winmd`檔案。
- 強化智慧型指標。 之前，無法撤銷時事件 revokers 移動-指派新的值。 這有助於發現的問題，其中智慧型指標類別無法可靠地處理自我指派;[**winrt:: com_ptr 結構範本**](/uwp/cpp-ref-for-winrt/com-ptr)中的根目錄。 **winrt:: com_ptr**已經修復，再固定來處理事件 revokers 移動語意正確，讓它們撤銷時指派。

> [!NOTE]
> 版本 1.0.181002.2 （或更新版本） 的[C + + /winrt Visual Studio 擴充功能 (VSIX)](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)安裝，請建立一個新的 C + + /winrt 專案會自動安裝[Microsoft.Windows.CppWinRT NuGet 封裝](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)專案的。 Microsoft.Windows.CppWinRT NuGet 套件提供更高的 C + + WinRT 專案建置支援，讓您的專案可攜式開發電腦之間的組建代理程式 （僅限 NuGet 套件，以及不 VSIX，安裝所在）。
>
> 現有的專案的&mdash;您已安裝的版本 1.0.181002.2 之後 （或更新版本） 的 VSIX&mdash;我們建議您在 Visual Studio 中開啟專案，按一下 [**專案**] \> **...管理 NuGet 套件** \> **瀏覽**，請輸入或貼**Microsoft.Windows.CppWinRT**在搜尋方塊中，在搜尋結果中選取的項目，然後按一下 [**安裝**安裝該專案的套件。


## <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK 標頭檔案從隔離

這可能是您的程式碼是重大變更。

針對它進行編譯，C + + /winrt 不再取決於 Windows SDK 中的標頭檔案。 C 執行階段程式庫 (CRT) 與 c + + 標準範本程式庫 (STL) 中的標頭檔案也不包含任何 Windows SDK 標頭。 且改善標準合規性、 可以避免意外的相依性，並大幅減少您有防範的巨集的數目。

這個獨立表示，C + + WinRT 現在是更多的移植和標準的相容性，以及它項功能擴展了它變得跨編譯器和跨平台媒體櫃的可能性。 這也表示的 C + + /winrt 標頭不造成負面影響的巨集。

如果您先前保留它至 C + + 在專案中，包含任何 Windows 標頭，您現在會以包含他們需要 WinRT。 是，在任何情況下，一律最佳作法明確包含的標頭，您取決於，和另一個程式庫來為您包含它們之間不留它。

目前，Windows SDK 標頭檔案隔離的唯一例外是內建函式，和數字。 不有任何這些最後一個剩餘的相依性的已知的問題。

在您專案中，您可以重新啟用與 Windows SDK 標頭的互通性如果您需要。 您為例，可能會想要實作 （ [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)中設定為根） 的 COM 介面。 例如，包括`unknwn.h`之前包含任何 C + + /winrt 標頭。 執行動作的原因是 C + + /winrt 基底程式庫來啟用各種不同的勾點，以支援傳統 COM 介面。 如需程式碼範例，請參閱[撰寫 COM 元件使用 C + + /winrt](author-coclasses.md)。 同樣地，明確包含宣告類型和/或您想要呼叫的函式的任何其他 Windows SDK 標頭。

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何重定您 C + + /winrt 專案，以更新版本的 Windows SDK

您可能會導致最少的編譯器和連結器問題您專案的方法也是最多處理。 該方法包括建立新的專案 （在您選擇的 Windows SDK 版本為目標），然後透過從您的舊，將檔案複製到新專案。 不會有的舊的小節`.vcxproj`和`.vcxproj.filters`檔案，您可以直接將超過複製到儲存您在 Visual Studio 中新增檔案。

不過，有重定您的專案，在 Visual Studio 中的其他兩種方式。

- 前往專案屬性**一般** \> **Windows SDK 版本**，並選取**全部的設定**和**所有平台**。 設定**Windows SDK 版本**，您想要為目標的版本。
- 在 [**方案總管]** 中，以滑鼠右鍵按一下專案節點、 按一下 [**重定專案**，選擇您想要為目標，是一個版本，然後按一下 **[確定]**。

如果您遇到任何編譯器或連結器錯誤之後使用其中一項下列兩種方法，則您可以嘗試清除方案 (**建置** > **清除方案**和 （或) 手動刪除所有的暫存資料夾和檔案) 之前，先建置一次。

如果 c + + 編譯器產生 「*錯誤 C2039: 'IUnknown': 不是成員的 '\'global 命名空間'*」，然後新增`#include <unknwn.h>`的頂端您`pch.h`檔案 (之前包含任何 C + + /winrt 標頭)。

您可能也需要新增`#include <hstring.h>`在那之後。

如果 c + + 連結器會產生 「*錯誤 LNK2019： 無法解析的外部符號_WINRT_CanUnloadNow@0函式中參考_VSDesignerCanUnloadNow@0*」，則您可以藉由新增解決的`#define _VSDESIGNER_DONT_LOAD_AS_DLL`以您`pch.h`檔案。
