---
title: 開始使用 Docker 以使用容器進行遠端開發
description: 用來在 Windows 或 WSL 上開始使用 Docker Desktop 的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, 遠端開發, 容器, Docker Desktop, Windows 與 WSL
ms.date: 09/24/2020
ms.openlocfilehash: b3fc2509aa6a623bebd9f4566f3b8e75301251db
ms.sourcegitcommit: c65f62bda57563f6196691e7b9c25cbf5a8b16e5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2020
ms.locfileid: "91780580"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>在 Windows 上進行 Docker 遠端開發的概觀

使用容器進行遠端開發以及使用 Docker 平台部署應用程式，是一個具有許多優點的熱門解決方案。 深入了解 Microsoft 工具和服務所提供的各種支援，包括 Windows 子系統 Linux 版 (WSL)、Visual Studio、Visual Studio Code、.NET 和各種不同的 Azure 服務。

## <a name="docker-on-windows-10"></a>Windows 10 上的 Docker

:::row:::
    :::column:::
       [![Docker 文件圖示](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[安裝適用於 Windows 的 Docker Desktop](https://docs.docker.com/docker-for-windows/install/)**<br>
        尋找安裝步驟、系統需求、安裝程式中包含的項目、如何解除安裝、穩定版和 Edge 版的差異，以及如何在 Windows 與 Linux 容器之間切換。
    :::column-end:::
    :::column:::
       [![Docker 正在執行的螢幕擷取畫面](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[開始使用 Docker](https://docs.docker.com/get-started/)**<br>
        Docker 方向和設定文件，內含如何開始使用的逐步指示，包括影片逐步解說。
    :::column-end:::
    :::column:::
       [![Microsoft Learn Docker 課程的螢幕擷取畫面](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[MS Learn 課程：Docker 容器簡介](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn 提供關於 Docker 容器的免費簡介課程，以及關於如何開始使用 Docker 並與 Azure 服務連線的[各種課程](/learn/browse/?terms=docker)。
    :::column-end:::
    :::column:::
       [![Docker Desktop WSL2 功能表螢幕擷取畫面](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[在 WSL 2 上開始使用 Docker 遠端容器](/windows/wsl/tutorials/wsl-containers)**<br>
        了解如何使用 WSL 2 (Windows 子系統 Linux 版，第2版) 來設定適用於 Windows 的 Docker Desktop，以便與 Linux 命令列搭配使用 (Ubuntu、Debian、SUSE 等等)。
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code 和 Docker

:::row:::
    :::column:::
       [![VS Code 遠端容器圖形](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[使用 VS Code 建立 Docker 容器](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        使用[遠端 - 容器延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)在容器內設定功能完整的開發環境，並尋找教學課程來設定 [NodeJS 容器](https://code.visualstudio.com/docs/containers/quickstart-node)、[Python 容器](https://code.visualstudio.com/docs/containers/quickstart-python)或 [ASP.NET Core 容器](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core)。
    :::column-end:::
    :::column:::
       [![VSCode 連結 Docker 螢幕擷取畫面](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[將 VS Code 連結至 Docker 容器](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        了解如何將 Visual Studio Code 連結至已在執行中的 Docker 容器，或連結至 [Kubernetes 叢集中的容器](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster)。
    :::column-end:::
    :::column:::
       [![VSCode 容器功能表螢幕擷取畫面](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[進階容器設定](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        了解可搭配使用 Docker 容器與 Visual Studio Code 的進階設定案例，或閱讀這篇有關如何[檢查容器](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers)以使用 VS Code 進行偵錯的文章。
    :::column-end:::
    :::column:::
       [![VSCode Docker Desktop 與 WSL 螢幕擷取畫面](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[在 WSL 2 中使用遠端容器](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        了解如何搭配 WSL 2 (Windows 子系統 Linux 版、第2版) 使用 Docker 容器，以及如何利用 VS Code 來完成一切設定。 您也可以了解[其運作方式](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works)。
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio 和 Docker

:::row:::
    :::column:::
       [![Visual Studio 圖示](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Visual Studio 中的 Docker 支援](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        除了了解容器協調流程的支援之外，還可了解適用於 ASP.NET 專案、ASP.NET Core 專案以及 Visual Studio 中的 .NET Core 和 .NET Framework 主控台專案的 Docker 支援。
    :::column-end:::
    :::column:::
       [![Visual Studio Docker 功能表](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[快速入門：Visual Studio 中的 Docker](/visualstudio/containers/container-tools)**<br>
        了解如何建置、偵測及執行容器化 .NET、ASP.NET 和 ASP.NET Core 應用程式，並使用 Visual Studio 將其發佈至 Azure Container Registry (ACR)、Docker Hub、Azure App Service 或您自己的容器登錄。
    :::column-end:::
    :::column:::
       [![VS 教學課程螢幕擷取畫面](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[教學課程：使用 Docker Compose 建立多容器應用程式](/visualstudio/containers/tutorial-multicontainer)**<br>
        了解如何管理多個容器，並在 Visual Studio 中使用容器工具時在兩者之間進行通訊。 您也可以找到教學課程的連結，例如如何[搭配使用 Docker 與 React 單頁應用程式](/visualstudio/containers/container-tools-react)。
    :::column-end:::
    :::column:::
       [![VS 容器連結](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Visual Studio 中的容器工具](/visualstudio/containers)**<br>
        尋找涵蓋如何在容器中執行建置工具、[對 Docker 應用程式進行偵錯](/visualstudio/containers/edit-and-refresh)、針對開發工具進行疑難排解、部署 Docker 容器，以及使用 Visual Studio 橋接 Kubernetes 的主題。
    :::column-end:::
:::row-end:::

![容器、映像和登錄的基本 Docker 分類法資訊圖](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core 和 Docker

:::row:::
    :::column:::
       [![.NET 微服務指南涵蓋](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[.NET 指南：微服務應用程式和容器](/dotnet/architecture/microservices/)**<br>
        以容器管理的微服務型應用程式簡介指南。
    :::column-end:::
    :::column:::
       [![Docker 資訊圖](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[什麼是 Docker？](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        Docker 容器的基本說明，包括[比較 Docker 容器與虛擬機器](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines)，以及 [Docker 術語和概念的基本分類法](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)，說明容器、映像和登錄的差異。
    :::column-end:::
    :::column:::
       [![Docker 分類法資訊圖](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[教學課程：將 .NET Core 應用程式容器化](/dotnet/core/docker/build-container?tabs=windows)**<br>
        了解如何使用 Docker 將 .NET Core 應用程式容器化，包括建立 Dockerfile、基本命令，以及清除資源。
    :::column-end:::
    :::column:::
       [![Docker 的內部迴圈開發工作流程資訊圖](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Docker 應用程式的開發工作流程](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        描述 Docker 容器型應用程式的內部迴圈開發工作流程。
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure Container Services

:::row:::
    :::column:::
       [![Azure 容器執行個體螢幕擷取畫面](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure Container Instances](/azure/container-instances/)**<br>
        了解如何在受控、無伺服器的 Azure 環境中隨選執行 Docker 容器，包括使用 Docker CLI、ARM、Azure 入口網站進行部署、建立多容器群組、在容器之間共用資料、連線至虛擬網路等等的方法。
    :::column-end:::
    :::column:::
       [![Azure Container Registry 螢幕擷取畫面](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure Container Registry](/azure/container-registry)**<br>
        了解如何在私人登錄中為所有容器部署類型建置、儲存和管理容器映像與成品。 為現有的容器開發和部署管線建立 Azure 容器登錄、設定自動化工作，並了解如何管理您的登錄，包括異地複寫和最佳做法。
    :::column-end:::
    :::column:::
       [![Azure Service Fabric 螢幕擷取畫面](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        了解 Azure Service Fabric，這是一種分散式系統平台，可用於封裝、部署及管理可調整和可靠的微服務和容器。
    :::column-end:::
    :::column:::
       [![Azure App Service 螢幕擷取畫面](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure App Service](/azure/app-service)**<br>
        了解如何以您選擇的程式設計語言來建置和裝載 Web 應用程式、行動後端和 RESTful API，而不需要管理基礎結構。 嘗試 [MS Learn 上的 Azure App Service 課程](/learn/modules/deploy-run-container-app-service)，根據 Docker 映像部署 Web 應用程式，並設定持續部署。
    :::column-end:::
:::row-end:::

深入了解[支援容器的 Azure 服務](https://azure.microsoft.com/overview/containers/)。

## <a name="docker-containers-explainer-video"></a>Docker 容器解說者影片

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Kubernetes 和容器協調流程解說者影片

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Windows 上的容器

:::row:::
    :::column:::
       [![Windows 伺服器容器圖示](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Windows 上的容器文件](/virtualization/windowscontainers)**<br>
        封裝應用程式及其相依性，並利用作業系統層級虛擬化，在單一系統上建立快速、完全隔離的環境。 了解 [Windows 容器](/virtualization/windowscontainers/about)，包括快速入門、部署指南和範例。
    :::column-end:::
    :::column:::
       [![常見問題集圖示](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[Windows 容器的常見問題集](/virtualization/windowscontainers/about/faq)**<br>
        尋找關於容器的常見問題集。 另請參閱 StackOverflow 中關於「[適用於 Windows 的 Docker 與 Windows 上的 Docker 之間有何差異？](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748)」的說明
    :::column-end:::
    :::column:::
       [![Windows 容器圖示](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[設定環境](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        了解如何設定 Windows 10 或 Windows Server 以建立、執行及部署容器，包括必要條件、安裝 Docker，以及使用 [Windows 容器基底映像](/virtualization/windowscontainers/manage-containers/container-base-images)。
    :::column-end:::
    :::column:::
       [![AKS 圖示](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[在 Azure Kubernetes Service (AKS) 上建立 Windows Server 容器](/azure/aks/windows-container-cli)**<br>
        了解如何使用 Azure CLI，將 Windows Server 容器中的 ASP.NET 範例應用程式部署至 AKS 叢集。
    :::column-end:::
:::row-end:::
