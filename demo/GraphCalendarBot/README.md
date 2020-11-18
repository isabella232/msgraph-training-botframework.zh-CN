---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347766"
---
# <a name="graphcalendarbot"></a><span data-ttu-id="1fc4a-101">GraphCalendarBot</span><span class="sxs-lookup"><span data-stu-id="1fc4a-101">GraphCalendarBot</span></span>

<span data-ttu-id="1fc4a-102">Bot 框架 v4 回显 bot 示例。</span><span class="sxs-lookup"><span data-stu-id="1fc4a-102">Bot Framework v4 echo bot sample.</span></span>

<span data-ttu-id="1fc4a-103">此 bot 已使用 [Bot 框架](https://dev.botframework.com)创建，它展示了如何创建一个简单的 bot 来接受用户输入并回显它。</span><span class="sxs-lookup"><span data-stu-id="1fc4a-103">This bot has been created using [Bot Framework](https://dev.botframework.com), it shows how to create a simple bot that accepts input from the user and echoes it back.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fc4a-104">先决条件</span><span class="sxs-lookup"><span data-stu-id="1fc4a-104">Prerequisites</span></span>

- <span data-ttu-id="1fc4a-105">[.Net CORE SDK](https://dotnet.microsoft.com/download) 版本3。1</span><span class="sxs-lookup"><span data-stu-id="1fc4a-105">[.NET Core SDK](https://dotnet.microsoft.com/download) version 3.1</span></span>

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a><span data-ttu-id="1fc4a-106">若要试用此示例</span><span class="sxs-lookup"><span data-stu-id="1fc4a-106">To try this sample</span></span>

- <span data-ttu-id="1fc4a-107">在终端中，导航到 `GraphCalendarBot`</span><span class="sxs-lookup"><span data-stu-id="1fc4a-107">In a terminal, navigate to `GraphCalendarBot`</span></span>

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- <span data-ttu-id="1fc4a-108">从终端或 Visual Studio 中运行机器人，选择选项 A 或 B。</span><span class="sxs-lookup"><span data-stu-id="1fc4a-108">Run the bot from a terminal or from Visual Studio, choose option A or B.</span></span>

  <span data-ttu-id="1fc4a-109">来自终端的) </span><span class="sxs-lookup"><span data-stu-id="1fc4a-109">A) From a terminal</span></span>

  ```bash
  # run the bot
  dotnet run
  ```

  <span data-ttu-id="1fc4a-110">B) 或从 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1fc4a-110">B) Or from Visual Studio</span></span>

  - <span data-ttu-id="1fc4a-111">启动 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1fc4a-111">Launch Visual Studio</span></span>
  - <span data-ttu-id="1fc4a-112">文件-> 打开-> 项目/解决方案</span><span class="sxs-lookup"><span data-stu-id="1fc4a-112">File -> Open -> Project/Solution</span></span>
  - <span data-ttu-id="1fc4a-113">导航到 `GraphCalendarBot` 文件夹</span><span class="sxs-lookup"><span data-stu-id="1fc4a-113">Navigate to `GraphCalendarBot` folder</span></span>
  - <span data-ttu-id="1fc4a-114">选择 `GraphCalendarBot.csproj` 文件</span><span class="sxs-lookup"><span data-stu-id="1fc4a-114">Select `GraphCalendarBot.csproj` file</span></span>
  - <span data-ttu-id="1fc4a-115">按 `F5` 运行项目</span><span class="sxs-lookup"><span data-stu-id="1fc4a-115">Press `F5` to run the project</span></span>

## <a name="testing-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="1fc4a-116">使用 Bot 框架仿真程序测试机器人</span><span class="sxs-lookup"><span data-stu-id="1fc4a-116">Testing the bot using Bot Framework Emulator</span></span>

<span data-ttu-id="1fc4a-117">[Bot 框架仿真器](https://github.com/microsoft/botframework-emulator) 是一种桌面应用程序，它允许 Bot 开发人员在本地主机上测试和调试其 bot 或通过隧道远程运行。</span><span class="sxs-lookup"><span data-stu-id="1fc4a-117">[Bot Framework Emulator](https://github.com/microsoft/botframework-emulator) is a desktop application that allows bot developers to test and debug their bots on localhost or running remotely through a tunnel.</span></span>

- <span data-ttu-id="1fc4a-118">在[此处](https://github.com/Microsoft/BotFramework-Emulator/releases)安装 4.9.0 Framework 模拟器版本或更高版本</span><span class="sxs-lookup"><span data-stu-id="1fc4a-118">Install the Bot Framework Emulator version 4.9.0 or greater from [here](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="1fc4a-119">使用 Bot 框架仿真程序连接到 bot</span><span class="sxs-lookup"><span data-stu-id="1fc4a-119">Connect to the bot using Bot Framework Emulator</span></span>

- <span data-ttu-id="1fc4a-120">启动 Bot 框架仿真程序</span><span class="sxs-lookup"><span data-stu-id="1fc4a-120">Launch Bot Framework Emulator</span></span>
- <span data-ttu-id="1fc4a-121">文件-> 打开 Bot</span><span class="sxs-lookup"><span data-stu-id="1fc4a-121">File -> Open Bot</span></span>
- <span data-ttu-id="1fc4a-122">输入的 Bot URL `http://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="1fc4a-122">Enter a Bot URL of `http://localhost:3978/api/messages`</span></span>

## <a name="deploy-the-bot-to-azure"></a><span data-ttu-id="1fc4a-123">将机器人部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="1fc4a-123">Deploy the bot to Azure</span></span>

<span data-ttu-id="1fc4a-124">若要了解有关将机器人部署到 Azure 的详细信息，请参阅将 [你的 Bot 部署到 azure](https://aka.ms/azuredeployment) ，获取部署说明的完整列表。</span><span class="sxs-lookup"><span data-stu-id="1fc4a-124">To learn more about deploying a bot to Azure, see [Deploy your bot to Azure](https://aka.ms/azuredeployment) for a complete list of deployment instructions.</span></span>

## <a name="further-reading"></a><span data-ttu-id="1fc4a-125">进一步阅读</span><span class="sxs-lookup"><span data-stu-id="1fc4a-125">Further reading</span></span>

- [<span data-ttu-id="1fc4a-126">Bot 框架文档</span><span class="sxs-lookup"><span data-stu-id="1fc4a-126">Bot Framework Documentation</span></span>](https://docs.botframework.com)
- [<span data-ttu-id="1fc4a-127">机器人基础知识</span><span class="sxs-lookup"><span data-stu-id="1fc4a-127">Bot Basics</span></span>](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [<span data-ttu-id="1fc4a-128">活动处理</span><span class="sxs-lookup"><span data-stu-id="1fc4a-128">Activity processing</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [<span data-ttu-id="1fc4a-129">Azure Bot 服务简介</span><span class="sxs-lookup"><span data-stu-id="1fc4a-129">Azure Bot Service Introduction</span></span>](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [<span data-ttu-id="1fc4a-130">Azure Bot 服务文档</span><span class="sxs-lookup"><span data-stu-id="1fc4a-130">Azure Bot Service Documentation</span></span>](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [<span data-ttu-id="1fc4a-131">.NET Core CLI 工具</span><span class="sxs-lookup"><span data-stu-id="1fc4a-131">.NET Core CLI tools</span></span>](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [<span data-ttu-id="1fc4a-132">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1fc4a-132">Azure CLI</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [<span data-ttu-id="1fc4a-133">Azure 门户</span><span class="sxs-lookup"><span data-stu-id="1fc4a-133">Azure Portal</span></span>](https://portal.azure.com)
- [<span data-ttu-id="1fc4a-134">使用 LUIS 的语言理解</span><span class="sxs-lookup"><span data-stu-id="1fc4a-134">Language Understanding using LUIS</span></span>](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [<span data-ttu-id="1fc4a-135">通道和机器人连接器服务</span><span class="sxs-lookup"><span data-stu-id="1fc4a-135">Channels and Bot Connector Service</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

<span data-ttu-id="1fc4a-136">使用 `dotnet new echobot` v 4.10.2 生成</span><span class="sxs-lookup"><span data-stu-id="1fc4a-136">Generated using `dotnet new echobot` v4.10.2</span></span>
