---
ms.openlocfilehash: 12a6c18b52e2bc9aba5ad69c8bb9ab8091c11a64
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347763"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7dac8-101">在本节中，你将创建一个 Bot 框架项目。</span><span class="sxs-lookup"><span data-stu-id="7dac8-101">In this section you'll create a Bot Framework project.</span></span>

1. <span data-ttu-id="7dac8-102">在要创建项目的目录中打开命令行界面 (CLI) 。</span><span class="sxs-lookup"><span data-stu-id="7dac8-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="7dac8-103">运行以下命令，以使用 **EchoBot** 模板创建新的项目。</span><span class="sxs-lookup"><span data-stu-id="7dac8-103">Run the following command to create new project using the **Microsoft.Bot.Framework.CSharp.EchoBot** template.</span></span>

    ```dotnetcli
    dotnet new echobot -n GraphCalendarBot
    ```

    > [!NOTE]
    > <span data-ttu-id="7dac8-104">如果收到 `No templates matched the input template name: echobot.` 错误，请使用以下命令安装该模板，然后重新运行上一个命令。</span><span class="sxs-lookup"><span data-stu-id="7dac8-104">If you receive an `No templates matched the input template name: echobot.` error, install the template with the following command and re-run the previous command.</span></span>
    >
    > ```dotnetcli
    > dotnet new -i Microsoft.Bot.Framework.CSharp.EchoBot
    > ```

1. <span data-ttu-id="7dac8-105">将默认的 **EchoBot** 类重命名为 **CalendarBot**。</span><span class="sxs-lookup"><span data-stu-id="7dac8-105">Rename the default **EchoBot** class to **CalendarBot**.</span></span> <span data-ttu-id="7dac8-106">打开 **/Bots/EchoBot.cs** ，并将所有实例替换 `EchoBot` 为 `CalendarBot` 。</span><span class="sxs-lookup"><span data-stu-id="7dac8-106">Open **./Bots/EchoBot.cs** and replace all instances of `EchoBot` with `CalendarBot`.</span></span> <span data-ttu-id="7dac8-107">将文件重命名为 **CalendarBot.cs**。</span><span class="sxs-lookup"><span data-stu-id="7dac8-107">Rename the file to **CalendarBot.cs**.</span></span>

1. <span data-ttu-id="7dac8-108">替换 `EchoBot` `CalendarBot` 其余 **.cs** 文件中的所有实例。</span><span class="sxs-lookup"><span data-stu-id="7dac8-108">Replace all instances of `EchoBot` with `CalendarBot` in the remaining **.cs** files.</span></span>

1. <span data-ttu-id="7dac8-109">在 CLI 中，将当前目录更改为 **GraphCalendarBot** 目录，并运行以下命令以确认项目生成。</span><span class="sxs-lookup"><span data-stu-id="7dac8-109">In your CLI, change the current directory to the **GraphCalendarBot** directory and run the following command to confirm the project builds.</span></span>

    ```dotnetcli
    dotnet build
    ```

## <a name="add-nuget-packages"></a><span data-ttu-id="7dac8-110">添加 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="7dac8-110">Add NuGet packages</span></span>

<span data-ttu-id="7dac8-111">在继续之前，请安装稍后将使用的一些其他 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7dac8-111">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="7dac8-112">[AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) ，以允许机器人在响应中发送自适应卡片。</span><span class="sxs-lookup"><span data-stu-id="7dac8-112">[AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) to allow the bot to send Adaptive Cards in responses.</span></span>
- <span data-ttu-id="7dac8-113">将对话框支持添加到 Bot[的对话框。](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/)</span><span class="sxs-lookup"><span data-stu-id="7dac8-113">[Microsoft.Bot.Builder.Dialogs](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/) to add dialog support to the bot.</span></span>
- <span data-ttu-id="7dac8-114">[TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) ，用于将从 bot 提示返回的 TIMEX 表达式转换为 **DateTime** 对象。</span><span class="sxs-lookup"><span data-stu-id="7dac8-114">[Microsoft.Recognizers.Text.DataTypes.TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) to convert the TIMEX expressions returned from bot prompts into **DateTime** objects.</span></span>
- <span data-ttu-id="7dac8-115">用于调用 Microsoft Graph 的[microsoft](https://www.nuget.org/packages/Microsoft.Graph/) graph。</span><span class="sxs-lookup"><span data-stu-id="7dac8-115">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="7dac8-116">在 CLI 中运行以下命令来安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="7dac8-116">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package AdaptiveCards --version 2.2.0
    dotnet add package Microsoft.Bot.Builder.Dialogs --version 4.10.3
    dotnet add package Microsoft.Bot.Builder.Integration.AspNet.Core --version 4.10.3
    dotnet add package Microsoft.Recognizers.Text.DataTypes.TimexExpression --version 1.4.1
    dotnet add package Microsoft.Graph --version 3.18.0
    ```

## <a name="test-the-bot"></a><span data-ttu-id="7dac8-117">测试 bot</span><span class="sxs-lookup"><span data-stu-id="7dac8-117">Test the bot</span></span>

<span data-ttu-id="7dac8-118">在添加任何代码之前，请测试 bot 以确保其正常运行，并配置 Bot 框架仿真器以对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="7dac8-118">Before adding any code, test the bot to make sure that it works correctly, and that the Bot Framework Emulator is configured to test it.</span></span>

1. <span data-ttu-id="7dac8-119">运行以下命令，启动机器人。</span><span class="sxs-lookup"><span data-stu-id="7dac8-119">Start the bot by running the following command.</span></span>

    ```dotnetcli
    dotnet run
    ```

    > [!TIP]
    > <span data-ttu-id="7dac8-120">虽然您可以使用任何文本编辑器来编辑项目中的源文件，但我们建议使用 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="7dac8-120">While you can use any text editor to edit the source files in the project, we recommend using [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="7dac8-121">Visual Studio Code 提供了调试支持、Intellisense 等。</span><span class="sxs-lookup"><span data-stu-id="7dac8-121">Visual Studio Code offers debugging support, Intellisense, and more.</span></span> <span data-ttu-id="7dac8-122">如果使用 Visual Studio Code，则可以使用 "**运行**  ->  **启动调试**" 菜单启动机器人。</span><span class="sxs-lookup"><span data-stu-id="7dac8-122">If using Visual Studio Code, you can start the bot using the **Run** -> **Start Debugging** menu.</span></span>

1. <span data-ttu-id="7dac8-123">通过打开浏览器并转到来确认机器人是否正在运行 `http://localhost:3978` 。</span><span class="sxs-lookup"><span data-stu-id="7dac8-123">Confirm the bot is running by opening your browser and going to `http://localhost:3978`.</span></span> <span data-ttu-id="7dac8-124">你应该会看到 **你的 bot 已准备就绪！**</span><span class="sxs-lookup"><span data-stu-id="7dac8-124">You should see a **Your bot is ready!**</span></span> <span data-ttu-id="7dac8-125">消息。</span><span class="sxs-lookup"><span data-stu-id="7dac8-125">message.</span></span>

1. <span data-ttu-id="7dac8-126">打开 "Bot 框架" 模拟器。</span><span class="sxs-lookup"><span data-stu-id="7dac8-126">Open the Bot Framework Emulator.</span></span> <span data-ttu-id="7dac8-127">依次选择 " **文件** " 菜单和 " **打开机器人**"。</span><span class="sxs-lookup"><span data-stu-id="7dac8-127">Choose the **File** menu, then **Open Bot**.</span></span>

1. <span data-ttu-id="7dac8-128">`http://localhost:3978/api/messages`在 **Bot URL** 中输入，然后选择 "**连接**"。</span><span class="sxs-lookup"><span data-stu-id="7dac8-128">Enter `http://localhost:3978/api/messages` in the **Bot URL**, then select **Connect**.</span></span>

1. <span data-ttu-id="7dac8-129">机器人 `Hello and welcome!` 在聊天窗口中响应。</span><span class="sxs-lookup"><span data-stu-id="7dac8-129">The bot responds with `Hello and welcome!` in the chat window.</span></span> <span data-ttu-id="7dac8-130">向 bot 发送一封邮件，确认它会将其回显回来。</span><span class="sxs-lookup"><span data-stu-id="7dac8-130">Send a message to the bot and confirm it echoes it back.</span></span>

    ![连接到 bot 的 Bot 框架仿真程序的屏幕截图](images/test-emulator.png)
