---
title: 具有 Node.js 的 Docker 容器
description: 此逐步指南可協助您開始使用 Docker 容器搭配您的 Node.js 應用程式。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 5a6ba80f96410ead8195e6175063916d2519ed91
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254558"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>開始在 Node.js 中使用 Docker 容器

此逐步指南可協助您開始使用 Docker 容器搭配您的 Node.js 應用程式。

## <a name="prerequisites"></a>必要條件

本指南假設您已經完成[使用 WSL 2 設定您的 Node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建 18932 或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 發行版本 (我們的範例適用 Ubuntu 18.04)。 您可以使用下列方式進行檢查：`wsl lsb_release -a`。
- 確保 Ubuntu 18.04 發行版本是在 WSL 2 模式下執行。 (WSL 可以在 v1 或 v2 模式中執行發行版本。)您可藉由開啟 PowerShell 並輸入下列內容進行檢查：`wsl -l -v`。
- 使用 PowerShell，將 Ubuntu 18.04 設定為預設發行版本，請透過：`wsl -s ubuntu 18.04`。

## <a name="overview-of-docker-containers"></a>Docker 容器概觀

**Docker** 是一種工具，可用來建立、部署及執行使用容器的應用程式。 容器可讓開發人員封裝含有其所需全部元件 (程式庫、架構、相依性等) 的應用程式，且全部以一個套件的形式出貨。 使用容器可確保應用程式會以相同的方式執行，而不管任何自訂的設定或先前在執行該應用程式的電腦 (可能與用來撰寫和測試應用程式程式碼的電腦不同) 上安裝的程式庫。 這可讓開發人員專注於撰寫程式碼，而不需擔心程式碼將在其上執行的系統。

Docker 容器類似於虛擬機器，但不會建立整個虛擬作業系統。 然而，Docker 可讓應用程式使用與系統執行所在的相同 Linux 核心。 這可讓應用程式套件只需要主機電腦上尚未存在的元件，以減少套件大小並提升效能。

使用 Docker 容器搭配 [Kubernetes](/azure/aks/) 之類工具的持續可用性，是容器廣受歡迎的另一個原因。 這可讓您在不同時間建立多個版本的應用程式容器。 每個容器 (及其特定微服務) 都可以隨時被取代，而不需要關閉整個系統以便進行更新或維護。 您可以準備包含所有更新的新容器、設定用於生產的容器，並在準備就緒後直接指向新容器。 您也可使用容器來封存不同版本的應用程式，並視需要使其以安全後援的形式執行。

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>安裝 Docker Desktop WSL 2 Tech Preview

先前，WSL 1 無法直接執行 Docker 精靈，但 WSL 2 已變更這點並使 Docker Desktop for WSL 2 的速度和效能大幅改進。

若要安裝及執行 Docker Desktop WSL 2 Tech Preview：

1. 下載 [Docker Desktop WSL 2 Tech Preview 安裝程式](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)。 (如有需要，您可參考[安裝程式文件](https://docs.docker.com/docker-for-windows/wsl-tech-preview/))。

2. 開啟您剛才下載的 Docker 安裝程式。 安裝精靈會詢問您是否要「使用 Windows 容器，而非 Linux 容器」 - 不要勾選此項，因為我們將使用 Linux 子系統。 Docker 將會安裝於預設 WSL 2 發行版本的受控目錄中，並將包含 Docker 精靈、CLI 和 Compose CLI。

    ![[安裝 Docker 桌面] 精靈 [設定] 頁面的螢幕擷取畫面，其中顯示已選取將捷徑新增至桌面選項。](../images/install-docker-1.png)

3. 如果您還沒有 Docker 識別碼，則必須造訪 [https://hub.docker.com/signup](https://hub.docker.com/signup) 進行設定。 您的識別碼必須全部是小寫的英數位元。

4. 安裝之後，請選取桌面上的捷徑圖示，或在您的 Windows [開始] 功能表中尋找，以啟動 Docker Desktop。 Docker 圖示會出現在工作列上隱藏的圖示功能表中。 在圖示上按一下滑鼠右鍵以顯示 Docker 命令功能表，然後選取 [WSL 2 Tech Preview]。

5. 在技術預覽版視窗開啟後，選取 [開始]  以開始在 WSL 2 中執行 Docker 精靈 (背景程序)。 當 WSL 2 Docker 精靈啟動時，系統會自動為其建立 Docker CLI 內容。

    ![顯示如何啟動 Docker 技術預覽的短片。](../images/start-docker.gif)

6. 若要確認已安裝 Docker並顯示版本號碼，請開啟命令列 (WSL 或 PowerShell) 並輸入：`docker --version`

7. 執行簡單的內建 Docker 映像，測試您的安裝是否運作正常：`docker run hello-world`

以下是您應該知道的幾個 Docker 命令：

- 輸入以下命令可列出 Docker CLI 中可用的命令：`docker`
- 使用以下命令，列出特定命令的資訊：`docker <COMMAND> --help`
- 使用以下命令，列出電腦上的 Docker 映像 (此時就是 hello-world 映像)：`docker image ls --all`
- 使用以下命令，列出電腦上的容器：`docker container ls --all`
- 使用以下命令，列出 WSL 2 內容中您可使用的 Docker 系統統計資料和資源 (CPU 與記憶體)：`docker info`
- 使用以下命令，顯示 Docker 目前執行所在的位置：`docker context ls`

您可以看到 Docker 在兩個環境中執行 - -`default` (傳統 Docker 精靈) 和 `wsl` (我們建議使用技術預覽版)。 (此外，`ls` 命令是 `list` 的縮寫，可以交換使用)。

![Powershell 中的 Docker 顯示內容](../images/docker-context.png)

> [!TIP]
> 嘗試使用此 [Docker Hub 教學課程](https://hub.docker.com/?overlay=onboarding)來建置範例 Docker 映像。 Docker Hub 也包含數千個開放原始碼映像，這些映像可能符合您要容器化的應用程式類型。 您可下載映像 (例如此 [Gatsby.js 架構容器](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds)或此 [Nuxt.js 架構容器](https://hub.docker.com/r/hobord/nuxtexpress))，並使用自己的應用程式代碼加以擴充。 您可以[從命令列使用 Docker](https://docs.docker.com/engine/reference/commandline/search/) 或使用 [Docker Hub 網站](https://hub.docker.com/search/?type=image)來搜尋登錄。

## <a name="install-the-docker-extension-on-vs-code"></a>在 VS Code 上安裝 Docker 擴充功能

Docker 擴充功能可讓您輕鬆地從 Visual Studio Code 建置、管理及部署容器化應用程式。

1. 在 VS Code 中開啟 [擴充功能]  視窗 (Ctrl+Shift+X)，然後搜尋 **Docker**。

2. 選取 [Microsoft Docker 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)並進行 **安裝**。 您必須在安裝之後重新載入 VS Code，才能啟用擴充功能。

    ![VS Code 上採用 Remote-WSL 的 Docker 擴充功能](../images/docker-vscode-extension.png)

在 VS Code 上安裝 Docker 擴充功能，您現在就能夠在下一節中使用快速鍵來顯示 `Dockerfile` 命令的清單：`Ctrl+Space`

深入了解如何[在 VS Code 中使用 Docker](https://code.visualstudio.com/docs/azure/docker)。

## <a name="create-a-container-image-with-dockerfile"></a>使用 DockerFile 建立容器映像

**容器映像** 可儲存您的應用程式程式碼、程式庫、組態檔、環境變數及執行階段。 使用映像可確保容器中的環境已標準化，而且只包含建置和執行應用程式所需的項目。

**DockerFile** - 包含建置新容器映像所需的指示。 換句話說，此檔案會建置可定義應用程式環境的容器映像，以便在任何地方重現。

讓我們使用 [Web 架構](./web-frameworks.md)指南中設定的 Next.js 應用程式來建置容器映像。

1. 在 VS Code 中開啟 Next.js 應用程式 (確保 Remote-WSL 擴充功能正在執行，如左下方綠色標籤所示)。 開啟 VS Code 整合中的 WSL 終端機 ([檢視] > [終端機]  )，並確定終端機路徑指向您的 Next.js 專案目錄 (即 `~/NextProjects/my-next-app$`)。

2. 在 Next.js 專案的根目錄中，建立名為 `Dockerfile` 的新檔案，然後新增下列內容：

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. 若要建置 Docker 映像，請從專案的根目錄執行下列命令 (但以您在 Docker Hub 上建立的使用者名稱取代 `<your_docker_username>`)：`docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker 必須使用 WSL Tech Preview 執行，此命令才能運作。 如需如何啟動 Docker 的提醒，請參閱安裝一節的[步驟 4](#install-docker-desktop-wsl-2-tech-preview)。 `-t` 旗標會指定要建立的映像名稱，在我們的案例中為 "my-nextjs-app:v1"。 我們建議您在建立映像時，一律[使用標記名稱上版本號碼](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375)。 請務必在命令的結尾包含句點，這會指定目前的工作目錄應用於尋找並複製 Next.js 應用程式的組建檔案。

4. 若要在容器中執行 Next.js 應用程式的新 Docker 映像，請輸入下列命令：`docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. `-p` 旗標會將連接埠 '3000' (應用程式在容器內執行的連接埠) 繫結至機器上的本機連接埠 '3333'，因此您現在可以將網頁瀏覽器指向 [http://localhost:3333](http://localhost:3333)，並查看以 Docker 容器映像形式執行的伺服器端轉譯 Next.js 應用程式。

> [!TIP]
> 我們使用了 `FROM node:12` 建置容器映像，其會參考 Docker Hub 上儲存的 Node.js 第 12 版預設映像。 此預設 Node.js 映像是以 Debian/Ubuntu Linux 系統為基礎，有許多不同的 Node.js 映像可供選擇，但您可能會考慮使用更輕量型或針對您的需求量身訂做的映像。 若要深入了解，請參閱 [Docker Hub 上的 Node.js 映像登錄](https://hub.docker.com/_/node/)。

## <a name="upload-your-container-image-to-a-repository"></a>將您的容器映像上傳至存放庫

**容器存放庫** 會將您的容器映像儲存在雲端。 容器存放庫通常會包含相關映像的集合 (例如不同版本)，這些映像全都可用於輕鬆設定和快速部署。 一般來說，您可透過安全的 HTTPs 端點來存取容器存放庫上的映像，進而讓您透過任何系統、硬體或 VM 執行個體來提取、推送或管理映像。

另一方面，**容器登錄** 會儲存存放庫的集合，以及索引、存取控制規則和 API 路徑。 您可以公開或私下裝載這些項目。 [Docker Hub](https://hub.docker.com/) 是一個開放原始碼的 Docker 登錄，以及在執行 `docker push` 和 `docker pull` 命令時所使用的預設值。 公用存放庫免費，而私人存放庫則需要付費。

若要將新的容器映像上傳至 Docker Hub 上裝載的存放庫：

1. 登入 Docker Hub。 系統會提示您輸入在安裝步驟中用來建立 Docker Hub 帳戶的使用者名稱和密碼。 若要在您的終端機中登入 Docker，請輸入：`docker login`

2. 若要取得您已在電腦上建立的 Docker 容器映像清單，請輸入：`docker image ls --all`

3. 將您的容器映像推送至 Docker Hub，並使用以下命令在該處為其建立新的存放庫：`docker push <your_docker_username>/my-nextjs-app:v1`

4. 您現在可造訪以下網址，以在 Docker Hub 上檢視您的存放庫、輸入描述，並連結您的 GitHub 帳戶 (如有需要)： https://cloud.docker.com/repository/list

5. 您也可使用以下命令來檢視作用中的 Docker 容器清單：`docker container ls` (或 `docker ps`)

6. 您應會看到 "my-nextjs-app:v1" 容器在連接埠 3333->3000/tcp 上作用。 您也可查看此處所列的「容器識別碼」。 若要停止執行您的容器，請輸入以下命令：`docker stop <container ID>`

7. 一般來說，停止容器後，也應該將其移除。 移除容器可清除其留下來的任何資源。 移除容器後，您在其映像檔案系統內所做的任何變更都會永久遺失。 您需要建置新的映像來代表變更。 若要移除容器，請使用 命令：`docker rm <container ID>`

深入了解如何[使用 Docker 建置容器化 Web 應用程式](/learn/modules/intro-to-containers/)。

## <a name="deploy-to-azure-container-registry"></a>部署至 Azure Container Registry

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) 可讓您在私人、已驗證的存放庫中，安全地儲存、管理及保留容器映應。 ACR 與標準 Docker 命令相容，可為您處理重要工作 (例如容器健康情況監視和維護)，並與 [Kubernetes](/azure/aks/intro-kubernetes) 配對，以建立可調整的協調流程系統。 視需要建置，或透過原始程式碼認可和基底映像更新等觸發程序，使建置完全自動化。 ACR 也會利用龐大的 Azure 雲端網路來管理網路延遲、全球部署，並且為任何使用 [Azure App Service](/azure/app-service/) (適用於 Web 裝載、行動後端、REST API) 或[其他 Azure 雲端服務](https://azure.microsoft.com/product-categories/containers/)的人員，建立順暢的原生體驗。

> [!IMPORTANT]
> 您需要自己的 Azure 訂用帳戶，才能將容器部署至 Azure，而可能會收到費用。 如果您還沒有 Azure 訂用帳戶，請在開始前[建立免費帳戶](https://azure.microsoft.com/free/)。

如需建立 Azure Container Registry 及部署應用程式容器映像的協助，請參閱練習：[將 Docker 映像部署至 Azure 容器執行個體](/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)。

## <a name="additional-resources"></a>其他資源

- [Azure 上的 Node.js](https://azure.microsoft.com/develop/nodejs/)
- 快速入門：[在 Azure 中建立 Node.js Web 應用程式](/azure/app-service/app-service-web-get-started-nodejs)
- 線上課程：[容器 Azure 中的容器](/learn/paths/administer-containers-in-azure/)
- 使用 VS Code：[使用 Docker](https://code.visualstudio.com/docs/azure/docker)
- Docker 文件：[Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)