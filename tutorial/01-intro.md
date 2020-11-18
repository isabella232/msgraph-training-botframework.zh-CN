---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347765"
---
<!-- markdownlint-disable MD002 MD041 -->

本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 Bot 框架机器人。

> [!TIP]
> 如果您只想下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-botframework)。 有关使用应用 ID 和密码配置应用程序的说明，请参阅 **demo** 文件夹中的自述文件。

## <a name="prerequisites"></a>先决条件

在开始本教程之前，您应在开发计算机上安装以下程序。

- [.NET Core SDK](https://dotnet.microsoft.com/download)
- [Bot 框架仿真程序](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：

- 你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以 [注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) 以获取免费的 office 365 订阅。
- Azure 订阅。 如果没有，在开始之前，请先创建一个 [免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

> [!NOTE]
> 本教程是使用 .NET Core SDK 版本3.1.401 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。

## <a name="feedback"></a>反馈

请在 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-botframework)中提供有关本教程的任何反馈。
