---
title: CPUSets 遊戲開發
description: 本文章提供通用 Windows 平台 (UWP) 新功能 CPUSets API 的概觀，並涵蓋遊戲與應用程式開發的相關核心資訊。
ms.topic: article
ms.localizationpriority: medium
ms.date: 02/08/2017
ms.openlocfilehash: 49662d476d6d022ca05d53e9358fc547fda92a32
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8887678"
---
# <a name="cpusets-for-game-development"></a>CPUSets 遊戲開發

## <a name="introduction"></a>簡介

通用 Windows 平台 (UWP) 是許多消費者電子裝置的核心。 因此，它需要一般用途的 API 以處理所有應用程式類型的需求，從遊戲到內嵌的應用程式，以及在伺服器上執行的企業軟體。 利用 API 提供的正確資訊，您可以確保遊戲在任何硬體上都能發揮最佳效能。

## <a name="cpusets-api"></a>CPUSets API

CPUSets API 可讓您控制要提供哪些 CPU 集合以供在上面排程執行緒。 有兩個函式可用來控制排程執行緒的所在位置：
- **SetProcessDefaultCpuSets** – 若新的執行緒未指派給特定的 CPU 集合，這個函式可用來指定新的執行緒可在哪些 CPU 集合上執行。
- **SetThreadSelectedCpuSets** – 這個函式可讓您將 CPU 集合限制為只有特定執行緒可在上面執行。

若從未使用 **SetProcessDefaultCpuSets** 函式，則新建立的執行緒可能會排程到可供處理程序使用的任何 CPU 集合。 本節將詳細說明 CPUSets API 的基本知識。

### <a name="getsystemcpusetinformation"></a>GetSystemCpuSetInformation

用來收集資訊的第一個 API 是 **GetSystemCpuSetInformation** 函式。 這個函式會填入標題程式碼所提供之 **SYSTEM_CPU_SET_INFORMATION** 物件陣列的資訊。 目的地記憶體必須由遊戲程式碼配置，其大小是由呼叫 **GetSystemCpuSetInformation** 本身來決定。 這需要呼叫 **GetSystemCpuSetInformation** 兩次，如下列範例所示。

```
unsigned long size;
HANDLE curProc = GetCurrentProcess();
GetSystemCpuSetInformation(nullptr, 0, &size, curProc, 0);

std::unique_ptr<uint8_t[]> buffer(new uint8_t[size]);

PSYSTEM_CPU_SET_INFORMATION cpuSets = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(buffer.get());
  
GetSystemCpuSetInformation(cpuSets, size, &size, curProc, 0);
```

每個傳回的 **SYSTEM_CPU_SET_INFORMATION** 執行個體會包含一個唯一的處理單元資訊，又稱為 CPU 集合。 這不一定表示它代表唯一的實際硬體。 利用超執行緒的 CPU 會有多個邏輯核心在單一實體處理核心上執行。 將多個執行緒排程在位於相同實體核心上的不同邏輯核心，可讓硬體層級資源最佳化，否則會需要在核心層級完成額外工作。 排程在相同實體核心上不同邏輯核心的兩個執行緒必須共用 CPU 時間，但比起將它們排程到同一個邏輯核心，執行起來會更有效率。

### <a name="systemcpusetinformation"></a>SYSTEM_CPU_SET_INFORMATION

此資料結構每個執行個體內的資訊 (由 **GetSystemCpuSetInformation** 傳回) 包含可能在上面排程執行緒的唯一處理單元資訊。 基於目標裝置的可能範圍，**SYSTEM_CPU_SET_INFORMATION** 資料結構中的許多資訊可能不適用於遊戲開發。 表 1 提供適用於遊戲開發的資料成員說明。

 **表 1. 適用於遊戲開發的資料成員。**

| 成員名稱  | 資料類型 | 說明 |
| ------------- | ------------- | ------------- |
| Type  | CPU_SET_INFORMATION_TYPE  | 結構中的資訊類型。 如果這個值不是 **CpuSetInformation**，則應該忽略。  |
| Id  | unsigned long  | 指定的 CPU 集合識別碼。 這是應該要搭配 CPU 集合函式 (例如 **SetThreadSelectedCpuSets**) 使用的識別碼。  |
| Group  | unsigned short  | 指定 CPU 集合的「處理器群組」。 處理器群組可讓電腦擁有超過 64 個邏輯核心，並允許在系統執行時進行 CPU 熱交換。 非伺服器電腦配備超過一個群組的狀況是很少見的。 除非您正在撰寫的應用程式是在大型的伺服器或伺服器陣列上執行，否則最好使用單一群組中的 CPU 集合，因為大部分的消費者電腦只會有一個處理器群組。 此結構中的所有其他值都與 Group 有關。  |
| LogicalProcessorIndex  | unsigned char  | CPU 集合的群組相關索引  |
| CoreIndex  | unsigned char  | CPU 集合所在位置之實體 CPU 核心的群組相關索引  |
| LastLevelCacheIndex  | unsigned char  | 和此 CPU 集合關聯之上次快取的群組相關索引 除非系統使用 NUMA 節點，否則這是最慢的快取，通常是 L2 或 L3 快取。  |

<br />

其他資料成員提供的資訊不太可能會描述消費者電腦或其他消費者裝置中的 CPU，所以不太可能是有用的資訊。 傳回之資料所提供的資訊可接著用來以各種方式組織執行緒。 此白皮書中[遊戲開發的考量](#considerations-for-game-development)一節，詳細說明利用此資料來最佳化執行緒配置的幾種方式。

以下是從各種不同類型硬體上執行的 UWP 應用程式所收集的一些資訊類型範例。

**表 2. 從 Microsoft Lumia 950 上執行的 UWP app 傳回的資訊。 這是使用多個末級快取的系統範例。 Lumia 950 配備 Qualcomm 808 Snapdragon 處理器，該處理器包含雙核心 ARM Cortex A57 和四核心 ARM Cortex A53 CPU。**

  ![表 2](images/cpusets-table2.png)

**表 3. 從一般電腦上執行的 UWP app 所傳回的資訊。 這是使用超執行緒的系統範例；每個實體核心具有兩個邏輯核心，可供在上面排程執行緒。 在此案例中，系統包含一顆 Intel Xenon CPU E5-2620。**

  ![表 3](images/cpusets-table3.png)

**表 4. 從四核心 Microsoft Surface Pro 4 上執行的 UWP app 傳回的資訊。 這個系統配備一顆 Intel Core i5-6300 CPU。**

  ![表 4](images/cpusets-table4.png)

### <a name="setthreadselectedcpusets"></a>SetThreadSelectedCpuSets

現在與 CPU 集合有關的資訊已可使用，可用來組織執行緒。 利用 **CreateThread** 建立的執行緒控制代碼，會與可在上面排程執行緒之 CPU 集合的識別碼陣列一起傳遞到此函式中。 其使用方式的其中一個範例如以下程式碼所示。

```
HANDLE audioHandle = CreateThread(nullptr, 0, AudioThread, nullptr, 0, nullptr);
unsigned long cores [] = { cpuSets[0].CpuSet.Id, cpuSets[1].CpuSet.Id };
SetThreadSelectedCpuSets(audioHandle, cores, 2);
```
在此範例中，根據函式所建立的執行緒是宣告為 **AudioThread**。 然後，此執行緒可排程到兩個 CPU 集合的其中之一。 CPU 集合的執行緒擁有權不是專屬的。 在未鎖定到特定 CPU 集合的情況下所建立的執行緒，可能會佔用 **AudioThread** 的時間。 同樣地，其他已建立的執行緒也可能會在稍後鎖定到這些 CPU 集合的其中之一或兩者。

### <a name="setprocessdefaultcpusets"></a>SetProcessDefaultCpuSets

與 **SetThreadSelectedCpuSets** 相反的是 **SetProcessDefaultCpuSets**。 當執行緒建立後，它們就不需要鎖定到特定的 CPU 集合。 如果您不想要這些執行緒在特定 CPU 集合 (例如轉譯執行緒或音訊執行緒所使用的 CPU 集合) 上執行，您可以使用此函式指定允許在哪些核心上面排程這些執行緒。

## <a name="considerations-for-game-development"></a>遊戲開發的考量

如我們所了解，在使用 CPUSets API 排程執行緒時，它可以提供許多資訊與彈性。 相較於透過由下而上的方法來嘗試尋找此資料的用法，以由上到下的方式尋找如何配合一般案例使用資料會比較有效率。

### <a name="working-with-time-critical-threads-and-hyperthreading"></a>使用時效性執行緒與超執行緒

若您的遊戲有幾個執行緒必須即時和其他需要相對較少 CPU 時間的背景工作執行緒搭配執行，這個方法很有效。 某些工作 (例如連續的背景音樂) 必須不間斷執行，以最佳化遊戲體驗。 即使有任一畫面格發生音訊執行緒耗盡，都可能會導致跳動或不順的情況，因此每個畫面格都接收到必要的 CPU 時間量是非常重要的。

使用 **SetThreadSelectedCpuSets** 搭配 **SetProcessDefaultCpuSets** 可確保您的重要執行緒維持不被任何背景工作執行緒中斷。 **SetThreadSelectedCpuSets** 可用來將您的大量執行緒指派到特定 CPU 集合。 **SetProcessDefaultCpuSets** 可接著用來確保任何未指派的已建立執行緒都會放置在其他 CPU 集合上。 如果是使用超執行緒的 CPU，考慮相同實體核心上的邏輯核心也很重要。 如果您要執行的執行緒具有即時回應性，那麼就不應該允許背景工作執行緒在與其共用相同實體核心的邏輯核心上執行。 下列程式碼示範如何判斷電腦是否使用超執行緒。

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation( nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data( new uint8_t[retsize] );
if ( !GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>( data.get() ),
    retsize, &retsize, GetCurrentProcess(), 0) )
{
    // Error!
}
 
std::set<DWORD> cores;
std::vector<DWORD> processors;
uint8_t const * ptr = data.get();
for( DWORD size = 0; size < retsize; ) {
    auto info = reinterpret_cast<const SYSTEM_CPU_SET_INFORMATION*>( ptr );
    if ( info->Type == CpuSetInformation ) {
         processors.push_back( info->CpuSet.Id );
         cores.insert( info->CpuSet.CoreIndex );
    }
    ptr += info->Size;
    size += info->Size;
}
 
bool hyperthreaded = processors.size() != cores.size();
```

如果系統使用超執行緒，預設 CPU 集合的集合中不得包含位於和任何即時執行緒所在之相同實體核心上的邏輯核心。 如果系統並未使用超執行緒，則僅需確定預設 CPU 集合不包含和執行音訊執行緒之 CPU 集合相同的核心。

根據實體核心所組織之執行緒的範例，可在[額外資源](#additional-resources)區段中連結之 GitHub 儲存機制上的 CPUSets 範例中找到。

### <a name="reducing-the-cost-of-cache-coherence-with-last-level-cache"></a>利用末級快取降低快取一致性成本

快取一致性是一項概念，代表橫跨多種硬體資源，在相同資料上動作的快取記憶體相同。 如果在不同核心上排程執行緒，但使用相同資料，它們可能會在不同的快取中使用個別的資料複本。 為了取得正確的結果，這些快取必須保持彼此間的一致性。 維護多個快取之間的一致性相當耗費資源，但對於任何多核心系統的運作而言是必要的。 此外，它完全不受用戶端程式碼控制；基礎系統會存取核心之間的共用記憶體資源，獨立運作以維持快取的最新狀態。

如果您的遊戲有共用特別大量資料的多個執行緒，您可以透過確認它們是否是排程在共用末級快取的 CPU 集合上，來將快取一致性成本最小化。 末級快取是最慢的快取，可供不使用 NUMA 節點的系統核心使用。 對於遊戲電腦來說，使用 NUMA 節點非常罕見。 如果核心不共用末級快取，維護一致性會需要存取更高層級 (因而更慢) 的記憶體資源。 如果兩個執行緒在任何指定的時間範圍內都不需要超過 50% 的時間，那麼相較於將它們排程到個別的實體核心上，將兩個執行緒鎖定到共用快取與實體核心的個別 CPU 集合上可提供更好的效能。 

這個程式碼範例說明如何判斷經常通訊的執行緒是否可以共用末級快取。

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation(nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data(new uint8_t[retsize]);
if (!GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get()),
    retsize, &retsize, GetCurrentProcess(), 0))
{
    // Error!
}
 
unsigned long count = retsize / sizeof(SYSTEM_CPU_SET_INFORMATION);
bool sharedcache = false;
 
std::map<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> cachemap;
for (size_t i = 0; i < count; ++i)
{
    auto cpuset = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get())[i];
    if (cpuset.Type == CPU_SET_INFORMATION_TYPE::CpuSetInformation)
    {
        if (cachemap.find(cpuset.CpuSet.LastLevelCacheIndex) == cachemap.end())
        {
            std::pair<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> newvalue;
            newvalue.first = cpuset.CpuSet.LastLevelCacheIndex;
            newvalue.second.push_back(cpuset);
            cachemap.insert(newvalue);
        }
        else
        {
            sharedcache = true;
            cachemap[cpuset.CpuSet.LastLevelCacheIndex].push_back(cpuset);
        }
    }
}
```

圖 1 中所示的快取配置，是您可能在系統中看到的配置類型範例。 下圖是在 Microsoft Lumia 950 中找到的快取圖例。 在 CPU 256 與 CPU 260 之間發生的內部執行緒通訊會產生大量的額外負荷，因為它需要系統維持 L2 快取一致性。

**圖 1. 在 Microsoft Lumia 950 裝置上找到的快取架構。**

![Lumia 950 快取](images/cpusets-lumia950cache.png)

## <a name="summary"></a>摘要

適用於 UWP 開發的 CPUSets API 提供大量的資訊和控制多執行緒處理的選項。 相較於之前適用於 Windows 開發的多執行緒 API，新增的彈性具有一些學習曲線，但增加的彈性最終可在各種消費者電腦和其他硬體目標上有更佳的效能。

## <a name="additional-resources"></a>其他資源
- [CPU 集合 (MSDN)](https://msdn.microsoft.com/library/windows/desktop/mt186420(v=vs.85).aspx)
- [ATG 提供的 CPUSets 範例](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/CPUSets)
- [Xbox One 上的 UWP](index.md)

