---
ms.openlocfilehash: 12a6c18b52e2bc9aba5ad69c8bb9ab8091c11a64
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347763"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，你将创建一个 Bot 框架项目。

1. 在要创建项目的目录中打开命令行界面 (CLI) 。 运行以下命令，以使用 **EchoBot** 模板创建新的项目。

    ```dotnetcli
    dotnet new echobot -n GraphCalendarBot
    ```

    > [!NOTE]
    > 如果收到 `No templates matched the input template name: echobot.` 错误，请使用以下命令安装该模板，然后重新运行上一个命令。
    >
    > ```dotnetcli
    > dotnet new -i Microsoft.Bot.Framework.CSharp.EchoBot
    > ```

1. 将默认的 **EchoBot** 类重命名为 **CalendarBot**。 打开 **/Bots/EchoBot.cs** ，并将所有实例替换 `EchoBot` 为 `CalendarBot` 。 将文件重命名为 **CalendarBot.cs**。

1. 替换 `EchoBot` `CalendarBot` 其余 **.cs** 文件中的所有实例。

1. 在 CLI 中，将当前目录更改为 **GraphCalendarBot** 目录，并运行以下命令以确认项目生成。

    ```dotnetcli
    dotnet build
    ```

## <a name="add-nuget-packages"></a>添加 NuGet 包

在继续之前，请安装稍后将使用的一些其他 NuGet 包。

- [AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) ，以允许机器人在响应中发送自适应卡片。
- 将对话框支持添加到 Bot[的对话框。](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/)
- [TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) ，用于将从 bot 提示返回的 TIMEX 表达式转换为 **DateTime** 对象。
- 用于调用 Microsoft Graph 的[microsoft](https://www.nuget.org/packages/Microsoft.Graph/) graph。

1. 在 CLI 中运行以下命令来安装依赖项。

    ```Shell
    dotnet add package AdaptiveCards --version 2.2.0
    dotnet add package Microsoft.Bot.Builder.Dialogs --version 4.10.3
    dotnet add package Microsoft.Bot.Builder.Integration.AspNet.Core --version 4.10.3
    dotnet add package Microsoft.Recognizers.Text.DataTypes.TimexExpression --version 1.4.1
    dotnet add package Microsoft.Graph --version 3.18.0
    ```

## <a name="test-the-bot"></a>测试 bot

在添加任何代码之前，请测试 bot 以确保其正常运行，并配置 Bot 框架仿真器以对其进行测试。

1. 运行以下命令，启动机器人。

    ```dotnetcli
    dotnet run
    ```

    > [!TIP]
    > 虽然您可以使用任何文本编辑器来编辑项目中的源文件，但我们建议使用 [Visual Studio Code](https://code.visualstudio.com/)。 Visual Studio Code 提供了调试支持、Intellisense 等。 如果使用 Visual Studio Code，则可以使用 "**运行**  ->  **启动调试**" 菜单启动机器人。

1. 通过打开浏览器并转到来确认机器人是否正在运行 `http://localhost:3978` 。 你应该会看到 **你的 bot 已准备就绪！** 消息。

1. 打开 "Bot 框架" 模拟器。 依次选择 " **文件** " 菜单和 " **打开机器人**"。

1. `http://localhost:3978/api/messages`在 **Bot URL** 中输入，然后选择 "**连接**"。

1. 机器人 `Hello and welcome!` 在聊天窗口中响应。 向 bot 发送一封邮件，确认它会将其回显回来。

    ![连接到 bot 的 Bot 框架仿真程序的屏幕截图](images/test-emulator.png)
