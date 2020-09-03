---
title: 將您的資訊清單提交至存放庫
description: 建立描述應用程式的封裝資訊清單之後，您就可以將資訊清單提交至 Windows 封裝管理員存放庫。
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ef94a77d5012adcedf31ae1ecfddc036bcc3a059
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166502"
---
# <a name="submit-your-manifest-to-the-repository"></a>將您的資訊清單提交至存放庫

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

建立描述應用程式的[封裝資訊清單](manifest.md)之後，您就可以將資訊清單提交至 Windows 封裝管理員存放庫。 這是對外公開的存放庫，其中包含 **winget** 工具可存取的資訊清單集合。 若要提交您的資訊清單，請將其上傳至 GitHub 上的開放原始碼 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 存放庫。

在您提交要將新資訊清單新增至 GitHub 存放庫的提取要求之後，自動化程序將會驗證您的資訊清單檔案，並確認其不是惡意封裝。 如果此驗證成功，您的封裝將會新增至對外公開的 Windows 封裝管理員存放庫，讓 **winget** 用戶端工具可以找到此項目。 請注意資訊清單在開放原始碼 GitHub 存放庫和公開 Windows 封裝管理員存放庫的區別。

> [!IMPORTANT]
> Microsoft 保留因任何原因而拒絕提交的權利。

## <a name="third-party-repositories"></a>第三方存放庫

目前沒有任何已知的第三方存放庫。 Microsoft 正與多個合作夥伴一起開發用來啟用第三方存放庫的通訊協定或 API。

## <a name="manifest-validation"></a>資訊清單驗證

當您將資訊清單提交至 GitHub 上的 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 存放庫時，系統會自動驗證您的資訊清單，並針對 Windows 生態系統的安全性進行評估。 您也可以手動檢查資訊清單。

## <a name="how-to-submit-your-manifest"></a>如何提交您的資訊清單

若要將資訊清單提交至存放庫，請遵循下列步驟。

### <a name="step-1-validate-your-manifest"></a>步驟 1：驗證您的資訊清單

**winget** 工具會提供 [validate](..\winget\validate.md) 命令，確認您已正確建立資訊清單。 若要驗證您的資訊清單，請使用此命令。

```CMD
winget validate \<manifest-file>
```

如果驗證失敗，請使用錯誤來尋找行號並進行更正。 驗證資訊清單之後，您就可以將其提交至存放庫。

### <a name="step-2-clone-the-repository"></a>步驟 2：複製存放庫

接下來，建立存放庫的分叉 (fork) 並加以複製。

1. 在瀏覽器中移至 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)，然後按一下 [分叉]。
    ![分叉的圖片](images\fork.png)

2. 從 Windows 命令提示字元或 PowerShell 之類的命令列環境中，使用下列命令來複製您的分叉 (fork)。
    ```CMD
    git clone \<your-fork-name>
    ```

 3. 如果您要提交多次，請建立一個分支 (branch)，而不是分叉 (fork)。 我們目前只允許每次提交一個資訊清單檔案。
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>步驟 3：將您的資訊清單新增至本機存放庫

您必須將資訊清單檔案新增至採用下列資料夾結構的存放庫：

**資訊清單** / **發行者** / **應用程式** / **version.yaml**

* [資訊清單] 資料夾是存放庫中所有資訊清單的根資料夾。
* [發行者] 資料夾是發行軟體的公司名稱。 例如 **Microsoft**。
* [應用程式] 資料夾是應用程式或工具的名稱。 例如 **VSCode**。
* **version.yaml** 是資訊清單的檔案名稱。 檔案名稱必須設定為目前的應用程式版本。 例如 **1.0.0.yaml**。

>[!IMPORTANT]
> 資訊清單中的 `Id` 值必須符合資訊清單資料夾路徑中的發行者和應用程式名稱，而且資訊清單中的 `version` 值必須符合檔案名稱中的版本。 如需詳細資訊，請參閱[建立您的封裝資訊清單](manifest.md#tips-and-best-practices)。

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>步驟 4：將您的資訊清單提交至遠端存放庫

您現在已可以將新的資訊清單推送至遠端存放庫。

1. 使用 `add` 命令來準備提交。
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. 使用 `commit` 命令來認可變更，並提供有關提交的資訊。
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. 使用 `push` 命令將變更推送至遠端存放庫。
    ```CMD
    git push
    ```

### <a name="step-5-create-a-pull-request"></a>步驟 5：建立提取要求

推送變更之後，請返回 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 並建立提取要求，以將您的分叉 (fork) 或分支 (branch) 合併至**主要**分支。

![提取要求索引標籤的圖片](images\pull-request.png)

## <a name="validation-process"></a>驗證程序

當您建立提取要求時，此動作會啟動自動化程序來驗證資訊清單並處理您的提取要求。 我們會將標籤新增至您的提取要求，以便您追蹤進度。

### <a name="submission-expectations"></a>提交的預期結果

所有提交至 Windows 封裝管理員存放庫的應用程式都應能正常運作。 以下是提交後的一些預期結果：

* 資訊清單會符合[結構描述需求](manifest.md#manifest-contents)。
* 資訊清單中的所有 URL 都會導向安全網站。
* 安裝程式和應用程式都沒有病毒。 此套件可能會被誤認為惡意程式碼。 如果您認為這是誤判，可以將安裝程式提交給 Defender 小組，以從[這裡](https://www.microsoft.com/wdsi/filesubmission)進行分析。
* 應用程式會為系統管理員和非系統管理員進行正確地安裝和卸載。
* 安裝程式支援非互動模式。
* 所有資訊清單項目都是正確的，不會產生誤導。
* 安裝程式直接來自發行者的網站。

### <a name="pull-request-labels"></a>提取要求標籤

在驗證期間，我們會將一系列的標籤套用至提取要求，以傳達進度狀況。

* **Needs: author feedback**：提交失敗。 我們會將提取要求重新指派給您。 如果您未在 10 天內解決此問題，我們將會關閉提取要求。
* **Manifest-Validation-Error**：提交的資訊清單包含語法錯誤。
* **URL-Validation-Error**：提交中的一或多個 URL 未通過 [SmartScreen](/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview) 驗證。
* **Binary-Validation-Error**：已提交的應用程式安裝程式未通過病毒掃描測試，或是雜湊不相符。
* **Pull-Request-Error**:提取要求發生問題。 例如，資料夾結構不是[所需的格式](#step-3-add-your-manifest-to-the-local-repository)。
* **Validation-Error**：已提交的應用程式無法通過一般驗證測試。
* **Validation-Installation-Error**：已提交的應用程式無法通過安裝測試。
* **Validation-Uninstall-Error**：提交的應用程式無法通過卸載測試。
* **Validation-Virus-Scan-Error**：已提交的應用程式無法通過病毒掃描測試。
* **Azure-Pipeline-Passed**：資訊清單已完成驗證的第一個部分。 在此步驟之後，您的提取要求會指派給我們的測試小組以進行最終驗證。
* **Validation-Completed**：驗證已完成，將會合併您的提取要求。