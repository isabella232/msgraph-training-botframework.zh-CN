---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347765"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3d58b-101">本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 Bot 框架机器人。</span><span class="sxs-lookup"><span data-stu-id="3d58b-101">This tutorial teaches you how to build a Bot Framework bot that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="3d58b-102">如果您只想下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-botframework)。</span><span class="sxs-lookup"><span data-stu-id="3d58b-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-botframework).</span></span> <span data-ttu-id="3d58b-103">有关使用应用 ID 和密码配置应用程序的说明，请参阅 **demo** 文件夹中的自述文件。</span><span class="sxs-lookup"><span data-stu-id="3d58b-103">See the README file in the **demo** folder for instructions on configuring the app with an app ID and secret.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d58b-104">先决条件</span><span class="sxs-lookup"><span data-stu-id="3d58b-104">Prerequisites</span></span>

<span data-ttu-id="3d58b-105">在开始本教程之前，您应在开发计算机上安装以下程序。</span><span class="sxs-lookup"><span data-stu-id="3d58b-105">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="3d58b-106">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3d58b-106">.NET Core SDK</span></span>](https://dotnet.microsoft.com/download)
- [<span data-ttu-id="3d58b-107">Bot 框架仿真程序</span><span class="sxs-lookup"><span data-stu-id="3d58b-107">Bot Framework Emulator</span></span>](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

<span data-ttu-id="3d58b-108">您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="3d58b-108">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="3d58b-109">如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="3d58b-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="3d58b-110">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="3d58b-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="3d58b-111">你可以 [注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) 以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="3d58b-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>
- <span data-ttu-id="3d58b-112">Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="3d58b-112">An Azure subscription.</span></span> <span data-ttu-id="3d58b-113">如果没有，在开始之前，请先创建一个 [免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="3d58b-113">If you do not have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

> [!NOTE]
> <span data-ttu-id="3d58b-114">本教程是使用 .NET Core SDK 版本3.1.401 编写的。</span><span class="sxs-lookup"><span data-stu-id="3d58b-114">This tutorial was written with .NET Core SDK version 3.1.401.</span></span> <span data-ttu-id="3d58b-115">本指南中的步骤可能适用于其他版本，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="3d58b-115">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="3d58b-116">反馈</span><span class="sxs-lookup"><span data-stu-id="3d58b-116">Feedback</span></span>

<span data-ttu-id="3d58b-117">请在 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-botframework)中提供有关本教程的任何反馈。</span><span class="sxs-lookup"><span data-stu-id="3d58b-117">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-botframework).</span></span>
