---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347766"
---
# <a name="graphcalendarbot"></a>GraphCalendarBot

Bot 框架 v4 回显 bot 示例。

此 bot 已使用 [Bot 框架](https://dev.botframework.com)创建，它展示了如何创建一个简单的 bot 来接受用户输入并回显它。

## <a name="prerequisites"></a>先决条件

- [.Net CORE SDK](https://dotnet.microsoft.com/download) 版本3。1

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a>若要试用此示例

- 在终端中，导航到 `GraphCalendarBot`

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- 从终端或 Visual Studio 中运行机器人，选择选项 A 或 B。

  来自终端的) 

  ```bash
  # run the bot
  dotnet run
  ```

  B) 或从 Visual Studio

  - 启动 Visual Studio
  - 文件-> 打开-> 项目/解决方案
  - 导航到 `GraphCalendarBot` 文件夹
  - 选择 `GraphCalendarBot.csproj` 文件
  - 按 `F5` 运行项目

## <a name="testing-the-bot-using-bot-framework-emulator"></a>使用 Bot 框架仿真程序测试机器人

[Bot 框架仿真器](https://github.com/microsoft/botframework-emulator) 是一种桌面应用程序，它允许 Bot 开发人员在本地主机上测试和调试其 bot 或通过隧道远程运行。

- 在[此处](https://github.com/Microsoft/BotFramework-Emulator/releases)安装 4.9.0 Framework 模拟器版本或更高版本

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a>使用 Bot 框架仿真程序连接到 bot

- 启动 Bot 框架仿真程序
- 文件-> 打开 Bot
- 输入的 Bot URL `http://localhost:3978/api/messages`

## <a name="deploy-the-bot-to-azure"></a>将机器人部署到 Azure

若要了解有关将机器人部署到 Azure 的详细信息，请参阅将 [你的 Bot 部署到 azure](https://aka.ms/azuredeployment) ，获取部署说明的完整列表。

## <a name="further-reading"></a>进一步阅读

- [Bot 框架文档](https://docs.botframework.com)
- [机器人基础知识](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [活动处理](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [Azure Bot 服务简介](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Azure Bot 服务文档](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [.NET Core CLI 工具](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Azure 门户](https://portal.azure.com)
- [使用 LUIS 的语言理解](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [通道和机器人连接器服务](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

使用 `dotnet new echobot` v 4.10.2 生成
