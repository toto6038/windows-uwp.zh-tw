---
title: 開始在 node.js 中使用 Docker 容器
description: 此逐步指南可協助您開始使用 Docker 容器搭配您的 node.js 應用程式。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 16b1421606d3c8271141256b80ae2600ec9ca49d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315122"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>開始在 node.js 中使用 Docker 容器

此逐步指南可協助您開始使用 Docker 容器搭配您的 node.js 應用程式。

## <a name="prerequisites"></a>必要條件

本指南假設您已完成[使用 WSL 2 設定 node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建18932或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 散發套件（適用于我們的範例的 Ubuntu 18.04）。 您可以使用下列方式來檢查： `wsl lsb_release -a`。
- 請確定您的 Ubuntu 18.04 散發套件在 WSL 2 模式下執行。 （WSL 可以在 v1 或 v2 模式中執行散發）。您可以藉由開啟 PowerShell 並輸入： `wsl -l -v`來進行檢查。
- 使用 PowerShell，將 Ubuntu 18.04 設定為預設散發，並使用： `wsl -s ubuntu 18.04`。

## <a name="overview-of-docker-containers"></a>Docker 容器的總覽

**Docker**是一種工具，用來建立、部署和執行使用容器的應用程式。 容器可讓開發人員封裝應用程式，其中包含所需的所有元件（程式庫、架構、相依性等），並將它全部以一個套件的形式出貨。 使用容器可確保不論任何自訂設定或先前在執行該應用程式的電腦上安裝的程式庫有何不同，其與用來撰寫和測試應用程式之程式碼的電腦不相同。 這可讓開發人員專注于撰寫程式碼，而不需要擔心程式碼將在其上執行的系統。

Docker 容器類似于虛擬機器，但不會建立整個虛擬作業系統。 取而代之的是，Docker 可讓應用程式使用與執行所在系統相同的 Linux 核心。 這可讓應用程式套件只需要主機電腦上尚未存在的元件，以減少封裝大小並提升效能。

使用 Docker 容器搭配[Kubernetes](https://docs.microsoft.com/azure/aks/)這類工具的持續可用性，是容器熱門程度的另一個原因。 這可讓您在不同的時間建立多個版本的應用程式容器。 並不需要將整個系統用於更新或維護，而是可以隨時取代每個容器（和其特定微服務）。 您可以使用您的所有更新來準備新的容器、設定用於生產的容器，並在準備就緒之後，直接指向新的容器。 您也可以使用容器來封存應用程式的不同版本，並在需要時讓它們以安全的方式執行。

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>安裝 Docker Desktop WSL 2 技術預覽

先前，WSL 1 無法直接執行 Docker daemon，但已變更 WSL 2，並導致 Docker Desktop for WSL 2 的速度和效能大幅改進。

若要安裝及執行 Docker Desktop WSL 2 技術預覽：

1. 下載[Docker DESKTOP WSL 2 Tech Preview 安裝程式](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)。 （如有需要，您可以參考[安裝程式](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)檔）。

2. 開啟您剛才下載的 Docker 安裝程式。 安裝精靈會詢問您是否要「使用 Windows 容器，而非 Linux 容器」-不要勾選此項，因為我們將使用 Linux 子系統。 Docker 將會安裝在預設 WSL 2 發佈的受控目錄中，並將包含 Docker daemon、CLI 和撰寫 CLI。

    ![Docker Desktop 啟動](../images/install-docker-1.png)

3. 如果您還沒有 Docker ID，則必須造訪下列內容來設定一個識別碼： [https://hub.docker.com/signup](https://hub.docker.com/signup)。 您的識別碼必須全部是小寫的英數位元。

4. 安裝之後，請選取桌面上的快捷方式圖示，或在您的 Windows [開始] 功能表中尋找，以啟動 Docker Desktop。 Docker 圖示會出現在工作列的 [隱藏的圖示] 功能表中。 在圖示上按一下滑鼠右鍵以顯示 [Docker 命令] 功能表，然後選取 [WSL 2 Tech Preview]。

5. 在 [技術預覽] 視窗開啟之後，選取 [**開始**] 以開始執行 WSL 2 中的 Docker daemon （背景進程）。 當 WSL 2 docker daemon 啟動時，系統會自動為它建立 docker CLI 內容。

    ![Docker Desktop 啟動](../images/start-docker.gif)

6. 若要確認 Docker 已安裝並顯示版本號碼，請開啟命令列（WSL 或 PowerShell）並輸入： `docker --version`

7. 執行簡單的內建 Docker 映射，測試您的安裝是否正常運作： `docker run hello-world`

以下是幾個您應該知道的 Docker 命令：

- 輸入下列命令，列出 Docker CLI 中可用的命令： `docker`
- 列出具有下列內容的特定命令資訊： `docker <COMMAND> --help`
- 列出電腦上的 docker 映射（此時就是 hello world 映射），其中包含： `docker image ls --all`
- 列出您電腦上的容器，以及： `docker container ls --all`
- 使用下列方式，列出您在 WSL 2 內容中可用的 Docker 系統統計資料和資源（CPU & 記憶體）： `docker info`
- 顯示目前正在執行 docker 的位置，包含： `docker context ls`

您可以看到 Docker 在中執行的兩個內容--`default` （傳統 Docker daemon）和 `wsl` （我們使用技術預覽的建議）。 （此外，`ls` 命令是 `list` 的簡短，而且可以交換使用）。

![Powershell 中的 Docker 顯示內容](../images/docker-context.png)

> [!TIP]
> 請嘗試[在 Docker Hub 上](https://hub.docker.com/?overlay=onboarding)使用本教學課程來建立範例 Docker 映射。 Docker Hub 也包含許多可能符合您想要容器化之應用程式類型的開放原始碼映射。 您可以下載映射，例如這個[Gatsby 架構容器](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds)或這個[Nuxt 架構容器](https://hub.docker.com/r/hobord/nuxtexpress)，然後使用您自己的應用程式代碼來擴充它。 您可以[從命令列](https://docs.docker.com/engine/reference/commandline/search/)或[docker Hub 網站](https://hub.docker.com/search/?type=image)使用 docker 來搜尋登錄。

## <a name="install-the-docker-extension-on-vs-code"></a>在 VS Code 上安裝 Docker 擴充功能

Docker 擴充功能可讓您輕鬆地從 Visual Studio Code 建立、管理和部署容器化應用程式。

1. 開啟 VS Code 中的 [**擴充**功能] 視窗（Ctrl + Shift + X），然後搜尋**Docker**。

2. 選取[Microsoft Docker 擴充](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)功能並**安裝**。 您必須在安裝之後重載 VS Code 以啟用擴充功能。

    ![遠端 WSL 中 VS Code 上的 Docker 擴充功能](../images/docker-vscode-extension.png)

藉由在 VS Code 上安裝 Docker 擴充功能，您現在可以在下一節中使用快捷方式來顯示 `Dockerfile` 命令的清單： `Ctrl+Space`

深入瞭解如何[在 VS Code 中使用 Docker](https://code.visualstudio.com/docs/azure/docker)。

## <a name="create-a-container-image-with-dockerfile"></a>使用 DockerFile 建立容器映像

**容器映射**會儲存您的應用程式程式碼、程式庫、設定檔、環境變數及執行時間。 使用映射可確保容器中的環境是標準化的，而且只包含建立和執行應用程式所需的功能。

**DockerFile**包含建立新容器映射所需的指示。 換句話說，此檔案會建立可定義應用程式環境的容器映射，以便在任何地方重現。

讓我們使用[web](./web-frameworks.md)架構指南中設定的下一個 .js 應用程式來建立容器映射。

1. 在 VS Code 中開啟您的下一個 .js 應用程式（確定 WSL 擴充功能正在執行，如左下方的綠色索引標籤所示）。 開啟 VS Code （**View > terminal**）中整合的 WSL 終端機，並確定終端機路徑指向您的下一個 .js 專案目錄（即 `~/NextProjects/my-next-app$`）。

2. 在您的下一個 .js 專案的根目錄中，建立名為 `Dockerfile` 的新檔案，然後新增下列內容：

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

3. 若要建立 docker 映射，請從專案的根目錄執行下列命令（但使用您在 Docker Hub 上建立的使用者名稱來取代 `<your_docker_username>`）： `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker 必須使用 WSL Tech Preview 來執行，才能讓此命令運作。 如需如何啟動 Docker 的提醒，請參閱安裝一節的[步驟 #4](#install-docker-desktop-wsl-2-tech-preview) 。 `-t` 旗標會指定要建立的映射名稱，在我們的案例中為 "nextjs-app： v1"。 建議您在建立映射時，一律[在標記名稱上使用版本 #](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) 。 請務必在命令的結尾包含句點，這會指定目前的工作目錄，以便用來尋找並複製您的下一個 .js 應用程式的組建檔案。

4. 若要在容器中執行下一個 .js 應用程式的新 docker 映射，請輸入下列命令： `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. `-p` 旗標會將埠 ' 3000 ' （應用程式在容器內執行的埠）系結至您電腦上的本機埠 ' 3333 '，因此您現在可以將網頁瀏覽器指向[http://localhost:3333](http://localhost:3333) ，並查看以 Docker 容器映射形式執行的下一個 .js 應用程式的伺服器端呈現。

> [!TIP]
> 我們使用參照儲存在 Docker Hub 上的 node.js 第12版預設映射的 `FROM node:12` 來建立容器映射。 這個預設的 node.js 映射是以 Debian/Ubuntu Linux 系統為基礎，有許多不同的 node.js 映射可供選擇，但您可能會想要考慮更輕量或量身訂做您的需求。 深入瞭解[Docker Hub 上的 Node.js 映射登錄](https://hub.docker.com/_/node/)。

## <a name="upload-your-container-image-to-a-repository"></a>將您的容器映像上傳至存放庫

**容器存放庫**會將您的容器映射儲存在雲端中。 容器存放庫通常會包含相關映射的集合（例如不同版本），這些影像全都可用於輕鬆設定和快速部署。 一般來說，您可以透過安全的 HTTPs 端點來存取容器存放庫上的映射，讓您可以透過任何系統、硬體或 VM 實例來提取、推送或管理映射。

另一方面， **container registry**會儲存存放庫的集合，以及索引、存取控制規則和 API 路徑。 這些可以公開或私下裝載。 [Docker Hub](https://hub.docker.com/)是一個開放原始碼的 Docker 登錄，以及執行 `docker push` 和 `docker pull` 命令時所使用的預設值。 公用存放庫免費，而且需要付費以進行私用存放庫。

若要將新的容器映射上傳至 Docker Hub 上裝載的存放庫：

1. 登入 Docker Hub。 系統會提示您輸入在安裝步驟中用來建立 Docker Hub 帳戶的使用者名稱和密碼。 若要登入您終端機中的 Docker，請輸入： `docker login`

2. 若要取得您已在電腦上建立的 docker 容器映射清單，請輸入： `docker image ls --all`

3. 將您的容器映射推送至 Docker Hub，並使用下列命令在該處為其建立新的存放庫： `docker push <your_docker_username>/my-nextjs-app:v1`

4. 您現在可以造訪下列網址，在 Docker Hub 上查看您的存放庫、輸入描述，並連結您的 GitHub 帳戶（如有需要）： https://cloud.docker.com/repository/list

5. 您也可以使用下列方式來查看作用中的 Docker 容器清單： `docker container ls` （或 `docker ps`）

6. 您應該會看到埠 3333-> 3000/tcp 上的「我的 nextjs 應用程式： v1」容器處於作用中。 您也可以查看此處所列的「容器識別碼」。 若要停止執行您的容器，請輸入下列命令： `docker stop <container ID>`

7. 一般來說，一旦停止容器，也應該將它移除。 移除容器會清除它留下的任何資源。 一旦您移除容器，在其映射檔案系統中所做的任何變更都會永久遺失。 您將需要建立新的映射來代表變更。 若要移除您的容器，請使用命令： `docker rm <container ID>`

深入瞭解如何[使用 Docker 建立容器化 web 應用程式](https://docs.microsoft.com/learn/modules/intro-to-containers/)。

## <a name="deploy-to-azure-container-registry"></a>部署至 Azure Container Registry

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) （ACR）可讓您在私用、已驗證的存放庫中，儲存、管理和保留容器映射安全。 與標準 Docker 命令相容，ACR 可以為您處理重要的工作，例如容器健康情況監視和維護，與[Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes)配對以建立可調整的協調流程系統。 視需要建立，或完全自動化具有觸發程式的組建，例如原始程式碼認可和基底映射更新。 ACR 也利用龐大的 Azure 雲端網路來管理網路延遲、全球部署，並為使用[Azure App Service](https://docs.microsoft.com/azure/app-service/) （適用于 web 裝載、行動後端、REST api）或[其他 Azure 雲端服務](https://azure.microsoft.com/product-categories/containers/)的任何人，建立順暢的原生體驗。

> [!IMPORTANT]
> 您需要自己的 Azure 訂用帳戶，才能將容器部署至 Azure，而您可能會收到費用。 如果您還沒有 Azure 訂用帳戶，請在開始前[建立免費帳戶](https://azure.microsoft.com/free/)。

如需建立 Azure Container Registry 和部署應用程式容器映射的說明，請參閱練習：將[Docker 映射部署至 Azure 容器實例](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)。

## <a name="additional-resources"></a>其他資源

- [Azure 上的 node.js](https://azure.microsoft.com/en-us/develop/nodejs/)
- 快速入門：[在 Azure 中建立 node.js web 應用程式](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- 線上課程：[在 Azure 中管理容器](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- 使用 VS Code：使用[Docker](https://code.visualstudio.com/docs/azure/docker)
- Docker 檔： [Docker DESKTOP WSL 2 技術預覽](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
