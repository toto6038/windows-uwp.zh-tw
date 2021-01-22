---
title: Windows 10 的 Powertoy PowerRename 公用程式
description: 用於大量重新命名檔案的 windows shell 擴充功能
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 39e06685b6948ed3d3935c69a8b4dafeb9ecc2ea
ms.sourcegitcommit: 8040760f5520bd1732c39aedc68144c4496319df
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2021
ms.locfileid: "98691333"
---
# <a name="powerrename-utility"></a>PowerRename 公用程式

PowerRename 是一種大量重新命名工具，可讓您：

- 修改大量檔案 (的檔案名 *，而不將所有檔案重新命名為相同的名稱)*。
- 在檔案名的目標區段上執行搜尋和取代。
- 在多個檔案上執行正則運算式重新命名。
- 在完成大量重新命名之前，請先在預覽視窗中檢查預期的重新命名結果。
- 在完成重新命名作業之後復原。

## <a name="demo"></a>示範

在此示範中，會以 "Pamplona" 取代檔案名 "Pampalona" 的所有實例。 由於所有檔案都是唯一的名稱，因此這可能會花很長的時間以手動方式逐一完成。 PowerRename 可啟用單一大量重新命名。 請注意，[復原重新命名] (Ctrl + Z) 命令可讓您復原變更。

![PowerRename 示範](../images/powerrename-demo.gif)

## <a name="powerrename-menu"></a>PowerRename 功能表

選取 [Windows 檔案總管中的某些檔案之後，以滑鼠右鍵按一下並選取 [ *PowerRename* ] (只有在 powertoy) 中啟用時，才會顯示 [PowerRename] 功能表。 您所選取) 的專案數目 (將會顯示，以及搜尋和取代值、選項清單，以及顯示搜尋結果和取代您所輸入之值的預覽視窗。

![PowerRename 功能表螢幕擷取畫面](../images/powerrename-menu.png)

### <a name="search-for"></a>搜尋

輸入文字或 [正則運算式](https://wikipedia.org/wiki/Regular_expression) ，以在您選取專案中尋找包含符合您專案的準則的檔案。 您將會在 *預覽* 視窗中看到相符的專案。

### <a name="replace-with"></a>更換為

輸入文字來取代先前輸入且符合您所選檔案的值 *搜尋* 值。 您可以在 [ *預覽* ] 視窗中，查看原始檔案名稱和重新命名的檔案。

### <a name="options---use-regular-expressions"></a>選項-使用正則運算式

若選取此選項，則會將搜尋值視為 [正則運算式](https://wikipedia.org/wiki/Regular_expression) (RegEx) 。 取代值也可以包含 RegEx 變數 (請參閱以下) 範例。  如果未核取，則會將搜尋值轉譯為純文字，以取代欄位中的文字取代。

### <a name="options---case-sensitive"></a>選項-區分大小寫

如果選取此選項，則在 [搜尋] 欄位中指定的文字只會比對專案中的文字（如果文字是相同的大小寫）。 大小寫比對不區分 (不會辨識大寫字母和小寫字母) 預設值之間的差異。

### <a name="options---match-all-occurrences"></a>選項-符合所有出現專案

若選取此選項，則會以取代文字取代搜尋欄位中的所有文字相符專案。  否則，只有在檔案名中搜尋文字的第一個實例， (由左到右的) 取代。

例如，假設 `powertoys-powerrename.txt` 有下列檔案名：

- 搜尋： `power`
- 以下列方式重新命名： `super`

重新命名的檔案值會導致：

- 符合未核取的所有出現 () ： `supertoys-powerrename.txt`
- 符合所有出現的 (檢查) ： `supertoys-superrename.txt`

### <a name="options---exclude-files"></a>選項-排除檔案

檔案將不會包含在作業中。 只會包含資料夾。

### <a name="options---exclude-folders"></a>選項-排除資料夾

資料夾將不會包含在作業中。 只會包含檔案。

### <a name="options---exclude-subfolder-items"></a>選項-排除子資料夾專案

資料夾內的專案將不會包含在作業中。 依預設，會包含所有子資料夾專案。

### <a name="options---enumerate-items"></a>選項-列舉專案

將數值尾碼附加至作業中所修改的檔案名。 例如：`foo.jpg` -> `foo (1).jpg`

### <a name="options---item-name-only"></a>選項-僅限專案名稱

作業只會修改檔案名部分 (不是副檔名) 。 例如：`txt.txt` ->  `NewName.txt`

### <a name="options---item-extension-only"></a>選項-僅限專案延伸

作業只會修改副檔名部分 (不是檔案名) 。 例如：`txt.txt` -> `txt.NewExtension`

## <a name="replace-using-file-creation-date-and-time"></a>取代使用檔案建立日期和時間

您可以根據下表輸入變數模式，在 [ *取代成* 文字] 中使用檔案的建立日期和時間屬性。

變數模式 |說明
|:---|:---|
|`$YYYY`|以完整四或五位數表示的年份，取決於所使用的行事曆。
|`$YY`|只以最後兩位數表示的年份。 會為單一位數年份新增前置零。
|`$Y`|只以最後一個位數表示的年份。
|`$MMMM`|月份名稱
|`$MMM`|月份的縮寫名稱
|`$MM`|以月為單位的數位，表示單一位數月份的前置零。
|`$M`|以月為單位的數位，不含前置零的數位月份。
|`$DDDD`|一周中的星期幾名稱
|`$DDD`|星期幾的縮寫名稱
|`$DD`|一個月中的第幾天，表示一位數的天數，前置零。
|`$D`|一個月中的日，表示單一位數的天數沒有前置零的數位。
|`$hh`|單一位數小時的前置零小時
|`$h`|單一位數小時沒有前置零的小時數
|`$mm`|在單一位數分鐘內使用前置零的分鐘數。
|`$m`|單一位數分鐘沒有前置零的分鐘數。
|`$ss`|以前置零表示的秒數（以秒為單位）。
|`$s`|單一位數秒沒有前置零的秒數。
|`$fff`|以完整三位數表示的毫秒數。
|`$ff`|只有前兩位數表示的毫秒數。
|`$f`|只以第一個數位表示的毫秒數。

例如，假設有檔案名：

- `powertoys.png`，建立于11/02/2020
- `powertoys-menu.png`，建立于11/03/2020

輸入要重新命名專案的準則：

- 搜尋： `powertoys`
- 以下列方式重新命名： `$MMM-$DD-$YY-powertoys`

重新命名的檔案值會導致：

- `Nov-02-20-powertoys.png`
- `Nov-03-20-powertoys-menu.png`

## <a name="regular-expressions"></a>[規則運算式]

在大部分的使用案例中，簡單的搜尋和取代都已足夠。 不過，有時候可能會有複雜的重新命名工作需要更多控制。 [正則運算式](https://wikipedia.org/wiki/Regular_expression) 可提供協助。

正則運算式會定義文字的搜尋模式。 可以用來搜尋、編輯和操作文字。 正則運算式所定義的模式可能會比對指定字串一次或完全不符合。 PowerRename 使用 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 文法，這在新式程式設計語言中很常見。

若要啟用正則運算式，請選取 [使用正則運算式] 核取方塊。

**注意：** 使用正則運算式時，您可能會想要選取 [符合所有出現專案]。

### <a name="examples-of-regular-expressions"></a>正則運算式的範例

#### <a name="simple-matching-examples"></a>簡單的相符範例

| 搜尋       | Description                                           |
| ---------------- | ------------- |
| `^`              | 符合檔案名開頭                   |
| `$`              | 符合檔案名的結尾                         |
| `.*`             | 符合名稱中的所有文字                        |
| `^foo`           | 比對以 "foo" 開頭的文字                     |
| `bar$`           | 比對結尾為 "bar" 的文字                       |
| `^foo.*bar$`     | 比對開頭為 "foo" 且結尾為 "bar" 的文字 |
| `.+?(?=bar)`     | 比對所有專案，最多為 "bar"                          |
| `foo[\s\S]*bar`  | 符合 "foo" 和 "bar" 之間的所有內容              |

#### <a name="matching-and-variable-examples"></a>相符和變數範例

*使用變數時，必須啟用 [符合所有出現專案] 選項。*

| 搜尋   | 取代為    | Description                                |
| ------------ | --------------- |--------------------------------------------|
| `(.*).png`   | `foo_$1.png`   | 在現有的檔案名前面加上 "foo \_ " |
| `(.*).png`   | `$1_foo.png`   | 將 " \_ foo" 附加至現有的檔案名  |
| `(.*)`       | `$1.txt`        | 將 ".txt" 副檔名附加至現有的檔案名 |
| `(^\w+\.$)|(^\w+$)` | `$2.txt` | 只有在沒有副檔名時，才會將 ".txt" 副檔名附加至現有的檔案名 |

### <a name="additional-resources-for-learning-regular-expressions"></a>學習正則運算式的其他資源

您可以從線上取得絕佳的範例/相關工作表，協助您

[Regex 教學課程-依範例的快速速查表](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[ECMAScript 正則運算式教學課程](https://o7planning.org/en/12219/ecmascript-regular-expressions-tutorial)

## <a name="file-list-filters"></a>檔案清單篩選

篩選準則可以在 PowerRename 中用來縮小重新命名的結果範圍。 使用 [ *預覽* ] 視窗檢查預期的結果。 選取要在篩選之間切換的資料行標頭。

- **原始**，在 *預覽* 視窗中的第一個資料行會在下列兩者之間迴圈：
  - Checked：已選取要重新命名的檔案。
  - 未選取：未選取要重新命名的檔案 (即使符合搜尋條件) 中輸入的值也一樣。

- 已重新 **命名**，可以切換 *預覽* 視窗中的第二個數據行。
  - 預設預覽會顯示所有選取的檔案，而且只有符合 *搜尋* 條件的檔案才會顯示更新的重新命名值。
  - 選取 [重新 *命名* ] 標頭將會切換預覽，只顯示將重新命名的檔案。 您將不會看到原始選取範圍中的其他選取檔案。

![Powertoy PowerRename 篩選示範](../images/powerrename-demo2.gif)
