---
title: 針對日本紀元變更準備您的應用程式
description: 了解 2019 年 5 月的日本紀元變更，以及如何準備您的應用程式。
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 12/7/2018
ms.topic: article
keywords: windows 10, uwp, 是否可本地化, 當地語系化, 日文, 紀元
ms.localizationpriority: high
ms.openlocfilehash: 0d5de4c1713ab80afcdf2e028d39340aebcc018b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617653"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>針對日本紀元變更準備您的應用程式

日本曆劃分為紀元，而我們經歷的現代電腦運算時期，大部分落在平成紀元；不過，2019 年 5 月 1 日將會開啟新的紀元。 因為這是數十年來第一次要改換年號重新紀元，支援日本曆的軟體需要進行測試，以確保該軟體可在新紀元開始時正常運作。

在下列各節中，您將了解如何因應即將來臨的新紀元，準備並測試您的應用程式。

> [!NOTE]
> 由於您所做的變更將會影響整部電腦的行為，我們建議使用測試電腦來進行這項工作。

## <a name="add-a-registry-key-for-the-new-era"></a>新增新紀元的登錄機碼

請務必在紀元變更之前測試相容性問題，而您目前可以先使用預留位置來進行。 若要這樣做，請使用 **\[登錄編輯程式\]** 新增新紀元的登錄機碼：

1. 瀏覽至 **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**。
2. 選取 **\[編輯\] > \[新增\] > \[字串值\]**，並指定其名稱 **2019 05 01**。
3. 以滑鼠右鍵按一下機碼，然後按一下 **\[修改\]**。
4. 在 **值的資料**欄位中，輸入**嗎？？\_嗎？\_??????\_?** (您從這裡複製後再貼上，會變得容易些)。

如需深入了解這些登錄機碼的格式，請參閱[日本曆的紀元處理方式](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)。

新紀元年號名稱一旦宣布，針對支援的 Windows 版本所發行使用新登錄機碼的更新就會包含此年號名稱，您可以驗證應用程式是否正確處理該名稱。 這項更新將會傳播至支援的舊版 Windows 10，以及 Windows 8 和 7。

完成應用程式的測試後，就可以刪除您的預留位置登錄機碼。 這可確保不會干擾更新 Windows 時新增的新登錄機碼。

## <a name="change-your-devices-calendar-format"></a>變更您裝置的日曆格式

新增新紀元的登錄機碼後，您必須將裝置設定為使用日本曆。 您的裝置沒有日文語言，也能這樣做。 進行完整測試時，您可能還需要安裝日文語言套件，但是基本測試就不需要如此。

若要將您的裝置設定為使用日本曆：

1. 開啟 **intl.cpl** (可從 Windows 搜尋列搜尋找出)。
2. 從 **\[格式\]** 下拉式清單選取 **\[日文 (日本)\]**。
3. 選取 **\[其他設定\]**。
4. 選取 **\[日期\]** 索引標籤。
5. 從 **\[月曆類型\]** 下拉式清單選取 **\[和暦\]** (*wareki* 日本曆)。 這應該會是第二個選項。
6. 按一下 [確定] 。
7. 按一下 **\[地區\]** 視窗中的 **\[確定\]**。

您的裝置現在應該已設定為使用日本曆，無論哪個紀元在登錄中，都會反映出來。 以下是您可能會在畫面右下角看到現的範例：

![使用日本曆格式的日期及時間](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>調整您裝置的時鐘

若要測試您的應用程式是否可搭配新紀元使用，您必須將電腦的時鐘往前調到 2019 年 5 月 1 日或更晚的日期。 下列指示適用於 Windows 10，但 Windows 8 和 7 也應該同樣有效：

1. 以滑鼠右鍵按一下畫面右下角的日期和時間區域。
2. 選取 **\[調整日期/時間\]**，
3. 在 \[設定\] 應用程式的 **\[變更日期和時間\]** 下方，選取 **\[變更\]**。
4. 將日期變更為 2019 年 5 月 1 日或之後的日期。

> [!NOTE]
> 您可能無法變更日期，根據組織設定;如果發生這種情況，，向您的管理員。或者，您可以編輯您的版面配置區登錄機碼，以有已經過期的日期。

## <a name="test-your-application"></a>測試您的應用程式

現在來測試應用程式處理新紀元的情況。 檢查會顯示日期的位置，例如時間戳記和日期選擇器。 請確定這個時代是正確的 1，2019 年 （平成、 平成） 之前和之後 （嗎？？）。

### <a name="gannen-"></a>*元年*

日本曆法的格式通常是**&lt;紀元名稱&gt;&lt;紀元的年份&gt;**。 例如，2018 年是**平成 30 年**。  不過，紀元的第一年有特殊格式；使用的不是 **&lt;紀元年號&gt; 1 年**，而是 **&lt;紀元年號&gt; 元年**。 因此，平成紀元的第一年會是*平成元年*。 確定您的應用程式適當地處理這個新的時代，第一年，並正確輸出？？ 元年。

## <a name="related-apis"></a>相關的 API

有幾個 WinRT、.NET 和 Win32 API 會更新來處理紀元變更，因此用到這些 API 的話，您應該不需要太過擔心。 不過，即使您可以完全依賴這些 API，最好還是要測試您的應用程式，確定看到您想要得到的行為，尤其是在您要用它們來執行任何特殊作業 (例如剖析) 的時候。

您可以透過[因應日本紀元於 2019 年 5 月變更的更新](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)深入了解 OS 與 SDK 的更新。

下列 API 將受影響：

### <a name="winrt"></a>WinRT

* [Windows.Globalization 命名空間](https://docs.microsoft.com/uwp/api/windows.globalization)
    * [行事曆類別](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
        * [AddDays 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
        * [AddEras 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
        * [AddHours 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
        * [AddMinutes 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
        * [AddMonths 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
        * [AddNanoseconds 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
        * [AddPeriods 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
        * [AddSeconds 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
        * [AddWeeks 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
        * [AddYears 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
        * [紀元屬性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
        * [EraAsString 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
        * [FirstYearInThisEra 屬性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
        * [LastEra 屬性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
        * [LastYearInThisEra 屬性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
        * [NumberOfYearsInThisEra 屬性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)     
* [Windows.Globalization.DateTimeFormatting 命名空間](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
    * [DateTimeFormatter 類別](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
        * [Format 方法](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [System 命名空間](https://docs.microsoft.com/dotnet/api/system)
    * [DateTime 結構](https://docs.microsoft.com/dotnet/api/system.datetime)
    * [DateTimeOffset 結構](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization 命名空間](https://docs.microsoft.com/dotnet/api/system.globalization)
    * [行事曆類別](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
    * [DateTimeFormatInfo 類別](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
    * [JapaneseCalendar 類別](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
    * [JapaneseLunisolarCalendar 類別](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h 標頭](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
    * [GetDateFormatA 函式](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
    * [GetDateFormatEx 函式](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
    * [GetDateFormatW 函式](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [winnls.h 標頭](https://docs.microsoft.com/windows/desktop/api/winnls/)
    * [EnumDateFormatsA 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
    * [EnumDateFormatsExA 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
    * [EnumDateFormatsExEx 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
    * [EnumDateFormatsExW 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
    * [EnumDateFormatsW 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
    * [GetCalendarInfoA 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
    * [GetCalendarInfoEx 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
    * [GetCalendarInfoW 函式](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>請參閱

* [處理日本曆法的紀元](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [日本曆法 Y2K 一點時間](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [若要測試這個新的日文時代在 Windows 上使用登錄](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [元年 vs Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [更新可能 2019年日本紀元變更](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [處理在.NET 中日本曆法中的新時代](https://blogs.msdn.microsoft.com/dotnet/2018/11/14/handling-a-new-era-in-the-japanese-calendar-in-net/)