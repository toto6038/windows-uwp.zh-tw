---
title: 管理本機連接的儲存體
description: 了解如何管理在開發環境中的本機連接的儲存體資料。
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 09fce637c50b0a03230d0808e51a9e5cfadc7179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642053"
---
# <a name="managing-local-connected-storage"></a>管理本機連接的儲存體
連接的儲存體是用來將遊戲資料儲存在雲端，還有連接的儲存體服務的本機儲存體元件。 無論您是在電腦或主控台上沒有連接的儲存體資料同步處理至雲端的資料所在的本機快取。 您所建立的 XDK 或 UWP 的標題有這個工具可讓您管理您的本機連接的儲存體資料。

請參閱下表來尋找適當的工具來管理本機快取連接的儲存體資料：

|標題分類  |裝置  |本機儲存體工具  |
|---------|---------|---------|
|XDK     |Xbox One 主控台     |*xbstorage*         |
|UWP     |電腦         |*gamesaveutil*         |
|UWP     |Xbox One 主控台     |Xbox 裝置入口網站 (XDP |)

- *Xbstorage*是命令列工具，管理在本機快取連接一個 Xbox 上的儲存體 XDK 的命令提示字元中執行。 *Xbstorage*工具可在 Xbox One XDK，底下的檔案路徑： **/Program 檔案 (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil*是命令列工具在本機管理 UWP 快取連線電腦上的儲存體。 *Gamesaveutil*工具是隨附[Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) Fall Creators Update 和更新版本 (建置 10.0.16299.15 和更新版本)。 您已安裝適當版本的 Windows 10 SDK，一旦*gamesaveutil*可以在資料夾下找到：**ProgramFiles(x86)/Windows 套件/10/bin / [SDK Version]/x64/gamesaveutil.exe**。
- *Xbox 裝置 Portal(XDP)* 是線上入口網站，可讓您管理在一個 Xbox 主機上的本機快取連接儲存體 UWP 資料。 這篇文章並未涵蓋 XDP 使用量。 若要了解如何使用 XDP 閱讀[裝置入口網站，xbox](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox)。

## <a name="xbstorage"></a>Xbstorage

*Xbstorage*允許清除本機快取的資料連接的儲存體的資料從硬碟機，以及匯入和匯出資料的使用者或電腦從連接的儲存空間藉由使用 XML 檔案。

當使用本機資料上執行作業*xbstorage*工具，系統就會如同該作業必須由執行應用程式本身，因此讀取連接的儲存空間中的資料，本機檔案中的動作將會造成同步處理之前複製的雲端使用。

同樣地，從開發電腦上的 XML 檔案到連接的儲存體容器上的 Xbox One 的開發套件的資料複本將導致，主控台將會開始將該資料上傳至雲端。 不過，在中這不會發生的情況： 如果開發人員套件無法取得鎖定，或如果沒有在主控台上的容器與雲端之間的資料衝突，主控台就會如同使用者已決定不解決的衝突挑選的容器保留，與主控台的一個版本就會如同在下次啟動標題之前，使用者在播放離線。

這些命令的一個例外狀況是**重設** **/force**這所有的使用者以及 SCIDs 清除儲存的資料的本機儲存體，但不會改變儲存在雲端中的資料。 這可用於將主控台放入它可以在使用者的漫遊至主控台並從時播放標題雲端下載資料的狀態。

### <a name="xbstorage-commands"></a>Xbstorage 命令

Xbstorage 具有下列六個命令的開發人員可用來管理其 Xbox 一個開發套件上的本機資料時的 XDK 命令提示字元：

<a id="xbstorage_reset"></a>

|命令  |描述  |
|---------|---------|
|重設    |執行原廠重設連接的儲存體上。         |
|匯入   |從指定的 XML 檔案匯入資料，來連接的儲存空間。         |
|export   |從連接的儲存空間的資料匯出至指定的 XML 檔案中。         |
|[刪除]   |從連接的儲存空間刪除資料。         |
|產生 |會產生空的資料，並將儲存至指定的 XML 檔案。         |
|模擬 |模擬超出儲存體空間的情況。         |

### <a name="xbstorage-reset"></a>Xbstorage 重設

`xbstorage reset [/force]`

清除所有本機連接儲存體中的資料從本機主控台中，還原為原廠設定。 已保存至雲端的資料不會修改，並將會視需要下載一次。

|選項  |描述  |
|---------|---------|
|/force   |確認應該重設連接的儲存體。 執行重設命令，而不指定 **/force**造成會顯示下列訊息： 為連接的儲存體原廠重設為可能破壞性作業，此命令不會執行重設，除非 **/force**旗標會存在。 |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Xbstorage 匯入

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

中指定的資料匯入*filename*連接的儲存體空間。
檔案是 XML 檔案包含的資料。 如需範例，請參閱[xbstorage 產生](#xbstorage_generate)。 如需有關檔案的 XML 格式的詳細資訊，請參閱[匯入和匯出檔案格式](#xbstorage_fileformat)稍後在本主題中。
有兩種方式指定連接的儲存空間：

- 如果輸入的檔案具有**ContextDescription**已正確填入，則這將用來指定連接的儲存體空間的區段。
- 儲存空間也可以部分或完整指定透過命令列參數，會優先於在輸入檔中指定的儲存空間的個別項目。

使用方式的範例：

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> 然後再匯入至指定的連接的儲存空間，系統會嘗試與雲端中使用相同的邏輯連接的儲存空間會取得所執行的應用程式時所要執行同步處理。
>
> 如果具有相同的主要 SCID 的應用程式正在執行，這項作業可能會造成競爭情形，並連接存放裝置空間的內容可能是處於不定狀態。
>
> 如果 **/取代**未指定，在輸入檔中指定的容器將被清除之前撰寫的輸入檔中指定的 blob。 輸入檔中未指定連接的儲存空間中的容器會保持不變。

|選項  |描述  |
|---------|---------|
|*file-name*     |指定包含要匯入資料的 XML 檔案。         |
|/scid:*SCID*    |指定服務設定識別項 (SCID)。         |
|/machine        |指定每台電腦連接的儲存空間。  這個選項不能同時使用 **/msa**選項。         |
|/msa:*account*  |指定要用於每個使用者連接的儲存體帳戶。 使用者必須登入，到主控台以供使用的空間。  這個選項不能同時使用 **/機器**選項。         |
|/replace        |匯入之前，會刪除指定連接的儲存空間中的所有容器。         |
|/verbose        |顯示匯入的狀態。         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Xbstorage 匯出

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

匯出資料至所指定的檔案連接的儲存空間**outfile**。    檔案是 XML 檔案包含的資料。 請參閱[xbstorage 產生](#xbstorage_generate)以了解如何產生的範例。 如需有關檔案的 XML 格式的詳細資訊，請參閱[匯入和匯出檔案格式](#xbstorage_fileformat)稍後在本主題中。 有兩種方式指定連接的儲存空間：

- 如果 **/context**使用參數，並藉由指定的檔名\<infile > 已**ContextDescription**已正確填入，則該檔案會使用指定的區段連接的儲存空間。
- 儲存空間，也可以部分或完整指定透過命令列參數，在個別的項目中指定的儲存空間中的優先順序高於 **/context**檔案。

使用方式的範例：

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> 然後再匯出指定的連接的儲存空間，系統會嘗試與雲端中使用相同的邏輯連接的儲存空間會取得所執行的應用程式時所要執行同步處理。
>
> 如果具有相同的主要 SCID 的應用程式正在執行，這項作業可能會造成競爭情形，並連接存放裝置空間的內容可能是處於不定狀態。

|選項  |描述  |
|---------|---------|
|*outfile*             |XML 檔案資料將匯出到。         |
|/context:*input-file* |指定要讀取的空間資訊的輸入的檔。         |
|/scid:*SCID*          |指定服務設定識別項 (SCID)。         |
|/machine              |指定每台電腦連接的儲存空間。  這個選項不能同時使用 **/msa**選項。         |
|/msa:*account*        |指定要用於每個使用者連接的儲存體帳戶。 使用者必須登入，到主控台以供使用的空間。  這個選項不能同時使用 **/機器**選項。         |
|/verbose              |顯示匯出作業的狀態。         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Xbstorage delete

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

從連接的儲存空間，會刪除所有的資料。
有兩種方式指定連接的儲存空間：

- 如果 **/context**使用參數，並藉由指定的檔名\<infile > 已**ContextDescription**已正確填入，則該檔案會使用指定的區段連接的儲存空間。
- 儲存空間，也可以部分或完整指定透過命令列參數，在個別的項目中指定的儲存空間中的優先順序高於 **/context**檔案。

使用方式的範例：

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> 在刪除之前的指定連接的儲存體空間，系統會嘗試與雲端中使用相同的邏輯連接的儲存空間會取得所執行的應用程式時所要執行同步處理。
>> 如果具有相同的主要 SCID 的應用程式正在執行，這項作業可能會造成競爭情形，並連接存放裝置空間的內容可能是處於不定狀態。

|選項  |描述 |
|---------|---------|
|/context:*input-file*     |指定要讀取的空間資訊的輸入的檔。         |
|/scid:*SCID*              |指定服務設定識別項 (SCID)。         |
|/machine                  |指定每台電腦連接的儲存空間。  這個選項不能同時使用 **/msa**選項。         |
|/msa:*account*            |指定要用於每個使用者連接的儲存體帳戶。 使用者必須登入，到主控台以供使用的空間。  這個選項不能同時使用 **/機器**選項。         |
|/verbose                  |顯示刪除作業的狀態。         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Xbstorage 產生

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

會產生空的資料，並將儲存至指定的檔案\<檔案名稱 >。 如需有關檔案的 XML 格式的詳細資訊，請參閱[匯入和匯出檔案格式](#xbstorage_fileformat)稍後在本主題中。    服務設定識別元 (SCID) 將會設定為 00000000-0000-0000-0000-000000000000，並將每台電腦連接的儲存空間設定的帳戶。 如果您想要變更這些值，您可以直接編輯檔案之後產生。

使用方式的範例：

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> 位元組資料是簡單的遞增順序;比方說，五個位元組的 blob 會有下列的位元組：00 01 02 03 04. >> 如果您想要指定每個使用者連接的儲存空間，變更**帳戶**類似下面的 XML 檔案中的節點：
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|選項  |描述  |
|---------|---------|
|*file-name*     | 將寫入資料的 XML 檔案。 |
|/containers:*n* | 指定的數字*n*，容器產生。 預設計數是 2。  |
|/blobs:*n*      | 指定的數字*n*，要產生的 blob。 預設計數是 3。  |
|/blobsize:*n*   | 指定的數字*n*，每個 blob 的位元組。 預設大小為 1024 個位元組。  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Xbstorage 模擬

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

模擬不足的本機儲存體連接的儲存體服務的情況。

|選項  |描述  |
|---------|---------|
|/reserveremainingspace | 保留連接的儲存體中所有剩餘的空間。 您可以使用的空間將會開啟從 ConnectedStorage 刪除的項目。 |

| / forceoutoflocalstorage |模擬連接的儲存體服務，而此一方有沒有可用的空間。 從連接的儲存體中刪除項目不會變更已連接的儲存體服務從報告記憶體不足。 |

|] / [停止 |停止所有模擬。 |

| /verbose |會顯示模擬作業的狀態。 |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>常見的選項

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|選項  |描述  |
|---------|---------|
| /?                           |  Xbstorage.exe 的顯示說明 |
| /X *:address* [*+accesskey*]  | 指定的主機名稱或位址 (顯示為**工具 IP**主控台上) 的目標的主控台中，但不會變更預設的主控台。 如果您不使用此選項，則會使用預設的主控台。*Accesskey*是一個字串，您可以使用來限制存取了主控台，只知道的存取金鑰的使用者。 使用命令中設定的存取金鑰**xbconfig** **accesskey = * * * 您金鑰*; 然後，重新啟動您的主控台，讓存取金鑰的生效。 若要存取已使用的存取金鑰的主控台，您必須包含一個加號 （+） 和存取金鑰之後在主控台的 IP 位址或主機名稱。
> [!NOTE]
> 如果當 xbconnect 設定預設主控台時，將提供的存取金鑰，做為預設主控台的位址的一部分預存的存取金鑰。
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil*可讓您管理您的應用程式，所有屬於同一個函式，在本機快取連接的儲存體*xbstorage*提供。 就像 xbstorage 工具*gamesaveutil*提供相同的六個資料管理函式，使用一些行為差異。 匯入、 匯出、 刪除和重設命令都需要在 Xbox Live 的使用者登入。 您可以使用在 Windows 10 的 Xbox 應用程式，來檢視和變更目前的使用者。

### <a name="commands"></a>命令

|命令  |描述  |
|---------|---------|
|匯入   |從指定的 XML 檔案匯入資料         |
|export   |將資料匯出到指定的 xml 檔案         |
|[刪除]   |從指定的應用程式刪除資料        |
|重設    |刪除本機資料，只針對指定的應用程式         |
|產生 |會產生空的資料，並將儲存至指定的 xml 檔案         |
|模擬 |模擬特殊狀況很難測試         |

### <a name="gamesaveutil-import"></a>Gamesaveutil 匯入

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

中指定的資料匯入\<檔名 >

檔案是 XML 檔案包含的資料。 型別`gamesaveutil help generate`以了解如何產生的範例。

有兩種方式來指定應用程式 PFN 和 SCID 儲存資料的位置：

如果輸入的檔案具有已正確填入，ContextDescription 區段，則這將用來指定目標應用程式 PFN 和 SCID。

PFN 和 SCID 可以部分或完全透過指定從輸入檔的優先順序高於在個別的項目，以及與的指定的 PFN SCID 的命令列參數。

|選項  |描述  |
|---------|---------|
|/pfn:\<PFN>       |指定要執行的匯入應用程式封裝系列 Name(PFN)。 必須安裝應用程式。         |
|/scid:\<SCID>     |指定服務設定識別項 (SCID)。 這是從您的 Xbox Live 的組態。         |
|/replace         |執行匯入之前刪除所有的容器。         |

使用方式範例：

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 應用程式必須安裝，且已同步處理的資料已經以匯入。
> 
> 如果未指定 /replace，現有的容器不會變更，除非輸入檔中指定它們。

### <a name="gamesaveutil-export"></a>Gamesaveutil 匯出

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

將資料匯出至所指定的檔案\<outfile >。

檔案是 XML 檔案包含的資料。 輸入 gamesaveutil help 產生以了解如何產生的範例。

有兩種方式可指定要匯出的資料的位置：

如果使用 /context 參數，而且所指定的檔名\<infile > 有已正確填入，ContextDescription 區段，則該檔案將用來指定來源資料的位置。

也可以透過命令列參數，在個別的項目 /context 檔所指定的優先順序高於指定的位置。

|選項  |描述  |
|---------|---------|
|/context:\<infile>     |使用指定的檔案來指定應用程式 PFN 和 SCID。         |
|/pfn:\<PFN>            |指定要從匯出的應用程式封裝系列 Name(PFN)。 必須安裝應用程式。       |
|/scid:\<SCID>          |指定服務設定識別項 (SCID)。 這是從您的 Xbox Live 的組態。        |

使用方式範例：

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 應用程式必須安裝，且已同步處理的資料已經才能匯出。

### <a name="gamesaveutil-delete"></a>Gamesaveutil delete

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

指定的 PFN 和 SCID 刪除所有資料。

有兩種方式可指定要刪除之資料的位置：

如果使用 /context 參數，而且所指定的檔名\<infile > 有已正確填入，ContextDescription 區段，則該檔案將用來指定來源資料的位置。

也可以透過命令列參數，在個別的項目 /context 檔所指定的優先順序高於指定的位置。

|選項  |描述  |
|---------|---------|
|/context:\<infile>     |使用指定的檔案來指定應用程式 PFN 和 SCID。         |
|/pfn:\<PFN>            |指定應用程式的套件系列 Name(PFN)，若要刪除的資料。 必須安裝應用程式。       |
|/scid:\<SCID>          |指定服務設定識別項 (SCID)。 這是從您的 Xbox Live 的組態。        |

使用方式範例：

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 必須安裝應用程式，才能刪除容器。

### <a name="gamesaveutil-reset"></a>Gamesaveutil 重設

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

指定的 PFN 和 SCID 會刪除所有本機資料。

有兩種方式可指定要刪除之資料的位置：

如果使用 /context 參數，而且所指定的檔名\<infile > 有已正確填入，ContextDescription 區段，則該檔案將用來指定來源資料的位置。

也可以透過命令列參數，在個別的項目 /context 檔所指定的優先順序高於指定的位置。

|選項  |描述  |
|---------|---------|
|/context:\<infile>     |使用指定的檔案來指定應用程式 PFN 和 SCID。         |
|/pfn:\<PFN>            |指定應用程式的套件系列 Name(PFN)，若要刪除的資料。 必須安裝應用程式。       |
|/scid:\<SCID>          |指定服務設定識別項 (SCID)。 這是從您的 Xbox Live 的組態。        |

使用方式範例：

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 若要刪除本機資料，必須安裝應用程式。

### <a name="gamesaveutil-generate"></a>Gamesaveutil 產生

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

會產生空的資料，並將儲存至指定的檔案\<檔案名稱 >。
服務設定識別項 (SCID) 會設定為 00000000-0000-0000-0000-000000000000。 如果您想要變更這些值的產生之後以手動方式編輯檔案。

|選項  |描述  |
|---------|---------|
|/containers:\<n >     |指定要產生多少容器。 預設值為 2。         |
|/ blob:\<n >          |指定數目的 blob，每個產生的容器。 預設值為 3。        |
|/blobsize:\<n >       |指定每個 blob 的多少個位元組。 預設值為 1024年。         |

使用方式範例：

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> 位元組資料是簡單的遞增順序，也就是五個位元組的 blob 會有 00 01 02 03 04 的位元組。

### <a name="gamesaveutil-simulate"></a>Gamesaveutil 模擬

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

模擬測試特定案例的特殊狀況。

|選項  |描述  |
|---------|---------|
|/markcontainerschanged     |強制從暫停恢復外觀看起來像已經變更，如果應用程式的所有容器，並取得新的提供者。 會影響使用 /stop 清除之前的所有應用程式。      |
|/stop                      |停止所有模擬。         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>匯入和匯出檔案格式

搭配使用的 XML 檔案**匯入**，**匯出**，並**產生**命令搭配*xbstorage*工具有中顯示的格式下列範例。

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

若要格式化的這個 xml 所需的唯一變更**匯入**，**匯出**，並**產生**中*gamesaveutil*是取代\<帳戶 > 成員節點\<ContextDescription > 節點具有\<PackageFamilyName > 節點。
這會變更\<ContextDescription > 從這個節點：

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

為此值：

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> 這些 XML 檔案中的資料格式不是相同的平台上，它是匯入和匯出之用。 應該將它們視為一個中繼或公用程式不是保存格式格式，因此，可能無法在未來，變更這些 XML 檔案的資料格式。

**ContextDescription**是選擇性的節點。 如果您要進行機器連接的儲存空間，您可以使用`<Account machine="true"/>`而不是`<Account msa="user@domain.com"/>`。 否則，內容可以指定匯入的命令列上。
Blob 和容器應該具有對應的名稱提供給他們的遊戲或應用程式所產生的檔案。
每個 blob 的內容應該是字串包裝在**CDATA**標記，以藉由呼叫會產生**CryptBinaryToStringW**旗標**CRYPT_STRING_BASE64**請提供該 blob 的資料做為未經處理位元組陣列。 相反地，blob 資料可以轉換回藉由呼叫**CryptStringToBinary**並提供先前加密的字串。 使用這兩個函數的範例所示[CryptBinaryToString 傳回無效的位元組](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral)Visual Studio 的 MSDN 論壇。

<a id="ID4EWBAE"></a>