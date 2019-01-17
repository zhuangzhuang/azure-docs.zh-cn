---
title: 开发和调试 Azure IoT Edge 模块 | Microsoft Docs
description: 使用 Visual Studio Code 开发、生成和调试使用 C#、Python、Node.js、Java 或 C 的 Azure IoT Edge 模块
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 463ab617051bf97bb3b1c38ed431c4b6936a9c90
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118687"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>使用 Visual Studio Code 开发和调试 Azure IoT Edge 模块

可以将业务逻辑转变为用于 Azure IoT Edge 的模块。 本文展示了如何使用 Visual Studio Code 作为主要工具来开发和调试模块。

## <a name="prerequisites"></a>先决条件

可以使用运行 Windows、macOS 或 Linux 的计算机或虚拟机作为开发计算机。 IoT Edge 设备可以是另一台物理设备。

对于用 C#、Node.js 或 Java 编写的模块，有两种方法可以在 Visual Studio Code 中调试模块：可在模块容器中附加一个进程，或在调试模式下启动模块代码。 对于用 Python 或 C 编写的模块，只能通过附加到 Linux amd64 容器中的进程来调试。

> [!TIP]
> 如果你不熟悉 Visual Studio Code 的调试功能，请阅读有关[Debugging](https://code.visualstudio.com/Docs/editor/debugging)（调试）的信息。

首先安装 [Visual Studio Code](https://code.visualstudio.com/)，然后添加以下扩展：

- [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
- [Docker 扩展](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- 特定于你正在开发的语言的 Visual Studio 扩展：
  - C#，包括 Azure Functions：[C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python:[Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java:[适用于 Visual Studio Code 的 Java 扩展包](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C:[扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

此外还需要安装一些其他特定于语言的工具来开发模块：

- C#，包括 Azure Functions：[.NET Core 2.1 SDK](https://www.microsoft.com/net/download)

- Python:[Python](https://www.python.org/downloads/) 和 [Pip](https://pip.pypa.io/en/stable/installing/#installation)用于安装 Python 包（通常包含在 Python 安装中）。 安装 Pip 后，使用以下命令安装“Cookiecutter”包：

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

- Node.js：[Node.js](https://nodejs.org)。 还需要安装 [Yeoman](https://www.npmjs.com/package/yo) 和 [Azure IoT Edge Node.js 模块生成器](https://www.npmjs.com/package/generator-azure-iot-edge-module)。

- Java:[Java SE 开发工具包 10](https://aka.ms/azure-jdks) 和 [Maven](https://maven.apache.org/)。 需要[设置`JAVA_HOME`环境变量](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/)，使其指向 JDK 安装项目。

若要生成并部署模块映像，需使用 Docker 生成模块映像，并使用容器注册表来保存模块映像：

- 开发计算机上的 [Docker Community Edition](https://docs.docker.com/install/)。

- [Azure 容器注册表](https://docs.microsoft.com/azure/container-registry/)或 [Docker 中心](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > 对于原型和测试用途，可以使用本地 Docker 注册表，而不使用云注册表。

除非使用 C 开发模块，否则还需要基于 Python 的 [Azure IoT EdgeHub 开发工具](https://pypi.org/project/iotedgehubdev/)，以便设置本地开发环境以调试、运行和测试 IoT Edge 解决方案。 如果尚未这样做，请安装 [Python (2.7/3.6) 和 Pip](https://www.python.org/)，然后在终端中运行此命令安装“iotedgehubdev”。

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> 若要在设备上测试模块，至少需要一个 IoT Edge 设备和一个活动的 IoT 中心。 若要将计算机用作 IoT Edge 设备，请按照 [Linux](quickstart-linux.md) 或 [Windows](quickstart.md) 快速入门中的步骤操作。 如果正在开发计算机上运行 IoT Edge 守护程序，则可能需要先停止 EdgeHub 和 EdgeAgent，然后再继续下一步。

## <a name="create-a-new-solution-template"></a>创建新的解决方案模板

以下步骤说明如何使用 Visual Studio Code 和 Azure IoT Tools 以首选开发语言（包括使用 C# 编写的 Azure Functions）创建 IoT Edge 模块。 首先创建一个解决方案，然后生成该解决方案中的第一个模块。 每个解决方案可以包含多个模块。

1. 选择“视图” > “命令面板”。

1. 在“命令面板”中，输入并运行 Azure IoT Edge：New IoT Edge solution”命令。

   ![运行新的 IoT Edge 解决方案](./media/how-to-develop-csharp-module/new-solution.png)

1. 浏览到要在其中创建新解决方案的文件夹，然后选择“选择文件夹”。

1. 输入解决方案的名称。

1. 选择首选开发语言的模块模板作为解决方案中的第一个模块。

1. 输入模块的名称。 选择容器注册表中唯一的名称。

1. 提供模块的映像存储库的名称。 Visual Studio Code 使用“localhost:5000/<你的模块名称\>”自动填充模块名。 将其替换为你自己的注册表信息。 如果使用本地 Docker 注册表进行测试，则可以使用 localhost。 如果使用 Azure 容器注册表，那么请从注册表的设置中使用登录服务器。 登录服务器如下所示：*\<注册表名称\>*.azurecr.io。 仅替换字符串的“localhost:5000”部分，以使最终结果看起来像*\<注册表名称\>.azurecr.io/\<你的模块名称\>***。

   ![提供 Docker 映像存储库](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code 采用你提供的信息，创建一个 IoT Edge 解决方案，然后在新窗口中加载它。

该解决方案中有四个项：

- 一个 .vscode 文件夹，包含调试配置。

- 一个 modules 文件夹，包含每个模块的子文件夹。 现在，只有一个模块。 但是可以在命令面板中使用以下命令添加更多模块：“Azure IoT Edge: Add IoT Edge Module”。

- 一个 .env 文件，列出环境变量。 如果 Azure 容器注册表是注册表，则其中将包含 Azure 容器注册表用户名和密码。

  > [!NOTE]
  > 仅当为模块提供了映像存储库时，才会创建环境文件。 如果接受 localhost 默认值在本地进行测试和调试，则不需要声明环境变量。

- 一个 deployment.template.json 文件，列出新模块以及模拟可用于测试的数据的示例 tempSensor 模块。 有关部署清单如何工作的详细信息，请参阅[了解如何使用部署清单部署模块和建立路由](module-composition.md)。

## <a name="add-additional-modules"></a>添加其他模块

若要向解决方案添加其他模块，请从命令面板运行命令“Azure IoT Edge：添加 IoT Edge 模块”。 也可以右键单击 Visual Studio Code 资源管理器视图中的“模块”文件夹或 `deployment.template.json` 文件，然后选择“添加 IoT Edge 模块”。

## <a name="develop-your-module"></a>开发模块

解决方案附带的默认 C# 模块代码位于以下位置：

- Azure Function (C#)：modules > &lt;你的模块名称&gt; > &lt;你的模块名称&gt;.cs
- C#：modules > &lt;你的模块名称&gt; > Program.cs
- Python：modules > &lt;你的模块名称&gt; > main.py
- Node.js：modules > &lt;你的模块名称&gt; > app.js
- Java：modules > &lt;你的模块名称&gt; > src > main > java > com > edgemodulemodules > App.java
- C：modules > &lt;你的模块名称&gt; > main.c

设置模块和 deployment.template.json 文件，以便可以生成解决方案，将其推送到容器注册表，然后部署到设备以开始测试而无需触及任何代码。 生成该模块仅为简单从源（在此示例中，为模拟数据的 tempSensor 模块）获取输入并通过管道将其传送到 IoT 中心。

当你准备使用自己的代码自定义模板时，请使用 [Azure IoT Hub SDK](../iot-hub/iot-hub-devguide-sdks.md) 生成模块，以满足 IoT 解决方案的关键需求（例如安全性、设备管理和可靠性）。

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>调试没有容器的模块 (C#、Node.js、Java)

如果使用 C#、Node.js 或 Java 进行开发，则模块需要在默认模块代码中使用“ModuleClient”对象，以便它可以启动、运行和路由消息。 还将使用默认输入通道“input1”在模块接收消息时执行操作。

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>为 IoT Edge 解决方案设置 IoT Edge 模拟器

在开发计算机中，可以启动 IoT Edge 模拟器（而不是安装 IoT Edge 安全守护程序）以运行 IoT Edge 解决方案。

1. 在左侧的设备资源管理器中，右键单击 IoT Edge 设备 ID，然后选择“设置 IoT Edge 模拟器”以使用设备连接字符串启动模拟器。
1. 通过读取集成终端中的进度详细信息，可以看到 IoT Edge 模拟器已成功设置。

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>为单个模块应用设置 IoT Edge 模拟器

若要设置和启动模拟器，请从 Visual Studio Code 命令选项板运行命令“Azure IoT Edge：启动单模块的 IoT Edge Hub 模拟器。 出现提示时，使用默认模块代码中的值“input1”（或代码中的等效值）作为应用程序的输入名称。 该命令触发“iotedgehubdev”CLI，然后启动 IoT Edge 模拟器并测试实用程序模块容器。 如果模拟器已成功以单模块模式启动，则可以在集成终端中看到下面的输出。 还可以看到 `curl` 命令以帮助发送消息。 稍后将使用它。

   ![为单个模块应用设置 IoT Edge 模拟器](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   可以使用 Visual Studio Code 中的 Docker 资源管理视图来查看模块的运行状态。

   ![模拟器模块状态](media/how-to-develop-csharp-module/simulator-status.png)

   edgeHubDev 容器是本地 IoT Edge 模拟器的核心。 它可以在没有 IoT Edge 安全守护程序的情况下在开发计算机上运行，并为本机模块应用或模块容器提供环境设置。 “input”容器公开 REST API 以帮助将消息桥接到模块上的目标输入通道。

### <a name="debug-module-in-launch-mode"></a>在启动模式下调试模块

1. 根据开发语言的要求准备环境以进行调试，在模块中设置断点，并选择要使用的调试配置：
   - **C#**
     - 在 Visual Studio Code 集成终端中，将目录更改为“&lt;你的模块名称&gt;”文件夹，然后运行以下命令以构建 .Net Core 应用程序。

       ```cmd
       dotnet build
       ```

     - 打开 `program.cs` 文件并添加断点。

     - 通过选择“视图”>“调试”以导航到 Visual Studio Code 调试视图。 从下拉列表中选择调试配置“&lt;你的模块名称&gt; 本地调试(.NET Core)”。

        > [!NOTE]
        > 如果 .Net Core `TargetFramework` 与 `launch.json` 中的程序路径不一致，则需要手动更新 `launch.json` 中的程序路径以匹配 .csproj 文件中的 `TargetFramework`，以便 Visual Studio 代码可以成功启动此计划。

   - **Node.js**
     - 在 Visual Studio Code 集成终端中，将目录更改为“&lt;你的模块名称&gt;”文件夹，然后运行以下命令以安装节点包

       ```cmd
       npm install
       ```

     - 打开 `app.js` 文件并添加断点。

     - 通过选择“视图”>“调试”以导航到 Visual Studio Code 调试视图。 从下拉列表中选择调试配置“&lt;你的模块名称&gt; 本地调试(Node.js)”。
   - **Java**
     - 打开 `App.java` 文件并添加断点。

     - 通过选择“视图”>“调试”以导航到 Visual Studio Code 调试视图。 从下拉列表中选择调试配置“&lt;你的模块名称&gt; 本地调试(Java)”。

1. 单击“开始调试”或按“F5”开始调试会话。

1. 在 Visual Studio Code 集成终端中，运行以下命令向模块发送“Hello World”消息。 这是在前面的步骤中设置 IoT Edge 模拟器时所显示的命令。

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > 如果使用的是 Windows，请确保 Visual Studio Code 集成终端的 shell 为 Git Bash 或 WSL Bash。 无法从 PowerShell 或命令提示符运行 `curl` 命令。
   > [!TIP]
   > 还可以使用 [PostMan](https://www.getpostman.com/) 或其他 API 工具（而不是 `curl`）来发送消息。

1. 在 Visual Studio Code 调试视图中，将在左侧面板中看到变量。

1. 若要停止调试会话，请选择“停止”按钮或按“Shift + F5”，然后在命令面板中运行“Azure IoT Edge：停止 IoT Edge 模拟器”以停止模拟器并清除。

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>使用 IoT Edge 模拟器在附加模式下进行调试 (C#、Node.js、Java、Azure Functions)

默认解决方案包含两个模块，一个是模拟的温度传感器模块，另一个是管道模块。 模拟的温度传感器向管道模块发送消息，然后将消息通过管道传送到 IoT 中心。 在创建的模块文件夹中，有适用于不同容器类型的多个 Docker 文件。 使用以扩展名“.debug”结尾的任何文件来生成用于测试的模块。

目前，仅支持以附加模式进行调试，如下所示：

- C# 模块（包括 Azure Functions 的模块）支持在 Linux amd64 容器进行调试
- Node.js 模块支持在 Linux amd64、arm32v7 容器以及 Windows amd64 容器中进行调试
- Java 模块支持在 Linux amd64 和 arm32v7 容器中进行调试

> [!TIP]
> 可以通过单击 Visual Studio 代码状态栏中的项目来切换 IoT Edge 解决方案的默认平台的选项。

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>为 IoT Edge 解决方案设置 IoT Edge 模拟器

在开发计算机中，可以启动 IoT Edge 模拟器（而不是安装 IoT Edge 安全守护程序）以运行 IoT Edge 解决方案。

1. 在左侧的设备资源管理器中，右键单击 IoT Edge 设备 ID，然后选择“设置 IoT Edge 模拟器”以使用设备连接字符串启动模拟器。

1. 通过读取集成终端中的进度详细信息，可以看到 IoT Edge 模拟器已成功设置。

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>生成和运行用于调试的容器并在附加模式下进行调试

1. 打开模块文件 (`program.cs`、`app.js``App.java` 或 `<your module name>.cs`) 并添加断点。

1. 在 Visual Studio Code 资源管理器视图中，右键单击解决方案的 `deployment.debug.template.json` 文件，然后选择“在模拟器中构建并运行 IoT Edge 解决方案”。 可以在同一窗口中监视所有模块容器日志。 还可以导航到 Docker 视图以监视容器状态。

   ![监视变量](media/how-to-develop-csharp-module/view-log.png)

1. 导航至 Visual Studio Code 调试视图，并选择模块的调试配置文件。 调试选项名称应类似于“&lt;你的模块名称&gt; 远程调试”

1. 选择“开始调试”或按“F5”。 选择要附加到的进程。

1. 在 Visual Studio Code 调试视图中，将在左侧面板中看到变量。

1. 若要停止调试会话，请先选择停止按钮或按“Shift + F5”，然后从命令面板中选择“Azure IoT Edge：停止 IoT Edge 模拟器”。

> [!NOTE]
> 上面的示例展示了如何调试容器上的 IoT Edge 模块。 它为模块的容器 `createOptions` 设置添加了公开的端口。 完成模块的调试后，建议为可用于生产的 IoT Edge 模块删除那些公开的端口。
>
> 对于使用 C# 编写的模块（包括 Azure Functions），此示例基于 `Dockerfile.amd64.debug` 的调试版本，在生成容器映像时这将在容器映像中包括 .NET Core 命令行调试器 (VSDBG)。 对于生产就绪的 IoT Edge 模块，建议在调试完 C# 模块后在不使用 VSDBG 的情况下直接使用 Dockerfile。

## <a name="debug-a-module-with-iot-edge-runtime"></a>使用 IoT Edge 运行时调试模块

每个模块文件夹中都有多个适用于不同容器类型的 Docker 文件。 使用以扩展名“.debug”结尾的任何文件来生成用于测试的模块。

使用 IoT Edge 运行时调试模块时，模块在 IoT Edge 运行时之上运行。 IoT Edge 设备和 VS Code 可以位于同一台计算机中；也有更常见的做法，位于不同的计算机中（VS Code 位于开发计算机中，IoT Edge 运行时和模块要在另一台物理计算机中运行）。 在 VS Code 中调试会话，需要完成以下步骤。

- 设置 IoT Edge 设备，请使用 .debug Dockerfile 生成 IoT Edge 模块，并将其部署到 IoT Edge 设备。 
- 公开模块的 IP 和端口以供调试程序连接。
- 更新 `launch.json` 文件，以便 VS Code 可以附加到远程计算机中的容器中的进程。

### <a name="build-and-deploy-your-module-and-deploy-to-iot-edge-device"></a>生成和部署模块并部署到 IoT Edge 设备

1. 在 Visual Studio 代码中，打开 `deployment.debug.template.json` 文件，其中包含模块映像的调试版本，并设置了正确的 `createOptions` 值。

1. 如果正在使用 Python 开发模块，请在继续之前按照以下步骤操作：
   - 打开文件 `main.py` 并在导入部分后添加此代码：

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```
   - 将以下单行代码添加到要调试的回叫中：

      ```python
      ptvsd.break_into_debugger()
      ```

     例如，如果要调试 `receive_message_callback` 方法，则应插入该行代码，如下所示：

      ```python
      def receive_message_callback(message, hubManager):
          ptvsd.break_into_debugger()
          global RECEIVE_CALLBACKS
          message_buffer = message.get_bytearray()
          size = len(message_buffer)
          print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode ('utf-8'), size) )
          map_properties = message.properties()
          key_value_pair = map_properties.get_internals()
          print ( "    Properties: %s" % key_value_pair )
          RECEIVE_CALLBACKS += 1
          print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
          hubManager.forward_event_to_output("output1", message, 0)
          return IoTHubMessageDispositionResult.ACCEPTED
      ```

1. 在 Visual Studio Code 命令面板中：
   1. 运行“Azure IoT Edge: Build and Push IoT Edge solution”命令。

   1. 选择解决方案的 `deployment.debug.template.json` 文件。

1. 在 Visual Studio Code 资源管理器视图的“Azure IoT 中心设备”部分中：
   1. 右键单击 IoT Edge 设备 ID，然后选择“为单个设备创建部署”。

   1. 导航到解决方案的“config”文件夹，选择 `deployment.debug.amd64.json` 文件，然后选择“选择 Edge 部署清单”。

将在集成终端中看到已成功创建部署且具有一个部署 ID。

可以通过在终端中运行 `docker ps` 命令来检查容器状态。 如果 VS Code 和 IoT Edge 运行时在同一台计算机上运行，则还可以在 Visual Studio Code Docker 视图中检查状态。

### <a name="expose-the-ip-and-port-of-the-module-for-the-debugger-to-attach"></a>公开模块的 IP 和端口以供调试程序连接

如果模块与 VS Code 在同一台计算机上运行。 你使用 localhost 附加容器，并且在 .debug Dockerfile、模块容器 CreateOptions 和 `launch.json` 中已有正确的端口设置。 你可以跳过此部分。 如果模块和 VS Code 在不同的计算机上运行，请为每种语言执行以下步骤。

  - **C#、C# 函数**：[在开发计算机和 IoT Edge 设备上配置 SSH 通道](https://github.com/OmniSharp/omnisharp-vscode/wiki/Attaching-to-remote-processes)，编辑要附加的 `launch.json` 文件。
  - **Node.js**：请确保模块已准备就绪，可供调试程序连接，并且可以从外部访问调试对象计算机的 9229 端口。 可通过在调试程序计算机上打开 [http://%3cdebuggee-machine-IP%3e:9229/json]http://<debuggee-machine-IP>:9229/json 对此进行验证。 此 URL 应显示有关要调试的 Node.js 的信息。 然后在调试程序计算机上打开 VS Code，编辑 `launch.json` 文件，因此，“<module-name> 远程调试 (Node.js)”配置文件（如果模块作为 Windows 容器运行，则为 “<module-name> 远程调试（Windows 容器中的 Node.js）”配置文件）的地址值是调试对象计算机的 IP。
  - **Java**：通过运行 `ssh -f <username>@<edgedevicehost> -L 5005:127.0.0.1:5005 -N` 生成到 edge 设备的 ssh 隧道，然后编辑要附加的 `launch.json` 文件。 可在[此处](https://code.visualstudio.com/docs/java/java-debugging)了解有关设置的详细信息。 
  - **Python**：在代码 `ptvsd.enable_attach(('0.0.0.0', 5678))` 中，将 0.0.0.0 更改为 IoT Edge 设备的 IP 地址。 再次生成、推送和部署 IoT Edge 模块。 在开发计算机的 `launch.json` 中，使用远程 IoT Edge 设备的公共 IP 地址更新 `"host"` `"localhost"` 更改 `"localhost"`。


### <a name="debug-your-module"></a>调试模块

Visual Studio Code 将调试配置信息保存在 `launch.json` 文件中，该文件位于工作区的 `.vscode` 文件夹中。 创建新的 IoT Edge 解决方案时就会生成此 `launch.json` 文件。 每次添加支持调试的新模块时，它都会随之更新。

1. 在 Visual Studio Code 调试视图中，选择模块的调试配置文件。 调试选项名称应类似于“&lt;你的模块名称&gt; 远程调试”

1. 打开开发语言的模块文件并添加断点：
   - **C#、C# 函数**：打开 `Program.cs` 文件并添加断点。
   - **Node.js**：打开 `app.js` 文件并添加断点。
   - **Java**：打开 `App.java` 文件并添加断点。
   - **Python**：打开 `main.py` 并在添加了 `ptvsd.break_into_debugger()` 行的回叫方法中添加断点。
   - **C**：打开 `main.c` 文件并添加断点。

1. 选择“开始调试”或选择 F5。 选择要附加到的进程。

1. 在 Visual Studio Code 调试视图中，将在左侧面板中看到变量。

> [!NOTE]
> 上面的示例展示了如何调试容器上的 IoT Edge 模块。 它为模块的容器 `createOptions` 设置添加了公开的端口。 完成模块的调试后，建议为可用于生产的 IoT Edge 模块删除那些公开的端口。

## <a name="next-steps"></a>后续步骤

生成模块后，了解如何[从 Visual Studio Code 部署 Azure IoT Edge 模块](how-to-deploy-modules-vscode.md)。

若要开发用于 IoT Edge 设备的模块，请参阅[了解并使用 Azure IoT 中心 SDK](../iot-hub/iot-hub-devguide-sdks.md)。